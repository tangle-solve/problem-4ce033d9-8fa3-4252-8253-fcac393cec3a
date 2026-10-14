<!-- solve-problem:4ce033d9-8fa3-4252-8253-fcac393cec3a -->
# Polynomial-time planted-clique recovery below square root

A [Tangle Solve problem](https://solve.tangle.tools/problems/4ce033d9-8fa3-4252-8253-fcac393cec3a).

## What is the problem?

Fix \(0 < \varepsilon < 1/2\). Generate a random graph on \(n\) vertices by sampling \(G \sim G(n,1/2)\), choosing a uniformly random subset \(K\) of \(k = n^{1/2 - \varepsilon}\) vertices, and adding all edges among \(K\). The task: **find a planted clique of size \(k\)** (or even \(K\) itself, or a clique of comparable size) in \(\mathrm{poly}(n)\) time, with probability \(1 - o(1)\) over the planting.

Known algorithms (e.g., Alon–Krivelevich–Sudakov spectral/enumera-tive methods) recover the clique w.h.p. whenever \(k \ge C\sqrt{n\log n}\) — and this is tight for those methods. The information-theoretic threshold is much lower: recovery is information-theoretically possible (indeed easy non-computationally) once \(k \gg \log n\), and detection is possible once \(k \gg 2\log n\). The open problem is to close, or provably obstruct, the computational gap between \(\sqrt{n}\) and \(\log n\). Ambiguity note: "recovery" could mean (a) output \(K\) exactly, (b) output a clique of size \(\ge k(1-o(1))\), or (c) output a clique overlapping \(K\) substantially. All are open for \(k = n^{1/2-\varepsilon}\); the standard target is (a) or (b).

## Why does it matter?

The planted clique problem is the canonical laboratory for statistical–computational gaps: reductions from it calibrate the assumed hardness of sparse PCA, community detection below the Kesten–Stigum threshold, cryptosystems (e.g., geodesic encryption / Jadu-safe schemes), and robust statistics. An algorithm below \(\sqrt{n}\) would collapse a web of average-case hardness assumptions and would likely transfer to these inference problems; a proof of hardness (even against a strong model like SoS, which is nearly in hand) certifies the gap. Either resolution reshapes how we formalize average-case complexity.

## What's already known?

- **Upper bounds:** Alon, Krivelevich, Sudakov (1998) give poly-time recovery for \(k \ge \Omega(\sqrt{n\log n})\), via degree counting combined with spectral or greedy peeling; later refinements (Deshpande–Montanari, via approximate message passing / tensor methods) recover for \(k = \Omega(\sqrt{n})\) — the constant doesn't matter, the exponent does. No known poly-time algorithm beats the \(\sqrt{n}\) exponent.
- **Lower bounds (restricted models):** the planted clique decision problem lies in \( \mathrm{SZK}\) intersection-style classes in ways suggesting it's not NP-hard under standard reductions (verify details); Feige–Krauthgamer showed Lovász–Schrijver SDP hierarchy fails for \(k \ll \sqrt{n}\); Deshpande–Montanari, Hopkins–Steier, and most strongly **Barak et al. (2016)** show degree-\(d\) sum-of-squares relaxations fail for \(k \ll n^{1/(d+1)}\)-type regimes, with the near-tight statement that polylog-degree SoS cannot beat \(\sqrt{n}\) up to polylog factors. MP-based and low-degree polynomial methods (Hopkins et al.) similarly stall at \(\sqrt{n}\).
- **Evidence for hardness:** low-degree-likelihood-ratio and SoS lower bounds are widely taken as strong evidence that no poly-time algorithm exists at \(n^{1/2-\varepsilon}\), but no unconditional barrier against all poly-time algorithms is known, nor a proof separating the statistical and computational thresholds.

## What would count as solving it?

Two complementary resolutions:

1. **Algorithmic (upper bound):** an explicit randomized algorithm and a proof that on the planted distribution with \(k = n^{1/2-\varepsilon}\) for some fixed \(\varepsilon > 0\), it outputs \(K\) (or a clique of size \((1-o(1))k\)) with probability \(1-o(1)\), in time \(n^{O(1)}\). A referee would require a rigorous average-case guarantee, not empirical performance.
2. **Lower bound:** a proof that no poly-time algorithm succeeds at \(k = n^{1/2-\varepsilon}\) — unconditional (currently out of reach, as it implies average-case circuit lower bounds) or against a broad enough class (e.g., all constant-degree SoS plus reductions) that it is accepted as settling the gap. Intermediate wins: extending SoS lower bounds to unbounded degree below polylog, or a reduction showing any poly-time algorithm for planted clique below \(\sqrt{n}\) yields one for another widely believed hard problem.

## How to start

1. Read Alon–Krivelevich–Sudakov (1998), Sections 2–3, to see exactly where the degree-counting + spectral argument requires \(k \gtrsim \sqrt{n}\); the failure mode is that below \(\sqrt{n}\), planted vertices are statistically indistinguishable one at a time — all signal is in higher-order correlations.
2. Read Barak et al. (2016) for the pseudo-calibration technique showing degree-\(d\) SoS achieves nothing below \(n^{1/2}\) (up to polylogs): this tells you what candidate algorithms must avoid.
3. Natural attack directions: (a) combine partial spectral information iteratively with cleanup (analyze sample splitting); (b) use tensor/AMP methods and check whether \(k \sim n^{1/2-\varepsilon}\) with \(O(1)\) iterations could succeed (known analyses say no — verify where they fail); (c) exploit non-spectral statistics of the planted graph (e.g., counts of small subgraphs beyond edges). The main obstacle: every known method's signal-to-noise analysis exhibits a \(\sqrt{n}\) phase transition tied to eigenvalue norms of \(G(n,1/2)\) adjacency matrices.
4. If pursuing hardness, study pseudo-calibration and the low-degree survey of Hopkins (thesis, 2018) — extending beyond degree polylog is the frontier.

## Key references

- N. Alon, M. Krivelevich, B. Sudakov, "Finding a Large Hidden Clique in a Random Graph," *Random Structures & Algorithms* 13 (1998), 457–470. Gives the poly-time algorithm recovering planted cliques of size \(\Omega(\sqrt{n \log n})\); still the best known poly-time exponent. https://doi.org/10.1002/(SICI)1098-2418(199810/12)13:3/4<457::AID-RSA14>3.0.CO;2-W
- B. Barak, S. B. Hopkins, J. Kelner, P. K. Kothari, A. Moitra, A. Potechin, "A Nearly Tight Sum-of-Squares Lower Bound for the Planted Clique Problem," FOCS 2016, arXiv:1604.03084. Uses pseudo-calibration to show degree-\(d\) SoS relaxations fail for \(k \ll n^{1/2}\) (up to polylog factors), the strongest known hardness evidence. https://arxiv.org/abs/1604.03084
- Y. Deshpande, A. Montanari, "Finding Hidden Cliques of Size \(\sqrt{N/e}\) in Nearly Linear Time," *Random Structures & Algorithms* (2015). AMP/spectral algorithm matching the \(\sqrt{n}\) barrier with a nearly linear running time. (verify citation details)
- S. B. Hopkins, thesis, "Statistical Inference and the Sum-of-Squares Method," Harvard 2018. Develops the low-degree polynomial framework establishing \(\sqrt{n}\) as a barrier for broad algorithm classes. https://arxiv.org/abs/1611.03147 (related version; verify)

## Warnings

- The problem is widely conjectured **impossible** for poly-time algorithms; the "attack" may be evidence-gathering rather than an algorithm. Don't over-invest in naive spectral/greedy variants — these provably stall at \(\sqrt{n}\).
- Distinguish detection (STAT-000001, related problem: does \(G\) contain a planted clique?) from recovery (here: find it); detection can be easier via non-constructive statistics.
- "Below \(\sqrt{n}\)" requires \(k \ge C\log n\) at minimum, or even detection is information-theoretically impossible.
- Empirical success on simulations does not count; many heuristics (e.g., simulated annealing, message passing variants) appear to work and fail under analysis.

---
*`ALGO-000146` · unverified*

## Acceptance guidance

Two complementary resolutions:

1. **Algorithmic (upper bound):** an explicit randomized algorithm and a proof that on the planted distribution with \(k = n^{1/2-\varepsilon}\) for some fixed \(\varepsilon > 0\), it outputs \(K\) (or a clique of size \((1-o(1))k\)) with probability \(1-o(1)\), in time \(n^{O(1)}\). A referee would require a rigorous average-case guarantee, not empirical performance.
2. **Lower bound:** a proof that no poly-time algorithm succeeds at \(k = n^{1/2-\varepsilon}\) — unconditional (currently out of reach, as it implies average-case circuit lower bounds) or against a broad enough class (e.g., all constant-degree SoS plus reductions) that it is accepted as settling the gap. Intermediate wins: extending SoS lower bounds to unbounded degree below polylog, or a reduction showing any poly-time algorithm for planted clique below \(\sqrt{n}\) yields one for another widely believed hard problem.

## Contribute

Contributions, audits, and critiques from humans, agents, or both are welcome. Open a pull request with your work, then [publish the exact revision or artifacts on Solve](https://solve.tangle.tools/problems/4ce033d9-8fa3-4252-8253-fcac393cec3a?contribute=1). Include reviewed agent traces when available. Contributions need not solve the entire problem.

Keep experiment code, reports, and trace artifacts in this repository. Publish only material you have reviewed for public sharing. Solve retains the contribution snapshot and its review history independently of whether a pull request is merged.

Source record: ALGO-000146.

- [Public source catalog](https://drewstone.github.io/problems/#/p/ALGO-000146)
