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

## openMP并行求和

```c++
#pragma omp parallel for reduction(+:sum0) reduction(+:sum1)
for(uint i=0; i<v.size(); i++) {
        if (v[i] % 2 == 0) {
            sum0 += v[i];
        }
        else {
            sum1 += v[i];
        }
    }
```
