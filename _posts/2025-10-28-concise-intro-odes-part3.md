---
layout: distill
output:
  distill::distill_article:
    toc: true
    toc_depth: 2
title: Reconstructing the Scaffolding A Learner’s Notes on Ordinary Differential Equations
description: Part &#8546 Understanding the Dahlquist Equivalence Theorem for Linear Multistep Methods
tags:
giscus_comments: true
date: 2025-10-28
featured: true

authors:
  - name: Xin Cai
    url: "https://totalvariation.github.io/"

bibliography: 2025-10-28-concise-intro-odes-part3.bib

# Optionally, you can add a table of contents to your post.
# NOTES:
#   - make sure that TOC names match the actual section names
#     for hyperlinks within the post to work correctly.
#   - we may want to automate TOC generation in the future using
#     jekyll-toc plugin (https://github.com/toshimaru/jekyll-toc).
#toc:
#  - name: Equations
#    # if a section has subsections, you can add them as follows:
#    # subsections:
#    #   - name: Example Child Subsection 1
#    #   - name: Example Child Subsection 2
#  - name: Citations
#  - name: Footnotes
#  - name: Code Blocks
#  - name: Interactive Plots
#  - name: Layouts
#  - name: Other Typography?

toc:
  - name: Consistency and Zero-Stability
  - name: Dahlquist Equivalence Theorem for Linear Multistep Methods
  - name: Concluding Remarks
  - name: Acknowledgement

# Below is an example of injecting additional post-specific styles.
# If you use this post as a template, delete this _styles block.
_styles: >
  .fake-img {
    background: #bbb;
    border: 1px solid rgba(0, 0, 0, 0.1);
    box-shadow: 0 0px 4px rgba(0, 0, 0, 0.1);
    margin-bottom: 12px;
  }
  .fake-img p {
    font-family: monospace;
    color: white;
    text-align: left;
    margin: 12px 0;
    text-align: center;
    font-size: 16px;
  }
  .theorem {
    display: block;
    font-style: italic;
  }
  .theorem:before {
    content: "Theorem. ";
    font-weight: bold;
    font-style: normal;
  }
  .theorem[text]:before {
    content: "Theorem (" attr(text) ") ";
  }
---

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include video.liquid path="assets/video/traj-lmm.mp4" class="img-fluid rounded z-depth-1" controls=true autoplay=true %}
    </div>
</div>
<div class="caption">
    Animated Trajectories of Linear Multistep Methods on Lotka-Volterra Equations
</div>

<p>
This post is part of my ongoing series of learning notes on Ordinary Differential Equations (ODEs). In this instalment, I will present detailed mathematical proofs of the Dahlquist Equivalence Theorem for Linear Multistep Methods (LMMs), which establishes that Convergence $ \iff $ Consistency + Zero-Stability.
</p>

<p>
Throughout this blog, we focus on the initial value problems (IVPs) for one-dimensional ODEs for notational simplicity

$$
y'(t) = f(t, y(t)), \; y(t_0) = y_0, \; t \in [t_0, T]
$$

with $ f $ a sufficiently smooth (as specified when needed) scalar function.
</p>

<h2> Consistency and Zero-Stability </h2>

<p>
A $ k $-step linear multistep method (LMM) has the form 

$$
\sum_{j=0}^k \alpha_j y_{n+j} = h \sum_{j=0}^k \beta_j f_{n+j}, \alpha_k = 1, n \geq 0.
$$

</p>

<p>Define the generating polynomials $ \rho(\zeta) = \sum_{j=0}^k \alpha_j \zeta^j $, $ \sigma(\zeta) = \sum_{j=0}^k \beta_j \zeta^j $.</p>

<h3>Consistency</h3>

<p>
Define the local truncation error (LTE) of an LMM by substituting the exact solution $ y(t) $ into the scheme:

$$
\tau_{n+k} := \sum_{j=0}^k \alpha_j y(t_{n+j}) - h \sum_{j=0}^k \beta_j y'(t_{n+j})
$$

The method has order $ p $ if $ \tau_{n+k} = \mathcal{O}(h^{p+1}) $ for smooth $ y(t) $.
</p>

<p>
Taylor expand about $ t_n $:

