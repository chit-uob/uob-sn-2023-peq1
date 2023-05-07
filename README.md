# Speculated answer for Past Exam Questions 1 for Security and Networking 2023
link to questions: [https://canvas.bham.ac.uk/courses/65776/files/14520879](https://canvas.bham.ac.uk/courses/65776/files/14520879)

# DISCLAIMER
The following answer is **NOT** official. It is based solely on my understanding and interpretation of the lecture material. I am not responsible for any marks lost by following this answer.

In light of not having an official answer, I am hoping to make a collaborative effort to make a set of answer, to help students to better revise.

If you see any answer you don't agree on, please submit a [pull request](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-a-pull-request) to correct it, explaining why you think it is wrong. Or you can also contact me on Discord. Feel free to submit pull requests to improve the answers too.

## Question 1
### a)
As block ciphers only work on fixed size blocks, padding works by adding extra bits to the end of the message to make it a multiple of the block size. One method of padding is PKCS, which writes 01 if there is one byte left, 0202 if there are two bytes left, and so on. So that when decrypting, the receiver can check the last byte, and remove the padding accordingly.

### b)
For full disk encryption in AES, assuming that we don't have to frequently encrypt or decrypt some parts of the disk, I would use CBC mode, as CTR mode is susceptible to known plain text attack, where if the attacker knows the plain text and encrypted plain text for a part of the disk, they can alter the ciphertext so that the plain text would decrypt to another plain text. Also, CBC provides message integrity, if one part of the disk is altered, the decryption of the next block would be wrong, and the receiver would know that the disk has been tampered with.

### c)
Yes, the attack can learn the key. This can be done by a Man-in-the-middle attack. The attacker can pretend to be A to B, and B to be A, and have a session key with both of them. When A first sends number to B, C intercepts it and create its own number, and use it to talk to B, then B also send its number to A, which C also intercepts it and use its own number to talk to A. Now C has a session key with both A and B, and can decrypt and encrypt messages between them.
This can be illustrated by the following diagram:
![1](https://github.com/chit-uob/uob-sn-2023-peq1/blob/main/img/1.png?raw=true)

### d)
Since CBC encryption maps any 128 (or 256) bit block to another 128 (or 256) bit block, the attacker can change bits in the ciphertext to make it decrypt to another plaintext, in our context, another account number. However, using CBC mode, if the first block of ciphertext is changed, the second block will decrypt into something different, and the receiver would know that the message has been tampered with.