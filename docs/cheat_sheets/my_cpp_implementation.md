---
layout: post
title: My C++ Implementations of Some Data Structures
categories:
- Cheat sheets
tags:
- C++
---
{% raw %}

## string

```cpp
#include <iostream>
#include <exception>
#include <cstring>

class string
{
    char* _buf;
    size_t _size;
public:
    string()
    {
        _size = 0;
        _buf = new char('\0');
    }
    string(const char str[])
    {
        if (str)
        {
            for (_size = 0; str[_size] != '\0'; ++_size);
            _buf = new char[_size + 1];
            memcpy(_buf, str, _size + 1);
        }
        else
        {
            _size = 0;
            _buf = new char('\0');
        }
    }
    string(const string& other)
    {
        _size = other._size;
        _buf = new char[_size + 1];
        memcpy(_buf, other._buf, _size + 1);
    }
    string(string&& other) : _size { other._size }, _buf { other._buf } {}
    ~string()
    {
        delete[] _buf;
    }
    const char* c_str() const
    {
        return _buf;
    }
    char& operator[] (size_t idx) 
    { 
        if (idx < _size) return _buf[idx];
        else throw std::out_of_range("string index out of bounds.");
    }
    char const& operator[] (size_t idx) const
    {
        if (idx < _size) return _buf[idx];
        else throw std::out_of_range("string index out of bounds.");
    }
    string operator+ (const string& other) const
    {
        size_t size = _size + other._size;
        char* buf = new char[size + 1];
        memcpy(buf, _buf, _size);
        memcpy(&buf[_size], other._buf, other._size+1);
        return string(size, buf);
    }
    string& operator+= (const string& other)
    {
        if (other._size > 0)
        {
            char* new_buf = new char[_size + other._size + 1];
            memcpy(new_buf, _buf, _size);
            memcpy(&new_buf[_size], other._buf, other._size + 1);
            _size += other._size;
            delete[] _buf;
            _buf = new_buf;
        }
        return *this;
    }
    string operator+ (const char str[]) const
    {
        size_t other_size = 0;
        for (; str[other_size] != '\0'; ++other_size);
        size_t size = _size + other_size;
        char* buf = new char[size + 1];
        memcpy(buf, _buf, _size);
        memcpy(&buf[_size], str, other_size + 1);
        return string(size, buf);
    }
    string& operator+= (const char str[])
    {
        size_t other_size = 0;
        for (; str[other_size] != '\0'; ++other_size);
        if (other_size > 0)
        {
            char* new_buf = new char[_size + other_size + 1];
            memcpy(new_buf, _buf, _size);
            memcpy(&new_buf[_size], str, other_size + 1);
            _size += other_size;
            delete[] _buf;
            _buf = new_buf;
        }
        return *this;
    }
    size_t size() const { return _size; }
    string sub_str(const size_t& start_idx)
    {
        if (start_idx > _size) throw std::out_of_range("string index out of bounds.");
        size_t size = _size - start_idx;
        char* buf = new char[size + 1];
        memcpy(buf, &_buf[start_idx], size);
        return string(size, buf);
    }
    string sub_str(const size_t& start_idx, size_t size)
    {
        if (size == 0)
        {
            return string();
        }
        if (start_idx + size > _size) throw std::out_of_range("string index out of bounds.");
        char* buf = new char[size + 1];
        memcpy(buf, &_buf[start_idx], size);
        return string(size, buf);
    }
private:
    string(const size_t& size, char* const & buf) : _size{ size }, _buf{ buf } { }
};
```

## Hash Table

