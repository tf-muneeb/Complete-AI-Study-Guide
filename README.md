# Complete AI Study Guide
## Probability, Bayesian Networks, Adversarial Search and Constraint Satisfaction

This guide follows all nine uploaded files. It starts from zero, explains the working behind each method, and then solves medium and hard examples step by step.

## How to use this guide

1. Learn the **meaning** of a formula before memorizing it.
2. In every solved example, identify the **given information**, the **required quantity**, and the **rule** connecting them.
3. Cover the worked steps and try to reproduce them yourself.
4. Use the final revision sheet only after understanding the chapters.

## Source coverage

| Uploaded material | Covered in |
|---|---|
| `CH#13 Uncertainty.pptx` | Chapters 1–2 |
| `Probability + bayes theorem.pdf` | Chapters 1–2 |
| `ch-14 probability.pdf` | Chapters 2–3 |
| `Bayesian network.pdf` and `Bayesian network (B).pdf` | Chapter 3 |
| `Ch6- Adverserial Search.pptx` | Chapter 4 |
| `Alpha beta pruning.pdf` and `pruningReview.pdf` | Chapter 4 |
| `Ch-6 Constraint Satisfaction Problem.pptx` | Chapter 5 |

---

# 1. Uncertainty and rational decisions

## 1.1 What does uncertainty mean?

An agent is **uncertain** when it does not know exactly what is true or exactly what will happen.

Examples:

- A patient has a toothache, but the doctor does not immediately know whether the cause is a cavity, a gum problem, or an abscess.
- You leave for the airport, but traffic may or may not delay you.
- A coin has not been tossed yet, so you do not know whether it will show heads or tails.

Uncertainty does **not** mean that anything is possible or that we know nothing. We can still have useful partial knowledge, such as “rain is unlikely today” or “this test is usually correct.”

## 1.2 Why simple logical rules are not enough

Consider this attempted rule:

\[
\text{Toothache}\Rightarrow \text{Cavity}
\]

It says that every toothache implies a cavity. But a toothache can have other causes:

\[
\text{Toothache}\Rightarrow
\text{Cavity}\lor\text{GumProblem}\lor\text{Abscess}\lor\cdots
\]

Trying the reverse direction also fails:

\[
\text{Cavity}\Rightarrow \text{Toothache}
\]

A cavity can exist without pain. Therefore:

- not everyone with a toothache has a cavity;
- not everyone with a cavity has a toothache;
- listing every possible cause and symptom is impractical.

Probability gives us a better statement:

> Given the evidence I currently have, my degree of belief that the patient has a cavity is 80%.

This does not say that the patient is “80% ill.” It means that, based on the available information, the chance assigned to cavity is 0.8.

## 1.3 Degree of belief

A **degree of belief** is a numerical measure of how strongly we expect something to be true.

- Probability 0: considered impossible under the model.
- Probability 1: considered certain under the model.
- Probability 0.5: equally likely to happen or not happen.
- Probability 0.8: fairly likely, but not certain.

A probability can change when new evidence arrives. For example, an X-ray may raise or lower the probability of a cavity.

## 1.4 Probability and utility are different

A good decision depends on two things:

1. **Probability:** How likely is each outcome?
2. **Utility:** How good or bad is each outcome for me?

A very likely outcome may still be undesirable. A rare outcome may be so serious that it strongly affects the decision.

\[
\boxed{\text{Decision theory}=\text{probability theory}+\text{utility theory}}
\]

### Utility in simple words

Utility is a score representing preference:

- higher utility = more preferred;
- lower utility = less preferred.

Utility is not always money. It can include time, comfort, safety, marks, health, or any other goal.

## 1.5 Expected utility and maximum expected utility

If an action can produce outcomes \(o_1,o_2,\ldots,o_n\), then:

\[
EU(a)=\sum_i P(o_i\mid a)\,U(o_i)
\]

In words:

> Multiply each outcome’s probability by its utility, then add all the products.

A rational agent using the **maximum expected utility** principle chooses the action with the largest expected utility.

### Small example

Action A gives:

- utility 100 with probability 0.8;
- utility 0 with probability 0.2.

Action B gives:

- utility 60 with probability 1.

Then:

\[
EU(A)=0.8(100)+0.2(0)=80
\]

\[
EU(B)=1(60)=60
\]

So A has the higher expected utility, even though it is risky.

### Airport example from the slides

You must be at the airport by 9:00 pm:

- Agent 1: leave at 7:00 pm, 95% chance of making it.
- Agent 2: leave at 8:00 pm, 98% chance of making it.
- Agent 3: leave at 10:00 pm, claimed 100% chance of making it.

Agent 3 is not a real valid option because leaving at 10:00 pm is already after the required arrival time. Agent 2 is the natural choice: it has a higher success probability than Agent 1 and avoids leaving an hour earlier. In a full decision-theory calculation, we would also assign utilities to arriving on time, missing the flight, and spending extra time waiting.

## 1.6 Practical difficulties with probability in AI

The slides list several difficulties:

- Reliable probabilities may require a lot of data.
- A full joint distribution may require enumerating a huge number of possibilities.
- A single probability can hide details about the source or type of uncertainty.
- People are often poor at estimating probabilities directly.
- Exact numbers may simply be unavailable.

Bayesian networks and independence assumptions later help reduce these problems.

---

# 2. Probability from the beginning

## 2.1 Experiment, outcome, sample space and event

An **experiment** is a process whose result is uncertain, such as rolling a die.

An **outcome** is one complete possible result.

The **sample space**, written \(\Omega\), is the set of all possible outcomes.

For one ordinary die:

\[
\Omega=\{1,2,3,4,5,6\}
\]

For one coin:

\[
\Omega=\{\text{Heads},\text{Tails}\}
\]

An **event** is a set of outcomes that we care about.

For example, “the die is even” is:

\[
E=\{2,4,6\}
\]

The event occurs if the actual outcome belongs to this set.

## 2.2 Basic probability rules

For every event \(A\):

\[
0\leq P(A)\leq 1
\]

The whole sample space has probability 1:

\[
P(\Omega)=1
\]

If all outcomes are equally likely:

\[
\boxed{P(A)=\frac{\text{number of outcomes in A}}{\text{total number of outcomes}}}
\]

### Examples

For a fair coin:

\[
P(\text{Heads})=\frac{1}{2}
\]

For an even die result:

\[
P(\text{Even})=\frac{3}{6}=\frac12
\]

For an odd die result:

\[
P(\text{Odd})=\frac{3}{6}=\frac12
\]

The equally likely formula must not be used when outcomes have different chances. A biased coin need not have \(P(H)=1/2\).

## 2.3 Complement rule

The complement \(\neg A\), also written \(A^c\), means “A does not happen.”

\[
\boxed{P(\neg A)=1-P(A)}
\]

If the probability of rain is 0.3:

\[
P(\text{no rain})=1-0.3=0.7
\]

This rule is often the easiest way to calculate “at least one.” For example, the probability of at least one head in two fair coin tosses is:

\[
1-P(\text{no heads})=1-P(TT)=1-\frac14=\frac34
\]

## 2.4 OR and the addition rule

For events \(A\) and \(B\), \(A\lor B\) means “A or B or both.”

\[
\boxed{P(A\lor B)=P(A)+P(B)-P(A\land B)}
\]

We subtract \(P(A\land B)\) because the overlap was counted twice: once in \(P(A)\) and once in \(P(B)\).

### Example

If:

- \(P(A)=0.5\);
- \(P(B)=0.4\);
- \(P(A\land B)=0.2\);

then:

\[
P(A\lor B)=0.5+0.4-0.2=0.7
\]

## 2.5 Mutually exclusive events

Events are **mutually exclusive** when they cannot happen together:

\[
P(A\land B)=0
\]

For one die, “result is 2” and “result is 5” are mutually exclusive.

For mutually exclusive events:

\[
P(A\lor B)=P(A)+P(B)
\]

### Do not confuse mutually exclusive with independent

These ideas are different:

- **Mutually exclusive:** if one happens, the other cannot happen.
- **Independent:** knowing one happened gives no information about the other.

Two events with positive probabilities cannot be both mutually exclusive and independent. If they are mutually exclusive, learning that A happened makes B impossible, so it certainly gives information about B.

## 2.6 Random variables and domains

A **random variable** is a variable whose value is uncertain.

Examples:

- \(Die\): the value shown by a die.
- \(Weather\): today’s weather category.
- \(Cavity\): whether a patient has a cavity.

The **domain** is the set of values the variable can take:

\[
Domain(Die)=\{1,2,3,4,5,6\}
\]

\[
Domain(Weather)=\{sunny,rainy,cloudy,snow\}
\]

\[
Domain(Cavity)=\{true,false\}
\]

A Boolean variable has exactly two values, usually true and false.

A **discrete** random variable has separate countable values. The values in its domain should be:

- **mutually exclusive:** the variable cannot take two domain values at the same time;
- **exhaustive:** one of the domain values must cover the actual situation.

## 2.7 Probability distributions

A probability distribution gives a probability to every value of a random variable.

Using the slide’s weather example:

| Weather | sunny | rainy | cloudy | snow |
|---|---:|---:|---:|---:|
| Probability | 0.60 | 0.10 | 0.29 | 0.01 |

We can write:

\[
\mathbf P(Weather)=\langle0.60,0.10,0.29,0.01\rangle
\]

The probabilities sum to 1:

\[
0.60+0.10+0.29+0.01=1
\]

The bold \(\mathbf P\) refers to the whole distribution, while \(P(sunny)\) refers to one probability.

## 2.8 Prior, evidence and posterior

- **Prior/unconditional probability:** what we believe before using the new evidence, such as \(P(Cavity)\).
- **Evidence:** something observed, such as \(Toothache=true\).
- **Posterior/conditional probability:** the updated belief after considering evidence, such as \(P(Cavity\mid Toothache=true)\).

