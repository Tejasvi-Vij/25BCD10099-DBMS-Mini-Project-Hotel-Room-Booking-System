# Hotel Room Booking System

**DBMS Mini Project | Oracle APEX 26.2 | Oracle SQL**

## 5. SQL Code Reference

The following SQL illustrates the database structure and queries used by the application. The DDL is a reference version and should be checked against the existing database before execution.

### A. Create the ROOM_TYPES table

```sql
CREATE TABLE ROOM_TYPES (
    TYPE_ID NUMBER PRIMARY KEY,
    TYPE_NAME VARCHAR2(50) NOT NULL,
    CAPACITY NUMBER,
    DESCRIPTION VARCHAR2(200)
);
```

### B. Create the ROOMS table

```sql
CREATE TABLE ROOMS (
    ROOM_ID NUMBER PRIMARY KEY,
    ROOM_NUMBER NUMBER UNIQUE NOT NULL,
    TYPE_ID NUMBER,
    PRICE_PER_NIGHT NUMBER(10,2) NOT NULL,
    ROOM_STATUS VARCHAR2(20) DEFAULT 'Available',
    CONSTRAINT FK_ROOMS_TYPE
        FOREIGN KEY (TYPE_ID)
        REFERENCES ROOM_TYPES(TYPE_ID)
);
```

### C. Create the CUSTOMERS table

```sql
CREATE TABLE CUSTOMERS (
    CUSTOMER_ID NUMBER PRIMARY KEY,
    CUSTOMER_NAME VARCHAR2(100) NOT NULL,
    PHONE VARCHAR2(15),
    EMAIL VARCHAR2(100)
);
```

### D. Create the BOOKINGS table

```sql
CREATE TABLE BOOKINGS (
    BOOKING_ID NUMBER PRIMARY KEY,
    CUSTOMER_ID NUMBER NOT NULL,
    ROOM_ID NUMBER NOT NULL,
    CHECK_IN DATE NOT NULL,
    CHECK_OUT DATE NOT NULL,
    BOOKING_STATUS VARCHAR2(20),
    CONSTRAINT FK_BOOKING_CUSTOMER
        FOREIGN KEY (CUSTOMER_ID)
        REFERENCES CUSTOMERS(CUSTOMER_ID),
    CONSTRAINT FK_BOOKING_ROOM
        FOREIGN KEY (ROOM_ID)
        REFERENCES ROOMS(ROOM_ID),
    CONSTRAINT CHK_BOOKING_DATES
        CHECK (CHECK_OUT > CHECK_IN)
);
```

### E. Create the PAYMENTS table

```sql
CREATE TABLE PAYMENTS (
    PAYMENT_ID NUMBER PRIMARY KEY,
    BOOKING_ID NUMBER NOT NULL,
    AMOUNT NUMBER(10,2) NOT NULL,
    PAYMENT_DATE DATE DEFAULT SYSDATE,
    PAYMENT_MODE VARCHAR2(20),
    PAYMENT_STATUS VARCHAR2(20),
    CONSTRAINT FK_PAYMENT_BOOKING
        FOREIGN KEY (BOOKING_ID)
        REFERENCES BOOKINGS(BOOKING_ID)
);
```

## 6. Sample Data

The examples below illustrate the records displayed in the application screenshots. They assume the corresponding IDs and foreign-key relationships exist.

### A. Insert room types

```sql
INSERT INTO ROOM_TYPES
(TYPE_ID, TYPE_NAME, CAPACITY, DESCRIPTION)
VALUES (1, 'Standard', 2, 'Standard hotel room');

INSERT INTO ROOM_TYPES
(TYPE_ID, TYPE_NAME, CAPACITY, DESCRIPTION)
VALUES (2, 'Deluxe', 3, 'Deluxe room with extra space');

INSERT INTO ROOM_TYPES
(TYPE_ID, TYPE_NAME, CAPACITY, DESCRIPTION)
VALUES (3, 'Suite', 4, 'Premium suite');
```

### B. Insert rooms

