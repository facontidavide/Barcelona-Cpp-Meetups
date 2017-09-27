Trying to replycate the result of a presentation about "fast module"... not quite there.

```C++

#include <random>
#include <algorithm>
#include <iterator>
#include <iostream>

const unsigned VECT_SIZE = 1000;
const unsigned MAX       = 500;

static std::vector<unsigned> buildShuffledVector(int size = VECT_SIZE)
{
   std::vector<unsigned> vect(size);
   for (int i=0; i<size; i++){
     vect[i] = i;
   }
    std::random_device rd;
    std::mt19937 g(rd());
 
    std::shuffle(vect.begin(), vect.end(), g);
    return vect;
}

static void BM_Mod(benchmark::State& state) {
  
  std::vector<unsigned> vect = buildShuffledVector();
  volatile unsigned value;

  while (state.KeepRunning())
  {
    for (const auto& v: vect) value = v % MAX;
  }
}
// Register the function as a benchmark
BENCHMARK(BM_Mod);

static void BM_FastMod(benchmark::State& state) {

  std::vector<unsigned> vect = buildShuffledVector();
  volatile unsigned value;
  while (state.KeepRunning())
  {
    for (const auto& v: vect) value = ( v < MAX ) ? v : (v % MAX);
  }
}
BENCHMARK(BM_FastMod);


```
