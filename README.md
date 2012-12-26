#DASM Cryptography library

An attempt to create a crypto library optimized for DCPU  
Currently, Hummingbird2.dasm is the only file being worked on.  
AES and SALSA will come up next for those that want more security.  
Finally, I will work on a block cipher optimized for the DCPU.  

##Usage

###Hummingbird
A is the pointer to the Initialization Vector  
B is the pointer to the Key  
C is the pointer to the message  
and the length of the message is pushed to the stack  
You then JSR Encrypt to encrypt, and Decrypt to decrypt

Hummingbird encrypts over your message, and returns the pointer in A  

##Comparison
Below, I am going to compare a couple algorithms: Hummingbird-2,which I provide
here, CipherSaber-2 which is used by project entropy, and AES and Salsa20 which are 
standards here in the US. I have seen a blowfish implementation in DASM, so I will 
add that once I find it again and can test. I will also update as I find more, or as
more are made.
Also, CipherSabre is being run with 20 as the pushed value for iterations, as the 
documentation lists that as the smallest value that is safe to use.

###Key Size
Key size is important for many reasons. First, you want a key long enough to
provide good security against a brute force. Current practice dictates that a 64 bit
(4 word) key is the absolute minimum, and even that is dangerously close to broken.  
If we want to look around, then the US NIST standard is AES which uses 128bit to 256 
bit keys (4-8 words).  
```
AES: 128-256 bit  
Salsa20: 128 bit  
Hummingbird: 128 bit  
CipherSaber: 2048 bit  
```
Another consideration is that entropy's implementation of CipherSabre codes on bytes, 
so that key that would already be 128 words, is now 256. While CipherSabre is more 
secure against brute force, you also have to remember, or store, 256 bytes of data.

###Running Time
As there are no implementations for AES and Salsa20 yet, those will not be included. 
However, you can assume that the will both be much slower, as they operate on 32 bit 
words, which will slow down computation.
My method of timing below was to use the dcpu clock ticking at 60Hz. An interrupt 0 
starts the clock, and interrupt 1 gives me a completion time in ticks. All algorithms 
are given 0's for the key and the message, and an 8 word message (expanded to 16 words
as per CipherSabre's usage instructions). The times listed below are an average of 
10 iterations of encryption and decryption.  
```
Hummingbird: 10.9 ticks  
CipherSabre: 42.3 ticks  
```
As shown here, CipherSabre has almost 4 times the run speed of Hummingbird.  

###Code Size
Word size of the library compiled with organic (as short literal optimization is cleaner
and easier in organic)
```
Hummingbird: 452  
CipherSabre: 375