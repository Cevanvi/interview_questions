# Data Vault

Data Vault is a system of business intelligence that includes modeling, methodology, and system architecture. It's built to be flexible, scalable, consistent, and adaptable to the needs of the business. The Data Vault consists of three types of tables:

1. Hubs: These are the unique list of business keys with low frequency of change.

2. Satellites: These store all the descriptive information (attributes) about the business keys.

3. Links: These represent the associations or transactions between business keys (Hubs).

Here's a high-level example of how these components might be implemented:

Let's say we have a business that sells products. 

- A Hub table might be a list of all products, each with a unique product ID.
```sql
CREATE TABLE Hub_Product (
    Product_ID INT PRIMARY KEY,
    Load_Date DATE,
    Record_Source VARCHAR(255)
);
 ```
| Product_ID | Load_Date  | Record_Source |
|------------|------------|---------------|
| 1          | 2022-01-01 | 'External'    |
| 2          | 2022-01-02 | 'Internal'    |


- A Satellite table might list descriptive information about each product, like name, category, price etc.
```sql
CREATE TABLE Satellite_Product (
    Product_ID INT,
    Name VARCHAR(255),
    Category VARCHAR(255),
    Price DECIMAL(10,2),
    Load_Date DATE,
    End_Date DATE,
    Record_Source VARCHAR(255),
    PRIMARY KEY (Product_ID, Load_Date),
    FOREIGN KEY (Product_ID) REFERENCES Hub_Product(Product_ID)
);
 ```
| Product_ID | Name             | Category | Price | Load_Date  | End_Date   | Record_Source |
|------------|------------------|----------|-------|------------|------------|---------------|
| 1          | 'Apple'          | 'Fruit'  | 1.00  | 2022-01-01 | NULL       | 'External'    |
| 2          | 'Banana'         | 'Fruit'  | 0.50  | 2022-01-02 | NULL       | 'Internal'    |

- A Link table might show transactions, linking the product to a customer who purchased it.
```sql
CREATE TABLE Link_Sale (
    Sale_ID INT PRIMARY KEY,
    Product_ID INT,
    Customer_ID INT,
    Load_Date DATE,
    Record_Source VARCHAR(255),
    FOREIGN KEY (Product_ID) REFERENCES Hub_Product(Product_ID),
    -- Assuming there is a Hub_Customer table
    FOREIGN KEY (Customer_ID) REFERENCES Hub_Customer(Customer_ID)
);
```
| Sale_ID | Product_ID | Customer_ID | Load_Date  | Record_Source |
|---------|------------|-------------|------------|---------------|
| 1       | 1          | 100         | 2022-01-03 | 'POS'         |
| 2       | 2          | 101         | 2022-01-04 | 'POS'         |

In this example, the Hub_Product table holds unique product IDs. 
The Satellite_Product table holds additional information about each product. 
The Link_Sale table links products to customers, representing a sale.

Let's consider some queries:

1. List all products sold:

```sql
SELECT HP.Product_ID, SP.Name
FROM Hub_Product HP
JOIN Link_Sale LS ON HP.Product_ID = LS.Product_ID
JOIN Satellite_Product SP ON HP.Product_ID = SP.Product_ID AND LS.Load_Date = SP.Load_Date;
```

2. Find all customers who bought apples:

```sql
SELECT LS.Customer_ID
FROM Link_Sale LS
JOIN Satellite_Product SP ON LS.Product_ID = SP.Product_ID AND LS.Load_Date = SP.Load_Date
WHERE SP.Name = 'Apple';
```

### Cons

1. **Scalability**: 

Data Vault is designed to be highly scalable, so it can handle small to very large amounts of data efficiently.

2. **Flexibility**: 

Data Vault architecture allows for easy adaptation to changes in business requirements, making it future-proof.

3. **Auditability and Compliance**: 

Data Vault retains all historical data and can track changes over time, which is beneficial for audit trails and compliance with regulations.

4. **Parallel Loading**: 

Data Vault allows for parallel loading of data into Hubs, Links, and Satellites, improving performance.

### Pros

1. **Complexity**: 

Data Vault modeling is more complex compared to traditional Star or Snowflake schemas. 
It requires a deeper understanding to design, implement and maintain. 
This complexity can lead to longer development times and require more skilled (and potentially more expensive) resources.

2. **Performance**:

