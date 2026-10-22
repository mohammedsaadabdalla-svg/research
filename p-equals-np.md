# P = NP as a Problem of Factorization

## Overview

This document presents a framework in which the question P = NP is recast as a problem of
factorization. The central claim of the framework is this: an NP problem instance arrives in
raw form as a circuit, and solving it means factoring that circuit into a closed-form
function — a finite formula — that directly produces every valid answer of the instance's
constraint. The difficulty of the problem is located not in searching the answer space, but
in decomposing the transformation that maps inputs to the required answer.

Within the framework, verification and solving are two different relationships to the same
transformation. The checker consumes the transformation as given, in its raw circuit form,
and evaluates it on a candidate. The solver must take the circuit apart, understand it, and
reassemble it as a factored formula whose index inputs stand in for the search that brute
force would otherwise perform.

This document states the framework only. It does not claim that the argument establishing
P = NP through decomposition is correct; it lays out the reasoning and its terms.

## The transformation

Every constraint satisfaction problem defines a transformation: a closed-form function,
where closed form means literally a finite formula, that given an input produces the
required output.

The transformation does not produce a single privileged answer. It produces all valid
answers of the constraint. Its structure is therefore indexed: the function takes, as one
of its inputs, an index over the state space of valid answers of the constraint. For an
instance with N valid answers, every index in the range corresponding to N selects a
member of the valid answer set, and evaluating the function at that index yields that
valid answer.

The transformation is thus of the shape:

    f(instance, index) -> valid answer

The instance fixes the constraint. The index selects which of the constraint's valid
answers is produced. The formula itself carries the entire solution space; no enumeration
of candidates is embedded in it, because position in the answer space is already an input.

## Factorization

To factor the transformation is to decompose it into a composition of sub-functions. The
raw object is a circuit: gates wired together, evaluating candidate against constraint.
The factored object is a formula: a closed-form expression in which the mapping from
instance and index to valid answer is made explicit as composed simpler functions.

The two forms are related as raw material and product. The circuit is the raw
transformation — its structure computes, but does not exhibit, how outputs follow from
inputs. The formula is the factored transformation — equivalent to the circuit in what it
computes, but organized so that its output can be produced directly from an index instead
of recovered by trial.

Factorization, in this sense, is not a search through the answer space. It is a
restructuring of the transformation itself: taking the wired-up circuit apart into a
hierarchy of sub-functions and composing them into a form where the answer space is
addressed by index.

## The checker

The instance given to the checker is a circuit. The checker's operation is evaluation: it
inputs a candidate into the circuit, receives an answer, and checks whether that answer is
equal to the constraint or the certificate. Acceptance depends only on the output matching;
it does not depend on where the candidate came from.

The candidate the checker tests may be treated as random in this specific sense: it is
origin-blind. A candidate may have been guessed, brute-forced, supplied by an adversary,
or produced by any other process — the checker does not ask. It evaluates and compares.

The path the checker recognizes is previously evaluated in the sense of the certificate or
witness: a recorded chain of values whose correctness is re-established by running it
against the circuit, not by any new discovery. The checker therefore confirms that a
supplied answer holds under the transformation; it never produces the transformation's
general behavior. Each check covers one candidate, once.

## The solver

The solver receives the same raw circuit, but evaluation is not its task. The solver must
understand the circuit — see how its gates compose — and from that understanding produce
an input that validates the answer without brute force.

Concretely, the solver's task has two outputs:

1. A custom factored formula f, generated toward the constraint goal of the instance — the
   closed-form function that produces all valid answers to that constraint; and
2. An index into that formula, whose evaluation yields a valid answer.

The solver's product is predetermined and non-random in two respects. The formula it
builds is fixed by the constraint: it is constructed for that constraint's goal, not sampled
from a distribution of possible formulas. And the answer it delivers is computed directly
from the formula at an index — produced, not found. For the solver's factored f, any index
with the size of the valid solution space outputs a valid solution; correctness comes from
the structure of f, not from the luck of the candidate.

Where the checker consumes one candidate at a time through the raw circuit, the solver
replaces the circuit with its general path: the factored closed-form function that answers
for every index at once.

## Checker and solver compared

| | Checker | Solver |
|---|---|---|
| **Input form** | The circuit, as given | The circuit, as given |
| **Operation** | Evaluate a candidate; compare against constraint or certificate | Understand the circuit; factor it into a closed form |
| **Output** | Accept or reject one candidate | A factored formula f, plus an index into it |
| **Relationship to answers** | Consumes a single answer supplied from outside | Produces answers from the formula for any index |
| **Role of the answer's origin** | Irrelevant — any candidate counts | Irrelevant after factoring — every index in range is valid |
| **Character of the result** | Previously evaluated path: certificate or witness, re-verified | Predetermined path: computed, not searched |

The asymmetry is structural. The checker never needs the formula because it never needs to
generate answers — it only needs to test them. The solver cannot afford brute force, so it
must replace search with the formula itself: factor the transformation until indexing
replaces enumeration.

## Decision and search problems

The framework covers all NP problems, in both of their standard forms.

For a search problem, the required output is an object satisfying the constraint — an
assignment, a coloring, a factorization, a witness of the desired kind. The indexed
transformation f(instance, index) produces such objects directly.

For a decision problem, the required output is the predicate's verdict, and the indexed
transformation evaluates that predicate; the same decomposition applies, with the answer
space reduced to the verdict the circuit computes. In both cases the instance is a circuit,
the checker tests candidates against it, and the solver factors it into a closed form. The
form of the answer changes; the structure of verification and of factorization does not.

## Space and time in decomposition

The framework treats the time cost of factorization as dependent on the space of the
computation. More memory means less time complexity: the decomposition process retains
intermediate state — partial sub-functions, intermediate results of the factoring — and the
retention of that state is what shortens the time the factorization takes.

This is stated qualitatively. The framework asserts a direction — memory available to the
decomposition buys reductions in the time needed to reach the factored form — without
fixing a specific bound. The space in question is the working memory of the decomposition
itself: the intermediate decomposition state held while the circuit is being taken apart
and recomposed.

## The P = NP claim as posed by this framework

The framework locates both sides of P = NP in one object. Checking is evaluating the raw
transformation on a supplied answer; solving is possessing the factored transformation,
whose indices address the answer space directly. If the transformation behind any NP
instance can be factored — decomposed into a composition of sub-functions yielding the
closed form — then the solver's product and the checker's input become two forms of the
same function: one wired to test a single candidate, the other written to produce all
candidates.

P = NP, as posed here, is the question of whether the factorization is always available:
whether every raw circuit whose answers a checker can verify is also a transformation that
can be decomposed into the closed form that a solver could hold. The framework's thesis is
that this — not the enumeration of candidates — is what the equality would mean.

## Status of this document

This is a neutral statement of the framework. It presents the definitions, the
checker–solver distinction, the factorization claim, and the space–time remark as the
framework gives them, and it makes no assertion of its own about whether the decomposition
argument settles P = NP.
