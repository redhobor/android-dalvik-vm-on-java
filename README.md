# android-dalvik-vm-on-java
Automatically exported from code.google.com/p/android-dalvik-vm-on-java

This project's objective is to develop a pure Java implementation of the Android's Dalvik virtual machine.

To study the concept and architecture of Dalvik VM, Koji Hisano is developing this for J2ME CLDC environments as on-going research at "eflow Inc."

Currently supported functions
Dalvik Execution file format (.dex)
Complete Dalvik instruction set
J2ME CLDC API
Multi-thread (include synchronized block, wait and notify)
Unsupported functions
Android frameworks (as implemented on CLDC)
Verifier
Debugger
Some array types
Annotation (as implemented on CLDC)
Enum (as implemented on CLDC)

Tutorial1  
How to run a program 
tutorial Updated Mar 3, 2009 by his...@gmail.com
Introduction
This tutorial describes how to run a simple program on this vm step by step.

Details
1. Compile the below code and package it as App.jar

package tutorial1;

public final class HelloWorld {
        public static void main(final String[] args) {
                System.out.println("Hello, World!");
        }
}
2. Convert App.jar to App.dex which is a Dalvik Executable file

dx --dex --output=<absolute file path to tutorial1/App.dex> <absolute file path to tutorial1/App.jar>
3. Write a launcher code and run it

import java.io.*;

import jp.eflow.hisano.dalvikvm.VirtualMachine;

public final class RunHelloWorld {
        public static void main(final String[] args) {
                final File dexFile = new File("tutorial1", "App.dex");
                final String absoluteMainClassName = "tutorial1.HelloWorld";

                final VirtualMachine vm = new VirtualMachine();
                vm.load(toBytes(dexFile));
                vm.run(absoluteMainClassName, new String[0]);
        }

        private static byte[] toBytes(final File dexFile) {
                final byte[] bytes = new byte[(int)dexFile.length()];
                DataInputStream in = null;
                try {
                        in = new DataInputStream(new BufferedInputStream(new FileInputStream(dexFile)));
                        in.readFully(bytes);
                } catch (IOException e) {
                        System.err.println("The specified dex file path is invalid: " + dexFile.getName());
                        System.exit(-1);
                } finally {
                        if (in != null) {
                                try {
                                        in.close();
                                } catch (IOException e) {
                                }
                        }
                }
                return bytes;
        }
}
