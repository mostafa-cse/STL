Below is a categorized list of the most commonly used “built-in” (i.e., standard-library) functions in C++. For each category, you’ll find:

1. The header you need to include.
2. A brief description of each function.
3. A minimal code example showing how to use it.

> **Note:** It’s impractical to list literally *every* function from the entire C++ Standard Library in one answer. Instead, this guide highlights the core, commonly used functions in each category. If you need something very specific (e.g., low-level memory intrinsics or compiler intrinsics like `__builtin_popcount`), let me know, and I can expand further.

---

## 1. Input/Output (I/O) Functions

Most I/O in modern C++ is done via the `<iostream>` header.

### 1.1 `std::cout` / `std::cin` / `std::cerr` / `std::clog`

* **Header:**

  ```cpp
  #include <iostream>
  ```

* **Purpose:**

  * `std::cout` – write formatted output to standard output (console).
  * `std::cin` – read formatted input from standard input (console).
  * `std::cerr` – write error messages (unbuffered) to standard error.
  * `std::clog` – write logging/info (buffered) to standard error.

* **Examples:**

  ```cpp
  #include <iostream>
  using namespace std;

  int main() {
      int age;
      cout << "Enter your age: ";
      cin >> age;                       // reads an integer from keyboard
      cout << "You are " << age << " years old.\n";

      if (age < 0) {
          cerr << "Error: Negative age!\n";  // unbuffered error
      } else {
          clog << "Info: Age input accepted.\n"; // buffered log
      }
      return 0;
  }
  ```

### 1.2 `std::getline`

* **Header:**

  ```cpp
  #include <iostream>
  #include <string>
  ```

* **Purpose:**
  Read an entire line (possibly containing spaces) into a `std::string`.

* **Example:**

  ```cpp
  #include <iostream>
  #include <string>
  using namespace std;

  int main() {
      string line;
      cout << "Enter a sentence: ";
      getline(cin, line);      // reads until newline (can include spaces)
      cout << "You typed: \"" << line << "\"\n";
      return 0;
  }
  ```

### 1.3 File-Based I/O: `std::ifstream` / `std::ofstream` / `std::fstream`

* **Header:**

  ```cpp
  #include <fstream>
  ```

* **Purpose:**

  * `std::ifstream` – input file stream (reading from files).
  * `std::ofstream` – output file stream (writing to files).
  * `std::fstream` – combined read/write file stream.

* **Example (writing & reading a file):**

  ```cpp
  #include <fstream>
  #include <iostream>
  #include <string>
  using namespace std;

  int main() {
      // Write to file
      ofstream fout("example.txt");
      if (!fout) {
          cerr << "Error: Could not open file for writing.\n";
          return 1;
      }
      fout << "Line 1: Hello, file!\n";
      fout << "Line 2: 12345\n";
      fout.close();

      // Read from file
      ifstream fin("example.txt");
      if (!fin) {
          cerr << "Error: Could not open file for reading.\n";
          return 1;
      }
      string line;
      while (getline(fin, line)) {
          cout << "Read: " << line << "\n";
      }
      fin.close();
      return 0;
  }
  ```

---

## 2. Math Functions (`<cmath>`)

All functions in `<cmath>` reside in namespace `std`. These cover common mathematical operations.

* **Header:**

  ```cpp
  #include <cmath>
  ```

Below are the most frequently used ones:

| Function                                       | Description                                    | Signature (simplified)                                        |
| ---------------------------------------------- | ---------------------------------------------- | ------------------------------------------------------------- |
| `std::abs(x)`                                  | Absolute value                                 | `int abs(int); double abs(double);` etc.                      |
| `std::ceil(x)`                                 | Smallest integer ≥ x                           | `double ceil(double);`                                        |
| `std::floor(x)`                                | Largest integer ≤ x                            | `double floor(double);`                                       |
| `std::round(x)`                                | Nearest integer (ties to nearest even integer) | `double round(double);`                                       |
| `std::sqrt(x)`                                 | Square root                                    | `double sqrt(double);`                                        |
| `std::pow(x, y)`                               | x raised to the power y                        | `double pow(double, double);`                                 |
| `std::exp(x)`                                  | Exponential (eˣ)                               | `double exp(double);`                                         |
| `std::log(x)`                                  | Natural logarithm (ln x)                       | `double log(double);`                                         |
| `std::log10(x)`                                | Base-10 logarithm                              | `double log10(double);`                                       |
| `std::sin(x)`, `std::cos(x)`, `std::tan(x)`    | Trigonometric sine/cosine/tangent              | `double sin(double);` etc.                                    |
| `std::asin(x)`, `std::acos(x)`, `std::atan(x)` | Inverse trigonometric functions                | `double asin(double);` etc.                                   |
| `std::sinh(x)`, `std::cosh(x)`, `std::tanh(x)` | Hyperbolic sine/cosine/tangent                 | `double sinh(double);` etc.                                   |
| `std::pow10(x)` (C++11: `std::powl(10, x)`)    | 10ˣ                                            | `double pow10(double);` (deprecated in C++11, use `std::pow`) |

