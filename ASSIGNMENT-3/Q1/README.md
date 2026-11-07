#include <iostream>
#include <stdexcept>
#include <string>
 
class Book {
    std::string title_;
    int copies_;
    bool complete_;
    static int checked(int n) {
        if (n < 0) throw std::invalid_argument("copies cannot be negative");
        return n;
    }
public:
    Book() : Book("", 0, false) {}
    explicit Book(const std::string& title) : Book(title, 0, false) {}
    Book(const std::string& title, int copies) : Book(title, copies, true) {}
private:
    Book(const std::string& title, int copies, bool complete)
        : title_(title), copies_(checked(copies)), complete_(complete) {}
public:
    void print() const {
        std::cout << "title='" << title_ << "', copies=" << copies_
                  << ", complete=" << std::boolalpha << complete_ << '\n';
    }
};
 
int main() {
    Book empty;
    Book donated("The C++ Programming Language");
    Book full("Effective C++", 4);
    empty.print(); donated.print(); full.print();
    try { Book invalid("Bad record", -2); }
    catch (const std::invalid_argument& e) { std::cout << e.what() << '\n'; }
    // Book implicit = "Title"; // ill-formed: title constructor is explicit
}
