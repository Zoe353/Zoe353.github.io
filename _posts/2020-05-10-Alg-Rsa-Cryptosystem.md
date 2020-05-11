---
layout: post
title: "05/10/2020-Alg-Rsa--Cryptosystem"
markdown: kramdown
date: 2020-05-10
---

The typical setting for cryptography can be described via a cast of three characters: Alice and Bob, who wish to communicate in private, and Eve, an 
eavesdropper who will go to great lengths to find out what they are saying. For concreteness, let's say Alice wants to snd a specific message x, written in binary 
, to her friend Bob. She encodes it as $e(x)$, sends it over, and then Bob applies his decryption function $d()$ to decode it: $d(e(x))=x$. Here, $e()$ and $d()$ are appropriate 
transformations of the messages.  
RSA cryptosystem  
Bob chooses his public and secret keys.  
* He starts by picking two large(n-bit) random primes $p$ and $q$.
* His public key is $(N,e)$ where $N = pq$ and $e$ is a $2n$-bit number relatively prime to $(p-1)(q-1)$. A common choice is $e = 3$ because it 
permits fast encoding 
* His secret key is $d$, the inverse of $e$ modulo $(p-1)(q-1)$, computed using the extended Euclid algorithm.  
Alice wished to send message $x$ to Bob.  
* She looks up his public key $(N,e)$ ans sends him $y = (x^e mod N)$, computed using an efficient modular exponentiation algorithm.  
Bob decodes the message by computing $y^d$ mod $N$.  

Now, let's dive into the details and begin from some definitions and rulers.  
Def: $x$ is congruent to $y$ mod $N$ if x divided by N has the remainder $y$, which is notated as $x \equiv y(mod N)$.  
Here are some related facets:  
If $x \equiv y(mod N)$ and $a \equiv b(mod N)$, then  
* $x + a \equiv y + b (mod N)$  
* $x \cdot a \equiv y \cdot b (mod N)$  
* Given $a \in N$ and $N \in N$, $a$ has an inverse (mod N) iff gcd($a$, $N$) = 1.  
Def: The number $b$ is inverse of $a$ mod $N$ if $a \cdot b \equiv 1$ (mod $N$). $b$ will be notated as $a^{-1}$.  
* The inverse is unique.
The quick and simple proof is listed below: $a \cdot b \equiv 1 \equiv a \cdot c$ (mod $N$), $1 \leq a,b,c \leq N-1$; $(a^{-1} a)b \equiv 
(a^{-1} a)c$ (mod $N$); $b \equiv c$ (mod $N$)  
* $x \geq y \geq 0$, gcd($x,y$) = gcd($y,x$ (mod y))
 


