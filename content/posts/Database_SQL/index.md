---
title: Database Fundamentals
date: 2024-04-04

category: 数据库
tag:
  - 数据库基础
series: ["Databse and SQL"]
series_order: 1

---
Database fundamental knowledge must be understood and memorized. Although this part is just theoretical knowledge, it is very important as it lays the foundation for later learning MySQL databases. PS: This part involves many conceptual contents, so it refers to Wikipedia and Baidu Baike for the introductions.

## Databse, Database Management System, Database System, Database Administrator

- **Database**: A database (abbreviated as DB) is a collection of information, or more precisely, a collection of data managed by a database management system.
- **Database Management System**: A Database Management System (abbreviated as DBMS) is a large software used to manipulate and manage databases, typically used to establish, use, and maintain databases.
 
- **Database System**: A Database System (abbreviated as DBS) generally consists of software, the database, and a database administrator (DBA).
  
- **Database Administrator**: A Database Administrator (abbreviated as DBA) is responsible for the comprehensive management and control of the database system.

## Tuple, Key, Candidate Key, Primary Key, Prime Attribute, Non-prime Attribute
- **Tuple**: A tuple in a relational database refers to a row in a table, which represents a record in the database. Each column in the table is considered an attribute. In the context of a two-dimensional table, a tuple is also referred to as a row.
- **Key**: A key is an attribute or a set of attributes that uniquely identifies an entity in a table.
- **Candidate Key**: A candidate key is an attribute or a set of attributes that uniquely identifies a tuple in a relation, and no subset of these attributes can uniquely identify a tuple. For example, in a student entity, "student ID" can uniquely identify a student, and it's assumed that a combination of "name" and "class" can also uniquely distinguish students, thus both {student ID} and {name, class} are candidate keys.
- **Primary Key**: A primary key, also known as a primary code, is selected from among the candidate keys. There can only be one primary key in an entity set, although there may be multiple candidate keys.
Foreign Key: A foreign key, also known as an external key, is an attribute in one relation that is a primary key in another relation.
- **Prime Attribute**: Attributes that appear in any candidate key are known as prime attributes. For example, in the relation Worker (worker ID, ID number, name, gender, department), both worker ID and ID number can uniquely identify the relation, hence they are prime attributes. If the primary key is a set of attributes, then all attributes in that set are prime attributes.
- **Non-prime Attribute**: Attributes that are not included in any candidate key are called non-prime attributes. For example, in the relation Student (student ID, name, age, gender, class), where "student ID" is the primary key, the attributes "name", "age", "gender", and "class" are non-prime attributes.

## ER Diagram

When working on a project, it is essential to try drawing an ER diagram to clarify the database design. This is also a common topic interviewers might ask. An ER diagram, short for Entity Relationship Diagram, provides a method to represent entity types, attributes, and relationships. An ER diagram consists of the following three elements:

Entity: Typically a real-world business object, though logical objects are also permissible. For example, in a campus management system, entities would include students, teachers, courses, classes, etc. In an ER diagram, entities are represented by rectangular boxes.
Attribute: Attributes are properties owned by an entity, used to describe the elements that make up the entity. In product design, this could be understood as fields. In an ER diagram, attributes are represented by ovals.
Relationship: This represents the relationships between entities. In ER diagrams, relationships are depicted using diamonds. These relationships not only have business associations but can also be quantitatively described in terms of the number of entities involved. For example, the relationship where a class has multiple students is represented as a kind of entity relationship. The diagram below is an ER diagram of students selecting courses, where each student can choose several courses, and each course can also be selected by several students, indicating a many-to-many (M:N) relationship. Other types of entity relationships include one-to-one (1:1) and one-to-many (1:N).

![学生与课程之间联系的E-R图](./photo_1.png)

In database design, ensuring that data tables conform to certain normal forms (Normalization) is crucial to avoid data redundancy and maintain data integrity. 

- **1NF (First Normal Form)**: The First Normal Form (1NF) requires that each field in a table must contain only atomic (indivisible) values, meaning no field can contain multiple values or repeated combinations. This ensures that each field maintains consistency and independence. First Normal Form is a basic requirement for relational databases to ensure the consistency of each field.