> **Example:**
>
> ```cpp
> #include <iostream>
> #include <cmath>
> using namespace std;
>
> int main() {
>     double x = -2.7, y = 3.14;
>
>     cout << "abs(" << x << ") = " << abs(x) << "\n";         // 2.7
>     cout << "ceil(" << x << ") = " << ceil(x) << "\n";       // -2
>     cout << "floor(" << x << ") = " << floor(x) << "\n";     // -3
>     cout << "round(" << x << ") = " << round(x) << "\n";     // -3
>     cout << "sqrt(9) = " << sqrt(9) << "\n";                 // 3
>     cout << "pow(2, 5) = " << pow(2, 5) << "\n";              // 32
>     cout << "exp(1) = " << exp(1) << "\n";                   // ≈2.71828
>     cout << "log(10) = " << log(10) << "\n";                 // ≈2.30258
>     cout << "sin(" << y << ") = " << sin(y) << "\n";         // ≈0.00159265
>     cout << "cos(" << y << ") = " << cos(y) << "\n";         // ≈−0.9999987
>     return 0;
> }
> ```

---

## 3. Character Classification & Transformation (`<cctype>`)

These functions test or transform single characters (usually `unsigned char` or `int` values representing `char`).

* **Header:**

  ```cpp
  #include <cctype>
  ```

| Function          | Description                                                                | Signature (simplified) |
| ----------------- | -------------------------------------------------------------------------- | ---------------------- |
| `std::isalpha(c)` | `true` if `c` is an alphabetic character (A–Z or a–z).                     | `int isalpha(int);`    |
| `std::isdigit(c)` | `true` if `c` is a decimal digit (0–9).                                    | `int isdigit(int);`    |
| `std::isspace(c)` | `true` if `c` is a whitespace character (space, tab, newline, etc.).       | `int isspace(int);`    |
| `std::isalnum(c)` | `true` if `c` is alphanumeric (letter or digit).                           | `int isalnum(int);`    |
| `std::isupper(c)` | `true` if `c` is an uppercase letter.                                      | `int isupper(int);`    |
| `std::islower(c)` | `true` if `c` is a lowercase letter.                                       | `int islower(int);`    |
| `std::toupper(c)` | Convert `c` to uppercase (if alphabetic), otherwise returns `c` unchanged. | `int toupper(int);`    |
| `std::tolower(c)` | Convert `c` to lowercase (if alphabetic), otherwise returns `c` unchanged. | `int tolower(int);`    |

> **Example:**
>
> ```cpp
> #include <iostream>
> #include <cctype>
> using namespace std;
>
> int main() {
>     char s[] = "Hello, World! 123\n";
>     cout << "Original: " << s;
>
>     // Convert letters to uppercase
>     for (int i = 0; s[i] != '\0'; ++i) {
>         if (isalpha(s[i])) {
>             s[i] = toupper(s[i]);
>         }
>     }
>     cout << "Uppercase: " << s;
>
>     // Count digits and spaces
>     int digitCount = 0, spaceCount = 0;
>     for (int i = 0; s[i] != '\0'; ++i) {
>         if (isdigit(s[i]))      ++digitCount;
>         if (isspace(s[i]))      ++spaceCount;
>     }
>     cout << "Digits: " << digitCount << ", Spaces (incl. newline): " << spaceCount << "\n";
>     return 0;
> }
> ```

---

## 4. C-String (Null-Terminated) Manipulation (`<cstring>`)

These functions work on “C-style” null-terminated character arrays (`char*`).

* **Header:**

  ```cpp
  #include <cstring>
  ```

