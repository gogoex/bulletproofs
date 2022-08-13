# Bulletproofs
An efficient method of constructing range proof

## Notation
| Symbol | Description | Note |
|------------|-------------|-------------|
| $\mathbb{Z}_p ^*$ | $\mathbb{Z}_p$ excluding $0$ | |
| $\mathbf{k}^n $| $(\mathbf{k}^0, \mathbf{k}^1,\ ...\ ,\mathbf{k}^{n-1})$ | $\mathbf{k} \in \mathbb{Z}_p ^* $ | 
| $\mathbf{a}$ | Vector | |
| $\mathbf{a}_{[:n]}$ | subset of $\mathbf{a}$ from index $0$ to $n-1$ | |
| $\mathbf{a}_{[n:]}$ | subset of $\mathbf{a}$ from index $n$ to the end | |
| $\mathbf{a} \circ \mathbf{b}$ |  Vector multiplication of $\mathbf{a,b}$ | |
| $\langle \mathbf{a,b} \rangle$ | Inner product of $\mathbf{a,b}$ | |
| LHS ?= RHS | Check if LHS equals to RHS | |

### Vector polynomial
$n$ is the degree of a polynomial

$ p(x) = \displaystyle \sum_{i=0}^d \mathbf{p_i} \cdot x^{i} \in \mathbb{Z}_p^n $

For example,

$ n = 2 $

$ d = n-1 = 1 $

$ \mathbf{p_0} = [2, 3] \in \mathbb{Z}_p^2 $

$ \mathbf{p_1} = [3, 4] \in \mathbb{Z}_p^2 $

$ p(x) = \mathbf{p_0} \cdot x^0 + \mathbf{p_1} \cdot x^1 $

$ p(x) = [2, 3] + [3, 4] \cdot x $

Evaluating at $x=5$,

$ p(5) = [2, 3] + [3, 4] \cdot 5 $

$ p(5) = [2, 3] + [15, 20] $

$ p(5) = [17, 23] \in \mathbb{Z}_p^2 $

### Inner product of vector polynomial
Assuming $l(x), r(x)$ to be vector polynomials,

$ \langle l(x), r(x) \rangle = \displaystyle \sum_{i=0}^d \displaystyle \sum_{j=0}^i \langle \mathbf{l_i}, \mathbf{r_i} \rangle \cdot x^{i+j} \in \mathbb{Z}_p^n $

For example,

$ n = 2 $

$ d = n-1 = 1 $

$ l(x) = [2, 3] + [3, 4] \cdot x $

$ r(x) = [3, 5] + [1, 2] \cdot x $

$ \langle l(x), r(x) \rangle = \langle \mathbf{l_0}, \mathbf{r_0} \rangle \cdot x^{0+0} + \langle \mathbf{l_1}, \mathbf{r_0} \rangle \cdot x^{1+0} + \langle \mathbf{l_1}, \mathbf{r_1} \rangle \cdot x^{1+1} $

$ = \langle \mathbf{l_0}, \mathbf{r_0} \rangle + \langle \mathbf{l_1}, \mathbf{r_0} \rangle \cdot x + \langle \mathbf{l_1}, \mathbf{r_1} \rangle \cdot x^2 $

$ = \langle [2, 3], [3, 5] \rangle + \langle [3, 4], [3, 5] \rangle \cdot x + \langle [3, 4], [1, 2] \rangle \cdot x^2 $

$ = (6 + 15) + (9 + 20) \cdot x + (3 + 12) \cdot x^2 $

$ = 21 + 29 \cdot x + 15 \cdot x^2 $

Evaluating at $x=2$,

$ = 21 + 29 \cdot 2 + 15 \cdot 2^2 $

$ = 21 + 58 + 60 $

$ = 139 $

## Inner product argument
Prover convinces that it knows $\mathbf{a,b}$ satisfying below:

$ P = \mathbf{g^{a} h^{b}} \in \mathbb{Z_p} $
$ c = \langle \mathbf{a,b} \rangle $

### Assumption
- $a, b$ are of the same length and multiples of 2
- $ \mathbf{g, h} \in \mathbb{G}^n $
- $ \mathbf{a, b} \in \mathbb{Z}_p^n $
- $ P = \mathbf{g^{a} h^{b}} \in \mathbb{Z_p} $
- $ c = \langle \mathbf{a,b} \rangle $

#### Prover and verifier know:
$ \mathbf{g, h} $
$ P, c $

#### Only prover knows:
$\mathbf{a,b}$

### Proof method
1. Prover sends $\mathbf{a,b}$ to verifier
2. Verifier computes:

    $ P = \mathbf{g^{a} h^{b}} \in \mathbb{Z_p} $

    $ c = \langle \mathbf{a,b} \rangle $

    and checks that if $P, c$ match

## Improved inner product argument
Prover convinces verifier that it knows $\mathbf{a,b}$ satisfying below:

$ P = \mathbf{g^{a} h^{b}} \cdot u^{\langle \mathbf{a,b} \rangle} \in \mathbb{G} $

### Assumption
- $a, b$ are of the same length and multiples of 2
- $ \mathbf{g, h} \in \mathbb{G}^n $
- $ u \in \mathbb{G} $
- $ p $ is the group order of $\mathbb{G}$
- $ \mathbf{a, b} \in \mathbb{Z}_p^n $
- $ P = \mathbf{g^{a} h^{b}} \cdot u^{\langle \mathbf{a,b} \rangle} \in \mathbb{G} $

### Hash function