The vertical bar \(|\) means **given that**.

\[
P(A\mid B)
\]

reads as “the probability of A given that B is known.”

For two dice:

- prior: \(P(Doubles)\);
- evidence: first die is 3;
- posterior: \(P(Doubles\mid Die_1=3)=1/6\), because the second die must also be 3.

## 2.9 Joint probability

A joint probability concerns two or more variables together:

\[
P(A\land B)
\]

means the probability that both A and B happen.

A **full joint probability distribution** assigns a probability to every complete combination of variable values. All entries must sum to 1.

### Cavity–toothache table from the slides

| | Toothache | No toothache | Row total |
|---|---:|---:|---:|
| Cavity | 0.04 | 0.06 | 0.10 |
| No cavity | 0.01 | 0.89 | 0.90 |
| Column total | 0.05 | 0.95 | 1.00 |

For example:

\[
P(Cavity\land Toothache)=0.04
\]

This is different from both:

\[
P(Cavity\mid Toothache)
\]

and:

\[
P(Toothache\mid Cavity)
\]

The joint entry answers “How often do both occur together out of all patients?”

## 2.10 Marginal probability and marginalization

A **marginal probability** is the probability of one variable, obtained by adding over the other variable(s).

From the table:

\[
P(Cavity)=0.04+0.06=0.10
\]

\[
P(Toothache)=0.04+0.01=0.05
\]

This adding process is called **marginalization**.

For a third variable \(X\):

\[
P(C,T)=\sum_x P(C,T,x)
\]

In simple words: to remove a variable from the expression, add the probabilities for all its possible values.

## 2.11 Conditional probability

\[
\boxed{P(A\mid B)=\frac{P(A\land B)}{P(B)}},\qquad P(B)>0
\]

The evidence B restricts our attention to the worlds where B is true. The denominator \(P(B)\) rescales those remaining probabilities so they sum to 1.

### Cavity example

\[
P(Cavity\mid Toothache)
=\frac{P(Cavity\land Toothache)}{P(Toothache)}
=\frac{0.04}{0.05}=0.8
\]

So 80% of patients with toothache have a cavity **in this model**.

Notice that:

\[
P(Toothache\mid Cavity)=\frac{0.04}{0.10}=0.4
\]

The two conditionals are not interchangeable.

## 2.12 Product rule and chain rule

Rearranging conditional probability gives the **product rule**:

\[
\boxed{P(A\land B)=P(A\mid B)P(B)}
\]

Equivalently:

\[
P(A\land B)=P(B\mid A)P(A)
\]

For three variables, repeated use gives the **chain rule**:

\[
P(A,B,C)=P(A\mid B,C)P(B\mid C)P(C)
\]

Other orders are also valid. For example:

\[
P(A,B,C)=P(C\mid A,B)P(B\mid A)P(A)
\]

## 2.13 Total probability and conditioning

If \(B\) and \(\neg B\) cover all possibilities:

\[
\boxed{P(A)=P(A\mid B)P(B)+P(A\mid\neg B)P(\neg B)}
\]

This is the **law of total probability**.

More generally, if the values \(z\) form a complete set of alternatives:

\[
P(Y)=\sum_z P(Y\mid z)P(z)
\]

It says: break A into separate cases, calculate each case, and add them.

## 2.14 Bayes’ rule

Start with two forms of the product rule:

\[
P(A\land B)=P(A\mid B)P(B)
\]

and:

\[
P(A\land B)=P(B\mid A)P(A)
\]

Set them equal and divide by \(P(B)\):

\[
\boxed{P(A\mid B)=\frac{P(B\mid A)P(A)}{P(B)}}
\]

The parts have names:

- \(P(A\mid B)\): **posterior**;
- \(P(B\mid A)\): **likelihood**;
- \(P(A)\): **prior**;
- \(P(B)\): **evidence probability/normalizing denominator**.

For diagnosis:

\[
P(Disease\mid Symptom)=
\frac{P(Symptom\mid Disease)P(Disease)}{P(Symptom)}
\]

Bayes’ rule is useful because medical knowledge often gives \(P(Symptom\mid Disease)\), while the question asks for the reverse, \(P(Disease\mid Symptom)\).

The denominator is usually found by total probability:

\[
P(Symptom)=P(Symptom\mid Disease)P(Disease)
+P(Symptom\mid\neg Disease)P(\neg Disease)
\]

### Why the prior matters

The same cough can imply different disease probabilities in different populations because the disease prior \(P(Disease)\) differs. Even an excellent test can produce many false alarms when the disease is extremely rare.

### When the denominator can be ignored

If we only want to know which disease is most likely given the same symptom, \(P(Symptom)\) is identical for every disease. We can compare:

\[
P(Symptom\mid Disease_i)P(Disease_i)
\]

without dividing. We cannot ignore the denominator when we need the actual posterior probability.

## 2.15 Normalization

Suppose unnormalized scores for two alternatives are 0.002 and 0.006. Their sum is 0.008. To convert them into probabilities:

\[
\alpha=\frac{1}{0.002+0.006}=\frac1{0.008}=125
\]

Then:

\[
P_1=125(0.002)=0.25,\qquad P_2=125(0.006)=0.75
\]

**Normalization** means dividing each score by the total score so the results sum to 1.

## 2.16 Independence

Events A and B are independent when:

\[
\boxed{P(A\land B)=P(A)P(B)}
\]

Equivalent statements, when the relevant probabilities are nonzero:

\[
P(A\mid B)=P(A)
\]

\[
P(B\mid A)=P(B)
\]

Knowing B does not change the probability of A.

### Fair coin example

Two tosses are normally modeled as independent:

\[
P(HH)=P(H)P(H)=\frac12\cdot\frac12=\frac14
\]

### Dependent events

Drawing cards without replacement creates dependence. If the first card is an ace, fewer aces remain for the second draw.

## 2.17 Conditional independence

A and B are conditionally independent given C when:

\[
\boxed{P(A\land B\mid C)=P(A\mid C)P(B\mid C)}
\]

Equivalently:

\[
P(A\mid B,C)=P(A\mid C)
\]

Once C is known, learning B gives no extra information about A.

In the dentistry example:

- Toothache and X-ray spot may be dependent overall, because both can indicate a cavity.
- Given that we already know whether a cavity is present, the model treats toothache and X-ray spot as conditionally independent.

Conditional independence does **not** claim that toothache and X-ray are independent in every situation. It claims this only after conditioning on cavity.

## 2.18 Why independence saves storage

For \(n\) Boolean variables, a full joint distribution has \(2^n\) entries, of which \(2^n-1\) independent numbers must be stored because the last is fixed by the sum-to-one rule.

For four variables, this is \(2^4-1=15\).

If Weather is independent of three dentistry variables:

\[
P(T,X,C,W)=P(T,X,C)P(W)
\]

We store:

- \(2^3-1=7\) numbers for the dentistry part;
- \(2^1-1=1\) number for Weather;

for a total of 8 instead of 15.

For \(n\) independent Boolean variables, storage can fall from exponential \(O(2^n)\) to linear \(O(n)\).

---

## Solved Example 2A: Full joint inference with three variables

The slides use Boolean variables:

- \(T\): toothache;
- \(C\): cavity;
- \(X\): shows on X-ray.

The joint table is:

| | \(t,x\) | \(t,\neg x\) | \(\neg t,x\) | \(\neg t,\neg x\) |
|---|---:|---:|---:|---:|
| \(c\) | 0.108 | 0.012 | 0.072 | 0.008 |
| \(\neg c\) | 0.016 | 0.064 | 0.144 | 0.576 |

### Question 1: Find \(P(Cavity)\)

Add every entry where cavity is true:

\[
P(c)=0.108+0.012+0.072+0.008=0.2
\]

### Question 2: Find \(P(Cavity\lor Toothache)\)

Include all entries where at least one is true. The only excluded entries have both false: \(0.144\) and \(0.576\).

Using the slide’s direct addition:

\[
P(c\lor t)=0.108+0.012+0.072+0.008+0.016+0.064=0.28
\]

A faster check is:

\[
1-(0.144+0.576)=1-0.72=0.28
\]

### Question 3: Find \(P(\neg Cavity\mid Toothache)\)

First find the numerator:

\[
P(\neg c\land t)=0.016+0.064=0.08
\]

Then the denominator:

\[
P(t)=0.108+0.012+0.016+0.064=0.2
\]

Therefore:

\[
P(\neg c\mid t)=\frac{0.08}{0.2}=0.4
\]

As a check:

\[
P(c\mid t)=\frac{0.108+0.012}{0.2}=\frac{0.12}{0.2}=0.6
\]

The two posteriors sum to 1.

---

## Solved Example 2B: Cheating contingency table

The handwritten notes give:

| | Cheated: Yes | Cheated: No | Total |
|---|---:|---:|---:|
| Male | 0.32 | 0.22 | 0.54 |
| Female | 0.28 | 0.18 | 0.46 |
| Total | 0.60 | 0.40 | 1.00 |

Let \(M\) = male and \(C\) = cheated.

### 1. Probability of male

\[
P(M)=0.32+0.22=0.54
\]

### 2. Probability of male given cheated

The condition “cheated” restricts us to the Yes column, whose total is 0.60. Within that column, males occupy 0.32:

\[
P(M\mid C)=\frac{P(M\land C)}{P(C)}
=\frac{0.32}{0.60}=0.5333\ldots
\]

So the answer is approximately **53.33%**.

### 3. Probability of male or cheated

“Or” includes male, cheated, or both:

\[
P(M\lor C)=P(M)+P(C)-P(M\land C)
\]

\[
=0.54+0.60-0.32=0.82
\]

We subtract 0.32 because male students who cheated were included in both initial terms.

