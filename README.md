Code for Paper D

Scripts accompanying Y. Kim, Pauli Weight and the Quantum Grandfather Paradox: Neutral Parties and Four-Party Coherence.
Repository: https://github.com/lucifer6542-gif/omega-feedback (code for Papers A, B, and C: https://github.com/lucifer6542-gif/S).

The main theorem (Theorem 10: affine feedback of Pauli weight at most three, all n), its quantitative form (Corollary 13), the three-party theorem (Theorem 31), the five-party counterexample (Theorem 34), and Proposition 30 are proved by hand and need no code. The scripts below provide independent cross-checks of hand arguments, the computer-assisted parts of the four-party theorem (Theorem 32) and of the core-size bound (Proposition B.1), and the statements marked [C] or [N] in the paper.

Each folder is self-contained; run the scripts from inside their folder. Requirements: pip install -r requirements.txt.

01_loop_trace — Proposition 1 (loop trace and postselected CTCs)

	•	pctc.py: simulates the postselected-CTC circuit and compares d²·p_succ with W(P;g) for random boxes and feedbacks, n = 1, 2, 3. Expected: agreement to ~1e-16.

02_lemma_Z — Lemma 12 (at most three targets)

	•	lemmaZ_hand_check.py: checks Steps 5–6 of the hand proof (the forced rows of the P5 and P4+K2 configurations and the violating good sets; all 64 completions of P4+K2).
	•	abstractZ2.py: checks the exclusion of the shapes P3+P3, P3+2K2, 4K2 (Step 4).
	•	satZ.py n 4: SAT check that no affine feedback of weight ≤ 3 on n parties has four designated targets. Expected: UNSAT for n = 4,…,12 (n = 12 in about 20 s).

03_general_feedback — Section "General feedback"

	•	thm16_check.py: Theorem 24 (det V = (det W0)² det K_c) and the identity behind Corollary 26, on random non-affine feedback, n = 2, 3, 4; projector eigenvalue −cot²(π/4) = −1 for GHZ4 at α = π/2.
	•	mc.py: the damped-coupling formula on five-party samples (gen5_s1.json).
	•	lmi2.py FILE n LIMIT: SDP search for the accretive multiplier (Lemma 29), e.g. python3 lmi2.py gen5_s1.json 5 80.
	•	switch_paradox.py: Remark 28 (one process-function branch is not enough). Exact sign change of det V′ at two interior rational points. Expected: sign change: True.
	•	kred.py, ctrl.py, lvl.py, fdet.py: helpers.

04_four_parties — Theorem 31 (three parties, by hand), Theorem 32 (all four-party feedback of weight ≤ 3), Proposition 30

	•	three_party_check.py: cross-check of Theorem 31, which is proved by hand. Expected: 3840 of 4096 three-party feedbacks have a neutral party; the other 256 are relabelings of the NOR loop (64) and the mixed loop (192); det V′ = 1 + t0² + t1² + t2² (NOR) and (1 + t2²)(1 + t0² + t1² + t2²) (mixed).
	•	neutral4.py: neutral parties among the 2,809 four-party classes. Expected: 2,667 with a neutral party (closed by Theorems 11 and 31), 142 without (123 with nonnegative coefficients, 19 mixed-sign).
	•	negative_cycles.py: history digraphs without a negative cycle (Proposition 30). Expected: 851 classes in total, 29 of them without a neutral party.

Pipeline:

	1.	enum4.py, canon4.py: enumerate the 26,023,472 feedbacks of weight ≤ 3 and reduce them to 2,809 classes under the group of order 6144. Output: enum4_classes.npy (included).
	2.	scan4.py: exact integer coefficients of det V′ for every class. Expected: classes 2809 all-positive 2772 all-negative 0 mixed 37.
	3.	n4_close33.py: certifies all 33 non-affine mixed-sign classes, including the 19 that have no neutral party (quadratic-discriminant, square-plus-positive, or two-representation certificates). Expected: total classes: 33 unresolved: 0. The 4 affine mixed classes have a neutral party.
	•	n4proof_check.py: symbolic check of the identities quoted in the text (e.g. the (18,1) and (173,30) identities).
	•	mixed37.py: numerical minima of λ_min for the 37 mixed classes.

05_mechanism — Section "Mechanism and quantitative bounds", Section "Discussion"

	•	satcore2.py n: SAT check that no core of weight ≤ 3 has n parties; n = 7 is the base case of Proposition B.1 (UNSAT in 0.2 s); cls.c (with output cls7.out): independent enumeration of cores up to seven parties (gcc -O2 -o cls cls.c && ./cls 7).
	•	minpi2_n5.py n: exact minimum of λ_min at α = (π/2)ⁿ over all affine feedbacks of weight ≤ 3, n = 4, 5. Expected: 3/2 − √2 for both.
	•	core6_pi2.py: the same for the two six-party cores. Expected: 1 − √6/4.

06_five_party_and_games — Theorem 34 (non-affine counterexample), Section "Loop-avoidance games" (Theorem 38, Proposition 39)

	•	cand1.jsonl: truth tables of the five-party feedback of the counterexample (bit k of the index = o_k).
	•	cert5.py: exact certificate. Interpolates det V′(t0, 1, 1, 3, t4) exactly over ℚ and substitutes t0 = √3. Expected factorization c (t4² + 1)³ (t4² − 5)⁴, i.e. a zero at t4 = √5.
	•	indep_check.py: independent Pauli expansion (11 strings, maximal weight 3) and the direct operator's zero.
	•	verify_g.py [FILE]: Pauli weight and targets of a feedback from its truth tables (default cand1.jsonl). Expected: Pauli weight 3.
	•	kramers_check.py: Remark 35 (fourfold degeneracy at every angle). Symplectic check that each of the 11 Pauli strings of g commutes with S = Y0Y3Y4 and T = Z̃1Z̃2Y3Z̃4 and that S, T anticommute, plus a numerical check at random angles. Expected: all strings commute with S and T; every eigenvalue multiplicity of A_g is a multiple of 4 (exactly 4 at the random angles tested); kernel dimension 4 at the paradox point.
	•	satZgen.py n n 1: SAT search for feedbacks of weight ≤ 3 in which every party is a target, with no constant input and every output read (cores). Finds five-party examples in about 30 s.
	•	ghz4_intrinsic.py: enumerates all general feedbacks with W = 0 for the GHZ4 box (six loops) and shows that no local deterministic box avoids all of them.
	•	loopgame.py: the GHZ4 loop-avoidance game. Expected: quantum value 1, classical value 94/96 = 47/48.
	•	intrinsic5.py, intrinsic_sat.py N|special, intrinsic5b.py, intrinsic_scan.py: the loop-avoidance game of the five-party box for kernel states (generic: 296 perfect classical strategies; maximal-zero states: 63 or 64). intrinsic_sat.py special needs psi_special.npy, which intrinsic5b.py or intrinsic_scan.py writes; run one of them first.
