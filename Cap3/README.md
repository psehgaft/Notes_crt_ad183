# Notas_crt_rhl8E Cap3

## An example of a stateful session bean is shown below with its getter and setter methods:

```java
@Stateful
@Named("hello")
public class Hello {

	private String name;

	@Inject
	private PersonService personService;

	public void sayHello() throws IllegalStateException, SecurityException, SystemException {
		String response = personService.hello(name);
		FacesContext.getCurrentInstance().addMessage(null, new FacesMessage(response));
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

}
```
## Converting a POJO to an EJB

> POJO

```java
public class TodoBean {

  public void addTodo(TodoItem item) {
  ...
  }

  public void findTodo(int id) {
  ...
  }

  public void updateTodo(TodoItem item) {
  ...
  }

  public void deleteTodo(int id) {
  ...
  }
}
```
> EJB Define tipe of EJB

```java
@Stateless
public class TodoBean {
...
}


@Stateful
public class TodoBean {
...
}

@Singleton
public class TodoBean {
...
}

```

> The following example shows an EJB that is initialized for application startup and uses the init() method to setup its initial state:

```java
@Singleton
@Startup
public class TodoBean {

  @PostConstruct
  public void init() {
    // do some initialization
  }
...
}
```

> The following example shows an EJB that is initialized for application startup and uses the init() method to setup its initial state:

Assume you have defined an EJB like the following

```java
@Stateless
public class TodoBean {

  public void addTodo(TodoItem item) {
  ...
  }

  public void findTodo(int id) {
  ...
  }
  ...
  }
...
}
```

Clients can invoke methods on the EJB by injecting the EJB directly into code using the @EJB annotation:

```java
public class TodoClient {

  @EJB
  TodoBean todo;

  TodoItem item = new TodoItem();
  item.setDescription("Buy milk");
  item.setStatus("PENDING");

  //invoke EJB methods
  todo.addTodo(item);
...
}
```
A sample JNDI client program that uses the JNDI naming scheme to look up the remote EJB looks like the following:

```java
package com.redhat.training.client;

public class CalculatorClient {

  public static void main(String[] args) throws Exception {

    String JNDI_URL= "myapp/calculator-ejb/CalculatorBean!com.redhat.training.ejb.Calculator";

    try {
      Context ic = new InitialContext();
      Calculator calc = (Calculator) ic.lookup(JNDI_URL);
      System.out.println("Response from server = " + calc.add(1,2);
      ...
    }
    catch (Exception e) {
      // handle the exception
    }

  }
...
}
```

> You also need to provide a file called jndi.properties in the class path of the client program, with the host name, IP address, port, and security details (if secured for remote access) of the remote application server where the EJB is running.

```java
java.naming.factory.initial=org.jboss.naming.remote.client.InitialContextFactory
java.naming.provider.url=http-remoting://10.2.0.15:8080
jboss.naming.client.ejb.context=true
```



## Info
Reviewing the Types of EJB
The Java EE specification defines two different types of EJBs:

1. Session: Performs an operation when called from a client. Usually an application's core business logic is exposed as a high-level API (Session Facade pattern) that can be distributed and can be accessed over a number of protocols (RMI, JNDI, web services).

    - Stateless Session Beans (SLSB): A stateless session bean does not maintain conversational state with clients between calls. When clients interact with the stateless session bean and invoke methods on it, the application server allocates an instance from a pool of stateless session beans, which are pre-instantiated. Once a client completes the invocation and disconnects, the bean instance is either released back into the pool or destroyed.

    - Stateful Session Beans (SFSB): In contrast to stateless session beans, stateful session beans maintain conversational state with clients across multiple calls. There is a one-to-one relationship between the number of stateful bean instances and the number of clients. When a client completes the interaction with the bean and disconnects, the bean instance is destroyed. A new client results in a new stateful bean with its own unique state. The application server ensures that each client receives the same instance of a stateful session bean for each method call.

    - Singleton Session Beans: Singleton session beans are session beans that are instantiated once per application and exists for the lifecycle of the application. Every client request for a singleton bean goes to the same instance. Singleton session beans are used in scenarios where a single enterprise bean instance is shared across multiple clients.