---

## Solved Example 2C: Queue and handedness questions

### Queue question

A daughter, father and mother line up randomly. Let:

- A = daughter is on one end;
- B = father is in the middle.

There are \(3!=6\) equally likely orders. Given B, the only orders are:

- Daughter–Father–Mother;
- Mother–Father–Daughter.

In both, the daughter is on an end. Therefore:

\[
\boxed{P(A\mid B)=1}
\]

Using the formula:

\[
P(B)=\frac26,\qquad P(A\land B)=\frac26
\]

\[
P(A\mid B)=\frac{2/6}{2/6}=1
\]

### Handedness table

| | Right-handed | Left-handed | Total |
|---|---:|---:|---:|
| Male | 0.41 | 0.08 | 0.49 |
| Female | 0.45 | 0.06 | 0.51 |
| Total | 0.86 | 0.14 | 1.00 |

**A. Male given right-handed:**

\[
P(M\mid R)=\frac{0.41}{0.86}=0.4767\approx47.67\%
\]

**B. Right-handed given male:**

\[
P(R\mid M)=\frac{0.41}{0.49}=0.8367\approx83.67\%
\]

**C. Female given left-handed:**

\[
P(F\mid L)=\frac{0.06}{0.14}=0.4286\approx42.86\%
\]

**D. Are female and left-handed independent?**

Check the product rule:

\[
P(F)P(L)=0.51(0.14)=0.0714
\]

But:

\[
P(F\land L)=0.06
\]

Since \(0.06\ne0.0714\), they are **not independent**.

Equivalently, \(P(F\mid L)=0.4286\ne P(F)=0.51\).

---

## Solved Example 2D: Bayes’ rule with three forms of transport

A person travels by car, bus, or train with equal prior probability \(1/3\). The probabilities of being late are:

\[
P(L\mid Car)=0.3,\qquad P(L\mid Bus)=0.4,\qquad P(L\mid Train)=0.1
\]

Given that the person is late, find the probability that the person used the bus.

### Step 1: Write the required Bayes expression

\[
P(Bus\mid L)=\frac{P(L\mid Bus)P(Bus)}{P(L)}
\]

### Step 2: Find total probability of being late

\[
P(L)=P(L\mid Car)P(Car)+P(L\mid Bus)P(Bus)+P(L\mid Train)P(Train)
\]

\[
P(L)=0.3\left(\frac13\right)+0.4\left(\frac13\right)+0.1\left(\frac13\right)
=\frac{0.8}{3}
\]

### Step 3: Substitute

\[
P(Bus\mid L)=\frac{0.4(1/3)}{0.8/3}
=\frac{0.4}{0.8}=0.5
\]

**Answer: 50%.**

The bus does not account for all late arrivals because car and train can also make the person late.

---

## Solved Example 2E: Malaria and high fever — a base-rate problem

The notes give:

\[
P(M)=\frac1{1000}=0.001
\]

\[
P(F\mid M)=0.9
\]

\[
P(F\mid\neg M)=0.15
\]

where \(M\) = malaria and \(F\) = high fever.

### Part A: Find \(P(M\mid F)\)

First:

\[
P(\neg M)=1-0.001=0.999
\]

Use total probability for fever:

\[
P(F)=P(F\mid M)P(M)+P(F\mid\neg M)P(\neg M)
\]

\[
P(F)=0.9(0.001)+0.15(0.999)
\]

\[
P(F)=0.0009+0.14985=0.15075
\]

Now apply Bayes’ rule:

\[
P(M\mid F)=\frac{0.9(0.001)}{0.15075}
=\frac{0.0009}{0.15075}
\approx0.005970
\]

\[
\boxed{P(M\mid F)\approx0.597\%}
\]

Although 90% of malaria cases produce fever in this model, malaria is initially very rare. Most fever cases come from the much larger non-malaria group.

### Part B: Find the probability of malaria and no fever

This is a joint probability, not a conditional one:

\[
P(M\land\neg F)=P(\neg F\mid M)P(M)
\]

Since:

\[
P(\neg F\mid M)=1-0.9=0.1
\]

we get:

\[
\boxed{P(M\land\neg F)=0.1(0.001)=0.0001}
\]

That is 0.01% of the whole population under this model.

---

## Solved Example 2F: Monty Hall problem

There are three doors:

- one hides a car;
- two hide goats.

You choose one door. The host, who knows where the car is, opens a different door showing a goat. You may stay with your first door or switch to the remaining closed door.

### Correct probabilities

- Your first choice had probability \(1/3\) of being the car.
- Therefore, the other two doors together had probability \(2/3\).
- The host’s deliberate reveal does not move probability back to your first door. It concentrates the other two doors’ \(2/3\) probability onto the one remaining unopened alternative.

Thus:

\[
\boxed{P(\text{win if stay})=1/3}
\]

\[
\boxed{P(\text{win if switch})=2/3}
\]

The key assumption is that the host always opens a goat door and never opens your chosen door. A host who behaves differently can change the answer.

---

# 3. Bayesian networks and naive Bayes

## 3.1 Why do we need Bayesian networks?

A full joint distribution can answer any probabilistic question, but it becomes enormous.

With 20 Boolean variables, it has:

\[
2^{20}=1,048,576
\]

complete combinations. Specifying all those probabilities is usually impossible in practice.

A **Bayesian network** gives a compact representation by using:

- a graph showing direct dependencies;
- conditional independence assumptions;
- small local probability tables instead of one huge table.

## 3.2 Definition

A Bayesian network has two main parts:

1. A **directed acyclic graph (DAG)**.
2. A **conditional probability table (CPT)** for every node.

### Directed

Every link has an arrow, such as:

\[
Rain\longrightarrow WetGrass
\]

### Acyclic

Following arrows can never bring you back to the starting node. There are no directed cycles.

### Nodes

Each node represents a random variable, discrete or continuous.

### Arrows and parents

If:

\[
X\longrightarrow Y
\]

then X is a **parent** of Y, and Y is a **child** of X. In a causal network, the arrow normally goes from a cause toward its direct effect.

### CPT

Each node stores:

\[
P(X_i\mid Parents(X_i))
\]

A node with no parents stores a prior distribution instead.

The graph describes **which variables directly influence which others**. The CPTs describe **how strong those influences are**.

## 3.3 Basic network example: cavity

A simplified network is:

\[
Cavity\longrightarrow Toothache
\]

\[
Cavity\longrightarrow XRaySpot
\]

Weather is disconnected from these variables.

This represents:

- Weather is independent of the dentistry variables.
- Toothache and X-ray spot are conditionally independent given Cavity.

The joint distribution factors as:

\[
P(C,T,X,W)=P(W)P(C)P(T\mid C)P(X\mid C)
\]

Compare this with a full chain-rule expression. The network removes unnecessary conditioning terms.

## 3.4 Global semantics: the network multiplication rule

For a complete assignment \(x_1,\ldots,x_n\):

\[
\boxed{P(x_1,\ldots,x_n)=\prod_{i=1}^{n}P(x_i\mid parents(X_i))}
\]

In simple words:

> For every node, select the CPT entry matching that node’s value and its parents’ values. Multiply all selected entries.

This is the most important working rule for Bayesian-network numerical questions.

## 3.5 Local conditional independence

Given its parents, a node is conditionally independent of its non-descendants. This is why the network can use local tables rather than storing every possible interaction.

The exact independence statement depends on the graph. Do not assume that every pair of nodes is independent merely because no arrow directly connects them.

## 3.6 The burglar-alarm network

The slide story is:

- A burglary can trigger the alarm.
- An earthquake can also trigger it.
- John calls when he thinks he hears the alarm, but sometimes mistakes a telephone ring for it.
- Mary usually calls when the alarm sounds, but loud music may make her miss it.

Variables:

- \(B\): burglary;
- \(E\): earthquake;
- \(A\): alarm;
- \(J\): John calls;
- \(M\): Mary calls.

Network:

\[
B\longrightarrow A\longleftarrow E
\]

\[
A\longrightarrow J,\qquad A\longrightarrow M
\]

### CPTs

**Priors:**

\[
P(B=true)=0.001,\qquad P(B=false)=0.999
\]

\[
P(E=true)=0.002,\qquad P(E=false)=0.998
\]

**Alarm, showing \(P(A=true\mid B,E)\):**

| B | E | \(P(A\mid B,E)\) |
|---|---|---:|
| T | T | 0.95 |
| T | F | 0.94 |
| F | T | 0.29 |
| F | F | 0.001 |

For the false row, use \(P(\neg A\mid B,E)=1-P(A\mid B,E)\).

**Calls:**

| Alarm | \(P(J\mid A)\) | \(P(M\mid A)\) |
|---|---:|---:|
| T | 0.90 | 0.70 |
| F | 0.05 | 0.01 |

The factorization is:

\[
\boxed{P(B,E,A,J,M)=P(B)P(E)P(A\mid B,E)P(J\mid A)P(M\mid A)}
\]

John and Mary are conditionally independent given the alarm. Before knowing the alarm, their calls can still be related because both may be responding to the same event.

## 3.7 Compactness of the alarm network

For a Boolean node with \(k\) Boolean parents:

- the CPT has \(2^k\) parent combinations;
- each row needs only one stored number \(p=P(X=true\mid parents)\);
- the false probability is \(1-p\).

The alarm network needs:

| Node | Parents | Numbers needed |
|---|---|---:|
| B | none | 1 |
| E | none | 1 |
| A | B, E | 4 |
| J | A | 2 |
| M | A | 2 |
| **Total** | | **10** |

A full joint distribution over five Boolean variables would need:

\[
2^5-1=31
\]

independent numbers.

If every one of \(n\) variables has at most \(k\) parents, the network needs approximately \(O(n2^k)\) numbers. When \(k\) is small and fixed, this grows linearly with \(n\), rather than as \(O(2^n)\).

