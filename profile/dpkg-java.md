# Oddball Project: dpkg-java

## Summary

Create a library to read (and write?) the Debian/Ubuntu files under /var/lib/dpkg
and /var/lib/apt. These directories contain all of the information related to both
installed and available software.

## Business Case

There are several motivations for this project.

### Prune list of available packages

One of the benefits of the Linux ecosystem is the vast number of packaged software
that's know to work with the rest of your system.

One of the downfalls of the Linux ecosystem is the vast number of packaged software
that makes it hard to find what you're looking for when you do a quick `apt-cache search`.

The solution is to have a tool that reads the existing information (Release, etc.)
and strips out entire categories that I know I will never have an interest in.
Games, ham radio, DNA analysis, etc. The tool should then be able to create and
sign this truncated list and this is what I'll use as my list of available packages.

It's small but it only takes a few searches with several pages of irrelevant results
for you to see the benefits of it.

An alternative approach - either re-implement `apt-cache search` or create a wrapper
for it that excludes unwanted categories.)

### Eliminate unnecessary backups

Security 101 says that we should never restore software - we should always reinstall
it. This ensures we don't accidently restore compromised software, etc.

An obvious precondition to this is **knowing** what can be reinstalled. Some of
this is obvious - we shouldn't need to back up `/bin`, `/lib`, or (most of) `/usr`,
although it wouldn't hurt to scan it for files that should have gone into `/usr/local`
or `/opt`. We should no longer need to back up `/dev`.

However some directories are a mismash of standard packages and local files.
`/var` will definitely have a mix, and some older packages may have not yet
removed their old `/usr/lib` files. (We also need to be careful to **not** follow
symlinks from `/usr/lib` into `/var/lib`.)

Fortunately most of this information is available in the `/var/lib/dpkg` and
`/var/lib/apt` directories. (It's not complete since a few packages still fail
to populate their `.files`.)

Another obvious precondition is knowing which configuration files have been
modified so we can ensure they're backed up. We're lucky here since the default
configuration files should be listed in `.conffile`.

Removing these files will go a long way towards reducing the size of all
backups. However developers will also want to exclude local caches created
by maven, npm, pip, etc., after capturing a list of what's in those caches.

### Support security scans

If we have a list of packaged files, including MD5, SHA1, or SHA256 hashes,
we can check for modified files by verifying this information. This is now
an option with `rpm`, but I don't know if it's possible to do this with
`dpkg`/`apt`. However it's not hard to simply run through the `.md5sum`
files.

We should also check for unusual file permissions - especially anything that's
SETUID or SETGID and owned by root. Sometimes this is legitimate - but it's
a huge red flag for anything not on a very short list.

We can normally just use the standard files - but are the complete? I know
they aren't - some packages don't have the proper `.files` or `.md5sum`,
Debian/Ubuntu doesn't provide ownership and permissions information (AFAIK),
etc. If we're paranoid we could download the package, verify the signature,
unpack it into a temporary directory, and then capture all details we
want.

## Practical Observations

This should not be a surprise but many of these tasks are much faster if we
use the existing text files instead of database queries. This is hardly
surprising when you have over 500k files.

I haven't checked the performance when using a noSQL database.

This is why I think the final solution should use python. It can still use
databases as an intermediate prep stage but the final work should be done
using trusted text files.
