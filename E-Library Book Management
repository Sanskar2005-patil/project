#include <iostream>
#include <string>
#include <stack>

// Book node for linked list
struct Book {
    std::string title;
    std::string author;
    bool borrowed;  // false = available, true = borrowed
    Book* next;
};

// Action struct for undo stack
struct Action {
    enum ActionType { BORROW, RETURN } type;
    Book* book;
};

class ELibrary {
private:
    Book* head;
    std::stack<Action> actionStack;

public:
    ELibrary() : head(nullptr) {}

    ~ELibrary() {
        while (head) {
            Book* temp = head;
            head = head->next;
            delete temp;
        }
    }

    void addBook(const std::string& title, const std::string& author) {
        Book* newBook = new Book{title, author, false, head};
        head = newBook;
    }

    Book* findBookByTitle(const std::string& title) {
        Book* curr = head;
        while (curr) {
            if (curr->title == title) return curr;
            curr = curr->next;
        }
        return nullptr;
    }

    void borrowBook(const std::string& title) {
        Book* book = findBookByTitle(title);
        if (!book) {
            std::cout << "Book not found.\n";
            return;
        }
        if (book->borrowed) {
            std::cout << "Book already borrowed.\n";
            return;
        }
        book->borrowed = true;
        actionStack.push(Action{Action::BORROW, book});
        std::cout << "Borrowed \"" << title << "\"\n";
    }

    void returnBook(const std::string& title) {
        Book* book = findBookByTitle(title);
        if (!book) {
            std::cout << "Book not found.\n";
            return;
        }
        if (!book->borrowed) {
            std::cout << "Book was not borrowed.\n";
            return;
        }
        book->borrowed = false;
        actionStack.push(Action{Action::RETURN, book});
        std::cout << "Returned \"" << title << "\"\n";
    }

    void undo() {
        if (actionStack.empty()) {
            std::cout << "Nothing to undo.\n";
            return;
        }
        Action last = actionStack.top();
        actionStack.pop();

        if (last.type == Action::BORROW) {
            last.book->borrowed = false;
            std::cout << "Undo borrow: \"" << last.book->title << "\" is now available.\n";
        } else {
            last.book->borrowed = true;
            std::cout << "Undo return: \"" << last.book->title << "\" is now borrowed.\n";
        }
    }

    void searchByAuthor(const std::string& author) {
        Book* curr = head;
        bool found = false;
        while (curr) {
            if (curr->author == author) {
                std::cout << curr->title << " by " << curr->author 
                          << (curr->borrowed ? " [Borrowed]" : " [Available]") << "\n";
                found = true;
            }
            curr = curr->next;
        }
        if (!found) std::cout << "No books found by " << author << "\n";
    }

    void listInventory() {
        Book* curr = head;
        if (!curr) {
            std::cout << "Inventory is empty.\n";
            return;
        }
        while (curr) {
            std::cout << curr->title << " by " << curr->author 
                      << (curr->borrowed ? " [Borrowed]" : " [Available]") << "\n";
            curr = curr->next;
        }
    }
};

int main() {
    ELibrary lib;

    lib.addBook("The Great Gatsby", "F. Scott Fitzgerald");
    lib.addBook("1984", "George Orwell");
    lib.addBook("To Kill a Mockingbird", "Harper Lee");

    std::cout << "Current Inventory:\n";
    lib.listInventory();

    lib.borrowBook("1984");
    lib.borrowBook("The Great Gatsby");

    std::cout << "\nInventory after borrowing:\n";
    lib.listInventory();

    lib.undo();  // Undo last borrow (The Great Gatsby)

    std::cout << "\nInventory after undo:\n";
    lib.listInventory();

    std::cout << "\nSearch books by author 'George Orwell':\n";
    lib.searchByAuthor("George Orwell");

    lib.returnBook("1984");

    lib.undo();  // Undo return

    return 0;
}
