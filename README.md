# Library Management System README

## Introduction
This is a simple Java program for managing a library. It allows users to register new customers, add new books, display customer and book information, borrow books, and return books.

## Code Structure and Documentation
The program consists of the following classes:

**LibrarySimulationProject:** Main class responsible for running the library simulation. It contains the main method that displays the menu and handles user input.

**Validation:** Class responsible for handling input validation. It contains methods to validate customer name, ISBN, phone number, and email. It also includes methods to check if a customer or book exists in the system.

**Library:** Class representing the library and its operations. It contains methods to add new customers and books, display customers and books, borrow books, and return books.

**Book:** Class representing a book in the library. It contains fields for title, author, ISBN, publisher, and quantity. It includes getter and setter methods for each field and overrides the toString method for a string representation of the Book object.

**Customer:** Class representing a customer of the library. It contains fields for first name, last name, phone number, email, address, state, customer ID, and a list of books borrowed. It includes methods to borrow and return books, generate a unique customer ID, and overrides the toString method for a string representation of the Customer object.

Each class, method, and major code block is extensively documented to explain its functionality and purpose.

### How to Compile the Program
Open NetBeans IDE.

Create a new Java project or open an existing one.

Add the Java files (LibrarySimulationProject.java, Book.java, Customer.java, Validation.java) to your project.

Ensure that all the files are included in the source package.

Right-click on your project in the project explorer.

Select "Build" or "Clean and Build" to compile the program. This will compile all the Java files in your project.

### Running the Program
After compiling, right-click on the project in the project explorer.

Select "Run" or press Shift + F6 to run the program.

The program will start executing, and you will see the output in the NetBeans output console.

Follow the on-screen instructions to navigate through the menu and perform actions.

## Sample Data
Below is the sample data used for testing the library management system:

### Sample Customers
**Customer 1:**

First Name: John

Last Name: Doe

Phone Number: 3094563345

Email: johndoe@example.com

Address: 1000 Main St.

State: IL

**Output Screenshot:**

