# Solutions to Exercises in *Modern Cryptography: Theory and Practice*

## Chapter 1
1) **What is the difference between a protocol and an algorithm?**
* *A protocol is a well-defined procedure that involves two or more participating parties.*
* *An algorithm is a well-defined procedure that an entity uses to complete a computation or goal oriented processes.*
2) **In Prot 1.1 Alice can decide HEADS or TAILS. This may be an unfair advantage for some applications. Modify the protocol so that Alice can no longer have this advantage. Hint: let a correct guess decide the side.**

    We use the hint, **let a correct guess decide the side** [of the coin], to derive a new protocol.
    
    **PREMISE**: 
    
        Alice and Bob have agreed to use a "magic function" f with properties as specified in Property 1.1.    
        Additionally, a correct guess by Bob is to represent HEADS and an incorrect guess is TAILS.
        
    1. Alice picks a large, random integer `x` and computes `f(x)`.
    2. Alice reads `f(x)` to Bob. Alice also informs Bob of her call, HEADS or TAILS.
    3. Bob tells Alice his guess of `x` as even or odd.
    4. Alice reads `x` to Bob.
    5. Bob verifies `f(x)` and sees the correctness or incorrectness of his guess, resulting in HEADS or TAILS respectively.
    
3) **Let function `f` map from the space of 200-bit integers to that of 100 bit ones with the following mapping rule: 
    ![](https://latex.codecogs.com/gif.latex?f%28x%29%5Cstackrel%7Bdef%7D%7B%3D%7D%28%5Ctext%7Bthe%20most%20significant%20100%20bits%20of%20%7Dx%29%5C%20%5Coplus%5C%20%28%5Ctext%7Bthe%20least%20significant%20100%20bits%20of%20%7Dx%29)**
    1. Is `f` efficient?
    
        Yes. Xor operations are very fast.
    2. Does `f` have the "Magic Property I"?
    
        For `f` to have "Magic Property I", it must satisfy two conditions:
        1. Given `x` it is easy to compute `f(x)`.
        2. Given `f(x)` it is impossible to extrapolate information about `x`.
        
        Since we are focused primarily on determining the parity of `x`, we can limit our simple analysis to 4 distinct cases. Both the 100th and 200th bit are 1's, both are 0's, and each case in which they differ from each other. By building a simple truth table (see below) we can see that parity of the original number's parity bit is not leaked via this hash function assuming all bits are randomly set.
        
        | 100th bit | Parity bit    |  A ^ B    |
        | --------- | ----------    | ------    |
        | 1         | 1             | 0         |
        | 1         | 0             | 1         |
        | 0         | 1             | 1         |
        | 0         | 0             | 0         |
        
        So, yes, `f` has "Magic Property I"
        
    3. Does `f` have the "Magic Property II"?
    
        For `f` to have "Magic Property II" `f(x)` must produce distinct outputs for every input `x`. Upon simple observation we can immediately tell that this is impossible because the domain is ![](https://latex.codecogs.com/gif.latex?2%5E%7B200%7D) and the range is ![](https://latex.codecogs.com/gif.latex?2%5E%7B100%7D) which means that a significant number of collisions must exist. Additionally, we can prove this further by providing a counter example to "Magic Property II". 
        
        ![Counter-example to Magic Property II for Question 1.3.3](https://latex.codecogs.com/gif.latex?%5C%5C%20f%28111...1%29%20%3D%20000...0%5C%5C%20f%28000...0%29%20%3D%20000...0)
        
    4. Can this function be used in Prot 1.1?
    
        No, this function cannot be used for `f` in Prot 1.1 because it does not meet the requirements as stated by the premise.
        
4) **Is an unbroken cryptographic algorithm more secure than a known broken one? If not, why?**

    Generally, no, but it depends. Cryptographic algorithms can be broken to varying degrees so just because an algorithm has been "broken" doesn't mean it is useless or significantly worse off. In fact, due to the fact that the algorithm was broken, we know that it must have been investigated to some degree. Additionally, the degree in which the algorithm has been broken can be an indication to the underlying strength of the algorithm. However, sometimes a proof by reduction to contradiction can be done on unbroken crypto which allows some degree of confidence. 
    
5) **Complex systems are error-prone. Give an additional reason for a complex security system to be even more error-prone.**

    There are plenty of answers that could be used here. One example could be that added complexity invites implementation errors.

## Chapter 2
1) **What sort of things can an active attacker do?**
    
    * They can listen to all traffic on the network.
    * They can initiate conversation with any other principal on the network.
    * They have the opportunity to be a receiver to any principal.
    * They can impersonate any other principal.
    
