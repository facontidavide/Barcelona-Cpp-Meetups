Std::string append seems to be slower than raw memcpy, even when std:.string::reserve is used.


```C++
#include <cstring>

static void BM_StringA(benchmark::State& state) {
  
  const std::string hello("Hello");
  const std::string world("World");

  std::string str;
  str.reserve( hello.size()*2 + world.size()*2 );

  while (state.KeepRunning())
  {
    str.clear();
    str += (hello);
    str += (world);
    str += (hello);
    str += (world);
  }
}
// Register the function as a benchmark
BENCHMARK(BM_StringA);

static void BM_StringC(benchmark::State& state) {

  const std::string hello("Hello");
  const std::string world("World");

  std::string str;
  str.resize( hello.size()*2 + world.size()*2 );

  while (state.KeepRunning())
  {
    char *buffer = &str[0];
    std::memcpy( buffer,    hello.data(), hello.size());
    buffer += hello.size();
    std::memcpy( buffer , world.data(), world.size());
    buffer += world.size();
    std::memcpy( buffer, hello.data(), hello.size());
    buffer += hello.size();
    std::memcpy( buffer, world.data(), world.size());
  }
}
BENCHMARK(BM_StringC);
```
