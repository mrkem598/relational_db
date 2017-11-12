***
# Introduction to database design
***

## This article/tutorial will teach the basis of relational database design and explains how to make a good database design.

It is a rather long text, but we advise to read all of it. Designing a database is in fact fairly easy, but there are a few rules to stick to. It is important to know what these rules are, but more importantly is to know why these rules exist, otherwise you will tend to make mistakes!
Standardization makes your data model flexible and that makes working with your data much easier. Please, take the time to learn these rules and apply them! The database used in this article is designed with our database design and modeling tool DeZign for Databases.

A good database design starts with a list of the data that you want to include in your database and what you want to be able to do with the database later on. This can all be written in your own language, without any SQL. In this stage you must try not to think in tables or columns, but just think: "What do I need to know?" Don't take this too lightly, because if you find out later that you forgot something, usually you need to start all over. Adding things to your database is mostly a lot of work.

Identifying Entities
The types of information that are saved in the database are called 'entities'. These entities exist in four kinds: people, things, events, and locations. Everything you could want to put in a database fits into one of these categories. If the information you want to include doesn't fit into these categories, than it is probably not an entity but a property of an entity, an attribute.
To clarify the information given in this article we'll use an example. Imagine that you are creating a website for a shop, what kind of information do you have to deal with? In a shop you sell your products to customers. The "Shop" is a location; "Sale" is an event; "Products" are things; and "Customers" are people. These are all entities that need to be included in your database.

But what other things are happening when selling a product? A customer comes into the shop, approaches the vendor, asks a question and gets an answer. "Vendors" also participate, and because vendors are people, we need a vendors entity.

Entities: types of information
Figure 1: Entities: types of information.
Identifying Relationships
The next step is to determine the relationships between the entities and to determine the cardinality of each relationship. The relationship is the connection between the entities, just like in the real world: what does one entity do with the other, how do they relate to each other? For example, customers buy products, products are sold to customers, a sale comprises products, a sale happens in a shop.
The cardinality shows how much of one side of the relationship belongs to how much of the other side of the relationship. First, you need to state for each relationship, how much of one side belongs to exactly 1 of the other side. For example: How many customers belong to 1 sale?; How many sales belong to 1 customer?; How many sales take place in 1 shop?

You'll get a list like this: (please note that 'product' represents a type of product, not an occurance of a product)

Customers --> Sales; 1 customer can buy something several times
Sales --> Customers; 1 sale is always made by 1 customer at the time
Customers --> Products; 1 customer can buy multiple products
Products --> Customers; 1 product can be purchased by multiple customers
Customers --> Shops; 1 customer can purchase in multiple shops
Shops --> Customers, 1 shop can receive multiple customers
Shops --> Products; in 1 shop there are multiple products
Products --> Shops; 1 product (type) can be sold in multiple shops
Shops --> Sales; in 1 shop multiple sales can me made
Sales --> Shops; 1 sale can only be made in 1 shop at the time
Products --> Sales; 1 product (type) can be purchased in multiple sales
Sales --> Products; 1 sale can exist out of multiple products
Did we mention all relationships? There are four entities and each entity has a relationship with every other entity, so each entity must have three relationships, and also appear on the left end of the relationship three times. Above, 12 relationships were mentioned, which is 4*3, so we can conclude that all relationships were mentioned.

Now we'll put the data together to find the cardinality of the whole relationship. In order to do this, we'll draft the cardinalities per relationship. To make this easy to do, we'll adjust the notation a bit, by noting the 'backward'-relationship the other way around:

Customers --> Sales; 1 customer can buy something several times
Sales --> Customers; 1 sale is always made by 1 customer at the time
The second relationship we will turn around so it has the same entity order as the first. Please notice the arrow that is now faced the other way!

Customers <-- Sales; 1 sale is always made by 1 customer at the time
Cardinality exists in four types: one-to-one, one-to-many, many-to-one, and many-to-many. In a database design this is indicated as: 1:1, 1:N, M:1, and M:N. To find the right indication just leave the '1'. If there is a 'many' on the left side, this will be indicated with 'M', if there is a 'many' on the right side it is indicated with 'N'.

Customers --> Sales; 1 customer can buy something several times; 1:N.
Customers <-- Sales; 1 sale is always made by 1 customer at the time; 1:1.
The true cardinality can be calculated through assigning the biggest values for left and right, for which 'N' or 'M' are greater than '1'. In thisexample, in both cases there is a '1' on the left side. On the right side, there is a 'N' and a '1', the 'N' is the biggest value. The total cardinality is therefore '1:N'. A customer can make multiple 'sales', but each 'sale' has just one customer.

If we do this for the other relationships too, we'll get:

Customers --> Sales; --> 1:N
Customers --> Products; --> M:N
Customers --> Shops; --> M:N
Sales --> Products; --> M:N
Shops --> Sales; --> 1:N
Shops --> Products; --> M:N
So, we have two '1-to-many' relationships, and four 'many-to-many' relationships.

Figure 2: Relationships between the entities.
Between the entities there may be a mutual dependency. This means that the one item cannot exist if the other item does not exist. For example, there cannot be a sale if there are no customers, and there cannot be a sale if there are no products.

The relationships Sales --> Customers, and Sales --> Products are mandatory, but the other way around this is not the case. A customer can exist without sale, and also a product can exist without sale. This is of importance for the next step.

Recursive Relationships

Sometimes an entity refers back to itself. For example, think of a work hierarchy: an employee has a boss; and the bosschef is an employee too. The attribute 'boss' of the entity 'employees' refers back to the entity 'employees'.
In an ERD (see next chapter) this type of relationship is a line that goes out of the entity and returns with a nice loop to the same entity.

Redundant Relationships

Sometimes in your model you will get a 'redundant relationship'. These are relationships that are already indicated by other relationships, although not directly.
In the case of our example there is a direct relationships between customers and products. But there are also relationships from customers to sales and from sales to products, so indirectly there already is a relationship between customers and products through sales. The relationship 'Customers <----> Products' is made twice, and one of them is therefore redundant. In this case, products are only purchased through a sale, so the relationships 'Customers <----> Products' can be deleted. The model will then look like this:


Figure 3: Relationships between the entities.
Solving Many-to-Many Relationships

Many-to-many relationships (M:N) are not directly possible in a database. What a M:N relationship says is that a number of records from one table belongs to a number of records from another table. Somewhere you need to save which records these are and the solution is to split the relationship up in two one-to-many relationships.
This can be done by creating a new entity that is in between the related entities. In our example, there is a many-to-many relationship between sales and products. This can be solved by creating a new entity: sales-products. This entity has a many-to-one relationship with Sales, and a many-to-one relationship with Products. In logical models this is called an associative entity and in physical database terms this is called a link table or junction table.

