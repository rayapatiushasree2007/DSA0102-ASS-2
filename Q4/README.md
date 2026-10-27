#include <iostream>

class SavingsAccount {
    static constexpr int CAPACITY = 10;
    long long accountNo;
    double balance;
    double transactions[CAPACITY]{};
    int count = 0;
    void record(double amount) {
        if (count < CAPACITY) transactions[count++] = amount;
        else {
            for (int i = 1; i < CAPACITY; ++i) transactions[i - 1] = transactions[i];
            transactions[CAPACITY - 1] = amount;
        }
    }
public:
    SavingsAccount(long long number, double openingBalance)
        : accountNo(number), balance(openingBalance) {}
    bool deposit(double amount) {
        if (amount <= 0) return false;
        balance += amount; record(amount); return true;
    }
    bool withdraw(double amount);
    void miniStatement() const;
};

bool SavingsAccount::withdraw(double amount) {
    if (amount <= 0 || amount > balance) return false;
    balance -= amount; record(-amount); return true;
}
void SavingsAccount::miniStatement() const {
    std::cout << "Account " << accountNo << " balance " << balance << '\n';
    std::cout << "Recent transactions (positive=deposit, negative=withdrawal):\n";
    for (int i = 0; i < count; ++i) std::cout << transactions[i] << '\n';
}
int main() {
    SavingsAccount account(123456, 1000.0);
    account.deposit(100); account.withdraw(50);
    for (int i = 1; i <= 10; ++i) account.deposit(i * 10.0);
    account.miniStatement();
}
