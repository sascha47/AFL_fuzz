
# A Comprehensive Introduction to AFL-Fuzz

## Understanding Fuzzing

Fuzzing is a dynamic software testing technique that involves providing invalid, unexpected, or random data to the inputs of a program. The primary aim of fuzzing is to trigger bugs that could potentially lead to program crashes, failure built-in code assertions, or potential memory leaks.

## The Importance of Fuzzing Applications

Fuzzing applications is an essential aspect of software security as it allows the discovery of vulnerabilities within an application. Attackers often use fuzzing techniques to identify zero-day exploits, while security professionals employ fuzzing to ascertain the extent of potential vulnerabilities. Ethical hackers also utilize fuzzing in bug bounty programs to discover elements within the application that require patching.

## Introduction to American Fuzzy Lop (AFL)

American Fuzzy Lop (AFL) is a powerful fuzzer employed for finding potential security vulnerabilities in software. AFL operates by using a seed input which is linked to the program under test. The seed input allows AFL to generate new test cases which could lead to discovering new paths and potential vulnerabilities within the program.

## Implementing AFL Fuzz

### Prerequisites 

Before you begin, it is crucial to install the necessary compilers:

```shell
sudo apt install gcc
sudo apt install clang
```
### AFL Installation Process

To install AFL, execute the following commands:

```shell
wget http://lcamtuf.coredump.cx/afl/releases/afl-latest.tgz
tar -xzvf afl-latest.tgz
cd afl-2.52b/
make
sudo make install
```
### Leveraging LLVM for Efficient Fuzzing

The default compiler might not provide optimal performance for fuzzing. Therefore, leveraging the built-in compiler LLVM can result in faster fuzzing. To do this, execute the following commands:

```shell
cd afl-2.51b/llvm_mode/
sudo apt-get install llvm-dev llvm
make
cd ..
make
sudo make install
```
Congratulations, you have successfully installed AFL Fuzz and LLVM!

### Hands-On Practice with Fuzzgoat

In this section, we will use an intentionally vulnerable program named "Fuzzgoat" to understand the practical application of AFL Fuzz.

### Installing Fuzzgoat

Before you begin, ensure that git is installed on your system. For Debian based distributions, you can install git with the following command:

```shell
sudo apt install git
```
Next, install Fuzzgoat by cloning the GitHub repository:

```shell
git clone https://github.com/fuzzstati0n/fuzzgoat
cd fuzzgoat/
```
### Configuring the Compiler

By default, Fuzzgoat uses a basic compiler. However, we will be using afl-clang-fast as our compiler for this exercise. AFL compilers are similar to basic compilers but with added functionality that allows AFL to interact with the target application while it's running. This interaction helps in generating new inputs and discovering new code paths through the seed input, a process referred to as 'instrumentation'.

### Setting the Environment Variable for the Compiler afl-clang-fast

To set the compiler, assign the compiler to an environment variable that Fuzzgoat can access:

```shell
export CC=afl-clang-fast
```
### Compiling Fuzzgoat into a Binary

After cloning the Fuzzgoat repository, the next step is to compile the application into a binary. Inside the Fuzzgoat directory, execute the following command:

```shell
make
```
### Defining Seed Input Files

AFL uses seed input files to feed commands into the

target application. The seed input files allow AFL to mutate its commands to discover new paths within the target application for potential exploitation.

Start by creating an input directory to hold our test cases produced by AFL, and an output directory to store the results. The output directory will contain subdirectories for different result types: `crashes` for results that caused the application to crash, `hangs` for results that caused the application to hang, and a `queue` directory that holds results that AFL has yet to test against the application. 

```shell
mkdir in
mkdir out
```
### Preparing for Fuzzing

Before beginning the fuzzing process, we need to create a seed input that connects to our target application. This seed input is used by AFL Fuzz to mutate commands within the target application, thus creating new test cases. You can add a binary to the `in` directory with the following command:

```shell
dd if=/dev/urandom of=.../in/random.input bs=1 count=1
```
After executing the above command, the `in` directory should contain a copy of your binary, ready to be input against the target application.

### Executing Fuzzing

To fuzz the application, execute the following command:

```shell
afl-fuzz -i in -o out -- ./fuzzgoat @@
```
In this command:

- `-i` specifies the directory from which the seed cases are fetched (this can be any directory of your choice)
- `-o` specifies the directory where AFL stores the results for crashes, hangs, and queues
- `--` separates the AFL flags on the left from where the target is run on the right
- `@@` is where AFL inserts the test seed input file for fuzzing the application. Alternatively, you can use STDIN instead of `@@`

After running the above command, observe the application for a few minutes and then terminate the process with `CTRL + C`.

Navigate to the `out/crashes` directory, and you should see results similar to the ones in the provided image. Please note that the results may vary based on the duration for which you ran the fuzzing process.

### Analyzing the Results

The crashes located in the `crashes` directory were used by AFL Fuzz in the instrumentation or seed input process to crash the binary. To run a crash against your application (Fuzzgoat in this instance), use the following command:

```shell
./fuzzgoat  aflout/crashes/id:000000,sig:06,src:000000,op:havoc,rep:16
```
Please refer to the provided image for the expected results of the above command.

Congratulations! You have successfully installed AFL Fuzz, the afl-clang-fast compiler, created seed inputs, and discovered vulnerabilities within Fuzzgoat that caused the application to crash.

