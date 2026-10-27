#include <chrono>
#include <iostream>

class Time {
    int h, m;
public:
    Time(int hh, int mm) : h(hh), m(mm) {}
    friend int minutesBetween(const Time&, const Time&);
    friend bool earlier(const Time&, const Time&);
};
int minutesBetween(const Time& a, const Time& b) {
    int x = a.h * 60 + a.m, y = b.h * 60 + b.m;
    return x > y ? x - y : y - x;
}
bool earlier(const Time& a, const Time& b) {
    return a.h * 60 + a.m < b.h * 60 + b.m;
}

#if defined(__GNUC__)
__attribute__((noinline))
#endif
int differenceNoInline(int a, int b) { return a > b ? a - b : b - a; }
inline int differenceInline(int a, int b) { return a > b ? a - b : b - a; }
volatile long long sink = 0;
template<class F> long long bench(F f) {
    const int calls = 10000000;
    auto start = std::chrono::steady_clock::now();
    long long sum = 0;
    for (int i = 0; i < calls; ++i) sum += f(i % 1000, (i * 7) % 1000);
    auto end = std::chrono::steady_clock::now(); sink = sum;
    return std::chrono::duration_cast<std::chrono::microseconds>(end - start).count();
}
int main() {
    Time departure(9,45), arrival(13,10), same1(12,0), same2(12,0), beforeNoon(11,59), afterNoon(12,1);
    std::cout << minutesBetween(departure,arrival) << ' ' << std::boolalpha
              << earlier(arrival,departure) << '\n';
    std::cout << minutesBetween(same1,same2) << ' ' << earlier(same1,same2) << '\n';
    std::cout << minutesBetween(beforeNoon,afterNoon) << ' ' << earlier(beforeNoon,afterNoon) << '\n';
    std::cout << "noinline_us=" << bench(differenceNoInline) << '\n';
    std::cout << "inline_us=" << bench(differenceInline) << '\n';
}
