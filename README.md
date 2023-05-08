# Speculated answer for Past Exam Questions 1 for Security and Networking 2023
link to questions: [https://canvas.bham.ac.uk/courses/65776/files/14520879](https://canvas.bham.ac.uk/courses/65776/files/14520879)

link to Questions 2 (work in progress): [https://github.com/chit-uob/uob-sn-2023-peq2](https://github.com/chit-uob/uob-sn-2023-peq2)

# DISCLAIMER
The following answer is **NOT** official. It is based solely on my understanding and interpretation of the lecture material. I am not responsible for any marks lost by following this answer.

In light of not having an official answer, I am hoping to make a collaborative effort to make a set of answer, to help students to better revise.

If you see any answer you don't agree on, please submit a [pull request](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-a-pull-request) to correct it, explaining why you think it is wrong. Or you can also contact me on Discord. Feel free to submit pull requests to improve the answers too.

## Question 1
### a)
As block ciphers only work on fixed size blocks, padding works by adding extra bits to the end of the message to make it a multiple of the block size. One method of padding is PKCS, which writes 01 if there is one byte left, 0202 if there are two bytes left, and so on. So that when decrypting, the receiver can check the last byte, and remove the padding accordingly.

### b)
For full disk encryption in AES, assuming that we don't have to frequently encrypt or decrypt some parts of the disk, I would use CBC mode, as CTR mode is susceptible to known plain text attack, where if the attacker knows the plain text and encrypted plain text for a part of the disk, they can alter the ciphertext so that the plain text would decrypt to another plain text. Also, CBC provides message integrity, if one part of the disk is altered, the decryption of the next block would be wrong, and the receiver would know that the disk has been tampered with.
> I'm not sure on this one, one can also argue CTR is better because don't have to encrypt or decrypt the entire disk if only a small part is changed, and that it runs quicker because it can be done in parallel. 

### c)
It is computationally impossible to learn the already shared key If A and B already performed the DH key exchange. However, the attacker can prevent this from happening and make it so that A and B thought they exchanged a key. This can be done by a Man-in-the-middle attack. The attacker can pretend to be A to B, and B to be A, and have a session key with both of them. When A first sends number to B, C intercepts it and create its own number, and use it to talk to B, then B also send its number to A, which C also intercepts it and use its own number to talk to A. Now C has a session key with both A and B, and can decrypt and encrypt messages between them.
This can be illustrated by the following diagram:
![1](https://github.com/chit-uob/uob-sn-2023-peq1/blob/main/img/1.png?raw=true)

### d)
Since CBC encryption maps any 128 (or 256) bit block to another 128 (or 256) bit block, the attacker can change bits in the ciphertext to make it decrypt to another plaintext. However, without the key, the attacker don't know what will the plaintext be after altering the ciphertext, and it is statisically impossible for the attacker to change the bits to make something readable. Also, using CBC mode, if the first block of ciphertext is changed, the second block will decrypt into something different, and the receiver would know that the message has been tampered with.

## Question 2
### a)
Man-in-the-middle attack is when the attack intercepts the communication between two parties, and can change the message before sending it to the other party, and can even pretend to be the other party.

### b)
No, using TLS is not enough to protect against malware running on the client. TLS is used to encrypt the message between the client and the server, and authenticating each other. However, malware can still read the already decrypted message on the user's computer, stealing credit card information that way.

### c)
This protocol is not secure, it is susceptible to replay attack, since the message {Pay Elvis £5} is encrypted using K_CS, which always stays the same, The next time A talks to B with another message and another nonce, the attacker can simply remove the actual message and replace it with {Pay Elvis £5}K_CS, and B wouldn't know that the message has been tampered with.

### d)
It is possible for the attacker to learn M without knowing the private key of A.
![2](https://github.com/chit-uob/uob-sn-2023-peq1/blob/main/img/2.png?raw=true)
C intercepts the initial message from A to B, finding out N_A, and editing it so that the signature of C is sent to B instead. Then B thinks C wants to talk to them, and send N_A, N_B, and its signature encrypted using the public key of C, where C can decrypt it using its private key, learn N_B, and send the same message to A, but this time encrypting it with A's public key. Then A thought B has replied, and sends the message encrypted using the hash of N_A, and N_B, which C has learnt already. Then C can decrypt the message and learn M.

## Question 3
### a)
Cross-site scripting is when the attacker injects malicious code into a website, and when the website is visited, the malicious code will be run. There is two main types of XSS, the first type is reflected XSS, where the malicious code is reflected off the web server and only the user issuing the malicious request is affected. The second type is stored XSS, where the malicious code is stored on the web server, and anyone visiting the website will be affected. Which can be used to steal information from the user, or even spread the attack further.


### b)
The four security weaknesses, ranked by most severe to least severe, are:
1. XSS: The code doesn't preform any sanitization on the user's input, so the attacker can preform cross-site scripting attack by having a malicious message, and when other users view the message, the code will run on their browser, and can steal information from the user, and further spread the attack by posting the same code too.
2. SQL injection: The code doesn't preform any sanitization on SQL query using the user's input, so the attacker can inject SQL code into the query, and steal information from the database, or even delete the database.
3. Password stored in plain text: The request is sent with a get request, where the parameters, including the user's password in plain text, are sent in the URL, which can be seen will appear in the browser history, be logged by the server and may be sent to third parties (eg. Google analytics), and can be easily modified by the attacker.
4. No authentication: The post isn't authenticated, the server code doesn't actually check the password when adding the message in a user's name, so anyone can posta message in anyone's name if they know their username.

The reason I ranked them this way is, with XSS attack, the attacker can run whatever code they want on unsuspecting users' browser, which with Javascript, the attacker can do a lot of dangerous things. Then the second is SQL injection, as it allows the attacker to do whatever they want in the server database. The third is password stored in plain text, as it exposes the user's password and increases the severity of the other vulnerabilities. The last one is authentication, it allows anyone to post as anyone, but in terms of damage done, this is the least significant. 

### c)
The four fixes are:
1. XSS: Sanitize the user's input, so that the user can't inject malicious code into the website.
2. SQL injection: Use prepared statement, so that the SQL query is precompiled, and the user's input is treated as a parameter, and not part of the query.
3. Password stored in plain text: Use POST request instead of GET request, so that the parameters are sent in the body of the request, and not in the URL. Additionally, the password could be stored as a hash to increase security. 
4. No authentication: Authenticate the user before adding the message, so that only the user can post a message in their name.


# Checked by
If you find these answers correct, you can submit a pull request to add your name here, to add to the credibility of the answers.
- Chit
- Tom

# Plug
If you find this helpful, please consider following my blog on [Chit's Programming Blog](https://blog.cpbprojects.me), where I post about my programming projects, and other things I find interesting.

You can also give this repository a star ⭐, so that more people can find it.
