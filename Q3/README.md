#include <iostream>

struct Parcel { double kg; bool express = false; bool insured = false; };
double charge(int grams, bool express = false, bool insured = false) {
    return grams * 0.02 + (express ? 50 : 0) + (insured ? 20 : 0);
}
double charge(double kg, bool express = false, bool insured = false) {
    return kg * 20.0 + (express ? 50 : 0) + (insured ? 20 : 0);
}
double charge(const Parcel& p) { return charge(p.kg, p.express, p.insured); }

int main() {
    std::cout << charge(2) << '\n';
    std::cout << charge(2.0f) << '\n';
    std::cout << charge(2.0) << '\n';
    std::cout << charge(500, true) << '\n';
    std::cout << charge(1.5, false, true) << '\n';
    std::cout << charge(Parcel{1.2, true, false}) << '\n';
}
