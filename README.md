# Mathematical Foundations

## Introduction

We shall build a Fitch-style system that satisfies the following goal

> That every sentence written is true in its context.

Why is that important? It means that any sentence that we can write without any context is then unconditionally true, and we call such sentences that we can write as a theorem.

Rules shall govern what we can write.
Truth tables are not enough to capture first-order logic (with quantifiers), so we use inference rules instead. Each inference rule is chosen to be sound, meaning that if you start with true statements and use the rule you will deduce only true statements. We say that these rules are truth-preserving. If you choose carefully enough, you can make it so that the rules are not just truth-preserving but also allow you to deduce every (well-formed) statement that is necessarily true (in all situations).

What you are probably looking for (namely a practical way to rigorously check the logical validity of your arguments) is natural deduction. There are many different styles, the most intuitive type being Fitch-style, which mark subcontexts using indentation or some related visual demarcation. The following system uses indentation and follows the intuition most closely in my opinion. 

A proof is a finite sequence of lines that can be written according to the rules. A theorem is a sentence that can be written in some proof under no context-header.

Next, suppose we are only dealing with boolean atomic propositions and propositions formed from them using "not" and "and" and "or" and "implies".
So we will only write context-header "If A:" where "A" is a proposition formed as above.
This system is now what is called classical propositional logic.
And the semantics we had given to "not" and "and" and "or" and "implies" are exactly the semantics for classical propositional logic.

## Contexts

Every line is either a header or a statement. We shall put a colon after each header and a full-stop after each statement. Each header specifies some subcontext (contained by the current context), and the lines governed by that header is indicated by the indentation. The full context of each line is specified by all the headers that govern it (i.e. all the nearest headers above it at each lower indentation level). Each context-header, creates a subcontext within the one it is in.

Note that what is stated in some context may be invalid in other contexts.

The entire proof is in the outermost context, which you can imagine has no assumptions whatsoever, but really it does not matter.

Basically, to ensure that we achieve our ultimate goal, it suffices to ensure that every rule we have is sound, namely if every sentence we wrote so far is true in its context then every sentence that the rule permits us to write is true in its context.

## Syntax rules

A statement must be an atomic (indivisible) proposition or a compound statement formed in the usual way using boolean operations or quantifiers, with the restriction that every variable that is bound by a quantifier is not already used to refer to some object in the current context, and that there are no nested quantifiers that bind the same variable.

## Natural deduction rules

Each inference rule is of the form:

| X
| ---
| Y

which means that if the last lines you have written match "X" then you can write "Y" immediately after that at the same level of indentation. Each application of an inference rule is also tied to the current context, namely the context of "X". We will not mention "current context" all the time.

We can see that if every rule we use is sound, the whole proof will achieve the goal (only sentences that are true in their context). That is why we are so concerned with checking soundness of every rule.

## Boolean operations

**Restate:** If we prove something we can affirm it again in the same context.

```
| A
| ...
| ---
| A.
```

Note that "..." denote any number of lines that are at least at the depicted indentation level. In the above rule, this means that all the lines written since the earlier writing of "A." must be in the same context (or some subcontext).

Here is that subcontext-creation rule, which we can call if-sub (for conditional-subcontext):

**⇒sub**
```
|
| -------
| If A:
|   A.
```

**restate** If we prove something we can affirm it again in the same context.

```
| A
| ...
| ---
| A.
```

**⇒restate**  (We can create a conditional subcontext where A holds.).

```
| B.
| ...
| If A:
|   ...
| -----
|   [B.]
```

**⇒intro**

```
| If A:
|   ...
|   B.
| -----
| [A ⇒ B.]
```

**⇒elim**

```
| A ⇒ B
| A.
| -----
| B.
```

**∧intro**

```
| A
| B.
| -----
| A ∧ B.
```

**∧elim**

```
| A ∧ B
| -----
| [A.]
| [B.]
```

**∨intro**

```
| A ∨ B
| -----
| [A ∨ B.]
| [B ∨ A.]
```

**∨elim**

```
| A ∨ B
| A ⇒ C
| B ⇒ C
| -----
| C.
```

**¬intro**

```
| A ⇒ ⊥
| -----
| ¬A.
```

**¬elim**

```
| A
| ¬A
| -----
| ⊥.
```

**¬¬elim**

```
| ¬¬A
| -----
| A.
```

Note that by using ¬intro and ¬¬elim we can get the following additional inference rule:

```
| ¬A ⇒ ⊥
| ------
| A.
```

which corresponds to how one would attempt to prove A by contradiction, namely to show that assuming ¬A implies a falsehood.

**⇔intro**

```
| A ⇒ B
| B ⇒ A
| -----
| A ⇔ B
```

