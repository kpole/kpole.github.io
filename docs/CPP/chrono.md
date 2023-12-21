---
comments: true
---

```cpp
#include <iostream>
#include <string>
#include <chrono>

void Run()
{
	for (int i = 0; i < 1000000000; ++i) {};
}

int main()
{
	auto beforeTime = std::chrono::steady_clock::now();
	
	Run();

	auto afterTime = std::chrono::steady_clock::now();

	std::cout << "总耗时:" << std::endl;
	//秒
	double duration_second = std::chrono::duration<double>(afterTime - beforeTime).count();
	std::cout << duration_second << "秒" << std::endl;

	//毫秒级
	double duration_millsecond = std::chrono::duration<double, std::milli>(afterTime - beforeTime).count();
	std::cout << duration_millsecond << "毫秒" << std::endl;

	//微妙级
	double duration_microsecond = std::chrono::duration<double, std::micro>(afterTime - beforeTime).count();
	std::cout << duration_microsecond << "微秒" << std::endl;

	//纳秒级
	double duration_nanosecond = std::chrono::duration<double, std::nano>(afterTime - beforeTime).count();
	std::cout << duration_nanosecond << "纳秒" << std::endl;

	getchar();
	return 0;
}
```

参考：

- [cppreference](https://en.cppreference.com/w/cpp/chrono)