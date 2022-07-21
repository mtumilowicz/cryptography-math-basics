* references
    * https://medium.com/intuition/basics-of-mathematical-cryptography-408fdee23826
    * https://medium.com/@lizrice/finding-an-intro-to-maths-for-cryptography-cc97ad6b04
    * https://math.stackexchange.com/questions/124891/proof-of-extended-euclidean-algorithm/124991#124991
    * https://www.amazon.com/Introduction-Mathematical-Cryptography-Undergraduate-Mathematics/dp/1441926747
    * https://www.amazon.com/Blockchain-Distributed-Ledgers-Alexander-Lipton/dp/9811221510
    * https://mathworld.wolfram.com/Unit.html
    * https://kconrad.math.uconn.edu/blurbs/ugradnumthy/eulerthm.pdf
    * https://cryptography.fandom.com/wiki/Trapdoor_function

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
        * any equation Au + Bv = gcd(A,B) can be reduced to the case of relatively prime numbers
            * a and b are relatively prime if gcd(a,b) = 1
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
            * unit is an element in a ring that has a multiplicative inverse
            * proof
                * b1 ≡ b1 * 1 ≡ b1 * (a * b2) ≡ (b1 * a) * b2 = 1 * b2 = b2
* Z/mZ = {0,1,2,...,m − 1}
    * https://mathworld.wolfram.com/Ring.html
    * https://mathworld.wolfram.com/Field.html
    * ring of integers modulo m
    * p is prime
        * => set Z/pZ of integers modulo p with its +, -, *, / is an example of a field
        * => every nonzero element a in Z/pZ has a multiplicative inverse
            * in other words: there is a number b satisfying ab ≡ 1 (mod p)
            * proof
                * if a ∈ Z/pZ is not zero, then gcd(a,p) = 1
                * special case of proved above: a * b ≡ 1 (mod m) for some integer b <=> gcd(a,m) = 1
            * consequences
                * efficient way to compute inverses modulo p
                * Fermat’s little theorem + fast powering algorithm
                    * a^−1 ≡ a^(p−2) (mod p)
                * extended Euclidean algorithm
                    * au + pv = 1 in integers u and v
                    * u = a^−1 mod p


## Fast Powering Algorithm
* suppose that we want to compute 3^218 (mod 1000)
* 218 = 2 + 2^3 + 2^4 + 2^6 + 2^7
* 3^218 = 3^(2 + 2^3 + 2^4 + 2^6 + 2^7) = 3^2 * 3^2^3 * 3^2^4 * 3^2^6 * 3^2^7
* it is relatively easy to compute the sequence of values: x^2, x^2^2, x^2^3, etc
    * each number is the square of the preceding one
* we need to store more at most three digits: (mod 1000)


## (Fermat’s Little Theorem)
* p prime, a any integer
    * p not divide a => a^(p-1) = 1 mod p
        * proof
            * a, 2a, 3a, ..., (p − 1)a reduced modulo p
            * we claim that they are all different
                * ja ≡ ka (mod p) => j ≡ k mod p (a is invertible)
            * a · 2a · 3a···(p − 1)a ≡ 1 · 2 · 3···(p − 1) (mod p)
            * a^(p−1) · (p − 1)! ≡ (p − 1)! (mod p)
                * (p − 1)! not divisible by p => we are allowed to cancel it from both sides
                * Fp is a field => we are allowed to divide by any nonzero number
            * a^(p−1) ≡ 1 (mod p)
    * p divide a => a^(p-1) = 0 mod p
        * proof
            * p | a => every power of a is divisible by p
* implications: perform efficiently calculations mod (p − 1) in the exponent
    * a^k ≡ a^(l(p-1) + r) ≡ A^r (mod p)

## Euler's totient function
* Euler’s phi function = Euler’s totient function
    * φ(m) = #(Z/mZ) = #{0 ≤ a < m : gcd(a,m) = 1}
* φ(n) is extremely difficult to compute
    * if someone knows that n = pq, computation of φ(n) is straightforward
    * large n cannot be factored efficiently!

## Euler’s theorem
* very useful generalization of Fermat’s little theorem
* gcd(A,m) = 1 => A^φ(m) = 1 mod m
    * proof
        * very similar to Fermat’s little theorem, instead of 1,2,...,p-1 we take all units
        * consider all units modulo m: u1, u2, ..., uφ(m)
            * why there is φ(m) units?
                * because a * b ≡ 1 (mod m) for some integer b <=> gcd(a,m) = 1
        * a·u1, a·u2, a·u3,... , a·uφ(m)
        * we claim that they are all different
            * ax ≡ ay mod m => x ≡ y mod m (a is invertible mod m)
        * (au1)(au2)(au3)·...·(auϕ(m))≡au1·au2·au3·...·auφ(m) (mod m)
        * a^φ(m)≡1 mod m

## trapdoor function
* is a function that is easy to compute in one direction, yet believed to be difficult to compute in the opposite
direction (finding its inverse) without special information, called the "trapdoor
* analogy
    * padlock and its key
    * it is trivial to change the padlock from open to closed without using the key
        * by pushing the shackle into the lock mechanism
    * opening the padlock easily requires the key to be used
        * key is the trapdoor
* example
    * y = x^e mod n
    * bad trapdoor function
        * n = p (prime)
        * y = x^e mod p
        * we have to find inverse of e mod (p - 1)
            * Fermat's little theorem => we can perform calculations mod (p − 1) in the exponent
            * ed = 1 mod (p - 1)
                * it is solvable (for example using extended Euclidean algorithm) if gdc(e, p-1) = 1
        * y^d = (x^e)^d = x^ed = x mod p-1
        * very easy to reverse
    * good trapdoor function
        * n = pq, p,q - prime
        * y = x^e mod n
        * solving y = x^e mod n is as hard as factoring n = pq
        * however, if we know the actual factors, we can use Euler’s theorem and write x as
            * x = y^d mod n
            * ed = 1 mod φ(n)