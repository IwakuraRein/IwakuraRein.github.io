---
layout: post
title: C++ Tips
categories:
- Cheat sheets
tags:
- C++
---
{% raw %}

## Miscellaneous
|  |  |  |
| -------- | ------- | ------- |
| <a href="#const_cast" target="_self"><code>const_cast</code></a> | [CPP Reference](https://en.cppreference.com/w/cpp/language/const_cast) |  |
| <a href="#distance" target="_self"><code>std::distance</code></a> | [CPP Reference](https://en.cppreference.com/w/cpp/iterator/distance) | [CPlusPlus.com](https://cplusplus.com/reference/iterator/distance/) |

<a name="const_cast"></a>
- `const_cast` allows you to modify a constant pointer or reference. Noticed that it won't work for constant variables.
  ```cpp
  int i = 3;
  const int& rci = i;
  const_cast<int&>(rci) = 4;

  const int i = 5;
  const int* p = &i;
  *const_cast<int*>(p) = 6; // won't work
  ```
- Meyer's Singleton
  ```cpp
  class Singleton {
    public:
      static Singleton& Instance() {
        static Singleton instance;
        return instance;
      }
      Singleton(const Singleton&) = delete;
      Singleton(Singleton&&) = delete;
      Singleton& operator=(const Singleton&) = delete;
      Singleton& operator=(Singleton&&) = delete;
    private:
      Singleton() = default;
      ~Singleton() = default;
  };
  ```
<a name="distance"></a>
- `distance(first, second)` returns the number of increments for `first` to get to `second`. For example, `distance(container.begin(), container.end())` returns the contianer's size.

## Memory

|  |  |  |
| -------- | ------- | ------- |
| <a href="#calloc" target="_self"><code>calloc</code></a> | [CPP Reference](https://en.cppreference.com/w/c/memory/calloc) | [CPlusPlus.com](https://cplusplus.com/reference/cstdlib/calloc/) |
| <a href="#allocator" target="_self"><code>std::allocator</code></a> | [CPP Reference](https://en.cppreference.com/w/cpp/memory/allocator) | [CPlusPlus.com](https://cplusplus.com/reference/memory/allocator/) |

<a name="calloc"></a>
- `calloc` takes two arguments and initializes the allocated memory block to zero: `int *arr = (int*)calloc(n, sizeof(int));`
- **Placement new**: Explicitly create objects on a specifized memory block. `char* buffer = new char[sizeof(MyClass)]; MyClass* obj = new (buffer) MyClass();`<br>Object Pooling can be achieved with placement new.
<a name="allocator"></a>
- `allocator` is a wrapper for `malloc` and `free`.
  ```cpp
  MyClass* buffer = std::allocator.allocate(5);
  for (int i = 0; i < 5; ++i) {
    allocator.construct(&buffer[i], MyClass());
  }
  for (int i = 0; i < 5; ++i) {
    allocator.destroy(&buffer[i]);
  }
  allocator.deallocate(buffer, 5);
  ```

## STL

|  |  |  |
| -------- | ------- | ------- |
| <a href="#reduce" target="_self"><code>std::reduce</code></a> | [CPP Reference](https://en.cppreference.com/w/cpp/algorithm/reduce) |  |
| <a href="#max_element" target="_self"><code>std::max_element</code></a> | [CPP Reference](https://en.cppreference.com/w/cpp/algorithm/max_element) | [CPlusPlus.com](https://cplusplus.com/reference/algorithm/max_element/) |
| <a href="#upper_bound" target="_self"><code>std::upper_bound</code></a> | [CPP Reference](https://en.cppreference.com/w/cpp/algorithm/upper_bound) | [CPlusPlus.com](https://cplusplus.com/reference/algorithm/upper_bound/) |
| <a href="#lower_bound" target="_self"><code>std::lower_bound</code></a> | [CPP Reference](https://en.cppreference.com/w/cpp/algorithm/lower_bound) | [CPlusPlus.com](https://cplusplus.com/reference/algorithm/lower_bound/) |
| <a href="#equal_range" target="_self"><code>std::equal_range</code></a> | [CPP Reference](https://en.cppreference.com/w/cpp/algorithm/equal_range) | [CPlusPlus.com](https://cplusplus.com/reference/algorithm/equal_range/) |
| <a href="#reverse" target="_self"><code>std::reverse</code></a> | [CPP Reference](https://en.cppreference.com/w/cpp/algorithm/reverse) | [CPlusPlus.com](https://cplusplus.com/reference/algorithm/reverse/) |
| <a href="#partition" target="_self"><code>std::partition</code></a> | [CPP Reference](https://en.cppreference.com/w/cpp/algorithm/partition) | [CPlusPlus.com](https://cplusplus.com/reference/algorithm/partition/) |

<a name="reduce"></a>
- To sum: `reduce(container.begin(), container.end)`.
  - Python equivalent: `sum(container)`.
- Customize the reduction operator: `auto multiply = [](int x, int y) { return x * y; }; int product = reduce(container.begin(), container.end(), 1, multiply);`
  - Python equivalent: `functools.reduce(lambda a, b: a*b, container, 1)`
<a name="max_element"></a>
- Find the maximum element: `max_element(container.begin(), container.end)`
  - Python equivalent: `max(container)`
<a name="upper_bound"></a>
<a name="lower_bound"></a>
<a name="equal_range"></a>
- Binary search: [`lower_bound`](https://en.cppreference.com/w/cpp/algorithm/lower_bound), [`upper_bound`](https://en.cppreference.com/w/cpp/algorithm/upper_bound) or [`equal_range`](https://en.cppreference.com/w/cpp/algorithm/equal_range). The container must be sorted. <br>`lower_bound` and `upper_bound` return the iterator. `equal_range` returns a pair of iterators. <br>`lower_bound` returns the first element that **not less** than the query and `upper_bound` returns the first element that **greater** than the query.
  - Python equivalent: `lower_bound = bisect.bisect_left(container, x); upper_bound = bisect.bisect_right(container, x)`
  - `equal_range` equivalent: `[i for i in container if x >= a && x <= b]`
<a name="reverse"></a>
- To reverse: `reverse(container.begin(), container.end())`.
  - Python equivalent: `container[::-1]`
<a name="reverse"></a>
- `partition(firstIt, secondIt, unaryPredicate)` can divide the container into two groupds: those for which the predicate returns true and those for which it returns false. It returns the iterator to the first element of the second group. An good example of `partition` is Quick Sort.
  -  Python equivalent: there isn't one.

## Algorithms

### Prim

```cpp
void prim(vector<vector<ii>> const& adj, int s) {
  priority_queue<ii, vector<ii>, greater<ii>> pq;
  parents.resize(N);
  keys.resize(N, INT_MAX);
  keys[s] = 0;
  pq.emplace(0, s);

  while(!pq.empty()) {
    auto [d, u] = pq.top();
    pq.pop();

    if(d > keys[u])
      continue; //lazy deletion

    for(auto &[v,w]: adj[u]) {  
      if(w < keys[v]) { // note: difference here
        keys[v] = w;
        parents[v] = u;
        pq.emplace(keys[v], v);
      }
    }
  }
}
```

### Quick Sort

```cpp
vector<int> sortArray(vector<int>& nums) {
  quickSort(nums, 0, nums.size()-1);
  return nums;
}
void quickSort(vector<int>& nums, int left, int right)
{
  if (left >= right) return;
  if (left < 0) return;
  if (right >= nums.size()) return;

  int pivot = nums[(left+right) / 2];
  swap(nums[left], nums[(left+right) / 2]);
  int i = left;
  int j = right;
  while (true)
  {
    while (nums[j] >= pivot && j > i) j--;
    if (j == i) break;
    while (nums[i] <= pivot && j > i) i++;
    if (j == i) break;
    swap(nums[i], nums[j]);
  }
  swap(nums[left], nums[i]);
  quickSort(nums, left, i-1);
  quickSort(nums, i+1, right);
}
```

### Quick Select

```cpp
```

{% endraw %}