many to many relationship
associative entity
Figure 4: Many to many relationship implementation via associative entity.
In the example there are two many-to-many relationships that need to be solved: 'Products <----> Sales', and 'Products <----> Shops'. For both situations there needs to be created a new entity, but what is that entity?

For the Products <----> Sales relationship, every sale includes more products. The relationship shows the content of the sale. In other words, it gives details about the sale. So the entity is called 'Sales details'. You could also name it 'sold products'.

The Products <----> Shops relationship shows which products are available in which the shops, also known as 'stock'. Our model would now look like this:


Figure 5: Model with link tables Stock and Sales_details.
Identifying Attributes
The data elements that you want to save for each entity are called 'attributes'.
About the products that you sell, you want to know, for example, what the price is, what the name of the manufacturer is, and what the type number is. About the customers you know their customer number, their name, and address. About the shops you know the location code, the name, the address. Of the sales you know when they happened, in which shop, what products were sold, and the sum total of the sale. Of the vendor you know his staff number, name, and address. What will be included precisely is not of importance yet; it is still only about what you want to save.

an entity with several attributes
Figure 6: Entities with attributes.
Derived Data

Derived data is data that is derived from the other data that you have already saved. In this case the 'sum total' is a classical case of derived data. You know exactly what has been sold and what each product costs, so you can always calculate how much the sum total of the sales is. So really it is not necessary to save the sum total.
So why is it saved here? Well, because it is a sale, and the price of the product can vary over time. A product can be priced at 10 euros today and at 8 euros next month, and for your administration you need to know what it cost at the time of the sale, and the easiest way to do this is to save it here. There are a lot of more elegant ways, but they are too profound for this article.

Presenting Entities and Relationships: Entity Relationship Diagram (ERD)
The Entity Relationship Diagram (ERD) gives a graphical overview of the database. There are several styles and types of ER Diagrams. A much-used notation is the 'crowfeet' notation, where entities are represented as rectangles and the relationships between the entities are represented as lines between the entities. The signs at the end of the lines indicate the type of relationship. The side of the relationship that is mandatory for the other to exist will be indicated through a dash on the line. Not mandatory entities are indicated through a circle. "Many" is indicated through a 'crowfeet'; de relationship-line splits up in three lines.
In this article we make use of DeZign for Databases to design and present our database.

A 1:1 mandatory relationship is represented as follows:


Figure 7: Mandatory one to one relationship.
A 1:N mandatory relationship:

one to many relationship
Figure 8: Mandatory one to many relationship.
A M:N relationship is:


Figure 9: Mandatory many to many relationship.
The model of our example will look like this:

relation/connection between two entities
Figure 10: Model with relationships.
Assigning Keys
Primary Keys

A primary key (PK) is one or more data attributes that uniquely identify an entity. A key that consists of two or more attributes is called a composite key. All attributes part of a primary key must have a value in every record (which cannot be left empty) and the combination of the values within these attributes must be unique in the table.
In the example there are a few obvious candidates for the primary key. Customers all have a customer number, products all have a unique product number and the sales have a sales number. Each of these data is unique and each record will contain a value, so these attributes can be a primary key. Often an integer column is used for the primary key so a record can be easily found through its number.

Link-entities usually refer to the primary key attributes of the entities that they link. The primary key of a link-entity is usually a collection of these reference-attributes. For example in the Sales_details entity we could use the combination of the PK's of the sales and products entities as the PK of Sales_details. In this way we enforce that the same product (type) can only be used once in the same sale. Multiple items of the same product type in a sale must be indicated by the quantity.

In the ERD the primary key attributes are indicated by the text 'PK' behind the name of the attribute. In the example only the entity 'shop' does not have an obvious candidate for the PK, so we will introduce a new attribute for that entity: shopnr.

Foreign Keys

The Foreign Key (FK) in an entity is the reference to the primary key of another entity. In the ERD that attribute will be indicated with 'FK' behind its name. The foreign key of an entity can also be part of the primary key, in that case the attribute will be indicated with 'PF' behind its name. This is usually the case with the link-entities, because you usually link two instances only once together (with 1 sale only 1 product type is sold 1 time).
If we put all link-entities, PK's and FK's into the ERD, we get the model as shown below. Please note that the attribute 'products' is no longer necessary in 'Sales', because 'sold products' is now included in the link-table. In the link-table another field was added, 'quantity', that indicates how many products were sold. The quantity field was also added in the stock-table, to indicate how many products are still in store.

primary keys and foreign keys
Figure 11: Primary keys and foreign keys.
Defining the Attribute's Data Type
Now it is time to figure out which data types need to be used for the attributes. There are a lot of different data types. A few are standardized, but many databases have their own data types that all have their own advantages. Some databases offerthe possibility to define your own data types, in case the standard types cannot do the things you need.
The standard data types that every database knows, and are most-used, are: CHAR, VARCHAR, TEXT, FLOAT, DOUBLE, and INT.

Text:

CHAR(length) - includes text (characters, numbers, punctuations...). CHAR has as characteristic that it always saves a fixed amount of positions. If you define a CHAR(10) you can save up to ten positions maximum, but if you only use two positions the database will still save 10 positions. The remaining eight positions will be filled by spaces.
VARCHAR(length) - includes text (characters, numbers, punctuation...). VARCHAR is the same as CHAR, the difference is that VARCHAR only takes as much space as necessary.
TEXT - can contain large amounts of text. Depending on the type of database this can add up to gigabytes.
Numbers:

INT - contains a positive or negative whole number. A lot of databases have variations of the INT, such as TINYINT, SMALLINT, MEDIUMINT, BIGINT, INT2, INT4, INT8. These variations differ from the INT only in the size of the figure that fits into it. A regular INT is 4 bytes (INT4) and fits figures from -2147483647 to +2147483646, or if you define it as UNSIGNED from 0 to 4294967296. The INT8, or BIGINT, can get even bigger in size, from 0 to 18446744073709551616, but takes up to 8 bytes of diskspace, even if there is just a small number in it.
FLOAT, DOUBLE - The same idea as INT, but can also store floating point numbers. . Do note that this does not always work perfectly. For instance in MySQL calculating with these floating point numbers is not perfect, (1/3)*3 will result with MySQL's floats in 0.9999999, not 1.
Other types:

