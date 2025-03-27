# Banking System in Core Java
🔹 Banking System in Core Java – Overview
This Core Java-based Banking System is a console-based or GUI-based application for managing user accounts, transactions, and bank-related operations. It follows OOP principles, utilizing Java classes, file handling, and exception handling for secure transactions.
🔹 Key Features & Modules
🏦 Account Management
✔ Create, update, and delete bank accounts.
✔ Different types of accounts (Savings, Current, Business).
✔ Store account details using file handling or a database.

💰 Transactions
✔ Deposit, withdraw, and transfer money between accounts.
✔ Generate account statements and transaction history.
✔ Implement security checks for fraud prevention.

🔒 User Authentication
✔ Secure login system with username and PIN/password.
✔ Role-based access: Admin & Customer.
✔ Password encryption using Java Security API (if applicable).

📊 Balance Inquiry & Reports
✔ Users can check their current balance anytime.
✔ Print transaction history and bank statements.

📂 Data Persistence
✔ Uses file handling (.txt, .csv, .dat) or a database (MySQL, SQLite) to store user details.

⚙️ Admin Features
✔ View all customer accounts.
✔ Modify account details and transactions.
✔ Generate reports on bank activities.

🔹 Project Structure (Example Folder Layout)
BankingSystem/
│── src/
│   ├── main/
│   │   ├── Account.java
│   │   ├── Bank.java
│   │   ├── Transaction.java
│   │   ├── Admin.java
│   │   ├── Customer.java
│   │   ├── Main.java
│   ├── utils/
│   │   ├── FileHandler.java
│   │   ├── SecurityUtil.java
│── data/
│   ├── accounts.txt
│   ├── transactions.txt
│── README.md
│── BankingSystem.jar
🔹 Code Implementation
📌 1. Account Class (Model for Bank Accounts)
import java.io.Serializable;

public class Account implements Serializable {
    private String accountNumber;
    private String accountHolder;
    private double balance;

    public Account(String accountNumber, String accountHolder, double balance) {
        this.accountNumber = accountNumber;
        this.accountHolder = accountHolder;
        this.balance = balance;
    }

    public void deposit(double amount) {
        balance += amount;
    }

    public boolean withdraw(double amount) {
        if (amount > balance) {
            System.out.println("Insufficient funds!");
            return false;
        }
        balance -= amount;
        return true;
    }

    public String getAccountDetails() {
        return "Account No: " + accountNumber + ", Holder: " + accountHolder + ", Balance: $" + balance;
    }
}
📌 2. Transaction Handling Class
import java.util.Date;

public class Transaction {
    private String accountNumber;
    private double amount;
    private String type;
    private Date date;

    public Transaction(String accountNumber, double amount, String type) {
        this.accountNumber = accountNumber;
        this.amount = amount;
        this.type = type;
        this.date = new Date();
    }

    public String getTransactionDetails() {
        return date + " - " + type + ": $" + amount + " [Account: " + accountNumber + "]";
    }
}
📌 3. File Handling for Data Storage
import java.io.*;
import java.util.ArrayList;
import java.util.List;

public class FileHandler {
    private static final String FILE_NAME = "data/accounts.txt";