| Function                        | Description                                                                                                                                                                | Signature (simplified)                                       |
| ------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------ |
| `std::strlen(s)`                | Returns the length (number of characters) of C-string `s` (not counting `'\0'`).                                                                                           | `size_t strlen(const char*);`                                |
| `std::strcpy(dst, src)`         | Copies the null-terminated string from `src` into `dst` (including the `'\0'`).                                                                                            | `char* strcpy(char* dst, const char* src);`                  |
| `std::strncpy(dst, src, n)`     | Copies at most `n` characters of `src` into `dst`.  If `src` is shorter than `n`, the remainder of `dst` is padded with `'\0'`. If `src` is longer, no `'\0'` is appended. | `char* strncpy(char* dst, const char* src, size_t n);`       |
| `std::strcat(dst, src)`         | Appends (concatenates) `src` onto the end of `dst`.  `dst` must be large enough to hold the result.                                                                        | `char* strcat(char* dst, const char* src);`                  |
| `std::strncat(dst, src, n)`     | Appends at most `n` characters of `src` to `dst`, then adds a single `'\0'`.                                                                                               | `char* strncat(char* dst, const char* src, size_t n);`       |
| `std::strcmp(s1, s2)`           | Compares two C-strings lexicographically. Returns <0 if `s1<s2`, 0 if `s1==s2`, >0 if `s1>s2`.                                                                             | `int strcmp(const char* s1, const char* s2);`                |
| `std::strncmp(s1, s2, n)`       | Compares at most `n` characters of `s1` and `s2`.                                                                                                                          | `int strncmp(const char* s1, const char* s2, size_t n);`     |
| `std::strchr(s, c)`             | Returns pointer to first occurrence of character `c` in `s`, or `nullptr` if not found.                                                                                    | `char* strchr(char* s, int c);` (and `const char*` overload) |
| `std::strrchr(s, c)`            | Returns pointer to last occurrence of character `c` in `s`, or `nullptr` if not found.                                                                                     | `char* strrchr(char* s, int c);`                             |
| `std::strstr(haystack, needle)` | Returns pointer to first occurrence of substring `needle` in `haystack`, or `nullptr` if not found.                                                                        | `char* strstr(char* haystack, const char* needle);`          |
| `std::memset(ptr, val, n)`      | Sets the first `n` bytes of memory at `ptr` to the byte value `val`. Useful for quickly zeroing or filling raw buffers.                                                    | `void* memset(void* ptr, int val, size_t n);`                |
| `std::memcpy(dest, src, n)`     | Copies exactly `n` bytes from `src` to `dest`. Does not handle overlap. Use `std::memmove` if overlapping.                                                                 | `void* memcpy(void* dest, const void* src, size_t n);`       |
| `std::memmove(dest, src, n)`    | Copies `n` bytes from `src` to `dest`, correctly handling overlapping memory regions.                                                                                      | `void* memmove(void* dest, const void* src, size_t n);`      |

> **Example:**
>
> ```cpp
> #include <iostream>
> #include <cstring>
> using namespace std;
>
> int main() {
>     char s1[50] = "Hello";
>     char s2[]     = ", World!";
>
>     // strlen
>     cout << "Length of \"" << s1 << "\": " << strlen(s1) << "\n";
>
>     // strcat
>     strcat(s1, s2);   // s1 now = "Hello, World!"
>     cout << "After strcat: " << s1 << "\n";
>
>     // strcpy
>     char copyBuf[50];
>     strcpy(copyBuf, s1);
>     cout << "Copied string: " << copyBuf << "\n";
>
>     // strstr
>     char* p = strstr(copyBuf, "World");
>     if (p) {
>         cout << "\"World\" found at index: " << (p - copyBuf) << "\n";
>     }
>
>     // memset
>     char buffer[10];
>     memset(buffer, 'A', sizeof(buffer));  // fills buffer with 'A'
>     cout << "memset buffer: ";
>     for (char c : buffer) cout << c;
>     cout << "\n";
>
>     // memcpy (copy raw bytes)
>     int arr1[5] = {1, 2, 3, 4, 5};
>     int arr2[5];
>     memcpy(arr2, arr1, sizeof(arr1));      // copy all 5 integers
>     cout << "arr2 after memcpy: ";
>     for (int i = 0; i < 5; ++i) cout << arr2[i] << " ";
>     cout << "\n";
>
>     return 0;
> }
> ```

---

## 5. General Utility & Conversion (`<cstdlib>`, `<utility>`)

### 5.1 Memory Management: `std::malloc`, `std::free`, `std::calloc`, `std::realloc`

* **Header:**

  ```cpp
  #include <cstdlib>
  ```
* **Purpose:**
  Low-level C-style dynamic allocation (rarely used in idiomatic C++; normally prefer `new`/`delete` or containers).

| Function                                      | Description                                                                                                             | Signature (simplified)          |
| --------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------- | ------------------------------- |
| `void* std::malloc(size_t n)`                 | Allocate `n` bytes of uninitialized memory. Returns pointer to start or `nullptr` on failure.                           | `void* malloc(size_t);`         |
| `void std::free(void* p)`                     | Free memory previously allocated by `malloc`/`calloc`/`realloc`. `p` may be `nullptr`.                                  | `void free(void*);`             |
| `void* std::calloc(size_t n, size_t size)`    | Allocate memory for an array of `n` elements, each of size `size`, and initialize all bytes to zero.                    | `void* calloc(size_t, size_t);` |
| `void* std::realloc(void* p, size_t newSize)` | Resize the memory block pointed to by `p` to `newSize` bytes. Contents are preserved up to the lesser of old/new sizes. | `void* realloc(void*, size_t);` |

