````
## Table of Contents

1. [Non-Modifying Sequence Operations](#non-modifying-sequence-operations)  
2. [Modifying Sequence Operations](#modifying-sequence-operations)  
3. [Heap Operations](#heap-operations)  
4. [Sorting and Related Operations](#sorting-and-related-operations)  
5. [Binary Search and Related Operations](#binary-search-and-related-operations)  
6. [Merging and Set Operations](#merging-and-set-operations)  
7. [Min/Max and Related](#minmax-and-related)  
8. [Permutations and Combinations](#permutations-and-combinations)  
9. [Miscellaneous Utilities](#miscellaneous-utilities)  
10. [Deprecated or Removed](#deprecated-or-removed)  

---

## 1. Non-Modifying Sequence Operations

These algorithms examine or traverse ranges without changing the underlying elements.

### 1.1 `for_each`

**Signature**  
```cpp
template<class InputIt, class UnaryFunction>
UnaryFunction for_each(InputIt first, InputIt last, UnaryFunction f);
````

**Description**
Applies a callable `f` to every element in `[first, last)`.

**Example**

```cpp
#include <algorithm>
#include <iostream>
#include <vector>

int main() {
    std::vector<int> nums = {1, 2, 3, 4, 5};
    std::cout << "Elements: ";
    std::for_each(nums.begin(), nums.end(), [](int x) {
        std::cout << x << " ";
    });
    std::cout << "\n";
    return 0;
}
```

---

### 1.2 `find` / `find_if` / `find_if_not`

* **`find(InputIt first, InputIt last, const T& value)`** — returns an iterator to the first element equal to `value`.
* **`find_if(InputIt first, InputIt last, UnaryPredicate p)`** — returns the first element for which `p(element)` is true.
* **`find_if_not(InputIt first, InputIt last, UnaryPredicate q)`** — returns the first element for which `q(element)` is false (since C++11).

**Example**

```cpp
#include <algorithm>
#include <iostream>
#include <vector>

bool is_even(int x) { return x % 2 == 0; }

int main() {
    std::vector<int> v = {3, 5, 7, 8, 10};

    // find first occurrence of 7
    auto it1 = std::find(v.begin(), v.end(), 7);
    if (it1 != v.end()) {
        std::cout << "Found 7 at index " << std::distance(v.begin(), it1) << "\n";
    }

    // find first even number
    auto it2 = std::find_if(v.begin(), v.end(), is_even);
    if (it2 != v.end()) {
        std::cout << "First even: " << *it2 << "\n";
    }

    // find first number that is NOT even
    auto it3 = std::find_if_not(v.begin(), v.end(), is_even);
    if (it3 != v.end()) {
        std::cout << "First not-even: " << *it3 << "\n";
    }
    return 0;
}
```

---

### 1.3 `count` / `count_if`

* **`count(InputIt first, InputIt last, const T& value)`** — returns the number of elements equal to `value`.
* **`count_if(InputIt first, InputIt last, UnaryPredicate p)`** — returns the number of elements satisfying predicate `p`.

**Example**

```cpp
#include <algorithm>
#include <iostream>
#include <vector>

bool is_odd(int x) { return x % 2 != 0; }

int main() {
    std::vector<int> data = {1, 2, 3, 2, 4, 2, 5};

    // Count how many times '2' appears:
    auto cnt2 = std::count(data.begin(), data.end(), 2);
    std::cout << "Number of 2's: " << cnt2 << "\n";

    // Count how many are odd
    auto cnt_odd = std::count_if(data.begin(), data.end(), is_odd);
    std::cout << "Number of odd elements: " << cnt_odd << "\n";

    return 0;
}
```

---

### 1.4 `mismatch` / `equal`

* **`mismatch(InputIt1 first1, InputIt1 last1, InputIt2 first2)`** — compares two ranges element-by-element and returns the first position where they differ (as a pair of iterators).
* **`equal(InputIt1 first1, InputIt1 last1, InputIt2 first2)`** — returns `true` if every element in `[first1, last1)` matches the corresponding element starting at `first2`. There is also an overload taking two ranges.

**Example**

```cpp
#include <algorithm>
#include <iostream>
#include <vector>
#include <utility> // for std::pair

int main() {
    std::vector<int> a = {1, 2, 3, 9, 5};
    std::vector<int> b = {1, 2, 3, 4, 5};

    // Find first mismatch
    auto [it_a, it_b] = std::mismatch(a.begin(), a.end(), b.begin());
    if (it_a != a.end()) {
        std::cout << "First mismatch at position " << std::distance(a.begin(), it_a)
                  << ": a=" << *it_a << ", b=" << *it_b << "\n";
    }

    // Check if two ranges are equal
    bool are_equal = std::equal(a.begin(), a.end(), b.begin());
    std::cout << "Ranges equal? " << (are_equal ? "Yes" : "No") << "\n";

    return 0;
}
```

---

### 1.5 `search` / `search_n`

* **`search(ForwardIt1 first1, ForwardIt1 last1, ForwardIt2 first2, ForwardIt2 last2)`** — looks for the first occurrence of the sequence `[first2, last2)` within `[first1, last1)`.
* **`search_n(ForwardIt first, ForwardIt last, Size count, const T& value)`** — finds the first subrange of `count` consecutive elements equal to `value`.

**Example**

```cpp
#include <algorithm>
#include <iostream>
#include <vector>

int main() {
    std::vector<int> v = {1, 2, 3, 4, 2, 3, 4, 5};
    std::vector<int> pattern = {2, 3, 4};

    // search for pattern {2,3,4}
    auto it = std::search(v.begin(), v.end(), pattern.begin(), pattern.end());
    if (it != v.end()) {
        std::cout << "Pattern found at index " << std::distance(v.begin(), it) << "\n";
    }

    // search_n: find three consecutive '4's (none in this example)
    std::vector<int> w = {4, 4, 4, 2, 3};
    auto it_n = std::search_n(w.begin(), w.end(), 3, 4); 
    if (it_n != w.end()) {
        std::cout << "Three consecutive 4's start at index " << std::distance(w.begin(), it_n) << "\n";
    } else {
        std::cout << "No three consecutive 4's found\n";
    }

    return 0;
}
```

---

## 2. Modifying Sequence Operations

These algorithms alter the contents of one or more ranges in place or by copying to an output range.

### 2.1 `copy` / `copy_if` / `copy_n` / `copy_backward`

* **`copy(InputIt first, InputIt last, OutputIt d_first)`** — copies `[first, last)` to `[d_first, d_first + (last-first))`.
* **`copy_if(InputIt first, InputIt last, OutputIt d_first, UnaryPredicate p)`** — copies only elements for which `p(element)` is true (since C++11).
* **`copy_n(InputIt first, Size count, OutputIt result)`** — copies exactly `count` elements starting at `first` to `result` (since C++11).
* **`copy_backward(BidirectionalIt1 first, BidirectionalIt1 last, BidirectionalIt2 d_last)`** — copies `[first, last)` into the range ending at `d_last`, going backwards (useful when source and destination overlap).

**Example**

```cpp
#include <algorithm>
#include <iostream>
#include <vector>

bool is_positive(int x) { return x > 0; }

int main() {
    std::vector<int> src = {3, -1, 4, -2, 5};
    std::vector<int> dest1(src.size());
    std::vector<int> filtered;

    // 1) copy all elements
    std::copy(src.begin(), src.end(), dest1.begin());
    std::cout << "Copied elements: ";
    for (int x : dest1) std::cout << x << " ";
    std::cout << "\n";

    // 2) copy only positive elements
    filtered.resize(src.size()); // allocate max possible space
    auto it = std::copy_if(src.begin(), src.end(), filtered.begin(), is_positive);
    filtered.erase(it, filtered.end()); // shrink to actual size
    std::cout << "Filtered positives: ";
    for (int x : filtered) std::cout << x << " ";
    std::cout << "\n";

    // 3) copy first 3 elements
    std::vector<int> first3(3);
    std::copy_n(src.begin(), 3, first3.begin());
    std::cout << "First 3 elements: ";
    for (int x : first3) std::cout << x << " ";
    std::cout << "\n";

    // 4) copy_backward when overlapping
    std::vector<int> overlap = {1, 2, 3, 4, 5, 0, 0};
    // Shift first 5 elements two positions to the right in-place:
    std::copy_backward(overlap.begin(), overlap.begin() + 5, overlap.end());
    // overlap now: [1, 2, 3, 1, 2, 3, 4] (original 5 dropped)
    std::cout << "After copy_backward: ";
    for (int x : overlap) std::cout << x << " ";
    std::cout << "\n";

    return 0;
}
```

---

### 2.2 `move` / `move_backward`

* **`move(InputIt first, InputIt last, OutputIt d_first)`** (since C++11) — moves (i.e., uses move‐semantics) elements from `[first, last)` into `[d_first, …)`.
* **`move_backward(BidirectionalIt1 first, BidirectionalIt1 last, BidirectionalIt2 d_last)`** (since C++11) — moves elements in reverse order to avoid overwriting when ranges overlap.

**Example**

```cpp
#include <algorithm>
#include <iostream>
#include <vector>
#include <string>
#include <utility> // for std::move

int main() {
    std::vector<std::string> src = {"Alice", "Bob", "Carol"};
    std::vector<std::string> dest(3);

    // Move each string into dest
    std::move(src.begin(), src.end(), dest.begin());
    std::cout << "After move, src: ";
    for (auto& s : src) {
        std::cout << "\"" << s << "\" "; // usually empty or unspecified
    }
    std::cout << "\nDest: ";
    for (auto& s : dest) {
        std::cout << "\"" << s << "\" ";
    }
    std::cout << "\n";

    // move_backward example with overlapping ranges
    std::vector<int> ov = {10, 20, 30, 40, 0, 0};
    // Move the first 4 elements two positions to the right:
    std::move_backward(ov.begin(), ov.begin() + 4, ov.end());
    // ov now: {10, 20, 10, 20, 30, 40}
    std::cout << "After move_backward: ";
    for (int x : ov) std::cout << x << " ";
    std::cout << "\n";

    return 0;
}
```

---

### 2.3 `transform`

* **Unary version**

  ```cpp
  template<class InputIt, class OutputIt, class UnaryOperation>
  OutputIt transform(InputIt first, InputIt last, OutputIt d_first, UnaryOperation unary_op);
  ```

  Applies `unary_op` to each element in `[first, last)` and writes the result to `[d_first, …)`.

* **Binary version**

  ```cpp
  template<class InputIt1, class InputIt2, class OutputIt, class BinaryOperation>
  OutputIt transform(InputIt1 first1, InputIt1 last1, InputIt2 first2,
                     OutputIt d_first, BinaryOperation binary_op);
  ```

  Applies `binary_op(*it1, *it2)` to corresponding elements of two ranges, writing the result to output.

**Example**

```cpp
#include <algorithm>
#include <iostream>
#include <vector>

int main() {
    std::vector<int> a = {1, 2, 3, 4, 5};
    std::vector<int> b(5);
    std::vector<int> c(5);

    // Unary transform: square each element of 'a' into 'b'
    std::transform(a.begin(), a.end(), b.begin(), [](int x) {
        return x * x;
    });
    std::cout << "Squares: ";
    for (int x : b) std::cout << x << " ";
    std::cout << "\n";

    // Binary transform: add corresponding elements of 'a' and 'b' into 'c'
    std::transform(a.begin(), a.end(), b.begin(), c.begin(), [](int x, int y) {
        return x + y;
    });
    std::cout << "Sum of a and squares: ";
    for (int x : c) std::cout << x << " ";
    std::cout << "\n";

    return 0;
}
```

---

### 2.4 `replace` / `replace_if` / `replace_copy`

* **`replace(ForwardIt first, ForwardIt last, const T& old_value, const T& new_value)`** — replaces every occurrence of `old_value` in `[first, last)` with `new_value`.
* **`replace_if(ForwardIt first, ForwardIt last, UnaryPredicate p, const T& new_value)`** — replaces elements satisfying `p(element)` with `new_value`.
* **`replace_copy(InputIt first, InputIt last, OutputIt d_first, const T& old_value, const T& new_value)`** — copies `[first, last)` to `d_first`, replacing `old_value` with `new_value` along the way.
* **`replace_copy_if(InputIt first, InputIt last, OutputIt d_first, UnaryPredicate p, const T& new_value)`** — similar to `replace_copy`, but uses predicate.

**Example**

```cpp
#include <algorithm>
#include <iostream>
#include <vector>

bool is_negative(int x) { return x < 0; }

int main() {
    std::vector<int> data = {1, -2, 3, -4, 5};

    // 1) replace -2 with 0 in-place
    std::replace(data.begin(), data.end(), -2, 0);
    // data = {1, 0, 3, -4, 5}

    // 2) replace all negative numbers with 0 using replace_if
    std::replace_if(data.begin(), data.end(), is_negative, 0);
    // data = {1, 0, 3, 0, 5}

    std::cout << "After replace/replace_if: ";
    for (int x : data) std::cout << x << " ";
    std::cout << "\n";

    // 3) replace_copy: replace 0 with 99 into a new vector
    std::vector<int> out(data.size());
    std::replace_copy(data.begin(), data.end(), out.begin(), 0, 99);
    // out = {1, 99, 3, 99, 5}

    std::cout << "After replace_copy: ";
    for (int x : out) std::cout << x << " ";
    std::cout << "\n";

    return 0;
}
```

---

### 2.5 `fill` / `fill_n` / `generate` / `generate_n`

* **`fill(ForwardIt first, ForwardIt last, const T& value)`** — assigns `value` to every element in `[first, last)`.
* **`fill_n(OutputIt first, Size count, const T& value)`** — assigns `value` to the next `count` elements starting at `first`.
* **`generate(ForwardIt first, ForwardIt last, Generator g)`** (since C++11) — assigns `g()` to every element in `[first, last)`.
* **`generate_n(OutputIt first, Size count, Generator g)`** (since C++11) — assigns `g()` to the next `count` elements starting at `first`.

**Example**

```cpp
#include <algorithm>
#include <iostream>
#include <vector>
#include <random>

int main() {
    std::vector<int> a(5);

    // 1) fill with 7
    std::fill(a.begin(), a.end(), 7);
    std::cout << "All 7's: ";
    for (int x : a) std::cout << x << " ";
    std::cout << "\n";

    // 2) fill first 3 elements with 42
    std::fill_n(a.begin(), 3, 42);
    // a = {42, 42, 42, 7, 7}
    std::cout << "After fill_n: ";
    for (int x : a) std::cout << x << " ";
    std::cout << "\n";

    // 3) generate random numbers
    std::mt19937 rng(123); // deterministic seed
    std::uniform_int_distribution<int> dist(1, 100);
    std::generate(a.begin(), a.end(), [&]() { return dist(rng); });
    std::cout << "Random fill: ";
    for (int x : a) std::cout << x << " ";
    std::cout << "\n";

    // 4) generate_n: replace next 2 elements with random values
    std::generate_n(a.begin(), 2, [&]() { return dist(rng); });
    std::cout << "After generate_n (2): ";
    for (int x : a) std::cout << x << " ";
    std::cout << "\n";

    return 0;
}
```

---

### 2.6 `remove` / `remove_if` / `remove_copy`

* **`remove(ForwardIt first, ForwardIt last, const T& value)`** — moves all elements not equal to `value` to the front of `[first, last)` and returns iterator to new “end” (does not actually shrink container).
* **`remove_if(ForwardIt first, ForwardIt last, UnaryPredicate p)`** — moves all elements for which `p(element)` is false to the front.
* **`remove_copy(InputIt first, InputIt last, OutputIt d_first, const T& value)`** — copies all elements not equal to `value` into `d_first`.
* **`remove_copy_if(InputIt first, InputIt last, OutputIt d_first, UnaryPredicate p)`** — copies all elements for which `p(element)` is false into `d_first`.

**Example**

```cpp
#include <algorithm>
#include <iostream>
#include <vector>

bool is_even(int x) { return x % 2 == 0; }

int main() {
    std::vector<int> v = {1, 2, 3, 2, 4, 2, 5};

    // 1) remove all '2's in-place
    auto new_end = std::remove(v.begin(), v.end(), 2);
    // v = {1, 3, 4, 5, ?, ?, ?}, new_end points to index 4
    v.erase(new_end, v.end()); // actually remove trailing “garbage”
    std::cout << "After remove(2): ";
    for (int x : v) std::cout << x << " ";
    std::cout << "\n";

    // 2) remove_if: remove all even numbers
    std::vector<int> w = {1, 2, 3, 4, 5, 6};
    auto new_end2 = std::remove_if(w.begin(), w.end(), is_even);
    w.erase(new_end2, w.end());
    std::cout << "After remove_if(is_even): ";
    for (int x : w) std::cout << x << " ";
    std::cout << "\n";

    // 3) remove_copy: copy all non-even elements to 'out'
    std::vector<int> out;
    out.reserve(6);
    std::remove_copy(w.begin(), w.end(), std::back_inserter(out), 3);
    // removes 3 (but w no longer contains 3). For demonstration, let's use original vector:
    std::vector<int> orig = {1, 2, 3, 4, 5};
    std::vector<int> out2;
    out2.reserve(5);
    std::remove_copy(orig.begin(), orig.end(), std::back_inserter(out2), 3);
    // out2 = {1, 2, 4, 5}
    std::cout << "After remove_copy(orig, 3): ";
    for (int x : out2) std::cout << x << " ";
    std::cout << "\n";

    return 0;
}
```

---

### 2.7 `unique` / `unique_copy`

* **`unique(ForwardIt first, ForwardIt last)`** — removes consecutive duplicates in-place (returns new “end”).
* **`unique_copy(InputIt first, InputIt last, OutputIt d_first)`** — copies elements to `d_first`, omitting all but the first of each group of consecutive duplicates.

**Example**

```cpp
#include <algorithm>
#include <iostream>
#include <vector>

int main() {
    std::vector<int> v = {1, 1, 2, 2, 2, 3, 3, 4};

    // 1) unique in-place
    auto new_end = std::unique(v.begin(), v.end());
    v.erase(new_end, v.end());
    std::cout << "After unique: ";
    for (int x : v) std::cout << x << " ";
    std::cout << "\n";
    // v = {1, 2, 3, 4}

    // 2) unique_copy
    std::vector<int> v2 = {1, 1, 2, 2, 3, 3, 3, 4};
    std::vector<int> out;
    std::unique_copy(v2.begin(), v2.end(), std::back_inserter(out));
    std::cout << "After unique_copy: ";
    for (int x : out) std::cout << x << " ";
    std::cout << "\n";
    // out = {1, 2, 3, 4}

    return 0;
}
```

---

### 2.8 `reverse` / `reverse_copy` / `rotate` / `rotate_copy` / `shuffle`

* **`reverse(BidirectionalIt first, BidirectionalIt last)`** — reverses the order of elements in `[first, last)`.
* **`reverse_copy(BidirectionalIt first, BidirectionalIt last, OutputIt d_first)`** — writes reversed sequence to `d_first`.
* **`rotate(ForwardIt first, ForwardIt n_first, ForwardIt last)`** — rotates the range so that `n_first` becomes the new `first`.
* **`rotate_copy(ForwardIt first, ForwardIt n_first, ForwardIt last, OutputIt d_first)`** — writes the rotated result to `d_first`.
* **`shuffle(RandomIt first, RandomIt last, URBG&& g)`** (since C++11) — randomly shuffles elements in `[first, last)` using uniform random bit generator `g`.

  > **Note**: `random_shuffle` was deprecated in C++14 and removed in C++17; use `shuffle` instead.

**Example**

```cpp
#include <algorithm>
#include <iostream>
#include <random>
#include <vector>

int main() {
    std::vector<int> v = {1, 2, 3, 4, 5};

    // reverse
    std::reverse(v.begin(), v.end());
    // v = {5, 4, 3, 2, 1}
    std::cout << "Reversed: ";
    for (int x : v) std::cout << x << " ";
    std::cout << "\n";

    // reverse_copy
    std::vector<int> rev_copy(5);
    std::reverse_copy(v.begin(), v.end(), rev_copy.begin());
    // rev_copy = {1, 2, 3, 4, 5}
    std::cout << "Reverse copy: ";
    for (int x : rev_copy) std::cout << x << " ";
    std::cout << "\n";

    // rotate: bring element at index 2 (value 3) to front
    std::rotate(v.begin(), v.begin() + 2, v.end());
    // v = {3, 2, 1, 5, 4}
    std::cout << "Rotated: ";
    for (int x : v) std::cout << x << " ";
    std::cout << "\n";

    // rotate_copy
    std::vector<int> rot_copy(5);
    std::rotate_copy(v.begin(), v.begin() + 2, v.end(), rot_copy.begin());
    // rot_copy is same as rotating 'v' a second time
    std::cout << "Rotate copy: ";
    for (int x : rot_copy) std::cout << x << " ";
    std::cout << "\n";

    // shuffle
    std::mt19937 rng(2025);
    std::shuffle(v.begin(), v.end(), rng);
    std::cout << "Shuffled: ";
    for (int x : v) std::cout << x << " ";
    std::cout << "\n";

    return 0;
}
```

---

### 2.9 `partition` / `stable_partition` / `partition_copy` / `partition_point` / `is_partitioned`

* **`partition(ForwardIt first, ForwardIt last, UnaryPredicate p)`** — reorders elements so that those satisfying `p` precede those that do not; returns iterator to the first element of the “second group.”
* **`stable_partition(ForwardIt first, ForwardIt last, UnaryPredicate p)`** — same as `partition`, but maintains relative order within each group.
* **`partition_copy(InputIt first, InputIt last, ForwardIt1 d_first_true, ForwardIt2 d_first_false, UnaryPredicate p)`** — copies elements into two separate output ranges: true-group and false-group. Returns a pair of iterators to the end of each output.
* **`partition_point(ForwardIt first, ForwardIt last, UnaryPredicate p)`** (since C++11) — given a range partitioned by `p`, returns the first element in the “second group.”
* **`is_partitioned(ForwardIt first, ForwardIt last, UnaryPredicate p)`** (since C++20) — returns `true` if all elements satisfying `p` precede all that do not.

**Example**

```cpp
#include <algorithm>
#include <iostream>
#include <vector>

bool is_even(int x) { return x % 2 == 0; }

int main() {
    std::vector<int> v = {3, 8, 1, 6, 5, 4, 7, 2};

    // 1) partition (not stable)
    auto mid = std::partition(v.begin(), v.end(), is_even);
    std::cout << "After partition: ";
    for (int x : v) std::cout << x << " ";
    std::cout << "\nFirst odd index: " << std::distance(v.begin(), mid) << "\n";

    // Check if partitioned
    std::cout << "Is partitioned? " << (std::is_partitioned(v.begin(), v.end(), is_even) ? "Yes" : "No") << "\n";

    // 2) stable_partition
    std::vector<int> w = {3, 8, 1, 6, 5, 4, 7, 2};
    auto mid2 = std::stable_partition(w.begin(), w.end(), is_even);
    std::cout << "After stable_partition: ";
    for (int x : w) std::cout << x << " ";
    std::cout << "\nFirst odd index: " << std::distance(w.begin(), mid2) << "\n";

    // 3) partition_copy
    std::vector<int> trues, falses;
    trues.reserve(w.size());
    falses.reserve(w.size());
    auto [t_it, f_it] = std::partition_copy(
        w.begin(), w.end(),
        std::back_inserter(trues),
        std::back_inserter(falses),
        is_even
    );
    std::cout << "Partition copy - evens: ";
    for (int x : trues) std::cout << x << " ";
    std::cout << "\nPartition copy - odds: ";
    for (int x : falses) std::cout << x << " ";
    std::cout << "\n";

    // 4) partition_point (after partitioning)
    auto it_pt = std::partition_point(v.begin(), v.end(), is_even);
    std::cout << "partition_point: first odd is at index " << std::distance(v.begin(), it_pt) << "\n";

    return 0;
}
```

---

## 3. Heap Operations

These algorithms treat a random-access range as a binary heap (max-heap by default).

### 3.1 `make_heap` / `push_heap` / `pop_heap` / `sort_heap`

* **`make_heap(RandomIt first, RandomIt last)`** — transforms `[first, last)` into a valid max-heap.
* **`push_heap(RandomIt first, RandomIt last)`** — assumes `[first, last−1)` is a heap; extends the heap to include `*(last−1)`.
* **`pop_heap(RandomIt first, RandomIt last)`** — swaps `*first` (max) with `*(last−1)`, then re-heapifies `[first, last−1)`.
* **`sort_heap(RandomIt first, RandomIt last)`** — repeatedly calls `pop_heap` to sort the entire heap in ascending order.

**Example**

```cpp
#include <algorithm>
#include <iostream>
#include <vector>

int main() {
    std::vector<int> v = {10, 20, 5, 15, 30};

    // 1) build a max-heap
    std::make_heap(v.begin(), v.end());
    std::cout << "After make_heap: ";
    for (int x : v) std::cout << x << " ";
    std::cout << "\n";

    // 2) push_heap: add a new element and restore heap
    v.push_back(25);
    std::push_heap(v.begin(), v.end());
    std::cout << "After push_heap(25): ";
    for (int x : v) std::cout << x << " ";
    std::cout << "\n";

    // 3) pop_heap: move max to end and re-heapify
    std::pop_heap(v.begin(), v.end());
    int max_val = v.back();
    v.pop_back();
    std::cout << "Popped max: " << max_val << "\nHeap now: ";
    for (int x : v) std::cout << x << " ";
    std::cout << "\n";

    // 4) sort_heap: sort the remaining elements
    std::sort_heap(v.begin(), v.end());
    std::cout << "After sort_heap: ";
    for (int x : v) std::cout << x << " ";
    std::cout << "\n";

    return 0;
}
```

---

### 3.2 `is_heap` / `is_heap_until`

* **`is_heap(RandomIt first, RandomIt last)`** — returns `true` if `[first, last)` satisfies max-heap property.
* **`is_heap_until(RandomIt first, RandomIt last)`** — returns the largest sorted prefix that is a heap (iterator to first element that violates heap property).

**Example**

```cpp
#include <algorithm>
#include <iostream>
#include <vector>

int main() {
    std::vector<int> h1 = {50, 30, 40, 10, 20};
    std::vector<int> h2 = {50, 30, 60, 10, 20};

    std::cout << "h1 is heap? " << (std::is_heap(h1.begin(), h1.end()) ? "Yes" : "No") << "\n";
    std::cout << "h2 is heap? " << (std::is_heap(h2.begin(), h2.end()) ? "Yes" : "No") << "\n";

    auto it = std::is_heap_until(h2.begin(), h2.end());
    std::cout << "First violation at index " << std::distance(h2.begin(), it) << "\n";

    return 0;
}
```

---

## 4. Sorting and Related Operations

These algorithms reorder a range into (partial) sorted order.

### 4.1 `sort` / `stable_sort`

* **`sort(RandomIt first, RandomIt last)`** — sorts `[first, last)` in ascending order (introsort).
* **`stable_sort(RandomIt first, RandomIt last)`** — sorts while preserving relative order of equivalent elements.

**Example**

```cpp
#include <algorithm>
#include <iostream>
#include <vector>

int main() {
    std::vector<int> v = {5, 2, 8, 3, 1, 7};

    // 1) sort
    std::sort(v.begin(), v.end());
    std::cout << "Sorted: ";
    for (int x : v) std::cout << x << " ";
    std::cout << "\n";

    // 2) stable_sort with custom comparator (descending)
    std::vector<int> w = {5, 2, 8, 3, 1, 7};
    std::stable_sort(w.begin(), w.end(), [](int a, int b) {
        return a > b;
    });
    std::cout << "Stable-sorted (descending): ";
    for (int x : w) std::cout << x << " ";
    std::cout << "\n";

    return 0;
}
```

---

### 4.2 `partial_sort` / `partial_sort_copy` / `nth_element`

* **`partial_sort(RandomIt first, RandomIt middle, RandomIt last)`** — rearranges elements so that `[first, middle)` are the smallest elements in sorted order; the rest are left in unspecified order.
* **`partial_sort_copy(InputIt first, InputIt last, RandomIt d_first, RandomIt d_last)`** — copies and sorts the smallest `min(last-first, d_last-d_first)` elements into `[d_first, …)`.
* **`nth_element(RandomIt first, RandomIt nth, RandomIt last)`** — rearranges so that the element at `nth` is the same element that would be there in a full sort, with all elements before it ≤ it and after it ≥ it (no full ordering guaranteed on either side).

**Example**

```cpp
#include <algorithm>
#include <iostream>
#include <vector>

int main() {
    std::vector<int> v = {7, 2, 9, 4, 6, 5, 3, 1, 8};

    // 1) partial_sort: get the 3 smallest elements sorted at front
    std::partial_sort(v.begin(), v.begin() + 3, v.end());
    std::cout << "After partial_sort (3 smallest at front): ";
    for (int x : v) std::cout << x << " ";
    std::cout << "\n";

    // 2) partial_sort_copy: copy top-5 smallest into new vector
    std::vector<int> out(5);
    std::partial_sort_copy(v.begin(), v.end(), out.begin(), out.end());
    std::cout << "partial_sort_copy (5 smallest): ";
    for (int x : out) std::cout << x << " ";
    std::cout << "\n";

    // 3) nth_element: find median (position 4 for 9 elements)
    std::nth_element(v.begin(), v.begin() + 4, v.end());
    std::cout << "Element at index 4 after nth_element: " << v[4] << "\n";
    // All elements before index 4 are ≤ v[4], after are ≥ v[4].

    return 0;
}
```

---

## 5. Binary Search and Related Operations

These assume a sorted range and perform logarithmic-time searches.

### 5.1 `lower_bound` / `upper_bound` / `binary_search` / `equal_range`

* **`lower_bound(ForwardIt first, ForwardIt last, const T& value)`** — returns iterator to the first element ≥ `value`.
* **`upper_bound(ForwardIt first, ForwardIt last, const T& value)`** — returns iterator to the first element > `value`.
* **`binary_search(ForwardIt first, ForwardIt last, const T& value)`** — returns `true` if `value` exists in the sorted range.
* **`equal_range(ForwardIt first, ForwardIt last, const T& value)`** — returns a pair of iterators delimiting the subrange of elements equal to `value` (i.e., `[lower_bound, upper_bound)`).

**Example**

```cpp
#include <algorithm>
#include <iostream>
#include <vector>

int main() {
    std::vector<int> v = {1, 2, 2, 3, 4, 4, 4, 5};

    // find lower_bound of 4
    auto lb = std::lower_bound(v.begin(), v.end(), 4);
    std::cout << "lower_bound(4) index: " << std::distance(v.begin(), lb) << "\n";

    // find upper_bound of 4
    auto ub = std::upper_bound(v.begin(), v.end(), 4);
    std::cout << "upper_bound(4) index: " << std::distance(v.begin(), ub) << "\n";

    // binary_search for 3
    bool found3 = std::binary_search(v.begin(), v.end(), 3);
    std::cout << "3 found? " << (found3 ? "Yes" : "No") << "\n";

    // equal_range for 2
    auto range2 = std::equal_range(v.begin(), v.end(), 2);
    std::cout << "equal_range(2): indices [" 
              << std::distance(v.begin(), range2.first) << ", "
              << std::distance(v.begin(), range2.second) << ")\n";

    return 0;
}
```

---

## 6. Merging and Set Operations

These operate on two sorted ranges and produce a sorted output or query set-relationships.

### 6.1 `merge` / `inplace_merge`

* **`merge(InputIt1 first1, InputIt1 last1, InputIt2 first2, InputIt2 last2, OutputIt d_first)`** — merges two sorted ranges into a new sorted output starting at `d_first`.
* **`inplace_merge(BidirIt first, BidirIt middle, BidirIt last)`** — merges two consecutive sorted subranges `[first, middle)` and `[middle, last)` in place.

**Example**

```cpp
#include <algorithm>
#include <iostream>
#include <vector>

int main() {
    std::vector<int> a = {1, 3, 5, 7};
    std::vector<int> b = {2, 4, 6, 8};
    std::vector<int> dest(a.size() + b.size());

    // 1) merge into a separate vector
    std::merge(a.begin(), a.end(), b.begin(), b.end(), dest.begin());
    std::cout << "Merged result: ";
    for (int x : dest) std::cout << x << " ";
    std::cout << "\n";

    // 2) inplace_merge: two sorted halves in one vector
    std::vector<int> c = {1, 3, 5, 7, 2, 4, 6, 8};
    // assume [c.begin(), c.begin()+4) and [c.begin()+4, c.end()) are each sorted
    std::inplace_merge(c.begin(), c.begin() + 4, c.end());
    std::cout << "After inplace_merge: ";
    for (int x : c) std::cout << x << " ";
    std::cout << "\n";

    return 0;
}
```

---

### 6.2 `includes` / `set_union` / `set_intersection` / `set_difference` / `set_symmetric_difference`

* **`includes(InputIt1 first1, InputIt1 last1, InputIt2 first2, InputIt2 last2)`** — returns `true` if every element in `[first2, last2)` is contained (in order) within `[first1, last1)`.
* **`set_union(InputIt1 first1, InputIt1 last1, InputIt2 first2, InputIt2 last2, OutputIt d_first)`** — computes the union of two sorted ranges into `d_first`.
* **`set_intersection(InputIt1 first1, InputIt1 last1, InputIt2 first2, InputIt2 last2, OutputIt d_first)`** — computes the intersection.
* **`set_difference(InputIt1 first1, InputIt1 last1, InputIt2 first2, InputIt2 last2, OutputIt d_first)`** — computes elements in first range not in second.
* **`set_symmetric_difference(InputIt1 first1, InputIt1 last1, InputIt2 first2, InputIt2 last2, OutputIt d_first)`** — computes elements that are in either range, but not both.

**Example**

```cpp
#include <algorithm>
#include <iostream>
#include <vector>

int main() {
    std::vector<int> A = {1, 2, 3, 5, 7};
    std::vector<int> B = {2, 3, 4, 6, 7};

    // 1) includes?
    bool all_in = std::includes(A.begin(), A.end(), B.begin(), B.end());
    std::cout << "A includes B? " << (all_in ? "Yes" : "No") << "\n";

    // 2) set_union
    std::vector<int> uni;
    uni.reserve(A.size() + B.size());
    std::set_union(A.begin(), A.end(), B.begin(), B.end(), std::back_inserter(uni));
    std::cout << "Union: ";
    for (int x : uni) std::cout << x << " ";
    std::cout << "\n";

    // 3) set_intersection
    std::vector<int> inter;
    inter.reserve(std::min(A.size(), B.size()));
    std::set_intersection(A.begin(), A.end(), B.begin(), B.end(), std::back_inserter(inter));
    std::cout << "Intersection: ";
    for (int x : inter) std::cout << x << " ";
    std::cout << "\n";

    // 4) set_difference (A − B)
    std::vector<int> diff;
    diff.reserve(A.size());
    std::set_difference(A.begin(), A.end(), B.begin(), B.end(), std::back_inserter(diff));
    std::cout << "A \\ B: ";
    for (int x : diff) std::cout << x << " ";
    std::cout << "\n";

    // 5) set_symmetric_difference
    std::vector<int> sym_diff;
    sym_diff.reserve(A.size() + B.size());
    std::set_symmetric_difference(A.begin(), A.end(), B.begin(), B.end(), std::back_inserter(sym_diff));
    std::cout << "Symmetric difference: ";
    for (int x : sym_diff) std::cout << x << " ";
    std::cout << "\n";

    return 0;
}
```

---

## 7. Min/Max and Related

### 7.1 `min` / `max` / `minmax`

* **`min(const T& a, const T& b)`** — returns the smaller of `a` and `b`. There are overloads for initializer-lists, ranges, and custom comparators.
* **`max(const T& a, const T& b)`** — returns the larger of `a` and `b`. Similar overloads.
* **`minmax(const T& a, const T& b)`** (since C++11) — returns a `pair<T, T>` where `.first` is the minimum and `.second` is the maximum.

**Example**

```cpp
#include <algorithm>
#include <iostream>
#include <utility> // for std::pair

int main() {
    int a = 10, b = 20;
    std::cout << "min(10,20): " << std::min(a, b) << "\n";
    std::cout << "max(10,20): " << std::max(a, b) << "\n";

    auto [mn, mx] = std::minmax(a, b);
    std::cout << "minmax(10,20): (" << mn << ", " << mx << ")\n";

    return 0;
}
```

---

### 7.2 `min_element` / `max_element` / `minmax_element` / `clamp`

* **`min_element(ForwardIt first, ForwardIt last)`** — returns iterator to the smallest element in `[first, last)`.
* **`max_element(ForwardIt first, ForwardIt last)`** — returns iterator to the largest element.
* **`minmax_element(ForwardIt first, ForwardIt last)`** (since C++11) — returns a pair of iterators—first to the minimum, second to the maximum element.
* **`clamp(const T& v, const T& lo, const T& hi)`** (since C++17) — returns `lo` if `v < lo`, `hi` if `v > hi`, otherwise returns `v` (with an overload accepting a custom comparator).

**Example**

```cpp
#include <algorithm>
#include <iostream>
#include <vector>
#include <tuple> // for structured bindings

int main() {
    std::vector<int> v = {15, 3, 27, 7, 19};

    // min_element / max_element
    auto it_min = std::min_element(v.begin(), v.end());
    auto it_max = std::max_element(v.begin(), v.end());
    std::cout << "Min element: " << *it_min << "\n";
    std::cout << "Max element: " << *it_max << "\n";

    // minmax_element
    auto [it1, it2] = std::minmax_element(v.begin(), v.end());
    std::cout << "minmax_element: (" << *it1 << ", " << *it2 << ")\n";

    // clamp
    int x = 50;
    int y = std::clamp(x, 0, 30);
    std::cout << "clamp(50, 0, 30): " << y << "  (since 50 > 30, returns 30)\n";

    return 0;
}
```

---

## 8. Permutations and Combinations

### 8.1 `next_permutation` / `prev_permutation` / `lexicographical_compare`

* **`next_permutation(BidirIt first, BidirIt last)`** — transforms the range into the next lexicographically greater permutation; returns `true` if successful, `false` if it was the last permutation (and transforms into the first).
* **`prev_permutation(BidirIt first, BidirIt last)`** — transforms into the previous lexicographically ordered permutation.
* **`lexicographical_compare(InputIt1 first1, InputIt1 last1, InputIt2 first2, InputIt2 last2)`** — returns `true` if the first range is lexicographically less than the second (element-by-element comparison).

**Example**

```cpp
#include <algorithm>
#include <iostream>
#include <vector>

int main() {
    std::vector<int> v = {1, 2, 3};

    // 1) next_permutation
    std::cout << "Permutations of {1,2,3}:\n";
    do {
        for (int x : v) std::cout << x;
        std::cout << "\n";
    } while (std::next_permutation(v.begin(), v.end()));
    // After the last permutation, v resets to {1,2,3}

    // 2) prev_permutation
    std::cout << "Previous permutation of {1,2,3} (wraps to last): ";
    bool has_prev = std::prev_permutation(v.begin(), v.end());
    // Since v is back to {1,2,3}, prev_permutation yields {3,2,1}
    if (has_prev) {
        for (int x : v) std::cout << x;
    }
    std::cout << "\n";

    // 3) lexicographical_compare
    std::vector<char> a = {'a', 'b', 'c'};
    std::vector<char> b = {'a', 'b', 'd'};
    bool lex_less = std::lexicographical_compare(a.begin(), a.end(), b.begin(), b.end());
    std::cout << "{a,b,c} < {a,b,d}? " << (lex_less ? "Yes" : "No") << "\n";

    return 0;
}
```

> **Note:** The standard `<algorithm>` does **not** provide `next_combination` or `prev_combination`; those must be implemented by the user or obtained from a non‐standard library.

---

## 9. Miscellaneous Utilities

### 9.1 `iota`

* **`iota(ForwardIt first, ForwardIt last, T value)`** (since C++11) — assigns sequentially increasing values starting from `value` to each element in `[first, last)`.

**Example**

```cpp
#include <algorithm>
#include <iostream>
#include <vector>

int main() {
    std::vector<int> v(5);
    std::iota(v.begin(), v.end(), 10);
    // v = {10, 11, 12, 13, 14}
    std::cout << "After iota starting at 10: ";
    for (int x : v) std::cout << x << " ";
    std::cout << "\n";

    return 0;
}
```

---

### 9.2 `is_sorted` / `is_sorted_until`

* **`is_sorted(ForwardIt first, ForwardIt last)`** (since C++11) — returns `true` if `[first, last)` is in non-decreasing order.
* **`is_sorted_until(ForwardIt first, ForwardIt last)`** (since C++11) — returns the first iterator that breaks the sorting order (i.e., first element where `*it < *(it−1)`).

**Example**

```cpp
#include <algorithm>
#include <iostream>
#include <vector>

int main() {
    std::vector<int> a = {1, 2, 3, 5, 4, 6};
    bool sorted_a = std::is_sorted(a.begin(), a.end());
    std::cout << "a is sorted? " << (sorted_a ? "Yes" : "No") << "\n";

    auto it_bad = std::is_sorted_until(a.begin(), a.end());
    std::cout << "First unsorted element at index " << std::distance(a.begin(), it_bad) 
              << " (value " << *it_bad << ")\n";

    std::vector<int> b = {1, 2, 3, 4, 5};
    std::cout << "b is sorted? " << (std::is_sorted(b.begin(), b.end()) ? "Yes" : "No") << "\n";

    return 0;
}
```

---

### 9.3 `all_of` / `any_of` / `none_of`

* **`all_of(InputIt first, InputIt last, UnaryPredicate p)`** (since C++11) — returns `true` if **all** elements satisfy `p`.
* **`any_of(InputIt first, InputIt last, UnaryPredicate p)`** (since C++11) — returns `true` if **any** element satisfies `p`.
* **`none_of(InputIt first, InputIt last, UnaryPredicate p)`** (since C++11) — returns `true` if **none** of the elements satisfy `p`.

**Example**

```cpp
#include <algorithm>
#include <iostream>
#include <vector>

bool is_positive(int x) { return x > 0; }

int main() {
    std::vector<int> v = {1, 2, 3, -4, 5};

    std::cout << "All positive? " << (std::all_of(v.begin(), v.end(), is_positive) ? "Yes" : "No") << "\n";
    std::cout << "Any negative? " << (std::any_of(v.begin(), v.end(), [](int x){ return x < 0; }) ? "Yes" : "No") << "\n";
    std::cout << "None zero? " << (std::none_of(v.begin(), v.end(), [](int x){ return x == 0; }) ? "Yes" : "No") << "\n";

    return 0;
}
```

---

### 9.4 `is_permutation`

* **`is_permutation(ForwardIt1 first1, ForwardIt1 last1, ForwardIt2 first2)`** — returns `true` if the range `[first1, last1)` is a permutation of the range starting at `first2` (same length). Overloads allow supplying an end iterator or a custom predicate.

**Example**

```cpp
#include <algorithm>
#include <iostream>
#include <vector>

int main() {
    std::vector<int> a = {1, 3, 2, 4};
    std::vector<int> b = {4, 2, 1, 3};

    std::cout << "a is a permutation of b? "
              << (std::is_permutation(a.begin(), a.end(), b.begin()) ? "Yes" : "No") << "\n";

    std::vector<int> c = {1, 2, 2, 3};
    std::vector<int> d = {2, 1, 3, 2};

    std::cout << "c is a permutation of d? "
              << (std::is_permutation(c.begin(), c.end(), d.begin()) ? "Yes" : "No") << "\n";

    return 0;
}
```

---

### 9.5 `sample`

* **`sample(InputIt first, InputIt last, SampleOutputIterator out, typename iterator_traits<InputIt>::difference_type n, URBG&& g)`** (since C++17) — selects `n` random elements from `[first, last)` without replacement, writing to the output iterator `out`, using uniform random bit generator `g`.

**Example**

```cpp
#include <algorithm>
#include <iostream>
#include <vector>
#include <random>

int main() {
    std::vector<int> v = {10, 20, 30, 40, 50, 60};
    std::vector<int> result;
    result.reserve(3);

    std::mt19937 rng(42); // deterministic seed
    std::sample(v.begin(), v.end(), std::back_inserter(result), 3, rng);

    std::cout << "Random sample of 3 elements: ";
    for (int x : result) std::cout << x << " ";
    std::cout << "\n";

    return 0;
}
```

---

## 10. Deprecated or Removed

* **`random_shuffle`**

  ```cpp
  template<class RandomIt>
  void random_shuffle(RandomIt first, RandomIt last);
  template<class RandomIt, class RandomFunc>
  void random_shuffle(RandomIt first, RandomIt last, RandomFunc&& r);
  ```

  * Removed in C++17. Use `std::shuffle` instead.

---

## 11. Flat List of All `<algorithm>` Functions

Below is a concise list of all functions (names only), including overloads for ranges, predicates, and (when applicable) custom comparators. Execution-policy overloads exist for most sequence and sorting algorithms (omitted here for brevity).

```
for_each
find
find_if
find_if_not
find_end
find_first_of
adjacent_find
count
count_if
mismatch
equal
search
search_n

copy
copy_if
copy_n
copy_backward
move
move_backward
swap_ranges
iter_swap
transform

replace
replace_if
replace_copy
replace_copy_if

fill
fill_n
generate
generate_n

remove
remove_if
remove_copy
remove_copy_if

unique
unique_copy

reverse
reverse_copy

rotate
rotate_copy

shuffle

partition
stable_partition
partition_copy
partition_point
is_partitioned

make_heap
push_heap
pop_heap
sort_heap
is_heap
is_heap_until

sort
stable_sort
partial_sort
partial_sort_copy
nth_element

lower_bound
upper_bound
binary_search
equal_range

merge
inplace_merge
includes
set_union
set_intersection
set_difference
set_symmetric_difference

min
max
minmax
min_element
max_element
minmax_element
clamp

next_permutation
prev_permutation
lexicographical_compare
lexicographical_compare_three_way

is_permutation

is_sorted
is_sorted_until

all_of
any_of
none_of

iota

sample
```

---
