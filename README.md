* references
    * https://medium.com/intuition/basics-of-mathematical-cryptography-408fdee23826
    * https://medium.com/@lizrice/finding-an-intro-to-maths-for-cryptography-cc97ad6b04
    * https://math.stackexchange.com/questions/124891/proof-of-extended-euclidean-algorithm/124991#124991
    * https://www.amazon.com/Introduction-Mathematical-Cryptography-Undergraduate-Mathematics/dp/1441926747
    * https://www.amazon.com/Blockchain-Distributed-Ledgers-Alexander-Lipton/dp/9811221510
    * https://mathworld.wolfram.com/Unit.html
    * https://kconrad.math.uconn.edu/blurbs/ugradnumthy/eulerthm.pdf
    * https://cryptography.fandom.com/wiki/Trapdoor_function
    * https://en.wikipedia.org/wiki/Carmichael_number
    * https://artofproblemsolving.com/wiki/index.php/Euclid%27s_Lemma
    * https://artofproblemsolving.com/wiki/index.php/Bezout%27s_Lemma
    * https://mathstats.uncg.edu/sites/pauli/112/HTML/secbezout.html
    * [The Extended Euclidean algorithm](https://www.youtube.com/watch?v=hB34-GSDT3k)

# cryptography-math-basics

## Euclidean Algorithm
* common divisor of two integers a and b is a positive integer d that d | a and d | b
    * gcd(a,b) = greatest common divisor
* Bezout Identity
    * a and b be integers with greatest common divisor d => there exist integers x and y such that ax + by = d
    * proof
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
    * example (Extended Euclidean algorithm)
        * find the Bezout Identity for a=34 and b=19
        * first, find the gcd(34, 19)
            ```
            34 = 19(1) + 15
            19 = 15(1) + 4
            15 = 4(3) + 3
            4 = 3(1) + 1
            3 = 1(3) + 0
            ```
        * then work backwards
            ```
            1 = 4 - 1(3)
            = 4 - 1(15 - 4(3)) = 4(4) - 1(15)
            = 4(19 - 15(1)) - 1(15) = 4(19) - 5(15)
            = 4(19) - 5(34 - 19(1)) = 9(19) - 5(34)
            ```
* Euclid's lemma
    * p is prime <=> p|ab => p|a or p|b
    * proof
        * suppose gcd(p,a)=1 (otherwise - done)
        * Bezout Identity => there exist integers such that x,y such that px+ay=1
        * b(px+ay)=b
        * pbx+aby=b
        * p|p and p|ab (hypothesis) => p|(pbx + aby)=b

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


## Fermat’s Little Theorem
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
* bad for primality testing
    * Carmichael number is a composite number n which satisfies the modular arithmetic congruence relation:
        ```
        b^(n-1) ≡ 1 (mod n)
        ```
        for all integers b which are relatively prime to n
    * example: 561
    * what should be used? Miller–Rabin test

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