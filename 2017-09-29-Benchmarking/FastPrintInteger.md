Testing the speed of a custom funtion to print numbers freqyently lower than 100.

```c++

inline int print_unsigned(char* buffer, unsigned value)
{
  const char DIGITS[] =
      "00010203040506070809"
      "10111213141516171819"
      "20212223242526272829"
      "30313233343536373839"
      "40414243444546474849"
      "50515253545556575859"
      "60616263646566676869"
      "70717273747576777879"
      "80818283848586878889"
      "90919293949596979899";
  if (value < 10)
  {
    buffer[0] = static_cast<char>('0' + value);
    return 1;
  }
  else if (value < 100) {
    value *= 2;
    buffer[0] = DIGITS[ value+1 ];
    buffer[1] = DIGITS[ value ];
    buffer[2] = '\0';
    return 2;
  }
  else{
    return sprintf( buffer,"%d", value );
  }
}

static void BM_sprintf(benchmark::State& state) {

    char buffer[10];
    while (state.KeepRunning())
    {
        benchmark::DoNotOptimize( sprintf(buffer, "%d", 42) );
    }
}

static void BM_PrintNumber(benchmark::State& state) {

    char buffer[10];
    while (state.KeepRunning())
    {
        benchmark::DoNotOptimize( print_unsigned(buffer, 42) );
    }
}

BENCHMARK(BM_sprintf);
BENCHMARK(BM_PrintNumber);

```
