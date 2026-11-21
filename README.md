# The Degenerate Hecke-Ideal Branch in the Claimed Lean Proof of the Riemann Hypothesis

**Independent mathematical and Lean source-code audit**  
**Author:** Ilgiz Faritovich Murtazin  
**Research note:** 21 November 2026  
**Status:** A documented degeneracy in intermediate statements; **no invalidity of the final RH/GRH proof has been established**.

## Research note

**[Read the full research note (PDF)](degenerate-hecke-ideal-note.pdf)**

This repository documents a narrowly scoped, reproducible examination of the **degenerate Hecke-ideal branch** in the claimed Lean formalization of the Riemann Hypothesis. It examines how a zero Hecke module can lead to the top ideal, how certain subsequent numerical assertions can then become *vacuously true*, and what would be required to establish whether this degeneracy matters to a downstream arithmetic argument.

The analysis concerns a **specific pinned revision** of the Lean source, not necessarily subsequent revisions.

## Main mathematical observation

Let \(B\) be a nonzero commutative ring with identity, \(M_K\) a relevant Hecke module, and \(J\) an ideal of \(B\). The examined source explicitly admits the branch

$$
M_K=0 \quad\Longrightarrow\quad J=B,
$$

so the quotient is the zero ring:

$$
B/J=B/B=0.
$$

For any nonzero field \(k\) and unital ring homomorphism \(x:B\to k\),

$$
J=B \quad\Longrightarrow\quad J\not\subseteq\ker x.
$$

Indeed, \(J\subseteq\ker x\) would imply \(x(1_B)=0\), contradicting \(x(1_B)=1_k\ne0\).

Consequently, when \(J=B\), a conditional claim of the form

$$
\forall x:B\to k,\qquad
\bigl(J\subseteq\ker x\bigr)\Longrightarrow \mathcal P(x)
$$

holds **vacuously**, regardless of the predicate \(\mathcal P\). Such a statement, by itself, produces neither an arithmetic point nor a positive arithmetic contribution.

## Findings in the pinned Lean source

1. **Explicit top-ideal branch.** `ExistsHeckeIdeal.lean` handles the zero-Hecke-module case by selecting the top ideal \(J=B\).
2. **Support and degeneracy.** Under the `TWBase` support-equivalence condition, the note proves \(J=B\iff M=0\) for the relevant module, under its stated assumptions. This statement is **not** asserted for arbitrary unrelated modules.
3. **Vacuous numerical branch.** `TNumJE.lean`, in the proof of `numJE_alt`, explicitly uses `PUnit` and a zero endomorphism for the top-ideal branch. The relevant pointwise numerical implication is discharged without constructing an arithmetic point.
4. **Existential formulation.** The displayed form of `ModNAlt` does not explicitly require \(J\ne B\). This alone does **not** establish that its other conjuncts are satisfied when \(J=B\).
5. **Existing safeguards.** The source also contains hypotheses capable of forcing \(J\ne B\) when a suitable arithmetic point or a specified nondegeneracy condition is actually available.

These are observations about **intermediate constructions and their logical content**. A vacuously true Lean proposition is not a kernel error or an invalid inference.

## What this analysis does *not* establish

This repository does **not** claim that:

- the final formal derivation of RH or GRH is incorrect;
- Lean accepted a logically invalid inference;
- the degenerate branch necessarily reaches a global conclusion without an independent nondegeneracy argument; or
- there is a verified direct dependency chain from `ExistsHeckeIdeal` through `ModNAlt` to the global Selmer estimate `PropGK`.

The note specifically observes that `PropGK` has other immediate inputs. Determining its full transitive dependency structure requires further source-level analysis.

## The decisive open audit question

**Where, before a nonvacuous arithmetic consequence is used, is a suitable arithmetic point actually constructed and shown to satisfy**

$$
J\subseteq\ker x\,?
$$

If such a point exists with values in a nonzero field, then \(J\ne B\) follows automatically. If instead a downstream theorem derives existence of a genuine point, nonzero invariant vector, or positive numerical contribution using *only* the vacuous top-ideal data, without an independent properness or existence argument, that would identify a substantive defect requiring precise proof.

The next audit steps are to track the chosen \(J\) through the relevant uses of `HidaUAlt`, `ModularAlt`, and `ModNAlt`; identify the first genuine arithmetic-point or positive-rank requirement; and trace any actual dependencies leading to `OneCharU_s` and `PropGK`.

## Pinned source and reproducibility

All source references below are to the same upstream commit:

