# Script versions using HEADER pseudo-opcode

Code: RSKIP50

Author: Sergio Demian Lerner

Creation Date: November 14th, 2016

Status: Draft

# Abstract

This RSKIP defines a new pseudo-opcode that is used to mark the existence of a header in the script. This header defines the script VM version, the script language version and can define upto 256 additional header bytes for future use.

# Motivation

RSK is definiting new functionality that is currently incompatible with the EVM. However RSK also wants to allow users to easily port their existing Ethereum solutions that do not make use of more advanced features. To acomplish this, we allow the VM code to define an optional header that specifies which virtual machine the code is targeted to.

# Specification

The new pseudo-opcode HEADER has value 0xfc. To specify a valid header, this opcode must stored at code address zero. If HEADER is stored at offset 0x00, then the following header values are interpreted:

The byte at code address 0x01 is the Executable version. Currently only version 0 is defined. This value currently does not affect how the contract is executed but contracts specifying a value different from 0 may be executed differently in the future. Therefore contract developers should always set this value to zero.
The byte at code offset 0x02 is the VM version. Currently only versions 0 and 1 are defined. Any other value will be currently interpreseted as 1. Developers are discouraged to set any value other than 0 or 1 because this may affect how the contract is executed in the future.

The byte at offset 0x03 is the extended header length (extHeaderLen). The extended header is left for future extensions.

If the header is present then the initial PC from where the contract is executed will be computed as:

 PC  = 4 + extHeaderLen

If the header is not present the PC willbe zero, as before.

If the HEADER opcode is present in the code at any offset diffrent of 0x00, it is intepreted as an invalid opcode.