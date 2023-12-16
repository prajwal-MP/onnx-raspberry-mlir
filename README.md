# Running ONNX Models on Raspberry Pi with ONNX-MLIR

In this tutorial, we will guide you through the process of running ONNX (Open Neural Network Exchange) models on a Raspberry Pi. Our approach leverages the ONNX-MLIR project to convert ONNX models into LLVM Intermediate Representation (IR), which can then be compiled on a Raspberry Pi using Clang. This method provides a streamlined way to deploy and run your machine learning models on edge devices like the Raspberry Pi.

## Prerequisites

Before we begin, ensure you have the following prerequisites ready:

- A Raspberry Pi (we recommend Raspberry Pi 3 or newer) running Raspberry Pi OS or any compatible Linux distribution.
- Basic familiarity with the command line interface.
- `clang` installed on your Raspberry Pi.
- `git`, `cmake`, and `ninja-build` for building ONNX-MLIR.

## Step 1: Installing Clang on Raspberry Pi

If you don't have Clang installed on your Raspberry Pi, you can install it using the following command:

```bash
sudo apt-get update
sudo apt-get install clang llvm
```

This will install the Clang compiler, which is essential for compiling the LLVM IR generated by ONNX-MLIR.

## Step 2: Setting Up ONNX-MLIR

ONNX-MLIR is a project that allows for the compilation of ONNX models into various targets, including LLVM IR. To use it, we first need to clone and build ONNX-MLIR on your development machine (not the Raspberry Pi, because building it on the Pi can be very time-consuming and might not succeed due to memory constraints).

1. Clone the ONNX-MLIR repository:

```bash
git clone https://github.com/onnx/onnx-mlir.git
cd onnx-mlir
```

2. Build ONNX-MLIR:

Please follow instructions on how to build in onnx-mlir repo. https://github.com/onnx/onnx-mlir.git

This will build the ONNX-MLIR project. This process might take some time.

## Step 3: Converting ONNX Model to LLVM IR

Once you have ONNX-MLIR set up, you can convert your ONNX model to LLVM IR as follows:

```bash
cd onnx-mlir/build
./onnx-mlir --EmitLLVMIR <path_to_your_onnx_model>
```

This command compiles the given ONNX model and outputs LLVM IR files and a shared library. Transfer the LLVM IR files and the shared library to your Raspberry Pi.

## Step 4: Compiling the LLVM IR on Raspberry Pi

After transferring the LLVM IR files to your Raspberry Pi, you can compile them into an executable using Clang:

```bash
# Compile LLVM IR to native ARM code
llc -march=arm modelName.ll -o modelName.s

# Assemble the ARM code
as -o modelName.o modelName.s

# Link the object file to create an executable
gcc -o modelName modelName.o -no-pie
```

## Step 5: Running Your Model

With the model compiled, you can now run it on your Raspberry Pi:

```bash
./modelName
```

## Conclusion

That's it! You've successfully run an ONNX model on Raspberry Pi using ONNX-MLIR and Clang. This approach allows you to leverage the ONNX ecosystem for your machine learning models and deploy them efficiently to edge devices like the Raspberry Pi.

By converting your ONNX models to LLVM IR, you gain the flexibility to compile and run them on various platforms, ensuring your machine learning applications can operate across different environments. Whether for hobby projects or professional deployments, using ONNX-MLIR opens up new possibilities for machine learning on edge devices.