- **Upstream repository:** [leanprover-community/riemann-hypothesis](https://github.com/leanprover-community/riemann-hypothesis)
- **Pinned commit:** [`40e56bab026552e089d7afcfeff08d2defb9c005`](https://github.com/leanprover-community/riemann-hypothesis/tree/40e56bab026552e089d7afcfeff08d2defb9c005)

| Source file | Relevant lines | Role in the audit |
| --- | --- | --- |
| [ExistsHeckeIdeal.lean](https://github.com/leanprover-community/riemann-hypothesis/blob/40e56bab026552e089d7afcfeff08d2defb9c005/src/Development/AutomorphicArithmetic/AutomorphicVarieties/ModularityArgument/ThresholdArguments/DenseThresholdCases/ExistsHeckeIdeal.lean#L52-L71) | 52–71 | The top-ideal branch |
| [ThetaCoefficientMapEq.lean](https://github.com/leanprover-community/riemann-hypothesis/blob/40e56bab026552e089d7afcfeff08d2defb9c005/src/Development/AutomorphicArithmetic/AutomorphicVarieties/PatchingModules/ThetaCoefficientMapEq.lean#L163-L175) | 163–175 | `TWBase` support equivalence |
| [TNumJE.lean](https://github.com/leanprover-community/riemann-hypothesis/blob/40e56bab026552e089d7afcfeff08d2defb9c005/src/Development/AutomorphicArithmetic/AutomorphicVarieties/ModularityArgument/ThresholdArguments/DenseThresholdCases/TNumJE.lean#L171-L198) | 171–198 | `numJE_alt` and the vacuous branch |
| [AbsGood.lean](https://github.com/leanprover-community/riemann-hypothesis/blob/40e56bab026552e089d7afcfeff08d2defb9c005/src/Development/AutomorphicArithmetic/AutomorphicVarieties/ModularityArgument/LinkedUnramifiedChains/AbsGood.lean#L17-L35) | 17–35; 90–100 | `Num` and `End` conditions |
| [LevelRelationTwoX.lean](https://github.com/leanprover-community/riemann-hypothesis/blob/40e56bab026552e089d7afcfeff08d2defb9c005/src/Development/AutomorphicArithmetic/AutomorphicVarieties/ModularityArgument/ThresholdArguments/DenseThresholdCases/LevelRelationTwoX.lean#L18-L23) | 18–23 | `ModNAlt` formulation |
| [JPTRow.lean](https://github.com/leanprover-community/riemann-hypothesis/blob/40e56bab026552e089d7afcfeff08d2defb9c005/src/Development/AutomorphicArithmetic/AutomorphicVarieties/ModularityArgument/ThresholdArguments/DenseThresholdCases/JPTRow.lean#L34-L41) | 34–41 | Point-implies-properness safeguard |
| [RadOfDense.lean](https://github.com/leanprover-community/riemann-hypothesis/blob/40e56bab026552e089d7afcfeff08d2defb9c005/src/Development/AutomorphicArithmetic/AutomorphicVarieties/ModularityArgument/ThresholdArguments/DenseThresholdCases/RadOfDense.lean#L50-L66) | 50–66 | Construction of the `ModNAlt` witness |
| [UniversalOrdinaryDeterminantRing.lean](https://github.com/leanprover-community/riemann-hypothesis/blob/40e56bab026552e089d7afcfeff08d2defb9c005/src/Development/AutomorphicArithmetic/AutomorphicVarieties/WeilGroupCharacters/UniversalOrdinaryDeterminantRing.lean#L284-L298) | 284–298 | Conditional core predicates |

To inspect the pinned source locally:

```bash
git clone https://github.com/leanprover-community/riemann-hypothesis.git
cd riemann-hypothesis
git checkout 40e56bab026552e089d7afcfeff08d2defb9c005
```

The audit is based on inspection and mathematical analysis of the specified source locations. **This repository does not claim to contain an independently recompiled, end-to-end verification of the entire upstream formalization.**

## Citation

Murtazin, Ilgiz Faritovich. *The Degenerate Hecke-Ideal Branch in the Claimed Lean Proof of the Riemann Hypothesis: A Code-Level Mathematical Examination of the Zero-Module Case.* Research note, 21 November 2026.

## Scope and updates

This repository is intended as a transparent record of a focused mathematical audit. Any later discovery of a valid nondegeneracy argument—or of a specific incorrect downstream inference—should be documented with its exact theorem references and commit identifier. Conclusions in this note are limited to the pinned source revision.