BLOB - for binary data such as files.INET - for IP addresses. Also useable for netmasks.
For our example the data types are as follows:

datatypes displayed in database diagram
Figure 12: Data model displaying data types.
Normalization
Normalization makes your data model flexible and reliable. It does generate some overhead because you usually get more tables, but it enables you to do many things with your data model without having to adjust it.
Normalization, the First Form

The first form of normalization states that there may be no repeating groups of columns in an entity. We could have created an entity 'sales' with attributes for each of the products that were bought. This would look like this:

Figure 13: Not in 1st normal form.
What is wrong about this is that now only 3 products can be sold. If you would have to sell 4 products, than you would have to start a second sale or adjust your data model by adding 'product4' attributes. Both solutions are unwanted. In these cases you should always create a new entity that you link to the old one via a one-to-many relationship.

In accordance with 1st normal form
Figure 14: In accordance with 1st normal form.
Normalization, the Second Form

The second form of normalization states that all attributes of an entity should be fully dependent on the whole primary key. This means that each attribute of an entity can only be identified through the whole primary key. Suppose we had the date in the Sales_details entity:
to be normalized (primary key)
Figure 15: Not in 2nd normal form.
This entity is not according the second normalization form, because in order to be able to look up the date of a sale, I do not have to know what is sold (productnr), the only thing I need to know is the sales number. This was solved by splitting up the tables into the sales and the Sales_details table:

2nd normal form
Figure 16: In accordance with 2nd normal form.
Now each attribute of the entities is dependent on the whole PK of the entity. The date is dependent on the sales number, and the quantity is dependent on the sales number and the sold product.

Normalization, the Third Form

The third form of normalization states that all attributes need to be directly dependent on the primary key, and not on other attributes. This seems to be what the second form of normalization states, but in the second form is actually stated the opposite. In the second form of normalization you point out attributes through the PK, in the third form of normalization every attribute needs to be dependent on the PK, and nothing else.
normalize
Figure 17: Not in 3rd normal form.
In this case the price of a loose product is dependent on the ordering number, and the ordering number is dependent on the product number and the sales number. This is not according to the third form of normalization. Again, splitting up the tables solves this.

3rd normal form
Figure 18: In accordance with 3rd normal form.
Normalization, More Forms

There are more normalization forms than the three forms mentioned above, but those are not of great interest for the average user. These other forms are highly specialized for certain applications. If you stick to the design rules and the normalization mentioned in this article, you will create a design that works great for most applications.
Normalized Data Model

If you apply the normalization rules, you will find that the 'manufacturer' in de product table should also be a separate table:
data model after normalization
Figure 19: Data model in accordance with 1st, 2nd and 3d normal form.
Glossary
Attributes - detailed data about an entity, such as price, length, name

Cardinality - the relationship between two entities, in figures. For example, a person can place multiple orders.

Entities - abstract data that you save in a database. For example: customers, products.

Foreign key (FK) - a referral to the Primary Key of another table. Foreign Key-columns can only contain values that exist in the Primary Key column that they refer to.

Key - a key is used to point out records. The most well-known key is the Primary Key (see Primary Key).

Normalization - A flexible data model needs to follow certain rules. Applying these rules is called normalizing.

Primary key - one or more columns within a table that together form a unique combination of values by which each record can be pointed out separately. For example: customer numbers, or the serial number of a product.
***
# Module 3: Relational Database Design
***

Logical RDB Design
Data Modeling Process
Entity Relationship Diagramming
Enhanced Entity/Relationship Modeling
Object Modeling
RDB Specifications from ERDs
Case Study #1 (Legal Cases)
Case Study #2 (10K Race)
Case Study #3 (Car Rental Application)

### I. Logical RDB Design

Most modern computer-based systems for enterprises now include a database component in addition to application software. Thus these applications are sometimes referred to as database applications. In this module we will be discussing the issues relevant to designing large, multi-user database applications, with an emphasis on high performance transaction processing systems.

### SDLC

When building a database application, the techniques of systems analysis describe the following steps as the systems development life cycle (SDLC) process:

Preliminary investigation (a.k.a. feasibility analysis)
Requirements analysis
Design
Development
Implementation
Acceptance testing
Delivery (a.k.a. deployment)
Maintenance
Database applications actually have their own set of activities that are more database-specific than the general SDLC. This set of database development steps is sometimes referred to by names such as the database application system life cycle.

### Development Methodologies

Because database systems include both an application program (i.e., functional) component and a database component, the database application system life cycle process for constructing these two parts of the system progresses along parallel paths. The functional portion of the database application is handled by application programs. Techniques such as data flow diagrams (DFDs) are used to design application programs. Various computer aided software engineering (CASE) tools are available to assist in the design of the application software for the user interface (forms, reports, etc.). The data portion of the system, consisting of data structure and content issues is predominantly handled by the database itself. CASE tools can also be used to assist in the logical and physical design of the data portion of the system (e.g., Oracle Designer).

Traditionally in the SDLC process above, steps are conducted in a serial fashion, where one step begins when the previous step is completed. This is referred to as the "waterfall" technique. Unfortunately this can result in a long time to deliver a system and frequently results in an inability to meet the evolving end-user's requirements. One technique to minimize this problem is the use of feedback loops between the different steps, such that one step will provide enhanced detail to an earlier step, and then the process moves forward again. Therefore the development process frequently progresses using a spiral approach between steps, versus a truly sequential process. A system built in this manner may be delivered in an incremental fashion.

Other techniques that help alleviate the long time to delivery, by speeding up the design and development process, include rapid prototyping using visual programming tools, and object-oriented methodologies (e.g., Oracle Developer and Sybase PowerBuilder). The object-oriented methodology is covered in module 6 of this course and examples of development tools are presented in the advanced relational databases course.

### Conceptual Design

Following the requirements analysis phase of database application development, the conceptual design of a database is established. The conceptual design is an attempt to demonstrate that all of the end-user's data requirements have been captured and are understood by the developer. The conceptual design is independent of both the data model and any specific DBMS. The conceptual model represents a global view of data. It is an enterprise-wide representation of data as viewed by senior management.

Since this course is concerned primarily with relational databases, the data model we will use is the relational model (versus the hierarchical, network, object-oriented, or other models). Following the choice of data model, the logical design of the relational database is conducted. A logical relational database design requires building an abstract (i.e., high level) model of the data elements to be included in the database and the relationships between the data elements.

### ERDs and Physical Design