> **Example:**
>
> ```cpp
> #include <iostream>
> #include <cstdlib>
> using namespace std;
>
> int main() {
>     int n = 5;
>     // Allocate array of 5 ints
>     int* arr = static_cast<int*>(malloc(n * sizeof(int)));
>     if (!arr) {
>         cerr << "malloc failed\n";
>         return 1;
>     }
>     for (int i = 0; i < n; ++i) {
>         arr[i] = (i + 1) * 10;
>     }
>
>     cout << "Original array: ";
>     for (int i = 0; i < n; ++i) cout << arr[i] << " ";
>     cout << "\n";
>
>     // Grow array to 10 ints
>     arr = static_cast<int*>(realloc(arr, 10 * sizeof(int)));
>     if (!arr) {
>         cerr << "realloc failed\n";
>         return 1;
>     }
>     for (int i = n; i < 10; ++i) {
>         arr[i] = (i + 1) * 10;
>     }
>
>     cout << "After realloc (size 10): ";
>     for (int i = 0; i < 10; ++i) cout << arr[i] << " ";
>     cout << "\n";
>
>     free(arr);  // Always free what you malloc/calloc/realloc
>     return 0;
> }
> ```

### 5.2 Process Control & Conversions: `std::exit`, `std::atoi`, `std::atof`, etc.

* **Header:**

  ```cpp
  #include <cstdlib>
  ```
* **Purpose:**

  * `std::exit(code)` – terminate the program immediately with exit status `code`.
  * `std::atoi(str)` – convert C-string `str` to `int`. Returns `0` if no valid conversion.
  * `std::atof(str)` – convert C-string to `double`.
  * `std::strtol`, `std::strtod` – more robust string-to-number conversions with error checking.

> **Example:**
>
> ```cpp
> #include <iostream>
> #include <cstdlib>
> using namespace std;
>
> int main(int argc, char* argv[]) {
>     if (argc != 2) {
>         cerr << "Usage: " << argv[0] << " <number>\n";
>         return 1;
>     }
>     int val = atoi(argv[1]);
>     if (val == 0 && argv[1][0] != '0') {
>         cerr << "Error: argument is not a valid integer\n";
>         exit(EXIT_FAILURE);  // equivalent to return 1 from main
>     }
>     cout << "You entered integer: " << val << "\n";
>     return 0;  // exit with status 0 (success)
> }
> ```

### 5.3 `std::swap`

* **Header:**

  ```cpp
  #include <utility>   // (or <algorithm>)
  ```

* **Purpose:**
  Exchange the values of two objects of the same type. Overloaded for most built-in and standard types.

* **Example:**

  ```cpp
  #include <iostream>
  #include <utility>
  using namespace std;

  int main() {
      int a = 5, b = 10;
      cout << "Before swap: a=" << a << ", b=" << b << "\n";
      swap(a, b);
      cout << "After swap:  a=" << a << ", b=" << b << "\n";
      return 0;
  }
  ```

---

## 6. Algorithms (`<algorithm>`)

The `<algorithm>` header provides many generic (templated) functions that work on iterators (e.g., arrays, `std::vector`, `std::string`). Below are some of the most common ones.

* **Header:**

  ```cpp
  #include <algorithm>
  ```

| Function                                               | Description                                                                                                                                                    |
| ------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `std::sort(begin, end)`                                | Sorts the elements in the half-open range `[begin, end)` in ascending order (using `<` by default).                                                            |
| `std::reverse(begin, end)`                             | Reverses the order of elements in `[begin, end)`.                                                                                                              |
| `std::find(begin, end, value)`                         | Returns iterator to the first element equal to `value` in `[begin, end)`, or `end` if none found.                                                              |
| `std::count(begin, end, value)`                        | Counts how many elements equal `value` in `[begin, end)`.                                                                                                      |
| `std::accumulate(begin, end, init)` (from `<numeric>`) | Sums up elements in `[begin, end)`, starting from initial value `init`. Must `#include <numeric>`.                                                             |
| `std::binary_search(begin, end, value)`                | Returns `true` if `value` is found in sorted range `[begin, end)`, otherwise `false`.                                                                          |
| `std::lower_bound(begin, end, value)`                  | Returns the first iterator `it` in `[begin, end)` such that `*it >= value` (requires sorted range).                                                            |
| `std::upper_bound(begin, end, value)`                  | Returns the first iterator `it` in `[begin, end)` such that `*it > value` (requires sorted range).                                                             |
| `std::max_element(begin, end)`                         | Returns iterator to the largest element in `[begin, end)`.                                                                                                     |
| `std::min_element(begin, end)`                         | Returns iterator to the smallest element in `[begin, end)`.                                                                                                    |
| `std::remove(begin, end, value)`                       | “Removes” all elements equal to `value` by shifting non-matching elements forward; returns new logical end (must call container’s `erase` to actually remove). |

