
```C++
static void StringPush(benchmark::State& state) 
{
  for (auto _ : state) 
  {
    std::vector<std::string> buffer;
    buffer.reserve(10);
    for(int i=0; i<10; i++)
    {
      std::string created_string("long string that will require memory allocation");
      buffer.push_back(created_string);
    }
    benchmark::DoNotOptimize(buffer);
  }
}
// Register the function as a benchmark
BENCHMARK(StringPush);

static void StringMove(benchmark::State& state) 
{
  for (auto _ : state) 
  {
    std::vector<std::string> buffer;
    buffer.reserve(10);
    for(int i=0; i<10; i++)
    {
      std::string created_string("long string that will require memory allocation");
      // we don't need created_string anymore, we can move it instead 
      // of copying it
      buffer.push_back( std::move(created_string) ) ;
    }
    benchmark::DoNotOptimize(buffer);
  }
}
BENCHMARK(StringMove);
```