2) **Under the Dolev-Yao Threat Model, Malice is very powerful because he is in control of the entire open communications network. Can he decrypt or create a ciphertext message without using the correct key? Can he find the key-encryption key from a ciphertext message? Can he predicate a none value?**

    1. Malice cannot decrypt ciphertext without the correct key.
    2. Malice cannot find the long-term key from a ciphertext message.
    3. Malice cannot predict nonce values.
    
3) **What is the role of Trent in authenticated key establishment protocols?**

    Trent is supposed to be a Trusted Third Party to Alice and Bob. Trent's job is to act as a directory service which acts like a name registration service; it maintains an indexed database of principals and related information that is required to perform authentication. 
    
 4) **What is a long-term key, a key-encryption key, a short-term key, and a session key?**
 
    * A key-encryption key, also known as a long-term key, is a cryptographic key used primarily for small messages, such as other encryption keys.
    * A session key, also known as a short-term key, is a cryptographic key used in scenarios where high volume data may be processed. A session key should not be reused after a session has been terminated as the expectation is that so much data has been encrypted/decrypted with this key that any potential cryptanalysis that could be performable on the traffic would be feasible at such high volumes.
   
5) **Why with the perfect encrypotion and the perfect message authentication services, can authentication protocols still be broken?**

    Because designing cryptographic systems is error prone. Just because one uses perfect cryptographic primitives does not mean that they will be combined in a compatible or correct way. For example, one might apply encryption where message authentication is required allowing information to be subsituted with old information, allowing replay attacks or impersonating another principal.
    
6) **What is a nonce? What is a timestamp? What are their roles in authentication or authenticated key establishment protocols?**
    
    * **Nonce**: A number used only once.
    * **Timestamp**: A timestamp is an indicator of when a message was sent. It, depending on the circumstance, can be used as a subsitute for a nonce. This requires that a good, reliable source of time is available.
    
    The role of a nonce in an authenicated key establishment protocol is to enable entity authentication. This allows a principal to determine if another principal has responded to a message only after it has been sent and being able to do this cryptographically.
    
7) **Why must some messages transmitted in authentication or authenticated key establishment protocols be fresh?**

    If a message is not "fresh" it could contain previously used cryptographic material, such as a session key, that may be compromised long after it was used. By determining that a message is fresh, you can prevent message replay attacks.
    
8) **How can a principal decide the freshness of a protocol message?**

    A principal can use a challenge-response pattern in their protocol using a nonce to ensure entity authentication. The initiating principal adds a nonce to their message. The responder ensures to add that nonce to their response while using message authentication to keep the integrity of the message. The initiator can then validate that the nonce returned is the one that was sent which will then ensure that the response was not resent from a previous communication.
    