**⇔elim**

```
| A ⇔ B
| -----
| [A ⇒ B]
| [B ⇒ A]
```

## Quantifiers and equality (In progress)
The rules here are for restricted quantifiers because usually we think in terms of them. First we need some definitions.

Used variable: A variable that is declared in the header of some containing ∀-context or declared in some previous ∃-elimination ("let") step in some containing context.

**Unused variable**: A variable that is not used.

**Fresh variable**: A variable that does not appear in any previous statement in any containing context.

**Object expression**: An expression that refers to an object (e.g. a used variable, or a function-symbol applied to object expressions).

**Property with _k_ parameters**: A string _P_ with some blanks where each blank has some label from 1 to _k_, such that replacing each blank in _P_ by some object expression yields a statement. If _k_ = 2, then _P(E,F)_ is the result of replacing each blank labelled 1 by _E_ and replacing each blank labelled 2 by _F_. Similarly for other _k_.

In this section, _E,F_ (if involved) can be any object expressions (in the current context).

We start with the following rules that provide a type of all objects.

**universe**: _obj_ is a type.

```
| 
| -----
| E ∈ obj
```

Now take any type _S_ and a 1-parameter property _P_ and an unused variable _x_ that does not appear in _S_ or _P_.

**∀sub** (We can create a ∀-subcontextt in which _x_ is of type S.)

```
| 
| -----
| Given x ∈ S:
|   [x ∈ S.]
```

**∀restate**

```
| A.
| ...
| Given x ∈ S:  (x must not appear in _A_)
|   ...
| -----
|   [A.]
```

**∀intro**

```
| Given x ∈ S:
|   ...
|   P(x).
| -----
| ∀x ∈ S ( P(x) ).
```

## Systems

We can axiomatizise ℕ,ℤ,ℚ,ℝ this way and it is non-trivial to actually construct ℤ,ℚ,ℝ in the base system (PA plus Set Theory) and extend the operations on ℕ to them in the manner that satisfies the axiomatizations here.

## PL (Propositional Logic)

### Preliminary exercises:

- A ⇒ (B ⇒ A)
- (P0) ((A ∨ B) ∧ ¬A) ⇒ ( ( A ∨ B ) and ¬A ) ⇒ B

- (P1) A or ( B and C ) iff ( A or B ) and ( A or C ).
- (P2) ( A or B ) and ( B or C ) and ( C or A ) implies ( A and B ) or ( B and C ) or ( C and A ).
- (P3) ( A implies not B ) and B implies not A.
- (P4) not ( A or B ) iff ( not A and not B ).
- (P5) not ( A and B ) iff ( not A or not B ).
- (P6) ( A implies B ) or ( B implies A ).
- (P7) ( A implies B or C ) implies ( A implies B ) or ( A implies C ).

## FOL (First-Order Logic)

For (Q1) to (Q5), S is a type, and P is a property, and Q is a 2-parameter property (i.e. "Q(x,y)" is a statement about "x" and "y").
- (Q1) ¬∀x∈S ( P(x) ) ⇒ ∃x∈S ( ¬P(x) ). 
- (Q2) ¬∃x∈S ( P(x) ) ⇒ ∀x∈S ( ¬P(x) ). 
- (Q3) ∃x∈S ( x∈S ) ⇒ ∃x∈S ( P(x) ⇒ ∀y∈S ( P(y) ) ). 
- (Q4) ∀x,y,z∈S ( x=z ∧ y=z ⇒ x=y ). 
- (Q5) ∀x∈S ( ∀y∈S ( Q(x,y) ⇒ P(x) ) ) iff ∀x∈S ( ∃y∈S ( Q(x,y) ) ⇒ P(x) ).

