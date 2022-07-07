* references
    * https://medium.com/intuition/basics-of-mathematical-cryptography-408fdee23826
    * https://medium.com/@lizrice/finding-an-intro-to-maths-for-cryptography-cc97ad6b04

# cryptography-math-basics

# common divisor
*  A common divisor of two integers a and b is a positive integer d
  that divides both of them
* greatest common divisor of a and b is, as
  its name suggests, the largest positive integer d such that d | a and d | b
* The greatest common divisor of a and b is denoted gcd(a,b)
*  (Extended Euclidean Algorithm). Let a and b be positive
  integers. Then the equation
  au + bv = gcd(a,b)
  always has a solution in integers u and v
  * proof: with bezout identity: https://math.stackexchange.com/a/124991
  * More generally, any equation
    Au + Bv = gcd(A,B)
    * can be reduced to the case of relatively prime numbers by dividing both sides
      by gcd(A,B)
    * u * A / gcd + v * B / gcd = 1
*  Let a and b be integers. We say that a and b are relatively prime
  if gcd(a,b) = 1
* Let m ≥ 1 be an integer. We say that the integers a and b are
  congruent modulo m if their difference a − b is divisible by m. We write
  a ≡ b (mod m)
  * properties
    * If a 1 ≡ a 2 (mod m) and b 1 ≡ b 2 (mod m), then
        a 1 ± b 1 ≡ a 2 ± b 2 (mod m) and a 1 · b 1 ≡ a 2 · b 2 (mod m)
    *  Let a be an integer. Then
      a · b ≡ 1 (mod m) for some integer b if and only if gcd(a,m) = 1
        * Further, if a · b 1 ≡ a · b 2 ≡ 1 (mod m), then b 1 ≡ b 2 (mod m). We call b
          the (multiplicative) inverse of a modulo m
* We write
  Z/mZ = {0,1,2,...,m − 1}
  and call Z/mZ the ring of integers modulo m
  * We add and multiply elements
    of Z/mZ by adding or multiplying them as integers and then dividing the
    result by m and taking the remainder in order to obtain an element in Z/mZ
* Euler’s phi function (also sometimes known as Euler’s totient
  function) is the function φ(m) defined by the rule
  φ(m) = #(Z/mZ) ∗ = #{0 ≤ a < m : gcd(a,m) = 1}
* Fast Powering Algorithm
    * Suppose that we want to compute 3 218 (mod 1000)
    * 218 = 2 + 2 3 + 2 4 + 2 6 + 2 7
    * 3 218 = 3 2+2
      3 +2 4 +2 6 +2 7
      = 3 2 · 3 2
      3
      · 3 2
      4
      · 3 2
      6
      · 3 2
      7
    *  each number in the sequence is the square of the preceding one
    *  Further,
      since we only need these values modulo 1000, we never need to store more
      than three digits
* Let p be a prime. Then every nonzero element a in Z/pZ
  has a multiplicative inverse, that is, there is a number b satisfying
  ab ≡ 1 (mod p)
    * using the prime
      modulus p, since if a ∈ Z/pZ is not zero, then gcd(a,p) = 1
    * Fermat’s little theorem (Theorem 1.24) and the fast power-
      ing algorithm (Sect.1.3.2) provide us with a reasonably efficient method of
      computing inverses modulo p, namely
      a −1 ≡ a p−2 (mod p)
    *  The extended Euclidean algorithm (Theorem 1.11) gives us an
      efficient computational method for computing a −1 mod p
       * au + pv = 1 in integers u and v
       *  then u = a −1 mod p
*  If p is prime, then the set Z/pZ of integers modulo p with its
  addition, subtraction, multiplication, and division rules is an example of a
  field
    * a field is the general name for a (commutative) ring in which every nonzero
      element has a multiplicative inverse
* (Fermat’s Little Theorem). Let p be a prime number and
  let a be any integer
    * a p-1 = 1 mod p (p not divide a)
        * a, 2a, 3a, ..., (p − 1)a reduced modulo p
        * There are p − 1 numbers in this list, and we claim that they are all different
            * take any two of them, say ja mod p and ka mod p, and suppose
              that they are the same. This means that
              ja ≡ ka (mod p), and hence that (j − k)a ≡ 0 (mod p)
            *  either p
              divides j − k or p divides a
            * both j and k are between 1
              and p − 1, so their difference j − k is between −(p − 2) and p − 2
            * tween 1
              and p − 1, so their difference j − k is between −(p − 2) and p − 2. There is
              only one number between −(p − 2) and p − 2 that is divisible by p, and that
              number is zero
        * a · 2a · 3a···(p − 1)a ≡ 1 · 2 · 3···(p − 1) (mod p)
        * a p−1 · (p − 1)! ≡ (p − 1)! (mod p)
            * we are allowed to cancel (p − 1)! from both sides, since it is not
              divisible by p
        * a p−1 ≡ 1 (mod p)
    * a p-1 = 0 mod p (p divide a)
        * If p | a, then it is clear that every power of a is divisible by p
* We start by describing a nonmathematical way to visualize public key
  cryptography
    * Alice buys a safe with a narrow slot in the top and puts her
      safe in a public location. Everyone in the world is allowed to examine the safe
      and see that it is securely made. Bob writes his message to Alice on a piece of
      paper and slips it through the slot in the top of the safe. Now only a person
      with the key to the safe, which presumably means only Alice, can retrieve
      and read Bob’s message. In this scenario, Alice’s public key is the safe, the
      encryption algorithm is the process of putting the message in the slot, and the
      decryption algorithm is the process of opening the safe with the key.
    * A useful feature of our “safe-with-a-slot” cryptosystem, which it shares
      with actual public key cryptosystems, is that Alice needs to put only one safe
      in a public location, and then everyone in the world can use it repeatedly
      to send encrypted messages to Alice
* Secure PKCs are built using one-way functions that have a trapdoor. The
  trapdoor is a piece of auxiliary information that allows the inverse to be easily
  computed
  * One says that the private key k priv is trapdoor information for the func-
    tion e k pub , because without the trapdoor information it is very hard to compute
    the inverse function to e k pub , but with the trapdoor information it is easy to
    compute the inverse
  * Notice that in particular, the function that is used to
    create k pub from k priv must be difficult to invert, since k pub is public knowledge
    and k priv allows efficient decryption