# SFINAE
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -  - - 
    by: Ksenia Milkand & Cxx
```cpp
// example1

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
    if constexpr (runnable <R, void(int)>::value          ) { puts("R"); }

    if constexpr (runnable <Q, void(int, int)>::value     ) { puts("Q"); }

    if constexpr (runnable <T, void(int, int, int)>::value) { puts("T"); }
        
    // R  // Q  // T
    return 0;
}

// ~example1
```

```cpp
// example2

struct R {    static void run () { puts("R"); } }; //    runnable
struct Q {           void run () { puts("Q"); } }; // no runnable
struct T {           void run_() { puts("T"); } }; // no runnable

//requieres static

template <class type>
decltype(type::run, void()) run(type&& object) {
    object.run();
}

void run(...) {
}


int main() {
    run(R{});
    
    run(Q{});
    
    run(T{});
    
    // R
    return 0;
}

// ~example2
```

```cpp
// example3

template <class class_i, class = void>
struct runnable : std::false_type {};

// requieres static

template <class class_i>
struct runnable <class_i, std::void_t <decltype(class_i::run)>> : std::true_type {
};

struct R {    static void run () { } }; //    runnable
struct Q {           void run () { } }; // no runnable
struct T {           void run_() { } }; // no runnable

template <class type>
decltype(auto) run(type&& object) {
    return object.run();
}

int main() {
    if constexpr (runnable <Q>::value) { putchar('a'); }

    if constexpr (runnable <R>::value) { putchar('b'); }

    if constexpr (runnable <T>::value) { putchar('c'); }

    // b
    return 0;
}

// ~example3
```