With relational databases, the most popular conceptual and logical data modeling technique is an entity/relationship diagram (ERD). An ERD is a special type of semantic model—a model that gives meaning to the data included. An ERD is commonly used to translate different views of data among management, database users, and developers to fit into a common framework, which will assist in many aspects of database development. ERDs are discussed thoroughly later in this module.

Later in the design phase of database development, the actual relational database is built as part of the physical design activity. This activity may consist of an enterprise data administrator turning over an ERD, representing a logical model of the database, to a DBA to implement to build the database, using a specific RDBMS and host platform. The physical database design phase also includes the specification of physical data layouts, indexing and issues related to database tuning for better performance. The details of relational database performance and tuning are covered in the advanced relational databases course.

Development

The database development approach with entity/relationship diagramming represents a top-down development approach, where the logical design phase is at a high level of abstraction, although the determination of entities and their entities (discussed in detail in the next section) from all of the required data elements is a bottom-up approach.

The DDL SQL used to construct the physical relational database and the DML SQL to initially populate the database will be discussed in module 4.

Implementation

Once the database is built, the implementation phase takes place. During this phase, data is added (called "populating the database"), user forms and reports are built, and end-user queries are developed. Legacy system data may also need to be added to the database, possibly using conversion routines to reformat the data for the new system.

Acceptance Testing, Delivery, and Maintenance

When the database application is complete, the end user conducts system acceptance testing and, if the system is able to meet their requirements, accepts delivery from the developer. System delivery usually includes training for the end users, the delivery of system documentation and a period (usually years) of maintenance. When the new system is no longer able to meet the end-user’s needs at some later time the SDLC process begins anew on a new system.

II. Data Modeling Process

End-User Interviews

During the requirements analysis phase there are several techniques to gather the end user's requirements, including end-user interviews, questionnaires, observing the existing operation, and conducting a functional analysis of existing system forms, reports, and documentation. Note that in this course we are stressing the gathering of requirements to develop the database, not the development of the application software.

The purpose of end-user interviews is to meet with various levels of the enterprise's end users, and in doing so, capture the data elements required, the way the data elements are related, and the ways these data elements will be entered, updated, and accessed by these users.

In a large system, the requirements analysis process consists of a series of data-gathering interview sessions in which the developers listen carefully and, through experience, ask the end users data-related questions so as to clarify the data that comprise the customer's business.

Entities, Attributes, and Relationships

The database developer strives to identify primarily three aspects of the data:

entities
attributes
relationships
Simply stated, the entities are the very important "things" that exist within the enterprise. By discussing the system requirements with the end user, the developer should be able to identify these "things" as the "nouns."

During requirements gathering, the developer should be able to identify the characteristics of the entities as attributes, or "adjectives."

Finally, the developer should identify the relationships between the entities. These relationships are like the "verbs" and "verb clauses" of the requirements. This noun-adjective-verb analogy is actually advocated by some texts as a procedure to analyze written requirements for a database application.

Business Rules

The database developer also seeks out the business rules and other requirements of the final system. Business rules describe the day-to-day processing rules for the enterprise. These rules, along with other requirements will determine the database constraints that must be included in the system.

The seasoned database developer will strive to capture business rule statements that are precise, atomic, and convey a business aspect of the end user's application. An understanding of the business rules of a system is critical to developing a system that meets the requirements of the users.

An example of a business rule might be that "all customers have unique account numbers". Another rule might be that "customers that place three or more orders per month are permitted a 10 percent discount on all subsequent orders during the remainder of the quarter." As we saw in module 2, the former rule is rather easy to include via declarative integrity (i.e., DDL SQL), while the latter might require procedural integrity techniques (e.g., triggers).

Although the business rules may not be evident from an ERD, it is important that they be captured and documented. As we shall soon see, some of the basic business rules can be captured as cardinalities on the ERD (i.e., which sides of the relationships are for the "one" side and which are for the "many" side).

III. Entity Relationship Diagramming

Once the entities, attributes, and relationships of the enterprise data are understood, the database developer constructs a data model for a relational database, usually in the form of an entity/relationship diagram. The ERD is actually a picture of the data, or a conceptual model, for an enterprise. The ERD does not show the functions the business performs, which is part of the application software design, a separate topic.

The ERD and the accompanying diagramming technique were developed by Peter Chen in 1976. There are now a number of different notations used for ERDs. Although there are no standards, there are a number of conventions.

Entities are usually indicated as rectangular boxes, and are the important "things" that have been noted by the database developer about the end-user's business. The entity names inside the boxes are in all capital letters. An example of an entity, the CUSTOMER entity, is shown in figure 3.1. Attributes are shown as words inside ovals with lines that connect to the entities of which they are characteristics. The many attributes of the CUSTOMER attributes are shown in figure 3.1, below.

Figure 3.1



Entity and Attribute Types

The CUSTOMER entity of figure 3.1 is an example of a strong entity. Strong entities have a unique key value of their own within the end user's application called a key attribute. The key attribute of an entity is the unique identifier of each instance of an entity. The Customer ID attribute is the key attribute of the CUSTOMER entity in figure 3.1. Key attributes are underlined.

A rectangular box with two borders is a special type of entity, called a weak entity. A weak entity is existence-dependent on a strong entity. Weak entities do not have unique key values of their own in the end user's application. Rather, their unique identification is partially or totally derived from the strong entity or entities upon which they are dependent.

A composite attribute is actually composed of the more atomic, simple attributes. Customer ID and Initial Contact Date are simple attributes of the CUSTOMER entity in figure 3.1, whereas Name and Address are composite attributes.

A multi-valued attribute exists for an entity when there is some indeterminate number of possible simple entities that might exist. The Phone Number attribute in figure 3.1 is an example of a multi-valued attribute, since the CUSTOMER entity may have various types of phone numbers: home phone, business phone, fax phone, cell phone number 1, cell phone number 2, etc. Multi-valued attributes are shown with a double oval.

A derived attribute can be calculated from another attribute of an entity. For the CUSTOMER entity we might have added a derived attribute of Customer Longevity, which could be calculated from the Initial Contact Date attribute.

Relationship Types

There are three types of relationships between entities in an ERD: one-to-one, one-to-many, and many-to-many. Note that the many-to-one relationship is the same as the one-to-many relationship, just viewed from the opposite direction.

As stated earlier, there are no standard notations for ERDs. The greatest variety in the ERD notations is the way in which relationships are depicted. Frequently relationships are shown as diamonds with names that are between two or more entities. As shown in figure 3.2A, which uses the Chen notation, the "1" and "M" indicators represent the types of relationships that exist between two adjacent entities. Figure 3.2B shows an alternate notation using "crow's feet" on the "many side" of the relationship. Note that there are a number of other popular ERD notations besides the two shown below.

