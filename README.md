# macOS - Custom Clang

Follow the following procedure (X.Y.Z is the chosen version):

```
curl -JLOk https://github.com/llvm/llvm-project/releases/download/llvmorg-X.Y.Z/llvm-project-X.Y.Z.src.tar.xz

tar -xJf llvm-project-X.Y.Z.src.tar.xz

cd llvm-project-X.Y.Z.src

cmake -S llvm \
      -B build \
      -DCMAKE_BUILD_TYPE=Release
      -DCMAKE_INSTALL_PREFIX=$HOME/LLVM/X.Y.Z \
      -DLLVM_ENABLE_PROJECTS="clang;clang-tools-extra;polly" \
      -DLLVM_TARGETS_TO_BUILD="AArch64;X86" \
      -DLLVM_INCLUDE_BENCHMARKS=OFF \
      -DLLVM_INCLUDE_EXAMPLES=OFF \
      -DLLVM_INCLUDE_TESTS=OFF \
      -DLLVM_ENABLE_RUNTIMES="compiler-rt;libcxx;libcxxabi;libunwind;openmp"

cmake --build build

cmake --install build
```

The new C++ compiler needs to link programs to its standard library (libc++) so you need to create a `clang++.cfg` file in the `$HOME/LLVM/X.Y.Z/bin` directory with the following contents:

```
-isysroot @SDK_PATH@
-nostdinc++
-isystem <CFGDIR>/../include/c++/v1
-L<CFGDIR>/../lib
-Wl,-rpath,<CFGDIR>/../lib
-lunwind
```

where SDK_PATH has to be replaced with output from `xcrun --show-sdk-path`.