## 3.8 Why causes are usually placed before effects

A causal ordering often produces:

- a simpler graph;
- smaller CPTs;
- probabilities that are easier for humans to estimate;
- clearer conditional-independence statements.

A noncausal ordering can require extra links and more numbers. The slides show an alternative alarm-network ordering needing 13 numbers rather than 10. Both can represent a distribution, but one may be much easier to use.

---

## Solved Example 3A: One complete joint probability

Find:

\[
P(J=true,M=true,A=true,B=false,E=false)
\]

Use the network factorization:

\[
P(j,m,a,\neg b,\neg e)
=P(j\mid a)P(m\mid a)P(a\mid\neg b,\neg e)P(\neg b)P(\neg e)
\]

Read each number from the CPTs:

\[
=0.90\times0.70\times0.001\times0.999\times0.998
\]

\[
\boxed{=0.00062811126\approx0.0006281}
\]

The small answer makes sense: the alarm rarely sounds when neither burglary nor earthquake occurs.

### Common mistake

Do not use \(P(A)\) in place of \(P(A\mid B,E)\). The alarm probability depends on the exact values of B and E.

---

## Solved Example 3B: Find the total probability of the alarm

The alarm depends on B and E. There are four possible parent combinations:

\[
(B,E),\quad(B,\neg E),\quad(\neg B,E),\quad(\neg B,\neg E)
\]

Because B and E are independent root nodes:

\[
P(B,E)=P(B)P(E)
\]

Now condition over all four cases:

\[
P(A)=P(A\mid B,E)P(B)P(E)
+P(A\mid B,\neg E)P(B)P(\neg E)
\]

\[
+P(A\mid\neg B,E)P(\neg B)P(E)
+P(A\mid\neg B,\neg E)P(\neg B)P(\neg E)
\]

Substitute each case:

1. Burglary and earthquake:

\[
0.95(0.001)(0.002)=0.0000019
\]

2. Burglary and no earthquake:

\[
0.94(0.001)(0.998)=0.00093812
\]

3. No burglary and earthquake:

\[
0.29(0.999)(0.002)=0.00057942
\]

4. No burglary and no earthquake:

\[
0.001(0.999)(0.998)=0.000997002
\]

Add:

\[
P(A)=0.0000019+0.00093812+0.00057942+0.000997002
\]

\[
\boxed{P(A)=0.002516442}
\]

So the alarm sounds about 0.2516% of the time under this model.

### Follow-up: total probability that John calls

John can call whether or not the alarm sounds:

\[
P(J)=P(J\mid A)P(A)+P(J\mid\neg A)P(\neg A)
\]

\[
=0.9(0.002516442)+0.05(1-0.002516442)
\]

\[
\boxed{P(J)=0.0521389757\approx0.052139}
\]

Most of John’s calls come from the much more common no-alarm situation, where he still calls with probability 0.05.

---

## Solved Example 3C: Burglary given that both John and Mary call

Required:

\[
P(B=true\mid J=true,M=true)
\]

This is a hard example because A and E are **hidden variables**. We must sum over them.

### Step 1: Use conditional probability

\[
P(b\mid j,m)=\frac{P(b,j,m)}{P(j,m)}
\]

The denominator is:

\[
P(j,m)=P(b,j,m)+P(\neg b,j,m)
\]

### Step 2: Expand using the network

For any fixed value of B:

\[
P(B,j,m)=P(B)\sum_e P(E)\sum_a P(a\mid B,e)P(j\mid a)P(m\mid a)
\]

### Step 3: Simplify the sum over alarm

For a particular alarm probability \(p=P(A=true\mid B,E)\):

- if alarm is true, the calls contribute \(0.9\times0.7=0.63\);
- if alarm is false, they contribute \(0.05\times0.01=0.0005\).

Therefore:

\[
\sum_a P(a\mid B,e)P(j\mid a)P(m\mid a)
=0.63p+0.0005(1-p)
\]

\[
=0.0005+0.6295p
\]

### Step 4: Calculate all four B–E cases

| B | E | \(P(B)P(E)\) | \(p=P(A\mid B,E)\) | Contribution to \(P(B,j,m)\) |
|---|---|---:|---:|---:|
| T | T | 0.000002 | 0.95 | 0.00000119705 |
| T | F | 0.000998 | 0.94 | 0.00059104554 |
| F | T | 0.001998 | 0.29 | 0.00036574389 |
| F | F | 0.9970019999999999 | 0.001 | 0.001126113759 |

Each contribution is:

\[
P(B)P(E)[0.0005+0.6295p]
\]

### Step 5: Numerator for burglary

Add the first two rows:

\[
P(b,j,m)=0.00000119705+0.00059104554
\]

\[
=0.00059224259
\]

### Step 6: Add no-burglary evidence

\[
P(\neg b,j,m)=0.00036574389+0.001126113759
\]

\[
=0.001491857649
\]

### Step 7: Normalize

\[
P(j,m)=0.00059224259+0.001491857649
=0.002084100239
\]

\[
P(b\mid j,m)=\frac{0.00059224259}{0.002084100239}
\approx0.2841718
\]

\[
\boxed{P(B\mid J,M)\approx0.2842\text{, or }28.42\%}
\]

The calls greatly raise the burglary probability from its 0.1% prior, but they do not make burglary certain. Earthquakes and false alarms remain possible explanations.

---

## 3.9 Wet-grass network

Network:

\[
Rain\longrightarrow Sprinkler
\]

\[
Rain\longrightarrow GrassWet\longleftarrow Sprinkler
\]

Variables:

- \(R\): rain;
- \(S\): sprinkler on;
- \(G\): grass wet.

The factorization is:

\[
\boxed{P(G,S,R)=P(G\mid S,R)P(S\mid R)P(R)}
\]

CPT values:

\[
P(R=true)=0.2
\]

| Rain | \(P(S=true\mid R)\) |
|---|---:|
| T | 0.01 |
| F | 0.40 |

| Sprinkler | Rain | \(P(G=true\mid S,R)\) |
|---|---|---:|
| F | F | 0.00 |
| F | T | 0.80 |
| T | F | 0.90 |
| T | T | 0.99 |

---

## Solved Example 3D: Rain given wet grass

Required:

\[
P(R=true\mid G=true)
\]

### Step 1: Conditional probability and marginalization

\[
P(r\mid g)=\frac{P(g,r)}{P(g)}
\]

Sum over the hidden sprinkler variable:

\[
P(g,r)=\sum_s P(g,s,r)
\]

For the denominator, sum over both sprinkler and rain:

\[
P(g)=\sum_{s,r}P(g,s,r)
\]

### Step 2: Evaluate all four combinations

**Case 1: \(S=true,R=true\)**

\[
P(g,s,r)=0.99(0.01)(0.2)=0.00198
\]

**Case 2: \(S=false,R=true\)**

Here \(P(S=false\mid R=true)=1-0.01=0.99\):

\[
P(g,\neg s,r)=0.80(0.99)(0.2)=0.1584
\]

**Case 3: \(S=true,R=false\)**

Here \(P(R=false)=0.8\):

\[
P(g,s,\neg r)=0.90(0.40)(0.8)=0.288
\]

**Case 4: \(S=false,R=false\)**

The grass cannot be wet in this model:

\[
P(g,\neg s,\neg r)=0.00(0.60)(0.8)=0
\]

### Step 3: Numerator and denominator

Rain is true in the first two cases:

\[
P(g,r)=0.00198+0.1584=0.16038
\]

Total wet-grass probability:

\[
P(g)=0.00198+0.1584+0.288+0=0.44838
\]

### Step 4: Divide

\[
P(r\mid g)=\frac{0.16038}{0.44838}
\approx0.3576877
\]

\[
\boxed{P(Rain\mid GrassWet)\approx35.77\%}
\]

### Interpretation

Wet grass could have been caused by rain or by the sprinkler. We included both possibilities instead of assuming that wet grass automatically means rain.

---

## 3.10 Bayesian classifiers

A classifier receives attributes \(A_1,\ldots,A_n\) and chooses a class C.

Bayes’ rule gives:

\[
P(C\mid A_1,\ldots,A_n)=
\frac{P(A_1,\ldots,A_n\mid C)P(C)}{P(A_1,\ldots,A_n)}
\]

To choose the most likely class, compare:

\[
P(A_1,\ldots,A_n\mid C)P(C)
\]

for every class. The denominator is the same for every class, so it is unnecessary for ranking.

The difficult part is estimating the full attribute joint probability:

\[
P(A_1,\ldots,A_n\mid C)
\]

## 3.11 Naive Bayes assumption

Naive Bayes assumes that attributes are conditionally independent given the class:

\[
\boxed{P(A_1,\ldots,A_n\mid C)=
\prod_{i=1}^{n}P(A_i\mid C)}
\]

It is called “naive” because real attributes are often related. Despite the simplifying assumption, the method can still work well.

The class score is:

\[
\boxed{Score(C)=P(C)\prod_iP(A_i\mid C)}
\]

Choose the class with the largest score.

## 3.12 Estimating probabilities from data

For class prior:

\[
P(C=c)=\frac{N_c}{N}
\]

where \(N_c\) is the number of training examples in class c and N is the total number.

For a discrete attribute:

\[
P(A_i=value\mid C=c)=
\frac{\text{number in class c with that attribute value}}{N_c}
\]

The slide’s ten-record dataset has:

- 7 records with class No, so \(P(No)=7/10\);
- 3 records with class Yes, so \(P(Yes)=3/10\).

---

## Solved Example 3E: Naive Bayes classification

Using the slide dataset, classify a record with:

- Refund = No;
- Marital Status = Married.

We use these two discrete attributes. Taxable income is ignored here because the slides do not give a continuous-distribution model for it.