Figure 3.2A



Figure 3.2B

fig3_2b_case.gif (2283 bytes)

Relationship Sentences and Participation

One popular technique to determine the proper relationships between entities is to construct a set of sentence pairs between all related entities. All sentences must begin with the word "Each." An entity name is then listed in singular case after the word "Each." One sentence describes the relationship in one direction and the other sentence describes the relationship in the other direction. Each pair of sentences describes the overall relationship between the two entities. This technique is based on the CASE*Method approach and is used by the Oracle Designer CASE tool.

For example, if we have two entities, CUSTOMER and ORDER, as shown in figure 3.2, we might discover from the end user that the following sentence pair describes the relationship that exists between these two entities:

Each CUSTOMER may place one or more ORDERs.
Each ORDER is placed by a CUSTOMER.
Notice in figure 3.2A that the relationship verb "place" is inside a diamond. In figure 3.2B there are two relationship verbs placed at either end of the single relationship line. This latter notation more easily facilitates the sentence pair strategy since two verbs (or verb clauses) are available (versus a single verb).

The first of these sentences describes a one-to-many relationship with a partial (or optional) participation (i.e., CUSTOMERS might not place ORDERs). The second sentence describes a one-to-one relationship with a total (or mandatory) participation (i.e., ORDERs are always placed by a CUSTOMER). The "may place" optional participation is indicated by a single line by the CUSTOMER entity in figure 3.2A and the oval by the ORDER entity in figure 3.2B. The "is placed by" mandatory participation is indicated by the double line from the ORDER entity in figure 3.2A and the hash mark by the CUSTOMER entity in figure 3.2B. Be sure you can correlate all parts of the sentence pair above to both of the ERD portions in figure 3.2.

It is the total of both of these individual relationship sentences that determines the overall relationship. In this case the individual one-to-many and one-to-one individual relationships form an overall one-to-many relationship.

Bridge Entities

In other cases, we might have an overall many-to-many relationship from two individual one-to-many relationship sentences. In this situation we must form a bridge entity between the two original entities, such that each of the original entities will be related to the bridge in a one-to-many relationship. For example, the sentence pair below (from a hypothetical hospital application) results in an overall many-to-many relationship (refer to the ERD diagram of figure 3.3A):

Each DOCTOR treats one or more PATIENTs.
Each PATIENT is treated by one or more DOCTORs.
Figure 3.3A

fig3_3bridgeentity_a.gif (2168 bytes)

Once the bridge entity is formed, the original sentence pair is no longer valid, since there is no longer a direct relationship between those entities. Two new sentence pairs must be formed between each of the original entities and the bridge entity.

For the above sentences we might have a bridge entity of TREATMENT. After forming this new entity, the above two sentences would be replaced by the two new sentence pairs below, in figure 3.3B:

Figure 3.3B



Each DOCTOR is part of one or more TREATMENTs.
Each TREATMENT involves a DOCTOR.
Each TREATMENT involves a PATIENT.
Each PATIENT is part of one or more TREATMENTs.
Note that each of these new sentence pairs describes an overall one-to-many relationship with the bridge entity on the many side of the relationship and the original two entities on the one side of the relationship. Bridge entities are also referred to as association entities or intersection entities in different textbooks. A bridge entity results from the many-to-many relationship itself.

Bridge entities are weak entities, whose key values are frequently a composite of the two strong entities' key values to which they are related. The key value for TREATMENT above might be the composite of Patient ID and Doctor ID. However, a surrogate key Treatment ID might be more appropriate in this situation, as the stated composite may not be unique (the same doctor can likely provide multiple treatments to the same patient).

Relationship Degrees

The number of entities that participate a relationship is called the degree of the relationship. The relationships discussed thus far have included two entities and are referred to as binary relationships. Another relationship worth noting is the classic PART entity, with its "part structure" relationship, as shown in figure 3.4, below.

Figure 3.4

fig3_4recursiverelation.gif (1684 bytes)

In this relationship, a PART is a component of a larger PART, which may be a component of yet a larger part, and so on, in a recursive relationship. A recursive relationship, where a single entity has a relationship with itself, is also referred to as a unary relationship. The relationship sentences describing the relationship in figure 3.4 are:

Each PART includes one or more PARTs.
Each PART is included in a PART.
In some applications we might have three entities that relate to a fourth bridge entity. This is a ternary relationship. Each of the three entities is related to the bridge entity by a pair of relationship sentences such that there are three sentence pairs in all. An example of a ternary relationship would be where sponsors are used to raise funds to support various charities, as shown in figure 3.5, below.

Figure 3.5

fig3_5rernaryrelation.gif (5335 bytes)

 

Thus we might have the three entities SPONSOR, AGENCY, and CHARITY, with a bridge entity of FUND relating them. Our relationship sentence pairs would then be:

Each SPONSOR provides money for one or more FUNDs.
Each FUND is financed by a SPONSOR.
Each AGENCY manages one or more FUNDs.
Each FUND is managed by an AGENCY.
Each CHARITY receives money from one or more FUNDs.
Each FUND may provide money to a CHARITY.
Note that in this example we have chosen to utilize a ternary relationship because all three of the major entities SPONSOR, AGENCY, and CHARITY, are assumed to be interrelated. If only two of these three entities could be related at any time we would do better to model this as three binary relationships.

It is also possible that two different entities have multiple relationships between them, and these relationships might even be of different degrees.

HAS-A versus IS-A Relationships

The relationships discussed thus far are all known as "HAS-A" relationships. There is another class of relationships known as "IS-A" relationships. The IS-A relationships exist in a type hierarchy of entities. In this type of relationship we might have a parent entity of EMPLOYEE, and then child entities of PROGRAMMER, DBA, and CLERK. In this particular relationship, sentences such as the following exist:

Each EMPLOYEE may be a PROGRAMMER, DBA, or CLERK.
Each PROGRAMMER IS-AN EMPLOYEE.
Each DBA IS-AN EMPLOYEE.
Each CLERK IS-AN EMPLOYEE.
The IS-A relation types are involved in generalizations and specializations, which will be discussed and in more detail, along with figures, in section IV of this module.

When developing an ERD, and preparing to move to the physical design phase of the database design process, you cannot have any many-to-many relationships, because it is then impossible to determine the primary key and foreign keys for the relationship. As we will see later in this module, the unique identifiers (i.e., key attributes) of the entities on the "one side" of a one-to-many relationship form primary keys and the corresponding attributes on the "many side" of the one-to-many relationship form the foreign keys.

