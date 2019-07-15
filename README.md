# SFINAE
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -  - - 
    by: Ksenia Milkand & Cxx
```cpp
template <class class_i, class func_i, class = void>
struct runnable : std::false_type {

};

template <class class_i, class func_i, class... args>
struct runnable <class_i, func_i(args...), std::void_t <typename std::is_same <func_i, decltype(std::declval <class_i>().run(std::declval <args>()...))>::type>> : std::true_type {

};

struct R {    static void run (int) { } };
struct Q {           void run (int, int) { } };
struct T {           void run (int, int, int) { } };

int main() {
    if constexpr (runnable <R, void(int)>::value) {
        puts("R");
    }

    if constexpr (runnable <Q, void(int, int)>::value) {
        puts("Q");
    }

    if constexpr (runnable <T, void(int, int, int)>::value) {
        puts("T");
    }
return 0;
}
```
