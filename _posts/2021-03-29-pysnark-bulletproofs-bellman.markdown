---
layout: post
title:  "PySNARK now supports bulletproofs and bellman"
date:   2021-03-29 19:55:23 +0200
---

The `zkinterface` backend of PySNARK has been updated to the latest version of `zkinterface`, and it is now possible to use PySNARK to generate zkinterface `zkif` files that can be used with zkinterface's bellman and bulletproofs backend to generate zero-knowledge proofs!

PySNARK with the `zkinterface` backend automatically produces a file `computation.zkif` containing the circuit, witness, and constraint system for the computation. This file can be used for example with the bellman and bulletproofs backends of `zkinterface`, see [here](https://github.com/QED-it/zkinterface/tree/master/ecosystem):

To generate a zkif file that should work with the zkinterface libsnark backend (not tested):

```
examples meilof$ PYSNARK_BACKEND=zkinterface python3 cube.py 33
The cube of 33 is 35937
*** zkinterface: writing circuit
*** zkinterface: writing witness
*** zkinterface: writing constraints
*** zkinterface circuit, witness, constraints written to 'computation.zkif', size 656
``` 

To generate a zkif file for the zkinterface bellman backend and use it:

```
examples meilof$ PYSNARK_BACKEND=zkifbellman python3 cube.py 33
The cube of 33 is 35937
*** zkinterface: writing circuit
*** zkinterface: writing witness
*** zkinterface: writing constraints
*** zkinterface circuit, witness, constraints written to 'computation.zkif', size 656
examples meilof$ cat computation.zkif | zkif_bellman setup
Written parameters into /Users/meilof/Subversion/pysnark/examples/bellman-pk
examples meilof$ cat computation.zkif | zkif_bellman prove
Reading parameters from /Users/meilof/Subversion/pysnark/examples/bellman-pk
Written proof into /Users/meilof/Subversion/pysnark/examples/bellman-proof
examples meilof$ cat computation.zkif | zkif_bellman verify
Reading parameters from /Users/meilof/Subversion/pysnark/examples/bellman-pk
Reading proof from /Users/meilof/Subversion/pysnark/examples/bellman-proof
The proof is valid.
```

To generate a zkif file for the zkinterface bulletproofs backend and use it:

```
examples meilof$ PYSNARK_BACKEND=zkifbulletproofs python3 cube.py 33
The cube of 33 is 35937
*** zkinterface: writing circuit
*** zkinterface: writing witness
*** zkinterface: writing constraints
*** zkinterface circuit, witness, constraints written to 'computation.zkif', size 656
examples meilof$ cat computation.zkif | zkif_bulletproofs prove
Saved proof into bulletproofs-proof
examples meilof$ cat computation.zkif | zkif_bulletproofs verify
Verifying proof in bulletproofs-proof
```