2. Message Driven Bean (MDB): Used for asynchronous communication between components in a Java EE application and can be used to receive Java Messaging Service (JMS) compatible messages and take some action based on the content of the received messages.

3. Describing the Stateful Session Bean Life Cycle
A stateful session bean has three distinct states in its life cycle:

    . Does Not Exist: The stateful EJB is not created and does not exist in application server memory.

    . Ready: The stateful EJB (object) is created in application server memory either by JNDI call or CDI injection and is ready to have its business methods invoked by clients.

    . Passivated: Since a stateful EJB has object state that is persisted across multiple client calls, the application server may decide to passivate (deactivate) the EJB to secondary storage to optimize memory consumption. It will activate the EJB back into Ready state when a client invokes any method on the EJB. The developer does not have any direct control of activation and passivation and it is handled transparently by the application server based on certain algorithms.

4. Describing the Stateless Session Bean Life Cycle

Since a stateless session bean has no state and is never passivated, it has just two distinct states in its life cycle:

    . Does Not Exist: The stateless EJB is not created and does not exist in application server memory.

    . Ready: The stateless EJB (object) is created in application server memory either by JNDI call or CDI injection and is ready to have its business methods invoked by clients.

5. Describing the Singleton Session Bean Life Cycle

Similar to a stateless session bean, a singleton session bean has two distinct states in its life cycle:

    . Does Not Exist: The singleton is not created and does not exist in application server memory.

    . Ready: The singleton EJB (a single object) is created in application server memory at startup, or by CDI injection and is ready to have its business methods invoked by clients.


### Bean Life Cycle Annotations

| Bean Type | Annotation | Description |
| --- | --- | --- |
| Stateful Session Bean	| @PostConstruct | method is invoked when a bean is created for the first time. |
| Stateful Session Bean	| @PreDestroy | method is invoked when a bean is destroyed. | 
| Stateful Session Bean	| @PostActivate | method is invoked when a bean is loaded to be used after activation.
| Stateful Session Bean	| @PrePassivate	| method is invoked when a bean is about to be passivated. |
| Stateless Session Bean | @PostConstruct | method is invoked when a bean is created for the first time. |
| Stateless Session Bean | @PreDestroy | method is invoked when a bean is removed from the bean pool or is destroyed. |
| Singleton Session Bean | @PostConstruct | method is invoked when a bean is created for the first time. | 
| Singleton Session Bean | @PreDestroy | method is invoked when a bean is destroyed. | 
| Singleton Session Bean | @Startup | The application server instantiates the singleton at startup. | 

### There are two different ways to manage transactions in Java EE:

Implicit or Container Managed Transaction (CMT): The application server manages the transaction boundary and automatically commits and rolls back transactions without the developer writing code for managing transactions. This is the default option unless explicitly overridden by the developer.

Explicit or Bean Managed Transaction (BMT): The transactions are managed by the developer in code at the bean level (in EJBs). The developer is responsible for controlling the transaction scope and boundary explicitly.

### Setting Transaction Attributes

In CMT, transaction attributes control the scope of a transaction and allows a developer to declaratively manage transactions at the individual method level in an EJB.

For example, consider the following code snippet where one stateless EJB calls a method from another stateless EJB:

```java
@Stateless
public class TodoService {

  @Inject
  UserService user;

  public void login(String user, String password) {
    user.authenticate(user,password);
  }
  ...
  }

@Stateless
public class UserService {

  public boolean authenticate(String user, String password) {
    ...
  }
  ...
}
```
> @Inject can inject any bean including EJBs, whereas @EJB can only inject EJBs. @Inject is described in more detail in the chapter titled Implementing Contexts and Dependency Injection.

### The Java EE specification defines six transaction attributes. They are illustrated below with reference to the code snippet above:

> @TransactionAttribute(TransactionAttributeType.REQUIRED)

