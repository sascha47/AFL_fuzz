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
### Configure fuzzgoat as a binary
<p>
After cloning the respository we need to compile that application into a binary. Inside the fuzzgoat directory run the code below
</p>

```
make
```
### Defining the seed input files

<p> 
 AFL uses the seed input file to input commands into the target application. The seed input files allows for AFL to mutate its commands to find new file paths within the targeth application to exploit.
</p>

<p> Start with creating and input file that will hold our test cases produced by AFL fuzz. Also create the output files that contains subdirectories: crashes for crashed results, hangs for results that causes the application to hang and a queue directory that holds results that AFL has not tested against the applicaiton. 
</p>

```
mkdir in
mkdir out

```
### Before fuzzing the application
<p>
 We need to create an input seed that connects to our target application and is used by AFL Fuzz to  mutate commands within the target application to create new test cases. To do this we can add an binary to the in folder
</p>

```
dd if=/dev/urandom of=.../in/random.input bs=1 count=1
```
<p>
 
Now the in folder should have a copy of your ps binary and ready to be input against the target application
</p>

### To fuzz the applicatoin
<p>
 Key:
 -i = specifies the directory from which the seed cases are run  (can be named anything)
 
 -o = specifies the the directory where AFL stores the results for crashes, hangs and queues
 
 -- = seperates the AFL flags on the left from where the target is run on the right
 
@@ = This is where AFL fuzzes the application by inserts the test seed input file here. STDIN can also be used instead of @@
</p>

### Run the applicatoin
```
afl-fuzz -i in -o out -- ./fuzzgoat @@
```
<p>
After running the application watch if for a couple of minutes and then CTRL + C
 
Navigate to /out/crashes and you should see a result similar to mine. I ran my results fo 5 minutes which could provide more or less results than you
</p>

![Results](https://github.com/sascha47/AFL_fuzz/blob/main/Crashes.PNG?raw=true)

### How to rung your crashes

<p>
 
These crashes in the crashes directory are what was used by AFL FUZZ in the instrumentation or seed input to crash the binary. To run crash against your application or fuzzgoat in this instance use the non standard input method below
 
</p>
  
#### Non StndInput
```
./fuzzgoat  aflout/crashes/id:000000,sig:06,src:000000,op:havoc,rep:16
```

#### Result for nonstnd input
![Result](https://github.com/sascha47/AFL_fuzz/blob/main/NON_stnd_input.PNG?raw=true)

<p> 
 
Congratuations, you have Installed AFL fuzz, the gcc-clang-fast compliers, created input seeds and found vulnerabilities within fuzzgoat that crashed the application!
</p>

### For using stdin methods (not used in this guide)
```
# fuzz using stdin
./afl-fuzz -i testcase_dir -o findings_dir -- /path/to/tested/program

# fuzz using file path
./afl-fuzz -i testcase_dir  -o findings_dir  /path/to/tested/program @@
```