Because of its complexity and the number of joins required to answer a query, Data Vault can suffer from performance issues, particularly when dealing with large datasets. 
To mitigate this, Data Marts or other reporting structures are often built on top of the Data Vault.

3. **Documentation** and Tooling: 

There are fewer resources, tools, and established standards for implementing and working with Data Vault models compared to more traditional approaches.

4. **Less Intuitive for Business Users**: 

The structure of a Data Vault model can be less intuitive for business users and analysts who are used to dealing with Star and Snowflake schemas.
This means additional training or resources may be required to help these users understand and interact with the system.

5. **Data Redundancy**: 

Data Vault can lead to data redundancy due to its design of retaining all the historical data. 
This might result in higher storage costs.


# Data Vault 2.0
Data Vault 2.0 is an enhancement of the original Data Vault 1.0 methodology. 
It introduces new concepts and offers a more comprehensive approach to building an enterprise data warehouse.
The aim of Data Vault 2.0 is to overcome some limitations of Data Vault 1.0 and adapt to new developments in the data landscape, such as big data, NoSQL databases, and real-time data processing.

Here are some of the main enhancements in Data Vault 2.0:

1. **Inclusion of NoSQL and unstructured data**:

Data Vault 2.0 incorporates techniques to handle NoSQL and unstructured data, which is important in the era of big data.

2. **Real-time data loading**: 

Data Vault 2.0 introduces concepts for real-time, near-real-time, and streaming data loads.

3. **New Data Vault modeling constructs**:

Data Vault 2.0 introduces new types of entities, like the Point-in-Time (PIT) and Bridge tables, to improve performance and querying.

4. **Hash keys**: 

Data Vault 2.0 recommends the use of hash keys for the business keys in Hub tables and the relationships in Link tables to improve join performance.

5. **Enforcement of standards**:

Data Vault 2.0 places a heavy emphasis on standards, documentation, and metadata to ensure consistency and quality.

Let's look at an example of a Data Vault 2.0 architecture:

Suppose we have a business that sells products to customers. Here's how you might model this in Data Vault 2.0:

**Hub Tables**

- Hub_Customer: Stores unique customers.
- Hub_Product: Stores unique products.
```sql
CREATE TABLE Hub_Customer (
    Customer_HashKey CHAR(64) PRIMARY KEY,
    Customer_ID INT,
    Load_Date DATE,
    Record_Source VARCHAR(255)
);
CREATE TABLE Hub_Product (
    Product_HashKey CHAR(64) PRIMARY KEY,
    Product_ID INT,
    Load_Date DATE,
    Record_Source VARCHAR(255)
);
INSERT INTO Hub_Customer (Customer_HashKey, Customer_ID, Load_Date, Record_Source)
VALUES ('hash1', 1, '2022-01-01', 'Source1'),
       ('hash2', 2, '2022-01-02', 'Source2');

INSERT INTO Hub_Product (Product_HashKey, Product_ID, Load_Date, Record_Source)
VALUES ('hash3', 1, '2022-01-03', 'Source1'),
       ('hash4', 2, '2022-01-04', 'Source2');
```
**Satellite Tables**

- Satellite_Customer: Stores descriptive attributes about customers.
- Satellite_Product: Stores descriptive attributes about products.

```sql
CREATE TABLE Satellite_Customer (
    Customer_HashKey CHAR(64),
    Load_Date DATE,
    End_Date DATE,
    Name VARCHAR(255),
    Email VARCHAR(255),
    Record_Source VARCHAR(255),
    PRIMARY KEY (Customer_HashKey, Load_Date),
    FOREIGN KEY (Customer_HashKey) REFERENCES Hub_Customer(Customer_HashKey)
);
CREATE TABLE Satellite_Product (
    Product_HashKey CHAR(64),
    Load_Date DATE,
    End_Date DATE,
    Name VARCHAR(255),
    Category VARCHAR(255),
    Price DECIMAL(10,2),
    Record_Source VARCHAR(255),
    PRIMARY KEY (Product_HashKey, Load_Date),
    FOREIGN KEY (Product_HashKey) REFERENCES Hub_Product(Product_HashKey)
);
INSERT INTO Satellite_Customer (Customer_HashKey, Load_Date, End_Date, Name, Email, Record_Source)
VALUES ('hash1', '2022-01-01', NULL, 'John Doe', 'john.doe@example.com', 'Source1'),
       ('hash2', '2022-01-02', NULL, 'Jane Doe', 'jane.doe@example.com', 'Source2');

INSERT INTO Satellite_Product (Product_HashKey, Load_Date, End_Date, Name, Category, Price, Record_Source)
VALUES ('hash3', '2022-01-03', NULL, 'Product1', 'Category1', 10.0, 'Source1'),
       ('hash4', '2022-01-04', NULL, 'Product2', 'Category2', 20.0, 'Source2');
```
**Link Table**

