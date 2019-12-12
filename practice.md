1. A symmetric-key encryption scheme is defined as a 5-tuple (P,C,K,E,D). Complete the definition. (I.e., what restriction must hold on the elements in K,E and D?)

- P plaintext space
- C ciphertext space
- K keyspace
- E encryption function space e_k(m) = c
- D decryption function space d_k(c) = m

2. Describe the shift cipher (Caesar cipher). Explain one situation when the shift cipher gives you perfect security? (That is, you cannot determine the secret key with probability greater than 1/26.)

i) pick a secret key 0 < k < |P| and take m_i + k mod |P|

ii) perfect security -> no mode of operation (i.e., only encrypt one letter)
                     -> OR encrypt random gibberish so no frequency analysis

3. Briefly explain the following (giving a possible real-world example for each if you can): a) ciphertext-only attack b) known-plaintext attack c) chosen plaintext attack d) chosen ciphertext attack

a) COA: an attacker only knows c and not plaintext
   example: listen for c on an insecure channel and try to solve for k
b) KPA: attacker knows one or more m,c pairs
   example: when cracking enigma they knew that the Germans sent the weather report every morning
c) CPA: attacker has an oracle they can use to encrypt messages
   example: sending messages to yourself in WEP
d) CCA: attacker has an oracle they can use to decrypt messages
   example: sending messages to yourself in WEP

4) What needs to be specified in a security model?

- level of security
    - information theoretic (infinite)
    - complexity theoretic (O(...))
    - computational (n)
- attack model
    - COA
    - KPA
    - CPA
    - CCA
    - etc.
- security goal
    - IND
    - NM
    - etc.

5. A symmetric-key encryption scheme is semantically secure if Pr(m) = Pr(m|c). Explain what this means.

- we have as much chance of guessing the message given no information as we do given the ciphertext
- the ciphertext leaks no information about the plaintext

6. We briefly discussed unicity distance in class. Briefly, what does this mean?

- expected ciphertext length needed to be able to uniquely compute the plaintext by guessing keys
- i.e., no spurious keys remain

7. The one-time pad is unconditionally secure (it provides perfect security). What are two disadvantages of the one-time pad?

- key length needs to be equal to plaintext length
- need a new key each time (need a new secure channel for each)
- malleable

8. The one-time pad is malleable. Briefly explain what this means.

- an attacker can trivially modify the ciphertext to decrypt to a new plaintext that still makes sense

9. How is a stream cipher related to the one-time pad?

- stream cipher generates a key stream
- one time pad is like a stream cipher where we figure out the entire key beforehand
- both encrypt each byte with a new "key"

10. Name a block cipher that you can use to encrypt Carleton’s yearly calendar. If you cannot explain why not.

- with just a block cipher, not possible
- we need a mode of operation too
- AES with CBC or CTR mode would be a good choice

11. Briefly explain the following security goals of block ciphers a) diffusion b) confusion c) key size

- diffusion
    - changing a bit of plaintext should change at least half the bits of the ciphertext
- confusion
    - each bit in c depends on several bits in k
- key size
    - key should be small but not too small

12. DES was a great block cipher when it was created. Briefly explain the two reasons why it is no longer great today.

- key size is too small
- block size is too small

13. Describe double-DES (2DES) (not the DES2 from the assignment). Why is 2DES essentially no more secure than DES even though the number of key bits has doubled? Explain the attack involved.

- e_k2(e_k1(m))
- now we have 112 bits of total key length
- but thanks to meet in the middle attack it's actually just 57 bits of effective key length
    - not really more secure than the original 56

14. Triple-DES is a more secure replacement for DES than double-DES. It encrypts, decrypts then encrypts using DES with 3 different keys K1, K2, K3. Explain why anyone might want to use K1=K2=K3.

- using all same k would give us backwards compatibility with the original

15. What block cipher should you use today if you were to need to use a block cipher?

- AES

16. Briefly explain what a block cipher mode of operation is.

- mode of operation -> tells us how to encrypt messages longer than basic encryption scheme
- for block ciphers, this means messages longer than block length

17. What is the ECB (electronic code book) mode of operation? What is one flaw of ECB?

- encrypt each block individually
- problem: no semantic security

