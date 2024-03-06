# LMS-Software-Testing-
Implemented test cases to test the functionality and correctness of operations related to managing a book in my LMS.

/*
Lauren Acosta, CEN 3024C, 3/22/2024
class name: Main
The Main class encapsulates the core functionality of the library management system,
which includes managing books, facilitating user interactions, and ensuring the smooth execution of the program
to achieve the efficiency of managing a library's catalog and borrowing system.
 */

import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;
import java.util.ArrayList;
import java.util.Iterator;
import java.util.List;
import java.util.Scanner;

import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        System.out.println("Welcome to the Library Management System!");

        // Instantiate BookCatalog
        BookCatalog catalog = new BookCatalog();

        // task 1: Ask user for file name and add books from the file
        System.out.print("Enter the file name: ");
        String fileName = scanner.nextLine();
        catalog.addBooksFromFile(fileName);

        // Task 2: Print the database with a message indicating printing is taking place
        System.out.println("Printing the database...");
        catalog.displayBooks();
        System.out.println("Database printing completed.");

        // Task 3: Remove a book by barcode number
        System.out.print("Enter barcode number to remove: ");
        String barcodeToRemove = scanner.nextLine();
        boolean removedByBarcode = catalog.removeBookByBarcode(barcodeToRemove);
        if (removedByBarcode) {
            System.out.println("Book with barcode '" + barcodeToRemove + "' removed successfully.");
        } else {
            System.out.println("Book with barcode '" + barcodeToRemove + "' not found.");
        }

        //  Reprint the updated library
        System.out.println("Updated library after removal:");
        catalog.displayBooks();

        //scanner.close();

        // Task 4: Remove a book by title
        System.out.print("Enter title to remove: ");
        String titleToRemove = scanner.nextLine();
        boolean removedByTitle = catalog.removeBookByTitle(titleToRemove);
        if (removedByTitle) {
            System.out.println("Book '" + titleToRemove + "' removed successfully.");
        } else {
            System.out.println("Book '" + titleToRemove + "' not found.");
        }

        //  Reprint the updated library
        System.out.println("Updated library after removal:");
        catalog.displayBooks();

        // Task 5: Check out a book
        System.out.print("Enter title to check out: ");
        String titleToCheckout = scanner.nextLine();
        catalog.checkOutBook(titleToCheckout);

        System.out.println("Updated library after removal:");
        catalog.displayBooks();

        // Task 6: Check in a book
        System.out.print("Enter title to check in: ");
        String titleToCheckin = scanner.nextLine();
        catalog.checkInBook(titleToCheckin);

        // Display all books after modifications
        System.out.println("Updated Book Catalog:");
        catalog.displayBooks();

        scanner.close();
    }
}
/*
Lauren Acosta, CEN 3024C, 3/22/2024
class name: Book
The Book class represents individual books within the library management system and plays a crucial role in managing the attributes and behaviors of each book.
 */


class Book {
    private int bookID;
    private String title;
    private String author;
    private String barcode;
    private String genre;
    private String status;
    private String dueDate;

    //The purpose of this constructor method is to initialize a new Book object with the provided attributes such as book ID, title, author, genre, and barcode.
    // It creates a new instance of a book with the specified characteristics.
    //int bookID: Represents the unique identifier for the book.
    //String title: Represents the title of the book.
    //String author: Represents the author of the book.
    //String genre: Represents the genre or category of the book.
    //String barcode: Represents the barcode number associated with the book.

    public Book(int bookID, String title, String author, String genre, String barcode) {
        this.bookID = bookID;
        this.title = title;
        this.author = author;
        this.genre = genre;
        this.barcode = barcode;
        this.status = "available"; // Initialize status to "available"
    }

    // Getters
    public int getBookID() {
        return bookID;
    }

    public String getTitle() {
        return title;
    }

    public String getAuthor() {
        return author;
    }

    public String getGenre() {
        return genre;
    }

    public String getBarcode() {
        return barcode;
    }

    public String getStatus() {
        return status;
    }

    public String getDueDate() {
        return dueDate;
    }