Also, one-to-one HAS-A relationships in an ERD should generally be combined, as the difficulty of working with two tables (versus one) will result in performance degradation (the transformation of entities to tables and performance issues will also be covered in module 4). But, if one of the two one-to-one related entities has the potential to have a large number of NULLs, or is part of a type hierarchy, then leaving the one-to-one relationship might be justified.

ERD Technique Summary

In summary, when using the sentence-pair strategy discussed in conjunction with developing an ERD the sequence of events is as follows:

Determine the entities from the requirements analysis.
Determine which entities are related to one another.
Develop sentence pairs between related entities.
Draw the ERD based on sentence pair relationships.
Repeat steps 3 and 4 iteratively, as necessary, until all overall relationships between entities are reduced to one-to-many (except for type hierarchies, if any).
Use the ERD to develop tables, columns, primary keys, and foreign keys (this will be discussed in module 4).
IV. Enhanced Entity/Relationship Modeling

EER Model

The classic ERD is well suited to many traditional database applications, but many modern databases require a more robust type of modeling. To model more complex applications accurately, developers must use sophisticated semantic data modeling techniques. In this section, we will explore how some of these techniques have led to the enhanced entity/relationship (EER) model.

The EER contains all of the ERD concepts plus many useful additions. Unfortunately, as with ERDs, there is no standard notation, just different preferred notations.

Superclasses and Subclasses

In a type hierarchy, a parent entity type is called a superclass. The entities in an entity set containing the child entities are called a subclass of the parent entity type. Between the parent entity types and child entity types a superclass/subclass relationship exists. Subclass entities are specific roles of the superclass entity. Every member of a subclass is also a member of their superclass, but not every superclass member belongs to a subclass.

Similar to what we will see with object classes later in module 6, in an entity hierarchy the subclass inherits the attributes of the superclass. In addition, the subclass entities may have specific entities of their own. As you can deduce from the example in  section III, PROGRAMMERs, DBAs, and CLERKs would certainly have different attributes, such as the skills unique to their specific jobs.

An entity type actually defines a set of entities in a relational database which all have the same structure (i.e., the same set of attributes). The collection of all entities of the same type is called an entity set. The entity type describes the intension of the entity set, whereas the members of the entity set are called the extension of the entity type.

Specialization and Generalization

A specialization exists where there are multiple subclasses for a superclass, where the subclasses have attributes that differentiate them from the superclass. For example, SAILBOAT and POWER_BOAT are subclasses of the superclass RECREATIONAL_BOAT (and RECREATIONAL_BOAT is a subclass of the more general BOAT superclass) as shown in figure 3.6.

Figure 3.6

fig3_3_rec_boat.gif (9789 bytes)

Both the members of the SAILBOAT and POWER_BOAT subclasses inherit the attributes of their superclass, but they also have specific attributes of their own, which differ between "sibling" subclasses. Also, different entities at the same subclass level can be in different relationships with other entities.

If we start with multiple, similar subclasses, it might be possible create a superclass that has the common attributes of all subclasses by using a process called generalization. The specialization process is a top-down conceptual refinement analysis, where new subclasses are derived; since generalization is the reverse of specialization, it is an example of a bottom-up conceptual synthesis.

A defining predicate of a subclass determines which members of the superclass belong to the subclass in a specialization. For example, if the value of Boat_Type='Sail' for a RECREATIONAL_BOAT member, then that member would also be a member of the SAILBOAT subclass. If an attribute's value can be used to determine subclass membership, we have an attribute-defined specialization. If this is not the case, we have user-defined subclasses.

Subclasses can be disjoint or overlapping. If superclass members can belong to at most one subclass then we have a constraint where the subclasses are disjoint. If superclass members can belong to multiple subclasses, then the subclasses are overlapping. To distinguish the two types, we place either a "d" or an "o" on the circle between the superclass and the subclasses, for disjoint or overlapping, respectively.

If all members of the superclass must be members of a subclass then we have total specialization. If not, then we have partial specialization. As can be seen in figure 3.6, we have a disjoint, total specialization because of the double line and "d" in the circle. Superclasses defined through generalization must result in a total specialization hierarchy.

Hierarchies and Lattices

If each subclass can be related to only one superclass entity, then we have a specialization hierarchy. If this is not the case we have the less restrictive specialization lattice organization.

A subclass that has no subclasses of its own in a specialization hierarchy or lattice is called a leaf node. A subclass with multiple superclasses is called a shared subclass and through multiple inheritance, inherits the attributes and relationships of all its superclasses. Specialization lattices have at least one shared subclass, whereas specialization hierarchies have none.

Categories

When we have a subclass that has multiple, distinct (i.e., not from the same type hierarchy or lattice) superclasses, we have a union type or category subclass. In the EER model this is represented by a "U" in the circle between the superclasses and the single subclass. Each member of a category subclass may only belong to one of its superclasses, and inherits the attributes of only that single superclass.

V. Object Modeling

In addition to ERD and EER modeling, in the object oriented modeling realm there are two additional and very widespread data modeling techniques—Universal Modeling Language (UML) and Object Modeling Technique (OMT).

In the object-oriented paradigm, objects, which represent abstractions of real world items, are paramount. Objects are created based on instantiation of object classes that specify their structure and behavior. Objects have attributes (i.e., characteristics) and methods, which are like software functions used to define their behavior.

In UML modeling, object classes are represented by a box partitioned vertically into three sections, with the class name in the top section, the attributes in the middle section, and the methods in the bottom section. Relationships between object classes are called associations, and each instance of a relationship is called a link. UML also includes special notation for representing specializations and generalizations.

VI. RDB Specifications from ERDs

In this section we will discuss the guidelines for converting an ERD into the specifications for a relational database. Specifically, we will describe how to convert entities, attributes, and relationships into relational database tables, columns, and integrity constraints.

Tables

Each of the entities in the ERD (or EER diagram) will become a separate table in our relational database. This includes strong entities, weak entities, and bridge entities. The entities in a specialization hierarchy or specialization lattice may be either converted into individual tables or combined into a single table with differentiating columns for the discriminating attribute (if any) and all other different attributes. This decision will be based on the application and the number of differences between subclass attributes.

A good practice is to make relational database table names plural. Thus the CUSTOMER entity of figure 3.1 will become the CUSTOMERS table.

Attributes