- Link_Sale: Represents a sale, linking a product to a customer.
```sql
CREATE TABLE Link_Sale (
    Sale_HashKey CHAR(64) PRIMARY KEY,
    Customer_HashKey CHAR(64),
    Product_HashKey CHAR(64),
    Sale_ID INT,
    Sale_Date DATE,
    Record_Source VARCHAR(255),
    FOREIGN KEY (Customer_HashKey) REFERENCES Hub_Customer(Customer_HashKey),
    FOREIGN KEY (Product_HashKey) REFERENCES Hub_Product(Product_HashKey)
);
INSERT INTO Link_Sale (Sale_HashKey, Customer_HashKey, Product_HashKey, Sale_ID, Sale_Date, Record_Source)
VALUES ('hash5', 'hash1', 'hash3', 1, '2022-01-05', 'Source1'),
       ('hash6', 'hash2', 'hash4', 2, '2022-01-06', 'Source2');
```
**Point-in-Time (PIT) Table**

- PIT_Sale: Provides a snapshot of a Link and its associated Satellites at a given point in time.
```sql
CREATE TABLE PIT_Sale (
    Sale_HashKey CHAR(64),
    As_Of_Date DATE,
    Customer_HashKey CHAR(64),
    Product_HashKey CHAR(64),
    PRIMARY KEY (Sale_HashKey, As_Of_Date),
    FOREIGN KEY (Sale_HashKey) REFERENCES Link_Sale(Sale_HashKey)
);
INSERT INTO PIT_Sale (Sale_HashKey, As_Of_Date, Customer_HashKey, Product_HashKey)
VALUES ('hash5', '2022-01-05', 'hash1', 'hash3'),
       ('hash6', '2022-01-06', 'hash2', 'hash4');
```
**Bridge Table**

- Bridge_Customer_Product: Provides a direct relationship between a customer and a product for a specific business question.
```sql
CREATE TABLE Bridge_Customer_Product (
    Customer_HashKey CHAR(64),
    Product_HashKey CHAR(64),
    FOREIGN KEY (Customer_HashKey) REFERENCES Hub_Customer(Customer_HashKey),
    FOREIGN KEY (Product_HashKey) REFERENCES Hub_Product(Product_HashKey)
);
INSERT INTO Bridge_Customer_Product (Customer_HashKey, Product_HashKey)
VALUES ('hash1', 'hash3'), ('hash2', 'hash4');
```
Here are some example queries:

1. To list all products sold:
```sql
SELECT HP.Product_ID, SP.Name
FROM Hub_Product HP
JOIN Link_Sale LS ON HP.Product_HashKey = LS.Product_HashKey
JOIN Satellite_Product SP ON HP.Product_HashKey = SP.Product_HashKey AND LS.Sale_Date = SP.Load_Date;
```

2. To find all customers who bought a specific product:
```sql
SELECT HC.Customer_ID, SC.Name
FROM Hub_Customer HC
JOIN Link_Sale LS ON HC.Customer_HashKey = LS.Customer_HashKey
JOIN Satellite_Customer SC ON HC.Customer_HashKey = SC.Customer_HashKey AND LS.Sale_Date = SC.Load_Date
WHERE LS.Product_HashKey = (SELECT Product_HashKey FROM Hub_Product WHERE Product_ID = ?);
```

3. To get a snapshot of all sales at a specific point in time:
```sql
SELECT LS.Sale_ID, HC.Customer_ID, HP.Product_ID
FROM PIT_Sale PIT
JOIN Link_Sale LS ON PIT.Sale_HashKey = LS.Sale_HashKey
JOIN Hub_Customer HC ON PIT.Customer_HashKey = HC.Customer_HashKey
JOIN Hub_Product HP ON PIT.Product_HashKey = HP.Product_HashKey
WHERE PIT.As_Of_Date = '2022-01-05';
```