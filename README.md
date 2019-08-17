# CLR-via-C-Notes
My notes on the book "CLR via C#" authored by Jeffrey Richter.

## The CLR
The common language runtime (*CLR*) is a runtime that can be used by various programming languages.
The core features of the CLR are:

* Memory management 
* Assembly loading
* Security
* Exception handling
* Thread synchronization

## Managed modules
Compilers that target the runtime produce *managed modules* that contain these components:

* A PE32(+) header
  - Type of file GUI, CUI or DLL
  - Timestamp
  - Native CPU code information
* CLR header
  - Required CLR version
  - Flags
  - Entry point for the managed module
  - Module metadata location
  - Resources
* Metadata
  - Defined type and member tables
  - Referenced type and member tables
* IL code

## Assemblies
Compilers do the work of turning managed modules and resources files into an *assembly*.
Each assembly can be an executable application or a DLL. 
Assemblies contain a manifest describing the modules and resources inside.
The CLR is responsible for managing the execution of code contained within these assemblies.

An assembly is the smallest unit of:
* Reuse
* Security
* Versioning

## IL code
Managed assemblies contain both metadata and IL.
IL is an object-oriented machine language created by Microsoft.
The IL assembly language allows a developer to access all of the CLR’s facilities which might not be available when using high-level languages.
IL can be easily reverse engineered from its assembly.

## Executing assembly code

The steps involved in running a managed application:
1. The EXE file header determines the process version 32-bit or 64-bit
2. The appropriate MSCorEE.dll is loaded into the process' address space.
3. The process' primary thread calls a method inside MSCorEE.dll that 
   1. Initializes the CLR
   2. Loads the EXE assembly
   3. Calls the entry point method (Main)
   
When a managed application calls a method for the first time, a component inside the CLR called the JIT compiler compiles the IL code into native CPU instructions. A performance hit is incurred due to verification and compilation; dynamic, non-shareable memory is allocated. Subsequent calls execute at full native speed.

Managed code can easily call functions contained in DLLs by using a mechanism called P/Invoke (for Platform Invoke). 

Multiple managed applications can run in a single Windows virtual address space.

# Compilation
## Switches
`/unsafe` allows unsafe code: working directly with memory addresses and manipulation of bytes

# The CTS
The Common Type System (CTS) describes how types are defined and how they behave.

Types can have 0+ members. The CTS specifies the rules for type visibility and access, type inheritance, virtual methods, object lifetime, etc.

All types must inherit from a predefined type `System.Object` which allows you to:
* Compare two instances for equality. 
* Obtain a hash code for the instance. 
* Query the true type of an instance. 
* Perform a shallow (bitwise) copy of the instance. 
* Obtain a string representation of the instance object’s current state.

# The CLS
The Common Language Specification (CLS) specifies the minimum set of features compilers must support to generate types compatible with other components written by other CLS-compliant languages on top of the CLR.

The CLR/CTS supports a lot more features than the subset defined by the CLS.

CLS rules don’t apply to code that is accessible only within the defining assembly.

Telling the compiler to check for CLS compliance in C#:
`[assembly: CLSCompliant(true)]`

For a complete list of CLS rules, refer to the “Cross-Language Interoperability” section in the .NET Framework.
