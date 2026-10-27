#include <iostream>

void change(int a, int& b, int* c) { a += 10; b += 10; *c += 10; }
int& bigger(int& p, int& q) { return p > q ? p : q; }
void show(int x, int y = 5, int z = 10) { std::cout << x + y + z << ' '; }

void byValue(long long fee, int percent) { fee -= fee * percent / 100; }
void byReference(long long& fee, int percent) { fee -= fee * percent / 100; }
void byPointer(long long* fee, int percent) {
    if (fee) *fee -= *fee * percent / 100;
}

int main() {
    int a = 1, b = 2, c = 3;
    change(a, b, &c);
    std::cout << a << ' ' << b << ' ' << c << '\n';
    bigger(a, c) = 100;
    std::cout << a << ' ' << c << '\n';
    show(1); show(1, 2); show(1, 2, 3); std::cout << '\n';

    long long value = 45000;
    byValue(value, 10);
    std::cout << "after call by value : " << value << '\n';
    byReference(value, 10);
    std::cout << "after call by reference: " << value << '\n';
    byPointer(&value, 10);
    std::cout << "after call by pointer: " << value << '\n';
}
