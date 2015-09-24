**java ee hints**

_Java EE_ is a set of specifications intended for enterprise applications. It can be seen as an extension of Java SE to facilitate the development of distributed, robust, powerful, and highly available applications.

The Java EE infrastructure is partitioned into logical domains called **containers**. Each container has a specific role, supports a set of APIs, and offers services to components (security, database access, transaction handling, naming directory, resource injection).

_For example, if you need to develop a web presentation layer, you will develop a JSF application and deploy it into a web container, not an EJB container. But if you want a web application to invoke a business tier, you might need both the web and EJB containers._


**The application client container (ACC)** includes a set of Java classes, libraries, and other files required to *bring injection, security management, and naming service* to **Java SE applications** (Swing, batch processing, or just a class with a main() method). ACC communicates with the EJB container using RMI-IIOP and the web container with HTTP (e.g., for web services).

**The web container** (a.k.a. servlet container) provides the underlying services for managing and executing web components (servlets, JSPs, filters, listeners, JSF pages, and web services). It is responsible for instantiating, initializing, and invoking servlets and supporting the HTTP and HTTPS protocols. It is the container used to feed web pages to client browsers.

**The EJB container** is responsible for managing the execution of the enterprise beans containing the business logic tier of your Java EE application. It _creates new instances_ of EJBs, _manages their life cycle_, and _provides services_ such as transaction, security, concurrency, distribution, naming service, or the possibility to be invoked asynchronously.

**Java EE offers the following services :**
* _JPA_: Standard API for object-relational mapping (ORM). With its Java Persistence Query Language (JPQL), you can query objects stored in the underlying database.
* _JTA_: This service offers a transaction demarcation API used by the container and the application.
* _JMS_: JMS allows components to communicate asynchronously through messages.It supports reliable point-to-point (P2P) messaging as well as the publish-subscribe (pub-sub) model.
* _Java Naming and Directory Interface (JNDI)_: This API, included in Java SE, is used to access naming and directory systems.JNDI is used in a more transparent way through injection.
* _JavaMail_: Many applications require the ability to send e-mails, which can be implemented through use of the JavaMail API.
* _Security services_: Java Authentication and Authorization Service (JAAS) enables services to authenticate and enforce access controls upon users
* _Management_: Java EE defines APIs for managing containers and servers using a special management enterprise bean. The **Java Management Extensions (JMX)** API is also used to provide some management support.


**Packaging**
  * _jar file_ can be executed in a _Java SE environment_ or in an **application client container**. Like any other archive format, the jar file contains an optional _META-INF directory_ for **meta information describing the archive**. The META-INF/MANIFEST.MF file is used to define
extension- and package-related data. If **deployed in an ACC**, the _deployment descriptor can optionally be located at META-INF/application-client.xml._

  * _EJb Module Jar_ It contains an optional META-INF/ejb-jar.xml deployment descriptor and can be deployed only in an EJB container.

  * _A web application module_ contains servlets, JSPs, JSF pages, and web services, as well as any other web-related files , All these artifacts are packaged in a jar file with a .war extension. The optional web **deployment descriptor** is defined in the _WEB-INF/web.xml_ file. If the war contains **EJB Lite beans**, an optional deployment descriptor can be set at _WEB-INF/ejb-jar.xml_. **Java .class** files are placed under the _WEB-INF/classes_ directory and **dependent jar files** in the _WEB-INF/lib_ directory.

  * _An enterprise module_ can contain zero or more web application modules, zero or more EJB modules, and other common or external libraries. All this is packaged into an enterprise archive _(a jar file with an .ear extension)_ so that the deployment of these various modules happens coherently.The optional enterprise module **deployment descriptor** defined in the _META-INF/application.xml_ file. The **special lib directory** is used to share common libraries between the modules.

**OSGI** is a standard for dynamic component management and discovery , which  allow building modular java Applications or components can be remotely installed, started, stopped, updated, and uninstalled without requiring a reboot. Components can also detect the addition or removal of new services dynamically and adapt accordingly. example implementation is Apache Felix.

The principle of **ORM** involves delegating access to relational databases to external tools
or frameworks, which in turn give an object-oriented view of relational data, and vice versa.
Mapping tools have a bidirectional correspondence between the database and objects.Several frameworks achieve this, such as __Hibernate, TopLink, and Java Data Objects (JDO)__ but __JPA__ is standard Java way to use ORM.


**JPA** is an abstraction above JDBC that makes it possible to be independent of SQL. All classes and annotations of this API are in the javax.persistence package. The main components of JPA are as follows:
  * ORM, which is the mechanism to map objects to data stored in a relational database.
  * An entity manager API to perform database-related operations, such as Create, Read, Update, Delete (CRUD) operations. This API allows you to avoid using the JDBC API directly.
  * The Java Persistence Query Language (JPQL), which allows you to retrieve data with an object-oriented query language.
  * Transactions and locking mechanisms when accessing data concurrently provided by Java Transaction API (JTA). Resource-local (non-JTA) transactions are also supported by JPA.
  * Callback and listeners to hook business logic into the life cycle of a persistent object.

Reference implementation of __JPA 2.0__ is __EclipseLink 1.1__.
Java EE 6 specifications uses the concept of **configuration by exception** , you can depend on default used configuration till you need custom one.

Associated with every entity is **metadata** that describes the mapping. __This metadata enables the persistence provider to recognize an entity and to interpret the mapping__ , this metadate come in 2 forms **annotations** or **xml descriptors**.

**Entities** are just Plain Old Java Objects (POJOs) that are managed, or not, by the entity manager. When they are managed, they have a persistence identity, and their state is synchronized
with the database. When they are not managed (i.e., they are detached from the entity manager),
they can be used like any other Java class.
The operations made to entities fall into four categories: __persisting, updating, removing,
and loading__, which correspond to the database operations of __inserting, updating, deleting, and
selecting__, respectively. all this operations has **callback listeners** they work like **triggers** in a relational database which __allow you to add your own business logic when certain life-cycle events occur on an entity__.

**An entity is managed :**
  1. when it is loaded from the database such calling EntityManager.find() method, or create a
  JPQL query to retrieve a list of entities.

**An entity is detached :**
  1. when calling the EntityManager.clear() method will clear the entity from the persistence context; it becomes detached.
  2. When a managed entity is serialized, crosses the network __to be invoked remotely__ , cross layers __to be displayed in a presentation tier__, and gets deserialized, it is seen as a detached object. To reattach an entity, you need to call the EntityManager.merge() method.
