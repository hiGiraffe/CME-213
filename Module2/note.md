# 学习笔记
## 高精度时间测量
```c++
#include <chrono>
using namespace std::chrono;
int main(){
    high_resolution_clock::time_point start, end;
    duration<double> delta;
    start = high_resolution_clock::now();
    ...
    end = high_resolution_clock::now();
    delta = duration_cast<duration<double>>(end - start);
}
```
