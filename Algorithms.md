## 1. `<iostream>` (console I/O)

```cpp
#include <iostream>
#include <string>
using namespace std;

int main() {
    // cout / cin / cerr / clog
    int x;
    cout << "Enter a number: ";
    cin  >> x;                              // read an int
    cout << "You entered: " << x << "\n";   // write to stdout

    if (x < 0) {
        cerr << "Error: negative!\n";       // unbuffered error output
    } else {
        clog << "Info: OK\n";               // buffered log output
    }

    // getline (read a full line into string)
    cin.ignore(numeric_limits<streamsize>::max(), '\n');
    string line;
    cout << "Enter a sentence: ";
    getline(cin, line);
    cout << "You said: " << line << "\n";

    return 0;
}
```

* **`cout` / `cin` / `cerr` / `clog`**
  Formatted console I/O streams.
* **`getline(cin, string)`**
  Reads an entire line (including spaces) into a `string`.

---

## 2. `<fstream>` (file I/O)

```cpp
#include <fstream>
#include <iostream>
#include <string>
using namespace std;

int main() {
    // Writing to a file
    ofstream fout("data.txt");
    if (!fout) { cerr << "Cannot open file\n"; return 1; }
    fout << "Hello, file!\n" << 123 << "\n";
    fout.close();

    // Reading from a file
    ifstream fin("data.txt");
    if (!fin) { cerr << "Cannot open file\n"; return 1; }
    string s;
    while (getline(fin, s)) {
        cout << "Read: " << s << "\n";
    }
    fin.close();
    return 0;
}
```

* **`ofstream`**: write‐only file stream.
* **`ifstream`**: read‐only file stream.
* **`fstream`**: read/write file stream.

---

## 3. `<cmath>` (math functions)

```cpp
#include <iostream>
#include <cmath>
using namespace std;

int main() {
    double a = -2.7, b = 5.3;

    cout << "abs(-2.7)       = " << abs(a)      << "\n"; // 2.7
    cout << "ceil(-2.7)      = " << ceil(a)     << "\n"; // -2
    cout << "floor(-2.7)     = " << floor(a)    << "\n"; // -3
    cout << "round(-2.7)     = " << round(a)    << "\n"; // -3
    cout << "sqrt(25)        = " << sqrt(25)    << "\n"; // 5
    cout << "pow(2, 8)       = " << pow(2, 8)   << "\n"; // 256
    cout << "exp(1)          = " << exp(1)      << "\n"; // e ≈ 2.71828
    cout << "log(10)         = " << log(10)     << "\n"; // ≈2.30258
    cout << "sin(3.14)       = " << sin(3.14)   << "\n"; // ≈0.00159
    cout << "cos(3.14)       = " << cos(3.14)   << "\n"; // ≈−0.99999

    return 0;
}
```

* **`abs(x)`**, **`ceil(x)`**, **`floor(x)`**, **`round(x)`**
* **`sqrt(x)`**, **`pow(x,y)`**, **`exp(x)`**, **`log(x)`**
* **Trigonometry:** `sin`, `cos`, `tan`, `asin`, `acos`, `atan`, plus hyperbolic variants (`sinh`, `cosh`, `tanh`).

---

## 4. `<cctype>` (character tests/transforms)

```cpp
#include <iostream>
#include <cctype>
using namespace std;

int main() {
    char s[] = "Hello World! 123";
    for (int i = 0; s[i]; ++i) {
        if (isalpha(s[i])) {
            s[i] = toupper(s[i]);   // convert letters to uppercase
        }
    }
    cout << "Uppercase: " << s << "\n";

    int digits = 0, spaces = 0;
    for (int i = 0; s[i]; ++i) {
        if (isdigit(s[i])) ++digits;
        if (isspace(s[i])) ++spaces;
    }
    cout << "Digits: " << digits << ", Spaces: " << spaces << "\n";
    return 0;
}
```

* **`isalpha(c)`**, **`isdigit(c)`**, **`isspace(c)`**, **`isalnum(c)`**, **`isupper(c)`**, **`islower(c)`**
* **`toupper(c)`**, **`tolower(c)`**

---

## 5. `<cstring>` / `<cstdlib>` (C‐string & memory)

