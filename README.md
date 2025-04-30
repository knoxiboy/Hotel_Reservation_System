<h1>Hotel Management System</h1>

<p>This project is a simple command-line Hotel Management System built using Java and MySQL. It allows users to manage hotel reservations by interacting with a MySQL database through JDBC (Java Database Connectivity).</p>

<h2>Features</h2>

<ul>
  <li><strong>Reserve a room:</strong> Allows users to book a room by providing their name, desired room number, and contact number.</li>
  <li><strong>View Reservations:</strong> Displays a list of all current reservations, including reservation ID, guest name, room number, contact number, and reservation date.</li>
  <li><strong>Get Room Number:</strong> Retrieves the room number for a specific reservation based on the reservation ID and guest name.</li>
  <li><strong>Update Reservations:</strong> Enables modification of existing reservation details (guest name, room number, contact number) using the reservation ID.</li>
  <li><strong>Delete Reservations:</strong> Allows removal of reservations from the system using the reservation ID.</li>
  <li><strong>Exit:</strong> Closes the application with a thank you message.</li>
</ul>

<h2>Technologies Used</h2>

<ul>
  <li><strong>Java:</strong> The primary programming language used for the application logic.</li>
  <li><strong>MySQL:</strong> The relational database used to store hotel and reservation data.</li>
  <li><strong>JDBC:</strong> Java Database Connectivity API used to connect and interact with the MySQL database.</li>
</ul>

<h2>Prerequisites</h2>

<p>Before running the application, ensure you have the following installed:</p>

<ul>
  <li><strong>Java Development Kit (JDK):</strong> Make sure you have a compatible JDK installed on your system.</li>
  <li><strong>MySQL Server:</strong> You need a running instance of MySQL Server.</li>
  <li><strong>MySQL JDBC Driver:</strong> The MySQL Connector/J JDBC driver is required for Java to communicate with the MySQL database. Ensure this driver is included in your project's classpath.</li>
</ul>

<h2>Database Setup</h2>

<p>You will need to create a database and a table named `reservations` in your MySQL server. The provided code connects to a database named `hotel_db` with the username `root` and password `knox`. You might need to adjust these credentials based on your MySQL setup.</p>

<p>Here's the SQL to create the `reservations` table:</p>