```cpp
template<typename keyT, typename valT>
class hash_table
{
	static constexpr size_t _primes[62]
	{
		5ull,
		11ull,
		23ull,
		47ull,
		97ull,
		199ull,
		409ull,
		823ull,
		1741ull,
		3469ull,
		6949ull,
		14033ull,
		28411ull,
		57557ull,
		116731ull,
		236897ull,
		480881ull,
		976369ull,
		1982627ull,
		4026031ull,
		8175383ull,
		16601593ull,
		33712729ull,
		68460391ull,
		139022417ull,
		282312799ull,
		573292817ull,
		1164186217ull,
		2364114217ull,
		4294967291ull,
		8589934583ull,
		17179869143ull,
		34359738337ull,
		68719476731ull,
		137438953447ull,
		274877906899ull,
		549755813881ull,
		1099511627689ull,
		2199023255531ull,
		4398046511093ull,
		8796093022151ull,
		17592186044399ull,
		35184372088777ull,
		70368744177643ull,
		140737488355213ull,
		281474976710597ull,
		562949953421231ull,
		1125899906842597ull,
		2251799813685119ull,
		4503599627370449ull,
		9007199254740881ull,
		18014398509481951ull,
		36028797018963913ull,
		72057594037927931ull,
		144115188075855859ull,
		288230376151711717ull,
		576460752303423433ull,
		1152921504606846883ull,
		2305843009213693951ull,
		4611686018427387847ull,
		9223372036854775783ull,
		18446744073709551557ull,
	};
	static constexpr int _rehash_threshold = 2;
	static constexpr int _shrink_threshold = 2;
	using pairT = std::pair<keyT, valT>;
	using bucketT = std::vector<pairT>;
	using bufferT = std::vector<bucketT>;
	std::hash<keyT> _hash;

	bufferT _buf;
	size_t _bucket_size{ 5 };
	size_t _bucket_idx{ 0 };
	size_t _size{ 0 };
	size_t _collision{ 0 };
    bool _inside_rehash { false };
public:
	hash_table()
	{
		_buf = bufferT();
		_buf.resize(_bucket_size);
	}
	hash_table(const hash_table& other) : _buf { other._buf }, _bucket_size{ other._bucket_size }, _bucket_idx{ other._bucket_idx }, _size { other._size }, _collision{ other._collision } { }
	hash_table(hash_table&& other) : _buf{ std::move(other._buf) }, _bucket_size{ other._bucket_size }, _bucket_idx{ other._bucket_idx }, _size{ other._size }, _collision{ other._collision } { }
	size_t size() const { return _size; }
	void add(const pairT& pair)
	{
		bucketT& bucket = _buf[_get_idx(pair.first)];
		for (auto&& p : bucket)
		{
			if (p.first == pair.first) throw std::runtime_error("the key already exists.");
		}
		bucket.emplace_back(pair);
		++_size;
		if (bucket.size() > 1) ++_collision;
		if (_collision > _bucket_size / _rehash_threshold) _rehash(_bucket_idx+1);
	}
	void add(const keyT& key, const valT& val)
	{
		add(pairT{ key, val });
	}
	void erase(const keyT& key)
	{
		bucketT& bucket = _buf[_get_idx(key)];
		for (auto it = bucket.begin(); it != bucket.end(); ++it)
		{
			if (it->first == key)
			{
				bucket.erase(it);
				--_size;
				if (bucket.size() == 0) _collision -= 1;

                if (_size < _bucket_size / _shrink_threshold && _bucket_idx > 0) _rehash(_bucket_idx-1);

				return;
			}
		}
		throw std::runtime_error("cannot find the key-value pair with the given key.");
	}
	valT& operator[] (const keyT& key)
	{
		bucketT& bucket = _buf[_get_idx(key)];
		for (auto&& p : bucket)
		{
			if (p.first == key) return p.second;
		}
		throw std::runtime_error("cannot find the key-value pair with the given key.");
	}
	valT const& operator[] (const keyT& key) const
	{
		bucketT& bucket = _buf[_get_idx(key)];
		for (auto&& p : bucket)
		{
			if (p.first == key) return p.second;
		}
		throw std::runtime_error("cannot find the key-value pair with the given key.");
	}
private:
	size_t _get_idx(keyT key) const
	{
		return _hash(key) % _bucket_size;
	}
	void _rehash(const size_t& new_bucket_idx)
	{
		// static bool _inside_rehash{ false };
		if (_inside_rehash) return;
		if (new_bucket_idx >= sizeof(_primes)) return;
		_inside_rehash = true;

		_bucket_idx = new_bucket_idx;
		_bucket_size = _primes[_bucket_idx];
		_collision = 0;
		_size = 0;

		bufferT new_buf = bufferT();
		new_buf.resize(_bucket_size);
		std::swap(_buf, new_buf);
		for (auto&& b : new_buf)
		{
			for (auto&& p : b) add(p);
		}
		_inside_rehash = false;
	}
};
```

## Deque

