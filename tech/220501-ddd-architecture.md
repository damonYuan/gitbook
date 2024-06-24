# Typical Domain Driven Design Application Architecture

This article summarizes the understanding and experience of using a typical Domain Driven Design application architecture.

## Project Architecture for Domain Modeling

A SpringBoot [project structure](https://github.com/damonYuan/ddd_example) based on the DDD design pattern is typically divided as follows

```
.
├── README.md
├── build.gradle
├── gradle
│   └── wrapper
│       ├── gradle-wrapper.jar
│       └── gradle-wrapper.properties
├── gradlew
├── gradlew.bat
├── settings.gradle
└── src
    ├── main
    │   ├── java
    │   │   └── com
    │   │       └── damonyuan
    │   │           └── ddd
    │   │               ├── DddApplication.java
    │   │               ├── config
    │   │               │   ├── SwaggerConfig.java
    │   │               │   └── package-info.java
    │   │               ├── controller
    │   │               │   ├── OrderController.java
    │   │               │   ├── ProductController.java
    │   │               │   └── package-info.java
    │   │               ├── dao
    │   │               │   ├── OrderMapper.java
    │   │               │   ├── OrderProductMapper.java
    │   │               │   ├── ProductMappter.java
    │   │               │   └── package-info.java
    │   │               ├── domain
    │   │               │   ├── Order.java
    │   │               │   ├── OrderProduct.java
    │   │               │   ├── Product.java
    │   │               │   └── package-info.java
    │   │               ├── service
    │   │               │   ├── IOrderService.java
    │   │               │   ├── IProductService.java
    │   │               │   ├── dto
    │   │               │   │   ├── OrderDetails.java
    │   │               │   │   └── package-info.java
    │   │               │   ├── impl
    │   │               │   │   └── package-info.java
    │   │               │   └── package-info.java
    │   │               └── utils
    │   │                   ├── RegexHelper.java
    │   │                   └── package-info.java
    │   └── resources
    │       └── application.properties
    └── test
        └── java
            └── com
                └── damonyuan
                    └── ddd
                        └── DddApplicationTests.java
```

where

- config: All SpringBoot configuration files are placed here.
- controller: the API entry controller file for all projects
- dao: Data Access Object, also called repository in some projects (e.g. Hibernate), where classes are responsible for processing data additions, deletions, modifications, and queries to the database, other APIs, or the file system.
- domain: from the database point of view can be better understood, a domain class generally represents a table in the database, with the corresponding fields in the database table structure.
- service: This directory stores the Interface of all services, which is the place where business logic is processed, and decoupling the definition and implementation of business logic through the Interface is a prerequisite for unit testing.
- service/dto: Sometimes the value returned by the API defined by the controller may not be exactly the data represented by the domain, it may be a collection of multiple domains. In this case, a Data Transfer Object is needed to aggregate and transform the data to meet the front-end requirements.
- service/impl: the classes in this directory are implementations of service interfaces.
- utils: This directory contains utility classes, which should generally only contain static methods.

## 4 patterns of the domain model

[Domain Driven Design](https://martinfowler.com/tags/domain%20driven%20design.html) is a design pattern proposed by Martin Fowler, and its models can be divided into 4 major categories:

1. blood loss pattern: Simply put, that is, the Domain Object only attributes of the getter/setter method of the pure data class, all the business logic is completely completed by Service
2. Anemia Model ([Anemia Domain Model](https://martinfowler.com/bliki/AnemicDomainModel.html)): Simply put, the Domain Object contains domain logic that does not depend on persistence, while those that do depend on persistence are not. logic (that deals with the DAO layer) is separated to the Service layer.
3. engorgement model: the engorgement model is similar to the second model, the difference is how to divide the business logic, i.e., it is believed that most of the business logic should be placed inside the Domain Object (including persistence logic), and the Service layer should be a very thin layer, only encapsulating transactions and a small amount of logic, and do not deal with the DAO layer
4. Bloat mode: Eliminate the Service layer, leaving only the Domain Object and DAO layers, and encapsulate transactions on top of the Domain Object.

### Blood loss mode

In this mode, the Domain Object is just a mapping of database tables to POJOs (Plain Old Java Objects).

  ```
  Controller -> Service (domain logic) -> DAO -> Domain
                    |                              ^
                    └------------------------------|
  ```

Pros.

- Simple. All business logic is implemented at the Service layer, and the Service accesses the data represented by the Domain by calling methods in the DAO.

Cons: Anti-OOP.

- Anti-OOP: According to the concept of Object-Oriented Programming, an Object should include the methods of the thing it represents. For example, an instance of `cat` generated by the `Cat` class should have the method `meow()`, and should not put the method `meow()` into `CatService`.

### Anemia Domain Model ([Anemia Domain Model](https://martinfowler.com/bliki/AnemicDomainModel.html))

The most important difference between this model and the Anemia model above is that both the Domain Object and the Service Implementation contain domain logic, which is classified by the following criteria

- Domain logic that depends on persistence (in other words, needs to read and write data to the database through the DAO) is separated into the Service tier
- Domain logic that does not depend on persistence is contained in the Domain

This model has been advocated by Martin Fowler.

Pros.

- Unidirectional dependencies between layers, clear structure, easy to implement and maintain.

  ```
  Controller -> Service (persistence-releted domain logic) -> DAO -> Domain (non-persistence-related domain logic)
                  |                                                    ^
                  └─---------------------------------------------------| 
  ```
- Simple and easy to design, the underlying model is very stable

Disadvantages.

- The more tightly dependent persistence Domain Logic is separated into the Service layer, which is not Object-Oriented enough.
- Service layer is too heavy

### Engorgement Model

The congestion model is similar to the second model, but the difference is how the business logic is divided, i.e., it is believed that most of the business logic should be placed inside the Domain Object (including the persistence logic), and the Service layer should be a very thin layer, encapsulating only the transaction and a small amount of logic, and not dealing with the DAO layer.

  ```
  Controller -> Service -> Domain (domain logic) <--> DAO
  ```

Pros.

- More in line with OO principles
- Service layer is very thin, acts as a facade and doesn't deal with DAOs.

Disadvantages.

- DAO and Domain Object form a two-way dependency, and the complexity of this two-way dependency can lead to many potential problems.
- How to divide Service layer logic and Domain layer logic is very ambiguous, and in real projects, due to differences in design and developer level, it may lead to confusion and disorder in the whole structure.
- Considering the transaction encapsulation feature of the Service layer, the Service layer must provide corresponding transaction encapsulation methods for all Domain Object logic, and as a result, the Service layer has to completely redefine all the domain logic, which is very cumbersome and makes no difference to the anemia model.

### Bloated blood model

Eliminate the Service layer, leaving only the Domain Object and DAO layers, and encapsulate transactions on top of the Domain Object.

  ```
  Controller -> Domain (domain logic) <--> DAO
  ```

Ruby on Rails is this model, and even Domain and DAO are directly merged, but this is partly due to Ruby as a language. Ruby's dynamic metaprogramming features and inheritance between modules enable logical layering, AOP, and Dependency Injection at the compiler level, so that the mock & stub handles that Unit Tests rely on don't need to be implemented through the layering of design patterns and the interface of Java. interface in Java.

In a Java project, the advantages are: - Simplified Layering

- Simplified layering
- More consistent with OO

Disadvantages.

- A lot of service logic that is not domain logic is forced into the Domain Object, causing instability in the Domain Object model.
- The Domain Object exposes too much information to the controller layer, which may cause unexpected side effects.

## Controller vs Service

The general criteria for what should be placed in a Controller are as follows. 1:

1. API endpoints exposure should be placed in the Controller. 2.
2. security-related logic, such as param filter, user authentication & authorization, should be placed in the Controller.
3. the validation of received data in the Request should be placed in the Controller
4. Next, the Controller should call the Service method and pass in the parameters to get the dto response back to the front-end.

## Lessons Learned

Among these four models, the blood loss model and the blood swelling model should not be advocated. The anemia model and the congestion model are technically feasible. But I personally still advocate the use of the anemia model. The reasons:

1. the third disadvantage of the congestive model is that the Service layer is just a mapping of the Domain layer, which is no longer meaningful, but increases the workload (a domain logic defined in the Domain needs to be packaged again in the Service layer)
2. the third disadvantage of the reference congestion model, the logic of different Services is concentrated in a Domain, which inevitably makes the Domain and its heavy weight
3. the bidirectional dependency between domain object and DAO in a large project, considering the level difference of the team members, it is easy to introduce unpredictable potential bugs.
4. The standard of how to divide domain logic and service logic is uncertain, often based on personal experience, some people just think that a certain business is closer to the domain, and some people think that this business is close to the service. Due to the uncertainty of the delineation standard, the consequence is that there will be a lot of disputes and disputes in the actual project. Different people will have different delineation methods, which will finally cause the logic layering of the whole project to be chaotic. This is not like the anemia model I proposed according to whether to rely on persistence for the division, this standard is very certain, will not cause controversy, so the team development, will not produce such problems!
5. anemia model of the domain object is indeed not rich enough, but we are doing the project, not to do research, good to use on the line, regardless of whether it is so pure OO it?