![image](https://github.com/dm963a/librarysimulationproject/assets/159193565/3e5b6a0c-9723-4386-9d67-a7145288a1c5)


**Customer 2:**

First Name: Alice

Last Name: Smith

Phone Number: 2177891234

Email: asmith@example.com

Address: 200 Maple Ave.

State: NY

**Output Screenshot:**

![image](https://github.com/dm963a/librarysimulationproject/assets/159193565/0fd179fe-5de7-441b-bcb6-666a9d63f9d5)


### Sample Books
**Book 1:**

Title: Introduction to Java Programming

Author: John Smith

ISBN: 1234567890

Publisher: ABC Publications

Quantity: 5

**Book 2:**

Title: Data Structures and Algorithms

Author: Alice Johnson

ISBN: 0987654321

Publisher: XYZ Books

Quantity: 3

## Code


```
import java.util.ArrayList;
import java.util.Scanner;

/**
 *
 * @author Daniel.Mariscal
 */

// Main class responsible for running the library simulation
public class LibrarySimulationProject {

    /**
     * @param args the command line arguments
     */
    public static void main(String[] args) {   

        Library library = new Library(); // Create a library object
        Scanner scanner = new Scanner(System.in); // Create a scanner object for user input
        int choice;
        do {
            // Display main menu options
            System.out.println("Main Menu Options:");
            System.out.println("1. Register New Customer");
            System.out.println("2. Display Customer");
            System.out.println("3. Add New Book");
            System.out.println("4. Display Books");
            System.out.println("5. Borrow Book");
            System.out.println("6. Return Book");
            System.out.println("7. Exit");
            System.out.print("Enter your choice: ");
            choice = scanner.nextInt(); // Read user's choice
            scanner.nextLine(); // Consume newline

            // Perform actions based on user's choice
            switch (choice) {
                case 1:
                    library.addCustomer(); // Register new customer
                    break;
                case 2:
                    library.displayCustomers(); // Display customer information
                    break;
                case 3:
                    library.addBook(); // Add new book
                    break;
                case 4:
                    library.displayBooks(); // Display book information
                    break;
                case 5:
                    library.borrowBook(); // Borrow a book
                    break;
                case 6:
                    library.returnBook(); // Return a book
                    break;
                case 7:
                    System.out.println("Exiting..."); // Exit the program
                    break;
                default:
                    System.out.println("Invalid choice. Please try again."); // Invalid choice
            }
        } while (choice != 7); // Continue until user chooses to exit
        scanner.close(); // Close scanner
    }
}

// Class responsible for handling input validation
class Validation {
    // Validate customer name
    public static boolean validCustomerName(String name) {
        return name.matches("[a-zA-Z ]{1,20}");
    }

     // Validate ISBN
    public static boolean validISBN(String isbn) {
        if (isbn.length() != 10)
            return false;
        int sum = 0;
        for (int i = 0; i < isbn.length(); i++) {
            sum += (isbn.charAt(i) - '0') * (10 - i);
        }
        return sum % 11 == 0;
    }

    // Validate phone number
    public static boolean validPhoneNumber(String phone) {
        return phone.matches("\\d{10}");
    }

    // Validate email
    public static boolean validEmail(String email) {
        String[] parts = email.split("@");
        if (parts.length != 2)
            return false;
        String prefix = parts[0];
        String[] domainParts = parts[1].split("\\.");
        if (domainParts.length != 2)
            return false;
        return prefix.length() >= 5 && prefix.matches("[a-zA-Z0-9_]+") &&
                domainParts[0].length() <= 8 && domainParts[1].matches("(com|org|edu)");
    }
    
    // Check if the provided customer is a valid library customer
    public static boolean checkCustomer(String customerId, ArrayList<Customer> customers) {
        for (Customer customer : customers) {
            if (customer.getCustomerId().equals(customerId)) {
                return true; // Customer found
            }
        }
        return false; // Customer not found
    }

    // Check if the provided ISBN is a valid library book
    public static boolean checkISBN(String isbn, ArrayList<Book> books) {
        for (Book book : books) {
            if (book.getIsbn().equals(isbn)) {
                return true; // Book found
            }
        }
        return false; // Book not found
    }
}

// Class representing the library and its operations
class Library {
    private ArrayList<Book> books = new ArrayList<>(); // List of books in the library
    private ArrayList<Customer> customers = new ArrayList<>(); // List of customers

    // Method to add a new customer to the library
    public void addCustomer() {
        Scanner scanner = new Scanner(System.in);
        System.out.print("Enter customer first name: ");
        String firstName = scanner.nextLine();
        System.out.print("Enter customer last name: ");
        String lastName = scanner.nextLine();
        System.out.print("Enter customer phone number: ");
        String phone = scanner.nextLine();
        System.out.print("Enter customer email: ");
        String email = scanner.nextLine();
        System.out.print("Enter customer address: ");
        String address = scanner.nextLine();
        System.out.print("Enter customer state: ");
        String state = scanner.nextLine();
        if (Validation.validCustomerName(firstName) && Validation.validCustomerName(lastName) &&
                Validation.validPhoneNumber(phone) && Validation.validEmail(email)) {
            Customer customer = new Customer(firstName, lastName, phone, email, address, state);
            customers.add(customer);
            System.out.println("Customer added successfully.");
        } else {
            System.out.println("Invalid input. Customer not added.");
        }
    }

    // Method to add a new book to the library
    public void addBook() {
        Scanner scanner = new Scanner(System.in);
        System.out.print("Enter book title: ");
        String title = scanner.nextLine();
        System.out.print("Enter book author: ");
        String author = scanner.nextLine();
        System.out.print("Enter book ISBN: ");
        String isbn = scanner.nextLine();
        System.out.print("Enter book publisher: ");
        String publisher = scanner.nextLine();
        System.out.print("Enter book quantity: ");
        int quantity = scanner.nextInt();
        scanner.nextLine(); // Consume newline
        if (Validation.validISBN(isbn)) {
            Book book = new Book(title, author, isbn, publisher, quantity);
            books.add(book);
            System.out.println("Book added successfully.");
        } else {
            System.out.println("Invalid ISBN. Book not added.");
        }
    }

    // Method to display all books in the library
    public void displayBooks() {
        System.out.println("List of Books:");
        for (Book book : books) {
            System.out.println(book);
        }
    }

    // Method to display all customers of the library
    public void displayCustomers() {
        System.out.println("List of Customers:");
        for (Customer customer : customers) {
            System.out.println(customer);
        }
    }

    // Method to borrow a book from the library
    public void borrowBook() {
        Scanner scanner = new Scanner(System.in);
        System.out.print("Enter customer ID: ");
        String customerId = scanner.nextLine();
        Customer customer = getCustomer(customerId);
        if (customer != null) {
            System.out.print("Enter book ISBN: ");
            String isbn = scanner.nextLine();
            Book book = getBook(isbn);
            if (book != null) {
                System.out.print("Enter quantity: ");
                int quantity = scanner.nextInt();
                if (book.getQuantity() >= quantity) {
                    customer.borrowBook(book, quantity);
                    book.setQuantity(book.getQuantity() - quantity);
                    System.out.println("Book borrowed successfully.");
                } else {
                    System.out.println("Not enough copies available.");
                }
            } else {
                System.out.println("Book not found.");
            }
        } else {
            System.out.println("Customer not found.");
        }
    }

    // Method to return a book to the library
    public void returnBook() {
        Scanner scanner = new Scanner(System.in);
        System.out.print("Enter customer ID: ");
        String customerId = scanner.nextLine();
        Customer customer = getCustomer(customerId);
        if (customer != null) {
            System.out.print("Enter book ISBN: ");
            String isbn = scanner.nextLine();
            Book book = getBook(isbn);
            if (book != null) {
                System.out.print("Enter quantity: ");
                int quantity = scanner.nextInt();
                customer.returnBook(book, quantity);
                book.setQuantity(book.getQuantity() + quantity);
                System.out.println("Book returned successfully.");
            } else {
                System.out.println("Book not found.");
            }
        } else {
            System.out.println("Customer not found.");
        }
    }

    // Method to retrieve customer by ID
    private Customer getCustomer(String customerId) {
        for (Customer customer : customers) {
            if (customer.getCustomerId().equals(customerId)) {
                return customer;
            }
        }
        return null;
    }

    // Method to retrieve book by ISBN
    private Book getBook(String isbn) {
        for (Book book : books) {
            if (book.getIsbn().equals(isbn)) {
                return book;
            }
        }
        return null;
    }
}

// Class representing a book in the library
class Book {
    // Private fields to store book information
    private String title;
    private String author;
    private String isbn;
    private String publisher;
    private int quantity;

    // Constructor to initialize a Book object with provided details
    public Book(String title, String author, String isbn, String publisher, int quantity) {
        this.title = title;
        this.author = author;
        this.isbn = isbn;
        this.publisher = publisher;
        this.quantity = quantity;
    }

    // Getter method for title
    public String getTitle() {
        return title;
    }

    // Setter method for title
    public void setTitle(String title) {
        this.title = title;
    }

    // Getter method for author
    public String getAuthor() {
        return author;
    }

    // Setter method for author
    public void setAuthor(String author) {
        this.author = author;
    }

    // Getter method for ISBN
    public String getIsbn() {
        return isbn;
    }

    // Setter method for ISBN
    public void setIsbn(String isbn) {
        this.isbn = isbn;
    }

    // Getter method for publisher
    public String getPublisher() {
        return publisher;
    }

    // Setter method for publisher
    public void setPublisher(String publisher) {
        this.publisher = publisher;
    }

    // Getter method for quantity
    public int getQuantity() {
        return quantity;
    }

    // Setter method for quantity
    public void setQuantity(int quantity) {
        this.quantity = quantity;
    }

    // Override toString method to provide a string representation of the Book object
    @Override
    public String toString() {
        return "Book{" +
                "title='" + title + '\'' +
                ", author='" + author + '\'' +
                ", isbn='" + isbn + '\'' +
                ", publisher='" + publisher + '\'' +
                ", quantity=" + quantity +
                '}';
    }
}

// Class representing a customer of the library
class Customer {
    // Private fields to store customer information
    private String firstName;
    private String lastName;
    private String phone;
    private String email;
    private String address;
    private String state;
    private String customerId;
    private ArrayList<Book> booksBorrowed = new ArrayList<>();

    // Constructor to initialize a Customer object with provided details
    public Customer(String firstName, String lastName, String phone, String email, String address, String state) {
        this.firstName = firstName;
        this.lastName = lastName;
        this.phone = phone;
        this.email = email;
        this.address = address;
        this.state = state;
        this.customerId = generateCustomerID(); // Generate unique customer ID
    }

    // Getter method for customer ID
    public String getCustomerId() {
        return customerId;
    }

    // Setter method for customer ID
    public void setCustomerId(String customerId) {
        this.customerId = customerId;
    }

    // Method to borrow a book
    public void borrowBook(Book book, int quantity) {
        // Check if the book is already borrowed
        for (Book borrowedBook : booksBorrowed) {
            if (borrowedBook.getIsbn().equals(book.getIsbn())) {
                borrowedBook.setQuantity(borrowedBook.getQuantity() + quantity);
                return;
            }
        }
        // If not already borrowed, add the book to the borrowed list
        booksBorrowed.add(new Book(book.getTitle(), book.getAuthor(), book.getIsbn(), book.getPublisher(), quantity));
    }

    // Method to return a book
    public void returnBook(Book book, int quantity) {
        for (Book borrowedBook : booksBorrowed) {
            if (borrowedBook.getIsbn().equals(book.getIsbn())) {
                int remainingQuantity = borrowedBook.getQuantity() - quantity;
                if (remainingQuantity > 0) {
                    borrowedBook.setQuantity(remainingQuantity); // Reduce quantity if returned partially
                } else {
                    booksBorrowed.remove(borrowedBook); // Remove book if returned completely
                }
                return;
            }
        }
    }

    // Method to generate a unique customer ID based on first and last initials
    private String generateCustomerID() {
        // Extract first initial
        char firstInitial = firstName.charAt(0);
        // Extract last initial
        char lastInitial = lastName.charAt(0);
        // Generate a random three-digit number
        String randomDigits = String.format("%03d", (int) (Math.random() * 1000));
        // Concatenate initials and random digits to form the customer ID
        String customerId = String.valueOf(firstInitial) + String.valueOf(lastInitial) + randomDigits;

        return customerId.toUpperCase(); // Convert to uppercase for consistency
    }

    // Override toString method to provide a string representation of the Customer object
    @Override
    public String toString() {
        return "Customer{" +
                "firstName='" + firstName + '\'' +
                ", lastName='" + lastName + '\'' +
                ", phone='" + phone + '\'' +
                ", email='" + email + '\'' +
                ", address='" + address + '\'' +
                ", state='" + state + '\'' +
                ", customerId='" + customerId + '\'' +
                ", booksBorrowed=" + booksBorrowed +
                '}';
    }
}
```
