---
layout: post
title: Building LLVM
tags: [llvm, clang, c++]
---

```bash
git clone https://github.com/llvm-project/llvm-project-20170507/ src

mkdir build-release
cd $_

cmake                                                                          \
  -G"Ninja"                                                                    \
  -DCMAKE_BUILD_TYPE=Release                                                   \
  -DCMAKE_INSTALL_PREFIX=/opt/llvm                                             \
  -DCLANG_VENDOR_STRING="ARH Release"                                          \
  -DLLVM_ENABLE_PROJECTS="clang;compiler-rt;polly;libcxx;libcxxabi;lldb;lld;libunwind" \
  -DLLVM_ENABLE_FFI=ON                                                         \
  -DENABLE_LIBCXX=ON                                                           \
  -DLLVM_OPTIMIZED_TABLEGEN=ON                                                 \
  -DLLVM_BUILD_EXTERNAL_COMPILER_RT=ON                                         \
  -DLLVM_INCLUDE_DOCS=OFF                                                      \
  -DLLVM_ENABLE_RTTI=ON                                                        \
  -DLLVM_ENABLE_EH=ON                                                          \
  -DLLVM_INSTALL_UTILS=ON                                                      \
  -DWITH_POLLY=ON                                                              \
  -DLINK_POLLY_INTO_TOOLS=ON                                                   \
  -DLLVM_TARGETS_TO_BUILD=all                                                  \
  ../src/llvm

# Live life dangerously...
cmake                                                                          \
  -DLLVM_BUILD_TESTS=OFF                                                       \
  -DLLVM_INCLUDE_TESTS=OFF                                                     \
  -DLLDB_INCLUDE_TESTS=OFF                                                     \
  -DLIBCXX_INCLUDE_TESTS=OFF                                                   \
  -DLIBCXXABI_INCLUDE_TESTS=OFF                                                \
  -DCLANG_INCLUDE_TESTS=OFF                                                    \
  -DCOMPILER_RT_INCLUDE_TESTS=OFF                                              \
  .

ninja
sudo ninja install
```
