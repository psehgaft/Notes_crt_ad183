# Notas_crt_rhl8E Cap2

# Resource Injection Using CDI

''' xml
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
'''
