# PractRand Patches

This is a series of quilt patches that adds a simple configure script
for building, [improves performance][pcg], and fixes bugs. These patches
specifically target [PractRand][pr] pre0.95.

Usage:

```sh
curl -OL https://downloads.sourceforge.net/project/pracrand/PractRand-pre0.95.zip
unzip PractRand-pre0.95.zip
quilt push -a
./configure
make -kj$(nproc)
```

In particular, I'm interested in running PractRand compiled with
sanitizer instrumentation since it, along with compiler warnings,
reveals lots of serious bugs in PractRand:

```sh
export UBSAN_OPTIONS=print_stacktrace=1
CXX=g++ \
  CXXFLAGS='-ggdb3 -Og -fsanitize=address -fsanitize=undefined' \
  LDFLAGS='-fsanitize=address -fsanitize=undefined' \
  ./configure
make -kj$(nproc)
./RNG_test stdin64 </dev/urandom
```

[pcg]: http://www.pcg-random.org/posts/how-to-test-with-practrand.html
[pr]: http://pracrand.sourceforge.net/
