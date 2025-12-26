# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This is a benchmark dataset for **Vericoding** - formally verified program synthesis. It contains 12,504 verification tasks in Dafny, Lean, and Verus, along with experiment results from AI vericoders.

Paper: https://www.arxiv.org/abs/2509.22908
Scripts: https://github.com/beneficial-AI-Foundation/vericoding

## Repository Structure

- `specs/` - Specification files with tasks to be solved (12,504 files)
- `vericoded/` - AI-generated solutions with implementations
- `handcoded/` - Human-written solutions (to be added)
- `jsonl/` - Tasks parsed into JSON lines format for experimentation
- `vericoding_benchmark_v1.csv` - Metadata for all tasks (source, quality scores)
- `vericoding_results_v1.csv` - Outcomes of 55,397 experiments across models

## Task ID Prefixes

- `DA` - Dafny tasks (e.g., `DA0000_specs.dfy`)
- `LA` - Lean tasks (e.g., `LA0000_specs.lean`)
- `VA` - Verus tasks (e.g., `VA0000_specs.rs`)

## File Naming Convention

- Specs: `{ID}_specs.{ext}`
- Solutions: `{ID}_vericoded.{ext}`

## Spec File Format by Language

### Dafny (`.dfy`)
```dafny
// <vc-preamble>
// Helper functions, predicates, type definitions
// </vc-preamble>

// <vc-helpers>
// Additional helper lemmas (empty in specs, filled in vericoded)
// </vc-helpers>

// <vc-spec>
method foo(...) returns (...)
  requires ...
  ensures ...
// </vc-spec>
// <vc-code>
{
  assume {:axiom} false;  // Placeholder
}
// </vc-code>
```

### Lean (`.lean`)
```lean
-- <vc-preamble>
-- Helper definitions and predicates
-- </vc-preamble>

-- <vc-helpers>
-- </vc-helpers>

-- <vc-definitions>
def solve (...) : ... :=
  sorry  -- Placeholder
-- </vc-definitions>

-- <vc-theorems>
theorem solve_spec_satisfied (...) : ... := by
  sorry  -- Placeholder
-- </vc-theorems>
```

### Verus (`.rs`)
```rust
// <vc-preamble>
use vstd::prelude::*;
verus! {
// spec functions
// </vc-preamble>

// <vc-helpers>
// </vc-helpers>

// <vc-spec>
fn solve(...) -> (...)
  requires ...
  ensures ...
// </vc-spec>
// <vc-code>
{
  assume(false);
  unreached()  // Placeholder
}
// </vc-code>
}
```

## JSONL Schema

Each task in `jsonl/*_tasks.jsonl` contains:
- `id`, `language`, `source`, `source-id`
- `vc-description` - Natural language description (when available)
- `vc-preamble`, `vc-helpers`, `vc-spec`, `vc-code`, `vc-postamble` - Code sections
- `qa-*` - Quality analysis fields (issue flags, scores)

## Source Benchmarks

Tasks are derived from:
- **Dafny**: DafnyBench, APPS, HumanEval, BigNum
- **Lean**: NumPyTriple, NumPySimple, Verina, FVAPPS, Clever
- **Verus**: VerifiedCogen

## Development

Uses `uv` for Python (3.12+) and `pnpm` for Node.js dependencies.
