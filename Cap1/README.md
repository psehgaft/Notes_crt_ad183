# Notas_crt_rhl8E Cap1

##Build the application

```sh
mvn clean package
```
## Run the application using the Maven exec

```sh
mvn clean package exec:java
```
### Summary
In this chapter, you learned:

Enterprise applications are characterized by their ability to handle transactional workloads, multi-component integration, security, distributed architectures, and scalability.

Java Enterprise Edition (Java EE) is a specification for developing enterprise applications using Java. It is a platform-independent standard that is developed under the aegis of the Java Community Process (JCP). A software system that implements the Java EE specification is called an application server.

The Java SE API provides a rich set of modular, reusable components for implementing Java applications. Java EE is built on top of Java SE and provides a set of APIs that are focused on developing enterprise applications.

Java EE applications are designed to be multi-tiered and can accommodate a variety of architectures depending on the use case.

Red Hat JBoss Developer Studio is an Eclipse™ based IDE provided by Red Hat that contains a set of integrated plug-ins and tools to simplify development of Java EE enterprise applications. It supports many application servers and you can manage the life cycle of the application server from within the IDE itself.

Apache Maven is the preferred tool for building, packaging, and deploying Java SE and Java EE applications. JBDS has built-in support for Maven. Projects can be built, tested, packaged, and deployed to application servers using Maven plug-ins.

