---
layout: post
title: C++ Tips
categories:
- Cheat sheets
tags:
- C++
---
{% raw %}

## TOC

|  |  |
| -------- | ------- |
| <a href="#reduce", target="_self"><code>std::reduce</code></a> | [CPP Reference](https://en.cppreference.com/w/cpp/algorithm/reduce) |
| <a href="#max_element", target="_self"><code>std::max_element</code></a> | [CPP Reference](https://en.cppreference.com/w/cpp/algorithm/max_element) |
| <a href="#upper_bound", target="_self"><code>std::upper_bound</code></a> | [CPP Reference](https://en.cppreference.com/w/cpp/algorithm/upper_bound) |
| <a href="#lower_bound", target="_self"><code>std::lower_bound</code></a> | [CPP Reference](https://en.cppreference.com/w/cpp/algorithm/lower_bound) |
| <a href="#equal_range", target="_self"><code>std::equal_range</code></a> | [CPP Reference](https://en.cppreference.com/w/cpp/algorithm/equal_range) |

## Miscellaneous

- `calloc` takes two arguments and initializes the allocated memory block to zero: `int *arr = (int*)calloc(n, sizeof(int));`
- 

## STL

<a name="reduce"></a>
- To sum: `std::reduce(container.begin(), container.end)`.
  - Python equivalent: `sum(container)`.
- Customize the reduction operator: `auto multiply = [](int x, int y) { return x * y; }; int product = std::reduce(container.begin(), container.end(), 1, multiply);`
  - Python equivalent: `functools.reduce(lambda a, b: a*b, container, 1)`
<a name="max_element"></a>
- Find the maximum element: `std::max_element(container.begin(), container.end)`
  - Python equivalent: `max(container)`
<a name="upper_bound"></a>
<a name="lower_bound"></a>
<a name="equal_range"></a>
- Binary search: [`std::lower_bound`](https://en.cppreference.com/w/cpp/algorithm/lower_bound), [`std::upper_bound`](https://en.cppreference.com/w/cpp/algorithm/upper_bound) or [`std::equal_range`](https://en.cppreference.com/w/cpp/algorithm/equal_range). The container must be sorted. <br>`lower_bound` and `upper_bound` return the iterator. `equal_range` returns a pair of iterators. <br>`lower_bound` returns the first element that **not less** than the query and `upper_bound` returns the first element that **greater** than the query.
  - Python equivalent: `lower_bound = bisect.bisect_left(container, x); upper_bound = bisect.bisect_right(container, x)`
  - To get all the elements in a range: `[i for i in container if x >= a && x <= b]`

{% endraw %}