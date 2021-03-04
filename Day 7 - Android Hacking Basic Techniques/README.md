### Android Hacking 101

Resources Used : https://tryhackme.com/room/androidhacking101

There are two types of Apps : Native and Hybrid Apps.

Native : Made for Mobile OS specifically, Android uses Java or Kotlin and IOS uses Swift or Objective-C.

Hybrid : These are made using Hybrid Frameworks like Flutter, React Native and so on.

As we are talking about mainly android hacking here if you create a android apk file it contain a .dex file, which contains binary Dalvik bytecode, and this contain Smali which is an assembly language that runs on Dalvik VM.

Smali Types :
v : void
z : boolean
B : byte
S : short
C : char
F : float
I : int
J : long
D : double
[ : array

Smali Registers by JesusFreke :

In dalvik's bytecode, registers are always 32 bits, and can hold any type of value. 2 registers are used to hold 64 bit types (Long and Double).

Two ways to spefiy how many registers are available in a method, .registers directive specifies the total number of registers in the method. and .locals directive specifies the number of non-parameter registers in the method.

When a method i called the parameters are place on the last n registers, for ex. if 2 arguements are passed and it has 5 registers (v0 - v4) the params would be placed in v3,v4.

Lets try to find a tool to first help us emulate stuff as well with android apps its a good call to find an appliation to emulate those apps on our computer.

https://www.genymotion.com/  : For Linux 
https://www.bignox.com/ : For Windows and Mac

Information Gathering -> Reversing -> Static Analysis -> Dynamic Analysis -> Report

To get some information we can search the app on playstore, and we can see the id of the package when we click on the app we search adn opened.

Now to debug these Apk's for them first we need to download the apk's for this we use a tool called **Android Debug Bridge (ADB)**.

How to extract the apk , For this we can use jadx suite to load a simple APK and look at its Java Code and jadx also has a gui version 

```bash
jadx -d [path-output-folder] [path-apk-or-dex-file]
```

We can also use this tool called Dex2Jar which converts apk to jar and then we inspect the Jar file in JD-GUI : http://java-decompiler.github.io/ 
```bash
d2j-dex2jar.sh /path/application.apk
```

Another tool to see the source code in smali is apktool 

```
apktool d app.apk
```