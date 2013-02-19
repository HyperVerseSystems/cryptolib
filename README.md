#DASM Cryptography library

An attempt to create a crypto library optimized for DCPU  
Currently, Hummingbird2.dasm is working, and can be used, and more functions are 
forthcoming. I am still actively working on all of this.  

##Todo List
* Add message authentication to hummingbird
* Finish up implementation of RG16 hash function
* Tie everything together in an actual library

If there is anything you would like to see added please let me know.
I plan on keeping code size down, but size can make way for good functional 
additions.

##Usage

###Hummingbird
A is the pointer to the 4 word Initialization Vector  
B is the pointer to the 8 word Key  
C is the pointer to the message  
and the length of the message is pushed to the stack  
You then JSR Encrypt to encrypt, and Decrypt to decrypt

Hummingbird encrypts over your message, and returns the pointer in A  

##Comparison
Below, I am going to compare a couple algorithms: Hummingbird-2,which I provide
here, CipherSaber-2 which is used by project entropy, Blowfish ( Entroper/DCPU-16-Blowfish ) 
and AES and Salsa20 which are standards here in the US. I have seen a blowfish 
implementation in DASM, so I will add that once I find it again and can test. I 
will also update as I find more, or as more are made.
Also, CipherSabre is being run with 20 as the pushed value for iterations, as the 
documentation lists that as the smallest value that is safe to use.

###Key Size
Key size is important for many reasons. First, you want a key long enough to
provide good security against a brute force. Current practice dictates that a 64 bit
(4 word) key is the absolute minimum, and even that is dangerously close to broken.  
If we want to look around, then the US NIST standard is AES which uses 128bit to 256 
bit keys (8-16 words).  
```
AES:         128-256 bit  
Salsa20:     128 bit  
Hummingbird: 128 bit  
Blowfish:    32-448 bit  
CipherSaber: 2048 bit  
```
Another consideration is that entropy's implementation of CipherSabre codes on bytes, 
so that key that would already be 128 words, is now 256. While CipherSabre is more 
secure against brute force, you also have to remember, or store, 256 bytes of data.

###Running Time
As there are no implementations for AES and Salsa20 yet, those will not be included. 
However, you can assume that the will both be much slower, as they operate on 32 bit 
words, which will slow down computation.
I used the included AvgRunSpeed code to run each algorithm. All algorithms 
are given 0's for the key and the message, and an 8 word message (expanded to 16 words
as per CipherSabre's usage instructions). Blowfish uses a 128 bit key and is initialized once. 
The times listed below are an average of 10 iterations of encryption and decryption.  
```
Hummingbird: 9.8 ticks  
CipherSabre: 65.7 ticks  
Blowfish   : (unable to run properly)  
```
As shown here, CipherSabre has almost 4 times the run speed of Hummingbird.  

###Code Size
Word size of the library compiled with organic (as short literal optimization is cleaner
and easier in organic)
```
Hummingbird: 452  
Blowfish:   2346
CipherSabre: 375
```
CipherSabre is 77 words smaller, but this is offset by the fact that you have a key that
is persistantly atleast 120 words long. Blowfish is huge, owing mainly to the large SBoxes 
needed.