> **Example (vector + algorithms):**
>
> ```cpp
> #include <iostream>
> #include <vector>
> #include <algorithm>
> #include <numeric>   // for accumulate
> using namespace std;
> ```

> int main() {
> vector<int> v = { 3, 1, 4, 1, 5, 9, 2, 6, 5 };
> cout << "Original: ";
> for (int x : v) cout << x << " ";
> cout << "\n";

> ```
> // Sort
> sort(v.begin(), v.end());  // now {1,1,2,3,4,5,5,6,9}
> cout << "Sorted:   ";
> for (int x : v) cout << x << " ";
> cout << "\n";
> ```

> ```
> // Count how many 5's
> int count5 = count(v.begin(), v.end(), 5);
> cout << "Number of 5's: " << count5 << "\n";
> ```

> ```
> // Find first element >= 4
> auto it = lower_bound(v.begin(), v.end(), 4);
> if (it != v.end()) {
>     cout << "First element >= 4 is " << *it << " at index " << (it - v.begin()) << "\n";
> }
> ```

> ```
> // Sum of all elements
> int sum = accumulate(v.begin(), v.end(), 0);
> cout << "Sum of elements: " << sum << "\n";
> ```

> ```
> // Remove all 1's (logical removal)
> auto newEnd = remove(v.begin(), v.end(), 1);
> v.erase(newEnd, v.end());  // actually shrink the vector
> cout << "After removing 1's: ";
> for (int x : v) cout << x << " ";
> cout << "\n";
> ```

> ```
> return 0;
> ```
>
> }
>
> ```
> ```

---

## 7. Containers & Their Member Functions

Strictly speaking, container member functions (e.g., `std::vector::push_back`) are NOT “built-in free functions” but members of template classes. However, they are so ubiquitous that it’s worth listing the most common container types and their basic member functions.

> **Header (most used containers):**
>
> ```cpp
> #include <vector>
> #include <string>
> #include <list>
> #include <deque>
> #include <map>
> #include <unordered_map>
> #include <set>
> #include <queue>
> #include <stack>
> #include <array>
> ```
>
> All containers live in namespace `std`.

Below are a few illustrative member functions for `std::vector<T>` (the others follow similar patterns):

| Member Function                 | Description                                                     |
| ------------------------------- | --------------------------------------------------------------- |
| `v.size()`                      | Returns number of elements in the container (constant time).    |
| `v.empty()`                     | Returns `true` if `size() == 0`.                                |
| `v.push_back(elem)`             | Appends `elem` to the end (amortized constant time).            |
| `v.pop_back()`                  | Removes the last element (constant time).                       |
| `v.front()` / `v.back()`        | Returns a reference to the first/last element (constant time).  |
| `v[i]` / `v.at(i)`              | Element access by index; `at(i)` does range checking (throws).  |
| `v.clear()`                     | Removes all elements (linear time).                             |
| `v.insert(it, elem)`            | Inserts `elem` at iterator position `it` (linear time).         |
| `v.erase(it)`                   | Removes element at `it` (linear time; iterator invalidated).    |
| `v.begin()` / `v.end()`         | Iterator to first/one-past-last element, used by algorithms.    |
| `v.capacity()` / `v.reserve(n)` | Inspect or request internal capacity to avoid repeated realloc. |

> **Example (`std::vector` basics):**
>
> ```cpp
> #include <iostream>
> #include <vector>
> using namespace std;
> ```

> int main() {
> vector<string> names;
> names.push\_back("Alice");
> names.push\_back("Bob");
> names.push\_back("Charlie");

> ```
> cout << "Size: " << names.size() << "\n";      // 3
> cout << "First: " << names.front() << "\n";    // "Alice"
> cout << "Last: " << names.back() << "\n";      // "Charlie"
> ```

> ```
> // Iterate
> for (size_t i = 0; i < names.size(); ++i) {
>     cout << "names[" << i << "] = " << names[i] << "\n";
> }
> ```

> ```
> // Erase “Bob”
> for (auto it = names.begin(); it != names.end(); ++it) {
>     if (*it == "Bob") {
>         names.erase(it);
>         break;
>     }
> }
> cout << "After erasing Bob, size = " << names.size() << "\n"; // 2
> ```

> ```
> // Clear all
> names.clear();
> cout << "After clear, empty? " << boolalpha << names.empty() << "\n"; // true
> ```

> ```
> return 0;
> ```
>
> }
>
> ```
> ```

---

## 8. String Class (`<string>`) Free Functions

Even though most string operations are member functions (`std::string::substr`, `.find`, etc.), there are a few non-member (free) functions that accept or return `std::string`.

