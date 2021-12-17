<img alt="points bar" align="right" height="36" src="../../blob/badges/.github/badges/points-bar.svg" />

![GitHub Classroom Workflow](../../workflows/GitHub%20Classroom%20Workflow/badge.svg?branch=main)

# COMP0061 -- Privacy Enhancing Technologies -- Lab 01

This lab will introduce the basics of Petlib, encryption, signatures, and an end-to-end encryption system.

### Structure of Labs
The structure of all the labs will be similar: two Python files will be provided. 

- The first is named `Lab0XCode.py` and contains the structure of the code you need to complete. 
- The second is named `Lab0XTests.py` and contains unit tests (written for the Pytest library) that you may execute to 
partially check your answers. 

Note that the tests passing is a necessary but not sufficient condition to fulfill each task.
There are programs that would make the tests pass that would still be invalid (or blatantly insecure) implementations.

The only dependency your Python code should have, besides Pytest and the standard library, is the Petlib library, 
which was specifically developed for this course (and also for our own use!). 

The petlib documentation is [available on-line here](http://petlib.readthedocs.org/en/latest/index.html).

### Setup
The intended environment for this lab is the Linux operating system with Python 3 installed. 

<!-- todo: write up virtual env, setup.sh, requirements.txt, VM-->

### Working with unit tests
Unit tests are run from the command line by executing the command:

```sh
$ pytest -v Lab01Tests.py
```

Note the `-v` flag toggles a more verbose output.
If you wish to inspect the output of the full tests run you may pipe this command to the `less` utility 
(execute `man less` for a full manual of the less utility):

```sh
$ pytest -v Lab01Tests.py | less
```

You can also run a selection of tests associated with each task by adding the Pytest marker for each task to the Pytest
command:

```sh
$ pytest -v Lab01Tests.py -m task1
```
The markers are defined in the test file and listed in `pytest.ini`.

You may also select tests to run based on their name using the `-k` flag.
Have a look at the test file to find out the function names of each test.
For example the following command executes the very first test of Lab 1:

```sh
$ pytest -v Lab01Tests.py -k test_petlib_present
```

The full documentation of pytest is [available here](http://pytest.org/latest/).


### What you will have to submit
The deadline for all labs is at the end of term but labs will be progressively released throughout the term, as new
concepts are introduced. 
We encourage you to attempt labs as soon as they are made available and to use the dedicated lab time to bring up any
queries with the TAs.

Labs will be checked using Github Classroom, and the tests will be run each
time you push any changes to the `main` branch of your Github repository.
The latest score from automarking should be shown in the Readme file.
To see the test runs, look at the Actions tab in your Github repository. 

Make sure the submitted `Lab01Code.py` file at least satisfies the tests, without the need for any external dependency 
except the Python standard libraries and the Petlib library. 
Only submissions prior to the Github Classroom deadline will be marked, so make sure you push your code in time.


To re-iterate, the tests passing is a necessary but not sufficient condition to fulfill each task.
All submissions will be checked by TAs for correctness and your final marks are based on their assessment of your work.  

<!-- todo: distribute the 6 marks-->
## TASK 1 -- Basic installation
> Ensure petlib is installed on the system
> and also pytest. Ensure the lab code can 
> be imported.

### Hints
- Execute the following command to ensure the tests run:

```sh
$ pytest -v Lab01Tests.py -m task1
```
- If everything is installed correctly the two selected tests should both pass without a problem, and without any 
modification to the code file.
This first task is meant to ensure everything is installed properly.
If it fails, talk to a Lab TA.

## TASK 2 -- Symmetric encryption using AES-GCM
> Implement encryption and decryption functions
> that simply performs AES_GCM symmetric encryption
> and decryption using the functions in `petlib.Cipher`.

### Hints
- This first task lets you explore how to use AES-GCM from petlib.
You may run the tests for this task using:

```sh
$ pytest -v Lab01Tests.py -m task2
```

- Consider these imports:
```python
from os import urandom 
from petlib.cipher import Cipher
```

- Note that `urandom` produces cryptographically strong bytes, which is handy for keys and initialisation vectors.

- The petlib.cipher package provides two handy functions `quick_gcm_enc` and `quick_gcm_dec` that will help you define the encryption and decryption functions.

- The documentation for petlib.cipher is [available here](http://petlib.readthedocs.org/en/latest/index.html#module-petlib-cipher).


## TASK 3 -- Understand Elliptic Curve Arithmetic
> - Test if a point is on a curve.
> - Implement Point addition.
> - Implement Point doubling.
> - Implement Scalar multiplication (double & add).
> - Implement Scalar multiplication (Montgomery ladder).
> 
> *Must not use any of the `petlib.ec` functions*. Only `petlib.Bn`!

### Hints
- The five (5) tests for this task run through:

```sh
$ pytest -v Lab01Tests.py -m task3
```

- petlib.bn provides facilities to do fast computations on `big numbers`.

```python
from petlib.bn import Bn
```

- The documentation of petlib.bn provides ample examples of the use of each function to manipulate big numbers.
You can find it [here](http://petlib.readthedocs.org/en/latest/index.html#module-petlib-bn)

- The documentation strings for each function provide guidance as to the algorithms you need to implement. 

- The tests provide you some guidance as to the inputs and outputs expected by each function.

- The lecture slides include the formulas for performing EC addition and doubling. Make use of them.

- Note that the neutral element `(infinity)` is encoded in `(x, y)` coordinates as `(None, None)`. Make sure you handle this input correctly. Do you also output it correctly?


## TASK 4 -- Standard ECDSA signatures
> - Implement a key / param generation 
> - Implement ECDSA signature using `petlib.ecdsa`
> - Implement ECDSA signature verification using `petlib.ecdsa`

### Hints

- The tests for this task run through:

```sh
$ pytest -v Lab01Tests.py -m task4
```

- This task lets you practice generating and verifying digital signatures. This is a vital skill, even if you do not know how digital signature work (we will actually study what is inside them later in this course).

- Note, that `petlib.ecdsa` provides both facilities to generate and verify signatures, as well as detailed documentation on how to use these. See `do_ecdsa_sign` and `do_ecdsa_verify`.

```python
from hashlib import sha256
from petlib.ec import EcGroup
from petlib.ecdsa import do_ecdsa_sign, do_ecdsa_verify
```

- The documentation for `module-petlib-ecdsa` is [available here](http://petlib.readthedocs.org/en/latest/index.html#module-petlib-ecdsa). Do use it.

- It is necessary to use a secure hash function to hash an input before signing or verifying it (self study: why is that?). Luckily, the hashlib Python library provides a number of secure hash functions, and a number of insecure ones (Question: which is which?).


## TASK 5 -- Diffie-Hellman Key Exchange and Derivation
> - use Bob's public key to derive a shared key.
> - Use Bob's public key to encrypt a message.
> - Use Bob's private key to decrypt the message.

### Hints
- The tests for this task run through:
    
```
$ pytest -v Lab01Tests.py -m task5
```
   
- This time you may use the facilities in petlib.ec to implement an EC Diffie-Hellman exchange.
The documentation [is here](http://petlib.readthedocs.org/en/latest/index.html#module-petlib-ec)

- Also: have a look at the provided key generation function to guide the remaining of the implementation.

- This task requires you to implement a simple hybrid encryption scheme, guided by the scheme presented in the slides. 
In a nutshell you may assume that Alice and Bob are aware of each other's public keys, and use those to eventually
derive a shared key.
This shared key is eventually used to key an AES-GCM cipher to protect the integrity and confidentiality of a message.

- You may assume that the public key passed to `dh_encrypt` is the public encryption key of the recipient, and the 
`alice_sig` parameter is the signature key of Alice the sender.
Conversely, the `priv` parameter of `dh_encrypt` is the recipient's (Bob) secret decryption key and `alice_ver` a public
verification key for a signature scheme.

- As part of this task you MUST implement a number of tests to ensure that your code is correct.
Stubs for 3 such tests are provided, namely `test_encrypt`, `test_decrypt` (which are self-explanatory), and 
`test_fails` which is meant to check for conditions under which the decryption must fail.
At least these should be implemented in the code file, but feel free to implement more.

- Your tests should run when you execute the following command, which produces a report on your test coverage.
Ensure all lines of code are fully covered by the test regime!

```
$ pytest --cov-report html --cov Lab01Code Lab01Tests.py 
```

## TASK 6 -- Time EC scalar multiplication
> *Open Task - Optional*
> 
> Time your implementations of scalar multiplication
>  (use time.clock() for measurements) for different 
>  scalar sizes

### Hints
- If you made it so far, talk to a TA. 

- If you are set on answering this question, you must time your execution of scalar multiplication to investigate timing
side channels. 

- Once you have observed timing channels that may leak secrets, go back and fix the scalar multiplication code to run in
constant time.
