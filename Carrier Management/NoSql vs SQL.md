# NoSql vs SQL

https://www.geeksforgeeks.org/sql-vs-nosql-which-one-is-better-to-use/ 
- https://integrant.com/blog/when-to-use-sql-vs-nosql/ 

 While a relational database is a viable option most times, it is unsuited for large datasets and big data analysis. This is the major reason for the popularity of the NoSQL database systems in major Internet companies like _Google_, _Yahoo_, _Amazon_, etc.

Shakespeare was probably not thinking about databases when he wrote this line but this is still the critical question most companies face these days. The biggest decision when it comes to choosing a database is picking a relational database (SQL) or a non-relational (NoSQL) database. While a relational database is a viable option most times, it is unsuited for large datasets and big data analysis. This is the major reason for the popularity of the NoSQL database systems in major Internet companies like _Google_, _Yahoo_, _Amazon_, etc.

![SQL-Vs-NoSQL](https://media.geeksforgeeks.org/wp-content/cdn-uploads/20191104165821/SQL-Vs-NoSQL1.png)

However, **the decision to choose a database is not that simple** (what is really?!!). Both the SQL and NoSQL databases have different structures and different data storage methods. So the choice between SQL vs NoSQL essentially boils down to the type of database that is required for a particular project.

### What’s so different?

Both SQL and NoSQL databases serve the same purpose i.e. storing data but they go about it in vastly different ways. There are multiple differences between the SQL and NoSQL databases and it is important to understand them in order to make an informed choice about the type of database required.

Keeping that in mind, some of the important differences between the SQL and NoSQL databases are given as follows:

### 1. Language:

Let’s imagine that in the database world, everyone speaks X Language. So it would be quite confusing if you started speaking Y language in the middle of that. This is the case with SQL databases. The SQL databases manipulate the data based on SQL which is one of the most versatile and widely-used language options available. While this makes it a safe choice especially for complex queries, it can also be restrictive. This is because it requires the use of predefined schemas to determine the structure of data before you work with it and changing the structure can be quite confusing (like using Y language).

Now again imagine a database world where multiple languages like are spoken. While this world would be a little chaotic, speaking Y language would be fine because you would be sure to find a fellow idiot! This is a NoSQL database that has a dynamic schema for unstructured data. Here, data is stored in many ways which means it can be document-oriented, column-oriented, graph-based, etc. This flexibility means that documents can be created without having a defined structure and so each document can have its own unique structure.

### 2. Scalability

Think about a tall building in your neighborhood. If given the option, would it be better to add more floors in this building or create a new building entirely for more residents?

This is the problem for SQL and NoSQL databases. SQL databases are vertically scalable. This means that the load on a single server can be increased by increasing things like RAM, CPU, or SSD. (More floors can be added to this building). On the other hand, NoSQL databases are horizontally scalable. This means that more traffic can be handled by sharding, or adding more servers in your NoSQL database. (More buildings can be added to the neighborhood).

In the long run, it is better to add more buildings than floors as that is more stable (Less chance of creating a Leaning Tower of Pisa!!!). Thus, NoSQL can ultimately become larger and more powerful, making NoSQL databases the preferred choice for large or ever-changing data sets.

### 3. Schema Design

A schema refers to the blueprint of a database i.e how the data is organized. The schema of an SQL database and a NoSQL database is markedly different. Let’s use a joke to better understand this.

![NoSQL-Meme](https://media.geeksforgeeks.org/wp-content/cdn-uploads/20190513170557/NoSQL-Meme.png)

This basically means that the poor database admins couldn’t find a table in NoSQL because there is no standard schema definition for NoSQL databases. They are either key-value pairs, document-based, graph databases or wide-column stores depending on the requirements. On the other hand, if those database admins had gone to a SQL bar, they certainly would have found tables as SQL databases have a table-based schema.

This difference in schema makes relational SQL databases a better option for applications that require multi-row transactions such as an accounting system or for legacy systems that were built for a relational structure. However, NoSQL databases are much better suited for big data as flexibility is an important requirement which is fulfilled by their dynamic schema.

### 4. Community

SQL is a mature technology(_Like your old but very wise Uncle_) and there are many experienced developers who understand it. Also, great support is available for all SQL databases from their vendors. There are even a lot of independent consultants who can help with the SQL database for very large scale deployments.

On the other hand, NoSQL is comparatively new(_The young and Fun Cousin!_) and so some NoSQL databases are reliant on community support. Also, only limited outside experts are available for setting up and deploying large scale NoSQL deployments.

### The Big Questions!!!

NoSQL is a recent technology compared to SQL. So naturally, there are lots of questions in regards to it especially in the context of big data and data analytics. Some of the major questions relating to this are addressed below:

### Is NoSQL faster than SQL?

In general, NoSQL is not faster than SQL just as SQL is not faster than NoSQL. For those that didn’t get that statement, it means that speed as a factor for SQL and NoSQL databases depends on the context.

SQL databases are normalized databases where the data is broken down into various logical tables to avoid data redundancy and data duplication. In this scenario, SQL databases are faster than their NoSQL counterparts for joins, queries, updates, etc.

On the other hand, NoSQL databases are specifically designed for unstructured data which can be document-oriented, column-oriented, graph-based, etc. In this case, a particular data entity is stored together and not partitioned. So performing read or write operations on a single data entity is faster for NoSQL databases as compared to SQL databases.

### Is NoSQL better for Big Data Applications?

They say “_Necessity is the Mother of Invention!_” and that certainly turned out to be true in the case of NoSQL. The NoSQL databases for big data were specifically developed by the top internet companies such as Google, Yahoo, Amazon, etc. as the existing relational databases were not able to cope with the increasing data processing requirements.

NoSQL databases have a dynamic schema that is much better suited for big data as flexibility is an important requirement. Also, large amounts of analytical data can be stored in NoSQL databases for predictive analysis. An example of this is data from various social media sites such as Instagram, Twitter, Facebook, etc. NoSQL databases are horizontally scalable and can ultimately become larger and more powerful if required. All of this makes NoSQL databases the preferred choice for big data applications.

### And Finally the Conclusion!!!

The choice between SQL and NoSQL depends entirely on individual circumstances as both of them have advantages as well as disadvantages. SQL databases are long established with fixed schema design and a set structure. They are ideal for applications that require multi-row transactions such as an accounting system or for legacy systems that were built for a relational structure.

On the other hand, NoSQL databases are easily scalable, flexible and simple to use as they have no rigid schema. They are ideal for applications with no specific schema definitions such as content management systems, big data applications, real-time analytics, etc.


**When to use SQL instead of NoSQL**

01

You’re working with complex queries and reports.

With SQL you can build one script that retrieves and presents your data. NoSQL doesn’t support relations between data types. Running queries in NoSQL is doable, but much slower.

02

You have a high transaction application.

SQL databases are a better fit for heavy duty or complex transactions because it’s more stable and ensure data integrity.

03

You need to ensure ACID compliance.

(Atomicity, Consistency, Isolation, Durability) or defining exactly how transactions interact with a database. 

04

You don’t anticipate a lot of changes or growth.

If you’re not working with a large volume of data or many data types, NoSQL would be overkill.

**When to use NoSQL instead of SQL**

01

You are constantly adding new features, functions, data types.

It’s difficult to predict how the application will grow over time.

02

Changing a data model is SQL is clunky and requires code changes.

A lot of time is invested designing the data model because changes will impact all or most of the layers in the application.

03

In NoSQL, we are working with a highly flexible schema design or no predefined schema.

The data modeling process is iterative and adaptive. Changing the structure or schema will not impact development cycles or create any downtime for the application.

04

You are not concerned about data consistency and 100% data integrity is not your top goal.

This is related to the above SQL requirement for ACID compliance. For example, with social media platforms, it isn’t important if everyone sees your new post at the exact same time, which means data consistency is not a priority.

05

You have a lot of data, many different data types, and your data needs will only grow over time.

NoSQL makes it easy to store all different types of data together and without having to invest time into defining what type of data you’re storing in advance.

06

Your data needs scale up, out, and down.

As discussed above, NoSQL provides much greater flexibility and the ability to control costs as your data needs change.

## Comparing SQL and NoSQL databases

When deciding between SQL and NoSQL databases, it can help to see a side-by-side comparison of the two types to better understand their differences. The following table breaks down many of the main characteristics that set SQL and NoSQL apart.

![[Pasted image 20220629144309.png]]
![[Pasted image 20220629144330.png]]

## How to choose between SQL and NoSQL databases

The decision between SQL and NoSQL will depend largely on the workloads you plan to support and the structure and amount of data. However, you should also consider the differences in the database products themselves, such as maturity, stability, licensing fees, vendor support, and the extent and participation of the developer communities.

In the meantime, the following table provides a few general guidelines you might consider when weighing one type against the other.

![[Pasted image 20220629144243.png]]