* **Header:**

  ```cpp
  #include <string>
  ```

| Function                                                                                                       | Description                                                                                            |
| -------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------ |
| `std::getline(cin, str)`                                                                                       | (already covered) reads a whole line into a `std::string`.                                             |
| `std::to_string(value)`                                                                                        | Convert an integer, float, double, etc., to an `std::string`.                                          |
| `std::stoi(str)`, `std::stol(str)`, `std::stoll(str)`<br>`std::stof(str)`, `std::stod(str)`, `std::stold(str)` | Convert `std::string` (or C-string) to integer/float/double/long double. Throws on invalid conversion. |

> **Example:**
>
> ```cpp
> #include <iostream>
> #include <string>
> using namespace std;
> ```

> int main() {
> string sNumber = "12345";
> int x = stoi(sNumber);       // convert "12345" → 12345
> double d = stod("3.14159");  // 3.14159
> cout << "x = " << x << ", d = " << d << "\n";

> ```
> int y = 42;
> string s = to_string(y);     // "42"
> cout << "String from int: " << s << "\n";
> ```

> ```
> return 0;
> ```
>
> }
>
> ```
> ```

---

## 9. Time & Date (`<ctime>`, `<chrono>`)

### 9.1 Traditional C API (`<ctime>`)

* **Header:**

  ```cpp
  #include <ctime>
  ```
* **Purpose:**

  * `std::time(…)` – get current calendar time (seconds since epoch).
  * `std::localtime(…)`, `std::gmtime(…)` – convert `time_t` to broken-down local/UTC time.
  * `std::strftime(buf, size, format, tm*)` – format a `struct tm` into a human-readable string.
  * `std::difftime(t2, t1)` – difference in seconds between two `time_t` values.

> **Example:**
>
> ```cpp
> #include <iostream>
> #include <ctime>
> using namespace std;
> ```

> int main() {
> // Get current time
> time\_t now = time(nullptr);              // seconds since Jan 1, 1970
> cout << "time\_t value: " << now << "\n";

> ```
> // Convert to local broken-down time
> tm* local_tm = localtime(&now);
> char buffer[80];
> strftime(buffer, sizeof(buffer), "%Y-%m-%d %H:%M:%S", local_tm);
> cout << "Local time: " << buffer << "\n";
> ```

> ```
> // Convert to UTC
> tm* utc_tm = gmtime(&now);
> strftime(buffer, sizeof(buffer), "%Y-%m-%d %H:%M:%S UTC", utc_tm);
> cout << "UTC time:   " << buffer << "\n";
> ```

> ```
> return 0;
> ```
>
> }
>
> ```
> ```

### 9.2 Modern C++ Chrono (`<chrono>`)

* **Header:**

  ```cpp
  #include <chrono>
  ```
* **Purpose:**
  Provides type-safe duration and clock types (`std::chrono::system_clock`, `steady_clock`, `high_resolution_clock`), plus utilities for measuring intervals.

> **Example (measuring elapsed time):**
>
> ```cpp
> #include <iostream>
> #include <chrono>
> using namespace std;
> using namespace std::chrono;
> ```

> int main() {
> auto start = high\_resolution\_clock::now();

> ```
> // Do some work
> long long sum = 0;
> for (int i = 0; i < 1000000; ++i) {
>     sum += i;
> }
> ```

> ```
> auto end = high_resolution_clock::now();
> auto duration = duration_cast<milliseconds>(end - start);
> ```

> ```
> cout << "Sum = " << sum << "\n";
> cout << "Elapsed time: " << duration.count() << " ms\n";
> return 0;
> ```
>
> }
>
> ```
> ```

---

## 10. Other Common Utility Functions

### 10.1 `<utility>`: `std::pair`, `std::make_pair`, `std::tie`, `std::exchange`

* **Header:**

  ```cpp
  #include <utility>
  ```
* **Purpose:**

  * `std::pair<T1, T2>` – simple struct holding two values of types `T1` and `T2`.
  * `std::make_pair(a, b)` – deduce types and return a `std::pair`.
  * `std::tie(...)` – unpack a tuple/pair into separate variables (with references).
  * `std::exchange(obj, newValue)` – assign `newValue` to `obj` and return the old value. Useful in move-semantics or resetting.

> **Example:**
>
> ```cpp
> #include <iostream>
> #include <utility>
> #include <tuple>
> using namespace std;
> ```

> int main() {
> // pair / make\_pair
> pair\<int, string> p = make\_pair(42, "Answer");
> cout << "First = " << p.first << ", Second = " << p.second << "\n";

> ```
> // tie (unpacking)
> int number;
> string text;
> tie(number, text) = p;   // unpack p into number and text
> cout << "Unpacked: " << number << ", " << text << "\n";
> ```

> ```
> // exchange
> int old = exchange(number, 100);  // sets number=100, returns old value (42)
> cout << "Old number = " << old << ", New number = " << number << "\n";
> ```

> ```
> return 0;
> ```
>
> }
>
> ```
> ```

