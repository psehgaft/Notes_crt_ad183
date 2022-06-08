# Notas_crt_rhl8E Cap2

## Resource Injection Using CDI

```xml
<subsystem xmlns="urn:jboss:domain:datasources:4.0">
<datasources>
   <datasource jndi-name="java:jboss/datasources/ExampleDS" pool-name="ExampleDS" enabled="true" use-java-context="true">
       <connection-url>jdbc:h2:mem:test;DB_CLOSE_DELAY=-1;DB_CLOSE_ON_EXIT=FALSE</connection-url>
       <driver>h2</driver>
       <security>
           <user-name>sa</user-name>
           <password>sa</password>
       </security>
   </datasource>
</datasources>
...
```
## JBoss home

> JBOSS_HOME=/opt/jboss-eap-7.0 || /opt/jboss-eap

```sh
echo $JBOSS_HOME
```

## Start JBoss EAP in a new terminal window

```sh
cd $JBOSS_HOME/bin

./standalone.sh -c standalone-full.xml
```

## View the server log messages

>***_NOTE:_*** 
jboss log path: /opt/jboss-eap-7.0/standalone/log/server.log

```sh
grep -i "datasources" /opt/jboss-eap-7.0/standalone/log/server.log
```
## Wildfly-maven-plugin, which provides features to deploy and undeploy applications to EAP

```xml
<plugin>
  <groupId>org.wildfly.plugins</groupId>
  <artifactId>wildfly-maven-plugin</artifactId>
  <version>${version.wildfly.maven.plugin}</version>
</plugin>
```

## To build, package, and deploy an application to EAP

```sh
mvn clean package wildfly:deploy
```
## Undeploy an application to EAP


> Default workspace=/home/student/JB183/workspace 

### Summary
In this chapter, you learned:

An application server provides the necessary runtime environment and infrastructure to host and manage Java EE enterprise applications.

Red Hat JBoss EAP 7 is a Java EE 7 compliant application server that provides a reliable, high-performance, light-weight, and supported infrastructure for deploying Java EE applications.

A profile in the context of a Java EE application server is a set of related APIs that target a specific application type. The Java EE 7 specification defines two profiles: web and full. JBoss EAP fully supports both profiles.

A resource is a logical object that can be looked up and used by components in a Java EE application. Each resource is identified by a unique name, called the JNDI name or a JNDI resource binding.

The JNDI bindings are organized under a logical namespace, in a tree-like hierarchy, usually called the JNDI tree.

You can inject JNDI resources into applications that require the resource by using a @Resource annotation.

Java EE applications are packaged and deployed in different formats. The three most common are: JAR, WAR, and EAR files.

Maven deploys applications to JBoss EAP using the Wildfly Maven plug-in, which provides features to deploy and undeploy applications to EAP. It supports deploying all three types of deployment units: JAR, WAR, and EAR.