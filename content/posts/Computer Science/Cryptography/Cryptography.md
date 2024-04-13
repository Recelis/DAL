---
title: "Cryptography"
date: 2024-04-13T11:34:45+10:00
draft: true
---

**Symmetric encryption**

https://www.khanacademy.org/computing/computers-and-internet/xcae6f4a7ff015e7d:online-data-security/xcae6f4a7ff015e7d:data-encryption-techniques/a/symmetric-encryption-techniques

This is any technique in which the same key is used to encrypt and decrypt data. It is symmetric in that the process for encrypting is simply reversed when decrypting data.

**Caesar Cipher**

The Caesar Cipher is an encryption method where data is encrypted by shifting everything by a number of letters.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/0c6a6719-3fa4-4bdc-98cb-a552932a7132/bab56463-6a6a-4d2e-93ab-7f446ed195cc/Untitled.png)

**Vigenere Cipher**

This is more complicated in that it shifts all the letters by a single key.

![Screen Shot 2024-01-01 at 8.52.28 am.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/0c6a6719-3fa4-4bdc-98cb-a552932a7132/d9551bb5-a6eb-402c-94da-7a781e29cacc/Screen_Shot_2024-01-01_at_8.52.28_am.png)

This means that every key will have a differing shift amount which makes it difficult to decode even when the letters of the original data are the same. You would go through a Vigenere table to then encode and decode the message data. If the message is longer than the key, then the key just wraps again.

![Screen Shot 2024-01-01 at 8.54.22 am.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/0c6a6719-3fa4-4bdc-98cb-a552932a7132/7c2c8631-c690-4b15-8619-7aa8d68376d3/Screen_Shot_2024-01-01_at_8.54.22_am.png)

However, you can use **Frequency Analysis** to crack this code because words that are common such as “the” will be repeated over and over again.

**AES-128**

This is an encryption technique that uses keys that have 128 bits long, leading to $2^{128}$ combinations. It also requires a sequence of 10 Mathematic operations on each bit. This means that the number of combinations to brute force is 10 times of the combinations. Frequency analysis also won’t work because the sequence of 10 mathematical operations obscures repeated message words.

**Public Key Encryption**

This is a process for two computers to exchange secure data with each other using **asymmetric encryption** where it uses different keys for encryption and decryption.

There are 4 steps:

**Key Generation** - creates a **private key** and a **public key** using **RSA algorithm**.

**Key exchange** - of the public keys

**Encryption** - sending computer encrypts data using receiving computer’s public key and a mathematical operation.

**Sending encrypted data**

**Decryption** - receiver decrypts message using their own private key.

**RSA encryption**

**Fundamental theorem of arithmetic**

https://www.khanacademy.org/computing/computer-science/cryptography/modern-crypt/v/the-fundamental-theorem-of-arithmetic-1

Essentially Euclid of Alexandria found that all numbers can be factorised into one unique group of prime numbers. This is also called **prime factorisation** so basically every number > 1 has a unique key.

**Public Key Cryptography**

works by having two parties a shared public key, e.g. A, then they each add their Private Keys B, C ⇒ 1:AB, 2:AC and exchange this. Afterwards they send this over to each other. 1:AC 2:AB and add their own private key to the message. 1:ACB 2:ABC. This leads to them having the same key combination.

**Discrete Logarithm problem**

Using exponents `3^x mod prime_number = modulus (integer)` Where modulus can be any number from 0 to the prime_number. This allows us to easily create a value. But when you reverse it, it is very difficult to figure out x from the integer.

**Diffie-hellman key exchange**

This uses the **Discrete Logarithm problem** as well as the **Fundamental theorem of arithmetic** to create a shared key between two parties that cannot be decrypted by a third party. The way it works is that party A and party B would agree on a generator and a modulus (3^x mod 17).

Now party A would create a random number (private key) for the exponent and calculates the modulus (public key) which they send to party B. Party B would also create a private key and calculates the modulus to send to party A.

Now they would take the public key that they got and raise it to the power of their private key and mod prime it to generate a new shared private key.

**X.509 Certificates**

https://learn.microsoft.com/en-us/azure/iot-hub/reference-x509-certificates

https://datatracker.ietf.org/doc/html/rfc5280

These certificates are the ones used for SSL/TLS to represent the public key as well as other fields to represent the root CA, encryption algorithm, and validity. There have been three iterations of these certificates.
