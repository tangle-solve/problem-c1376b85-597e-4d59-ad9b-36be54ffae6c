<!-- solve-problem:c1376b85-597e-4d59-ad9b-36be54ffae6c -->
# Single-exponential MWIS by clique-width without an expression

A [Tangle Solve problem](https://solve.tangle.tools/problems/c1376b85-597e-4d59-ad9b-36be54ffae6c).

## What is the problem?

**Maximum Weight Independent Set (MWIS)**: given a graph G on n vertices with vertex weights, find a maximum-weight set of pairwise non-adjacent vertices. The parameter is the **clique-width** cw(G) ≤ k: the minimum number of labels needed to construct G using the operations (i) create a vertex with label i, (ii) disjoint union, (iii) connect all vertices of label i to all of label j, (iv) relabel i→j (a *k-expression*).

If a k-expression of length O(n) is **given**, a standard dynamic program over the expression tree solves MWIS in 2^{O(k)}·n^{O(1)} time (one DP state per subset of the k labels that could be "occupied" by the independent set). The open problem: achieve **2^{O(k)}·n^{O(1)} time given only the graph**, with cw(G) ≤ k as the parameter, i.e., the algorithm must simultaneously discover the structure and exploit it without a log factor in the exponent. The output is just the optimum (or witness set); correctness and running time must hold for all graphs of clique-width ≤ k even though no expression is supplied.

The subtlety: the best known route to *find* a decomposition uses **rank-width** rw(G) ≤ cw(G), computable in f(w)·n^{O(1)} time (Oum, based on Oum–Seymour), but converting a rank-decomposition of width w into a clique-width expression costs width up to 2^{w+1}−1, blowing up the exponent to roughly 2^{O(k log k)} — this is exactly the gap to close.

## Why does it matter?

Clique-width generalizes treewidth (cw ≤ t+2 for treewidth t), so a positive answer would give single-exponential algorithms on dense structured graph classes (distance-hereditary, some power graphs, graphs of bounded rank-width) where treewidth is unbounded. It matters practically because decomposition tools are rarely available in real inputs — assuming a supplied expression is unrealistic. A negative answer or a proven barrier would clarify whether clique-width is fundamentally "harder" than treewidth for single-exponential DP. It also bears on the broader meta-question of which width parameters admit self-reducible, single-exponential algorithms.

## What's already known?

- **With an expression:** MWIS in 2^{O(k)}·n^{O(1)} by DP over the k-expression tree (standard; cf. Courcelle–Olariu for bounded cw ⇒ bounded something else; see also Bui-Xuan, Telle, Vatshelle's Boolean-width framework, which gives 2^{O(2^d)}-type bounds where d is Boolean-width).
- **Without an expression:** Oum–Seymour showed cw and rank-width are within a function of each other (rw ≤ cw ≤ 2^{rw+1}−1) and rank-decompositions of width w are computable in f(w)·n^{O(1)}. Combining yields roughly 2^{O(k log k)}·n^{O(1)} for MSO-expressible problems, including MWIS.
- The cited benchmark, Oum–Sæther–Vatshelle (arXiv:1311.0224), improves vertex-partitioning problems (including MWIS-type problems) parameterized by clique-width k to 2^{O(k log k)}·n^{O(1)} — this is the state of the art and the gap: the log factor survives every known approach.
- Obstacle: all known pipelines route through a rank-width/Boolean-width decomposition and pay an exponential (hence log-in-exponent) conversion to labels or pay for many label classes per DP state. No known technique extracts "label-like" single-exponential structure directly from the graph. Compare: for treewidth, Bodlaender's linear-time exact-decomposition theorem closes the analogous gap; nothing similar is known for clique-width — even computing cw exactly is NP-hard and the best recognition/approximation carries large blowups (verify current best approximation factors).

## What would count as solving it?

1. **Positive (main target):** a deterministic or randomized algorithm that, given only G and weights, with promise cw(G) ≤ k, computes MWIS in 2^{O(k)}·n^{O(1)} time. A referee accepts a full correctness proof and running-time analysis; the expression need never be output.
2. **Partial:** single-exponential time parameterized by rank-width w for MWIS (this would essentially resolve the question since rw ≤ cw).
3. **Negative/barrier:** evidence (e.g., a reduction, or a lower bound under ETH/SETH matching 2^{Ω(k log k)}) that single-exponential is impossible without the expression — this would also be a major result.

## How to start

1. Read Oum–Sæther–Vatshelle (arXiv:1311.0224) to see precisely where the k log k arises: count the DP states (label subsets / neighborhood classes) and identify the step whose state space is super-exponential in k.
2. Study the Boolean-width framework (Bui-Xuan–Telle–Vatshelle) and Oum's rank-width papers; ask whether MWIS DP can be reformulated over rank-decompositions with 2^{O(w)} states by exploiting that independent-set constraints are cut functions of low rank over GF(2) — the d = O(rw) Boolean dimension suggests the information content is only single-exponential.
3. Investigate whether a "local" or "amortized" expression can be built lazily during the DP, paying only for labels actually needed at the current cut — the natural fix for the 2^{rw+1} conversion blowup.
4. Main obstacle: the label-conversion lemma is tight as stated (complete graphs etc.), so any solution must bypass expressions entirely rather than improve the conversion.

## Key references

- S.-I. Oum and P. Seymour, *Approximating clique-width and branch-decomposition*, J. Combin. Theory B 96 (2006) 514–528. — Establishes rw ≤ cw ≤ 2^{rw+1}−1 and f(w)n^{O(1)} computation of rank-decompositions; the source of the log-factor gap.
- S.-I. Oum, *Rank-width and vertex-minors*, J. Combin. Theory B 95 (2005) 79–100. — Foundational theory of rank-width and its computation. (verify exact venue/pages)
- S.-I. Oum, M. Sæther, and M. Vatshelle, *Faster algorithms for vertex partitioning problems parameterized by clique-width*, arXiv:1311.0224. — The benchmark 2^{O(k log k)}-time algorithm for vertex partitioning problems, including the MWIS-type problems, with and without a supplied expression.
- B.-M. Bui-Xuan, J. A. Telle, M. Vatshelle, *Boolean-width of graphs*, Theor. Comput. Sci. 412 (2011). — DP framework where state counts depend on Boolean dimension of cuts; relevant alternative decomposition basis. (verify)
- H. L. Bodlaender, *A linear-time algorithm for finding tree-decompositions of small treewidth*, SIAM J. Comput. 25 (1996). — The treewidth analogue that was solved via self-reduction; a conceptual template.

## Warnings

- Do not re-derive the "given expression" DP — it is folklore and correct; the entire difficulty is *finding* usable structure within a single-exponential budget.
- The bound cw ≤ 2^{rw+1}−1 is essentially tight, so "compute rank-width then convert" cannot be patched; the log factor is inherent to that pipeline.
- Ambiguity in the problem statement: it is a promise problem (cw(G) ≤ k assumed). Decide up front whether your algorithm must also handle graphs with cw > k gracefully (e.g., reject or run in the 2^{O(k log k)} fallback).
- Parameter is clique-width, not treewidth; results for treewidth do not transfer, and bounded cw does not imply bounded tw.
- Computing clique-width exactly is NP-hard (Fellows et al.), so exact recognition is off the table.

---
*`ALGO-000098` · unverified*

## Acceptance guidance

1. **Positive (main target):** a deterministic or randomized algorithm that, given only G and weights, with promise cw(G) ≤ k, computes MWIS in 2^{O(k)}·n^{O(1)} time. A referee accepts a full correctness proof and running-time analysis; the expression need never be output.
2. **Partial:** single-exponential time parameterized by rank-width w for MWIS (this would essentially resolve the question since rw ≤ cw).
3. **Negative/barrier:** evidence (e.g., a reduction, or a lower bound under ETH/SETH matching 2^{Ω(k log k)}) that single-exponential is impossible without the expression — this would also be a major result.

## Contribute

Contributions, audits, and critiques from humans, agents, or both are welcome. Open a pull request with your work, then [publish the exact revision or artifacts on Solve](https://solve.tangle.tools/problems/c1376b85-597e-4d59-ad9b-36be54ffae6c?contribute=1). Include reviewed agent traces when available. Contributions need not solve the entire problem.

Keep experiment code, reports, and trace artifacts in this repository. Publish only material you have reviewed for public sharing. Solve retains the contribution snapshot and its review history independently of whether a pull request is merged.

Source record: ALGO-000098.

- [Public source catalog](https://drewstone.github.io/problems/#/p/ALGO-000098)
