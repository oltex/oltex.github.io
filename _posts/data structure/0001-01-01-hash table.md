```cpp
template<typename _Kty, typename _Ty>
struct Node final {
	explicit Node(const _Kty& key, const _Ty& value) :
		_key(key), _value(value) {
	}
	_Kty _key;
	_Ty _value;
};
```
```cpp
template<typename _Kty, typename _Ty>
class Table final {
public:
	void Insert(const _Kty& key, const _Ty& value);
	void Erase(const _Kty& key);
	typename std::list<Node<_Kty, _Ty>>::iterator Find(const _Kty& key);
	_Ty& operator[](const _Kty& key);
private:
	int Hash(const _Kty& key) const;
private:
	std::list<Node<_Kty, _Ty>> bucket[100];
};
```
```cpp
template<typename _Kty, typename _Ty>
void Table<_Kty, _Ty>::Insert(const _Kty& key, const _Ty& value) {
	auto iter = Find(key);
	int hash = Hash(key);
	if (iter != bucket[hash].end())
		return;
	bucket[hash].emplace_back(key, value);
}

template<typename _Kty, typename _Ty>
void Table<_Kty, _Ty>::Erase(const _Kty& key) {
	auto iter = Find(key);
	int hash = Hash(key);
	if (iter == bucket[hash].end())
		return;
	bucket[hash].erase(iter);
}

template<typename _Kty, typename _Ty>
typename std::list<Node<_Kty, _Ty>>::iterator Table<_Kty, _Ty>::Find(const _Kty& key) {
	int hash = Hash(key);
	auto iter = std::find_if(bucket[hash].begin(), bucket[hash].end(), [&](Node<_Kty, _Ty> temp) {
		return key == temp._key;
		});
	return iter;
}

template<typename _Kty, typename _Ty>
_Ty& Table<_Kty, _Ty>::operator[](const _Kty& key) {
	auto iter = Find(key);
	return (*iter)._value;
}

template<typename _Kty, typename _Ty>
int Table<_Kty, _Ty>::Hash(const _Kty& key) const {
	return ((key / 10000 % 100) + (key / 100 % 100) + (key % 100)) % 100;
}
```
