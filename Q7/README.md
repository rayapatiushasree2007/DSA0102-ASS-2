#include <chrono>
#include <cstdlib>
#include <iostream>
#include <vector>

class ShiftHistory {
    std::vector<int> data; int capacity;
public:
    explicit ShiftHistory(int k) : capacity(k) {}
    void add(int value) {
        if (static_cast<int>(data.size()) < capacity) data.push_back(value);
        else {
            for (int i = 1; i < capacity; ++i) data[i - 1] = data[i];
            data[capacity - 1] = value;
        }
    }
    int at(int i) const { return data[i]; }
    int size() const { return static_cast<int>(data.size()); }
};
class CircularHistory {
    std::vector<int> data; int capacity, start = 0, count = 0;
public:
    explicit CircularHistory(int k) : data(k), capacity(k) {}
    void add(int value) {
        if (count < capacity) { data[(start + count) % capacity] = value; ++count; }
        else { data[start] = value; start = (start + 1) % capacity; }
    }
    int at(int i) const { return data[(start + i) % capacity]; }
    int size() const { return count; }
};
template<class H> long long run(H& h, int operations, long long& checksum) {
    auto begin = std::chrono::steady_clock::now();
    for (int i = 0; i < operations; ++i) h.add(i);
    auto end = std::chrono::steady_clock::now();
    checksum = 0;
    for (int i = 0; i < h.size(); ++i) checksum += h.at(i);
    return std::chrono::duration_cast<std::chrono::microseconds>(end - begin).count();
}
int main(int argc, char** argv) {
    int k = argc > 1 ? std::atoi(argv[1]) : 500;
    int operations = argc > 2 ? std::atoi(argv[2]) : 1000000;
    if (k <= 0 || operations < k) { std::cerr << "Require capacity > 0 and operations >= capacity\n"; return 1; }
    ShiftHistory shifted(k); CircularHistory circular(k);
    long long shiftChecksum = 0, circularChecksum = 0;
    long long shiftTime = run(shifted, operations, shiftChecksum);
    long long circularTime = run(circular, operations, circularChecksum);
    std::cout << "shift_us=" << shiftTime << " checksum=" << shiftChecksum << '\n';
    std::cout << "circular_us=" << circularTime << " checksum=" << circularChecksum << '\n';
    if (shifted.size() != circular.size()) return 2;
    for (int i = 0; i < k; ++i) if (shifted.at(i) != circular.at(i)) return 3;
    std::cout << "retained histories match\n";
}