For (Q6) to (Q10), S,T,B,G,V are types, and "f : S→T" denotes "f is a 1-input function-symbol whose input must be of type S and whose output is of type T, and "f : S^2→T" denotes "f is a 2-input function-symbol whose inputs are both of type S and whose output is of type T". Note that if the output type is Bool then it denotes a predicate-symbol instead of a function-symbol.
- (Q6) ∀x∈S ( f(f(f(x))) = f(f(x)) ) ∧ ∀x∈S ∃y∈S ( x = f(y) ) ⇒ ∀x∈S ( f(x) = x ), where f : S→S. 
- (Q7) ∀y∈T ( f(g(y)) = y ) ∧ ∀x∈S ∃y∈T ( g(y) = x ) ⇒ ∀x,y∈S ( f(x) = f(y) ⇒ x = y ), where f : S→T and g : T→S. 
- (Q8) ∀x,y,z∈B ( p(x) = p(y) ∧ p(y) = p(z) ⇒ x = y ∨ y = z ∨ z = x ) ⇒ ∀x∈B ∃y,z∈B ∀w∈B ( p(w) = x ⇒ w = y ∨ w = z ), where p : B→B.
- (Q9) ∀x,y,z∈G ( x*(y*z) = (x*y)*z ) ∧ ∀x,y∈G ( x*i(x) = y*i(y) ) ∧ ∀x∈G ( x*(x*i(x)) = x ) ⇒ ∀x,y∈G ( (i(y)*y)*x = x ), where (infix) * : G^2→G and i : G→G. 
- (Q10) ∀x,y∈V ( c(x,y) ⇒ c(y,x) ) ∧ ∀x,y,z∈V ( c(x,y) ∨ c(y,z) ∨ c(z,x) ) ⇒ ∀w∈V ∃x,y,z∈V ( c(x,y) ∧ c(y,z) ∧ c(z,x) ∧ x ≠ y ∧ y ≠ z ∧ z ≠ x ) ∨ ∃v,w,x,y,z∈V ∀t∈V ( t = v ∨ t = w ∨ t = x ∨ t = y ∨ t = z ), where c : V^2→Bool.

## PA− (PA without induction). 

**Non-strict inequality lemmas**:

∀a,b,c∈ℕ ( a ≤ b ⇒ a+c ≤ b+c )

∀a,b,c∈ℕ ( a < b ≤ c ⇒ a < c ) 

∀a,b,c∈ℕ ( a ≤ b < c ⇒ a < c ) 

∀a,b,c∈ℕ ( a ≤ b ≤ c ⇒ a ≤ c ), where "x ≤ y" is short for "x < y ∨ x = y".

Here we use associativity of +,· to drop brackets whenever not needed. Also, "2" means "1+1" and "4" means "1+1+1+1", and you should omit all steps involving just plain algebra; there is no need to give a proof of things like "(k+1)·(k+1) = k·k+k·2+1".

- [PA−1] ∀k,m∈ℕ ( k<m ⇒ k·2+1<m·2 ). 
- [PA−2] ¬∃x,y,z∈ℕ ( 1<x<y<z<4 ). 
- [PA−3] ∀k∈ℕ ¬∃m,n∈ℕ ( 0 < k·k < m·m < n·n < k·k+3k+5 ). 
- [PA−4] PA− ⊢ No natural is both even and odd.

## PA (which includes induction).

[Induction] For any property P on ℕ, P(0) ∧ ∀ k ∈ ℕ ( P(k) ⇒ P(k+1) ) ⇒ ∀ k ∈ ℕ ( P(k) )

[Strong induction] For any property P on ℕ, ∀k∈ℕ ( ∀i∈ℕ ( i<k ⇒ P(i) ) ⇒ P(k) ) ⇒ ∀k∈ℕ ( P(k) ).

[Well-ordering] For any property P on ℕ, ∃k∈ℕ ( P(k) ) ⇒ ∃m∈ℕ ( P(m) ∧ ∀k∈ℕ ( P(k) ⇒ k≥m ) ).

- [PA1] PA ⊢ ∀k∈ℕ ( ∃m∈ℕ ( k = m·2 ) ∨ ∃m∈ℕ ( k = m·2+1 ) ), where "2" denotes "(1+1)". 
- [PA2] ∀k∈ℕ ( 4 | k·k ∨ 4 | k·k+3 ), where (infix) | : ℕ^2→Bool is defined via ∀x,y∈ℕ ( x | y ⇔ ∃t∈ℕ ( x·t = y ) ). 
- (PA3) ∀k∈ℕ ( k > 1 ⇒ ∃p∈ℕ ( p > 1 ∧ p | k ∧ ¬∃q∈ℕ ( 1 < q < p ∧ q | p ) ), where "1 < q < p" is short-hand for "1 < q ∧ q < p". 
- (PA4) ∀k,m∈ℕ ( k·k = m·m·2 ⇒ k = 0 ). 
- (PA5) ∀k,m∈ℕ ( m > 1 ∧ ¬∃d∈ℕ ( d > 1 ∧ d | k ∧ d | m ) ⇒ ∃x,y∈ℕ ( k·x = m·y+1 ) ).

## RA (Real Analysis).

Useful lemmas:
∀ k,m ∈ ℤ ( k < m ⇒ k + 1 ≤ m )

## ST (Set Theory).

**Credits**:

This Natural Deduction system was created by [user21820](https://math.stackexchange.com/users/21820/user21820) from Math.Stackexchange forums; I organized and studied it. The original post is: [Predicate logic: How do you self-check the logical structure of your own arguments?](https://math.stackexchange.com/a/1684204/560067)