```cpp
#include <iostream>
#include <cstring>  // C‐string routines
#include <cstdlib>  // malloc/free, atoi, exit
using namespace std;

int main() {
    char src[] = "Hello";
    char dst[20];
    strcpy(dst, src);                  // copy C‐string
    strcat(dst, ", C++");              // concatenate
    cout << "dst = " << dst << "\n";   // "Hello, C++"

    cout << "Length: " << strlen(dst) << "\n"; // 9

    char* p = strstr(dst, "C++");      // find substring
    if (p) cout << "\"C++\" at index " << (p - dst) << "\n";

    // memset / memcpy / memmove
    char buf[6];
    memset(buf, 'A', 5);               // fill first 5 bytes with 'A'
    buf[5] = '\0';
    cout << "memset buf = " << buf << "\n";

    int arr1[3] = {1,2,3}, arr2[3];
    memcpy(arr2, arr1, sizeof(arr1));  // copy raw bytes
    cout << "arr2: " << arr2[0] << "," << arr2[1] << "," << arr2[2] << "\n";

    // malloc / realloc / free
    int* arr = (int*)malloc(4 * sizeof(int)); // allocate 4 ints
    if (!arr) exit(1);
    for (int i = 0; i < 4; ++i) arr[i] = i+1;
    arr = (int*)realloc(arr, 8 * sizeof(int)); // grow to 8 ints
    for (int i = 4; i < 8; ++i) arr[i] = i+1;
    for (int i = 0; i < 8; ++i) cout << arr[i] << " ";
    cout << "\n";
    free(arr);

    // atoi / atof
    int v = atoi("123");     // 123
    double d = atof("3.14"); // 3.14
    cout << "atoi: " << v << ", atof: " << d << "\n";

    return 0;
}
```

* **`strlen(s)`**, **`strcpy(dst,src)`**, **`strcat(dst,src)`**, **`strcmp(s1,s2)`**, **`strstr(hay,needle)`**, **`strchr(s,c)`**, **`strrchr(s,c)`**
* **`memset(ptr,val,n)`**, **`memcpy(dest,src,n)`**, **`memmove(dest,src,n)`**
* **`malloc(n)`**, **`calloc(n,size)`**, **`realloc(ptr,newSize)`**, **`free(ptr)`**
* **`atoi(str)`**, **`atof(str)`**, **`exit(code)`**

---

## 6. `<utility>` (swap, pair, exchange, tie)

```cpp
#include <iostream>
#include <utility>  // swap, pair, make_pair, tie, exchange
#include <tuple>    // for tie unpacking
using namespace std;

int main() {
    // swap
    int a = 10, b = 20;
    cout << "Before: a=" << a << ", b=" << b << "\n";
    swap(a, b);
    cout << "After:  a=" << a << ", b=" << b << "\n";

    // pair / make_pair
    pair<int,string> p = make_pair(42, "Answer");
    cout << "pair: (" << p.first << ", " << p.second << ")\n";

    // tie (unpack)
    int num; string txt;
    tie(num, txt) = p;
    cout << "Unpacked: " << num << ", " << txt << "\n";

    // exchange (set to new value, return old)
    int old = exchange(a, 100);
    cout << "old=" << old << ", new a=" << a << "\n";

    return 0;
}
```

* **`swap(x,y)`**
* **`pair<T1,T2>`**, **`make_pair(a,b)`**
* **`tie(v1,v2,…)`** (unpack tuple/pair into variables)
* **`exchange(obj,newVal)`**

---

## 7. `<algorithm>` (generic algorithms)