9) **For the perfect encryption notation ![](https://latex.codecogs.com/gif.latex?%5C%7BM%5C%7D_K), differentiate the following three properties: (i) message confidentiality, (ii) key secrecy, and (iii) message authentication.**

    1. **Message confidentiality**: This ensures that the message is incomprehensible by anyone without the encryption key. This protects the knowledge transmitted in the message.
    2. **Key Secrecy**: This ensures that the ciphertext does not leak information about the key used during encryption.
    3. **Message Authentication**: This ensures that the message does not get altered during transmission without detection. This prevent unseen and unauthorized modifications.
    
10) **Provide another attack on Protocol "Session Key From Trent" (Prot 2.2), which allows Malice to masquerade not only as Bob toward Alice as in Attack 2.1, but at the same time also as Alice toward Bob, and hence Malice can relay "confidential" communications between Alice and Bob. Hint: run another instance of Attack 2.1 between Malice("Alice") and Bob.**

    ![](https://github.com/d0nutptr/Modern-Cryptography-Solutions/blob/master/2.10.PNG?raw=true)
    
1. Alice sends to Malice("Trent"): `Alice, Bob`
2. Malice("Alice") sends to Trent: `Alice, Malice`
3. Trent sends to Alice: ![](https://latex.codecogs.com/gif.latex?%5C%7BK_1%5C%7D_%7BAT%7D%2C%5C%20%5C%7BK_1%5C%7D_%7BMT%7D)
4. Alice sends to Malice("Bob"): `Trent, Alice, `![](https://latex.codecogs.com/gif.latex?%5C%7BK_1%5C%7D_%7BMT%7D)
5. Malice sends to Trent: `Malice, Bob`
6. Trent sends to Malice: ![](https://latex.codecogs.com/gif.latex?%5C%7BK_2%5C%7D_%7BMT%7D%2C%5C%20%5C%7BK_2%5C%7D_%7BBT%7D)
7. Malice("Alice") sends to Bob: `Trent, Alice, `![](https://latex.codecogs.com/gif.latex?%5C%7BK_2%5C%7D_%7BBT%7D)
8. Bob sends to Malice("Alice"): ![](https://latex.codecogs.com/gif.latex?%5C%7B%5Ctext%7BHello%20Alice%2C%20I%27m%20Bob%21%7D%5C%7D_%7BK_2%7D)
9. Malice decrypts the message from Bob using their shared cryptographic key and then reencrypts the message with Malice's and Alice's shared cryptographic key. Malice("Bob") then sends to Alice: ![](https://latex.codecogs.com/gif.latex?%5C%7B%5Ctext%7BHello%20Alice%2C%20I%27m%20Bob%21%7D%5C%7D_%7BK_1%7D)

11) **What is the difference between *Message Authentication* and *Entity Authentication*?**

    * **Message Authentication** ensures that a message has not been altered since it has been sent out and then received.
    * **Entity Authentication** ensures that a responder could only have crafted a particular message *after* receiving the intiating message to do so.
    
12) **Provide another attack on the Needham-Schroeder Authentication Protocol in which Alice (and Trent) stays offline completely.**

    I, originally, found this question a bit confusing. I will provide my solution below that I believe Mao intended the user to respond with. 
    
    ![](https://github.com/d0nutptr/Modern-Cryptography-Solutions/blob/master/2.12.PNG?raw=true)
    
1. Malice("Alice") sends to Bob ![](https://latex.codecogs.com/gif.latex?%5C%7BK%5E%7B%27%7D%2C%20%5Ctext%7BAlice%7D%5C%7D_%7BK_%7BBT%7D%7D)
2. Bob decrypts the message. He then checks Alice's ID and sends to Malice("Alice") ![](https://latex.codecogs.com/gif.latex?%5C%7B%5Ctext%7BI%27m%20Bob%2C%20%7DN_B%5C%7D_K_%7B%27%7D)
3. Malice("Alice") sends to Bob: ![](https://latex.codecogs.com/gif.latex?%5C%7B%5Ctext%7BI%27m%20Alice%21%2C%20%7DN_B%20-%201%5C%7D_K_%7B%27%7D)

13) **Does digital signature play an important role in the Needham-Schroeder Public-key Authentication Protocol? Hint: consider that the protocol can be simplified to the version which only contains message lines 2, 6, 7.**

    Yes. Otherwise Malice could modify the public keys being received and begin intercepting communications.

## Chapter 3
1) **Throw two dice one after the other. Find the probability of the following events**:
    
    1. **sum is 7, 1, and less than or equal to 12;**
    
        For this question, we'll assume that the author intended these to be 3 separate prompts, otherwise it would be simple to prove the probability as 0 since the sum of two dice can't be both 7 and 1. 
        
        1. ***Sum is 7***
        
        ![](https://latex.codecogs.com/gif.latex?E%20%3D%20%5Cleft%5C%7B%281%2C%206%29%2C%20%282%2C%205%29%2C%20%283%2C%204%29%2C%20%284%2C%203%29%2C%20%285%2C%202%29%2C%20%286%2C%201%29%5Cright%5C%7D)
        
        Since each roll of a pair of dice is mutually exclusive to any other roll of a pair of dice, the probability is defined by definition 3.1
        
        ![](https://latex.codecogs.com/gif.latex?Prob%5BE%5D%20%3D%20%5Cfrac%7B6%7D%7B36%7D%20%3D%20%5Cfrac%7B1%7D%7B6%7D)
            
        2. ***Sum is 1***
        
        ![](https://latex.codecogs.com/gif.latex?E%20%3D%20%5Cemptyset)
        
        Since each roll of a pair of dice is mutually exclusive to any other roll of a pair of dice, the probability is defined by definition 3.1
        
        ![](https://latex.codecogs.com/gif.latex?Prob%5BE%5D%20%3D%20%5Cfrac%7B0%7D%7B36%7D%20%3D%200)
        
        3. ***Sum is less than or equal to 12***:
        
        ![](https://latex.codecogs.com/gif.latex?Prob%5BE%5D%20%3D%20%5Csum_%7Bi%20%3D%200%7D%5E%7B12%7D%20Prob%5BE_i%5D)
        
        ![](https://latex.codecogs.com/gif.latex?%5Cnewline%20Prob%5BE_0%5D%20%3D%20%5Cemptyset%20%5Cnewline%20Prob%5BE_1%5D%20%3D%20%5Cemptyset%20%5Cnewline%20Prob%5BE_2%5D%20%3D%20%5Cleft%5C%7B%281%2C%201%29%5Cright%5C%7D%20%5Cnewline%20Prob%5BE_3%5D%20%3D%20%5Cleft%5C%7B%281%2C%202%29%2C%20%282%2C%201%29%5Cright%5C%7D%20%5Cnewline%20%5Cvdots%20%5Cnewline%20Prob%5BE_%7B12%7D%5D%20%3D%20%5Cleft%5C%7B%286%2C%206%29%5Cright%5C%7D)
        
        We know that ![](https://latex.codecogs.com/gif.latex?%5Cbigcup_%7Bi%20%3D%201%7D%5E%7Bn%7D%20E_i%20%3D%20%5Cmathbb%7BS%7D) and ![](https://latex.codecogs.com/gif.latex?E_i%20%5Ccap%20E_j%20%3D%20%5Cmathbb%7BO%7D%20%5Ctext%7B%20%7D%20%28i%20%5Cneq%20j%29) therefore we can conclude:
        
        ![](https://latex.codecogs.com/gif.latex?Prob%5BE%5D%20%3D%20%5Csum_%7Bi%20%3D%201%7D%5E%7Bn%7D%20Prob%5BE_i%5D%20%3D%201)
        
    2. **second die < first die;**
    
        Let ![](https://latex.codecogs.com/gif.latex?F_i) represent the event that the first die is equal to `i`.
        
        ![](https://latex.codecogs.com/gif.latex?%5Cdpi%7B120%7D%20Prob%5B%5Ctext%7BSecond%20die%20is%20less%20than%20first%7D%5D%20%3D%20%5Csum_%7Bi%20%3D%201%7D%5E%7B6%7D%20Prob%5B%5Ctext%7BSecond%20die%20is%20less%20than%20first%7D%20%7C%20F_i%5D%20*%20Prob%5BF_i%5D)
        
        We know that ![](https://latex.codecogs.com/gif.latex?%5Cinline%20%5Cdpi%7B120%7D%20Prob%5BF_i%5D) should be ![](https://latex.codecogs.com/gif.latex?%5Cinline%20%5Cfrac%7B1%7D%7B6%7D) for any given `i` assuming that these are fair dice.
        
        ![](https://latex.codecogs.com/gif.latex?%5Cfrac%7B1%7D%7B6%7D%20%5Csum_%7Bi%20%3D%201%7D%5E%7B6%7D%20Prob%5BSecond%20...%20first%20%7C%20F_i%5D)
        
        ![](https://latex.codecogs.com/gif.latex?%5Cnewline%20Prob%5B%5Ctext%7BSecond%20...%20first%7D%20%7C%201%5D%20%3D%200%20%5Cnewline%20Prob%5B%5Ctext%7BSecond%20...%20first%7D%20%7C%202%5D%20%3D%20%5Cfrac%7B1%7D%7B6%7D%20%5Cnewline%20Prob%5B%5Ctext%7BSecond%20...%20first%7D%20%7C%203%5D%20%3D%20%5Cfrac%7B2%7D%7B6%7D%20%5Cnewline%20%5Cvdots%20%5Cnewline%20Prob%5B%5Ctext%7BSecond%20...%20first%7D%20%7C%206%5D%20%3D%20%5Cfrac%7B5%7D%7B6%7D)
        
        ![](https://latex.codecogs.com/gif.latex?Prob%5B%5Ctext%7BSecond%20...%20first%7D%5D%20%3D%20%5Cfrac%7B1%7D%7B36%7D%20%5Csum_%7Bi%20%3D%201%7D%5E%7B6%7D%28i%20-%201%29%20%3D%20%5Cfrac%7B15%7D%7B36%7D)
        
    3. **at least one die is 6;**
    
        We can use binomials here since we are basically asking what's the probability of getting 6 in 2 rolls.
        
        Probability of getting one 6 in a roll:
        ![](https://latex.codecogs.com/gif.latex?b%281%3B%202%3B%20%5Cfrac%7B1%7D%7B6%7D%29%20%3D%20%5Cbinom%7B2%7D%7B1%7D%28%5Cfrac%7B1%7D%7B6%7D%29%281%20-%20%5Cfrac%7B1%7D%7B6%7D%29%20%3D%20%5Cfrac%7B2%7D%7B1%7D*%5Cfrac%7B5%7D%7B36%7D%20%3D%20%5Cfrac%7B10%7D%7B36%7D)
        
        Probability of getting two 6's in a roll:
        ![](https://latex.codecogs.com/gif.latex?b%282%3B%202%3B%20%5Cfrac%7B1%7D%7B6%7D%29%20%3D%20%5Cbinom%7B2%7D%7B2%7D%28%5Cfrac%7B1%7D%7B6%7D%29%5E2%20%3D%20%5Cfrac%7B1%7D%7B36%7D)
        
        Therefore if we add the probabilities of these two events together we should get the probability of at least 1 die being a 6. ![](https://latex.codecogs.com/gif.latex?%5Cinline%20%5Cfrac%7B10%7D%7B36%7D%20&plus;%20%5Cfrac%7B1%7D%7B36%7D%20%3D%20%5Cfrac%7B11%7D%7B36%7D)
        
    4. **given that the first die is 6, the second die is 6.**
    
        ![](https://latex.codecogs.com/gif.latex?%5Cinline%20Prob%5B%5Ctext%7BSecond%20die%20is%206%7D%20%7C%20%5Ctext%7BFirst%20die%20is%206%7D%5D%20%3D%20%5Cfrac%7BProb%5B%5Ctext%7BBoth%20are%206%7D%5D%7D%7BProb%5B%5Ctext%7BFirst%20die%20is%206%7D%5D%7D%20%3D%20%5Cfrac%7B%5Cfrac%7B1%7D%7B36%7D%7D%7B%5Cfrac%7B1%7D%7B6%7D%7D%20%3D%20%5Cfrac%7B1%7D%7B6%7D)
        
2) **In the preceding problem, find the probability that the first die is 3 given that the sum is greater or equal to 8.**:

    ![](https://latex.codecogs.com/gif.latex?%5Cnewline%20Prob%5B%5Ctext%7BFirst%20die%20is%203%7D%20%7C%20%5Ctext%7BSum%20of%20dice%20is%208%7D%5D%20%3D%20%5Cnewline%20%5Cnewline%20%5Cfrac%7BProb%5B%5Ctext%7BFirst%20die%20is%203%7D%5Ccap%5Ctext%7BSum%20of%20dice%20is%208%7D%5D%7D%7BProb%5B%5Ctext%7BSum%20of%20dice%20is%208%7D%5D%7D)
    
    We can find the individual probabilities now.
    
    ![](https://latex.codecogs.com/gif.latex?Prob%5B%5Ctext%7BSum%20of%20dice%20is%208%7D%5D%20%3D%20%5Cfrac%7B%5C%23%5Cleft%5C%7B%282%2C%206%29%2C%20%283%2C%205%29%2C%20%284%2C%204%29%2C%20%285%2C%203%29%2C%20%286%2C%202%29%5Cright%5C%7D%7D%7B%5C%23%5Cmathbb%7BS%7D%7D)
    
    ![](https://latex.codecogs.com/gif.latex?Prob%5B%5Ctext%7BFirst%20die%20is%203%7D%5Ccap%5Ctext%7BSum%20of%20dice%20is%208%7D%5D%20%3D%20%5Cfrac%7B%5C%23%5Cleft%5C%7B%283%2C%205%29%5Cright%5C%7D%7D%7B%5C%23%5Cmathbb%7BS%7D%7D)
    
    ![](https://latex.codecogs.com/gif.latex?%5Cfrac%7BProb%5B%5Ctext%7BFirst%20die%20is%203%7D%5Ccap%5Ctext%7BSum%20of%20dice%20is%208%7D%5D%7D%7BProb%5B%5Ctext%7BSum%20of%20dice%20is%208%7D%5D%7D%20%3D%20%5Cfrac%7B%5Cfrac%7B1%7D%7B36%7D%7D%7B%5Cfrac%7B5%7D%7B36%7D%7D%20%3D%20%5Cfrac%7B1%7D%7B5%7D)
    