---

## 11. Dynamic Memory (Preferred C++ Way): `new` / `delete` (operators, not functions)

Though not functions in the `<…>` sense, `new` and `delete` are built-in C++ operators for dynamic allocation.

* **Usage:**

  ```cpp
  int* p = new int(5);    // allocate one int, initialize to 5
  delete p;               // free it (must match exactly)

  double* arr = new double[10];  // allocate array of 10 doubles
  delete[] arr;                  // free array (note [] here)
  ```

> **Example:**
>
> ```cpp
> #include <iostream>
> using namespace std;
> ```

> int main() {
> // Single object
> int\* ptr = new int(123);
> cout << "\*ptr = " << \*ptr << "\n";
> delete ptr;

> ```
> // Array of objects
> int n = 5;
> int* arr = new int[n];
> for (int i = 0; i < n; ++i) {
>     arr[i] = (i + 1) * 10;
> }
> cout << "arr: ";
> for (int i = 0; i < n; ++i) {
>     cout << arr[i] << " ";
> }
> cout << "\n";
> delete[] arr;  // always use delete[] for arrays
> return 0;
> ```
>
> }
>
> ```
> ```

---

## 12. Summary of Headers & Key Functions

Below is a concise reference table (not exhaustive) of headers you’ll most often need, along with a few key functions from each:

| Header        | Key Functions/Classes                                                                                                                                                     |
| ------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `<iostream>`  | `std::cout`, `std::cin`, `std::cerr`, `std::clog`, `std::endl`, `std::getline`                                                                                            |
| `<fstream>`   | `std::ifstream`, `std::ofstream`, `std::fstream`                                                                                                                          |
| `<string>`    | `std::string`, `std::getline`, `std::to_string`, `std::stoi` / `std::stod`                                                                                                |
| `<cstring>`   | `std::strlen`, `std::strcpy`, `std::strncpy`, `std::strcat`, `std::strcmp`, `std::strstr`, `std::memset`, `std::memcpy`, `std::memmove`                                   |
| `<cctype>`    | `std::isalpha`, `std::isdigit`, `std::isalnum`, `std::isspace`, `std::toupper`, `std::tolower`                                                                            |
| `<cmath>`     | `std::abs`, `std::ceil`, `std::floor`, `std::round`, `std::sqrt`, `std::pow`, `std::exp`, `std::log`, `std::log10`, `std::sin`, `std::cos`, `std::tan`, `std::sinh` etc.  |
| `<cstdlib>`   | `std::malloc`, `std::calloc`, `std::realloc`, `std::free`, `std::exit`, `std::atoi`, `std::atof`, `std::strtol` / `std::strtod`, `std::rand`, `std::srand`                |
| `<algorithm>` | `std::sort`, `std::reverse`, `std::find`, `std::count`, `std::accumulate` (from `<numeric>`), `std::binary_search`, `std::lower_bound`, `std::upper_bound`, `std::remove` |
| `<utility>`   | `std::pair`, `std::make_pair`, `std::swap`, `std::tie`, `std::exchange`                                                                                                   |
| `<vector>`    | Container methods: `.push_back()`, `.pop_back()`, `.size()`, `.empty()`, `.clear()`, `.insert()`, `.erase()`, `.begin()`, `.end()`, `.capacity()`, `.reserve()`           |
| `<map>`       | `std::map<Key,Value>`, methods: `.insert()`, `.find()`, `.erase()`, `.operator[]`, `.begin()`, `.end()`                                                                   |
| `<set>`       | `std::set<Value>`, methods: `.insert()`, `.find()`, `.erase()`, `.begin()`, `.end()`                                                                                      |
| `<chrono>`    | `std::chrono::high_resolution_clock`, `std::chrono::system_clock`, `std::chrono::duration_cast`, `std::chrono::milliseconds`, `std::chrono::seconds`                      |
| `<ctime>`     | `std::time`, `std::localtime`, `std::gmtime`, `std::strftime`, `std::difftime`                                                                                            |

---

### Tips on When to Use Which Category

1. **Basic console I/O:** `<iostream>`, `std::cout`, `std::cin`, `std::getline`.
2. **File I/O:** `<fstream>` (if you need to read/write files).
3. **Math-heavy code:** `<cmath>` (e.g., `sin`, `cos`, `sqrt`, `pow`).
4. **String/character processing:**

   * If using `std::string`, stick with string member functions (`.length()`, `.substr()`, `.find()`, etc.) and free functions `<string>` offers (`std::stoi`, `std::to_string`).
   * If you must interoperate with C-style strings (`char[]`), use `<cstring>` (`strlen`, `strcpy`, `strcat`, `strcmp`, etc.) and `<cctype>` (`isalpha`, `isdigit`, `toupper`, `tolower`, etc.).