Each of the simple attributes, and the outermost attributes of any composite attributes will become columns in the table that is formed by the entity to which they are attached. Derived attributes may be stored as columns for the table if calculating their values would have performance impacts. The data types that will be chosen for columns in the physical design phase (covered in module 4) will be determined by the domain of the attribute.

Multi-valued attributes should be used to form a new table and possibly a bridge table if needed. For example, if a CUSTOMER entity (refer to figure 3.1) has Phone Number as a multi-valued attribute, this might be better handled in the database by forming a PHONE_TYPES table and then, since this results in a many-to-many relationship with CUSTOMER, a bridge table of CUSTOMER_PHONES between CUSTOMERS and PHONE_TYPES. This will allow each customer an unlimited number of different phone numbers for different types of phones.

Keys

Although it is not required for relational database tables, good relational database design dictates that all tables should have a primary key. The unique identifier (i.e., key attribute) of each entity will become the primary key of the table. If there are multiple attributes that form the unique identifier then we will have a composite primary key.

Since the unique identifiers of categories are not based on a common superclass identifier, we must generate a surrogate key for the primary key. A surrogate key is a machine generated, usually sequential, unique numerical value that has no significance other than to provide uniqueness. Also, in cases where the number of columns and different data types for a primary key of any table becomes unwieldy, it might be advisable to generate a surrogate key for the primary key.

The many side of all one-to-many relationships will have a foreign key that references the primary key side on the one side of the same relationship. If the keys are composite, ensure that the same column order and data types exist for both the primary and referencing foreign keys. Usually the composite of the foreign keys of a bridge entity is used to form its primary key.

The specification of foreign keys not allowed to be NULL, or having cascade deletion and updating, is based on the requirements of the application and the capabilities of the RDBMS. Note, however, that total participation of an entity in a relationship implies that foreign keys cannot be NULL.

VII. Case Study #1 (Legal Cases)

In this section and in the next two sections, some simple relational database data modeling examples will be presented to reinforce the concepts presented earlier in the commentary. Learning how to design a good relational database is the most important practical goal of this course.

In this and subsequent examples, the ERD notation that will be used is that shown in figure 3.2B.

You should not be shocked to learn that numerous different ERD notations exist in texts and within vendor products at this time. Once you grasp the fundamentals, adapting to a slightly different notation for a specific design activity should not be difficult.

Case study #1 involves a lawyer who has been in practice for several years and has maintained her record of cases on 3 Ã— 5 cards. Maintaining this "database" has been cumbersome, and you have been hired to automate the process. You find that each 3 Ã— 5 card represents a different case, and includes a client's name, address, phone number(s), case number, case description, and other notes. The end user (the lawyer) wants you to include both home and work phones in the database, along with personal information and the name of the person who referred the client. Even though each card may have multiple names on it, each case will have only one client associated with it (this is a business rule!).

This brief information in the above paragraph represents the end user interview portion of the requirements analysis phase of the project. In this relatively simple application the end-user interview may have taken only an hour or two. In other cases, the requirements gathering and analysis process can take weeks or months. After asking the end user questions for clarification, you, the developer, are able to identify the entities, attributes, and relationships.

On subsequent end-user interviews to your end user, you could present an ERD to them, since an ERD conveys even to non-technical people the data involved in their business. Notice there is no discussion of what RDBMS will be used, how many tables will be developed, the method of data entry, specific forms and reports, and so on. These all come later. Based on the previous end-user interview, the ERD in figure 3.7 may be developed.

Figure 3.7

fig3_4_legalcase.gif (15432 bytes)

In this diagram there are two entities, CLIENT and CASE, which are related as follows:

Each CLIENT may generate one or more CASEs.
Each CASE involves a CLIENT.
Initially we were told that "Each CLIENT generates one or more CASEs". But in a subsequent interview, we found that there are situations in which clients do not generate a case as we have defined it. Thus the above sentences, where the first sentence involves a partial inclusion, most accurately reflect the business, and matches the ERD drawn in figure 3.7.

As you can see, our two entities have an overall one-to-many relationship. Although a CASE could have had multiple CLIENTs, in this application the end user wanted only the primary client noted for each case.

Note the "crow's feet" at the entity on the many side of the relationship. The relationship verbs (i.e., "generates" and "involves") are shown one above the relationship line and one below.

The attributes are connected to the entity for which they are a characteristic. There are two composite attributes for the CLIENT entity, Name and Address. Entity names are indicated in the singular (as opposed to CLIENTs or CASEs) on the ERD. The lawyer, even though she is obviously essential to the enterprise, is not shown in the ERD. The end user is usually not part of the data diagram, since to be an entity there must be multiple instances of the entity. If there had been multiple lawyers, or multiple offices involved in this application, then the situation and the data model might be different.

The attributes that uniquely identify each entity member are underlined. These unique identifiers will become primarily keys of the tables formed by their entities. The Case Number was provided by the end user from their 3 Ã— 5 card file. The Client ID was a surrogate key generated as a simple way to identify different clients. As you can imagine, using names would lead to eventual problems that could be avoided by using a single column surrogate key.

The case outcomes, judge assigned, trial date(s), income from the case, billing information, and so on, although interesting, were not requested by the end user, and therefore are not shown on the ERD. They could be easily added later, as relational databases are a very flexible database design.

In module 4 we will show how this ERD, and the others in the following case studies, can be converted into tables for a relational database.

VIII. Case Study #2 (10K Race)

For this database application, we will consider a community's annual fund-raising event—a 10 K (10 kilometers, or about 6.2 miles) foot race, in which 200 to 300 runners regularly participate.

The race director (our end user) informs the database developer that he is tired of manually entering the entrants onto a spreadsheet and is having an especially hard time determining the overall winners and age-group winners from the finish times of those entrants who actually compete and finish the event. Plus there is a requirement to keep runners' names and addresses to solicit their entry in each year's race via a bulk mailing. As entrants mail in their entry fee, they are checked against a list of past entrants to see if addresses have changed.

Phone numbers and e-mail addresses are not kept in the database. Entrants can be identified only by name because of privacy concerns about social security numbers, so we will probably want to generate a surrogate key to identify them. Each entrant is assigned a BIB number (the unique number they will wear in that race) for the event, but no one is guaranteed a particular number, as the same set of numbers, typically from 1 to 300, are re-used every year.

On race day, approximately 15 percent of the runners who entered do not show up, and another 50 percent beyond those pre-registered sign up on race day, just before the event starts. As runners finish, their times are recorded. But not all starters may finish the event for a variety of reasons.