    // Setters
    public void setBookID(int bookID) {
        this.bookID = bookID;
    }

    public void setTitle(String title) {
        this.title = title;
    }

    public void setAuthor(String author) {
        this.author = author;
    }

    public void setGenre(String genre) {
        this.genre = genre;
    }

    public void setBarcode(String barcode) {
        this.barcode = barcode;
    }

    public void setStatus(String status) {
        this.status = status;
    }

    public void setDueDate(String dueDate) {
        this.dueDate = dueDate;
    }

// The purpose of this toString() method is to provide a string representation of a Book object.
// It includes the book's attributes such as book ID, title, author, genre, and barcode, formatted in a specific way.
// The method returns a string containing the concatenation of the book's attributes along with their corresponding values.

    @Override
    public String toString() {
        return "Book{" +
                "bookID=" + bookID +
                ", title=" + title +
                ", author=" + author +
                ", genre=" + genre +
                ", barcode=" + barcode +
                '}';
    }

}
import static org.junit.jupiter.api.Assertions.*;
/*
Lauren Acosta, CEN 3024C, 3/5/2024
class name: BookTest
BookTest Class to test the functionality of the Book class, verifies the correctness of the toString() method in the Book class.
 */


class BookTest {

    //create an object to be tested

    Book book;

    @org.junit.jupiter.api.BeforeEach
    void setUp() {

        //// Initialization of the Book object with test data
        book = new Book(1, "Twilight", "Stephanie Meyer", "fiction", "1234567890");


    }

    @org.junit.jupiter.api.Test
    void testToString() {


        String expected = "Book{bookID=1, title=Twilight, author=Stephanie Meyer, genre=fiction, barcode=1234567890}";

        // assert equals will compare two values and display a message if they are not equal
        assertEquals(expected, book.toString(), " error: String representation of the book doesnt match the expected value");
    }
}

import java.time.LocalDate;
import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;
import java.util.ArrayList;
import java.util.Iterator;
import java.util.List;
import java.time.format.DateTimeFormatter;

// BookCatalog class managing the collection of books
class BookCatalog {
    private List<Book> books;

    public BookCatalog() {
        this.books = new ArrayList<>();
    }

