# Coyote Song

This is the public repo of Coyote Song - the freelance contractor
company for [Bear Giles](mailto:bgiles@coyotesong.com). I'm currently focused on databases,
testing, and security, and starting to take a deep dive into locally hosted AI + RAG
using both NVIDIA Jetson Orin Nano (2) and Corel accelerators now that I have finally
received them.

## Portfolio

I've been using my sabbatical to follow a former boss's advice and submitting some of
my custom tools to related projects.

I can do much more than this but that code is either covered by NDAs (and I no longer
have access to it anyway) or is so complex that it can't fit a description into an
'elevator pitch'. In contrast these are generally bite-sized and demonstrate an ability
to think beyond the immediate problem.

This does not include a ton of database-specific projects, esp. the ones involving
server-side extensions.

| Project                      | Status   | Description                                              |
|------------------------------|----------|----------------------------------------------------------|
| apache commons-archive       | accepted | read-only access to legacy Unix backup files             |
| maven-dependency-plugin      | pre-PR   | refactored 'tree' to use velocity, improved .dot         |
| apache velocity-engine       | pre-PR   | new directives #indent and #collapse                     |
| [coyotesong/erdiagram-maven-plugin](https://github.com/coyotesong/erdiagram-maven-plugin) | active   | creates Entity-Relationship Diagrams |
| testcontainers               | pre-PR   | I have a ton but the team feels they're too specialized. |
| hamcrest matchers            | POC      | annotations -> custom Hamcrest matcher (plugin)          |
| RSS client + persistence     | POC      | client + database schema + implementation                |
| YouTube client + persistence | POC      | client + database schema + implementation                |

"POC" = "Proof of Concept". I've been using it in my own projects and am now smoothing
off the rough edges so other people can use it. This often involves identifying a suitable
existing project to extend via PR, vs. having yet another standalone implementation.l

Notes:

- RSS: I have since learned of Apache Nutch and may not pursue this much further. My primary
  goal had been using RSS to get a list of URLs to download and cache for use by subsequent
  applications and it looks like Nutch is a better solution since it already knows how to
  "play nice". The database was only used to avoid duplication, esp. when the same item
  is listed in multiple RSS feeds. (I'm looking at you, DZone.)

- YouTube: the goal is to download and cache all metadata and use it in advanced searches.

### Pre-Proof of Concept

These are projects where I have working code but its still in the exploration stage.

- Maven archetype to create skeleton implementation of a Spring Boot webapp. The intention
  is to use it in my blog - the blog examples can always start with this architype and
  the gist can focus on just the changes required for the subject being discussed.

- Maven archetype to create skeleton implementation of an NIO Filesystem, in particular
  the `Path` logic and unit tests to ensure the behavior is consistent with the standard
  filesystem implementation. This is definitely a "scratch an itch" project but sometimes
  there are clear benefits to using the filesystem as an abstraction instead of a
  conventional API. An NIO filesystem is mostly boilerplate plus a little bit of
  custom code.

- Ansible molecule to replace EC2. The existing code keeps everything on the local
  system and focused on EC2 (plus a bit of networking and IAM) but it has problems.
  I have ideas on a serverless implementation that manages the resources but I don't
  know if there's still sufficient need for this given the improvements in docker,
  IDEs, etc.

### Oddball Projects

I also have a few "what if?..." projects... but I know I should be using python
for them instead of Java. (But I can knock out working code in Java and then decide
if it's worth redoing it in python as an exercise.)

- [dpkg-java](dpkg-java.md)

- [paper-java](paper-java.md)

## Spring Boot

### [spring-boot-with-testcontainers-examples](https://github.com/coyotesong/spring-boot-with-testcontainers-examples)

Examples of using TestContainers with Spring Boot. At the moment this is limited to
relational databases and jOOQ, but the project is designed to support additional
persistence mechanisms.

This project demonstrates:

- using flyway to initialize the database schema
- using the org.jooq maven plugin to autogenerate the required source files in a separate source directory

(I haven't added Spring JdbcTemplate, Spring Data, etc., since there's an unexpected jOOQ error
when I add JPA annotations to my persisted classes.)

## Databases

### [database-metadata-comparison](https://github.com/coyotesong/database-metadata-comparison)

This repo, still in its early stages, uses TestContainers to create tables which provide a comparison
of database metadata across multiple databases. The current output is useful if you're routinely
working on multiple databases, e.g., "what's the 'quote' character?" but I suspect the real value
will be in comparising multiple versions of the same database.

Taking a step back this demonstrates a way to perform A/B testing using real servers. There is
no need for multiple runs and comparing results - you can spin up two (or more!) servers and
run identical tests on them. This could be useful when verifying that a new database version
won't break existing behavior, etc.

Running the same tests with different dependencies, but identical servers, is also possible
with a small amount of extra effort in setting up a different classpath in each thread. Again
this could be useful when verifying that an updated dependency will not break existing
behavior.

### [postgresql-pljava-docker](https://github.com/beargiles/postgresql-pljava-docker)

This repo, still on my 'beargiles' account, uses GitHub actions to automatically build several
docker images that extend the official PostgreSQL docker image. It only builds pl/java at the
moment but it could be easily extended to include other PGXN-based extensions.

Note: this hasn't been updated in a while since a build required information (pl/java release)
available only when the upstream source code had been unpacked. I'm sure this is possible in
ansible (somehow) but thought I should investigate a different approach first. I just haven't
gotten to it yet.

However further research shows that each upstream update changes the pl/java release for
all supported versions. This changes the nature of the required work - I might not try
to automate it again for awhile but I can definitely reduce the effort required to manually
trigger a release.

I also want to use the maven archetype mentioned above to show how test java apps
that rely on this server extension... or really any of the postgresql server extensions.

## Testing

### [testcontainers-java-extras](https://github.com/coyotesong/testcontainers-java-extras)

This repo contains new test containers that I plan to contribute to the TestContainer project
once they're stable. The current focus is cloud databases (using the docker images provided for
development and testing) but I have plans to also support IdP containers in the future.

Update: I offered this work but it was largely rejected by the TestContainer project due to
the extra work required to maintain the additional containers. They also felt it was outside
of their focus area - altough I noticed a Go LDAP container was accepted after I argued
that the ability to test authentication is an important feature and many enterprises
use LDAP (Active Directory) for this.

## Security

### [YubiKeys-TPMs-and-more](https://github.com/coyotesong/YubiKeys-TPMs-and-more)

This repo mostly contains notes at the moment. I plan to organize it, with some DevOps tools,
to simplify the adoption of YubiKeys by developers and users.

I also want to investigate how to unlock an encrypted (linux) disk using the motherboard's TPM
chip. It's not as secure as a user typing in a good passpharse but it will allow a system to fully
reboot without human intervention.

## Blog: [Invariant Properties](https://invariantproperties.com/)

### History

My prior blog was at the same URL - I took it down because I felt I didn't have sufficient time
to keep the self-hosted Wordpress instance secure. This is the main reason my revived blog is
a static site built using [Hugo](https://gohugo.io/). However I have not ruled out using
[Medium](https://medium.com) instead since that would simplify syndication.

The name of the blog comes from computer science and software reliability. The idea is
that every loop will have one or more invariant properties that we can use to optimize
our design and to verify correct behavior. This is easy to use in C/C++ since we can
use conditional `assert` macros that run in development but not production.

In theory it should also be easy to use assertions in java - they can be enabled and
disabled with a simple JVM parameter - but for some reason I've rarely seen them used.
I don't know if it's been because of ignorance of their benefits, older tools making
it harder to consistently enable and disable them, or just a preference for (oft-incomplete)
unit tests.

A classic example of a loop invariant is the quicksort algorithm:

```java
<T> List<T> sort(Collection<T> all) {
   T pivot = findPivot(all);
   List<T> left = all.filter(s -> s.compareTo(pivot) <= 0).toList();
   List<T> right = all.filter(s -> s.compareTo(pivot) > 0).toList();

   // first loop invariant
   assert(left.forEach(s -> s.compareTo(pivot) <= 0));
   assert(right.forEach(s -> s.compareTo(pivot) > 0));

   // alternate: ensure every element of 'left' is < every element of 'right'

   sort(left);
   sort(right);

   // second loop invariant
   assert(isSorted(left));
   assert(isSorted(right));

   left.addAll(right);

   // third loop invariant
   // every element in 'left' is present in 'all' and vice versa

   return left;
}
```

These assertions seem pointless - the code is so simple. But that overlooks
the facts that:

- starting with the assertions tells you what the code must do
- people make dumb mistakes and a few well-placed assertions can save a lot of trouble later
- few loop invariants are this simple

### Future Blog Topics

I'm still working on the infrastructure but I hope to soon have a maven archetype
that creates a basic [Spring Boot](https://spring.io/projects/spring-boot/)
application with a database, a basic
[Spring MVC](https://docs.spring.io/spring-framework/reference/web/webmvc.html)
controller, and
[Spring Security](https://spring.io/projects/spring-security/).

This archetype will also include functional tests using
[TestContainers](https://testcontainers.com). (Functional tests depend on
access to external resources but unlike integration tests these tests
have full control of those resources.)

With that I can write blog posts that provide a potential vulnerability,
an example of how it can be exploited, one potential solution, and proof
that that specific vulnerability has been closed. The archetype will
allow me to demonstrate this in a nontrivial application while limiting
the blog itself to the necessary changes.

Some examples:

- using a separate schema for the actual 'password' value
- replacing the standard authentication (which reads the database) with one that uses a stored procedure that indicates 'accept' or 'deny'
- autogenerating an audit trial using database triggers and capturing the values in JSON (if supported by the database)




## About Me

- Java developer since early 2000s

- C/C++ developer before that.

- 'Homelab' since long before that became a term. E.g., I remember exploring Kerberos in the late 1990s - before Active Directory "embraced and extended" it.