$$
\begin{align*}
y(t_{n+j}) &= y(t_n) + jh \cdot y'(t_n) + \frac{(jh)^2}{2!} y''(t_n) + \cdots + \frac{(jh)^p}{p!}y^{(p)}(t_n) + \mathcal{O} (h^{p+1}) \\
y'(t_{n+j}) &=  y'(t_n) + jh \cdot y''(t_n) + \frac{(jh)^2}{2!} y'''(t_n) + \cdots + \frac{(jh)^{p-1}}{(p-1)!}y^{(p)}(t_n) + \mathcal{O} (h^{p+1})
\end{align*}
$$

</p>

<p>
Insert into $ \tau_{n+k} $, collect powers of $ h $, and require the coefficients of $ y^{(\ell)}(t_n) $ to vanish for $ \ell = 0, 1, \ldots, p $.

$$
\tau_{n+k} = C_0 y(t_n) + C_1 h y'(t_n) + \cdots + C_p h^p y^{(p)}(t_n) + \mathcal{O}(h^{p+1})
$$

$$
\begin{align*}
C_0 &: \sum_{j=0}^k \alpha_j = \rho(1) = 0. \\
C_1 &: \sum_{j=0}^k j \alpha_j = \sum_{j=0}^k \beta_j, \iff \rho'(1) = \sigma(1).  
\end{align*}
$$

</p>

<p>
In general, the coefficient of $ h^{\ell}y^{(\ell)}(t_n) $ for $ \ell \geq 1 $ is

$$
\frac{1}{\ell !} \sum_{j=0}^k \alpha_j j^{\ell} - \frac{1}{(\ell - 1) !} \sum_{j=0}^k \beta_j j^{\ell - 1}.
$$

For order $ p $, these must vanish for $ \ell = 1, 2, \ldots, p $:

$$
\sum_{j=0}^k \alpha_j j^{\ell} = \ell \sum_{j=0}^k \beta_j j^{\ell - 1}, \; \ell = 0, 1, \ldots, p,
$$

An LMM is consistent if $ C_0 = C_1 = 0 $, i.e., $ \rho(1) = 0 $ and $ \rho'(1) = \sigma(1) $, which means $ \frac{1}{h} \tau_{n+k} = \mathcal{O}(h) $.

</p>

<h3>Zero-Stability</h3>

<p>
Consider the homogeneous recurrence with a perturbation:

$$
\sum_{j=0}^k \alpha_j w_{n+j} = \delta_{n+k}, \; n \geq 0,
$$

with given starting values $ w_0, w_1, \ldots, w_{k-1} $.
</p>

<p>
The LMM is zero-stable if there exists a constant $ C $ independent of $ h $ such that 

$$
\max_{0 \leq n \leq N} |w_n| \leq C \left(\max_{0 \leq j \leq k-1} |w_j| + \max_{k \leq m \leq N} |\delta_m|  \right)
$$

</p>

<p><strong>The root condition</strong></p> 

<p>
Let $ \rho(\zeta) = \sum_{j=0}^k \alpha_j \zeta^j $. The root condition is: All roots of $ \rho $ satisfy $ |\zeta| \le 1 $, and any root with $ |\zeta| = 1 $ is simple (i.e., algebraic multiplicity is equal to 1).
</p>

<p><strong>Theorem</strong> An LMM is zero-stable $ \iff $ its $ \rho(\zeta) $ satisfies the root condition.</p>

<p>Assume the root condition holds. Consider the homogeneous recurrence $ \sum_{j=0}^k \alpha_j w_{n+j} = 0, \; \alpha_k = 1 $.</p>

<p>
Introduce the companion matrix

$$
\mathbf{C} = \begin{pmatrix} 0 & 1 & \cdots & 0 & 0 \\ 0 & 0 & 1 & \cdots & 0 \\ \vdots & \vdots & \ddots & \ddots &\vdots \\ 0 & 0 & \cdots & 0 & 1 \\ - \alpha_0 & - \alpha_1 & \cdots & - \alpha_{k-2} & -\alpha_{k-1} \end{pmatrix}_{k \times k}
$$

and 

$$
\mathbf{w}_n = \begin{pmatrix} w_{n} \\ \vdots \\ w_{n+k-2} \\ w_{n+k-1}  \end{pmatrix}
$$

$$
\mathbf{w}_{n+1} = \mathbf{C} \mathbf{w}_n
$$

The eigenvalues of $ \mathbf{C} $ are exactly the roots of $ \rho $.
</p>

<p><strong>Companion Matrix of a Polynomial</strong></p>

<p>
For each monic polynomial $$ p(x) = x^n + \alpha_{n-1}x^{n-1} + \cdots + \alpha_1 x + \alpha_0 $$, the companion matrix $$ p(x) $$ is defined to be

$$
\mathbf{C} = \begin{pmatrix} 0 & 0 & \cdots & 0 & -\alpha_0 \\ 1 & 0 & \cdots & 0 & -\alpha_1 \\ \vdots & \ddots & \ddots & &\vdots \\ 0 & \cdots & 1 & 0 & -\alpha_{n-2} \\ 0 & 0 & \cdots & 1 & -\alpha_{n-1} \end{pmatrix}_{n \times n}
$$

</p>

<p>
The polynomial $ p(x) $ is both the characteristic and minimum polynomial for $ \mathbf{C} $ (i.e., $ \mathbf{C} $ is nonderogatory), which implies $ \text{geo multi}(\lambda_j) = 1 $ for each $ \lambda_j $, or, equivalently, $ \text{alg mult}(\lambda_j) = \text{index}(\lambda_j) $ for each $ \lambda_j \in \sigma(\mathbf{C}) $. 
</p>

{% details Click here to know more %}
<p><strong>Proof</strong><d-footnote>The proof is borrowed from the book Matrix Analysis and Applied Linear Algebra <d-cite key="meyer2023matrix"></d-cite>. Please refer to the 7.11 for more details.</d-footnote>:</p>

<p>
Write $ \mathbf{C} = \mathbf{N} - \mathbf{c}\mathbf{e}^T_n $, where 

$$
\mathbf{N} = \begin{pmatrix} 0 & & & \\ 1 & \ddots & & \\ & \ddots & \ddots & \\ & & 1 & 0  \end{pmatrix}
$$

and

$$
\mathbf{c} = \begin{pmatrix} \alpha_0 \\ \alpha_1 \\ \vdots \\ \alpha_{n-1}  \end{pmatrix}
$$

</p>

<p>

$$
\begin{align*}
\det (x\mathbf{I} - \mathbf{C}) &= \det(x\mathbf{I} - \mathbf{N} + \mathbf{c}\mathbf{e}^T_n) \\
	&= \det \left((x\mathbf{I} - \mathbf{N})(\mathbf{I} - (x\mathbf{I} - \mathbf{N})^{-1}\mathbf{c}\mathbf{e}^T_n)  \right) \\
	&= \det(x\mathbf{I} - \mathbf{N}) \left(1 + \mathbf{e}_n^T(x\mathbf{I} - \mathbf{N})^{-1}\mathbf{c} \right)\\
	&= x^n \left( 1 + \mathbf{e}^T_n\left(\frac{\mathbf{I}}{x} + \frac{\mathbf{N}}{x^2} + \frac{\mathbf{N}^2}{x^3} + \cdots + \frac{\mathbf{N}^{n-1}}{x^n}  \right)\mathbf{c} \right) \\
	&= x^n + \alpha_{n-1} x^{n-1} + \alpha_{n-2} x^{n-2} + \cdots + \alpha_0 = p(x) 
\end{align*}
$$

</p>

<p>
Set $ \mathcal{B} = \{ \mathbf{e}_1, \mathbf{e}_2, \ldots, \mathbf{e}_n \} $, and let $ v_i(x) = x^k - \sum_{j=0}^{k-1} \alpha_j x^j $ be the minimum polynomial of $ \mathbf{e}_i $ w.r.t. $ \mathbf{C} $, i.e., $ v(\mathbf{A})\mathbf{e}_i = \mathbf{0} $. Observe that $ \mathbf{C}\mathbf{e}_j = \mathbf{e}_{j+1} $ for $ j = 1, \ldots, n-1 $, so

$$
\{ \mathbf{e}_1, \mathbf{C}\mathbf{e}_1, \mathbf{C}^2 \mathbf{e}_1, \ldots, \mathbf{C}^{n-1} \mathbf{e}_1 \} = \{ \mathbf{e}_1, \mathbf{e}_2, \mathbf{e}_3, \ldots, \mathbf{e}_n \}
$$

and 

$$
\mathbf{C}^n \mathbf{e}_1 = \mathbf{C} \mathbf{e}_n = \mathbf{C}_{\star n}  = - \sum_{j=0}^{n-1} \alpha_j \mathbf{e}_{j+1} = - \sum_{j=0}^{n-1} \alpha_j \mathbf{C}^j \mathbf{e}_j \implies v_1(x) = p(x)
$$

</p>

<p>
Since $ v_1(x) $ divides the least common multiple polynomial of all $ v_i(x) $'s, which is known to be the minimum polynomial $ m(x) $ of $ \mathbf{C} $, we conclude that $ p(x) $ divides $ m(x) $. Given $ m(x) $ always divides $ p(x) $, so $ m(x) = p(x) $. 
</p>
{% enddetails %}

<p><strong>Proof of the sufficiency half:</strong></p>

<p>
By the root condition, every eigenvalue $ \lambda $ satisfies $ |\lambda| \le 1 $, with the Jordan block corresponding to $ | \lambda | = 1 $ being of size $ 1 \times 1 $ as $ \text{alg mult}(\lambda) = \text{index}(\lambda) $ for each $ \lambda \in \mathbf{C} $. Therefore, $ \mathbf{C} $ is power-bounded:

$$
\exists M \; \text{s.t.}\; \| \mathbf{C} \| \leq M \; \forall n \geq 0
$$

</p>

{% details Click here to know more %}
<p><strong>Sketch of proof:</strong></p>

<p>
Converting $ \mathbf{C} = \mathbf{S} \mathbf{J} \mathbf{S}^{-1} $ by similarity transformation to its Jordan form. Blocks with $ | \lambda | < 1 $ decay to zero as $ n \to \infty $; 

$$
\mathbf{J}_{\star}^k = \begin{pmatrix} \lambda & 1 & & \\ & \ddots & \ddots &\\ & & \ddots & 1\\ & & & \lambda \end{pmatrix}_{m \times m}^k = \begin{pmatrix} \lambda^k & \begin{pmatrix} k \\ 1 \end{pmatrix} \lambda^{k-1} & \begin{pmatrix} k \\ 2 \end{pmatrix} \lambda^{k-2} & \cdots & \begin{pmatrix} k \\ m-1 \end{pmatrix} \lambda^{k-m+1} \\  & \lambda^k & \begin{pmatrix} k \\ 1 \end{pmatrix} \lambda^{k-1} & \ddots & \vdots \\  &  & \ddots & \ddots & \begin{pmatrix} k \\ 2 \end{pmatrix} \lambda^{k-2} \\  &  &  & \lambda^k & \begin{pmatrix} k \\ 1 \end{pmatrix} \lambda^{k-1} \\  &  &  &  & \lambda^k  \end{pmatrix}_{m \times m}
$$

It is clear that if $ |\lambda| < 1 $, $ \lim_{k \to \infty} \lambda^k = 0 $ and $ \lim_{k \to \infty} \begin{pmatrix} k\\ j  \end{pmatrix} \lambda^{k-j} = 0 $ for each fixed value of $ j $.
</p>
 
<p>
The block with $ |\lambda| = 1 $ is $ \left[ 1 \right]_{1 \times 1} $. Thus, $ \| \mathbf{C}^n \| \le \| \mathbf{S} \| \| \mathbf{S}^{-1} \| $ (e.g., $ \| \mathbf{C}^n \|_2 \le \frac{\sigma_1}{\sigma_n} $, where $ \sigma_1 $ and $ \sigma_n $ are the largest and smallest singular values of $ \mathbf{S} $ respectively, and matrix norms are equivalent) uniformly in $ n $.

</p>
{% enddetails %}

<p>
For the perturbed recurrence $ \sum_{j=0}^k \alpha_j w_{n+j} = \delta_{n+k}, \; n \geq 0 $, reformulate it as the matrix-vector form:
 
$$
\mathbf{w}_{n+1} = \mathbf{C} \mathbf{w}_n + \mathbf{e}_k \delta_{n+k}, \; \text{with} \; \mathbf{w}_{n} = \left(w_{n}, \cdots , w_{n+k-2} , w_{n+k-1} \right)^T, \; \mathbf{e}_k = \left(0, 0, \ldots, 1 \right)^T.
$$

</p>

<p>
Unroll:
 
$$
\mathbf{w}_n = \mathbf{C}^n \mathbf{w}_0 + \sum_{m=0}^{n-1} \mathbf{C}^{n-1-m}\mathbf{e}_k \delta_{m+k}
$$
 
$$
\| \mathbf{w}_n \|_{\infty} \leq M \| \mathbf{w}_0 \|_{\infty} + M \sum_{m=0}^{n-1} | \delta_{m+k} | \leq M \left( \|\mathbf{w}_0\|_{\infty} + \sum_{j=k}^{n-1+k} | \delta_j | \right) \le M \left( \max_{0 \le j \le k-1} |w_j| + \max_{k \le m \le N} |\delta_m| \right).
$$

</p>

<p><strong>Proof of the necessity half:</strong></p>
 
<p>
The general solution to the homogeneous linear difference equation $ \sum_{j=0}^k \alpha_j w_{n+j} = 0 $ can be expressed as $ w_n = \sum_{j=1}^s \sum_{\ell = 0}^{m_j - 1} c_{j, \ell}n^{\ell}\zeta_j^n $, where $ \zeta_j $ are distinct complex roots of the characteristic polynomial $ p(\zeta) = a_k \prod_{j=1}^s (\zeta - \zeta_j)^{m_j} $, and $ m_j $ are their respective multiplicities satisfying $ \sum_{j=1}^s m_j = k $.
</p>
 
<p><em>Proof by contradiction</em></p>

<p>
<ul>
<li>If there is a root $ | \zeta | > 1 $, then the homogeneous solution contains terms $ | \zeta |^n $ growing exponentially.</li>
<li>If there is a root $ | \zeta | = 1 $ with multiplicity $ \ge 2 $, the homogenous solution contains $ n \zeta $, which grows linearly in $ n $, which is also unbounded since $ n = \frac{T - t_0}{h} \to \infty \; \text{as} \; h \to 0 $. Thus, zero-stability implies the root condition.</li>
</ul>
</p>

## Convergence Theorem (Consistency + Zero-Stability $$ \iff $$ Convergence.)

**Convergence of linear multistep methods** A $$ k $$-step linear multi-step method is convergent on $$ [t_0, T] $$ if for sufficiently smooth IVP $$ y' = f(t, y) $$ with the exact solution $$ y(t) $$, and for any family of starting values $$ y_j(h), j = 0, \ldots, k-1 $$ satisfying $$ \lim_{h \to 0} | y(t_j) - y_j(h) | = 0, \; j = 0, \ldots, k-1 $$ and the numerical solution $$ \{ y_n(h) \}_{n=0}^{N(h)} $$ produced by the LMM satisfies $$ \lim_{h \to 0} \max_{0 \le n \le N(h)} | e_n | = 0 $$ with $$ e_n := y(t_n) - y_n(h) $$.

**Dahlquist Equivalence Theorem for Linear Multistep Methods**
If a linear multistep method for solving ODE IVPs is convergent, i.e., the global error $$ e_n := y(t_n) - y_n $$ satisfies $$ \max_{0 \leq n \leq N} | e_n | \to 0 \; \text{as} \; h \to 0 $$, if and only if it is consistent and zero-stable. Moreover, if $$ \max_{0 \leq n \leq N} | e_n | \leq C h^p $$, then the linear multistep method is of order $$ p $$.

Proof of sufficiency (Consistency + Zero-Stability $$ \implies $$ Convergence):

Subtracting the numerical scheme from the equation of the local truncation error $$ \tau_{n+k} := \sum_{j=0}^k \alpha_j y(t_{n+j}) - h \sum_{j=0}^k \beta_j f(t_{n+j}, y(t_{n+j})) $$:
 
$$
\sum_{j=0}^k \alpha_j e_{n+j} = h \sum_{j=0}^k \beta_j \left(f(t_{n+j}, y(t_{n+j})) - f(t_{n+j}, y_{n+j}) \right) + \tau_{n+k}
$$
 
Let $$ r_{n+k} = \sum_{j=0}^k \beta_j \left(f(t_{n+j}, y(t_{n+j})) - f(t_{n+j}, y_{n+j}) \right) $$, then the error recurrence can be viewed as a perturbed linear recurrence $$ \sum_{j=0}^k \alpha_j e_{n+j} = \delta_{n+k}, \; n \geq 0 $$ with $$ \delta_{n+k} = r_{n+k} + \tau_{n+k} $$. By zero-stability, there exists $$ C_0 $$ (independent of $$ h $$) such that 
 
$$
\max_{0 \leq n \leq N} | e_n | \leq C_0 \left( \max_{0 \leq j \leq k-1} | e_j | + \sum_{m=k}^N | r_m | + \sum_{m=k}^N | \tau_m | \right)
$$
 
The starting errors $$ e_0, \ldots, e_{k-1} $$ are assumed $$ \mathcal{O}(h^p) $$, which are usually obtained by a one-step method of order $$ p $$, so $$ \max_{0 \leq j \leq k-1} |e_j| \leq C_{s}h^p $$.
 
The LTE has order $$ p $$ by assumption: $$ | \tau_m | \leq C_{\tau} h^{p+1} $$. Hence $$ \sum_{m=k}^N | \tau_m | \leq C_{\tau} (Nh) h^p = C_{\tau}(T - t_0) h^p $$.
 
Using Lipschitz continuity of $$ f $$, i.e., $$ | f(t, y_1) - f(t, y_2) | \leq L | y_1 - y_2 | $$ to obtain an error bound of terms $$ r_m $$,
 
$$
| \sum_{j=0}^k \beta_j \left( f(t_{n+j}, y(t_{n+j})) - f(t_{n+j}, y_{n+j}) \right) | \le L \sum_{j=0}^k | \beta_j | | e_{n+j} |  \leq \widetilde{L} \max_{0 \leq j \leq k} | e_{n+j} |, \; \text{where} \;  \widetilde{L} = L\sum_{j=0}^k | \beta_j |
$$
 
$$
h \sum_{m=k}^{N}|r_m|\le h  \widetilde{L} \sum_{m=k}^{N} \max_{0\le j\le k}|e_{m-k+j}| \le \widetilde{L} h \sum_{m=0}^{N} \, \max_{0\le j\le k}|e_{m-k+j}|
$$

Let $$ E := \max_{0\le n\le N}|e_n| $$, $$ h\sum_{m=k}^N |r_m| \leq \widetilde{L}(T - t_0)E $$.

Collecting terms,

$$
E \leq C_0\left(C_s h^p + C_{\tau}(T - t_0)h^p + \widetilde{L}(T - t_0)E \right)
$$
 
$$
E \le \frac{C_0}{1 - C_0\widetilde{L}(T - t_0)}\left(C_s + C_{\tau}(T - t_0) \right) h^p = \mathcal{O}(h^p)
$$
 
If $$ 1 - C_0\widetilde{L}(T - t_0) > 0 $$, we are done. If not, we use a standard patching trick: splitting the time interval $$ [t_0, T] $$ into $$ M $$ short subintervals of length $$ \Delta < T - t_0 $$, chosen such that $$ C_0\widetilde{L}\Delta \le \frac{1}{2} $$. One the first subinterval, the error bound derived above holds true, implying the local maximum error is of order $$ \mathcal{O}(h^p) $$. Therefore, the endpoints of the previous subinterval can be used as starting values of the next subinterval, leading to the same error estimate by applying the above derivation. Iterating over a finite number of subintervals yields an overall bound $$ \max_{0 \le n \le N} |e_n| \le Ch^p $$.

Furthermore, it is evident that the source of global error of LMMs can be decomposed into two terms:
 
$$
\max_{0\le n\le N} |e_n| \le C_1 \cdot \underbrace{\max_{0 \le j \le k-1} | e_j |}_{\text{initial error}} + C_2 \cdot \underbrace{\max_{ k \le m \le N} |\tau_m |}_{\substack{\text{the sum of LTE} \\ \text{controlled by} \\ \text{zero-stability}}}
$$

Proof of necessity (Convergence $$ \implies $$ Consistency + Zero-Stability):

1. Convergence $$ \implies $$ Consistency

Subtracting the numerical scheme from the formula defining the LTE $$ \tau_{n+k} $$:
 
$$
\tau_{n+k} = \sum_{j=0}^k \alpha_j \left( y(t_{n+j}) - y_{n+j}(h) \right) + h \sum_{j=0}^k \beta_j \left( f(t_{n+j}, y_{n+j}(h)) - f(t_{n+j}, y(t_{n+j})) \right)
$$
 
By the Lipschitz continuity of $$ f $$,
 
$$
|\tau_{n+k}| \le \left( \sum_{j=0}^k |\alpha_j| \right) \max_{0 \le j \le k} | e_{n+j}(h) | + hL\left( \sum_{j=0}^k |\beta_j| \right) \max_{0 \le j \le k} | e_{n+j}(h) |
$$
 
Hence
 
$$
|\tau_{n+k}| \le C(h) \max_{0 \le j \le k} | e_{n+j}(h) |
$$
 
Since $$ e_n(h) \to 0 $$ (by convergence), $$ \lim_{h \to 0} | \tau_{n+k}(h) | = 0 \; \forall n \ge 0 $$.
 
Finally, applying this to the test function $$ y(t) = 1 $$ and $$ y(t) = t $$ gives the usual algebraic conditions. Specifically,
 
If $$ y(t) = 1 $$, then $$ y'(t) = 0 $$ and $$ \tau = \sum_{j=0}^k \alpha_j = \rho(1) $$. Since $$ \tau \to 0 $$, we get $$ \rho(1) = 0 $$.
 
If $$ y(t) = t $$, then $$ y(t_{n+j}) = t_n + jh, y'(t_{n+j}) = 1 $$. Then
 
$$
\tau = t_n \sum_j \alpha_j + h\left( \sum_j j \alpha_j - \sum_j \beta_j \right)
$$

Using $$ \sum_j \alpha_j = 0 $$, we conclude that $$ \sum_j j \alpha_j = \sum_j \beta_j $$, i.e., $$ \rho'(1) = \sigma(1) $$.

2. Convergence $$ \implies $$ Zero-Stability

Proof by contradiction. Assume the method is convergent as defined above, but not zero-stable. Then $$ \rho $$ violates the root condition:
 
<ul>
 	<li> Case A: $ \rho $ has a root $ \zeta_0 $ with $ | \zeta_0 | \ge 1 $;</li>
 	<li> Case B: $ \rho $ has a root $ \zeta_0 $ on the unit circle but with algebraic multiplicity $ m \ge 2 $.</li>
</ul>
 
To arrive at the contradiction, in each case, we only need to find a family of starting perturbations whose magnitude tends to zero as $$ h \to 0 $$, but for which the propagated solutions as per the numerical scheme $$ \sum_{j=0}^k \alpha_j y_{n+j} = h \sum_{j=0}^k \beta_j f_{n+j} $$ do not tend to zero.
 
Recall that the homogeneous recurrence $$ \sum_{j=0}^k \alpha_j w_{n+j} = 0 $$ can be written in the matrix-vector form $$ \mathbf{w}_{n+1} = \mathbf{C} \mathbf{w}_n $$, where $$ \mathbf{w}_n = ( w_{n}, \ldots, w_{n+k-2}, w_{n+k-1})^T $$ and $$ \mathbf{C} $$ is the companion matrix

$$
\mathbf{C} = \begin{pmatrix} 0 & 1 & \cdots & 0 & 0 \\ 0 & 0 & 1 & \cdots & 0 \\ \vdots & \vdots & \ddots & \ddots &\vdots \\ 0 & 0 & \cdots & 0 & 1 \\ - \alpha_0 & - \alpha_1 & \cdots & - \alpha_{k-2} & -\alpha_{k-1} \end{pmatrix}_{k \times k}
$$

The polynomial $$ p(\zeta) = \sum_{j=0}^k \alpha_j \zeta^j, \alpha_k = 1 $$ is both the characteristic and minimum polynomial for $$ \mathbf{C} $$, which means the eigenvalues of $$ \mathbf{C} $$ are exactly the roots of $$ \rho $$ and $$ \text{alg mult}(\lambda_j) = \text{index}(\lambda_j) $$ for each eigenvalue $$ \lambda_j \in \sigma(\mathbf{C}) $$.

Case A: $$ | \zeta_0 | \ge 1 $$.

Let $$ \mathbf{v} $$ be a (right) eigenvector of $$ C $$ associated to $$ \zeta_0 $$: $$ \mathbf{Cv} = \zeta_0 \mathbf{v} $$. Consider the homogeneous solution generated by initial state $$ \mathbf{w}_0 = \eta(h) \mathbf{v} $$. Then

$$
\mathbf{w}_n = \mathbf{C}^n \mathbf{w}_0 = \eta(h) \zeta_0^n \mathbf{v}
$$

Pick $$ \eta(h) = | \zeta_0 |^{-N(h)} $$. Then $$ \eta(h) \to 0 \; \text{as} \; h \to 0 $$, ensuring the starting perturbation $$ \mathbf{w}_0 \to 0 \; \text{as} \; h \to 0 $$, and thus it is an admissible family of starting perturbations under our convergence definition. But the propagated solution at the step $$ n = N(h) $$ is

$$
\mathbf{w}_{N(h)} = \eta(h) \zeta_0^{N(h)} \mathbf{v} = \left( \frac{\zeta_0}{|\zeta_0|} \right)^{N(h)} \mathbf{v}
$$

Since $$ \| \mathbf{w}_{N(h)} \| = \| v \| $$, the propagated perturbation does not tend to zero, contradicting convergence (which requires that any starting perturbation tending to zero produces a final error tending to zero). Therefore, there is no root with $$ | \zeta_0 | \ge 1 $$.

Case B: $$ |\zeta_0| = 1 $$ and algebraic multiplicity $$ m \ge 2 $$.

In this case, the companion matrix $$ \mathbf{C} $$ has a Jordan block for $$ \zeta_0 $$ of size $$ m \times m $$: 

$$ 
\begin{pmatrix} \zeta_0 & 1 & & \\ & \ddots & \ddots & \\ & & \ddots & 1 \\ & & & \zeta_0 \end{pmatrix}_{m \times m}.
$$ 

The nonderogatory property of the companion matrix $$ \mathbf{C} $$ indicates that $$ \text{alg mult}(\lambda_j) = \text{index}(\lambda_j) $$ for each eigenvalue $$ \lambda_j \in \sigma(\mathbf{C}) $$, excluding the possibility of eigenvalues being semisimple, i.e., the diagonal block $$ \begin{pmatrix} \lambda & & \\ & \ddots &  \\ & & \lambda \end{pmatrix}_{m \times m} $$.

$$
\mathbf{C}^n = \mathbf{S} \begin{pmatrix} \ddots & & \\ & \mathbf{J}^n_{\star} & \\ & & \ddots \end{pmatrix} \mathbf{S}^{-1}
$$

where 

$$
\mathbf{J}_{\star}^n = \begin{pmatrix} \zeta_0 & 1 & & \\ & \ddots & \ddots &\\ & & \ddots & 1\\ & & & \zeta_0 \end{pmatrix}_{m \times m}^n = \begin{pmatrix} \zeta_0^n & \begin{pmatrix} n \\ 1 \end{pmatrix} \zeta_0^{n-1} & \begin{pmatrix} n \\ 2 \end{pmatrix} \zeta_0^{n-2} & \cdots & \begin{pmatrix} n \\ m-1 \end{pmatrix} \zeta_0^{n-m+1} \\  & \zeta_0^n & \begin{pmatrix} n \\ 1 \end{pmatrix} \zeta_0^{n-1} & \ddots & \vdots \\  &  & \ddots & \ddots & \begin{pmatrix} n \\ 2 \end{pmatrix} \zeta_0^{n-2} \\  &  &  & \zeta_0^n & \begin{pmatrix} n \\ 1 \end{pmatrix} \zeta_0^{n-1} \\  &  &  &  & \zeta_0^n  \end{pmatrix}_{m \times m}
$$

Focus on the single $$ m \times m $$ Jordan block $$ \mathbf{J}^n_{\star} $$ associated with the eigenvalue $$ \zeta_0 $$, and $$ \mathbf{S}_{\star} = \left[\mathbf{v}_1, \mathbf{v}_2, \ldots, \mathbf{v}_m  \right] $$ be the portion of $$ \mathbf{S} = \left[ \cdots | \mathbf{S}_{\star} | \cdots \right] $$ corresponding to the position of $$ \mathbf{J}^n_{\star} $ in $ \mathbf{J}^n $$. Then, $$ \mathbf{C}^n \mathbf{S} = \mathbf{S} \begin{pmatrix} \ddots & & \\ & \mathbf{J}^n_{\star} & \\ & & \ddots \end{pmatrix} $$ implies $$ \mathbf{C}^n\mathbf{S}_{\star} = \mathbf{S}_{\star} \mathbf{J}^n_{\star}(\zeta_0) $$, i.e., 

$$
\mathbf{C}^n [\mathbf{v}_1, \mathbf{v}_2, \cdots, \mathbf{v}_m ] = [\mathbf{v}_1, \mathbf{v}_2, \cdots, \mathbf{v}_m ] \mathbf{J}_{\star}^n = [\mathbf{v}_1, \mathbf{v}_2, \cdots, \mathbf{v}_m ] \begin{pmatrix} \zeta_0^n & \begin{pmatrix} n \\ 1 \end{pmatrix} \zeta_0^{n-1} & \begin{pmatrix} n \\ 2 \end{pmatrix} \zeta_0^{n-2} & \cdots & \begin{pmatrix} n \\ m-1 \end{pmatrix} \zeta_0^{n-m+1} \\  & \zeta_0^n & \begin{pmatrix} n \\ 1 \end{pmatrix} \zeta_0^{n-1} & \ddots & \vdots \\  &  & \ddots & \ddots & \begin{pmatrix} n \\ 2 \end{pmatrix} \zeta_0^{n-2} \\  &  &  & \zeta_0^n & \begin{pmatrix} n \\ 1 \end{pmatrix} \zeta_0^{n-1} \\  &  &  &  & \zeta_0^n  \end{pmatrix}_{m \times m}
$$

Pick a generalised eigenvector in $$ \mathbf{S}_{\star} $$ of the highest order $$ \mathbf{v}_m $$, then

$$
\| \mathbf{C}^n \mathbf{v}_m \|_{\infty} = \| \mathbf{v}_m \mathbf{J}^n_{\star} \|_{\infty} \propto K n^{m-1} 
$$ 

Now constructing starting data as follows. Set $$ \eta(h) = N(h)^{-(m-1)} $$. Then, take the initial perturbation as $$ \mathbf{w}_0 = \eta(h)\mathbf{v}_m $$, which is admissible as $$ \eta(h) \to 0 \; \text{as} \; h \to 0 $$.

But the propagated perturbation at step $$ n = N(h) $$ is

$$
\| \mathbf{w}_{N(h)} \|_{\infty} = \| \mathbf{C}^{N(h)} \mathbf{w}_0 \|_{\infty} \propto \eta(h) K N(h)^{m-1} = K
$$

i.e., a nonzero constant independent of $$ h $$. Thus the final error does not vanish, again contradicting convergence.

Combining the conclusions obtained above shows that every root $$ \zeta $$ of $$ \rho $$ satisfies $$ | \zeta | \le 1 $$ and any root with $$ | \zeta | = 1 $$ is simple (algebraic multiplicity 1), which is exactly the root condition.

Next, let us take a quick look at some important linear multi-step methods.

1. Adams-Bashforth methods

Starting from the integral form:

$$
y(t_{n+1}) = y(t_n) + \int_{t_n}^{t_{n+1}} f(s, y(s)) ds
$$
 
Approximate $$ f(s, y(s)) $$ by the Lagrange interpolating polynomials.
 
AB2 (order 2)
 
Interpolate $$ f $$ through $$ t_n, t_{n-1} $$:
 
$$
P_1(s) = f_n \cdot \frac{s - t_{n-1}}{t_n - t_{n-1}} + f_{n-1} \cdot \frac{s - t_n}{t_{n-1} - t_n} = f_n \cdot \frac{s - t_{n-1}}{h} - f_{n-1} \cdot \frac{s - t_n}{h}
$$
 
Integrate on $$ [t_n, t_{n+1}] $$ (set $$ \tau = s - t_n \in [0, h] $$):
 
$$
\int_{t_n}^{t_{n+1}} P_1(s)ds = \int_0^h \left( f_n \frac{\tau + h}{h} - f_{n-1} \frac{\tau}{h} \right) d\tau = h\left(\frac{3}{2} f_n - \frac{1}{2} f_{n-1} \right)
$$
 
Thus
 
$$
y_{n+1} = y_n + h\left( \frac{3}{2} f_n - \frac{1}{2} f_{n-1} \right)
$$
 
Shifting the index forward to conform to the general form of a k-step LMM:
 
$$
y_{n+2} - y_{n+1} = h\left( \frac{3}{2} f_{n+1} - \frac{1}{2} f_n \right)
$$
 
with coefficients $$ (\alpha_0, \alpha_1, \alpha_2) = (0, -1, 1) $$ and $$ (\beta_0, \beta_1, \beta_2) = (-\frac{1}{2}, \frac{3}{2}, 0) $$.
 
It is straightforward to check the consistency and order conditions:
 
$$
\sum_j \alpha_j = 0 + (-1) + 1 = 0
$$

$$ 
\begin{align*}
    \sum_j \alpha_j &= 0 + (1 \cdot -1) + (2 \cdot 1) = 1 \\
 	\sum_j \beta_j &= - \frac{1}{2} + \frac{3}{2} + 0 =1
\end{align*}
$$

$$ 
\begin{align*}
    \sum_j j^2 \alpha_j &= 0 + (1 \cdot -1) + ( 2^2 \cdot 1) = 3\\
 	2 \cdot \sum_j j \beta_j &= 2\left( (0 \cdot - \frac{1}{2}) + (1 \cdot \frac{3}{2}) + (2 \cdot 0) \right)
\end{align*}
$$ 

$$
\begin{align*}
    \sum_j j^3 \alpha_j &= 0 + (1 \cdot -1) + (2^3 \cdot 1) = 7 \\
 	3 \cdot \sum_j j^2 \beta_j &= 3\left( (0 \cdot - \frac{1}{2}) + (1 \cdot \frac{3}{2}) + (2^2 \cdot 0) \right) = \frac{9}{2} 
\end{align*}
$$
 
The LTE of AB2 is of order $$ \mathcal{O}(h^3) $$, and thus AB2 is of order $$ \mathcal{O}(h^2) $$. 
 
2. Adams-Moulton methods
 
AM2 (trapezoidal rule)
 
Interpolate through $$ t_n, t_{n+1} $$:
 
$$
P_1(s) = f_n \cdot \frac{t_{n+1} - s}{h} + f_{n+1} \cdot \frac{s - t_n}{h}
$$
 
Integrate on $$ [t_n, t_{n+1}] $$ (set $$ \tau = s - t_n \in [0, h] $$):
 
$$
\int_{t_n}^{t_{n+1}} P_1(s) ds = \int_0^h \left( f_n \frac{h - \tau}{h} + f_{n+1}\frac{\tau}{h} \right)d\tau = h \cdot \frac{f_n + f_{n+1}}{2}
$$
 
This yields
 
$$
y_{n+1} - y_n = h \left( \frac{1}{2} f_n  + \frac{1}{2} f_{n+1} \right)
$$
 
It is straightforward to check consistency conditions are satisfied and the $$ \ell = 2 $$ condition holds; the LTE leading term is $$ -\frac{1}{12}h^3 y^{(3)}(t_n) $$. Hence, the AM2 method is of order $$ \mathcal{O}(h^2) $$.

AM3 (2-step, order 3)
 
Interpolate $$ f $$ through $$ t_{n+1}, t_n, t_{n-1} $$:
  
$$
P_2(s) = f_{n-1} \frac{(s - t_n)(s - t_{n+1})}{(t_{n-1} - t_n)(t_{n-1} - t_{n+1})} + f_n \frac{(s - t_{n-1})(s - t_{n+1})}{(t_n - t_{n-1})(t_n - t_{n+1})} + f_{n+1} \frac{(s - t_{n-1})(s - t_n)}{(t_{n+1} - t_{n-1})(t_{n+1} - t_n)}
$$
 
Integrate on $$ [t_n, t_{n+1}] $$ (set $$ \tau = s - t_n \in [0, h] $$):
 
$$
\begin{align*}
 	\int_{t_n}^{t_{n+1}} P_2(s) ds &= \int_0^h \left( f_n \frac{\tau(\tau - h)}{2h^2} - f_n \frac{(\tau + h)(\tau - h)}{h^2} + f_{n+1}\frac{(\tau + h)\tau}{2h^2} \right) d\tau \\
 		&= \frac{h}{12} \left( -f_{n-1} + 8f_n + 5f_{n+1} \right)
\end{align*}
$$
 
Shift index
 
$$
y_{n+2} = y_{n+1} + \frac{h}{12} \left(5f_{n+2} + 8f_{n+1} - f_n \right)
$$
 
It is straightforward to check consistency conditions are satisfied and the $$ \ell = 2, 3 $$ conditions hold; the LTE leading term is $$ -\frac{1}{24}h^4 y^{(4)}(t_n) $$. Hence, the AM3 method (2-step) is of order $$ \mathcal{O}(h^3) $$. 

3. Backward Differentiation Formulas
 
This type of linear multistep method uses a polynomial $$ P_k(t) $$ of degree $$ k $$ passing through past values $$ y_{n+1}, y_n, \ldots, y_{n-k+1} $$ to approximate the solution $$ y(t) $$ instead of its derivative $$ y'(t) $$ as in the Adams methods. Then differentiate $$ P_k(t) $$ and enforce $$ P_k'(t_{n+1}) = f(t_{n+1}, y_{n+1}) $$.
 
BDF2
 
The degree 2 Lagrange interpolating polynomial through $$ (t_{n-1}, y_{n-1}), (t_n, y_n), (t_{n+1}, y_{n+1}) $$ is 
 
$$
P_2(t) = y_{n-1} \frac{(t - t_n)(t - t_{n+1})}{(t_{n-1} - t_n)(t_{n-1} - t_{n+1})} + y_n \frac{(t - t_{n-1})(t - t_{n+1})}{(t_n - t_{n-1})(t_n - t_{n+1})} + y_{n+1} \frac{(t - t_{n-1})(t - t_n)}{(t_{n+1}- t_{n-1})(t_{n+1}- t_n)}
$$
 
Let $$ \theta = \frac{t - t_{n-1}}{h} $$.
 
$$
P_2(t) = y_{n-1} \frac{(\theta - 1)(\theta - 2)}{2} + y_n \theta(2 - \theta) + y_{n+1} \frac{\theta (\theta - 1)}{2}
$$
 
Differentiate (note $$ \frac{d\theta}{dt} = \frac{1}{h} $$) and evaluate at $$ t_{n+1} $$ (i.e., $$ \theta = 2 $$):
 
$$
\begin{align*}
 	P'_2(t_{n+1}) &= \frac{1}{h} \left( y_{n-1} \cdot \frac{(\theta - 1) + (\theta - 2)}{2} + y_n \cdot ((2 - \theta) - \theta)  + y_{n+1} \frac{\theta + (\theta - 1)}{2} \right)|_{\theta=2} \\ 
 	&=  \frac{1}{h} \left( \frac{1}{2}y_{n-1} - 2y_n + \frac{3}{2} y_{n+1} \right)
\end{align*}
$$
 
Enforce $$ P_k'(t_{n+1}) = f(t_{n+1}, y_{n+1}) $$ and shift index to obtain
 
$$
\frac{3}{2} y_{n+2} - 2y_{n+1} + \frac{1}{2}y_n = h f_{n+2}
$$
 
It is straightforward to check consistency conditions are satisfied and the $$ \ell = 2 $$ condition holds; the LTE leading term is $$ -\frac{1}{3}h^3 y^{(3)}(t_n) $$. Hence, the BDF2 method is of order $$ \mathcal{O}(h^2) $$.

## Concluding Remarks

Voil\`{a}! This concludes an article devoted to presenting a detailed mathematical proof of the Dahlquist Equivalence Theorem for Linear Multistep Methods (LMMs), which states that Convergence $$ \iff $$ Consistency + Zero-Stability. The key takeaway is that the global error of LMMs can be decomposed into two distinct components:

$$
\max_{0\le n\le N} |e_n| \le C_1 \cdot \underbrace{\max_{0 \le j \le k-1} | e_j |}_{\text{initial error}} + C_2 \cdot \underbrace{\max_{ k \le m \le N} |\tau_m |}_{\substack{\text{the sum of LTE} \\ \text{controlled by} \\ \text{zero-stability}}}.
$$

Moreover, the mathematical proof fundamentally relies on tools from matrix analysis, particularly the equivalence between the root condition and the boundedness of matrix powers.

Should this article serve as a source of motivation, I highly recommend Matrix Analysis and Applied Linear Algebra <d-cite key="meyer2023matrix"></d-cite> by Carl D. Meyer, a book I repeatedly consulted while writing this post, which beautifully illustrates both the utility and elegance of the subject. One of Meyer’s guiding principles in composing the book is to reveal portions of the scaffolding, narratives, examples and summaries to enhance readability. This stands in contrast to the somewhat condescending view attributed to Carl Friedrich Gauss, who once remarked that ``architects of great cathedrals do not obscure the beauty of their work by leaving the scaffolding in place after the construction has been completed." In this sense, mathematically proficient large language models (LLMs) offer an unprecedented opportunity for learners like myself—not only to admire the beauty of the “cathedral”, but also to explore the underlying scaffolding of its construction.

## Acknowledgement

This blog was developed from a self-contained, step-by-step tutorial generated by OpenAI ChatGPT, which also assisted in refining the phrasing and expressions, as well as in suggesting suitable titles.

{% raw %}

```html
@misc{Cai2025odelearningnotes-3,
    author = {Xin Cai},
    title = {Reconstructing the Scaffolding A Learner’s Notes on Ordinary Differential Equations: Part 3 Understanding the Dahlquist Equivalence Theorem for Linear Multistep Methods},
    howpublished = {\url{https://totalvariation.github.io/blog/2025/concise-intro-odes-part3/}},
    note = {Accessed: 2025-10-28},
    year = {2025}
}
```

{% endraw %}