### Step 1: Class priors

\[
P(Yes)=\frac3{10},\qquad P(No)=\frac7{10}
\]

### Step 2: Conditional counts for class Yes

The class is the final `Evade` column. Its three Yes records are rows 5, 8 and 10:

1. Refund No, Divorced;
2. Refund No, Single;
3. Refund No, Single.

Therefore:

\[
P(Refund=No\mid Yes)=\frac33=1
\]

and:

\[
P(Status=Married\mid Yes)=\frac03=0
\]

Because one required conditional probability is zero, the unsmoothed Yes score becomes zero:

\[
Score(Yes)=\frac3{10}\times1\times0=0
\]

### Step 3: Conditional counts for class No

The seven class-No records have:

- Refund = No in 4 of 7;
- Status = Married in 4 of 7.

Thus:

\[
P(Refund=No\mid No)=\frac47
\]

\[
P(Status=Married\mid No)=\frac47
\]

The No score is:

\[
Score(No)=\frac7{10}\times\frac47\times\frac47
=\frac{16}{70}\approx0.22857
\]

### Step 4: Classify

\[
Score(No)>Score(Yes)
\]

Therefore, unsmoothed naive Bayes predicts:

\[
\boxed{No}
\]

### Why the zero occurs

One zero conditional probability wipes out the entire class score because everything is multiplied. This is called the **zero-frequency problem**.

### Laplace smoothing

A common fix is to add a small pseudocount. With add-one smoothing for a binary attribute:

\[
P(A_i=v\mid C=c)=
\frac{count(c,v)+1}{N_c+2}
\]

For a status attribute with three possible values, the denominator would be \(N_c+3\). Smoothing avoids exact zeros and is usually safer with small datasets.

## 3.13 Bayesian network versus naive Bayes

- A **Bayesian network** is a general graph that can represent many kinds of dependency.
- **Naive Bayes** is a particular classifier with one class variable and a strong conditional-independence assumption between attributes.
- Bayesian-network inference often sums over hidden variables.
- Naive Bayes classification usually multiplies a class prior by attribute likelihoods and chooses the largest score.

---

# 4. Adversarial search, minimax and alpha–beta pruning

## 4.1 What is adversarial search?

A **multi-agent environment** contains more than one agent.

**Adversarial search** is search in which two or more players with conflicting goals explore the same game situation. Chess is a standard example: your move changes the state in which your opponent must move.

Ordinary search asks:

> Which sequence of actions reaches my goal?

Adversarial search must also ask:

> What will the other player do in response, and how should I plan around that response?

## 4.2 Types of games

Games can be classified using two independent questions.

### Is information perfect or imperfect?

- **Perfect information:** every player can see the complete relevant game state. Chess is an example.
- **Imperfect information:** some relevant information is hidden. Poker and Battleship are examples.

### Are moves deterministic or nondeterministic?

- **Deterministic:** a chosen action has a fully determined result, with no random event such as dice.
- **Nondeterministic/stochastic:** chance helps determine the result. Backgammon and Monopoly include dice.

The slide’s classification is:

| | Deterministic | Nondeterministic |
|---|---|---|
| Perfect information | Chess | Backgammon, Monopoly |
| Imperfect information | Battleship | Poker, Scrabble |

These categories are models. Real implementations may simplify or model a game differently.

## 4.3 Formal parts of a deterministic game

A game can be described by:

- **States \(S\):** all game configurations, with initial state \(s_0\).
- **Players \(P\):** the participants, usually taking turns.
- **Actions \(A(s)\):** legal moves in state s.
- **Transition/result function:** \(Result(s,a)\) gives the state after action a.
- **Terminal test:** tells whether the game has ended.
- **Terminal utility:** gives each player’s score at a terminal state.

A **policy** tells a player which action to choose in each state:

\[
Policy:S\rightarrow A
\]

A single move is not a complete strategy because the best next move depends on the opponent’s responses.

## 4.4 Zero-sum, general-sum and team games

### Zero-sum game

One player’s gain is exactly balanced by another’s loss. If the utilities are +1 and −1, their sum is zero.

One player can be called MAX and the other MIN because:

- MAX tries to maximize the shared utility value;
- MIN tries to minimize it.

### General-sum game

Players have separate utilities that do not necessarily sum to zero. Cooperation, competition, indifference, and changing alliances are possible.

### Team game

Members of a team share a common payoff, even though the team competes against others.

## 4.5 Game trees

A **game tree** represents possible futures:

- each node is a game state;
- each edge is a legal move;
- each level is often called a **ply**;
- terminal leaves contain utility values.

A two-ply tree might contain one MAX move followed by one MIN move.

For tic-tac-toe, if X is MAX, common utilities are:

- X wins: +1;
- draw: 0;
- X loses: −1.

The tree is searched to find a policy that accounts for the opponent’s replies.

## 4.6 Brief historical context from the slides

The deck uses game milestones to show increasing difficulty:

- **Checkers:** early computer players, Samuel’s self-taught program, later championship-level play, and a complete solution announced in 2007.
- **Chess:** gradual progress followed by Deep Blue’s 1997 victory over Garry Kasparov and later much stronger engines.
- **Go:** very large branching factor; Monte Carlo tree search greatly improved play, and AlphaGo defeated leading human players in 2016–2017.
- **Pac-Man:** another common game-search environment.

The main lesson is that larger branching factors and deeper games require better search, evaluation, and learning methods—not just raw enumeration.

---

## 4.7 Minimax: the central idea

Minimax assumes:

1. MAX chooses the move with the largest value.
2. MIN chooses the move with the smallest value.
3. Both continue playing optimally.

The value of a state is therefore the result obtained under optimal play by both sides.

\[
Minimax(s)=
\begin{cases}
Utility(s), & \text{if }s\text{ is terminal}\\
\max_{a\in Actions(s)}Minimax(Result(s,a)), & \text{if MAX moves at }s\\
\min_{a\in Actions(s)}Minimax(Result(s,a)), & \text{if MIN moves at }s
\end{cases}
\]

### How the working happens

Minimax uses depth-first search:

1. Go down to terminal leaves.
2. Read their utility values.
3. Move back upward.
4. At a MAX node, take the maximum child value.
5. At a MIN node, take the minimum child value.
6. At the root, choose the action leading to the best backed-up value.

This upward process is called **backing up** values.

### Important interpretation

Minimax does not assume that the opponent will make a mistake. It chooses a move that is best even when the opponent responds optimally.

---

## Solved Example 4A: Complete minimax backup

The slide tree has:

- root A: MAX;
- B and C: MIN;
- D, E, F and G: MAX.

Terminal values are:

| Parent | Leaf values |
|---|---|
| D | −1, 4 |
| E | 2, 6 |
| F | −3, −5 |
| G | 0, 7 |

### Step 1: Back up the MAX level

D is MAX:

\[
D=\max(-1,4)=4
\]

E is MAX:

\[
E=\max(2,6)=6
\]

F is MAX:

\[
F=\max(-3,-5)=-3
\]

G is MAX:

\[
G=\max(0,7)=7
\]

### Step 2: Back up the MIN level

B chooses between D = 4 and E = 6:

\[
B=\min(4,6)=4
\]

C chooses between F = −3 and G = 7:

\[
C=\min(-3,7)=-3
\]

### Step 3: Back up the root MAX node

\[
A=\max(B,C)=\max(4,-3)=4
\]

### Optimal move

MAX should move from A to B. If MIN then plays optimally, MIN moves to D, and MAX’s next optimal choice gives value 4.

\[
\boxed{\text{Root value}=4,\quad A\rightarrow B\rightarrow D}
\]

Notice that E contains value 6, but MAX cannot force that result because MIN controls B and prefers D = 4.

---

## 4.8 Minimax pseudocode

```text
MAX-VALUE(state):
    if state is terminal:
        return utility(state)
    v = -infinity
    for each legal action a:
        v = max(v, MIN-VALUE(Result(state, a)))
    return v

MIN-VALUE(state):
    if state is terminal:
        return utility(state)
    v = +infinity
    for each legal action a:
        v = min(v, MAX-VALUE(Result(state, a)))
    return v
```

At the initial MAX state, select the action whose resulting MIN-VALUE is largest.

## 4.9 Properties and complexity

Let:

- \(b\) = branching factor, the typical number of legal moves;
- \(m\) = maximum search depth.

### Complete

For a finite game tree, minimax is complete: it eventually examines the tree and finds a solution.

### Optimal

It is optimal against an opponent who also plays optimally and when terminal utilities are available.

### Time

It may visit approximately \(O(b^m)\) leaf positions, so the time complexity is exponential in depth.

### Space

With ordinary depth-first traversal, space is \(O(bm)\) when storing a path and the remaining siblings along that path.

> Correction to a common slide typo: depth-first minimax space is generally \(O(bm)\), not \(O(b^m)\). The \(O(b^m)\) figure describes the tree/time growth.

---

## 4.10 Multiplayer and non-zero-sum games

With three or more players, a terminal value can be a tuple:

\[
(U_A,U_B,U_C)
\]

Each component is one player’s utility.

The generalized backup rule is:

- each player chooses the child whose tuple has the largest value in **that player’s own component**;
- the entire selected tuple is backed up to the parent.

For example, suppose player C chooses between:

\[
(1,2,6)\quad\text{and}\quad(5,4,5)
\]

C compares only the third components, 6 and 5, so C chooses:

\[
(1,2,6)
\]

A single MAX/MIN scalar is no longer enough because players may have independent goals.

## 4.11 Prisoner’s dilemma

Two prisoners separately choose whether to confess.

The slide’s payoffs, in years of sentence expressed as utilities, are:

| | Prisoner 2: Confess | Prisoner 2: Don’t confess |
|---|---|---|
| **Prisoner 1: Confess** | (−5, −5) | (0, −10) |
| **Prisoner 1: Don’t confess** | (−10, 0) | (−3, −3) |

