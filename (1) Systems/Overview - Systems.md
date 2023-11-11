## Precedence rules:
- The standard precedence rules are (highest to lowest): [infix operations],[relations including =],¬,∧,∨,{⇒,⇔}

## Syntax
### Syntax rules

**Notation**

```
( Given x∈S ⊢ A : bool ) ⊢ ∀x∈S ( A ) : bool
Which means:
| Given x∈S:
|   A : bool.
|−−−−−−−−−−−−−−−−−−−−
| ∀x∈S ( A ) : bool.
```

*Note: we use a colon after each header and the full-stop after each statement, because we would want to allow breaking up a statement over multiple lines without ambiguity.*

Now to define what (boolean) statements are valid in a given context.
To make things easier I shall write "A,...,B : bool" to mean "A,...,B are boolean statements".
In any context, we have the following syntax rules:

```
A : bool ⊢ ¬A : bool
A,B : bool ⊢ A∧B : bool
A,B : bool ⊢ A∨B : bool
A,B : bool ⊢ A⇒B : bool
A,B : bool ⊢ A⇔B : bool
Given x∈S ⊢ x∈S : bool
( Given x∈S ⊢ A : bool ) ⊢ ∀x∈S ( A ) : bool
( Given x∈S ⊢ A : bool ) ⊢ ∃x∈S ( A ) : bool
[v is a used variable] ⊢ v : term
t,u : term ⊢ t=u : bool
t[1],...,t[k] : term ; [f is a k-input function-symbol] ⊢ f(t[1],...,t[k]) : term
t[1],...,t[k] : term ; [Q is a k-input predicate-symbol] ⊢ Q(t[1],...,t[k]) : bool
```

Here a rule of the form "... ⊢ ..." means from the left-hand stuff you can deduce the right-hand stuff (in the current context), and if you have "( ... ⊢ ... )" in the left-hand stuff it means that you have deduced that kind of subcontext previously. For example we can literally do the following deduction if Q is a 1-input predicate symbol:

```
Given x∈S:
	Q(x) : bool
∀x∈S ( Q(x) ) : bool
```

All these deductions should never be written out explicitly, but it should be how you think of the syntax rules if you want to be precise. Note that the ∀sub rule ensures you cannot have nested quantification of the same variable. For example you cannot do:

```
Given x∈S:
	Given x∈T:  [forbidden!]
		Q(x) : bool
	∃x∈T ( Q(x) ) : bool
∀x∈S ( ∃x∈T ( Q(x) ) ) : bool
```

I've also included the recursive definition of terms in the above rules, just to let you see how one can think of them. For instance, if we have the binary operation + we can actually do the following deduction:

```
Given x,y,z ∈ ℕ:
	Given y ∈ ℕ:
		Given z ∈ ℕ:
			x,y,z : term
			x+y : term
			(x+y)+z : term
```

In each subcontext, a property is simply a (boolean) statement (in that context) except with one or more terms replaced by blanks. Each blank is to be filled with a particular 'parameter'. For example, we might have a 1-parameter property Q = "[1] > k ∧ ¬∃d∈ℕ ∃x∈ℕ ( 1 < d < [1] ∧ [1] = d·x )" where each "[1]" denotes a blank for the 1st parameter to Q, in which case "Q(t)" denotes "t > k ∧ ¬∃d∈ℕ ∃x∈ℕ ( 1 < d < t ∧ t = d·x )".
For another example, we might have a 2-parameter property R = "[1] < [2] ∨ [1] = [2]" where each "[i]" denotes a blank for the i-th parameter to R, in which case "R(m,n)" denotes "m < n ∨ m = n".

**Useful short-forms**
	
"Given x∈S such that Q(x):", expands to:
```
Given x∈S:
	If Q(x):
		...
```

## Axioms