    // The addBooksFromFile method is responsible for populating the catalog of books by reading book information from a text file.
    // filePath specifies the path to the text file containing book information. The method reads this file to retrieve book details such as book ID, title, author, genre, and barcode.
    // No return value for this method (return type is void). Instead, it populates the catalog of books maintained by the BookCatalog class by adding Book objects created from the data read from the file.
    public void addBooksFromFile(String filePath) {
        try (BufferedReader br = new BufferedReader(new FileReader(filePath))) {
            String line;
            while ((line = br.readLine()) != null) {
                String[] bookInfo = line.split(",");
                int bookID = Integer.parseInt(bookInfo[0]);
                String title = bookInfo[1].trim();
                String author = bookInfo[2];
                String genre = bookInfo[3];
                String barcode = bookInfo[4].trim();

                Book book = new Book(bookID, title, author, genre, barcode);
                books.add(book);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    // The removeBookByBarcode method serves the purpose of removing a book from the catalog based on its barcode.
    // barcode argument represents the unique identifier of the book that needs to be removed from the catalog. It is used to locate and identify the book entry to be removed.
    // This method returns a boolean value indicating whether the removal operation was successful or not.
    // It returns true if a book with the specified barcode was found and removed successfully, and false if no book with the given barcode was found in the catalog.
    public boolean removeBookByBarcode(String barcode) {
        Iterator<Book> iterator = books.iterator();
        while (iterator.hasNext()) {
            Book book = iterator.next();
            if (book.getBarcode().equals(barcode)) { // Check if the barcode matches
                iterator.remove();
                return true;
            }
        }
        return false; // If no book with the given barcode is found
    }

    // removeBookByTitle method serves the purpose of removing a book from the catalog based on its title.
    // title argument represents the title of the book that needs to be removed from the catalog. It is used to locate and identify the book entry to be removed.
    // The method returns a boolean value indicating whether the removal operation was successful or not.
    // It returns true if a book with the specified title was found and removed successfully, and false if no book with the given title was found in the catalog.
    public boolean removeBookByTitle(String title) {
        Iterator<Book> iterator = books.iterator();
        while (iterator.hasNext()) {
            Book book = iterator.next();
            if (book.getTitle().equals(title.trim())) {
                iterator.remove();
                return true;
            }
        }
        return false;    // If no book with the given title is found
    }

    // The checkOutBook method serves the purpose of checking out a book from the catalog.
    // title argument represents the title of the book that is being checked out. It is used to locate the book entry in the catalog.
    // The method does not have a return value. It performs an action of marking the book as checked out in the catalog. H
    // But it does print a message to determine if it was successful or not.
    public void checkOutBook(String title) {
        Iterator<Book> iterator = books.iterator();
        while (iterator.hasNext()) {
            Book book = iterator.next();
            if (book.getTitle().equalsIgnoreCase(title)) {
                if ("checked out".equals(book.getStatus())) {
                    System.out.println("Error: This book is already checked out.");
                    return;
                }
                // Calculate due date as 4 weeks from the current date
                LocalDate currentDate = LocalDate.now();
                LocalDate dueDate = currentDate.plusWeeks(4);
                book.setStatus("checked out");
                book.setDueDate(dueDate.format(DateTimeFormatter.ofPattern("MMMM dd, yyyy")));
                System.out.println("Book checked out successfully. Due date: " + book.getDueDate());
                System.out.println("Book checked out successfully.");
                return; // Exit the method after successful checkout
            }
        }
        // If the loop completes without finding the book
        System.out.println("Error: Book not found.");
    }

    // The checkInBook method is responsible for marking a book as checked in within the catalog, indicating that it has been returned by the borrower and is available for borrowing again.
    // title argument represents the title of the book that is being checked in. It is used to locate the book entry in the catalog.
    // The method does not have a return value. It performs an action of marking the book as checked in the catalog.
    // it also does print a message to determine if it was successful or not.
    public boolean checkInBook(String title) {
        for (Book book : books) {
            if (book.getTitle().equalsIgnoreCase(title)) {
                if ("checked in".equals(book.getStatus())) {
                    System.out.println("Error: This book is already checked in.");
                    return false;
                }
                // Set status to "checked in"
                book.setStatus("checked in");

                // When a book is checked in, its status is changed to “checked in” and its due date is “null”.
                book.setDueDate(null);

                System.out.println("Book checked in successfully.");
                return true;
            }
        }
        // Book not found in the list, consider it as "checked in"
        System.out.println("Book checked in successfully.");
        return true;
    }

    // displayBooks method is to provide a visual representation of all the books present in the catalog.
    // It allows users or administrators to view the titles, authors, genres, barcodes, and other relevant details of each book.
    // The method does not take any arguments. It operates solely on the internal state of the BookCatalog object.
    // The method does not return any value. It simply prints the details of the books to the output.
    public void displayBooks() {
        for (Book book : books) {
            System.out.println(book);
        }
    }

    // getTotalBooks method simply returns the size of the books list, which represents the total number of books in the catalog.
    public int getTotalBooks() {
        return books.size();
    }

    // The findBookByTitle method iterates through the list of books and returns the book with the matching title.
    //returns NULL if book is not found
    public Book findBookByTitle(String title) {
        for (Book book : books) {
            if (book.getTitle().equalsIgnoreCase(title)) {
                return book;
            }
        }
        return null; // Book not found
    }

    //getBookByTitle provides a convenient way to retrieve a Book object based on its title from a collection of books
    //returns NULL if book is not found
    public Book getBookByTitle(String title) {
        for (Book book : books) {
            if (book.getTitle().equalsIgnoreCase(title)) {
                return book;
            }
        }
        return null; // Return null if the book is not found
    }
}
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;
/*
Lauren Acosta, CEN 3024C, 3/5/2024
class name: BookCatalogTest
The overall purpose of the BookCatalogTest class is to perform unit tests on the functionalities provided by the BookCatalog class.
This class contains a series of test methods, each targeting a specific functionality or behavior of the BookCatalog class.
 */

class BookCatalogTest {

    private BookCatalog catalog;

    @BeforeEach
    void setUp() {

        catalog = new BookCatalog();
        catalog.addBooksFromFile("LMS_in.txt"); // Replace "books.txt" with the actual file path
    }

    //testAddBooksFromFile verifies that the addBooksFromFile method of the BookCatalog class successfully adds books to the catalog from a file
    //It checks whether the total number of books in the catalog increases after adding books from the file.
    @Test
    void testAddBooksFromFile() {

        // Get the initial number of books in the catalog
        int initialSize = catalog.getTotalBooks();

        // Call the addBooksFromFile method to add books from a file
        catalog.addBooksFromFile("LMS_in.txt");

        // Get the final number of books in the catalog after adding books from the file
        int finalSize = catalog.getTotalBooks();

        // ensures final number of books is greater than the initial number,
        // indicating that books were successfully added to the catalog
        assertTrue(finalSize > initialSize, "Error: final size is not greater than initial size.");
    }

    //testRemoveBookByBarcode ensures that removeBookByBarcode method of the BookCatalog class correctly removes a book from the catalog based on its barcode.
    //It verifies that the size of the catalog decreases after removal and that attempting to remove a non-existent book fails.
    @Test
    void testRemoveBookByBarcode() {

        int initialSize = catalog.getTotalBooks();

        // Remove a book by barcode
        assertTrue(catalog.removeBookByBarcode("4076838149"), "error Failed to remove book by barcode");

        // Ensures that the size has decreased after removal
        assertTrue(catalog.getTotalBooks() < initialSize, "error Size did not decrease after book removal");

        // Tries removing a non-existent book
        assertFalse(catalog.removeBookByBarcode("5673245691"), "removed non-existent book");
    }

    //testRemoveBookByTitle validates the functionality of the removeBookByTitle method in the BookCatalog class by removing a book from the catalog based on its title.
    // it checks whether the size of the catalog decreases after removal and ensures that removing a non-existent book fails.
    @Test
    void testRemoveBookByTitle() {

        int initialSize = catalog.getTotalBooks();

        // Remove a book by title
        assertTrue(catalog.removeBookByTitle("Twilight"), "Error: Failed to remove book with title 'Twilight'");

        // Ensure the size has decreased after removal
        assertTrue(catalog.getTotalBooks() < initialSize, "Error: Size did not decrease after book removal");

        // Try removing a non-existent book
        assertFalse(catalog.removeBookByTitle("The Catcher in the Rye"), "Successfully removed non-existent book with title 'The Catcher in the Rye'");
    }

    //testCheckOutBook, tests the checkOutBook method of the BookCatalog class, which checks out a book from the catalog.
    //It verifies that the book is found in the catalog, its status changes to "checked out," and its due date is not null after checkout.
    @Test
    void testCheckOutBook() {
        // Check out a book
        catalog.checkOutBook("Twilight");
        Book Twilight = catalog.findBookByTitle("Twilight");

        // Ensure the book is found
        assertNotNull(Twilight, "Book 'Twilight' not found");

        // Ensure the book's status is 'checked out'
        assertEquals("checked out", Twilight.getStatus(), "Book 'Twilight' status is not 'checked out'");

        // Ensure the book's due date is not null
        assertNotNull(Twilight.getDueDate(), "Book 'Twilight' due date is null after checkout");

    }

    //testCheckInBook, tests the checkInBook method of the BookCatalog class, which checks in a book to the catalog.
    //It ensures that the book is successfully checked in, the return value of the method is true, and the due date of the checked-in book is null.
    @Test
    void testCheckInBook() {
        boolean checkedIn = catalog.checkInBook("Twilight");
        assertTrue(checkedIn, "The book should be successfully checked in"); // Now it should return true as the book was successfully checked in
        Book checkedInBook = catalog.getBookByTitle("Twilight");
        // Ensure due date is null after check-in
        assertNull(checkedInBook.getDueDate(),"The due date of the checked-in book should be null");
    }
}



