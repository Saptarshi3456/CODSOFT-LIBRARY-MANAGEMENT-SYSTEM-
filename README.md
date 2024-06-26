# CODSOFT-LIBRARY-MANAGEMENT-SYSTEM-
Write a C++ program to develop a system to manage books, borrowers and transactions in a Library.

#include <iostream>
#include <string>
#include <vector>
#include <ctime>

class Book {
public:
    std::string title;
    std::string author;
    std::string ISBN;
    bool isCheckedOut;
    time_t dueDate;

    Book(std::string title, std::string author, std::string ISBN, time_t dueDate) {
        this->title = title;
        this->author = author;
        this->ISBN = ISBN;
        this->isCheckedOut = false;
        this->dueDate = dueDate;
    }
};

class Library {
private:
    std::vector<Book> bookList;

public:
    void addBook(Book book) {
        bookList.push_back(book);
    }

    void searchBook(std::string searchQuery) {
        bool found = false;
        for (Book book : bookList) {
            if (book.title == searchQuery || book.author == searchQuery || book.ISBN == searchQuery) {
                found = true;
                std::cout << "Title: " << book.title << std::endl;
                std::cout << "Author: " << book.author << std::endl;
                std::cout << "ISBN: " << book.ISBN << std::endl;
                std::cout << "Availability: " << (book.isCheckedOut ? "Not Available" : "Available") << std::endl;
            }
        }
        if (!found) {
            std::cout << "No books found with the search query: " << searchQuery << std::endl;
        }
    }

    void checkOutBook(std::string title) {
        for (Book &book : bookList) {
            if (book.title == title && !book.isCheckedOut) {
                book.isCheckedOut = true;
                std::cout << "Book checked out successfully." << std::endl;
                time_t currentTime;
                time(&currentTime);
                book.dueDate = currentTime + (30 * 24 * 60 * 60); // Adding 30 days to the current date
                return;
            }
        }
        std::cout << "Could not find the book or it is already checked out." << std::endl;
    }

    void returnBook(std::string title) {
        for (Book &book : bookList) {
            if (book.title == title && book.isCheckedOut) {
                book.isCheckedOut = false;
                time_t currentTime;
                time(&currentTime);
                int overdueDays = difftime(currentTime, book.dueDate) / (24 * 60 * 60);
                if (overdueDays > 0) {
                    std::cout << "The book is overdue by " << overdueDays << " days. The fine is $" << 2 * overdueDays << std::endl;
                } else {
                    std::cout << "Book returned successfully." << std::endl;
                }
                return;
            }
        }
        std::cout << "Could not find the book or it is not checked out." << std::endl;
    }

    void fineCalculator() {
        for (Book &book : bookList) {
            if (book.isCheckedOut) {
                time_t currentTime;
                time(&currentTime);
                int overdueDays = difftime(currentTime, book.dueDate) / (24 * 60 * 60);
                if (overdueDays > 0) {
                    std::cout << "The book '" << book.title << "' is overdue by " << overdueDays << " days. The fine is $" << 2 * overdueDays << std::endl;
                }
            }
        }
    }
};

int main() {
    Library library;

    // Adding books
    library.addBook(Book("C++ Programming Language", "Bjarne Stroustrup", "1234567890", time(0) + (30 * 24 * 60 * 60)));
    library.addBook(Book("Effective Java", "Joshua Bloch", "0987654321", time(0) + (30 * 24 * 60 * 60)));

 library.searchBook("C++ Programming Language");
    

    // Checking out books
    library.checkOutBook("C++ Programming Language");
    library.checkOutBook("Effective Java");

    // Returning books
    library.returnBook("C++ Programming Language");
    library.returnBook("Effective Java");

    // Fines Calculator
    library.fineCalculator();

    return 0;
}