For each prisoner, confessing gives the better individual payoff no matter what the other does:

- if the other confesses, −5 is better than −10;
- if the other does not confess, 0 is better than −3.

Thus both have an incentive to confess, producing \((-5,-5)\), even though mutual refusal \((-3,-3)\) would be better for both.

The **extensive form** draws the same situation as a game tree with decision nodes and payoff leaves. The **matrix/normal form** shows all strategy combinations in a table.

---

# 4.12 Alpha–beta pruning

## 4.13 Why pruning is needed

Plain minimax examines the whole tree. Alpha–beta pruning skips branches that cannot affect the final decision.

It gives the **same root value and same optimal move** as minimax when implemented correctly. It is an optimization, not a new game strategy.

A branch can be pruned only when we already have enough information to prove that rational players will never choose it.

## 4.14 Alpha and beta in very easy words

- **Alpha \(\alpha\):** the best score MAX can already guarantee somewhere along the current path. It only increases.
- **Beta \(\beta\):** the best score MIN can already guarantee somewhere along the current path. It only decreases.

Initial values:

\[
\alpha=-\infty,\qquad \beta=+\infty
\]

At a MAX node:

\[
\alpha\leftarrow\max(\alpha,v)
\]

At a MIN node:

\[
\beta\leftarrow\min(\beta,v)
\]

### Cutoff condition

\[
\boxed{\alpha\geq\beta}
\]

The remaining siblings at that node can be skipped.

### Simple intuition

If MAX already has a guaranteed 8 elsewhere, a MIN branch that MIN can force down to 4 or less cannot become MAX’s preferred choice. Its unexplored remainder cannot improve the root decision.

## 4.15 Alpha–beta pseudocode

```text
ALPHA-BETA(state, alpha, beta):
    if state is terminal:
        return utility(state)

    if MAX moves at state:
        v = -infinity
        for each action a in left-to-right order:
            v = max(v, ALPHA-BETA(Result(state,a), alpha, beta))
            alpha = max(alpha, v)
            if alpha >= beta:
                break                 # beta cutoff
        return v

    if MIN moves at state:
        v = +infinity
        for each action a in left-to-right order:
            v = min(v, ALPHA-BETA(Result(state,a), alpha, beta))
            beta = min(beta, v)
            if alpha >= beta:
                break                 # alpha cutoff
        return v
```

Values of alpha and beta are passed down the path. They are not restarted at every node.

---

## Solved Example 4B: Alpha–beta trace from the slide tree

Root is MAX. Its three children are MIN nodes with leaves:

- first MIN: 4, 8, 5;
- second MIN: 9, 3, 7;
- third MIN: 2, 4, 6.

Search left to right.

### Step 1: First MIN subtree

Read 4:

\[
\beta=4
\]

Read 8:

\[
\beta=\min(4,8)=4
\]

Read 5:

\[
\beta=\min(4,5)=4
\]

The first MIN value is 4.

At the root MAX:

\[
\alpha=\max(-\infty,4)=4
\]

MAX can now guarantee at least 4.

### Step 2: Second MIN subtree

This MIN node inherits \(\alpha=4\).

Read 9:

\[
\beta=9
\]

Read 3:

\[
\beta=\min(9,3)=3
\]

Now:

\[
\alpha=4\geq\beta=3
\]

So prune the remaining leaf 7.

Why? MIN can already make this branch worth at most 3, while MAX has a 4 elsewhere. Whether the last leaf is 7, 100, or −100 cannot make this branch better than the first for MAX.

### Step 3: Third MIN subtree

It also inherits \(\alpha=4\).

Read 2:

\[
\beta=2
\]

Immediately:

\[
\alpha=4\geq\beta=2
\]

Prune leaves 4 and 6.

### Step 4: Root decision

The best confirmed choice remains the first branch:

\[
\boxed{\text{Root value}=4}
\]

Pruned leaves: **7 in the second subtree, and 4 and 6 in the third subtree**.

---

## Solved Example 4C: Harder alpha–beta quiz

Consider this tree:

- root: MAX;
- root children L and R: MIN;
- L has two MAX children with leaves \((10,6)\) and \((100,8)\);
- R has two MAX children with leaves \((1,2)\) and \((20,4)\).

Search left to right.

### Step 1: Finish L

First MAX child:

\[
\max(10,6)=10
\]

Second MAX child:

\[
\max(100,8)=100
\]

L is MIN:

\[
L=\min(10,100)=10
\]

At root MAX:

\[
\alpha=10
\]

### Step 2: Enter R

R is MIN and inherits \(\alpha=10\).

Its first MAX child evaluates to:

\[
\max(1,2)=2
\]

At R:

\[
\beta=2
\]

Now:

\[
\alpha=10\geq\beta=2
\]

Prune R’s entire second MAX subtree, including leaves 20 and 4.

### Step 3: Explain the cutoff

R is worth at most 2 because MIN can choose the child worth 2. MAX already has a guaranteed 10 through L. The unseen leaves cannot change the fact that R is worse for MAX.

\[
\boxed{\text{Root value}=10,\quad\text{choose L}}
\]

Without pruning, ordinary minimax would also compute R = 2 and root = 10, but it would inspect every leaf.

### Related short quiz answer

For root MAX with two MIN children whose leaf pairs are \((10,8)\) and \((4,50)\):

- left MIN = 8, so root \(\alpha=8\);
- right MIN sees 4, giving \(\beta=4\leq8\);
- prune 50;
- root value = **8**.

## 4.16 Move ordering

Pruning depends on the order in which moves are examined.

- Good ordering: strong moves first, producing early cutoffs.
- Bad ordering: little or no pruning.

In the best case, alpha–beta can reduce the effective work to roughly \(O(b^{m/2})\). In the worst case, it remains \(O(b^m)\), like minimax.

This is why game engines spend effort ordering captures, checks, and previously successful moves near the front.

## 4.17 Heuristic alpha–beta search

Real games such as chess are usually too large to search to terminal states. We stop at a depth cutoff and estimate the value of the nonterminal position.

An **evaluation function** estimates how good a state is:

\[
Eval(s)=w_1f_1(s)+w_2f_2(s)+\cdots+w_nf_n(s)
\]

Here:

- \(f_i(s)\) is a feature, such as piece advantage or mobility;
- \(w_i\) is its weight.

A simple feature could be:

\[
f_1(s)=\text{number of white queens}-\text{number of black queens}
\]

### H-MINIMAX idea

\[
H\text{-}MINIMAX(s,d)=
\begin{cases}
Eval(s), & \text{if cutoff is reached}\\
\max_a H\text{-}MINIMAX(Result(s,a),d+1), & \text{MAX turn}\\
\min_a H\text{-}MINIMAX(Result(s,a),d+1), & \text{MIN turn}
\end{cases}
\]

Alpha–beta pruning can then be applied to this depth-limited search.

### Important limitation

The result is only as reliable as the evaluation function and search depth. A position that looks good at the cutoff may hide a tactic just beyond it.

---

# 5. Constraint Satisfaction Problems

## 5.1 What is a CSP?

A **constraint satisfaction problem** is a problem in which we must assign values to variables without breaking any constraints.

A CSP has three parts:

\[
\boxed{CSP=(X,D,C)}
\]

where:

- \(X=\{X_1,\ldots,X_n\}\): variables;
- \(D=\{D_1,\ldots,D_n\}\): their domains;
- \(C\): constraints.

A **solution** is a complete assignment that satisfies every constraint.

### CSP versus ordinary search

In ordinary search, a state can be treated as a black box. We mainly need a successor function, goal test, and possibly a heuristic.

In a CSP, the state has explicit structure:

- variables;
- possible values;
- allowed combinations.

This structure lets general algorithms detect illegal assignments and impossible values early.

## 5.2 How to convert a problem into a CSP

Use three steps:

1. **Choose variables:** What decisions must be made?
2. **Choose domains:** What values can each decision take?
3. **Write constraints:** Which assignments are forbidden or allowed?

A clear formulation often makes the algorithm straightforward.

## 5.3 Sudoku as a CSP

- **Variables:** the cells A1 through I9.
- **Domains:** usually \(\{1,2,3,4,5,6,7,8,9\}\) for an empty cell; a fixed cell has a singleton domain.
- **Constraints:** digits in each row are all different; digits in each column are all different; digits in each \(3\times3\) box are all different.

A complete grid is a solution only if all three constraint types hold.

## 5.4 Map coloring as a CSP

Rule: color each region red, green, or blue so that adjacent regions have different colors.

Variables for the Australia example:

\[
WA,NT,SA,Q,NSW,V,T
\]

Domain for every region:

\[
D_i=\{R,G,B\}
\]

Example binary constraint:

\[
WA\ne NT
\]

As allowed pairs, this includes:

\[
(R,G),(R,B),(G,R),(G,B),(B,R),(B,G)
\]

The pair \((R,R)\) is not allowed.

### One valid assignment

\[
WA=R,\quad NT=G,\quad SA=B,\quad Q=R,
\]

\[
NSW=G,\quad V=R,\quad T=R
\]

Check the important adjacencies:

- WA–NT: R–G, allowed;
- WA–SA: R–B, allowed;
- NT–SA: G–B, allowed;
- NT–Q: G–R, allowed;
- SA–Q: B–R, allowed;
- SA–NSW: B–G, allowed;
- SA–V: B–R, allowed;
- Q–NSW: R–G, allowed;
- NSW–V: G–R, allowed.

Tasmania has no adjacency constraint with the mainland in this graph, so any of its three colors works.

## 5.5 Constraint graphs

In a **binary CSP**, every constraint involves at most two variables.

In its constraint graph:

- nodes are variables;
- an edge/arc between nodes means a constraint exists.