```sql
INSERT INTO ROOMS
(ROOM_ID, ROOM_NUMBER, TYPE_ID, PRICE_PER_NIGHT, ROOM_STATUS)
VALUES (1, 101, 1, 1500, 'Available');

INSERT INTO ROOMS
(ROOM_ID, ROOM_NUMBER, TYPE_ID, PRICE_PER_NIGHT, ROOM_STATUS)
VALUES (2, 102, 1, 1500, 'Available');

INSERT INTO ROOMS
(ROOM_ID, ROOM_NUMBER, TYPE_ID, PRICE_PER_NIGHT, ROOM_STATUS)
VALUES (3, 201, 2, 2500, 'Available');

INSERT INTO ROOMS
(ROOM_ID, ROOM_NUMBER, TYPE_ID, PRICE_PER_NIGHT, ROOM_STATUS)
VALUES (4, 301, 3, 4500, 'Available');
```

### C. Insert customers

```sql
INSERT INTO CUSTOMERS
(CUSTOMER_ID, CUSTOMER_NAME, PHONE, EMAIL)
VALUES (1, 'Aarav Sharma', '9876500101', 'aarav@example.com');

INSERT INTO CUSTOMERS
(CUSTOMER_ID, CUSTOMER_NAME, PHONE, EMAIL)
VALUES (2, 'Priya Verma', '9876500102', 'priya@example.com');

INSERT INTO CUSTOMERS
(CUSTOMER_ID, CUSTOMER_NAME, PHONE, EMAIL)
VALUES (3, 'Rahul Singh', '9876500103', 'rahul@example.com');

INSERT INTO CUSTOMERS
(CUSTOMER_ID, CUSTOMER_NAME, PHONE, EMAIL)
VALUES (4, 'Simran Kaur', '9876500104', 'simran@example.com');
```

### D. Insert bookings

```sql
INSERT INTO BOOKINGS
(BOOKING_ID, CUSTOMER_ID, ROOM_ID,
 CHECK_IN, CHECK_OUT, BOOKING_STATUS)
VALUES
(1001, 1, 1, DATE '2026-10-22',
 DATE '2026-10-24', 'Confirmed');

INSERT INTO BOOKINGS
(BOOKING_ID, CUSTOMER_ID, ROOM_ID,
 CHECK_IN, CHECK_OUT, BOOKING_STATUS)
VALUES
(1002, 2, 3, DATE '2026-10-23',
 DATE '2026-10-26', 'Confirmed');

INSERT INTO BOOKINGS
(BOOKING_ID, CUSTOMER_ID, ROOM_ID,
 CHECK_IN, CHECK_OUT, BOOKING_STATUS)
VALUES
(1003, 3, 4, DATE '2026-10-24',
 DATE '2026-10-27', 'Confirmed');

INSERT INTO BOOKINGS
(BOOKING_ID, CUSTOMER_ID, ROOM_ID,
 CHECK_IN, CHECK_OUT, BOOKING_STATUS)
VALUES
(1004, 4, 2, DATE '2026-10-25',
 DATE '2026-10-26', 'Confirmed');
```

### E. Insert payments

```sql
INSERT INTO PAYMENTS
(PAYMENT_ID, BOOKING_ID, AMOUNT,
 PAYMENT_MODE, PAYMENT_STATUS)
VALUES (5001, 1001, 3000, 'UPI', 'Paid');

INSERT INTO PAYMENTS
(PAYMENT_ID, BOOKING_ID, AMOUNT,
 PAYMENT_MODE, PAYMENT_STATUS)
VALUES (5002, 1002, 7500, 'Card', 'Paid');

INSERT INTO PAYMENTS
(PAYMENT_ID, BOOKING_ID, AMOUNT,
 PAYMENT_MODE, PAYMENT_STATUS)
VALUES (5003, 1003, 13500, 'Cash', 'Paid');

INSERT INTO PAYMENTS
(PAYMENT_ID, BOOKING_ID, AMOUNT,
 PAYMENT_MODE, PAYMENT_STATUS)
VALUES (5004, 1004, 1500, 'UPI', 'Pending');

COMMIT;
```

## 7. SQL Queries

### A. Display all rooms

