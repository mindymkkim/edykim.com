---
title: "CS11 Intro C++: Lecture 2"
author: haruair
type: note
date: "2020-05-18T11:28:00"
lang: en
url: /note/cs-11-intro-cpp/lecture-2
linkToParent: true
---

## Compilation

```bash
> g++ -Wall -Werror units.cpp convert.cpp -o convert
```

1. Compiler performs preprocessing and compilation on `units.cpp` and `convert.cpp` separately: Produces `units.o`, `convert.o`
1. The linking phase combines the results of the compilation phase: `units.o` + `convert.o` = `convert`.

## Build Phases

```cpp
# compile source without linking it: `units.o`
> g++ -Wall -Werror -c units.cpp
# output files from each phases: `units.ii`, `units.s`
> g++ -Wall --save-temps -c units.cpp
```

### The Preprocessor

- Performs text-processing operations on each source file
  - Remove all comments
  - Handle **preprocessor directives** such as `#include`, `#define`
- Generate **translation unit** which compiler actually compiles

### The Compiler

- Using translation units, translates it to machine code from C++ code.
- Result is called an **object file**. (relocatable object file).
  - e.g. `units.o`, `convert.o`
  - It's a machine-code instruction but not a runnable program yet.
  - Some obj files doesn't contain definitions of the functions

### The Linker

- Combine the object files that generated by the compiler.
- Make sure every functions is defined in some obj files.

## `std::vector<T>`

`vector` is dynamically-resizeable, growable array.

```cpp
vector<int> v1;
vector<string> v2(10);
```

```cpp
#include <vector>
vector<int> v;
v.push_back(15);
v.push_back(42);
v.push_back(-9);

cout << "Number of elements: " << v.size() << "\n";
for (int i = 0; i < v.size(); ++i)
  cout << "v[" << i << "] = " << v[i] << "\n";

for (int n : v) // C++11 range-based for loop
  cout << " " << n;
```

## Structs

- Struct members are public by default; class members are private by default
- Generally, it used when full functionality of classes isn't required

```cpp
struct TodoItem { // these members are public
  int id;
  string description;
  bool completed;
};

class TodoList {
  int next_id;
  vector<TodoItem> items;
public:
  int add_task(string description);
  void complete_task(int task_id);
};
```

C++ allows class/struct declarations to be nested.

```cpp
class TodoList {
  struct TodoItem {
    // ...
  }
  // ...
}
// TodoItem type isn't visible from the outside of TodoList
```

Intialize with values:
```cpp
TodoItem i = {next_id, description, false};
```

## Exception Handling

`throw` and `catch` the exception. e.g. `out_of_range`, `regex_error`.

```cpp
double compute_value(double x) {
  if (x < 3.0)
    throw invalid_argument("x must be >= 3");
  return sqrt(x - 3.0);
}

try {
  double v = compute_value(1.0);
  cout << "Answer is "<< v << "\n";
}
catch (invalid_argument) {
  cout << "Error occurred!\n";
}

try {
  double v = compute_value(3.0);
  cout << "Answer is "<< v << "\n";
}
catch (invalid_argument e) {
  cout << "Error occurred!\n";
  cout << e.what() << "\n";
}
```

Exceptions are as much a part of a function's public interface, as the arguments and the return-value. _important_ to document what exceptions are thrown and the circumstances in which they are thrown.

```cpp
/**
 * ...
 * Throws invalid_argument if x< 3
 */
double compute_value(double x) {
  // ...
}
```

---

## Coming up next
{class="no-number"}

- [Lecture 3](/note/cs-11-intro-cpp/lecture-3): `fstream`, scope, pass by value/reference