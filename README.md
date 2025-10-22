# Hotel Management System

This project is a simple command-line Hotel Management System built using Java and MySQL. It allows users to manage hotel reservations by interacting with a MySQL database through JDBC (Java Database Connectivity).

## ✨ Features

- **Reserve a room:** Allows users to book a room by providing their name, desired room number, and contact number.
- **View Reservations:** Displays a list of all current reservations, including reservation ID, guest name, room number, contact number, and reservation date.
- **Get Room Number:** Retrieves the room number for a specific reservation based on the reservation ID and guest name.
- **Update Reservations:** Enables modification of existing reservation details (guest name, room number, contact number) using the reservation ID.
- **Delete Reservations:** Allows removal of reservations from the system using the reservation ID.
- **Exit:** Closes the application with a thank you message.

## 🛠️ Technologies Used

- **Java:** The primary programming language used for the application logic.
- **MySQL:** The relational database used to store hotel and reservation data.
- **JDBC:** Java Database Connectivity API used to connect and interact with the MySQL database.

## ⚙️ Prerequisites

Before running the application, ensure you have the following installed:

- **Java Development Kit (JDK):** Make sure you have a compatible JDK installed on your system.
- **MySQL Server:** You need a running instance of MySQL Server.
- **MySQL JDBC Driver:** The MySQL Connector/J JDBC driver is required for Java to communicate with the MySQL database. Ensure this driver is included in your project's classpath.

## 🗄️ Database Setup

You will need to create a database and a table named `reservations` in your MySQL server. The provided code connects to a database named `hotel_db` with the username `root` and password `knox`. You might need to adjust these credentials based on your MySQL setup.

Here's the SQL to create the `reservations` table:

```sql
CREATE TABLE reservations (
    reservation_id INT AUTO_INCREMENT PRIMARY KEY,
    guest_name VARCHAR(255) NOT NULL,
    room_num INT NOT NULL,
    cont_num VARCHAR(20) NOT NULL,
    reservation_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

**Important:**
- Ensure that the database named `hotel_db` exists in your MySQL server.
- Verify that the username (`root`) and password (`knox`) in the Java code match your MySQL server configuration. Update them if necessary.
- The `reservations` table should be created with the specified columns.

## 🚀 Running the Application

To run the application:

1. **Compile the Java code:** Navigate to the directory containing the `HRS_JDBC.java` file in your terminal or command prompt and compile it using the Java compiler:
   ```bash
   javac Projects_java/Hotel_Reservation_System/HRS_JDBC.java
   ```

2. **Run the application:** Execute the compiled class file using the Java Virtual Machine. Make sure the MySQL JDBC driver JAR file is in your classpath. You might need to use the `-cp` or `--classpath` option followed by the path to the driver JAR file (e.g., `mysql-connector-j-8.x.x.jar`). Assuming the compiled class is in `Projects_java/Hotel_Reservation_System/`:
   ```bash
   java -cp .:path/to/mysql-connector-j-8.x.x.jar Projects_java.Hotel_Reservation_System.HRS_JDBC
   ```
   (Replace `path/to/mysql-connector-j-8.x.x.jar` with the actual path to your JDBC driver JAR file. The `.` in `-cp` includes the current directory in the classpath.)

3. **Follow the on-screen menu:** The application will present a menu with options to perform different hotel management tasks. Enter the corresponding number to choose an action and follow the prompts.

## 🔧 Code Explanation

The application provides a comprehensive hotel reservation system with the following functionalities:

- Reservation creation with guest details
- Reservation viewing in a tabular format
- Room retrieval by reservation ID and guest name
- Reservation updates and deletions
- Input validation and error handling

## 🚀 Future Development

This is a functional command-line Hotel Management System. Potential areas for further development include:

- **Input Validation:** Implement more robust input validation to prevent errors due to incorrect user input (e.g., non-numeric room numbers, invalid contact formats).
- **Error Handling:** Improve error handling for database operations to provide more informative messages to the user.
- **User Interface:** Develop a graphical user interface (GUI) using libraries like Swing or JavaFX for a more user-friendly experience.
- **Room Management:** Add functionality to manage rooms (e.g., add new rooms, update room details, track room availability).
- **Date/Time Handling:** Implement more sophisticated handling of reservation dates and times.
- **Searching and Filtering:** Allow users to search and filter reservations based on various criteria (e.g., guest name, room number, reservation date).
- **Reporting:** Generate reports on reservations, occupancy rates, etc.
- **Security:** Implement security measures, especially if the application were to handle more sensitive data.