For map coloring, edges connect adjacent regions.

### Why the graph helps

The graph supports:

- variable-ordering heuristics;
- constraint propagation;
- identification of separate subproblems.

Tasmania is disconnected, so it can be solved independently. There is no reason to recolor Tasmania to fix a conflict in South Australia.

## 5.6 Types of constraints

### Unary constraint

Involves one variable:

\[
SA\ne Green
\]

It can usually be applied immediately by removing Green from SA’s domain.

### Binary constraint

Involves two variables:

\[
SA\ne WA
\]

### Higher-order constraint

Involves three or more variables. Cryptarithmetic column constraints and Sudoku’s all-different constraints are examples.

An **Alldiff** constraint requires every variable in a set to have a different value.

---

# 5.7 Backtracking search

## 5.8 Basic idea

Backtracking is depth-first search with one variable assigned at each level.

At each step:

1. Choose an unassigned variable.
2. Try a value from its domain.
3. Check whether the value violates any constraint with already assigned variables.
4. If legal, assign it and continue.
5. If no legal value remains, undo the previous assignment and try another value.

The undo-and-return step is called **backtracking**.

### Commutativity of assignments

These produce the same partial assignment:

\[
WA=Red\text{ then }NT=Green
\]

and:

\[
NT=Green\text{ then }WA=Red
\]

Therefore, we do not need separate branches for every possible order of assigning the same variables. We can choose one variable at each depth.

### Fail early

A partial assignment is abandoned as soon as it violates a constraint. There is no benefit in completing an already invalid assignment.

\[
\boxed{\text{Backtracking}=\text{DFS}+\text{variable ordering}+\text{fail on violation}}
\]

## 5.9 Backtracking pseudocode

```text
BACKTRACK(assignment, csp):
    if assignment is complete:
        return assignment

    X = select an unassigned variable

    for each value v in DOMAIN[X]:
        if v is consistent with assignment:
            assign X = v
            result = BACKTRACK(assignment, csp)
            if result is not failure:
                return result
            remove X = v                 # backtrack

    return failure
```

The worst case can still be exponential. Heuristics and propagation reduce the number of failed branches in practice.

---

# 5.10 Variable and value ordering

Backtracking improves greatly when we answer three questions well:

1. Which variable should be assigned next?
2. In what order should its values be tried?
3. Can inevitable failure be detected early?

## 5.11 Minimum Remaining Values — MRV

Choose the unassigned variable with the fewest legal values remaining.

It is also called the **most constrained variable** heuristic.

### Why it helps

A variable with one remaining value is forced. Assigning it now either:

- makes progress immediately; or
- exposes a failure before more work is wasted.

This is a **fail-first** strategy.

### Map-coloring example

After WA = Red:

- NT cannot be Red, so it has 2 values;
- SA cannot be Red, so it has 2 values;
- several others still have 3.

After NT = Green, SA cannot use Red or Green, so:

\[
D_{SA}=\{Blue\}
\]

MRV chooses SA next because it has only one value.

If a variable has zero values, the current branch fails immediately.

## 5.12 Degree heuristic

The **degree heuristic** chooses the variable with the most constraints on remaining unassigned variables.

It is also called the **most constraining variable** heuristic.

In the Australia graph, SA has the highest degree because it constrains many other regions. Choosing SA early can reduce many neighbors’ domains.

### Combining MRV and degree

A common rule is:

1. Use MRV first.
2. If several variables tie, use degree to break the tie.

MRV finds the variable closest to failure; degree chooses the one likely to have the greatest effect on the rest.

## 5.13 Least Constraining Value — LCV

After choosing a variable, LCV orders its values by how few choices they remove from neighboring variables.

Try the value that leaves the most flexibility first.

- MRV and degree choose a **variable**.
- LCV chooses a **value order**.

LCV is often called a **succeed-first** heuristic because it tries to preserve options.

---

# 5.14 Filtering and forward checking

**Filtering** means removing values that cannot currently be legal.

**Forward checking** works after each assignment:

1. Assign \(X=v\).
2. Look at every unassigned neighbor of X.
3. Remove values that conflict with \(X=v\).
4. If any domain becomes empty, backtrack immediately.

This empty-domain event is often called **domain wipeout**.

### What forward checking does not do

It checks constraints between:

- the newly assigned variable; and
- its unassigned neighbors.

It does not necessarily check whether two unassigned variables can still support each other. Arc consistency does more propagation.

---

## Solved Example 5A: 4-Queens with forward checking

Place four queens on a \(4\times4\) board so that no two share:

- a row;
- a column;
- a diagonal.

### CSP formulation

Let \(Q_i\) be the row of the queen in column i.

\[
D(Q_i)=\{1,2,3,4\},\qquad i=1,2,3,4
\]

For every pair \(i<j\), require:

\[
Q_i\ne Q_j
\]

to prevent the same row, and:

\[
|Q_i-Q_j|\ne|i-j|
\]

to prevent a diagonal attack.

Using one queen per column already prevents same-column attacks.

### Branch 1: Try \(Q_1=1\)

Forward checking removes row 1 and diagonal attacks from future columns:

\[
D(Q_2)=\{3,4\}
\]

\[
D(Q_3)=\{2,4\}
\]

\[
D(Q_4)=\{2,3\}
\]

For example, \(Q_2\) cannot be 1 or 2: 1 is the same row and 2 is diagonal to row 1 in the previous column.

#### Try \(Q_2=3\)

For \(Q_3\):

- start with \(\{2,4\}\);
- row 2 is diagonal to \(Q_2=3\);
- row 4 is also diagonal to \(Q_2=3\).

Thus:

\[
D(Q_3)=\varnothing
\]

Domain wipeout. Backtrack.

#### Try \(Q_2=4\)

Forward checking leaves:

\[
D(Q_3)=\{2\}
\]

So assign \(Q_3=2\). Now \(Q_4\) has no legal value:

- row 2 is already used by \(Q_3\);
- row 3 is diagonal to \(Q_3=2\);
- other rows were already removed.

Again:

\[
D(Q_4)=\varnothing
\]

Therefore \(Q_1=1\) cannot lead to a solution under this search.

### Branch 2: Try \(Q_1=2\)

Forward checking gives:

\[
D(Q_2)=\{4\},\qquad D(Q_3)=\{1,3\},\qquad D(Q_4)=\{1,3,4\}
\]

Assign the forced value:

\[
Q_2=4
\]

Now:

\[
D(Q_3)=\{1\}
\]

So:

\[
Q_3=1
\]

The only remaining legal value for the last queen is:

\[
Q_4=3
\]

### Solution

\[
\boxed{(Q_1,Q_2,Q_3,Q_4)=(2,4,1,3)}
\]

Board, with Q marking queens:

| Row | Column 1 | Column 2 | Column 3 | Column 4 |
|---|---|---|---|---|
| 1 |  |  | Q |  |
| 2 | Q |  |  |  |
| 3 |  |  |  | Q |
| 4 |  | Q |  |  |

Final check:

- rows used: 2, 4, 1, 3 — all different;
- columns: one queen in each by formulation;
- diagonal row differences never equal column differences.

The other 4-Queens solution is \((3,1,4,2)\), its mirror image.

---

# 5.15 Arc consistency

## 5.16 Consistency of one arc

An arc:

\[
X\rightarrow Y
\]

is consistent when, for **every** value x in \(D(X)\), there is **at least one** value y in \(D(Y)\) such that \((x,y)\) satisfies the constraint.

If some x has no supporting y, delete x from the **tail** X’s domain.

> Remember: delete from the tail, not the head.

### Small example

Suppose:

\[
D(X)=\{1,2\},\qquad D(Y)=\{1\}
\]

with constraint \(X\ne Y\).

For arc \(X\rightarrow Y\):

- X = 1 has no allowed Y value, because Y = 1 is equal;
- X = 2 has support, because Y = 1 is different.

Delete 1 from X:

\[
D(X)=\{2\}
\]

For the reverse arc \(Y\rightarrow X\), Y = 1 has support X = 2, so nothing is removed.

## 5.17 Arc consistency of the whole CSP

A CSP is arc consistent when every directed arc is consistent.

If X loses a value, arcs pointing from its neighbors back to X must be checked again:

\[
Z\rightarrow X
\]

A value in Z that previously relied on the deleted X value may now have lost its only support.

This repeated spreading of deletions is called **constraint propagation**.

## 5.18 AC-3 algorithm

AC-3 keeps a queue of arcs to examine.

```text
AC-3(csp):
    queue = every directed arc (Xi, Xj)

    while queue is not empty:
        (Xi, Xj) = remove first arc from queue

        if REVISE(Xi, Xj):
            if DOMAIN[Xi] is empty:
                return failure

            for each neighbor Xk of Xi except Xj:
                add (Xk, Xi) to queue

    return reduced domains

REVISE(Xi, Xj):
    removed = false

    for each x in DOMAIN[Xi]:
        if no y in DOMAIN[Xj] allows (x, y):
            delete x from DOMAIN[Xi]
            removed = true

    return removed
```

### Why neighbors are re-added

A revision can cause a chain reaction:

\[
A\text{ loses a value}\Rightarrow B\text{ loses support}\Rightarrow C\text{ loses support}
\]

The queue ensures that the process continues until no more values need removal.

## 5.19 Forward checking versus arc consistency

| Feature | Forward checking | Arc consistency |
|---|---|---|
| Main trigger | A new assignment | Can run before search and after assignments |
| Checks | Assigned variable against unassigned neighbors | Both directions across arcs until stable |
| Propagation between unassigned variables | Limited | Yes |
| Failure detection | Earlier than plain backtracking | Often earlier than forward checking |
| Cost per step | Usually cheaper | More work maintaining the queue |