5. **Raw memory or legacy C code:** `<cstdlib>`’s `malloc`/`free`/`realloc`.
6. **Generic algorithms on containers:** `<algorithm>` + `<numeric>`. If you store elements in a `std::vector`, `std::deque`, or `std::string`, you can apply `std::sort`, `std::find`, `std::count`, `std::accumulate`, etc., to them.
7. **Time measurement:** `<chrono>` (modern C++, type-safe) vs. `<ctime>` (C-style).
8. **Containers (STL):** Each container has its own header: `<vector>`, `<list>`, `<deque>`, `<map>`, `<unordered_map>`, `<set>`, `<queue>`, `<stack>`, `<array>`. You typically use their member functions (e.g., `push_back`, `emplace`, `insert`, `erase`, etc.).

---

## 13. Putting It All Together: A Larger Example

Below is a small program that touches on several built-in functions across different headers, just to illustrate how they fit together.

```cpp
#include <iostream>     // cout, cin
#include <string>       // std::string, getline, stoi, to_string
#include <vector>       // std::vector
#include <algorithm>    // sort, count
#include <iomanip>      // std::fixed, std::setprecision
#include <cmath>        // sqrt, pow
#include <ctime>        // time, localtime, strftime
using namespace std;

int main() {
    cout << "=== C++ Built-in Functions Demo ===\n\n";

    // 1) Get user input as a line, then convert to an integer
    cout << "Enter the number of elements you want (integer): ";
    string line;
    getline(cin, line);
    int n = 0;
    try {
        n = stoi(line);                // std::stoi from <string>
    } catch (...) {
        cerr << "Invalid number, defaulting to 5.\n";
        n = 5;
    }

    // 2) Fill a vector with squares of indices using <vector> + <cmath>
    vector<double> data;
    for (int i = 1; i <= n; ++i) {
        data.push_back(pow(i, 2));     // pow(x,y) from <cmath>
    }

    // 3) Sort the vector (though it is already sorted here)
    sort(data.begin(), data.end());   // std::sort from <algorithm>

    // 4) Print the sorted data with fixed precision
    cout << fixed << setprecision(2); // from <iomanip>
    cout << "Squares of 1 through " << n << ": ";
    for (double x : data) {
        cout << x << " ";
    }
    cout << "\n";

    // 5) Compute average using <numeric> (accumulate)
    double sum = accumulate(data.begin(), data.end(), 0.0); // #include <numeric>
    double avg = sum / n;
    cout << "Average of those squares: " << avg << "\n";

    // 6) Count how many squares exceed a threshold
    double threshold = 10.0;
    int countHigh = count_if(data.begin(), data.end(), [threshold](double v) {
        return v > threshold;         // lambda + <algorithm>::count_if
    });
    cout << "Number of squares > " << threshold << ": " << countHigh << "\n";

    // 7) Print current local time (C style)
    time_t now = time(nullptr);        // <ctime>
    char tbuf[64];
    strftime(tbuf, sizeof(tbuf), "%Y-%m-%d %H:%M:%S", localtime(&now));
    cout << "Current Local Time: " << tbuf << "\n";

    return 0;
}
```

**Key built-in functions used in the example:**

* `std::getline` (getline user input)
* `std::stoi` (convert string → int)
* `std::vector::push_back` (append to vector)
* `std::pow` (square computation)
* `std::sort` (sort a `std::vector<double>`)
* `std::accumulate` (sum elements, from `<numeric>`)
* `std::count_if` (count elements via a predicate, from `<algorithm>`)
* `std::time`, `std::localtime`, `std::strftime` (get and format current time, from `<ctime>`)

---

### Conclusion

This guide has covered:

1. **Basic console and file I/O** (`<iostream>`, `<fstream>`).
2. **Mathematical operations** (`<cmath>`).
3. **Character classification & transformations** (`<cctype>`).
4. **C-string manipulation & memory routines** (`<cstring>`, `<cstdlib>`).
5. **General utility/conversion functions** (`std::atoi`, `std::exit`, `std::swap`, etc.).
6. **Common generic algorithms** (`<algorithm>`, `<numeric>`).
7. **Container member functions** (`<vector>`, `<string>`, `<map>`, `<set>`, etc.).
8. **Time/date functions** (`<ctime>`, `<chrono>`).

Feel free to refer to each header’s documentation (e.g., cppreference.com) for an exhaustive list of functions. However, the ones shown above form the core set you’ll use day-to-day in most C++ programs. If you need deeper coverage (e.g., `<iterator>`, `<functional>`, `<thread>`, `<mutex>`, etc.), let me know!
