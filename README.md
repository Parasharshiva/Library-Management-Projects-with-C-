# Library-Management-Projects-with-C-
It was a group project and my 2nd project during academics. When i studied in 2nd year,  we all are engaged this project and got to learn lots of new things along with new challenges 
<br>
Author:- Shivam Parashar
<br>
code:-
<br>
#include <iostream>
#include <iomanip>
#include <vector>
#include <string>
#include <algorithm>

using namespace std;

class Book {
public:
    string title;
    string author;
    int id;
    int quantity; // New variable to track the number of copies available

    Book(string t, string a, int i, int q) : title(t), author(a), id(i), quantity(q) {}

    bool isAvailable() const {
        return quantity > 0;
    }

    void lendBook() {
        if (quantity > 0) {
            quantity--;
            cout << "Book successfully lent. Remaining quantity: " << quantity << "\n";
        } else {
            cout << "No copies available for lending.\n";
        }
    }

    void returnBook() {
        quantity++;
        cout << "Book successfully returned. Updated quantity: " << quantity << "\n";
    }
};

class Student {
public:
    string name;
    int studentID;

    Student(string n, int id) : name(n), studentID(id) {}
};

class Library {
private:
    vector<Book> books;
    vector<Student> students;
    int booksLent;
    int booksReturned;

public:
    Library() : booksLent(0), booksReturned(0) {}

    void addBook(const Book& book) {
        books.push_back(book);
        cout << "Book added successfully.\n";
    }

    void addBooks() {
        string title, author;
        int id, amount;
        cout << "Enter book details:\n";
        cout << "Enter book title: ";
        cin.ignore();
        getline(cin, title);
        cout << "Enter author: ";
        getline(cin, author);
        cout << "Enter book ID: ";
        cin >> id;
        cout << "Enter quantity: ";
        cin >> amount;

        Book newBook(title, author, id, amount);
        addBook(newBook);
    }

    void addStudent(const Student& student) {
        students.push_back(student);
        cout << "Student added successfully.\n";
    }

    void displayBooks() {
        if (books.empty()) {
            cout << "No books available in the library.\n";
        } else {
            cout << setw(5) << "ID" << setw(30) << "Title" << setw(20) << "Author" << setw(10) << "Quantity" << setw(15) << "Available\n";
            cout << "------------------------------------------------------------\n";
            for (const auto& book : books) {
                cout << setw(5) << book.id << setw(30) << book.title << setw(20) << book.author << setw(10)
                     << book.quantity << setw(15) << (book.isAvailable() ? "Yes" : "No") << endl;
            }
        }
    }

    void displayStudents() {
        if (students.empty()) {
            cout << "No students registered in the library.\n";
        } else {
            cout << setw(10) << "Student ID" << setw(30) << "Name\n";
            cout << "--------------------------------\n";
            for (const auto& student : students) {
                cout << setw(10) << student.studentID << setw(30) << student.name << endl;
            }
        }
    }

    bool searchBook(int id) {
        for (const auto& book : books) {
            if (book.id == id) {
                cout << "Book found:\n";
                cout << "Title: " << book.title << "\nAuthor: " << book.author << "\n";
                return true;
            }
        }
        cout << "Book not found.\n";
        return false;
    }

    void lendBook(int bookID, int studentID) {
        auto bookIt = find_if(books.begin(), books.end(), [bookID](const Book& book) { return book.id == bookID; });
        auto studentIt = find_if(students.begin(), students.end(), [studentID](const Student& student) {
            return student.studentID == studentID;
        });

        if (bookIt != books.end() && studentIt != students.end()) {
            if (bookIt->isAvailable()) {
                bookIt->lendBook();
                cout << "Book successfully lent to " << studentIt->name << ".\n";
                booksLent++;
            } else {
                cout << "Book is not available for lending.\n";
            }
        } else {
            cout << "Book or student not found.\n";
        }
    }

    void returnBook(int bookID) {
        auto bookIt = find_if(books.begin(), books.end(), [bookID](const Book& book) { return book.id == bookID; });

        if (bookIt != books.end()) {
            bookIt->returnBook();
            booksReturned++;
        } else {
            cout << "Book not found.\n";
        }
    }

    void monthlyReport() const {
        cout << "\nMonthly Report\n";
        cout << "Books Lent: " << booksLent << "\n";
        cout << "Books Returned: " << booksReturned << "\n";
        cout << "Remaining Books: " << (books.size() > 0 ? books.back().quantity : 0) << "\n";
    }
};

int main() {
    Library library;

    // Adding some initial students
    library.addStudent(Student("Alice", 101));
    library.addStudent(Student("Bob", 102));

    while (true) {
        cout << "\nLibrary Management System\n";
        cout << "1. Add Book\n";
        cout << "2. Add Student\n";
        cout << "3. Display Books\n";
        cout << "4. Display Students\n";
        cout << "5. Search Book\n";
        cout << "6. Lend Book\n";
        cout << "7. Return Book\n";
        cout << "8. Monthly Report\n";
        cout << "9. Exit\n";
        cout << "Enter your choice: ";

        int choice;
        cin >> choice;

        switch (choice) {
            case 1:
                library.addBooks();
                break;
            case 2: {
                string name;
                int studentID;
                cout << "Enter student name: ";
                cin.ignore();
                getline(cin, name);
                cout << "Enter student ID: ";
                cin >> studentID;

                Student newStudent(name, studentID);
                library.addStudent(newStudent);
                break;
            }
            case 3:
                library.displayBooks();
                break;
            case 4:
                library.displayStudents();
                break;
            case 5: {
                int id;
                cout << "Enter book ID to search: ";
                cin >> id;
                library.searchBook(id);
                break;
            }
            case 6: {
                int bookID, studentID;
                cout << "Enter book ID to lend: ";
                cin >> bookID;
                cout << "Enter student ID: ";
                cin >> studentID;
                library.lendBook(bookID, studentID);
                break;
            }
             case 7: {
                int bookID;
                cout << "Enter book ID to return: ";
                cin >> bookID;
                library.returnBook(bookID);
                break;
            }
            case 8:
                library.monthlyReport();
                break;
            case 9:
                cout << "Exiting the program.\n";
                return 0;
            default:
                cout << "Invalid choice. Please try again.\n";
        }
    }

    return 0;
}

