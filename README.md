# A3Q1
Assignment 3 Question 1 Submission for 3005

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

```
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
```

Setting up JAVA CRUD code using JDBC:
Download PostgreSQL JDBC driver
You will want to create a new project and create a file in that project in src called Q1.
Add the .jar file to you project as an external library (if you are using intelliJ press 'F4' go to the libraries section and click the '+' sign and select 'Java' and now navigate to where you downloaded the .jar file and select it and click 'ok'.

You can find the java code in the repository and you want to paste that into your file. Replace the username and password with your PostgreSQL username and password (the default username is postgres). Run the java program and it should work.

```
import java.sql.*;
import java.util.Scanner;

public class Q1 {

    private final String url = "jdbc:postgresql://localhost:5432/A3Q1";
    private final String user = "postgres";
    private final String password = "mikatoktamyssov";

    // get all students
    public void getAllStudents() {
        String SQL = "SELECT * FROM students";

        try (Connection conn = DriverManager.getConnection(url, user, password);
             PreparedStatement pstmt = conn.prepareStatement(SQL)) {

            ResultSet result = pstmt.executeQuery();
            while (result.next()) {
                System.out.println("Student ID: " + result.getInt("student_id"));
                System.out.println("First Name: " + result.getString("first_name"));
                System.out.println("Last Name: " + result.getString("last_name"));
                System.out.println("Email: " + result.getString("email"));
                System.out.println("Enrollment Date: " + result.getDate("enrollment_date"));
            }

        } catch (SQLException ex) {
            System.out.println(ex.getMessage());
        }
    }
    // Add a student
    public void addStudent(String first_name, String last_name, String email, Date enrollment_date) {
        // check to see if email already exists
        if (emailExists(email)) {
            System.out.println("A student with this email already exists.");
            return;
        }

        String SQL = "INSERT INTO students(first_name,last_name,email,enrollment_date) VALUES(?,?,?,?)";

        try (Connection conn = DriverManager.getConnection(url, user, password);
             PreparedStatement pstmt = conn.prepareStatement(SQL)) {

            pstmt.setString(1, first_name);
            pstmt.setString(2, last_name);
            pstmt.setString(3, email);
            pstmt.setDate(4, enrollment_date);
            pstmt.executeUpdate();
            System.out.println("Student added successfully");

        } catch (SQLException ex) {
            System.out.println(ex.getMessage());
        }
    }

    // Update student's email based on student_id
    public void updateStudentEmail(int student_id, String newEmail) {
        // check to see if email already exists
        if (emailExists(newEmail)) {
            System.out.println("A student with this email already exists.");
            return;
        }

        String SQL = "UPDATE students SET email=? WHERE student_id=?";

        try (Connection conn = DriverManager.getConnection(url, user, password);
             PreparedStatement pstmt = conn.prepareStatement(SQL)) {

            pstmt.setString(1, newEmail);
            pstmt.setInt(2, student_id);
            pstmt.executeUpdate();
            System.out.println("Student email updated");

        } catch (SQLException ex) {
            System.out.println(ex.getMessage());
        }
    }

    // Delete student based on student_id
    public void deleteStudent(int student_id) {
        String SQL = "DELETE FROM students WHERE student_id=?";

        try (Connection conn = DriverManager.getConnection(url, user, password);
             PreparedStatement pstmt = conn.prepareStatement(SQL)) {

            pstmt.setInt(1, student_id);
            int affectedRows = pstmt.executeUpdate(); // This returns the number of affected rows, so we know if a student with that id was found

            if (affectedRows > 0) {
                System.out.println("Student deleted");
            } else {
                System.out.println("No student found with ID: " + student_id);
            }

        } catch (SQLException ex) {
            System.out.println(ex.getMessage());
        }
    }

    // Helper method to check if email already exists in the database since email is unique
    private boolean emailExists(String email) {
        String SQL = "SELECT count(*) FROM students WHERE email = ?";

        try (Connection conn = DriverManager.getConnection(url, user, password);
             PreparedStatement pstmt = conn.prepareStatement(SQL)) {

            pstmt.setString(1, email);
            try (ResultSet result = pstmt.executeQuery()) {
                if (result.next()) {
                    int count = result.getInt(1);
                    return count > 0;
                }
            }

        } catch (SQLException ex) {
            System.out.println(ex.getMessage());
        }
        return false;
    }


    public static void main(String[] args) {
        Q1 dbOps = new Q1();
        Scanner scanner = new Scanner(System.in);

        // Display all students
        System.out.println("Do you want to display all students? (yes/no)");
        if (scanner.nextLine().equalsIgnoreCase("yes")) {
            dbOps.getAllStudents();
        }

        // Add a student
        System.out.println("Do you want to add a student? (yes/no)");
        if (scanner.nextLine().equalsIgnoreCase("yes")) {
            dbOps.addStudent("Mika", "Toktamyssov", "mika.toktamyssov@example.com", Date.valueOf("2023-09-01"));
        }

        // Modify student email
        System.out.println("Do you want to modify a student's email? (yes/no)");
        if (scanner.nextLine().equalsIgnoreCase("yes")) {
            dbOps.updateStudentEmail(11, "Updated2mika.toktamyssov@example.com");
        }

        // Delete student
        System.out.println("Do you want to delete a student? (yes/no)");
        if (scanner.nextLine().equalsIgnoreCase("yes")) {
            dbOps.deleteStudent(11);
        }
        scanner.close();
    }
}
```
