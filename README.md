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
As soon as I tidy up my code a bit, I will post up some comparisons between my algorithms, and other popular implementations on
DCPU