    public static void saveAccount(Account account) {
        try (FileWriter writer = new FileWriter(FILE_NAME, true);
             BufferedWriter bw = new BufferedWriter(writer)) {
            bw.write(account.getAccountDetails() + "\n");
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    public static List<String> loadAccounts() {
        List<String> accounts = new ArrayList<>();
        try (BufferedReader br = new BufferedReader(new FileReader(FILE_NAME))) {
            String line;
            while ((line = br.readLine()) != null) {
                accounts.add(line);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
        return accounts;
    }
}
📌 4. Main Class (Program Entry Point)
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        Account account = new Account("123456", "John Doe", 1000.00);

        while (true) {
            System.out.println("\nBanking System Menu:");
            System.out.println("1. Deposit");
            System.out.println("2. Withdraw");
            System.out.println("3. View Balance");
            System.out.println("4. Exit");
            System.out.print("Enter choice: ");
            int choice = scanner.nextInt();

            switch (choice) {
                case 1:
                    System.out.print("Enter amount to deposit: ");
                    double depositAmount = scanner.nextDouble();
                    account.deposit(depositAmount);
                    System.out.println("Deposit Successful.");
                    break;
                case 2:
                    System.out.print("Enter amount to withdraw: ");
                    double withdrawAmount = scanner.nextDouble();
                    if (account.withdraw(withdrawAmount)) {
                        System.out.println("Withdrawal Successful.");
                    }
                    break;
                case 3:
                    System.out.println(account.getAccountDetails());
                    break;
                case 4:
                    System.out.println("Thank you for using the Banking System.");
                    scanner.close();
                    System.exit(0);
                    break;
                default:
                    System.out.println("Invalid choice! Please try again.");
            }
        }
    }
}
To extend this Banking System in Core Java, I'll outline the next steps for adding:

1️⃣ GUI using Java Swing/JavaFX
2️⃣ Database Integration (MySQL/SQLite)
3️⃣ Multi-User Authentication (Admin & Customer)

1️⃣ Enhancing UI with Java Swing/JavaFX
A Graphical User Interface (GUI) will replace the console-based interface, making it more user-friendly.

🔹 Example: Java Swing GUI for Login & Banking Operations
import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

public class BankingGUI {
    private JFrame frame;
    private JTextField usernameField;
    private JPasswordField passwordField;

    public BankingGUI() {
        frame = new JFrame("Banking System");
        frame.setSize(350, 200);
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setLayout(new GridLayout(3, 2));

        JLabel userLabel = new JLabel("Username:");
        usernameField = new JTextField();
        JLabel passLabel = new JLabel("Password:");
        passwordField = new JPasswordField();

        JButton loginButton = new JButton("Login");
        loginButton.addActionListener(new ActionListener() {
            public void actionPerformed(ActionEvent e) {
                authenticateUser(usernameField.getText(), new String(passwordField.getPassword()));
            }
        });

        frame.add(userLabel);
        frame.add(usernameField);
        frame.add(passLabel);
        frame.add(passwordField);
        frame.add(loginButton);

        frame.setVisible(true);
    }

    private void authenticateUser(String username, String password) {
        if (username.equals("admin") && password.equals("admin123")) {
            JOptionPane.showMessageDialog(frame, "Login Successful!");
        } else {
            JOptionPane.showMessageDialog(frame, "Invalid Credentials!");
        }
    }

    public static void main(String[] args) {
        new BankingGUI();
    }
}
✅ This GUI provides a login form using Swing.
✅ After login, it can open a dashboard for banking operations.

2️⃣ Adding Database Integration (MySQL/SQLite)
Instead of storing user data in text files, we will store it in a relational database.

🔹 Database Schema (MySQL)
CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(50) UNIQUE NOT NULL,
    password VARCHAR(255) NOT NULL,
    role ENUM('ADMIN', 'CUSTOMER') NOT NULL
);

CREATE TABLE accounts (
    id INT AUTO_INCREMENT PRIMARY KEY,
    account_number VARCHAR(20) UNIQUE NOT NULL,
    user_id INT,
    balance DOUBLE DEFAULT 0.0,
    FOREIGN KEY (user_id) REFERENCES users(id)
);

CREATE TABLE transactions (
    id INT AUTO_INCREMENT PRIMARY KEY,
    account_number VARCHAR(20),
    amount DOUBLE NOT NULL,
    type ENUM('DEPOSIT', 'WITHDRAW'),
    timestamp TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (account_number) REFERENCES accounts(account_number)
);
🔹 JDBC Code for Connecting to MySQL
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;

public class DatabaseConnection {
    private static final String URL = "jdbc:mysql://localhost:3306/banking";
    private static final String USER = "root";
    private static final String PASSWORD = "password";

    public static Connection getConnection() {
        try {
            return DriverManager.getConnection(URL, USER, PASSWORD);
        } catch (SQLException e) {
            throw new RuntimeException("Error connecting to the database!", e);
        }
    }
}
✅ This allows the Java application to interact with MySQL.

🔹 Storing & Retrieving Users from Database
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

public class UserDAO {
    public boolean authenticate(String username, String password) {
        String query = "SELECT * FROM users WHERE username = ? AND password = ?";
        try (Connection conn = DatabaseConnection.getConnection();
             PreparedStatement stmt = conn.prepareStatement(query)) {
            stmt.setString(1, username);
            stmt.setString(2, password);
            ResultSet rs = stmt.executeQuery();
            return rs.next();
        } catch (SQLException e) {
            e.printStackTrace();
        }
        return false;
    }
}
✅ This method checks if the user exists in the database.
✅ For security, we should use password hashing (BCrypt) instead of storing plain-text passwords.

3️⃣ Multi-User Authentication (Admin & Customer)
The banking system should allow:
✔ Admins to manage users and transactions.
✔ Customers to perform banking operations.
🔹 Adding Role-Based Authentication
public String getUserRole(String username) {
    String query = "SELECT role FROM users WHERE username = ?";
    try (Connection conn = DatabaseConnection.getConnection();
         PreparedStatement stmt = conn.prepareStatement(query)) {
        stmt.setString(1, username);
        ResultSet rs = stmt.executeQuery();
        if (rs.next()) {
            return rs.getString("role");
        }
    } catch (SQLException e) {
        e.printStackTrace();
    }
    return null;
}
✅ This function retrieves the role of a user from the database.

🔹 Role-Based Login with GUI
private void authenticateUser(String username, String password) {
    UserDAO userDAO = new UserDAO();
    if (userDAO.authenticate(username, password)) {
        String role = userDAO.getUserRole(username);
        if ("ADMIN".equals(role)) {
            new AdminDashboard(); // Open admin dashboard
        } else {
            new CustomerDashboard(); // Open customer dashboard
        }
    } else {
        JOptionPane.showMessageDialog(frame, "Invalid Credentials!");
    }
}
✅ Admins and customers get redirected to different dashboards after login.

Final Expected Outputs
✅ Fully functional Banking System with GUI (Java Swing/JavaFX).
✅ Secure authentication with role-based access (Admin & Customer).
✅ Database integration for real-time transactions and account management.
✅ User-friendly interface with login, deposit, withdrawal, and history features.

Real-Time Applications
📌 Banking Portals → For online transactions, customer accounts, and admin management.
📌 ATM Machines → Secure transaction handling for withdrawals and deposits.
📌 Mobile Banking Apps → Digital wallets and online banking features.

