# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This is a benchmark dataset for **Vericoding** - formally verified program synthesis. It contains 12,504 verification tasks in Dafny, Lean, and Verus, along with experiment results from AI vericoders.

Paper: https://www.arxiv.org/abs/2509.22908
Scripts: https://github.com/beneficial-AI-Foundation/vericoding

## Repository Structure

- `specs/` - Specification files with tasks to be solved (12,504 files)
  - Naming: `{ID}_specs.{ext}` (e.g., `DA0000_specs.dfy`, `LE0000_specs.lean`, `VE0000_specs.rs`)
  - Files use `<vc-preamble>`, `<vc-helpers>`, `<vc-spec>`, `<vc-code>` markers to delineate components
  - Code sections contain `assume false` or `sorry` placeholders to be filled in

- `vericoded/` - AI-generated solutions
  - Naming: `{ID}_vericoded.{ext}`
  - Contains completed implementations with helper lemmas and proofs

- `handcoded/` - Human-written solutions (to be added)

- `jsonl/` - Tasks parsed into JSON lines format for experimentation
  - `dafny_tasks.jsonl`, `lean_tasks.jsonl`, `verus_tasks.jsonl` - Task definitions
  - `dafny_issues.jsonl`, `lean_issues.jsonl`, `verus_issues.jsonl` - Known issues

- `vericoding_benchmark_v1.csv` - Metadata for all 12,504 tasks including source, quality scores
- `vericoding_results_v1.csv` - Outcomes of 55,397 experiments across models

## Task ID Prefixes

- `DA` - Dafny tasks
- `LE` - Lean tasks
- `VE` - Verus tasks

## Source Benchmarks

Tasks are derived from:
- DafnyBench, APPS, HumanEval, BigNum (Dafny)
- NumPyTriple, NumPySimple, Verina, FVAPPS, Clever (Lean)
- VerifiedCogen (Verus)

## Working with Spec Files

Spec files are structured with XML-like markers:
```
// <vc-preamble>
// Helper functions, predicates, type definitions
// </vc-preamble>

// <vc-helpers>
// Additional helper lemmas (empty in specs, filled in vericoded)
// </vc-helpers>

// <vc-spec>
// Method/function signature with requires/ensures
// </vc-spec>

// <vc-code>
{
  assume {:axiom} false;  // Placeholder to be replaced with implementation
}
// </vc-code>
```

The goal of a vericoder is to replace the placeholder with a verified implementation that satisfies the specification.