```cpp
#include <iostream>
#include <vector>
#include <algorithm> // lots of algorithms
#include <numeric>   // for accumulate, iota, partial_sum, inner_product
#include <random>    // for shuffle
using namespace std;

int main() {
    vector<int> v = {3,1,4,1,5,9,2,6,5};

    // sort / reverse / shuffle
    sort(v.begin(), v.end());         // sort ascending
    reverse(v.begin(), v.end());      // reverse order

    random_device rd;
    mt19937 g(rd());
    shuffle(v.begin(), v.end(), g);   // random shuffle

    // find / count / count_if
    auto it = find(v.begin(), v.end(), 5);  // first 5
    int c1 = count(v.begin(), v.end(), 5);  // how many 5’s
    int c2 = count_if(v.begin(), v.end(), [](int x){ return x > 5; });

    // copy / copy_if / copy_n
    vector<int> dst(v.size());
    copy(v.begin(), v.end(), dst.begin());           // copy all
    vector<int> odds;
    copy_if(v.begin(), v.end(), back_inserter(odds), [](int x){ return x % 2; });

    // fill / fill_n / generate
    vector<int> a(5);
    fill(a.begin(), a.end(), 42);                    // fill all with 42
    fill_n(a.begin(), 3, 7);                          // first 3 elements = 7
    int counter = 0;
    generate(a.begin(), a.end(), [&]{ return counter++; }); // fill with 0,1,2,3,4

    // remove / remove_if + erase
    vector<int> w = {1,2,3,2,4,2,5};
    auto newEnd = remove(w.begin(), w.end(), 2);      // “remove” 2’s (shifts forward)
    w.erase(newEnd, w.end());                         // actually erase from container

    // replace / replace_if
    replace(v.begin(), v.end(), 9, 99);               // replace 9→99
    replace_if(v.begin(), v.end(), [](int x){ return x % 2 == 0; }, 0); // evens→0

    // unique / unique_copy
    vector<int> dup = {1,1,2,2,3,3,3};
    auto it2 = unique(dup.begin(), dup.end());        // removes consecutive duplicates
    dup.erase(it2, dup.end());

    vector<int> noDup;
    unique_copy(v.begin(), v.end(), back_inserter(noDup)); // copy skipping consecutive duplicates

    // partition / stable_partition
    partition(v.begin(), v.end(), [](int x){ return x < 5; }); // those<5 first
    stable_partition(v.begin(), v.end(), [](int x){ return x < 5; }); // stable version

    // nth_element (partial sort)
    vector<int> b = {7,2,9,4,1,6,5,3,8};
    nth_element(b.begin(), b.begin()+2, b.end());      // b[2] becomes 3rd-smallest

    // min_element / max_element / minmax_element
    auto minIt = min_element(v.begin(), v.end());
    auto maxIt = max_element(v.begin(), v.end());
    auto [mnIt, mxIt] = minmax_element(v.begin(), v.end());

    // binary_search / lower_bound / upper_bound  (requires sorted)
    sort(v.begin(), v.end());
    bool found = binary_search(v.begin(), v.end(), 4);
    auto lb = lower_bound(v.begin(), v.end(), 4);
    auto ub = upper_bound(v.begin(), v.end(), 4);

    // distance (iterator difference)
    auto dist = distance(v.begin(), lb);

    return 0;
}
```

* **Mutators / reorderers:**
  `sort`, `reverse`, `shuffle`, `rotate`, `next_permutation`, `prev_permutation`, `partition`, `stable_partition`, `remove`, `remove_if`, `replace`, `replace_if`, `unique`, `unique_copy`, `fill`, `fill_n`, `generate`.
* **Queries / inspections:**
  `find`, `find_if`, `count`, `count_if`, `min_element`, `max_element`, `minmax_element`, `binary_search`, `lower_bound`, `upper_bound`, `equal`, `mismatch`, `distance`.
* **Copying & merging:**
  `copy`, `copy_if`, `copy_n`, `reverse_copy`, `merge`.
* **Partial sorts:**
  `partition`, `nth_element`.

---

## 8. `<numeric>` (numeric algorithms)

```cpp
#include <iostream>
#include <vector>
#include <numeric> // accumulate, iota, partial_sum, adjacent_difference, inner_product, gcd, lcm
using namespace std;

int main() {
    vector<int> v(10);

    // iota (fill with sequential values)
    iota(v.begin(), v.end(), 1); // v = {1,2,...,10}

    // accumulate (sum)
    int sum = accumulate(v.begin(), v.end(), 0);

    // partial_sum (prefix sums)
    vector<int> ps(v.size());
    partial_sum(v.begin(), v.end(), ps.begin()); // ps[i] = sum of v[0]..v[i]

    // adjacent_difference (differences)
    vector<int> diff(v.size());
    adjacent_difference(v.begin(), v.end(), diff.begin()); // diff[0]=v[0], diff[i]=v[i]-v[i-1]

    // inner_product (dot product)
    long long dot = inner_product(v.begin(), v.end(), v.begin(), 0LL); // sum of squares

    // gcd / lcm (C++17+)
    int a = 48, b = 18;
    int g = gcd(a, b);  // 6
    int l = lcm(a, b);  // 144

    return 0;
}
```

