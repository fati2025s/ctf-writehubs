# Blu CTF
**Platform:** blu ctf  
**Date:** 2026  
**Challenges Solved:** 1

**Category:** Mobile appsec  
**Challenge Name:** Green Chain



**What I did:** 
#### in diagram context
                    APK
                     │
          ┌──────────┴──────────┐
          │                     │
      flag.dat              steward.dat
          │                     │
          │              deriveAesKey()
          │                     │
          │              package + salt
          │                     │
          │                 SHA-256
          │                     │
          │                first 16 bytes
          │                     │
          │                 AES key
          │                     │
          │              decrypt steward.dat
          │                     │
          │               privateKeyHex
          │                     │
          └───────┐             │
                  ↓             │
          privateKeyHex + "flag"
                  │
               SHA-256
                  │
             first 16 bytes
                  │
              AES key
                  │
             decrypt flag.dat
                  │
                FLAG


The challenge provided a single APK file. I opened it in Android Studio on Windows and inspected its files and directory structure.
We first examined the APK and analyzed its internal structure.

Key aspects that could be assessed at first glance included:
AndroidManifest.xml 
classes.dex 
res/ 
assets/

Among the files in `assets`, two important files were found:

assets/ 
├── flag.dat 
└── steward.dat

The flag.dat file was unreadable and in binary format.

For example:

xxd flag.dat

Beginning of the file:
1307 fb28 3baf 7c1f 5c4d afe4 4375 06ef 
42cf 6fd6 05bd 5bf0 c332 0df8 db64 31cd 
...

It indicated that the data was likely encrypted.

### Finding the decryption logic

I searched flag in project and found very methods like: 

decryptFlag()

An analysis of this method revealed that flag.dat is processed as follows:
flag.dat 
│ 
├── 16 bytes → IV 
└── remaining bytes → Ciphertext

The AES key was not directly stored in flag.dat; it was derived from privateKeyHex.

### Encryption algorithms

The aesCbcDecrypt() method revealed that the application uses the following algorithm:

AES/CBC/PKCS5Padding

And `Cipher.DECRYPT_MODE` is used for decryption.

Therefore, for `flag.dat`, we have:

IV =  First 16 bytes
Ciphertext = The rest of the file
Algorithm = AES/CBC/PKCS5Padding

but we dont have AES key.

### Find first key

The AES key for flag.dat is derived from the decrypted privateKeyHex, rather than being stored directly in the APK.

First, another file named:

steward.dat

It is being deciphered.

To decrypt steward.dat, the `deriveAesKey()` method was examined.

Takes the application's package name.
Obtains the salt value from `buildSalt()`.
Concatenates these two values.
Computes the SHA-256 hash.
Uses the first 16 bytes of the SHA-256 hash as the AES key.

package name: com.invoxes.greenchain
Season value: 2026
and buildsalt() generate this value: eco-2026
so, final string: com.invoxes.greenchaineco-2026

SHA-256:
90034cebb6a7868d233884eedb45d72395ecd6486ff63c0075a22297bc2c1f00
just first 16 byte:
90034cebb6a7868d233884eedb45d723

### steward.dat decryption

Analysis showed that the file was 96 bytes in size.

16 bytes -> IV
80 bytes -> Ciphertext

IV: e0d51cfcc5a3bee5bcfbf62a6d28cf6b
key: 90034cebb6a7868d233884eedb45d723

Using: AES/CBC/PKCS5Padding 

that file decrypted.
output: d92b206f1525872e69988c816811f57c6085978e3aa37013baf956adaf8ab5fe

it is privateKeyHex.

### create final flag key

It was determined in decryptFlag():
privateKeyHex + "flag" = d92b206f1525872e69988c816811f57c6085978e3aa37013baf956adaf8ab5feflag
SHA-256:

4ff5baa7f6db11fb0883c1553eba510d3d5501044564d5b99b8e559abc42de32

first 16 bytes:
4ff5baa7f6db11fb0883c1553eba510d

### flag.dat decrypted 

with key:
4ff5baa7f6db11fb0883c1553eba510d

algorithm:
AES/CBC/PKCS5Padding

### What I Learned
How to inspect an Android APK and identify interesting files in assets/.
How to read Smali code and understand the behavior of a decryption function.
How to identify the encryption algorithm, mode, IV, and key derivation process.
How to trace a multi-stage key derivation instead of looking for the final key directly.
How to use Python and PyCryptodome to reproduce Android AES-CBC decryption.