18. What is the CBC (cipher block chaining) mode of operation? Why should the initialization vector (IV) be different for each plaintext message we want to encrypt with this mode?

- for first block:
    - XOR with IV
    - encrypt with key
- for each block after:
    - XOR with previous ciphertext
    - encrypt with key
- with same IV we risk leaking information if we encrypt the same message twice
    - or even the same beginning of a message

19. There are three properties a cryptographic hash function might have. Briefly explain each. a) pre-image resistance b) second pre-image resistance c) collision resistance

- PIR
    - if we have H(x) it's hard to find x
- SPIR
    - if we have m, H(m), it's hard to find m' != m such that H(m) = H(m')
- CR
    - it's hard to find any pair m != m' such that H(m) = H(m')

20. The hash function from class J(x) = ( 1||x if |x| = n bits; 0||H(x) if |x| != n bits is collision resistant, if H(x) is collision resistant, but not pre-image resistant. Briefly explain why this so.

- 1||x will always be unique for x, so this is CR
- 0||H(x) will always be CR since H(x) is CR
- but if our first bit is 1, we know remaining bits are X, so we're not PIR

21. Recall the hash function from the assignment. The assignment asked you to prove that f ∈ CR ∧ g ∈ CR → h ∈ CR What we can say about the converse statement? h ∈ CR ??? → f ∈ CR ∧ g ∈ CR

- if g was not CR
    - we would have same input to f twice, so h wouldn't be CR
- if f was not CR
    - h would not be CR
- therefore both must be CR

22. If a hash function outputs an n-bit hash. How many times do we expect to compute this function (with random inputs) before we get a collision? What name to we give to this result?

- birthday attack
- 2^(n/2)

23. What is the MD construction?

- take an IV for first compression function, run block 1 through it
- previous f output as IV for next f, run block i through it
- at the end there may be some final steps

24. What is MAC? (What is this an acronym for?) What is it used for (which problem does it help address)?

- message authentication code
- provides
    - data integrity (to those with key)
    - data origin authentication (to those with key)

25. Briefly explain what each of the following mean w.r.t. MACs.

- existential forgery
    - an attacker can create some forgery m' for a known message-tag pair m,t
    - they have no choice as to what this forgery is
- selective forgery
    - an attacker can choose from a set of possible m' for a known message-tag pair m,t
- universal forgery
    - an attacker can compute the tag for any message

26. Explain why Hk(m) = h(k||m) and Hk(m) = h(m||k) are not secure MACs when the underlying hash function uses the Merkle-Damgard constructions? Go through the attacks for each.

- H(k||m)
    - secret prefix
    - vulnerable to length extension attack
- H(m||k)
    - secret suffix
    - try to find collisions

27. The CBC-MAC uses a block cipher in cbc-mode to create a hash function. Is this secure? When is secure and when is it not?

- it is only secure when we allow fixed length messages
    - otherwise, we are weak to length extension attacks
- we can fix this using EMAC instead

29. EMAC is the encrypted CBC-MAC. How does it differ from cbc-mac? Is this secure? What flaw in CBC-MAC does EMAC address?

- in EMAC we encrypt the last ciphertext block again
- this prevents length extension attacks
- now we can have arbitrary length messages

30. In class we saw why that the first version of SSL in Netscape was insecure. Briefly explain why. (The full exact details are not required)

- implementation used known values for seeding
- values were also skewed
    - ppid is often 1
- seed was public

31. A cryptographically secure pseudorandom bit generator (CSPRBG) should have two additional properties compared to a typical PRBG. Briefly, explain the following two properties.

- next-bit test
    - knowing previous bits does not reveal next bit
- forward security
    - knowing current bit does not reveal previous bits

32. Why is Blum-Blum-Shub...

- ...not really used in practice?
    - very slow
    - need to compute large exponent
    - N needs to be huge for it to be secure
- ...considered secure?
    - depends on difficulty of factoring pq
    - factoring large primes is considered a hard problem

34. Suppose you have a private key encryption scheme. Explain why you might want to send a MAC along with the ciphertext when sending messages.

- MAC would provide data integrity and data origin authentication
    - secret key crypto doesn't really provide this
