* references
    * https://medium.com/intuition/basics-of-mathematical-cryptography-408fdee23826
    * https://medium.com/@lizrice/finding-an-intro-to-maths-for-cryptography-cc97ad6b04
    * https://math.stackexchange.com/questions/124891/proof-of-extended-euclidean-algorithm/124991#124991

# cryptography-math-basics

## Euclidean Algorithm
* common divisor of two integers a and b is a positive integer d that d | a and d | b
    * gcd(a,b) = greatest common divisor
* Extended Euclidean Algorithm
    * a, b positive integer => au + bv = gcd(a,b) always has a solution in integers u and v
    * proof
        * using Bezout Identity: https://en.wikipedia.org/wiki/B%C3%A9zout%27s_identity
            *  a and b be integers with greatest common divisor d => there exist integers x and y such that ax + by = d
        * consider set K = {ax + by | x,y e Z}
        * let k = min positive K
        * k = ax + by for some x,y e K
        * a = qk + r, 0 ≤ r < k
        * r = a - qk
            = a - q(ax + by)
            = a(1 - qx) + b(-qy) e K
        * k is the smallest element => r = 0 ( r < k, r e K )
        * k is common divisor a, b => k ≤ gcd(a, b)
        * gcd(a, b) | a and gcd(a, b) | b => gcd(a, b) | k because k = ax + by
        * gcd(a, b) = k = ax + by
    * digression
        * a, b integers: a and b are relatively prime if gcd(a,b) = 1
        * any equation Au + Bv = gcd(A,B) can be reduced to the case of relatively prime numbers
            * u(A / gcd) + v(B / gcd) = 1

## modulo arithmetic
* let m >= 1 be an integer
    * a ≡ b (mod m) <=> m | (a - b)
    * properties
        * if a1 ≡ a2 (mod m) and b1 ≡ b2 (mod m) then
            * a1 ± b1 ≡ a2 ± b2 (mod m)
            * a1 * b1 ≡ a2 * b2 (mod m)
        * a * b ≡ 1 (mod m) for some integer b <=> gcd(a,m) = 1
            * proof
                * <=
                    * gcd(a,m) = 1
                    * there is u and v satisfying au + mv = 1 (extended euclidean algorithm)
                    * au − 1 = −mv => m | au - 1 => au ≡ 1 (mod m)
                    * take b = u
                * =>
                    * a * b ≡ 1 (mod m) => ab − 1 = cm for some integer c
                    * gcd(a,m) | ab − cm => gcd(a,m) | 1
            * so gcd(a,m) = 1 => there exists an inverse b of a modulo m
        * a * b1 ≡ a * b2 ≡ 1 (mod m) => b1 ≡ b2 (mod m)
            * we call b the (multiplicative) inverse of a modulo m
            * proof
                * b1 ≡ b1 * 1 ≡ b1 * (a * b2) ≡ (b1 * a) * b2 = 1 * b2 = b2
* Z/mZ = {0,1,2,...,m − 1}
    * ring of integers modulo m
    * https://mathworld.wolfram.com/Ring.html

## Euler's totient function
* Euler’s phi function = Euler’s totient function
    * φ(m) = #(Z/mZ) = #{0 ≤ a < m : gcd(a,m) = 1}
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