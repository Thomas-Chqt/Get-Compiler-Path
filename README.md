Exemple:

```
name: Print compilers path
run-name: Print compilers path for ${{ github.ref }} by ${{ github.actor }}

on:
  pull_request_target:
    branches: [ $default-branch ]
  push:
    branches-ignore: []

jobs:      
  Print-Path:
    strategy:
        matrix:
          os:       [ macos-latest, ubuntu-latest ]
          compiler: [ clang-13, clang-14, clang-15, gcc-10, gcc-11, gcc-12, gcc-13 ]
            
          include:
            - os: windows-latest
              compiler: clang
            - os: windows-latest
              compiler: gcc

          exclude:
            - os: macos-latest
              compiler: clang-13
            - os: macos-latest
              compiler: clang-14
            - os: macos-latest
              compiler: gcc-10
            - os: ubuntu-latest
              compiler: gcc-13

        fail-fast: false

    runs-on: ${{ matrix.os }}
    steps:
      - name: Get compiler path
        id: get-path
        uses: Thomas-Chqt/Get-Compiler-Path@master
        with:
            compiler: ${{ matrix.compiler }}

      - name: Print compiler path
        run: | 
            echo "C Compiler path is ${{ steps.get-path.outputs.c-path }}"
            echo "C++ Compiler path is ${{ steps.get-path.outputs.cpp-path }}"

```
