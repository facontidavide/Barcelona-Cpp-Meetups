Trying to replycate the result of a presentation about "fast module"... not quite there.

```C++

#include <random>
#include <algorithm>
#include <iterator>
#include <iostream>

const unsigned VECT_SIZE = 100;


static std::vector<unsigned> buildShuffledVector(int size = VECT_SIZE)
{
    std::vector<unsigned> vect(size);
    for (size_t i=0; i<size; i++){
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
    const unsigned ceil = state.range(0);

    while (state.KeepRunning())
    {
        for (size_t i=0; i< vect.size(); i++){
            value = (vect[i] % ceil );
        }
    }
}

static void BM_FastMod(benchmark::State& state) {

    std::vector<unsigned> vect = buildShuffledVector();
    volatile unsigned value;
    const unsigned ceil = state.range(0);

    while (state.KeepRunning())
    {
        for (size_t i=0; i< vect.size(); i++)
        {
            value =  vect[i] < ceil ? vect[i]  : vect[i] % ceil;
        }
    }
}
BENCHMARK(BM_Mod)->DenseRange(1, 100, 20);
BENCHMARK(BM_FastMod)->DenseRange(1, 100, 20);


```