* **`iota(first, last, value)`**: fill `[first,last)` with `value,value+1,...`.
* **`accumulate(first, last, init)`**: returns `init + ∑(*it)`.
* **`partial_sum(first, last, dest)`**: prefix sums into `dest`.
* **`adjacent_difference(first, last, dest)`**: differences between adjacent elements.
* **`inner_product(first1, last1, first2, init)`**: dot-product style accumulation.
* **`gcd(a,b)`**, **`lcm(a,b)`** (C++17 and above).

---

## 9. `<limits>` / `<climits>` / `<cfloat>` (numeric limits)

```cpp
#include <iostream>
#include <limits>   // numeric_limits
#include <climits>  // INT_MAX, INT_MIN, etc.
#include <cfloat>   // FLT_MAX, DBL_EPSILON, etc.
using namespace std;

int main() {
    // <limits> (type-safe)
    cout << "int min:    " << numeric_limits<int>::min()   << "\n";
    cout << "int max:    " << numeric_limits<int>::max()   << "\n";
    cout << "double eps: " << numeric_limits<double>::epsilon() << "\n";
    cout << "float inf:  " << numeric_limits<float>::infinity() << "\n";

    // <climits> (C macros)
    cout << "INT_MIN:    " << INT_MIN   << "\n";
    cout << "INT_MAX:    " << INT_MAX   << "\n";
    cout << "LONG_MIN:   " << LONG_MIN  << "\n";
    cout << "LONG_MAX:   " << LONG_MAX  << "\n";

    // <cfloat> (floating macros)
    cout << "FLT_MIN:    " << FLT_MIN   << "\n";
    cout << "FLT_MAX:    " << FLT_MAX   << "\n";
    cout << "DBL_EPSILON:"<< DBL_EPSILON << "\n";

    return 0;
}
```

* **`numeric_limits<T>::min()/max()/lowest()/epsilon()/infinity()/quiet_NaN()/denorm_min()`**
* **Macros in `<climits>`**: `CHAR_BIT`, `SCHAR_MIN`, `SCHAR_MAX`, `UCHAR_MAX`, `SHRT_MIN`, `SHRT_MAX`, `USHRT_MAX`, `INT_MIN`, `INT_MAX`, `UINT_MAX`, `LONG_MIN`, `LONG_MAX`, `ULONG_MAX`, `LLONG_MIN`, `LLONG_MAX`, `ULLONG_MAX`.
* **Macros in `<cfloat>`**: `FLT_MIN`, `FLT_MAX`, `DBL_MIN`, `DBL_MAX`, `FLT_EPSILON`, `DBL_EPSILON`, etc.

---

## 10. `<chrono>` / `<ctime>` (time & clocks)

```cpp
#include <iostream>
#include <chrono>  // C++11+ chrono library
#include <ctime>   // C-style time
using namespace std;
using namespace chrono;

int main() {
    // C++11-style: measure elapsed time
    auto t0 = high_resolution_clock::now();
    // … do work …
    for (volatile int i = 0; i < 1000000; ++i);
    auto t1 = high_resolution_clock::now();
    auto ms = duration_cast<milliseconds>(t1 - t0).count();
    cout << "Elapsed: " << ms << " ms\n";

    // C-style: get current calendar time
    time_t now = time(nullptr);
    tm* lt = localtime(&now);
    char buf[64];
    strftime(buf, sizeof(buf), "%Y-%m-%d %H:%M:%S", lt);
    cout << "Local time: " << buf << "\n";

    return 0;
}
```

* **`chrono::high_resolution_clock::now()`**, **`system_clock::now()`**, **`steady_clock::now()`**
* **`duration_cast<…>(duration)`**
* **`time(nullptr)`** (returns `time_t`)
* **`localtime(&time_t)`**, **`gmtime(&time_t)`**
* **`strftime(buf,size,format,tm*)`**

---

## 11. Containers & Member Functions (most common)

