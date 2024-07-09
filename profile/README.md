# Coyote Song

This is the public repo of Coyote Song - the freelance contractor
company for [Bear Giles](mailto:bgiles@coyotesong.com). I'm currently focused on databases, testing, and security.

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

### [postgresql-pljava-docker](https://github.com/beargiles/postgresql-pljava-docker)

This repo, still on my 'beargiles' account, uses GitHub actions to automatically build several
docker images that extend the official PostgreSQL docker image. It only builds pl/java at the
moment but it could be easily extended to include other PGXN-based extensions.

## Testing

### [testcontainers-java-extras](https://github.com/coyotesong/testcontainers-java-extras)

This repo contains new test containers that I plan to contribute to the TestContainer project
once they're stable. The current focus is cloud databases (using the docker images provided for
development and testing) but I have plans to also support IdP containers in the future.

## Security

### [Using AccessControlContext to restrict access to sensitive resources](https://gist.github.com/beargiles/69f34af3bec7cde3a0c2bddb1409f3b5) (gist)

This is outdated since the SecurityManager has been removed from the JVM but this
functionality may return in a JEP. It uses the SecurityManager infrastructure but with a much
more functional focus. The general idea is to create an AccessControlContext that permits
access to the sensitive resource (file, network) and then use a SecurityManager that blocks
that access. All access will fail unless run as a PrivilegedAction by that previously
created AccessControlContext.

We can still use a similar approach within our libraries but we can't (currently) restrict
access at the JVM level.

### [YubiKeys-TPMs-and-more](https://github.com/coyotesong/YubiKeys-TPMs-and-more)

This repo mostly contains notes at the moment. I plan to organize it, with some DevOps tools,
to simplify the adoption of YubiKeys by developers and users.

I also want to investigate how to unlock an encrypted (linux) disk using the motherboard's TPM
chip. It's not as secure as a user typing in a good passpharse but it will allow a system to fully
reboot without human intervention.

## About Me

- Java developer since early 2000s

- C/C++ developer before that.

- 'Homelab' since long before that became a term. E.g., I remember exploring Kerberos in the late 1990s - before Active Directory "embraced and extended" it.