Awards are given to the fastest male and female runners overall as well as to the first one to three finishers in each age group, based on the number of participants. Age groups consist of runners of the same sex who share the same "age decade" (e.g., all 20- to 29-year-old males compete as an age group, and females who are aged 60 and above compete as another age group).

Based on this end-user interview, the ERD shown in figure 3.8, below, was developed.

Figure 3-8

fig3_5_10krace.gif (17145 bytes)

Notice that there are three entities in a type hierarchy: RUNNER, ENTRANT, and FINISHER. Through an analysis of the application we deduced that each RUNNER may either be an ENTRANT or a NON_ENTRANT each year. Since we are not interested in RUNNERs that don't enter, we have not included the NON_ENTRANT entity in the ERD. In a similar manner, there are two subclasses of ENTRANT, FINISHER and NON_FINISHER, and we are not interested in the non-finishers (called DNFs for "did not finish").

The weak entity ENTRANT has all of the attributes of its strong entity RUNNER, plus some of its own attributes. Similarly, the FINISHER subclass inherits all of the ENTRANT entity attributes.

RUNNER contains attributes that should not change, as well as other information about the runner, such as the Address and Last Year Raced. Note that the derived attribute Last Year Raced could be determined by searching the ENTRANT entity's instances, but this turns out to be cumbersome and leads to poor query performance when preparing the bulk mailing.

The ENTRANT entity contains attributes that will change from year to year, such as the Year of the event, runner's Age, runner's BIB number and runner's Age Group (another derived attribute). Weak entity FINISHER is dependent on another weak entity, ENTRANT, plus it has some additional attributes of its own, such as Finish Time and Award (yet another derived attribute).

Prior to developing the ERD we noted the following relationship sentence pairs:

Each RUNNER registers as one or more ENTRANTs.
Each ENTRANT IS-A RUNNER.
Each ENTRANT may become a FINISHER.
Each FINISHER IS-AN ENTRANT.
In this ERD, the ENTRANT/FINISHER relationship is one-to-one, which is common in a type hierarchy of entities. Notice that there is full participation of the RUNNER entity in ENTRANT (you don't get into the database unless you've entered at least one race), but a partial participation of the ENTRANT entity in FINISHER (you might not finish just because you enter).

The composite attributes Name and Address are similar to those in the Legal Cases database. Last Year Raced is an attribute of RUNNER that indicates the last time the runner registered for a race and is used to help in preparing a bulk mailing to those runners most likely to participate in a new year's event. The derived attribute Award has values such as "First Place Male Overall" or "2nd Place Men 50-59". Note that specific attribute values, such as "Male" or "Female" for Gender, or the values for the derived attribute Age Group are not shown in an ERD. We only show the generic attribute names.

As an interesting note, observe that the composite unique identifier of both the ENTRANT and FINISHER entities is Year and BIB. This composite is also the foreign key of the FINISHER entity.

Many solutions to this and most applications are possible. But as long as each ERD meets the requirements for the data and queries of the application, and is properly normalized (a topic discussed in detail in module 5), the design is correct. Phrased another way, there is no single database design for every problem, but each solution must meet the requirements and result in a manageable database.

IX. Case Study #3 (Car Rental Application)

The final case study presents a company that is involved in renting a fleet of cars to customers. Obviously, to be profitable, the company rents cars as often as possible and tries to achieve repeat business with its customers.

Each car is identified by a vehicle identification number (VIN). Charges for rental cars are based on class of car rented (make, model, and size), days rented, length of rental, whether the rental takes place on weekends or holidays, mileage driven, discounts, and insurance. Customer information must be maintained along with the date and time of rental and rental pick-up and drop-off locations.

Initially the developer tried to relate entities CUSTOMER and CAR as follows:

Each CUSTOMER rents one or more CARs.
Each CAR may be rented by one or more CUSTOMERs.
This ERD portion is shown in figure 3.9A, below. These two separate relationship sentences combine to form a many-to-many relationship. As stated in the commentary earlier, for many-to-many relationships we must form a bridge entity and then split the many-to-many relationship into two new one-to-many relationships, as shown in figure 3.9B. below.

Figure 3.9A

fig3_6_custcar.gif (1981 bytes)

Figure 3.9B

fig3_7_custcarbridge.gif (3414 bytes)

The initial sentence pair is now abandoned since CUSTOMER and CAR are no longer directly related. The two new relationship sentence pairs we will use are:

Each CUSTOMER is involved in one or more RENTALs.
Each RENTAL includes a CUSTOMER.
and

Each RENTAL includes a CAR.
Each CAR may be involved in one or more RENTALs.
Note that RENTAL is really a weak entity, whose existence depends on the two strong entities, CUSTOMER and CAR. Adding the other entities and their attributes results in the final ERD for this application as shown in figure 3-10. below.

Figure 3-10



In the final ERD, we decided to add two entities: RATE_TABLE and RATE_SCHEDULE. The RATE_TABLE entity consists of separate RATE_SCHEDULE information to note the particular schedule's standard Daily Rate. This then becomes the derived Base Rate attribute of the RENTAL entity when multiplied by the number of days of the rental. Each of the RENTAL charge components is a simple attribute.

The Rental ID is a unique invoice number for each rental transaction. We chose to use this surrogate key, versus the composite of Account Number, VIN and Date/Time Out, to identify each RENTAL.

Developing the two entities RATE_SCHEDULE and RATE TABLE requires a bit of a leap in thinking. A RATE_TABLE comprises all the possible RATE_SCHEDULEs that may be in effect at any specific time. For example, at the present time, assume there are 55 different RATE_SCHEDULEs in the single, current RATE TABLE.

Each RATE SCHEDULE is like a line in a RATE_TABLE schedule. For example, one RATE_SCHEDULE would be for compact cars rented on a weekday for only one day, with a mileage charge. A second may be for the same type of car rented at the same time, also for one day, but with unlimited mileage. A third schedule may be for compact cars rented on a weekday for three or more days, with a mileage charge. Continuing this thinking, there could be a large number of RATE_SCHEDULEs, each with their own standard Daily Rate (e.g., $19/day) based on size/class of car (compact; mid-size; full size; luxury), day rented (weekday; weekend; holiday), days rented (1; 3 to 5; 6 or more) and mileage option (charge per mile; unlimited; combination of unlimited to some point, then a charge per mile).

In the next module, we will see how the logical design using an ERD is analyzed to specify the tables for a relational database, how these tables are constructed using SQL during the physical design for a relational database, and, finally, how the tables are populated with SQL.