- **2NF (Second Normal Form)**: 2NF, based on 1NF, eliminates partial functional dependencies of non-prime attributes on the key. As shown in the diagram below, it demonstrates the transition from the First Normal Form to the Second Normal Form. The Second Normal Form adds a column based on the First Normal Form, which is called the primary key, and all non-prime attributes depend on this primary key.
  
    | StudentID | StudentName | Course |
    |-----------|-------------|--------|
    | 1         | Xiao Ming   | Math   |
    | 1         | Xiao Ming   | Physics|
    | 2         | Xiao Hong   | Chemistry|
    | 3         | Xiao Gang   | Biology|
    | 3         | Xiao Gang   | Chemistry|
    | 3         | Xiao Gang   | Physics|
    (Conforming to 1NF)

    | StudentID | StudentName | Courses       |
    |-----------|-------------|---------------|
    | 1         | Xiao Ming   | Math, Physics |
    | 2         | Xiao Hong   | Chemistry     |
    | 3         | Xiao Gang   | Biology, Chemistry, Physics |
    (Not Conforming to 1NF)
    
    | StudentID | StudentName | Major       |
    |-----------|-------------|-------------|
    | 1         | Xiao Ming   | Computer Science |
    | 2         | Xiao Hong   | Chemistry   |

    | StudentID | Course   | Grade |
    |-----------|----------|-------|
    | 1         | Math     | 90    |
    | 1         | Physics  | 85    |
    | 2         | Chemistry| 88    |
    Conforming to 2NF

    | StudentID | StudentName | Course   | Grade | Major         |
    |-----------|-------------|----------|-------|---------------|
    | 1         | Xiao Ming   | Math     | 90    | Computer Science |
    | 1         | Xiao Ming   | Physics  | 85    | Computer Science |
    | 2         | Xiao Hong   | Chemistry| 88    | Chemistry       |
    Not Conforming to 2NF



- **3NF (Thrid Normal Form)**: 3NF builds on 2NF by eliminating transitive functional dependencies of non-prime attributes on the key. A database design that conforms to 3NF typically resolves issues of excessive data redundancy and anomalies related to insertion, modification, and deletion. For example, in the relation R(StudentID, Name, DepartmentName, DepartmentHead), where StudentID → DepartmentName and DepartmentName → DepartmentHead, there exists a transitive functional dependency of the non-prime attribute DepartmentHead on StudentID. Therefore, the design of this table does not meet the requirements of 3NF

### Important Concepts:
- Functional Dependency: If attribute set X uniquely determines attribute set Y, then Y is functionally dependent on X, and this relationship is denoted as X → Y.
- Partial Functional Dependency: If attribute set X → Y, and there exists a subset X0 of X, where X0 → Y and no proper subset of X0 can functionally determine Y, then Y has a partial functional dependency on X0. This type of dependency occurs commonly within composite keys where only part of the key determines another attribute.
- Full Functional Dependency: If every element of X is required to uniquely determine Y (i.e., no subset of X can determine Y), then Y has a full functional dependency on X. This type of dependency means Y is fully functionally dependent on X without involving any subsets of X.
- Transitive Functional Dependency: In a relation R(U), where X, Y, Z are subsets of U and X determines Y, Y determines Z, but not necessarily X determines Z, there exists a transitive functional dependency (XUY) → X. Transitive functional dependency involves a situation where one attribute indirectly determines another through a third attribute.

## Difference between Primary Key and Foreign Key
Primary Key (PK): The primary key is used to uniquely identify a tuple in a table, cannot be duplicated, and must not be null. A table can only have one primary key.

Foreign Key (FK): The foreign key is used to establish a relationship with another table. A foreign key is to link to the primary key of another table, can be duplicated, and can be null. A table can have multiple foreign keys.

## Difference between drop, delete,  truncate？

- `drop`: directly drop the whole table
- `truncate` : empty the data in the table, but not delete the table itself
- `delete` : delete is similar to `truncate`, but delete can be combined with `where` clause.

### Different Database Language Commands

TRUNCATE and DROP are DDL (Data Definition Language) statements. Their operations take effect immediately, are not placed into the rollback segment, and cannot be rolled back. These operations do not trigger triggers. In contrast, the DELETE statement is a DML (Data Manipulation Language) statement, which places the operation into the rollback segment and only takes effect after the transaction is committed.

Differences Between DML and DDL Statements:

DML refers to operations on the records in database tables. This mainly includes inserting, updating, deleting, and querying table records. It is the most frequently used operation by developers in daily work.

DDL refers to the language used to create, delete, and modify database objects. 

The main difference between DDL and DML is that DML only operates on the data within tables and does not involve modifying the table definitions, structures, or other objects. DDL statements are more commonly used by database administrators (DBAs) and are rarely used by regular developers.
Additionally, since SELECT does not alter the table, it is sometimes categorized separately as DQL (Data Query Language).

Execution Speed Differences
Generally speaking: DROP > TRUNCATE > DELETE.

The DELETE command generates database binlog logs during execution, which takes time. However, this also has the benefit of facilitating data rollback and recovery.
The TRUNCATE command does not generate database logs, making it faster than DELETE. Additionally, it resets the table's auto-increment value and restores indexes to their initial size.
The DROP command releases all space occupied by the table.
