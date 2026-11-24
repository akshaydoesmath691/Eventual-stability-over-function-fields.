# Counterexamples to the function-field eventual stability conjecture

Akshay Sant (University of Rochester), November 2026.

## The conjecture

Let $K$ be a field, $\varphi \in K(z)$ of degree $\ge 2$, and write $\varphi^n = F_n/G_n$ in lowest terms. For $\alpha \in K$ let $r_n(\varphi,\alpha)$ be the number of irreducible factors of $F_n - \alpha G_n$ in $K[z]$, counted with multiplicity. The pair $(\varphi,\alpha)$ is *eventually stable* if $r_n(\varphi,\alpha)$ is bounded.

**Conjecture (Jones–Levy, *Eventually stable rational functions*, Conj. 1.2).** If $K$ is a function field, $\alpha$ is not periodic under $\varphi$, and the pair $(\varphi,\alpha)$ is not isotrivial, then $(\varphi,\alpha)$ is eventually stable.

Here $(\varphi,\alpha)$ is *isotrivial* if some $\mu \in \mathrm{PGL}_2(\overline K)$ makes both $\mu\varphi\mu^{-1}$ and $\mu(\alpha)$ defined over the algebraic closure of the constant field.

## Counterexamples

**1. Over $\mathbb{F}_2(T)$ (polynomial, exact count).** Let

$$\varphi(z) = z^4 + (T^2+T+1)z^2 + (T^2+T)z, \qquad \alpha = T.$$

Then $\varphi = h\circ g = g \circ h$ with $g = z^2 + Tz$ and $h = z^2 + (T+1)z$ (the Carlitz polynomials $C_T$ and $C_{T+1}$), the orbit of $T$ is $T \mapsto 0 \mapsto 0$, and

$$\varphi^n(z) - T = \bigl(g^n(z) - T\bigr)\prod_{k=0}^{n-1}\bigl(h^k(g^n(z)) + 1\bigr).$$

The $n+1$ factors are irreducible over $\mathbb{F}_2(T)$, of degrees $2^n, 2^n, 2^{n+1}, \dots, 2^{2n-1}$, so $r_n(\varphi, T) = n+1$. The multiplier of $\varphi$ at the fixed point $0$ is $T^2+T$, which is not constant, so the pair is not isotrivial.

**2. Over $\mathbb{F}_p(u)$, every prime $p$ (polynomial).** Let $g = z^p - u^{p-1}z$, $h = g + z$, $\varphi = h \circ g = g\circ h$, $\alpha = u$. Then $u \mapsto 0 \mapsto 0$, the pair $(\varphi,u)$ is not isotrivial (the multiplier at $0$ is $-u^{p-1}(1-u^{p-1})$), and $r_n(\varphi,u) \ge n+1$. For $p = 2$ this is Example 1.

**3. Over $k(t)$, $\operatorname{char} k \ne 2,3$, including $\mathbb{Q}(t)$ and $\mathbb{C}(t)$ (Lattès map).** Let $E : y^2 = x(x-1)(x-t)$ and let $\varphi$ be the degree-36 map with $x\circ[6] = \varphi\circ x$, and $\alpha = 0$. Then $0$ is not periodic ($0 \mapsto \infty \mapsto \infty$), the pair is not isotrivial (the four critical values $\infty, 0, 1, t$ have non-constant cross-ratio $t$), and $F_n$ has at least $n+1$ distinct irreducible factors over $k(t)$.

All three come from a factorisation $\varphi = \theta\circ g$ into commuting maps with $\theta(\alpha) = \alpha$, the mechanism behind the Tucker–Zhu counterexample $(z^6,-1)$ over $\mathbb{Q}$ (arXiv:2610.28688).

## What is verified, and how

| Claim | Status |
|---|---|
| Commuting-factor identity, for arbitrary commuting maps on any commutative ring | **Lean 4 + Mathlib**: `commuting_factor` |
| Ex. 1: $g\circ h = h\circ g$, formula for $\varphi$, orbit $T\mapsto0\mapsto0$, the factorisation for all $n$ | **Lean**: `hC_gC_comm`, `phi_formula`, `orbit_T`, `carlitz_factorisation` |
| Ex. 2: $g\circ h = h\circ g$, orbit $u\mapsto 0\mapsto 0$, the factorisation for all $n$ and all $p$ | **Lean**: `hA_gA_comm`, `orbit_u`, `additive_factorisation` |
| The factors are non-constant, so $r_n \ge n+1$ | Degree count (by hand); not formalised |
| Ex. 1: irreducibility, $r_n = n+1$ exactly | Proof via Hayes' theorem on Carlitz torsion; exact factorisation for $n \le 5$ in `verification/verify_es.py` |
| Non-isotriviality (Ex. 1, 2) | Multiplier argument (by hand); not formalised |
| Ex. 3 | Proof by hand; level-1 and level-2 factorisations over $\mathbb{F}_5(t)$, $\mathbb{F}_7(t)$ in `verification/` |

The Lean statements are proved over an arbitrary commutative ring (of characteristic 2, respectively $p$) and arbitrary elements $x, T, u$. Taking the ring to be $K[z]$ and $x = z$ gives the polynomial identities.

## Reproducing

**Lean** (needs [elan](https://github.com/leanprover/elan)):

```
cd lean
lake exe cache get
lake build
```

**Exact computations** (Python 3, `pip install python-flint`):

```
cd verification
python3 verify_es.py 5 3 3     # factorisations of phi^n - alpha; n = 5 takes ~5 minutes
python3 verify_lattes2.py 7    # Lattès example over F_7(t)
python3 commutant.py 18        # commuting polynomials of given degree (exact decision procedure)
```

The outputs we obtained are in `verification/outputs/`.

## Acknowledgements

The search for these examples, the computations, and the drafting of proofs and code were carried out with the assistance of an AI system (Claude, Anthropic).

## Citation

See `CITATION.cff`.
