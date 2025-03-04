PostgreSQL Server Extensions
============================

I've played with PostgreSQL server extensions for a while, using C, PGXN, and
the pl/java extension.

Docker Images
-------------

One of the biggest hurdles new developers face is setting up a suitable
server. It's not difficult but does require a modest amount of research 
and sysadmin skills. Many, but far from all, extensions are available as
.rpm and .deb files at the official PostgreSQL site but that requires
modest sysadmin skills to use.

The individual projects often include a list of known docker images - but
those images may be outdated if not unavailable.

The obvious solution is maintaining a list of current docker images at
Docker Hub. It can be kept current via CI/CD using GitHub Actions.

This is what [coyotesong/postgres-pgxnclient](https://github.com/coyotesong/postgres-pgxnclient)
provides. It currently maintains the latest releases (12+) for

- pgxnclient - everything required for [pgxn](https://pgxn.org) development. This includes [pgTAP](https://pgtap.org).
- pl/java - allows the use of java (jars) in UDF and UDT.

The new developer can now start using a PL/Java enabled server, for instance, wit a simple

```
docker run -d -p 5432:5432 -e POSTGRES_PASSWORD=password coyotesong/postgres-pljava
```

In development or wish list
---------------------------

- pl/java cryptographic UDTs (keys, digital certificates, etc.)
- pl/java foreign data wrappers (FDWs) - only POC for now
- module for 'enterprise encryption' inspired by [Cryptography in the database](https://www.amazon.com/Cryptography-Database-Last-Line-Defense/dp/0321320735)
- FDW(?) wrapper for Hashicorp Vault


Official Packages: Debian and Ubuntu
------------------------------------

To add the official PostgreSQL packages for Debian and Ubuntu are located at
[Apt - PostgreSQL wiki](https://wiki.postgresql.org/wiki/Apt). I would recommend
a small change from their 'quickstart' instructions in order to comply with the
current recommendations for package signatures.

```
sudo apt install curl ca-certificates gnupg

curl https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo gpg --no-default-keyring --keyring=/usr/share/keyrings/pgdg-keyring.gpg --import
sudo sh -c 'echo "deb [signed-by=/usr/share/keyrings/pgdg-keyring.gpg] http://apt.postgresql.org/pub/repos/apt $(lsb_release -ce)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'
sudo apt update
sudo apt install postgresql-15
```

An additional step will give preference to these packages over the
distribution's packages. Create this file then re-run `apt update`.

_/etc/apt/preferences.d/99-postgresql_
```
Package: *
Pin: release o=apt.postgresql.org
Pin-Priority: 990
```

Official Packages: RedHat and CentOS
------------------------------------

TBA
