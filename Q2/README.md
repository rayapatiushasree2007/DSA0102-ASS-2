#include <iostream>

int& highest(int marks[], int n) {
    int best = 0;
    for (int i = 1; i < n; ++i)
        if (marks[i] > marks[best]) best = i;
    return marks[best];
}

// Do not call this: it returns a dangling reference and causes undefined behavior.
// int& invalidReference() { int local = 7; return local; }

int highestIndex(const int marks[], int n) {
    int best = 0;
    for (int i = 1; i < n; ++i)
        if (marks[i] > marks[best]) best = i;
    return best;
}

int main() {
    int marks[] = {78, 91, 84, 88};
    int n = sizeof(marks) / sizeof(marks[0]);
    highest(marks, n) += 2;
    std::cout << "Moderated highest mark: " << highest(marks, n) << '\n';
    std::cout << "Highest mark index: " << highestIndex(marks, n) << '\n';
}
