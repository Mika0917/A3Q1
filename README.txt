Name: Mika Toktamyssov
Student id: 101229597

Link to youtube video: https://youtu.be/ML6CIbSRMAE

You will need:
Java JDK 11 or newer
PostgreSQL
PostgreSQL JDBC driver

Database setup:
Open pgAdmin.
Create a database named A3Q1 (right click on Databases -> create -> Database then Enter A3Q1 in the Database section.
Create a table called students.
Open a SQL Query tool and run:

CREATE TABLE students (
	student_id SERIAL PRIMARY KEY,
	first_name TEXT NOT NULL,
	last_name TEXT NOT NULL,
	email TEXT NOT NULL UNIQUE,
	enrollment_date DATE
);

INSERT INTO students (first_name, last_name, email, enrollment_date) VALUES
('John', 'Doe', 'john.doe@example.com', '2023-09-01'),
('Jane', 'Smith', 'jane.smith@example.com', '2023-09-01'),
('Jim', 'Beam', 'jim.beam@example.com', '2023-09-02');

Setting up JAVA CRUD code using JDBC:
Download PostgreSQL JDBC driver
Add the .jar file to you project as an external library (if you are using intelliJ press 'F4' go to the libraries section and click the '+' sign and select 'Java' and now navigate to where you downloaded the .jar file and select it and click 'ok'.

You can find the java code in the repository. Replace the username and password with your PostgreSQL username and password (the default username is postgres). Run the java program and it should work.