The axioms I have given you for ℚ minus "∀x∈ℚ ∃p,q∈ℤ ( q ≠ 0 ∧ p = q·x )" are the axioms for ordered fields that contain ℤ. (And every ordered field contains a copy of ℤ, but you don't need to care about that now.) Both ℚ and ℝ are ordered fields that contain ℤ, so they both satisfy every theorem you can prove from the ordered field axioms.
Notice that the (dedekind-)completeness axiom is the only axiom that goes beyond the structures involved. The others are all about ℕ,ℤ,ℚ,ℝ, so what you can prove from them don't involve set theory, since you can treat ℕ,ℤ,ℚ,ℝ as mere types. However, the completeness axiom involves set theory because it is useless without any axioms that allow you to prove existence of members of P(ℝ).
It's really up to you how descriptive you want your labels to be. I hope you recognize most of these labels. Long forms include "irreflexive" and "trichotomy" and "transitivity". And "+ over <" intuitively stands for "when + 'distributes' over <".
	
**Division axiom**

Conventionally, ordered fields are axiomatized without the division operation, and with the axiom "∀x∈ℚ ( x≠0 ⇒ ∃y∈ℚ ( x·y=1 ) )" instead of this axiom. But with the conventional axiomatization, division is definable because we can define the type ℝ[≠0] = { x : x∈ℝ ∧ x≠0 } and use the theorem ∀x∈ℝ ∀y∈ℝ[≠0] !∃z∈ℝ ( x = y·z ) and apply the symbol-defining rule to get ∀x∈ℝ ∀y∈ℝ[≠0] ( x = y·(x/y) ).
And conversely, with our axiomatization, we can prove ∀x∈ℚ ( x≠0 ⇒ ∃y∈ℚ ( x·y=1 ) ). So both alternatives are essentially equivalent. The advantage of our axiomatization is that it is much easier to use (because it has no ∃-quantifiers). The advantage of the conventional one is that it does not require quantifiers over different types (because we don't have a division-by-zero issue).

The ordered field axioms for ℝ imply the same for ℚ because they are all purely universal — purely universal means not just having only ∀-quantifiers, but also that those quantifiers are outside.
Notes:
		Before that, let us define division / for ℝ×ℝ[≠0] via this mechanism just as we did with subtraction and negation, for convenience. That is, we can define the type ℝ[≠0] = { x : x∈ℝ ∧ x≠0 } and use the theorem ∀x∈ℝ ∀y∈ℝ[≠0] ∃z∈ℝ ( x = y·z ), which you can prove from the field axioms, and apply the rule to get ∀x∈ℝ ∀y∈ℝ[≠0] ( x = y·(x/y) ).
		Define the binary operation division / :  ℝ×ℝ[≠0] → ℝ via ∀x∈ℝ ∀y∈ℝ[≠0] ( x = y·(x/y) ).

**Dedekind-completeness axiom**

The axioms for ℝ are same as the axioms for ℚ (change every "ℚ" to "ℝ") except that the last two are ∀x∈ℚ ( x∈ℝ ) and the dedekind-completeness axiom: 
∀S⊆ℝ ( ∃u∈ℝ ∀x∈S ( x≤u ) ⇒ ∃m∈ℝ ( ∀x∈S ( x≤m ) ∧ ∀u∈ℝ ( ∀x∈S ( x≤u ) ⇒ m≤u ) ) ). 
If you abuse notation a bit and write "S≤u" to mean "∀x∈S ( x≤u )" where x is an unused variable, then this is: 
∀S⊆ℝ ( ∃u∈ℝ S≤u ⇒ ∃m∈ℝ ( S≤m ∧ ∀u∈ℝ ( S≤u ⇒ m≤u ) ) ). 
Which says "every subset of ℝ with an upper bound has a minimum upper bound". 
Of course, "S⊆ℝ" means "S∈P(ℝ)" where "P" here is the power-set operation.

**Rings and semiring axioms**

"Semiring" stands for "half a ring", because it is only on one side (positive side).

**Semiring axioms**

	- closure
	- commute
	- assoc
	- distrib
	- ident rules
	- [+diff]
	- [0<1]. 

**Ordered semiring axioms**
		add the axioms governing <. 
		Discrete ordered semiring axioms:
		add [discrete].

**Ring axioms**

The semiring axioms but with [+diff] replaced by existence of additive inverse. All you need technically is the negation operation, but for convenience we added both subtraction and negation. So to get the ring axioms you start from the semiring axioms and replace [+diff] by the axioms governing −. 

**Ordered ring axioms**		

add the axioms governing <. 
Ordered field axioms:
add the axioms governing "/".
ℚ and ℝ are ordered fields (i.e. they satisfy the ordered field axioms).



Triage:
		Note that the "⊢" used in a rule means something slightly different from when used to state theorems.
		There is no ambiguity because for theorem statements "S ⊢ Q" has a system S, whereas for rules "X;...;Y ⊢ Z;...;W" the stuff on the left are either statements or conditions or proof structure.
		Symbol-defining rule:
		For many-sorted FOL, you can define a restricted function-symbol (i.e. with specified input/output types):
		∀x[1]∈S[1] ... ∀x[k]∈S[k] ∃!y∈T ( Q(x[1],...,x[k],y) ) ⊢ ∀x[1]∈S[1] ... ∀x[k]∈S[k] ∀y∈T ( f(x[1],...,x[k]) = y ⇔ Q(x[1],...,x[k],y) ). [where f is a fresh function-symbol]
