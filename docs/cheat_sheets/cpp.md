---
layout: post
title: C++ Tips
categories:
- Cheat sheets
tags:
- C++
---
{% raw %}

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
- Binary search: [`std::lower_bound`](https://en.cppreference.com/w/cpp/algorithm/lower_bound), [`std::upper_bound`](https://en.cppreference.com/w/cpp/algorithm/upper_bound) or [`std::equal_range`](https://en.cppreference.com/w/cpp/algorithm/equal_range). `lower_bound` and `upper_bound` return the iterator. `equal_range` returns a pair of iterators.
  - Python equivalent to `lower_bound`: `container.index(x)`.
  - Python equivalent to `equal_range`: `[i for i in container if x == i]`.
  - or use the module `bisect`: `lower_bound = bisect.bisect_left(container, x); upper_bound = bisect.bisect_right(container, x)`

## TOC

|  |  |
| -------- | ------- |
| [`std::reduce`](#reduce) | [CPP Reference](https://en.cppreference.com/w/cpp/algorithm/reduce) |
| [`std::max_element`]() | [CPP Reference](https://en.cppreference.com/w/cpp/algorithm/max_element) |
| [`std::upper_bound`]() | [CPP Reference](https://en.cppreference.com/w/cpp/algorithm/upper_bound) |
| [`std::lower_bound`]() | [CPP Reference](https://en.cppreference.com/w/cpp/algorithm/lower_bound) |
| [`std::equal_range`]() | [CPP Reference](https://en.cppreference.com/w/cpp/algorithm/equal_range) |

{% endraw %}