```sql
SELECT *
FROM ROOMS;
```

### B. Display all room types

```sql
SELECT *
FROM ROOM_TYPES;
```

### C. Display all customers

```sql
SELECT *
FROM CUSTOMERS;
```

### D. Display all bookings

```sql
SELECT *
FROM BOOKINGS;
```

### E. Display all payments

```sql
SELECT *
FROM PAYMENTS;
```

### F. Display rooms with their room types

```sql
SELECT R.ROOM_NUMBER,
       RT.TYPE_NAME,
       R.PRICE_PER_NIGHT,
       R.ROOM_STATUS
FROM ROOMS R
JOIN ROOM_TYPES RT
    ON R.TYPE_ID = RT.TYPE_ID;
```

### G. Display bookings with customer names

```sql
SELECT B.BOOKING_ID,
       C.CUSTOMER_NAME,
       B.ROOM_ID,
       B.CHECK_IN,
       B.CHECK_OUT,
       B.BOOKING_STATUS
FROM BOOKINGS B
JOIN CUSTOMERS C
    ON B.CUSTOMER_ID = C.CUSTOMER_ID;
```

### H. Calculate total paid revenue

```sql
SELECT SUM(AMOUNT) AS TOTAL_PAID_REVENUE
FROM PAYMENTS
WHERE PAYMENT_STATUS = 'Paid';
```

### I. Count bookings by status

```sql
SELECT BOOKING_STATUS,
       COUNT(*) AS TOTAL_BOOKINGS
FROM BOOKINGS
GROUP BY BOOKING_STATUS;
```

### J. Display payment details with customer names

```sql
SELECT P.PAYMENT_ID,
       C.CUSTOMER_NAME,
       P.AMOUNT,
       P.PAYMENT_MODE,
       P.PAYMENT_STATUS
FROM PAYMENTS P
JOIN BOOKINGS B
    ON P.BOOKING_ID = B.BOOKING_ID
JOIN CUSTOMERS C
    ON B.CUSTOMER_ID = C.CUSTOMER_ID;
```

### K. Display available rooms ordered by price

```sql
SELECT ROOM_NUMBER,
       PRICE_PER_NIGHT
FROM ROOMS
WHERE ROOM_STATUS = 'Available'
ORDER BY PRICE_PER_NIGHT ASC;
```

### L. Calculate average payment

```sql
SELECT AVG(AMOUNT) AS AVERAGE_PAYMENT
FROM PAYMENTS;
```

### M. Count all bookings

```sql
SELECT COUNT(*) AS TOTAL_BOOKINGS
FROM BOOKINGS;
```

### N. Display bookings with room numbers

```sql
SELECT B.BOOKING_ID,
       C.CUSTOMER_NAME,
       R.ROOM_NUMBER,
       B.CHECK_IN,
       B.CHECK_OUT,
       B.BOOKING_STATUS
FROM BOOKINGS B
JOIN CUSTOMERS C
    ON B.CUSTOMER_ID = C.CUSTOMER_ID
JOIN ROOMS R
    ON B.ROOM_ID = R.ROOM_ID;
```

## 8. SQL Concepts Demonstrated

- DDL: `CREATE TABLE`
- DML: `INSERT`, `UPDATE`, `DELETE`
- DQL: `SELECT`
- Constraints: primary key, foreign key, unique, not null, and check
- Joins: `INNER JOIN`
- Aggregate functions: `SUM()`, `AVG()`, `COUNT()`
- Filtering: `WHERE`
- Sorting: `ORDER BY`
- Grouping: `GROUP BY`
- Transaction control: `COMMIT`

## 9. Oracle APEX Application Export

The repository contains the original exported application file:

`25BCD10099_DBMS_Mini_Project.sql`

This file contains the Oracle APEX application definition, including its pages, forms, reports, processes, and shared components. It is separate from the illustrative schema and query examples in this README.

## 10. Project Objective

The objective is to demonstrate the practical application of DBMS concepts by building a hotel management system that organizes room information, customer records, reservations, and payments.

## 11. Author

**Tejasvi Vij**

DBMS Mini Project