If the login() method is running within a transaction and calls the authenticate() method in the UserService class, then the authenticate() executes within the same transaction. If there is no transaction when authenticate() is called, then the application server starts a new transaction before executing authenticate(). This is the default transaction attribute unless explicitly overridden with other transaction attribute annotations.

> @TransactionAttribute(TransactionAttributeType.REQUIRES_NEW)

If the login() method is running within a transaction and calls the authenticate() method in the UserService class, then the application server suspends the transaction and starts a new transaction before executing authenticate(). Once authenticate() finishes executing, control moves back to login() and the suspended transaction resumes. If there is no transaction when authenticate() is called, then the application server starts a new transaction before executing authenticate(). This attribute ensures that your method always runs with a new transaction.

> @TransactionAttribute(TransactionAttributeType.MANDATORY)

If the login() method is running within a transaction and calls the authenticate() method in the UserService class, then the authenticate() executes within the same transaction. If there is no transaction when authenticate() is called, then the application server throws a TransactionRequiredException. Use the this attribute if you want a method to always execute in the transaction context of the calling client.

> @TransactionAttribute(TransactionAttributeType.NOT_SUPPORTED)

If the login() method is running within a transaction and calls the authenticate() method in the UserService class, then the application server suspends the transaction and runs authenticate() without any transaction context. Once authenticate() finishes executing, control moves back to login() and the suspended transaction resumes. If there is no transaction when authenticate() is called, then the application server does not start a new transaction before executing authenticate(). Use this attribute for methods that don't need transactions.

> @TransactionAttribute(TransactionAttributeType.SUPPORTS)

If the login() method is running within a transaction and calls authenticate(), then authenticate() executes within the same transaction. If there is no transaction when authenticate() is called, then the application server does NOT start a new transaction before executing authenticate().

> @TransactionAttribute(TransactionAttributeType.NEVER)

If the login() method is running within a transaction and calls the authenticate() method in the UserService class, then the application server throws a RemoteException. If there is no transaction when authenticate() is called, then the application server does not start a new transaction before executing authenticate().

### Summary
In this chapter, you learned:

An Enterprise Java Bean (EJB) is a portable Java EE component that is typically used to encapsulate business logic in an enterprise application. It runs on an application server and can be consumed by remote clients as well as other Java EE components running locally in the same JVM process.

An EJB provides multi-threading, concurrency, transactions, and security for enterprise applications without requiring the developer to write code for these features explicitly. Furthermore, the developer can declaratively add annotations to the EJB to expose the business methods as web service end-points.

There are two different types of EJB: Session Beans and Message Driven Beans (MDB). Session beans can be of three types: Stateless Session Beans (SLSB), Stateful Session Beans (SFSB) and, Singleton Session Beans.

A Message Driven Bean (MDB) enables Java EE applications to process messages asynchronously. An MDB listens for JMS messages. For each message received, it performs an action. MDBs provide an event driven, loosely coupled model for application development.

If an EJB client and the EJB are running locally in the same JVM process, clients can directly inject references to the EJB using the @EJB annotation. If the client is remote, then JNDI lookups are used.

EJB components in an application run within the context of a container inside an application server. The container is responsible for managing the life cycle (creation, execution, and destruction) of the EJBs. Each of the different types of EJBs (Stateless, Stateful, Singleton, MDB) have their own life cycle.

Java EE has support for Transactions to ensure that data integrity is maintained by controlling concurrent access to the data, and to ensure that a failed business transaction does not leave the system in an inconsistent or invalid state.

In Java EE, transactions can be managed in two different ways: Container Managed Transactions (CMT) and Bean Managed Transactions (BMT).

In CMT, the application server manages the transactions without the developer writing any explicit code and the scope can be controlled by using Transaction attributes. The application server can automatically perform a rollback when it encounters a failure or an exception.

In BMT, the developer is responsible for managing the transactions and is in full control of the scope of transactions. The developer has to manually commit and rollback transactions if there is an exception or a failure.

