# Notes_crt_ad183 — Java EE Certification Notes

Study notes for **AD183 — Red Hat Application Development I: Programming in Java EE**, the course behind the Red Hat Certified Enterprise Application Developer path. The notes cover building enterprise applications with Maven and JBoss EAP, dependency injection with CDI, EJBs, and REST services.

## Contents

- **Cap1/** — Enterprise application concepts: transactional workloads, multi-tier architectures, Java EE vs Java SE, application servers. Build and run examples with Maven (`mvn clean package`, `mvn clean package exec:java`).
- **Cap2/** — Resource injection with CDI: defining datasources on JBoss EAP 7, `standalone-full.xml` configuration, starting the server and reading the server log.
- **Cap3/** — Enterprise JavaBeans: converting a POJO into a `@Stateless`/`@Stateful` bean, injecting services with `@Inject`, managed beans with JSF.
- **practice.md** — Hands-on exam practice checklist: JPA persistence and data mapping, parameter validation, dependency injection, JMS messaging, named queries, EJB conversion, JAX-RS REST services and REST security.

## Usage

Each chapter documents its own Maven commands. In general:

```sh
# Build the example application
mvn clean package

# Run the application with the Maven exec plugin
mvn clean package exec:java
```

To run the JBoss EAP server examples from Cap2:

```sh
cd $JBOSS_HOME/bin
./standalone.sh -c standalone-full.xml
```

## License

This repository is licensed under the [GNU General Public License v3.0](LICENSE).