<pre><code>
CREATE TABLE reservations (
    reservation_id INT AUTO_INCREMENT PRIMARY KEY,
    guest_name VARCHAR(255) NOT NULL,
    room_num INT NOT NULL,
    cont_num VARCHAR(20) NOT NULL,
    reservation_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
</code></pre>

<p><strong>Important:</strong></p>
<ul>
  <li>Ensure that the database named `hotel_db` exists in your MySQL server.</li>
  <li>Verify that the username (`root`) and password (`knox`) in the Java code match your MySQL server configuration. Update them if necessary.</li>
  <li>The `reservations` table should be created with the specified columns.</li>
</ul>

<h2>Running the Application</h2>

<p>To run the application:</p>

<ol>
  <li><strong>Compile the Java code:</strong> Navigate to the directory containing the `HRS_JDBC.java` file in your terminal or command prompt and compile it using the Java compiler:
    <pre><code>
    javac Projects_java/Hotel_Reservation_System/HRS_JDBC.java
    </code></pre>
  </li>
  <li><strong>Run the application:</strong> Execute the compiled class file using the Java Virtual Machine. Make sure the MySQL JDBC driver JAR file is in your classpath. You might need to use the `-cp` or `--classpath` option followed by the path to the driver JAR file (e.g., `mysql-connector-j-8.x.x.jar`). Assuming the compiled class is in `Projects_java/Hotel_Reservation_System/`:
    <pre><code>
    java -cp .:path/to/mysql-connector-j-8.x.x.jar Projects_java.Hotel_Reservation_System.HRS_JDBC
    </code></pre>
    (Replace `path/to/mysql-connector-j-8.x.x.jar` with the actual path to your JDBC driver JAR file. The `.` in `-cp` includes the current directory in the classpath.)
  </li>
  <li><strong>Follow the on-screen menu:</strong> The application will present a menu with options to perform different hotel management tasks. Enter the corresponding number to choose an action and follow the prompts.</li>
</ol>

<h2>Code Explanation</h2>

<pre><code class="language-java">
package Projects_java.Hotel_Reservation_System;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.sql.Statement;
import java.util.Scanner;
import java.sql.ResultSet;

public class HRS_JDBC {
    // Database URL, username, and password
    private static final String url = "jdbc:mysql://localhost:3306/hotel_db";
    private static final String username = "root";
    private static final String password = "knox";

    public static void main(String[] args) throws ClassNotFoundException, SQLException {
        // Load the JDBC driver
        try{
            Class.forName("com.mysql.cj.jdbc.Driver");
        }
        catch (ClassNotFoundException e) {
            System.out.println("Error loading driver: " + e.getMessage());
            return;
        }

        try {
            // Establishing the con
            Connection con = DriverManager.getConnection(url, username, password);

            while(true){
                System.out.println();
                System.out.println("HOTEL MANAGEMENT SYSTEM");
                Scanner sc = new Scanner(System.in);
                System.out.println("1. Reserve a room");
                System.out.println("2. View Reservations");
                System.out.println("3. Get Room Number");
                System.out.println("4. Update Reservations");
                System.out.println("5. Delete Reservations");
                System.out.println("0. Exit");
                System.out.print("Choose an option: ");
                int choice = sc.nextInt();
                switch (choice) {
                    case 1:
                        reserveRoom(con, sc);
                        break;
                    case 2:
                        viewReservations(con);
                        break;
                    case 3:
                        getRoomNumber(con, sc);
                        break;
                    case 4:
                        updateReservation(con, sc);
                        break;
                    case 5:
                        deleteReservation(con, sc);
                        break;
                    case 0:
                        exit();
                        sc.close();
                        return;
                    default:
                        System.out.println("Invalid choice. Try again.");
                }
            }

        }
        catch(SQLException e){
            System.out.println("Error while connecting to the database: " + e.getMessage());
        }
        catch(InterruptedException e){
            throw new RuntimeException(e);
        }
    }

    private static void reserveRoom(Connection con, Scanner sc) {
        try {
            System.out.print("Enter guest name: ");
            String guestName = sc.next();
            sc.nextLine();
            System.out.print("Enter room number: ");
            int roomNumber = sc.nextInt();
            System.out.print("Enter contact number: ");
            String contactNumber = sc.next();

            String sql = "INSERT INTO reservations (guest_name, room_num, cont_num) " +
                         "VALUES ('" + guestName + "', " + roomNumber + ", '" + contactNumber + "')";

            try (Statement statement = con.createStatement()) {
                int affectedRows = statement.executeUpdate(sql);

                if (affectedRows > 0) {
                    System.out.println("Reservation successful!");
                } else {
                    System.out.println("Reservation failed.");
                }
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }

    private static void viewReservations(Connection con) throws SQLException {
        String sql = "SELECT reservation_id, guest_name, room_num, cont_num, reservation_date FROM reservations";

        try (Statement statement = con.createStatement();
             ResultSet resultSet = statement.executeQuery(sql)) {

            System.out.println("Current Reservations:");
            System.out.println("+----------------+-----------------+---------------+----------------------+-------------------------+");
            System.out.println("| Reservation ID | Guest             | Room Number   | Contact Number       | Reservation Date          |");
            System.out.println("+----------------+-----------------+---------------+----------------------+-------------------------+");

            while (resultSet.next()) {
                int reservationId = resultSet.getInt("reservation_id");
                String guestName = resultSet.getString("guest_name");
                int roomNumber = resultSet.getInt("room_num");
                String contactNumber = resultSet.getString("cont_num");
                String reservationDate = resultSet.getTimestamp("reservation_date").toString();

                // Format and display the reservation data in a table-like format
                System.out.printf("| %-14d | %-15s | %-13d | %-20s | %-25s |\n",
                        reservationId, guestName, roomNumber, contactNumber, reservationDate);
            }

            System.out.println("+----------------+-----------------+---------------+----------------------+-------------------------+");
        }
    }


    private static void getRoomNumber(Connection con, Scanner sc) {
        try {
            System.out.print("Enter reservation ID: ");
            int reservationId = sc.nextInt();
            System.out.print("Enter guest name: ");
            String guestName = sc.next();

            String sql = "SELECT room_num FROM reservations " +
                         "WHERE reservation_id = " + reservationId +
                         " AND guest_name = '" + guestName + "'";

            try (Statement statement = con.createStatement();
                 ResultSet resultSet = statement.executeQuery(sql)) {

                if (resultSet.next()) {
                    int roomNumber = resultSet.getInt("room_num");
                    System.out.println("Room number for Reservation ID " + reservationId +
                            " and Guest " + guestName + " is: " + roomNumber);
                } else {
                    System.out.println("Reservation not found for the given ID and guest name.");
                }
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
    private static void updateReservation(Connection con, Scanner sc) {
        try {
            System.out.print("Enter reservation ID to update: ");
            int reservationId = sc.nextInt();
            sc.nextLine(); // Consume the newline character

            if (!reservationExists(con, reservationId)) {
                System.out.println("Reservation not found for the given ID.");
                return;
            }

            System.out.print("Enter new guest name: ");
            String newGuestName = sc.nextLine();
            System.out.print("Enter new room number: ");
            int newRoomNumber = sc.nextInt();
            System.out.print("Enter new contact number: ");
            String newContactNumber = sc.next();

            String sql = "UPDATE reservations SET guest_name = '" + newGuestName + "', " +
                         "room_num = " + newRoomNumber + ", " +
                         "cont_num = '" + newContactNumber + "' " +
                         "WHERE reservation_id = " + reservationId;

            try (Statement statement = con.createStatement()) {
                int affectedRows = statement.executeUpdate(sql);

                if (affectedRows > 0) {
                    System.out.println("Reservation updated successfully!");
                } else {
                    System.out.println("Reservation update failed.");
                }
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }

    private static void deleteReservation(Connection con, Scanner sc) {
        try {
            System.out.print("Enter reservation ID to delete: ");
            int reservationId = sc.nextInt();

            if (!reservationExists(con, reservationId)) {
                System.out.println("Reservation not found for the given ID.");
                return;
            }

            String sql = "DELETE FROM reservations WHERE reservation_id = " + reservationId;

            try (Statement statement = con.createStatement()) {
                int affectedRows = statement.executeUpdate(sql);

                if (affectedRows > 0) {
                    System.out.println("Reservation deleted successfully!");
                } else {
                    System.out.println("Reservation deletion failed.");
                }
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }

    private static boolean reservationExists(Connection con, int reservationId) {
        try {
            String sql = "SELECT reservation_id FROM reservations WHERE reservation_id = " + reservationId;

            try (Statement statement = con.createStatement();
                 ResultSet resultSet = statement.executeQuery(sql)) {

                return resultSet.next(); // If there's a result, the reservation exists
            }
        } catch (SQLException e) {
            e.printStackTrace();
            return false; // Handle database errors as needed
        }
    }


    public static void exit() throws InterruptedException {
        System.out.print("Exiting System");
        int i = 5;
        while(i!=0){
            System.out.print(".");
            Thread.sleep(1000);
            i--;
        }
        System.out.println();
        System.out.println("ThankYou For Using Hotel Reservation System!!!");
    }
}
</code></pre>

<h2>Further Development</h2>

<p>This is a functional command-line Hotel Management System. Potential areas for further development include:</p>

<ul>
  <li><strong>Input Validation:</strong> Implement more robust input validation to prevent errors due to incorrect user input (e.g., non-numeric room numbers, invalid contact formats).</li>
  <li><strong>Error Handling:</strong> Improve error handling for database operations to provide more informative messages to the user.</li>
  <li><strong>User Interface:</strong> Develop a graphical user interface (GUI) using libraries like Swing or JavaFX for a more user-friendly experience.</li>
  <li><strong>Room Management:</strong> Add functionality to manage rooms (e.g., add new rooms, update room details, track room availability).</li>
  <li><strong>Date/Time Handling:</strong> Implement more sophisticated handling of reservation dates and times.</li>
  <li><strong>Searching and Filtering:</strong> Allow users to search and filter reservations based on various criteria (e.g., guest name, room number, reservation date).</li>
  <li><strong>Reporting:</strong> Generate reports on reservations, occupancy rates, etc.</li>
  <li><strong>Security:</strong> Implement security measures, especially if the application were to handle more sensitive data.</li>
</ul>