**MAC**, or **Maintaining Arc Consistency**, means enforcing arc consistency after every search assignment. It combines backtracking search with propagation.

Arc consistency can be used:

- as preprocessing before search;
- after each assignment during search.

The trade-off is stronger pruning versus more computation at each node.

---

## Solved Example 5B: Arc consistency on three color variables

The slide example has three variables with pairwise different-color constraints:

\[
D(V_1)=\{R,G,B\},\quad D(V_2)=\{R,G\},\quad D(V_3)=\{G\}
\]

Every pair must be different.

### Step 1: Examine \(V_1\rightarrow V_3\)

Because \(V_3\) can only be G, \(V_1=G\) has no support. Delete G:

\[
D(V_1)=\{R,B\}
\]

R and B both have support G.

### Step 2: Examine \(V_2\rightarrow V_3\)

\(V_2=G\) has no support, so delete G:

\[
D(V_2)=\{R\}
\]

### Step 3: Recheck \(V_1\rightarrow V_2\)

Now \(V_2\) can only be R. Therefore \(V_1=R\) has no support and is deleted:

\[
D(V_1)=\{B\}
\]

### Final domains

\[
\boxed{V_1=B,\qquad V_2=R,\qquad V_3=G}
\]

All pairwise constraints are satisfied.

The important working pattern is: a deletion in one domain can force another deletion elsewhere, so previously checked arcs may need to return to the queue.

---

## 5.20 Limitations of arc consistency

After arc consistency finishes, any of these can be true:

1. Exactly one solution remains.
2. Several solutions remain.
3. No solution exists, but arc consistency has not proved it.

Arc consistency is a **local** consistency check. It does not guarantee global satisfiability.

### Unsolvable example that is still arc consistent

Color a triangle using only two colors, with every adjacent pair different.

Each variable starts with:

\[
D_i=\{R,G\}
\]

For every arc, either color has support in the other node: R is supported by G, and G is supported by R. Thus no value is deleted.

Yet a triangle cannot be colored with only two colors: after A = R and B = G, C must differ from both, which is impossible.

Therefore arc consistency still needs to run inside backtracking search.

---

# 5.21 Intelligent backtracking

## 5.22 Chronological backtracking

Ordinary chronological backtracking returns to the immediately previous variable, even if that variable had nothing to do with the failure.

This can waste time.

## 5.23 Conflict sets and backjumping

A **conflict set** is a set of earlier assignments responsible for the current variable having no legal value.

**Backjumping** returns directly to a variable in the conflict set that might actually fix the failure, skipping irrelevant variables.

### Map-coloring example from the slides

Variable order:

\[
Q,NSW,V,T,SA,WA,NT
\]

Partial assignment:

\[
Q=Red,\quad NSW=Green,\quad V=Blue,\quad T=Red
\]

When SA is tried, every color conflicts with its neighbors. Tasmania is not connected to SA, so changing T cannot help.

The conflict set for SA is based on:

\[
\{Q=Red,\ NSW=Green,\ V=Blue\}
\]

Backjumping skips Tasmania and returns to V, the most recent relevant variable, to try another value.

## 5.24 Constraint learning

**Constraint learning** records the reason for failure so the same impossible combination is not tried again.

A learned **nogood** could say:

> The combination \(Q=Red,NSW=Green,V=Blue\) cannot lead to a solution under the relevant remaining constraints.

Future search can reject that combination immediately.

Backjumping uses the conflict to decide **where to return**. Constraint learning stores information to avoid **repeating the same failure later**.

---

# 5.25 Cryptarithmetic puzzles

In cryptarithmetic, each letter represents a digit. Usually:

- different letters have different digits;
- the arithmetic equation must be correct;
- leading letters cannot be zero.

For:

\[
TWO+TWO=FOUR
\]

variables include the letters and carry digits. Constraints include:

- \(Alldiff(F,T,U,W,R,O)\);
- column-addition equations;
- carries in \(\{0,1\}\);
- \(T\ne0\) and \(F\ne0\).

The carry variables are essential because a column’s result affects the next column. These are higher-order constraints, not just pairwise ones.

---

## Solved Example 5C: SEND + MORE = MONEY

The puzzle is:

```text
  S E N D
+ M O R E
---------
M O N E Y
```

A valid digit assignment is:

| Letter | S | E | N | D | M | O | R | Y |
|---|---:|---:|---:|---:|---:|---:|---:|---:|
| Digit | 9 | 5 | 6 | 7 | 1 | 0 | 8 | 2 |

Substitute:

```text
  9567
+ 1085
------
 10652
```

### Column-by-column verification

**Units:**

\[
D+E=7+5=12
\]

Write Y = 2 and carry 1.

**Tens:**

\[
N+R+1=6+8+1=15
\]

Write E = 5 and carry 1.

**Hundreds:**

\[
E+O+1=5+0+1=6
\]

Write N = 6 and carry 0.

**Thousands:**

\[
S+M=9+1=10
\]

Write O = 0 and carry 1.

**Ten-thousands:**

The final carry is M = 1.

### Constraint check

- All letter digits are distinct: 9, 5, 6, 7, 1, 0, 8, 2.
- S = 9 and M = 1, so neither leading letter is zero.
- The arithmetic is correct: \(9567+1085=10652\).

A CSP solver would discover this through assignments, carry constraints, propagation, and backtracking rather than by merely guessing the final mapping.

---

# 6. Final revision guide

## 6.1 Probability formulas

\[
P(\neg A)=1-P(A)
\]

\[
P(A\lor B)=P(A)+P(B)-P(A\land B)
\]

\[
P(A\mid B)=\frac{P(A\land B)}{P(B)}
\]

\[
P(A\land B)=P(A\mid B)P(B)
\]

\[
P(A)=P(A\mid B)P(B)+P(A\mid\neg B)P(\neg B)
\]

\[
P(A\mid B)=\frac{P(B\mid A)P(A)}{P(B)}
\]

Independent:

\[
P(A\land B)=P(A)P(B)
\]

Conditionally independent given C:

\[
P(A,B\mid C)=P(A\mid C)P(B\mid C)
\]

Expected utility:

\[
EU(a)=\sum_iP(o_i\mid a)U(o_i)
\]

## 6.2 Bayesian-network working pattern

1. Write the network factorization.
2. For a complete assignment, multiply the selected CPT entries.
3. For a hidden variable, sum over all its values.
4. For a conditional query, calculate numerator and denominator, then normalize.

Network rule:

\[
P(x_1,\ldots,x_n)=\prod_iP(x_i\mid parents(X_i))
\]

Naive Bayes score:

\[
Score(C)=P(C)\prod_iP(A_i\mid C)
\]

## 6.3 Minimax working pattern

1. Start at terminal leaves.
2. Back up maximum values at MAX nodes.
3. Back up minimum values at MIN nodes.
4. Choose the root action with the best backed-up value.

## 6.4 Alpha–beta working pattern

Start:

\[
\alpha=-\infty,\qquad\beta=+\infty
\]

At MAX:

\[
v\leftarrow\max(v,child),\qquad \alpha\leftarrow\max(\alpha,v)
\]

At MIN:

\[
v\leftarrow\min(v,child),\qquad \beta\leftarrow\min(\beta,v)
\]

Prune remaining siblings when:

\[
\alpha\geq\beta
\]

Always explain a cutoff using the alternative already available to the opponent/player higher in the tree.

## 6.5 CSP working pattern

1. Define variables, domains and constraints.
2. Choose a variable using MRV, with degree as a useful tie-breaker.
3. Order values, often using LCV.
4. Check each new assignment immediately.
5. Use forward checking or arc consistency to remove impossible values.
6. Backtrack on domain wipeout.
7. Use backjumping when only earlier relevant variables can fix the conflict.

Arc \(X\rightarrow Y\):

> Every value in X needs at least one supporting value in Y. Otherwise, delete the unsupported value from X.

## 6.6 Common exam mistakes

- Treating \(P(A\mid B)\) as equal to \(P(B\mid A)\).
- Forgetting the denominator in Bayes’ rule when an actual probability is requested.
- Using the favorable-outcomes formula when outcomes are not equally likely.
- Confusing mutually exclusive events with independent events.
- Forgetting to sum over hidden variables in a Bayesian network.
- Using the wrong CPT row for parent values.
- Taking a maximum at a MIN node or a minimum at a MAX node.
- Resetting alpha and beta at every node instead of passing them down the path.
- Pruning merely because a value looks bad, without proving \(\alpha\geq\beta\).
- Believing alpha–beta changes the minimax answer.
- Choosing MRV by the largest domain instead of the smallest.
- Deleting from the head during arc revision instead of the tail.
- Assuming arc consistency guarantees that a solution exists.
- Backtracking to Tasmania when Tasmania is unrelated to the conflict.

## 6.7 One-page concept comparison

| Concept | Main question answered |
|---|---|
| Probability | How likely is an event? |
| Conditional probability | How does evidence change that likelihood? |
| Bayes’ rule | How can I reverse cause–effect probability? |
| Independence | Does one event tell me anything about another? |
| Bayesian network | How can many probabilities be represented compactly? |
| Naive Bayes | Which class best explains the observed attributes? |
| Expected utility | Which action is best after considering chance and value? |
| Minimax | What is the best move against an optimal opponent? |
| Alpha–beta | Which game-tree branches provably cannot matter? |
| Backtracking | Which partial assignment should I undo and retry? |
| MRV | Which variable is closest to failing or being forced? |
| Degree heuristic | Which variable constrains the most remaining variables? |
| Forward checking | Which neighbor values conflict with my new assignment? |
| Arc consistency | Does every value still have support across each constraint? |
| Backjumping | Which earlier relevant assignment caused this failure? |

You should now be able to solve the uploaded examples by recognizing the problem type, writing the governing rule, substituting carefully, and interpreting the final result in plain language.