```cpp
#include <iostream>
#include <vector>
#include <string>
#include <map>
#include <set>
#include <queue>
#include <stack>
using namespace std;

int main() {
    // vector<T>
    vector<int> v;
    v.push_back(10);            // append
    v.emplace_back(20);         // append by constructing
    cout << "v[0] = " << v[0] << "\n";
    cout << "size = " << v.size() << ", empty? " << boolalpha << v.empty() << "\n";
    v.pop_back();               // erase last
    v.clear();                  // remove all

    // string
    string s = "Hello";
    s += " World";              // append
    cout << s << "\n";
    string sub = s.substr(6, 5); // "World"
    size_t pos = s.find("lo");  // 3

    // map<Key,Value>
    map<string,int> mp;
    mp["apple"] = 5;
    mp.insert({"banana", 3});
    auto it = mp.find("apple");
    if (it != mp.end()) cout << it->first << "→" << it->second << "\n";
    mp.erase("banana");

    // set<T>
    set<int> st = {3,1,4};
    st.insert(2);
    if (st.count(3)) cout << "3 is in set\n";
    st.erase(1);

    // queue<T>
    queue<int> qu;
    qu.push(1);
    qu.push(2);
    cout << "front: " << qu.front() << ", back: " << qu.back() << "\n";
    qu.pop();

    // stack<T>
    stack<string> stck;
    stck.push("A");
    stck.push("B");
    cout << "top: " << stck.top() << "\n";
    stck.pop();

    return 0;
}
```

* **`vector<T>`**:
  `.push_back()`, `.emplace_back()`, `.pop_back()`, `.size()`, `.empty()`, `.clear()`, `.insert()`, `.erase()`, `.reserve()`, `.capacity()`.
* **`string`**:
  `.length()/size()`, `.empty()`, `.substr(pos,len)`, `.find(str)`, `.append()`, `.erase()`, `.replace()`, `.c_str()`.
* **`map<Key,Value>` / `unordered_map<Key,Value>`**:
  `.insert()`, `operator[]`, `.find()`, `.erase()`, `.count()`, `.begin()`, `.end()`.
* **`set<T>` / `unordered_set<T>`**:
  `.insert()`, `.find()`, `.count()`, `.erase()`.
* **`queue<T>`**, **`stack<T>`**, **`deque<T>`**, **`list<T>`**, **`array<T,N>`**, **`bitset<N>`**:
  Container‐specific member functions (`.push()`, `.pop()`, `.front()`, `.back()`, `.size()`, `.fill()`, `.count()`, `.test()`).

---

## 12. Miscellaneous Utilities

```cpp
#include <iostream>
#include <bitset>
#include <array>
#include <random>
using namespace std;

int main() {
    // bitset<N>
    bitset<8> b(0b00101101);
    cout << "bits = " << b << "\n";      // 00101101
    cout << "count = " << b.count() << "\n"; // 4 bits set
    b.flip(7);                            // set highest bit
    cout << "after flip = " << b << "\n";

    // array<T,N>
    array<int,5> arr;
    arr.fill(7);                          // all elements = 7
    for (int x : arr) cout << x << " ";
    cout << "\n";
    cout << "arr[2] = " << arr.at(2) << "\n";

    // random (C++11)
    random_device rd;
    mt19937 gen(rd());
    uniform_int_distribution<> dist(1, 100);
    cout << "Random number ∈ [1,100]: " << dist(gen) << "\n";

    return 0;
}
```

* **`bitset<N>`**: fixed‐size bit vector. Methods: `.set(pos)`, `.reset(pos)`, `.flip(pos)`, `.count()`, `.to_ulong()`.
* **`array<T,N>`**: fixed‐size array. Methods: `.fill(val)`, `.at(i)`, `.size()`, `.begin()/end()`.
* **`<random>`**: random‐number generation: `mt19937`, `uniform_int_distribution`, `shuffle`, etc.

---

### Header‐by‐Header Quick Reference

