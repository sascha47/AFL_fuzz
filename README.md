# AFL-FUZZ 101

## What Is Fuzzing?
<p>
Fuzzing is looking for behavior in a program that resembles a crash, halt, error or memory leakage by running invalid input into the application.
</p>


## Why Fuzz Applications?

<p>
 Fuzzing applications allows us to find vulnerabiliies within an applications. Hackers use fuzzing to find zero day exploits and blue hats use fuzzing to determinet the scope of vulnerabilities to process. Ethical Hackers can can also use fuzzing in bug bounties to find things that need to be patched.
</p>


## American Fuzzy Lop or AFL fuzz

<p>
AFL is a fuzzer that uses a seed input that is connected to the program we are fuzzing. The seed input allows for AFL to potentially fuzz the program for new test cases which could lead to new path discoveries. 
</p>

## How to use AFL fuzzer

### Prequisites

<p> First you need to install the compilers</p>

```
sudo apt install gcc
sudo apt install clang
```
  
### Install AFL

```
wget http://lcamtuf.coredump.cx/afl/releases/afl-latest.tgz
tar -xzvf afl-latest.tgz
cd afl-2.52b/
make
sudo make install
```
### Default compiler is slow, leverage built in complier LLVM for faster fuzzing

```
cd afl-2.51b/llvm_mode/
sudo apt-get install llvm-dev llvm
make
cd ..
make
sudo make install
```

### Congratulations, AFL Fuzz and LLVM are installed

### Practice Project
<p>
  In this project we will be fuzzing an intentionally vulnerable program called "Fuzzgoat"
</p>
  
### Installing fuzzgoat

<p>
  First makes sure you have git installed 
</p>

 <p>
 For Debian Distros
 </p>
 
 ```
 sudo apt install git
 ```
 <p>Then install fuzzgoat</p>
  
 ```
  git clone https://github.com/fuzzstati0n/fuzzgoat
cd fuzzgoat/
 ```

### Set up or compiler
<p> 
  To use a non default compiler, we need to assign the compiler to an enviroment variable that Fuzzgoat can access.
  
We will use afl-clang-fast as the compiler. AFL compilers are like basic compilers like GCC but with modifications that allow AFL to talk with the target application while it is running to generate new inputs and discover new code paths through the input seed. This process is called instrumentation.
</p>

### Setting enviroment variable for compiler afl-clang-fast
```
export CC=afl-clang-fast
```




  
#### Non StndInput
```
./fuzzgoat  aflout/crashes/id:000000,sig:06,src:000000,op:havoc,rep:16
```

#### Result for nonstnd input
![Result](https://github.com/sascha47/AFL_fuzz/blob/main/NON_stnd_input.PNG?raw=true)