```cpp
#include <utility>
#include <functional>
#include <vector>
#include <list>

template<typename T>
class dequeue
{
	static constexpr size_t _ARR_SIZE = 128;
	using arrT = std::vector<T>;
	using mapT = std::list<arrT>;
	mapT _map{};
	size_t _size{ 0 };
public:
	dequeue() {}
	dequeue(const dequeue& other) : _map { other._map }, _size{ other._size } { }
	dequeue(dequeue&& other) : _map { std::move(other._map) }, _size { other._size } { }
	void push_back(const T& val)
	{
		if (_map.empty() || _map.back().size() >= _ARR_SIZE)
		{
			_map.emplace_back(arrT());
			_map.back().reserve(_ARR_SIZE);
		}
		_map.back().push_back(val);
		++_size;
	}
	void emplace_back(T&& val)
	{
		if (_map.empty() || _map.back().size() >= _ARR_SIZE)
		{
			_map.emplace_back(arrT());
			_map.back().reserve(_ARR_SIZE);
		}
		_map.back().emplace_back(val);
		++_size;
	}
	void push_front(const T& val)
	{
		if (_map.empty() || _map.front().size() >= _ARR_SIZE)
		{
			_map.emplace_front(arrT());
			_map.front().reserve(_ARR_SIZE);
		}
		_map.front().push_front(val);
		++_size;
	}
	void emplace_front(T&& val)
	{
		if (_map.empty() || _map.front().size() >= _ARR_SIZE)
		{
			_map.emplace_front(arrT());
			_map.front().reserve(_ARR_SIZE);
		}
		_map.front().emplace_front(val);
		++_size;
	}
	void pop_front()
	{
		if (_size == 0) throw std::runtime_error("dequeue is empty.");
		auto& arr = _map.front();
		if (arr.size() == 1)
			_map.pop_front();
		else
		{
			arr.erase(arr.begin());
		}
		--_size;
	}
	void pop_back()
	{
		if (_size == 0) throw std::runtime_error("dequeue is empty.");
		auto& arr = _map.back();
		if (arr.size() == 1)
			_map.pop_back();
		else
		{
			arr.erase(std::prev(arr.end()));
		}
		--_size;
	}
	size_t size() const { return _size; }
	T& operator[] (const size_t& idx)
	{
		if (idx > _size) throw std::out_of_range("dequeue index out of range.");
		size_t map_idx = idx / _ARR_SIZE;
		size_t arr_idx = idx - map_idx * _ARR_SIZE;
		auto it = _map.begin();
		for (size_t i = 0; i < map_idx; ++i) ++it;
		return (*it)[arr_idx];
	}
	T const& operator[] (const size_t& idx) const
	{
		if (idx > _size) throw std::out_of_range("dequeue index out of range.");
		size_t map_idx = idx / _ARR_SIZE;
		size_t arr_idx = idx - map_idx * _ARR_SIZE;
		auto it = _map.begin();
		for (size_t i = 0; i < map_idx; ++i) ++it;
		return (*it)[arr_idx];
	}
};
```

## Unique Pointer

```cpp
template<typename T>
class unique_ptr
{
	T* _resource;
public:
	unique_ptr(const unique_ptr&) = delete;
	unique_ptr() : _resource{ nullptr } {};
	unique_ptr(std::nullptr_t) : _resource{ nullptr } {};
	unique_ptr(unique_ptr&& other) noexcept
	{
		_resource = other._resource;
	}
	unique_ptr& operator==(unique_ptr&& other) noexcept
	{
		_resource = other._resource;
	}
	explicit unique_ptr(T* resource)
	{
		_resource = resource;
	}
	~unique_ptr()
	{
		delete _resource;
	}
	explicit operator bool() const noexcept 
	{
		return this->_resource;
	}
	T* operator->() const noexcept { return _resource; }
	T& operator*() const noexcept { return *_resource; }

	T* get() const { return _resource; }
	void reset(const T* resource = nullptr)
	{
		delete _resource;
		_resource = resource;
	}
	void swap(const unique_ptr& other)
	{
		std::swap(_resource, other._resource);
	}
};
```

## Shared Pointer

```cpp
template<typename T>
class shared_ptr
{
	T* _resource;
	unsigned int* _counter;
public:
	shared_ptr() noexcept : _resource{ nullptr }, _counter{ new unsigned int (0) } {}
	shared_ptr(std::nullptr_t) noexcept : _resource{ nullptr }, _counter{ new unsigned int(0) } {}
	shared_ptr(const shared_ptr& other) noexcept : _resource{ other._resource }, _counter{ other._counter }
	{
		++(*_counter);
	}
	shared_ptr(shared_ptr&& other) noexcept : _resource{ other._resource }, _counter{ other._counter } {}
	shared_ptr& operator=(const shared_ptr& other)
	{
		if (--(*_counter) == 0)
		{
			delete _resource;
			delete _counter;
		}
		_resource = other._resource;
		_counter = other._counter;
		++(*_counter);
		return *this;
	}
	explicit shared_ptr(T* resource) noexcept : _resource{ resource }, _counter{ new unsigned int(1) } {}
	~shared_ptr()
	{
		if (--(*_counter) == 0 )
		{
			delete _resource;
			delete _counter;
		}
	}
	explicit operator bool() const noexcept
	{
		return _resource;
	}
	T* operator->() const noexcept { return _resource; }
	T& operator*() const noexcept { return *_resource; }
	T* get() const { return _resource; }
	void reset(const T*& resource = nullptr)
	{
		if (--(*_counter) == 0)
		{
			delete _resource;
			delete _counter;
		}
		_resource = resource;
		if (_resource) _counter = new unsigned int(1);
		else _counter = nullptr;
	}
	void reset(const shared_ptr& other = nullptr)
	{
		*this = other;
	}
	void swap(const shared_ptr& other)
	{
		std::swap(other, *this);
	}
};
```

{% endraw %}