| Header                   | Key Functions / Utilities                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| ------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `<iostream>`             | `cout`, `cin`, `cerr`, `clog`, `getline`, `endl`.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| `<fstream>`              | `ifstream`, `ofstream`, `fstream`, `getline(fin, str)`.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| `<string>`               | `string`, `.length()/.size()`, `.empty()`, `.substr()`, `.find()`, `.append()`, `.erase()`, `.replace()`, `to_string`, `stoi`, `stod`.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| `<cstring>`              | `strlen`, `strcpy`, `strncpy`, `strcat`, `strncat`, `strcmp`, `strncmp`, `strchr`, `strrchr`, `strstr`.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| `<cstdlib>`              | `malloc`, `calloc`, `realloc`, `free`, `atoi`, `atof`, `exit`, `rand`, `srand`.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| `<cmath>`                | `abs`, `ceil`, `floor`, `round`, `sqrt`, `pow`, `exp`, `log`, `log10`, `sin`, `cos`, `tan`, `asin`, `acos`, `atan`, `sinh`, `cosh`, `tanh`, etc.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| `<cctype>`               | `isalpha`, `isdigit`, `isspace`, `isalnum`, `isupper`, `islower`, `toupper`, `tolower`.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| `<algorithm>`            | **Mutators:** `sort`, `reverse`, `shuffle`, `rotate`, `next_permutation`, `prev_permutation`, `partition`, `stable_partition`, `remove`, `remove_if`, `replace`, `replace_if`, `unique`, `unique_copy`, `fill`, `fill_n`, `generate`.<br>**Queries:** `find`, `find_if`, `count`, `count_if`, `min_element`, `max_element`, `minmax_element`, `binary_search`, `lower_bound`, `upper_bound`, `equal`, `mismatch`, `distance`.<br>**Copy/Merge:** `copy`, `copy_if`, `copy_n`, `reverse_copy`, `merge`.<br>**Partial Sort:** `partition`, `nth_element`. |
| `<numeric>`              | `accumulate`, `iota`, `partial_sum`, `adjacent_difference`, `inner_product`, `gcd`, `lcm`, `reduce` (C++17).                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| `<limits>`               | `numeric_limits<T>::min()/max()/lowest()/epsilon()/infinity()/quiet_NaN()/denorm_min()`, plus boolean flags `.is_signed`, `.is_integer`.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| `<climits>` / `<cfloat>` | **Integers:** `CHAR_BIT`, `SCHAR_MIN`, `SCHAR_MAX`, `UCHAR_MAX`, `SHRT_MIN`, `SHRT_MAX`, `USHRT_MAX`, `INT_MIN`, `INT_MAX`, `UINT_MAX`, `LONG_MIN`, `LONG_MAX`, `ULONG_MAX`, `LLONG_MIN`, `LLONG_MAX`, `ULLONG_MAX`.  **Floats:** `FLT_MIN`, `FLT_MAX`, `DBL_MIN`, `DBL_MAX`, `FLT_EPSILON`, `DBL_EPSILON`, etc.                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| `<chrono>`               | `chrono::high_resolution_clock::now()`, `system_clock::now()`, `steady_clock::now()`, `chrono::duration_cast<…>`.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| `<ctime>`                | `time(nullptr)`, `localtime`, `gmtime`, `strftime`, `difftime`.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| `<utility>`              | `swap`, `pair`, `make_pair`, `tie`, `exchange`.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| `<vector>`               | `vector<T>`, `.push_back`, `.emplace_back`, `.pop_back`, `.size`, `.empty`, `.clear`, `.insert`, `.erase`, `.reserve`, `.capacity`, `.begin`, `.end`, `.front`, `.back`, `.operator[]`, `.at`.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| `<array>`                | `array<T,N>`, `.fill`, `.at`, `.size`, `.begin`, `.end`.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| `<bitset>`               | `bitset<N>`, `.set(pos)`, `.reset(pos)`, `.flip(pos)`, `.count`, `.to_ulong`, `.test(pos)`.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| `<map>` / `<set>`        | `map<Key,Value>`, `set<Value>`, `.insert`, `operator[]`, `.find`, `.erase`, `.count`, `.begin`, `.end`.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| `<queue>` / `<stack>`    | `queue<T>`, `stack<T>`, `.push`, `.pop`, `.front`, `.back`, `.top`, `.size`, `.empty`.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| `<random>`               | `random_device`, `mt19937`, `uniform_int_distribution`, `shuffle`, etc.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