$H(\mathbf{a}, \mathbf{a'}, \mathbf{b}, \mathbf{b'},c) = \mathbf{g} ^ { \mathbf{a} } _ {[:n']} 
 \cdot \mathbf{g} ^ { \mathbf{a'} } _ {[n':]} \cdot \mathbf{h} ^ { \mathbf{b} } _ {[:n']} \cdot \mathbf{h} ^ { \mathbf{b'} } _ {[n':]} \cdot u^c $

Operation " $\cdot$ " is addively homomorphic, and therefore has below property:

$ H(\mathbf{a}_1, \mathbf{a'}_1, \mathbf{b}_1, \mathbf{b'}_1, c_1) \cdot H(\mathbf{a}_2, \mathbf{a'}_2, \mathbf{b}_2, \mathbf{b'}_2, c_2) = H(\mathbf{a}_1 + \mathbf{a}_2, \mathbf{a'}_1 + \mathbf{a'}_2, \mathbf{b}_1 + \mathbf{b}_2, \mathbf{b'}_1 + \mathbf{b'}_2, c_1 + c_2) $

### Proof method

1. $n = length(a)$ 
1. $n' = n / 2$
1. Prover computes $ L, R \in\mathbb{G} $ and sends to verifier

    $ L = H(\mathbf{0}^{n'}, \mathbf{a}_{[:n']}, \mathbf{b} _ {[n':]}, \mathbf{0}^{n'}, \langle \mathbf{a} _{[:n']}, \mathbf{b} _{[n':]} \rangle) $

    $ R = H( \mathbf{a}_{[n':]}, \mathbf{0}^{n'}, \mathbf{0}^{n'}, \mathbf{b} _{[:n']},  \langle \mathbf{a} _{[n':]}, \mathbf{b} _{[:n']} \rangle)$

    where 
    $ P = H( \mathbf{a} _{[:n']}, \mathbf{a} _{[n':]}, \mathbf{b} _{[:n']}, \mathbf{b} _{[n':]}, \langle \mathbf{a}, \mathbf{b} \rangle ) $

1. Verifier selects $ x \in \mathbb{Z}_p $ and sends to prover
1. Prover computes $ \mathbf{a}', \mathbf{b}' $ and sends to verifier

    $ \mathbf{a}' = x \mathbf{a} _{[:n']} +  x^{-1} \mathbf{a} _{[n':]} \in \mathbb{Z}_p^{n'} $

    $ \mathbf{b}' = x^{-1} \mathbf{b} _{[:n']} +  x \mathbf{b} _{[n':]} \in \mathbb{Z}_p^{n'} $
1. Verifier computes $ P' = L^{(x^2)} \cdot P \cdot R^{(x^{-2})} $ using $ (L, R, \mathbf{a}', \mathbf{b}') $ and accepts prover's argument if below equation holds:

    $ P' = H(x^{-1}  \mathbf{a}', x \mathbf{a}', x \mathbf{b}', x^{-1}  \mathbf{b}', \langle \mathbf{a}', \mathbf{b}' \rangle) $

### Supplementary explanation for step 6
Since $H$ is additively homomorphic, $ L^{(x^2)} \cdot P \cdot R^{(x^{-2})} $ equals to the addition of $L, R, P$ above with its element appropriately raised by $x^2$ or $x^{-2}$:

$ L^{(x^2)} \cdot P \cdot R^{(x^{-2})} = H(\mathbf{a} _{[:n']} + x^{-2} \mathbf{a} _{[n':]}, x^2 \mathbf{a} _{[:n']} + \mathbf{a} _{[n':]}, x^2 \mathbf{b} _{[n':]} + \mathbf{b} _{[:n']}, \mathbf{b} _{[n':]} + x^{-2} \mathbf{b} _{[:n']}, \langle \mathbf{a}', \mathbf{b}' \rangle) $

Another point is that:

$ \langle \mathbf{a} _{[:n']}, \mathbf{b} _{[n':]} \rangle ^{x^2} + \langle \mathbf{a}, \mathbf{b} \rangle + \langle \mathbf{a} _{[n':]}, \mathbf{b} _{[:n']} \rangle ^{x^{-2}} = \langle \mathbf{a}', \mathbf{b}' \rangle $ 

is seemingly not easy to understand. But using concrete numbers, it becomes easier to understand. e.g.

$n = 4, n' = 2$,

$ \mathbf{a} = [a_1, a_2, a_3, a_4] $
$ \mathbf{b} = [b_1, b_2, b_3, b_4] $

result in

$ \langle \mathbf{a} _{[:n']}, \mathbf{b} _{[n':]} \rangle ^{x^2} = \langle [a_1, a_2], [b_3, b_4] \rangle ^{x^2} = (a_1 b_3 + a_2 b_4) ^ {x^2} = x^2 a_1 b_3 + x^2 a_2 b_4 $

$ \langle \mathbf{a}, \mathbf{b} \rangle = \langle [a_1, a_2, a_3, a_4], [b_1, b_2, b_3, b_4] \rangle = a_1 b_1 + a_2 b_2 + a_3 b_3 + a_4 b_4 $

$ \langle \mathbf{a} _{[n':]}, \mathbf{b} _{[:n']} \rangle ^{x^{-2}} = \langle [a_3, a_4], [b_1, b_2] \rangle ^{x^{-2}} = (a_3 b_1 + a_4 b_2) ^ {x^{-2}} = x^{-2} a_3 b_1 + x^{-2} a_4 b_2 $

$ \mathbf{a}' = x \mathbf{a} _{[:n']} +  x^{-1} \mathbf{a} _{[n':]} = x [a_1, a_2] + x^{-1} [a_3, a_4] = [x \cdot a_1, x \cdot a_2] + [x^{-1} \cdot a_3, x^{-1} \cdot a_4] = [x \cdot a_1 + x^{-1} \cdot a_3, x \cdot a_2 + x^{-1} \cdot a_4] $
 
$ \mathbf{b}' = x^{-1} \mathbf{b} _{[:n']} +  x \mathbf{b} _{[n':]} =  x^{-1}  [b_1, b_2] + x [b_3, b_4] = [x^{-1} \cdot b_1, x^{-1} \cdot b_2] + [x \cdot b_3, x \cdot b_4] = [x^{-1} \cdot b_1 + x \cdot b_3, x^{-1} \cdot b_2 + x \cdot b_4] $

and the RHS is:

$ \langle \mathbf{a}', \mathbf{b}' \rangle = \langle [x \cdot a_1 + x^{-1} \cdot a_3, x \cdot a_2 + x^{-1} \cdot a_4], [x^{-1} \cdot b_1 + x \cdot b_3, x^{-1} \cdot b_2 + x \cdot b_4] \rangle $

$ = (x \cdot a_1 + x^{-1} \cdot a_3) \cdot (x^{-1} \cdot b_1 + x \cdot b_3) + (x \cdot a_2 + x^{-1} \cdot a_4) \cdot (x^{-1} \cdot b_2 + x \cdot b_4) $

$ = (a_1 b_1 + x^2 a_1 b_3 + x^{-2} a_3 b_1 + a_3 b_3) + (a_2 b_2 + x^2 a_2 b_4 + x^{-2} a_4 b_2 + a_4 b_4) $

$ = (x^2 a_1 b_3 + x^{-2} a_3 b_1) + (a_1 b_1 + a_2 b_2 + a_3 b_3 + a_4 b_4) + (x^2 a_2 b_4 + x^{-2} a_4 b_2) $

$ = (x^2 a_1 b_3 + x^2 a_2 b_4) + (a_1 b_1 + a_2 b_2 + a_3 b_3 + a_4 b_4) + ( x^{-2} a_3 b_1 + x^{-2} a_4 b_2) $

and the LHS matches with the RHS:

$ \langle \mathbf{a} _{[:n']}, \mathbf{b} _{[n':]} \rangle ^{x^2} + \langle \mathbf{a}, \mathbf{b} \rangle + \langle \mathbf{a} _{[n':]}, \mathbf{b} _{[:n']} \rangle ^{x^{-2}} $

$ = (x^2 a_1 b_3 + x^2 a_2 b_4) + (a_1 b_1 + a_2 b_2 + a_3 b_3 + a_4 b_4) + (x^{-2} a_3 b_1 + x^{-2} a_4 b_2) $

### Shrinking the proof by recursion

In the last step, the proof checks if below equation holds:

$ P' = H(x^{-1}  \mathbf{a}', x \mathbf{a}', x \mathbf{b}', x^{-1}  \mathbf{b}', \langle \mathbf{a}', \mathbf{b}' \rangle) $

But because it can be transformed to:

$ P' = \mathbf{g} _{[:n']} ^{x^{-1} \mathbf{a}'} \cdot \mathbf{g} _{[n':]} ^{x \mathbf{a}'} \cdot \mathbf{h} _{[:n']} ^{x \mathbf{b}'} \cdot \mathbf{h} _{[n':]} ^{x^{-1}  \mathbf{b}'}  \cdot u^{\langle \mathbf{a}', \mathbf{b}' \rangle} $

$ P' = (\mathbf{g} _{[:n']} ^{x^{-1}} \cdot \mathbf{g} _{[n':]} ^x)^{\mathbf{a}'} \cdot (\mathbf{h} _{[:n']} ^x \cdot \mathbf{h} _{[n':]} ^{x^{-1}})^{\mathbf{b}'} \cdot u^{\langle \mathbf{a}', \mathbf{b}' \rangle} $


$ P' = (\mathbf{g} _{[:n']} ^{x^{-1}} \circ \mathbf{g} _{[n':]} ^x)^{\mathbf{a}'} \cdot (\mathbf{h} _{[:n']} ^x \circ \mathbf{h} _{[n':]} ^{x^{-1}})^{\mathbf{b}'} \cdot u^{\langle \mathbf{a}', \mathbf{b}' \rangle} $

instead of sending whole $\mathbf{a}', \mathbf{b}'$ to verifier which costs $2n$, inner product argument can be executed.

Starting from $n$, in each execution of inner product argument, provers makes $n$ a half and computes $L, R$ with the new $n$, passes $L, R$ to verifier, and recursively excutes with reduced generators and input strings. At the end of execution when $n=1$, prover send the last input strings of size $1$, $a, b$, instead of $L,R$, and the verifier accepts or rejects based on $a,b$ and last set of generators which are of size $1$.

$(L_1, R_1),\ ...\ ,\ (L_{log_2 n}, R_{log_2 n}),\ (a, b)$

This way, communication cost between prover and verifier goes down to $2 \lceil log_2 (n) \rceil \in \mathbb{G} + 2 \in \mathbb{Z}_p$. 

### Multi-Exponentiation
Generating $\mathbf{g}'$, $\mathbf{h}'$ of length $n/2$ from $\mathbf{g}$, $\mathbf{h}$ of length $n$ requires

$ x \in \mathbb{Z} _ {p} $ (random)

$\mathbf{g}' = \mathbf{g} ^{x^{-1}} _{[:n']} \circ \mathbf{g} ^x _{[n':]} \in \mathbb{G} ^{n'}$

$\mathbf{h}' = \mathbf{h} ^x _{[:n']} \circ \mathbf{h} ^{x^{-1}} _{[n':]} \in \mathbb{G} ^{n'}$

which costs $2n$. Moreover, repeating the same computation until $n=1$ costs $4n$.

But if all the coefficients used throughout the entire computation are aggregated and multiplied to the generators only once at the end, the cost goes down to $2n$.

At the point when $n=1$, $a, b$ are scalar values and $\mathbf{g}, \mathbf{h}$ are not vectors but single elements on $\mathbb{G}$.　

$ g = \displaystyle \prod_{i=1}^n g_i^{s_i} \in \mathbb{G}$

$ h = \displaystyle \prod_{i=1}^n h_i^{1 / s_i} \in \mathbb{G}$

$s_i$ is defined as follows:

For $i=1,...,n$: 
$ s_i = \displaystyle \prod_{j=1}^{log_2(n)} x_j^{b(i,j)} $
$ b(i,j) $ is $1$ if $j$-th bit of $i-1$ is $1$. Otherwise $0$.

Using this, the entire checks can be aggregated to a single check below:

$ \mathbf{g} ^{a \cdot \mathbf{s}} \cdot \mathbf{h} ^{b \cdot \mathbf{s} ^{-1}} \cdot u^{a \cdot b} \ ?= P \cdot \displaystyle \prod_{j=1}^{log_2(n)} L_j^{(x^2_j)} \cdot R_j^{(x_j^{-2})} $

## Pedersen Commitment
Given $\upsilon, \gamma \in \mathbb{Z}_p $, $g, h, V \in \mathbb{G}$,

Pedersen Commitment $V$ to $\upsilon$ with randomness $\gamma$ is:

$ V = h^{\gamma} g^{\upsilon} $

### Properties
- Additively homomorphic.
- By adjusting randomness, the same commitment value can be generated for different secret values. Because of this, it's impossible to know which value a commitment opens to only based on the commitment value.

## Confidential Transaction
Replaces amounts in transaction with Pedersen commitments.

Without knowing the values hidden inside the commitments, by utilizing additively homomorphic property of the commitments, whether the sum of inputs, outputs and fees equals to 0 can be checked.

### Procedure to check if the sum of commitment is 0
Assume that $C = xG + aH$ is a Pedersen commitment where $a$ is the secret amount, $x$ is the blinding factor (randomness).

Since the commitment becomes $C = xG$ when $a = 0$, the below procedure can check if the sum is $0$ or not.

1. Sign on the hash of the commitment representing the sum with blinding factor as a private key
1. Verify the signature with the commitment $C = xG$ as the public key
1. If the verification passes, the sum is $0$

The message to sign has to be the hash of the commitment. If arbitrary signature can be used and the public key matching the signature is solved, the transaction becomes valid eventhough the sum is not equal to $0$.

### Negative values

Because each value represents as an element of cyclic group, negative values can be generated by making a value exceed the group order.
 
For example, assuming the group order is $11$, 

$3 + 9 = 12 = -1 \bmod 11$

Using negative values, invalid transaction like

$(1 + 1) - (-5 + 7) == 0$

can be created, but because values are hidden behind commitments, checking this is difficult by just looking at the commitment values.

In order not to allow negative values in amounts, range proof can be added to the transaction.

### Simple range proof

#### Application of 0-checking method
If $C$ is a commitment to $1$, the commitment is $C = xG + 1H$.
And if some message is signed by the blinding Factor $x$, $C' = C - 1H$ becomes the matching public key.
The amount $1$ here can replaced by any amount.

#### Ring Signature
Ring signature is a type of signatures with below properties:
- Contains multiple signatures
- Valid if any one of the contained signatures is valid. 
- When a ring signature is valid, it cannot be known which signature is valid

#### OR proof 
Assume $ C' = C - 1H $ is a Pedersen commitment and $R = \lbrace C, C' \rbrace $ is a ring signature.

If $C$ is a commitment to $0$, signature verification against $C$ passes, and if $C$ is a commitment $1$, signature verification against $C'$ passes.

So, the ring signature $R$ becomes valid in either case. 
$R$ becomes invalid otherwise.

This is called OR proof.

#### Constructing Range Proof using OR proofs
Using OR proofs, a range proof that proves a value to be between 0 and 7 can be constructed as follows:

1. Create Pedersen commitment $C$ for value $c$
1. Create $C1, C2, C3$ such that $Cn$ is a commitment for $c$'s $n$-th bit and $C1 + C2 + C3 = C$
1. Create $Cn'$ as follows:
    $ C1' = C1 - 1H $
    $ C2' = C2 - 2H $
    $ C3' = C3 - 4H $
1. Create below 3 range proofs
    $ \lbrace C1, C1' \rbrace $
    $ \lbrace C2, C2' \rbrace $
    $ \lbrace C3, C3' \rbrace $
1. If all ranges proofs are valid, the value is inside the range. If not, the value is outside the range.

## Inner-Product Range Proof
Range proof explained in Bulletproofs paper.

Prover convinces verifier that $V$ is a commitment to $\upsilon$ that is within a specific range without disclosing $\upsilon$

### Proof method

Assuming:

- $n$ is number of bits required to represent the value range
- $\upsilon$ is the value inside the commitment
- $V$ is the Pedersen commitment to $\upsilon$
- $\gamma$ is the randomness used for Pederseon Commitment $V$
- $g, h$ are group elements

Proves below:

$ \lbrace (g, h, V \in \mathbb{G}; n, \upsilon, \gamma \in \mathbb{Z}_p): V = h^{\gamma} g^{\upsilon} \land v \in [0,2^n - 1]  \rbrace $

1. Define $\mathbf{a}_L = (a_1,\ ...\ ,a_n) \in \lbrace 0,1 \rbrace ^n$ such that $ \langle \mathbf{a}_L, \mathbf{2}^n \rangle = \upsilon$
1. Prover creates $ A \in \mathbb{G} $ that is a commitment to $\mathbf{a}_L$
1. Prover convinces verifier that it knows $\mathbf{a}_L \in \mathbb{Z}_p^n$ and  $\upsilon, \gamma \in \mathbb{Z}_p$ that satisfies below:

    $a$. $ V = h^{\gamma} g^{\upsilon} $

    $b$. $ \langle \mathbf{a}_L, \mathbf{2}^n \rangle = \upsilon $
  
    $c$. $ \mathbf{a}_R = \mathbf{a}_L - \mathbf{1} ^n $

    $d$. $ \mathbf{a}_L \circ \mathbf{a}_R = \mathbf{0}^n $　

### Purpose of condition $c, d$

- $ \mathbf{a}_L $ consists of elements in $\mathbb{Z}_p$, and therefore can contain an element that is neither $0$ or $1$.
- An element of $ \mathbf{a}_R $ becomes $0$ if the corresponding element of $ \mathbf{a}_L $ is $1$. Similarly $ \mathbf{a}_R $ element become $-1$ if corresponding element is $0$. In other cases, both $ \mathbf{a}_L $ and $ \mathbf{a}_R $ elements are non-zero. 
- So, multiplying $ \mathbf{a}_L $ by $ \mathbf{a}_R $ can only result in $ \mathbf{0}^n $ when all elements of $ \mathbf{a}_L $ is $0$ or $1$.

Condition $c, d$ together guarantee that $\mathbf{a}_L = (a_1,\ ...\ ,a_n) \in \lbrace 0,1 \rbrace ^n$

### Combining conditions $b, c, d$

In order for prover to convince verifier that $ \mathbf{b} = \mathbf{0}^n \in \mathbb{Z}_p^n $, it can ask the verifier to send a random $ \mathbf{y} $ and then show $ \langle \mathbf{b}, \mathbf{y}^n \rangle = 0 $.

Utilizing this and random $\mathbf{y} \in \mathbb{Z}_p $ from verifier, condition $b, c, d$ can be rewritten as below:

$b$. $ \langle \mathbf{a}_L, \mathbf{2}^n \rangle = \upsilon $
$c$. $ \langle \mathbf{a}_L - \mathbf{1}^n - \mathbf{a}_R, \mathbf{y}^n  \rangle = 0 $
$d$.  $ \langle \mathbf{a}_L, \mathbf{a}_R \circ \mathbf{y}^n  \rangle = 0 $

Furthermore, using random $z \in \mathbb{Z}_p $ from verifier, $b, c, d$ can be aggregated to single equation:

$ z^2 \cdot \langle \mathbf{a}_L, \mathbf{2}^n \rangle + z \cdot \langle \mathbf{a}_L - \mathbf{1}^n - \mathbf{a}_R, \mathbf{y}^n  \rangle + \langle \mathbf{a}_L, \mathbf{a}_R \circ \mathbf{y}^n  \rangle = z^2 \cdot \upsilon $

The equation can be transformed to:

$ \delta (y, z) = (z - z^2) \cdot \langle \mathbf{1}^n, \mathbf{y}^n \rangle - z^3 \langle \mathbf{1}^n, \mathbf{2}^n \rangle \in \mathbb{Z}_p $

$ \langle \mathbf{a}_L - z \cdot \mathbf{1}^n, \mathbf{y}^n \circ (\mathbf{a}_R + z \cdot \mathbf{1}^n) + z^2 \cdot \mathbf{2}^n \rangle = z^2 \cdot \upsilon + \delta (y, z) $ (39)

Prover will show that (39) holds for any $y, z$ sent from verifier.
 
Note that $ (z - z^2) \cdot \langle \mathbf{1}^n, \mathbf{y}^n \rangle - z^3 \langle \mathbf{1}^n, \mathbf{2}^n \rangle \in \mathbb{Z}_p $ on the RHS of (39) can be easily calculated by verifier.

### Proof method

1. Prover creates $ \mathbf{a}_L $ from $ \upsilon, \gamma $, and from that, it creates $ \mathbf{a}_R $. It then creates $A$ which is a commitment to $ \mathbf{a}_L, \mathbf{a}_R $, $S$ which is a commitment to $ \mathbf{s}_L, \mathbf{s}_R \in \mathbb{Z}_p^n $ that are used to hide $ \mathbf{a}_L, \mathbf{a}_R $, and sends them to verifier.

    [Computation details]

     1. $ \mathbf{a}_L \in \lbrace 0, 1 \rbrace ^n s.t. \langle \mathbf{a}_L, \mathbf{2}^n \rangle = \upsilon $
     1. $ \mathbf{a}_R = \mathbf{a}_L - \mathbf{1}^n \in \mathbb{Z}_p^n $
     1. $ \alpha \in \mathbb{Z}_p $ (random) 
     1. $ A = h^{\alpha} \mathbf{g}^{\mathbf{a}_L} \mathbf{h}^{\mathbf{a}_R} \in \mathbb{G} $ ($A$ is commitment to $ \mathbf{a}_L, \mathbf{a}_R $)
     1. $ \mathbf{s}_L, \mathbf{s}_R \in \mathbb{Z}_p^n $
     1. $ \rho \in \mathbb{Z}_p $ (random) 
     1. $ S = h^{\rho} \mathbf{g}^{\mathbf{s}_L} \mathbf{h}^{\mathbf{s}_R} \in \mathbb{G} $ ($S$ is commitment to $ \mathbf{s}_L, \mathbf{s}_R $)

1. Verifier randomly selects $y,z \in \mathbb{Z}^*_p $ and sends tot prover

1. Using $y,z$, prover defines $l(x)$ that combines LHS input of inner product in (39)with $\mathbf{s}_L$, and $r(x) that combines RHS input of inner product in (39) with $\mathbf{s}_R$, and $t(x)$ that is an inner product of $l(x), r(x)$. $t_0$ equals to $ z^2 \cdot \upsilon + \delta (y, z) $ which is the RHS of (39), and $t_1, t_2$ becomes as follows. The details of this calculation is explained in later section.

    $ l(x) = (\mathbf{a}_L - z \cdot \mathbf{1}^n) + \mathbf{s}_L \cdot x \in \mathbb{Z}_p^n $

    $ r(x) = \mathbf{y}^n \circ (\mathbf{a}_R + z \cdot \mathbf{1}^n + \mathbf{s}_R \cdot x) + z^2 \cdot \mathbf{2}^n \in \mathbb{Z}_p^n $

    $ t(x) = \langle l(x), r(x) \rangle = t_0 + t1 \cdot x + t2 \cdot x^2 \in \mathbb{Z}_p $

    $ t_0 = z^2 \cdot \upsilon + \delta (y, z) $

    $ t_1 = \langle \mathbf{l_1}, \mathbf{r_0} \rangle = \langle \mathbf{s}_L, \mathbf{y}^n \circ (\mathbf{a}_R + z \cdot \mathbf{1}^n) + z^2 \cdot \mathbf{2}^n \rangle $

    $ t_2 = \langle \mathbf{l_1}, \mathbf{r_1} \rangle = \langle \mathbf{s}_L, \mathbf{y}^n \circ \mathbf{s}_R \rangle $

1. Prover computes $T_1, T_2$ that are commitment to $t_1, t_2$, and sends them to verifier

    $ \tau_1, \tau_2 \in \mathbb{Z}_p $ (random)

    $ T_1 = g^{t_1} h^{\tau_1} \in \mathbb{G} $

    $ T_2 = g^{t_2} h^{\tau_2} \in \mathbb{G} $

1. Verifier randomly selects $ x \in \mathbb{Z}_p^* $ and sends to prover

1. Prover computes $\mathbf{l}, \mathbf{r}$ by evaluating  $l(x), r(x)$ at $x$. It then computers $\hat{t}$ that is an inner product of $\mathbf{l}, \mathbf{r}$. It also computes $\tau_x, \mu$ aggregating randomness used and $x$. Prover sends $\mathbf{l}, \mathbf{r}, \hat{t}, \tau_x, \mu$ to verifier

    [Computation details]

    $ \mathbf{l} = l(x) = \mathbf{a}_L - z \cdot \mathbf{1}^n + \mathbf{s}_L \cdot x \in \mathbb{Z}_p ^n $

    $ \mathbf{r} = r(x) = \mathbf{y}^n \circ (\mathbf{a}_R + z \cdot \mathbf{1}^n + \mathbf{s}_R \cdot x) + z^2 \cdot \mathbf{2}^n \in \mathbb{Z}_p ^n $

    $ \hat{t} = \langle \mathbf{l}, \mathbf{r} \rangle \in \mathbb{Z}_p $

    $ \tau_x = \tau_2 \cdot x^2 + \tau_1 \cdot x + z^2 \cdot \gamma \in \mathbb{Z}_p $
    
    $ \mu = \alpha + \rho \cdot x \in \mathbb{Z}_p $

1. Verifier prepares new set of generators $ \mathbf{h'} = \mathbf{h}^{\mathbf{y}^{-n}} \in \mathbb{G}^n $ to compute $ \mathbf{a_R} \circ \mathbf{y}^n $, and checks if $ \mathbf{l} = l(x)$、$ \mathbf{r} = r(x) $、$ t(x) = \langle \mathbf{l}, \mathbf{r} \rangle $ holds

    [Computation details]

    $ h'_i = h_i^{(y^{-i+1})} \in \mathbb{G} \ \forall i \in [1,n] $

    $ g^{\hat{t}} h^{\tau_x} \ ?= V^{z^2} \cdot g^{\delta (y, z)} \cdot T_1^x \cdot T_2^{x^2} $ (65) // checks if $\hat{t}$ matches with $t(x)$

    $ \hat{t}\ \ ?= \langle \mathbf{l}, \mathbf{r} \rangle \in \mathbb{Z}_p $ 

    $ P = A \cdot S^x \cdot \mathbf{g}^{-z} \cdot (\mathbf{h}')^{z \cdot \mathbf{y} + z^2 \cdot \mathbf{2}^n} \in \mathbb{G} $ (66) // calculates commitment to $l(x), r(x)$
    
    $ P\ \ ?= h^u \cdot \mathbf{g}^{\mathbf{l}} \cdot (\mathbf{h}')^{\mathbf{r}} $ (67) // checks if the commitment agrees with $\mathbf{l}, \mathbf{r}$

    $ \hat{t}\ \ ?=　\langle \mathbf{l}, \mathbf{r} \rangle \in \mathbb{Z}_p $ (68)

## Fiat-Shamir Heuristic
Instead of letting verifier select random value, create a hash value of a transcript at that point. For instance, 

$y = H(\lbrace V,n \rbrace, A, S)$
$z = H(A, S, y)$

Generates $y, z$ based on the parameters of range proof and values that have been generated on the course of range proof calculation eliminating the need of existance of verifier. 

### Performance improvement using inner product argument
Because steps (67) and (68) are:

$ P\ \ ?= h^u \cdot \mathbf{g}^{\mathbf{l}} \cdot (\mathbf{h}')^{\mathbf{r}} $ (67) 

$ \hat{t}\ \ ?=　\langle \mathbf{l}, \mathbf{r} \rangle \in \mathbb{Z}_p $ (68)

and inner product argument proves that prover knows $\mathbf{a,b}$ that satisfies: 

$ P = \mathbf{g^{a} h^{b}} \in \mathbb{Z_p} $

$ c = \langle \mathbf{a,b} \rangle $

With the below variable replacement,

$ \mathbf{a} = \mathbf{l} $

$ \mathbf{b} = \mathbf{r} $

$ \mathbf{h} = \mathbf{h'} $

$ \mathbf{c} = \mathbf{\hat{t}} $

$ \mathbf{P} = \mathbf{P} h^{-u} $

Instead of sending $ \mathbf{l},  \mathbf{r} $ to verifier at the end of step 6 and doing (67) and (68), prover can execute inner product argument.

By doing so, prover can reduce number of elements to send to verifier from $2n$ ($ \mathbf{l}, \mathbf{r} $) to $2 \cdot \lceil log_2(n) \rceil + 4$ increading the performance.

### Calculation details of \<l(x), r(x)\>

Groupinng terms of $ l(x), r(x) $ by term degree to $ \mathbf{l_n}, \mathbf{r_n} $,

$ l(x) = (\mathbf{a}_L - z \cdot \mathbf{1}^n) + \mathbf{s}_L \cdot x 
 $

$ \mathbf{l_0} = \mathbf{a}_L - z \cdot \mathbf{1}^n $

$ \mathbf{l_1} = \mathbf{s}_L $

$ r(x) = \mathbf{y}^n \circ (\mathbf{a}_R + z \cdot \mathbf{1}^n + \mathbf{s}_R \cdot x) + z^2 \cdot \mathbf{2}^n \in \mathbb{Z}_p^n $

$ \mathbf{r_0} = \mathbf{y}^n \circ (\mathbf{a}_R + z \cdot \mathbf{1}^n) + z^2 \cdot \mathbf{2}^n $

$ \mathbf{r_1} = \mathbf{y}^n \circ \mathbf{s}_R $

Because definition of inner product of vector polynomial is:

$ \langle l(x), r(x) \rangle = \langle \mathbf{l_0}, \mathbf{r_0} \rangle \cdot x^{0+0} + \langle \mathbf{l_1}, \mathbf{r_0} \rangle \cdot x^{1+0} + \langle \mathbf{l_1}, \mathbf{r_1} \rangle \cdot x^{1+1} $

Looking at the definition of $t(x)$:

$ t(x) = \langle l(x), r(x) \rangle = t_0 + t1 \cdot x + t2 \cdot x^2 \in \mathbb{Z}_p $

We can see:

$t_0 = \langle \mathbf{l_0}, \mathbf{r_0} \rangle \cdot x^{0+0} = \langle \mathbf{a}_L - z \cdot \mathbf{1}^n , \mathbf{y}^n \circ (\mathbf{a}_R + z \cdot \mathbf{1}^n) + z^2 \cdot \mathbf{2}^n  \rangle $

which matches with LHS of (39), and it can be deduced that $t_0$ equals to RHS of (49) which is $ z^2 \cdot \upsilon + \delta (y, z) $.

By the same reasoning $t_1, t_2$ are:

$ t_1 = \langle \mathbf{l_1}, \mathbf{r_0} \rangle = \langle \mathbf{s}_L, \mathbf{y}^n \circ (\mathbf{a}_R + z \cdot \mathbf{1}^n) + z^2 \cdot \mathbf{2}^n \rangle $

$ t_2 = \langle \mathbf{l_1}, \mathbf{r_1} \rangle = \langle \mathbf{s}_L, \mathbf{y}^n \circ \mathbf{s}_R \rangle $

### Supplementary explanation for (65)
$ g^{\hat{t}} h^{\tau_x} \ ?= V^{z^2} \cdot g^{\delta (y, z)} \cdot T_1^x \cdot T_2^{x^2} $ (65)

- LHS is commitment to $ \langle \mathbf{l}, \mathbf{r} \rangle $ with randomness $ \tau_x $.
- $\tau_x$ matches with the $h$ part of $ V^{z^2}, T_1^x, T_2^{x^2} $ on RHS.
- $g$ part of RHS equals the RHS of $t(x)$ 

#### LHS
$ t_0 = \upsilon \cdot z^2 + \delta (y, z) $

$ \hat{t} = \langle \mathbf{l}, \mathbf{r} \rangle \in \mathbb{Z_p} $

$ = t(x) = t_0 + t_1 \cdot x + t_2 \cdot x^2 $

$ = \upsilon \cdot z^2 + \delta (y, z) + t_1 \cdot x + t_2 \cdot x^2 $

$ t_x = \gamma \cdot z^2 + t_1 \cdot x + t_2 \cdot x^2 $

$ g^{\hat{t}} \cdot h^{t_x} $

#### RHS
$ V = h^{\gamma} g^{\upsilon} \in \mathbb{G} $

$ {\tau}_1 \in \mathbb{Z} _ {p} $ (random)

$ {\tau}_2 \in \mathbb{Z} _ {p} $ (random)

$ T_1 = g^{t_1} h^{{\tau}_1} \in \mathbb{G}$

$ T_2 = g^{t_2} h^{{\tau}_2} \in \mathbb{G}$

$ V^{z^2} \cdot g^{\delta (y,z)} \cdot T_1^x \cdot T_2^{x^2} $

$= (h^{\gamma} g^{\upsilon})^{z^2} \cdot g^{\delta (y,z)} \cdot (g^{t_1} h^{{\tau}_1})^x \cdot (g^{t_2} h^{{\tau}_2})^{x^2}$

$= (h^{\gamma \cdot z^2} g^{\upsilon \cdot z^2}) \cdot g^{\delta (y,z)} \cdot (g^{t_1 \cdot x} h^{{\tau}_1 \cdot x}) \cdot (g^{t_2 \cdot {x^2}} h^{{\tau}_2 \cdot {x^2}})$

$= g^{\upsilon \cdot z^2} \cdot g^{\delta (y,z)} \cdot g^{t_1 \cdot x} \cdot g^{t_2 \cdot {x^2}} \cdot h^{\gamma \cdot z^2} \cdot h^{{\tau}_1 \cdot x} \cdot h^{{\tau}_2 \cdot {x^2}} $

$= g^{\upsilon \cdot z^2 + \delta (y,z) + t_1 \cdot x + t_2 \cdot {x^2}} \cdot h^{\gamma \cdot z^2 + {\tau}_1 \cdot x + {\tau}_2 \cdot {x^2}} $

$= g^{\hat{t}} \cdot h^{t_x} $

### Supplementary explanation for (66)
$ P = A \cdot S^x \cdot \mathbf{g}^{-z} \cdot (\mathbf{h}')^{z \cdot \mathbf{y} + z^2 \cdot \mathbf{2}^n} \in \mathbb{G} $ (66)

$ \mathbf{h'} = \mathbf{h} ^ {(\mathbf{y} ^{-n})} = (h_1, h_2^{(y^{-1})}, h_2^{(y^{-2})}, ..., h_n^{(y^{-n + 1})} ) $


- $h$ part of $ A \cdot S^x $ equals to $ h^{\mu} $.
- $\mathbf{g}$ part of $ A \cdot S^x $ plus $ \mathbf{g} ^ {-z} $ equals to $ \mathbf{g} ^ {\mathbf{a}_L + \mathbf{s}_L \cdot x -z} = \mathbf{g}^{\mathbf{l}} $
- $\mathbf{h}$ part of $ A \cdot S^x $ converted to $\mathbf{h}'$ basis plus $\mathbf{h}'$ term equals to $ (\mathbf{h}')^{\mathbf{r}} $

#### RHS
$ \alpha \in \mathbb{Z} _ {p} $ (random)

$ \rho \in \mathbb{Z} _ {p} $ (random)

$ A = h^{\alpha} \mathbf{g} ^ { \mathbf{a}_L } \mathbf{h} ^ { \mathbf{a}_R } \in \mathbb{G} $

$ S = h^{\rho} \mathbf{g} ^ { \mathbf{s}_L } \mathbf{h} ^ { \mathbf{s}_R } \in \mathbb{G} $

$ P = A \cdot S^x \cdot \mathbf{g} ^{-z} \cdot (\mathbf{h}')^{z \cdot \mathbf{y}^n + z^2 \cdot \mathbf{2}^n } $

$ = (h^{\alpha} \mathbf{g} ^ { \mathbf{a}_L } \mathbf{h} ^ { \mathbf{a}_R }) \cdot (h^{\rho} \mathbf{g} ^ { \mathbf{s}_L } \mathbf{h} ^ { \mathbf{s}_R })^x \cdot \mathbf{g} ^{-z} \cdot (\mathbf{h}')^{z \cdot \mathbf{y}^n + z^2 \cdot \mathbf{2}^n } $

$ = (h^{\alpha} \mathbf{g} ^ {\mathbf{a}_L} \mathbf{h} ^ { \mathbf{a}_R }) \cdot (h^{\rho \cdot x} \mathbf{g} ^ {\mathbf{s}_L \cdot x} \mathbf{h} ^ {\mathbf{s}_R \cdot x}) \cdot \mathbf{g} ^{-z} \cdot (\mathbf{h}')^{z \cdot \mathbf{y}^n + z^2 \cdot \mathbf{2}^n } $

$ = (h^{\alpha} h^{\rho \cdot x}) \cdot (\mathbf{g} ^ {\mathbf{a}_L} \mathbf{g} ^ {\mathbf{s}_L \cdot x} \mathbf{g} ^{-z}) \cdot (\mathbf{h} ^ { \mathbf{a}_R } \cdot  \mathbf{h} ^ {\mathbf{s}_R \cdot x}) \cdot (\mathbf{h}')^{z \cdot \mathbf{y}^n + z^2 \cdot \mathbf{2}^n } $

$ = h^{\alpha + \rho \cdot x} \cdot \mathbf{g} ^ {\mathbf{a}_L + \mathbf{s}_L \cdot x -z} \cdot (\mathbf{h} ^ { \mathbf{a}_R } \cdot  \mathbf{h} ^ {\mathbf{s}_R \cdot x}) \cdot (\mathbf{h}')^{z \cdot \mathbf{y}^n + z^2 \cdot \mathbf{2}^n } $

$ = h^{\alpha + \rho \cdot x} \cdot \mathbf{g} ^ {\mathbf{a}_L + \mathbf{s}_L \cdot x -z} \cdot \mathbf{h} ^ {\mathbf{a}_R + \mathbf{s}_R \cdot x} \cdot (\mathbf{h}')^{z \cdot \mathbf{y}^n + z^2 \cdot \mathbf{2}^n } $

$ = h^{\alpha + \rho \cdot x} \cdot \mathbf{g} ^ {\mathbf{a}_L + \mathbf{s}_L \cdot x -z} \cdot ((\mathbf{h}')^{\mathbf{y}^n}) ^ {\mathbf{a}_R + \mathbf{s}_R \cdot x} \cdot (\mathbf{h}')^{z \cdot \mathbf{y}^n + z^2 \cdot \mathbf{2}^n } $

$ = h^{\alpha + \rho \cdot x} \cdot \mathbf{g} ^ {\mathbf{a}_L + \mathbf{s}_L \cdot x -z} \cdot (\mathbf{h}')^{\mathbf{y} \circ (\mathbf{a}_R + \mathbf{s}_R \cdot x)} \cdot (\mathbf{h}')^{z \cdot \mathbf{y}^n + z^2 \cdot \mathbf{2}^n } $

$ = h^{\alpha + \rho \cdot x} \cdot \mathbf{g} ^ {\mathbf{a}_L + \mathbf{s}_L \cdot x -z} \cdot (\mathbf{h}')^{\mathbf{y} \circ (\mathbf{a}_R + \mathbf{s}_R \cdot x) \cdot z \cdot \mathbf{y}^n + z^2 \cdot \mathbf{2}^n } $

$ = h^{\alpha + \rho \cdot x} \cdot \mathbf{g} ^ {\mathbf{a}_L + \mathbf{s}_L \cdot x -z} \cdot (\mathbf{h}')^{\mathbf{y} \circ (\mathbf{a}_R + z \cdot \mathbf{1}^n + \mathbf{s}_R \cdot x) \cdot + z^2 \cdot \mathbf{2}^n } $

### Supplementary explanation for (67)

$ P\ \ ?= h^u \cdot \mathbf{g}^{\mathbf{l}} \cdot (\mathbf{h}')^{\mathbf{r}} $ (67)

#### RHS
$ \mu = \alpha + \rho \cdot x \in \mathbb{Z} _ {p} $

$ \mathbf{l} = \mathbf{a}_L - z \cdot \mathbf{1}^n + \mathbf{S}_L \cdot x \in \mathbb{Z} ^n _ {p} $

$ \mathbf{r} = \mathbf{y} ^ n \circ (\mathbf{a}_R + z \cdot \mathbf{1}^n + \mathbf{s}_R \cdot x) + z^2 \cdot \mathbf{2}^n \in \mathbb{Z} ^n _ {p} $

$ h^\mu \cdot  \mathbf{g} ^ \mathbf{l} \cdot (\mathbf{h}') ^ \mathbf{r} $

$ = h ^ {\alpha + \rho \cdot x} \cdot \mathbf{g} ^ { \mathbf{a}_L - z \cdot \mathbf{1}^n + \mathbf{s}_L \cdot x }  \cdot (\mathbf {h'})^{\mathbf{y} ^ n \circ (\mathbf{a}_R + z \cdot \mathbf{1}^n + \mathbf{s}_R \cdot x) + z^2 \cdot \mathbf{2}^n} $

$ = h ^ {\alpha + \rho \cdot x} \cdot \mathbf{g} ^ { \mathbf{a}_L + \mathbf{s}_L \cdot x - z}  \cdot (\mathbf {h'})^{\mathbf{y} ^ n \circ (\mathbf{a}_R + z \cdot \mathbf{1}^n + \mathbf{s}_R \cdot x) + z^2 \cdot \mathbf{2}^n} $

## References
- [Bulletproofs: Short Proofs for Confidential Transactions and More](https://eprint.iacr.org/2017/1066.pdf)
- [Confidential Transactions - Investigation](https://elementsproject.org/features/confidential-transactions/investigation)