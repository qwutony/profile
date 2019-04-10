---
layout: post
title:  "Seckids: The difference between Encoding, Encryption and Hashing"
---

Welcome back to the **'Security:Kids'** series where I try to explain many concepts of cyber security in a simplistic and interesting manner. Here is an interesting topic that is important for aspiring security students to understand - what is the difference between encoding, encryption and hashing?

Before we should try to understand the differences, we should try to find out what is similar between all of these terms.

**Data Transformation**

The simplest explanation is that encoding, encryption and hashing are all processes for converting data from one format to another. Generally speaking, all three methods transform data from a human-readable state to something that is nearly impossible to read. However that is probably as far as their similarities go.

The fundamental difference between the data transformation techniques depends on the *intention and reasoning behind the need to convert data*. In other words, why does the data need to be transformed?

**Encoding**

Encoding is the process of transforming data to another format. Generally, encoding uses a specific publically available algorithm to convert information to cipher text, which is then easily able to be reversed. The most common format for converting letters, symbols and numbers is ASCII, which is an acronym for the American Standard Code for Information Exchange. For more information, see: <a href='https://en.wikipedia.org/wiki/ASCII'>ASCII (Wikipedia)</a>.

Fundamentally, the purpose of encoding is to convert data into formats that are easier to be used by other external processes. This can involve converting to a format that is easier to comprehend by the program, or encoding to reduce the overall size of the data.

**Encryption**

Encryption is also the process of transforming data to another format. The difference is that encryption involves the use of an encryption algorithm to convert the original message, usually in plaintext format, to an unreadable ciphertext. The ciphertext can only be reversed using an encryption key, although it is possible to decrypt a message without a key, but substantial resources are required.

The purpose of encryption is to ensure information can only be consumed by authorized entities. It can be used to prevent data theft since the attacker will be unable to decipher the information. Therefore, encryption is used to for the **security of data**.

**Hashing**

Hashing also allows for the transformation of data from one format to another. Hashing uses a mathematical algorithm to generate a value (known as a hash or digest) from data. Due to the nature of hashing, it is considered to be a one-way function, meaning that data can be turned into a hash, but a hash would be difficult to reconstruct back to the same data. 

Since the hashing of identical data inputs will always result in the same output, hashing can be used to **verify the integrity of data**. 