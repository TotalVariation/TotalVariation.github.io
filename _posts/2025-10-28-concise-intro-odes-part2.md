---
layout: distill
output:
  distill::distill_article:
    toc: true
    toc_depth: 2
title: Reconstructing the Scaffolding A Learner’s Notes on Ordinary Differential Equations
description: Part &#8545 Convergence and Order in One-Step (Runge–Kutta) Methods
tags:
giscus_comments: true
date: 2025-10-28
featured: true

authors:
  - name: Xin Cai
    url: "https://totalvariation.github.io/"

bibliography: 2025-10-28-concise-intro-odes-part2.bib

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
  - name: Convergence of General One Step Methods
  - name: General Runge-Kutta Methods
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
---

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include video.liquid path="assets/video/traj-one-step-methods.mp4" class="img-fluid rounded z-depth-1" controls=true autoplay=true %}
    </div>
</div>
<div class="caption">
    Animated Trajectories of One-Step Methods on Lotka-Volterra Equations
</div>

<p>
This post is part of my ongoing series of learning notes on Ordinary Differential Equations (ODEs). In this instalment, I will first present a fundamental theorem on the convergence of general one-step methods, followed by a concise introduction to Runge–Kutta (RK) methods, including detailed derivations of the order conditions up to order four.
</p>

<p>
Throughout this blog, we focus on the initial value problems (IVPs) for one-dimensional ODEs for notational simplicity
 
$$
y'(t) = f(t, y(t)), \; y(t_0) = y_0, \; t \in [t_0, T]
$$
 
with $ f $ a sufficiently smooth (as specified when needed) scalar function. In the more general setting, where $ f : \mathbb{R} \times \mathbb{R}^d \to \mathbb{R}^d $, the corresponding proofs remain essentially identical, with vector norms used in place of absolute values where appropriate.
</p>

<h2> Convergence of General One Step Methods </h2>

<p>
A general (explicit) one-step method can be written in the form:

$$
y_{n+1} = y_n + h \Phi(t_n, y_n, h), \; n=0,1,\ldots, N-1, \; h = \frac{T - t_0}{N}, \; t_n = t_0 + nh, \; y_0 = y(t_0)
$$

</p>

<p>
<strong>Definition Local Truncation Error (LTE):</strong> The local truncation error $ \tau_{n+1} $ at step $ t_{n+1} $ is defined as by assuming $ y_n = y(t_n) $ is exact:

$$
\tau_{n+1} = \left[ y(t_{n+1}) - y(t_n) - h \Phi(t_n, y(t_n), h) \right] 
$$

</p>

<p>
The method is consistent if $ \max_{0 \leq n \leq N-1} \frac{1}{h} |\tau_{n+1}| \to 0 \; \text{as} \; h \to 0 $, which is equivalent to $ \Phi(t, y, 0) = f(t, y), \; \text{for all} \; t \in [0, T] $ as $ \lim_{h \to 0} \frac{1}{h} \tau_{n+1} = y'(t_n) - \Phi(t_n, y(t_n), 0) $.
</p>

<p>
Yet, the convergence of a numerical scheme concerns the global error $ e_n = y(t_n) - y_n $, which accumulates over steps. The method is convergent if: $ \max_{0 \leq n \leq N} |e_n| \to 0 \; \text{as} \; h \to 0 $, with $ t \in [0, T] $. It is convergent of order $ p $ if $ \max |e_n| = O(h^p) $.

The consistent condition alone does not guarantee convergence, we need an additional stability condition on $ \Phi $: $ |\Phi(t, y_1, h) - \Phi(t, y_2, h)| \leq L_{\Phi} |y_1 - y_2|, t \in [0, T], h \in [0, h_0] $, i.e.,  $ \Phi $ satisfying the Lipschitz condition in $ y $ and independent of $ h $. This ensures errors do not amplify excessively.
</p>

<p>
<strong>Theorem Convergence of One-Step Method</strong>

If the one-step method is consistent and $ \Phi $ is Lipschitz in $ y $ with constant $ L_{\Phi} $ independent of $ h $, then it is convergent with the global error expressed as: $ |e_n| \le \frac{\tau}{hL_{\Phi}} (e^{L_{\Phi}(T - t_0)} - 1) $, where $ \tau = \max_{0 \leq n \leq N} |\tau_n| $.
</p>

<p><strong>Proof:</strong></p>

<p>
By the definition of the LTE, the exact solution satisfies:

$$
y(t_{n+1}) = y(t_n) + h \Phi(t_n, y(t_n), h) + \tau_{n+1}
$$

Subtracting the numerical scheme $$ y_{n+1} = y_n + h \Phi(t_n, y_n, h) $$, we obtain a recursive formula for error function:

$$
\begin{align*}
e_{n+1} &= y(t_{n+1}) - y_{n+1} = [y(t_n) - y_n] + h [\Phi(t_n, y(t_n), h) - \Phi(t_n, y_n, h)] + \tau_{n+1} \\
e_{n+1} &= e_n + h [\Phi(t_n, y(t_n), h) - \Phi(t_n, y_n, h)] + \tau_{n+1} \\
|e_{n+1}| &\le |e_n| + h |\Phi(t_n, y(t_n), h) - \Phi(t_n, y_n, h)| + |\tau|
\end{align*}
$$

</p>

<p>
By the Lipschitz condition on $ \Phi $, i.e., $ |\Phi(t_n, y(t_n), h) - \Phi(t_n, y_n, h)| \leq L_{\Phi} |y(t_n) - y_n| = L_{\Phi} |e_n| $, we get,

$$
\begin{align*}
|e_{n+1}| &\le (1 + hL_{\Phi} ) |e_n| + |\tau| \\
|e_{n}| &\le (1 + h L_{\Phi} )^n |e_0| + |\tau| \sum_{k=0}^{n-1} (1 + hL_{\Phi})^k
\end{align*}
$$

</p>

<p>
If $ |e_0| = y_0 - y(t_0) = 0 $, we get

$$
|e_n| \leq |\tau| \cdot \frac{(1 + h L_{\Phi})^n - 1}{h L_{\Phi}} = \frac{|\tau|}{hL_{\Phi}} \left[ (1 + h L_{\Phi})^n - 1 \right]
$$

</p>

<p>
Since $ 1 + x \leq e^x $ and $ n h = t_n - t_0 \le T - t_0 $,  we have $ (1 + h L_{\Phi})^n \leq e^{n h L_{\Phi}} \leq e^{L_{\Phi} (T - t_0)} $, thus yielding the final expression: 

$$ 
|e_n| \leq \frac{|\tau|}{hL_{\Phi}} (e^{L_{\Phi} (T - t_0)} - 1) 
$$ 

As $ h \to 0 $, consistency implies $ \frac{1}{h}|\tau| \to 0 $, so $ |e_n| \to 0 $, proving convergence. If the method is of order $ p $, then $ |\tau| = O(h^{p+1}) $, so $ |e_n| = O(h^p) $, proving order $ p $ convergence.
</p>

<p>
The main takeaway is, for one-step methods, <strong>consistency + one-step stability (Lipschitz continuity) $ \implies $ convergence</strong>. Furthermore, the LTE is $ \mathcal{O}(h^{p+1}) $ $ \implies $ the global error is $ \mathcal{O}(h^p) $.
</p>

<p>
Next, we will examine the LTE of some common one-step methods:

<ol>
<li> Explicit Euler $ y_{n+1} = y_n + hf(t_n, y_n) $. Use Taylor expansion of the exact solution:

$$
y(t_{n+1}) = y(t_n) + h y'(t_n) +  \mathcal{O}(h^2)
$$

Given $ y'(t_n) = f(t_n, y(t_n)) $, so
 
$$
\tau_{n+1} = y(t_{n+1}) - \left(y(t_n) + hf(t_n, y(t_n)) \right) =  \mathcal{O}(h^2)
$$
 
hence $ |\tau| \le Ch^2 $ (with $ C = \max_{t \in [t_0, T]} |y''(t)| $). Thus, explicit Euler has order $ p = 1 $.
</li>
 
<li> Improved Euler $ y_{n+1} = y_n + \frac{h}{2} \left[ f(t_n, y_n) + f(t_n + h, y_n + h f(t_n, y_n)) \right] $. Use Taylor expansion of the exact solution:
 
$$
y(t_{n+1}) = y(t_n) + h y'(t_n) + \frac{h^2}{2} y''(t_n) + \mathcal{O}(h^3)
$$
 
Given $ y'(t_n) = f(t_n, y(t_n)) $ and 
 
$$
y''(t_n) = f_t(t_n, y(t_n)) + f_y(t_n, y(t_n)) f(t_n, y(t_n))
$$
 
we have
 
$$
y(t_{n+1}) = y(t_n) + h f(t_n, y(t_n)) + \frac{h^2}{2} \left[ f_t(t_n, y(t_n)) + f_y(t_n, y(t_n)) f(t_n, y(t_n)) \right] + \mathcal{O}(h^3)
$$
 
Use multivariate Taylor expansion:
 
$$
f(t_n + h, y(t_n) + h f(t_n, y(t_n))) = f(t_n, y(t_n)) + h  f_t(t_n, y(t_n)) + h  f_y(t_n, y(t_n)) f(t_n, y(t_n)) + \mathcal{O}(h^2)
$$
 
so
 
$$
\tau_{n+1} = y(t_{n+1}) - \left( y(t_n) + \frac{h}{2} \left( f(t_n, y(t_n)) + f(t_n + h, y(t_n) + h f(t_n, y(t_n))) \right) \right) =  \mathcal{O}(h^3)
$$
 
hence $ |\tau| \le Ch^3 $. Thus, improved Euler has order $ p = 2 $.
</li>
</ol>
</p>

<p>It turns out that the methods we discussed above, i.e., explicit Euler and improved Euler, are special cases of Runge-Kutta methods, which we will inspect shortly.</p>

<h2> General Runge-Kutta Methods </h2>

An $$ s $$-stage Runge-Kutta (RK) method computes stage derivatives
 
$$
k_i = f \left(t_n + c_i h, y_n + h \sum_{j=1}^s a_{ij}k_j \right), i=1,\ldots,s,
$$
 
and updates as per the following formula:
 
$$
y_{n+1} = y_n + h \sum_{i=1}^s b_i k_i.
$$
 
The coefficients $$ \mathbf{A} = [a_{ij}]_{s \times s}, \mathbf{b} = (b_1, \cdots, b_s), \mathbf{c}=(c_1, \cdots, c_s)^T $$ are real numbers arranged in the Butcher tableau

$$
\begin{array}{c|ccc}
	c_1 & a_{11} & \cdots & a_{1s} \\
	\vdots & \vdots & \ddots & \vdots \\
	c_s & a_{s1} & \cdots & a_{ss} \\
	\hline
 	& b_1 & \cdots & b_s
	\end{array}
$$

The $$ k_i $$ are slopes evaluated at intermediate points. If $$ \mathbf{A} $$ is strictly lower triangular ($$ a_{ij} = 0 $$ for $$ j \ge i $$), the method is explicit. Otherwise, it is implicit.
 
To derive RK methods of order $$ p $$, the numerical solution must match the exact Taylor expansion up to $$ O(h^{p+1}) $$ (i.e., ensuring the LTE is $$ O(h^{p+1}) $$). Expand $$ y_{n+1} $$ and each $$ k_i $$ in Taylor series, then equate coefficients. This yields order conditions on $$ \mathbf{b}, \mathbf{c}, A $$.
 
We will use one-dimensional ODEs for simplicity, but the methods extend naturally to systems of ODEs, i.e., $$ y(t) \in \mathbb{R}^d, f : \mathbb{R} \times \mathbb{R}^d \to \mathbb{R}^d $$ with higher-order derivatives generalised to multilinear maps (e.g., second order derivatives correspond to bilinear maps).

Taylor expansion of the exact solution
 
Fix $$ t = t_n $$. Denote $$ f = f(t, y(t)), f_t = \frac{\partial f}{\partial t}, f_y = \frac{\partial f}{\partial y} $$. Differentiating the ODE $$ y' = f $$ yields
 
$$
y'' = \frac{d}{dt} f(t,y) = f_t + f_y y' = f_t + f_y f
$$
 
$$
y^{(3)} = \frac{d}{dt} (f_t + f_y f) = f_{tt} + f_{ty}y' + \frac{d}{dt}(f_y f)
$$
 
$$
\frac{d}{dt}(f_y f) = (f_{yt} + f_{yy}y')f + f_y(f_t + f_y y')
$$
 
Substitute $$ y' = f $$ to obtain
 
$$
y^{(3)} = f_{tt} + 2f_{ty}f + f_{yy}ff + f_y f_t + f_y f_y f
$$
 
Thus the Taylor expansion of the exact solution is
 
$$
y(t + h) = y + hf + \frac{h^2}{2}(f_t + f_y f) + \frac{h^3}{6} (f_{tt} + 2f_{ty}f + f_{yy}ff + f_y f_t + f_y f_y f) + \mathcal{O}(h^4)
$$
 
If $$ f : \mathbb{R} \times \mathbb{R}^d \to \mathbb{R}^d $$, then $$ f_y \in \mathbb{R}^{d \times d} $$ is the Jacobian matrix. Correspondingly, $$ f_y f $$ denotes matrix-vector multiplication, and $$ f_{yy} f f := f_{yy}[f, f] $$ denotes the bilinear map <d-footnote>We encourage readers seeking a deep understanding of this concept to consult the chapter 12 of the book Matrix Calculus (for Machine Learning and Beyond). <d-cite key="bright2025matrix"></d-cite> </d-footnote> acting on vectors, where $$ f_{yy} \in \mathcal{L}(\mathbb{R}^d, \mathcal{L}(\mathbb{R}^d, \mathbb{R}^d)) $$, $$ f_{yy} \cdot e_i = H_{f_i} $$ with $$ H_{f_i} $$ defined as per the Hessian matrix of a scalar-valued function.

Stage expansions
 
For small $$ h $$, expand each stage:
 
$$
k_i = f \left(t + c_i h, y + h \sum_j a_{ij}k_j \right)
$$
 
1. Zeroth order: $$ k_i = f + \mathcal{O}(h) $$.
 
2. First order: $$ k_i = f + c_i h f_t + h f_y \sum_j a_{ij} k_j + \mathcal{O}(h^2) $$
 
Replace $$ k_j = f + \mathcal{O}(h) $$ inside the sum to get
 
$$
k_i = f + c_i h f_t + h f_y \sum_j a_{ij} f + \mathcal{O}(h^2)
$$
 
Assume the row sum condition $$ c_i := \sum_j a_{ij} $$, which holds true for all classic RK methods. Then
 
$$
k_i = f + h(c_i f_t + c_i f_y f) + \mathcal{O}(h^2)
$$
 
Insert the expansion of $$ k_i $$ into the numerical update $$ y_{n+1} = y_n + h \sum_i b_i k_i $$:
 
$$
y_{n+1} = y_n + h \sum_i b_i f + h^2 \sum_i b_i (c_i f_t + \alpha_i f_y f) + \mathcal{O}(h^3)
$$
 
Group terms:
 
$$
y_{n+1} = y_n + h \left( \sum_i b_i \right) f + \frac{h^2}{2} \left(2 \sum_i b_i c_i \right) f_t + \frac{h^2}{2}\left( 2 \sum_i b_i c_i \right)f_y f + \mathcal{O}(h^3)
$$
 
Matching with the exact Taylor series to order 2
 
$$
y(t + h) = y + hf + \frac{h^2}{2} (f_t + f_y f) + \mathcal{O}(h^3)
$$
 
Equate coefficients of $$ h $$ and $$ h^2 $$:
 
**Order 1 condition**:
 
$$
\sum_{i=1}^s b_i = 1
$$
 
**Order 2 condition**:
 
$$
\sum_{i=1}^s b_i c_i = \frac{1}{2}
$$
 
**Order 3 conditions**:
 
We need one higher order in the stage expansion.
 
$$
k_i = f + c_i h f_t + h f_y \sum_j a_{ij} k_j + \frac{1}{2}(c_i h)^2 f_{tt} + c_i h f_{ty} \left( h \sum_j a_{ij} k_j  \right) + \frac{1}{2} \left( h \sum_j a_{ij} k_j \right) f_{yy} \left( h \sum_{\ell} a_{i \ell} k_{\ell} \right) + \mathcal{O}(h^3)
$$
 
Now substitute $$ k_j = f + h \left( c_j f_t + c_j f_y f \right) +  \mathcal{O}(h^2) $$ into those sums,
 
$$ 
\sum_j a_{ij} k_j = c_i f + h \sum_j a_{ij}\left( c_j f_t + c_j f_y f \right) +  \mathcal{O}(h^2) 
$$

$$
\begin{align*}
	k_i &= f + c_i h f_t + h f_y (c_i f) + \frac{h^2}{2}c^2_i f_{tt} + c_i h f_{ty} \left( h c_i f \right) + \frac{h^2}{2} (c_i f) f_{yy} (c_i f) \\
	& + h^2 \left( \sum_j a_{ij} c_j \right) f_y f_t + h^2 \left( \sum_j a_{ij} c_j \right) f_y f_y f + \mathcal{O}(h^3)
\end{align*}
$$

$$
\begin{align*}
	k_i &= f + h (c_i f_t + c_i f_y f) \\
	&+ \frac{h^2}{2} c^2_i f_{tt} + h^2 c_i^2 f_{ty}f + \frac{h^2}{2} c^2_i f_{yy} f f \\
	&+ h^2 \left( \sum_j a_{ij} c_j \right) f_y f_t + h^2 \left( \sum_j a_{ij} c_j \right) f_y f_y f + \mathcal{O}(h^3)
\end{align*}
$$

Now substitute into the update $$ y_{n+1} = y + h \sum_i b_i k_i $$ to obtain,

$$
\begin{align*}
	y_{n+1} &= y + h \sum_i b_i f \\
		& + h^2 \sum_i b_i \left( c_i f_t + c_i f_y f \right) \\
		& + h^3 \left[ \frac{1}{2} \sum_i b_i c_i^2 f_{tt} + \sum_i b_i c_i^2 f_{ty}f + \frac{1}{2}\sum_i b_i c_i^2 f_{yy} f f  \right. \\ & \left. + \left(\sum_{i, j}b_i a_{ij} c_j \right) f_y f_t + \left(\sum_{i, j}b_i a_{ij} c_j \right) f_y f_y f   \right] + \mathcal{O}(h^4).
\end{align*}
$$

Compare with the exact Taylor series to order 3:

$$
y(t + h) = y + hf + \frac{h^2}{2} (f_t + f_y f) + \frac{h^3}{3} \left( f_{tt} + 2 f_{ty} f + f_{yy} f f + f_y f_t + f_y f_y f \right) + \mathcal{O}(h^4)
$$

we get:
 
$$
\sum_i b_i = 1
$$
 
$$
\sum_i b_i c_i = \frac{1}{2}
$$
 
$$
\sum_i b_i c_i^2 = \frac{1}{3}
$$
 
$$
\sum_{i,j} b_i a_{ij} c_j = \frac{1}{6}
$$
 
So up to order 3, the independent order conditions are
 
$$
\begin{align*}
 	\sum_i b_i = 1, \sum_i b_i c_i = \frac{1}{2}, \sum_i b_i c_i^2 = \frac{1}{3}, \sum_{i,j} b_i a_{ij} c_j = \frac{1}{6}
\end{align*}
$$

**Order 4 conditions**:
 
$$
\begin{align*}
    \sum_i b_i c_i^3 = \frac{1}{4} \\
 	\sum_{i, j} b_i c_i a_{ij} c_j = \frac{1}{8} \\
 	\sum_{i, j} b_i a_{ij} c_j^2 = \frac{1}{12} \\
 	\sum_{i, j, \ell} b_i a_{ij} a_{j \ell} c_{\ell} = \frac{1}{24}
\end{align*}
$$ 
 
Sketch of derivation <d-footnote>In the following derivations, we assume the general setting, where $$ y(t) \in \mathbb{R}^d, f : \mathbb{R} \times \mathbb{R}^d \to \mathbb{R}^d $$</d-footnote>
 
Extend the stage expansion by one more order to order 4:
 
$$
\begin{align*}
    k_i &= f + h(c_i f_t + c_i f_y f) \\
 		&+ h^2\left( (\sum_j a_{ij}c_j)f_y f_t + (\sum_j a_{ij}c_j)f_y f_y f + \frac{1}{2} c_i^2 f_{tt} + c_i^2 f_{ty}f + \frac{1}{2} c_i^2 f_{yy}(f, f) \right) \\ 
 		&+ h^3 \left( \frac{1}{2} (\sum_j a_{ij}c_j^2) f_y f_{tt} +  (\sum_j a_{ij}c_j^2) f_y f_{ty} f + \frac{1}{2}(\sum_j a_{ij}c_j^2) f_y f_{yy}(f, f) \right. \\
 		&\left. + (\sum_{j \ell} a_{ij} a_{j \ell} c_{\ell}) f_y f_y f_t + (\sum_{j \ell} a_{ij} a_{j \ell} c_{\ell}) f_y f_y f_y f + c_i(\sum_j a_{ij}c_j)f_{ty}f_t + c_i(\sum_j a_{ij}c_j) f_{ty} f_y f  \right. \\ 
 		&\left. + c_i(\sum_j a_{ij}c_j) f_{yy} (f_t, f) +   c_i(\sum_j a_{ij}c_j) f_{yy}(f, f_y f) \right. \\ 
 		&\left. + \frac{1}{6} c_i^3 f_{ttt} + \frac{1}{6} c_i^3 f_{yyy}(f, f, f) + \frac{1}{2} c_i^3 f_{tyy}(f, f) + \frac{1}{2} c_i^3 f_{tty} f  \right) + \mathcal{O}(h^4)
\end{align*}
$$
 
substitute into the update:

$$
\begin{align*}
 	y_{n+1} &= y_n + h (\sum_i b_i) f + h^2 \left( (\sum_i b_i c_i)f_t + (\sum_i b_i c_i)f_y f \right) \\
 		& + h^3 \left( (\sum_{ij} b_i a_{ij} c_j )f_y f_t + (\sum_{ij} b_i a_{ij} c_j ) f_y f_y f + \right. \\
 		&\left. + \frac{1}{2} (\sum_i b_i c_i^2) f_{tt} + (\sum_i b_i c_i^2) f_{ty} f + \frac{1}{2} (\sum_i b_i c_i^2) f_{yy}(f, f)  \right) \\
 		&+ h^4 \left( \frac{1}{2}(\sum_{ij} b_i a_{ij} c_j^2) f_y f_{tt} + (\sum_{ij} b_i a_{ij} c_j^2) f_y f_{ty} f + \frac{1}{2} (\sum_i b_i a_{ij} c_j^2) f_y f_{yy}(f, f) \right. \\
 		&\left. + (\sum_{i j \ell} b_i a_{ij} a_{j\ell} c_{\ell})f_y f_y f_t + (\sum_{i j \ell} b_i a_{ij} a_{j\ell} c_{\ell}) f_y f_y f_y f + (\sum_{ij} b_i c_i a_{ij} c_j) f_{ty} f_t + (\sum_{ij} b_i c_i a_{ij} c_j)f_{ty} f_y f \right. \\ 
 		& \left. + (\sum_{ij} b_i c_i a_{ij} c_j) f_{yy}(f_t, f) + (\sum_{ij} b_i c_i a_{ij} c_j) f_{yy}(f, f_y f) + \frac{1}{6} (\sum_i b_i c_i^3) f_{ttt} \right. \\ 
 		&\left. + \frac{1}{6} (\sum_i b_i c_i^3) f_{yyy}(f, f, f) + \frac{1}{2} (\sum_i b_i c_i^3) f_{tyy}(f, f) + \frac{1}{2} (\sum_i b_i c_i^3) f_{tty}f  \right) + \mathcal{O}(h^5)
\end{align*}
$$
 
match with the exact Taylor series

$$ 
\begin{align*}
 	y(t + h) &= y + hf + \frac{h^2}{2} \left(f_t + f_y f \right) + \frac{h^3}{6} \left(f_{tt} + 2f_{ty}f + f_{yy}(f, f) + f_y f_t + f_y f_y f \right) \\
 		& + \frac{h^4}{24} \left( f_{ttt} + 3 f_{tty}f + 3 f_{tyy}(f, f) + 3 f_{ty}f_t + 3 f_{ty} f_y f + f_{yyy}(f, f, f) + 3f_{yy}(f_t, f) \right. \\
 		&\left. + 3f_{yy} (f_y f, f) + f_y f_{tt} + 2 f_y f_{ty} f + f_y f_{yy}(f, f) + f_y f_y f_t + f_y f_y f_y f \right) + \mathcal{O}(h^5)
\end{align*}
$$

Next, let us take a quick look at the classical RK4 method
 
Butcher tableau:
 
$$
\begin{array}{c|cccc}
	0 & 0 & 0 & 0 & 0 \\
	\frac{1}{2} & \frac{1}{2} & 0 & 0 & 0 \\
	\frac{1}{2} & 0 & \frac{1}{2} & 0 & 0 \\
	1 & 0 & 0 & 1 & 0 \\
	\hline
 	& \frac{1}{6} & \frac{1}{3} & \frac{1}{3} & \frac{1}{6}
\end{array}
$$
 
Stage derivatives:
 
$$
\begin{align*}
    k_1 &= f(t_n ,y_n) \\
 	k_2 &= f(t_n + \frac{1}{2}h, y_n + \frac{1}{2}h k_1)\\
 	k_3 &= f(t_n + \frac{1}{2}h, y_n + \frac{1}{2}h k_2)\\
 	k_4 &= f(t_n + h ,y_n + h k_3)
\end{align*}
$$

The numerical update:
 
$$
y_{n+1} = y_n + \frac{h}{6} \left(k_1 + 2 k_2 + 2 k_3 + k_4 \right)
$$
 
Next, we will construct a general 2-stage, order-2 explicit RK by using order conditions derived above. Let
 
$$
\begin{array}{c|cc}
    0 & 0 & 0 \\
 	c_2 & a_{21} & 0 \\
 	\hline
    & b_1 & b_2 
\end{array} \quad (c_2 = a_{21}).
$$
 
Order conditions:
 
$$
b_1 + b_2 = 1, \; b_2 c_2 = \frac{1}{2}
$$
 
Choose a free parameter $$ c_2 = \alpha \neq 0 $$. Then
 
$$
b_2 = \frac{1}{2 \alpha}, \; b_1 = 1 - \frac{1}{2 \alpha}
$$
 
Thus the Butcher tableau of the 2-stage explicit RK is
 
$$
\begin{array}{c|cc}
 	0 & 0 & 0 \\
 	\alpha & \alpha & 0 \\
 	\hline
 	& 1 - \frac{1}{2 \alpha} & \frac{1}{2 \alpha}
\end{array} \quad (\alpha \neq 0)
$$
 
Stage derivatives:
 
$$
\begin{align*}
    k_1 &= f(t_n, y_n) \\
 	k_2 &= f(t_n + \alpha h, y_n + \alpha h k_1)
\end{align*}
$$
 
when $$ \alpha = \frac{1}{2} $$, we recover the midpoint method:
 
$$
y_{n+1} = y_n + h f(t_n + \frac{1}{2}h, y_n + \frac{1}{2}h f(t_n, y_n))
$$
 
when $$ \alpha = 1 $$, we recover Heun's method (improved Euler):
 
$$
y_{n+1} =  y_n + \frac{h}{2}\left(f(t_n, y_n) + f(t_n +h, y_n + hf(t_n, y_n)) \right)
$$

Last, let us take a look at an implicit RK2 method (i.e., implicit trapezoidal method or Crank-Nicolson method):
 
Butcher tableau:
 
$$
\begin{array}{c|cc}
 	0 & 0 & 0\\
 	1 & \frac{1}{2} & \frac{1}{2}\\
 	\hline
 	& \frac{1}{2} & \frac{1}{2}
\end{array}
$$
 
Update equations:
 
$$
\begin{align*}
    k_1 &= f(t_n, y_n) \\
 	k_2 &= f(t_n + h, y_n +h(\frac{1}{2}k_1 + \frac{1}{2}k_2))\\
 	y_{n+1} &= y_n + \frac{h}{2}(k_1 + k_2)
\end{align*}
$$
 
We can verify order conditions
 
$$
\begin{align*}
    \sum_i b_i &= \frac{1}{2} + \frac{1}{2} = 1 \\
 	\sum_i b_i c_i &= \frac{1}{2} \cdot 0 + \frac{1}{2} \cdot 1 = \frac{1}{2}
\end{align*}
$$

## Concluding Remarks

Voil\`{a}! This concludes a concise yet mathematically rigorous introduction to general one-step methods for numerically solving IVPs of ODEs. We have carefully derived the key convergence theorem—that consistency + stability (Lipschitz continuity) $$ \implies $$ convergence—and established the order conditions (up to order four) for general Runge–Kutta (RK) methods.

As an aside, current large language models (LLMs) still tend to struggle with the detailed derivation of higher-order RK conditions, particularly beyond third order. For instance, expanding the stage derivatives $$ k_i = f \left(t_n + c_i h, y_n + h \sum_{j=1}^s a_{ij}k_j \right) $$ to order four using multivariate Taylor series, as we did above, is challenging for LLMs. I suspect this limitation arises from the relative scarcity of third-order differential terms (i.e., $$ f_{ttt}, f_{tty}f, f_{tyy}(f, f), f_{yyy}(f, f, f), f_y f_{tt}, f_y f_{ty} f,f_y f_{yy}(f, f), f_y f_y f_t, f_y f_y f_y f $$) in their pre-training corpora. In such cases, symbolic computation systems, also known as computer algebra systems (CAS), remain more suitable tools for handling the intricate algebraic manipulations required. Nevertheless, I believe that as long as LLMs can function as faithful interpolating functions of existing knowledge, they hold immense potential to transform the educational landscape, particularly by democratising access to advanced mathematical and scientific knowledge.

## Acknowledgement

This blog was developed from a self-contained, step-by-step tutorial generated by OpenAI ChatGPT, which also assisted in refining the phrasing and expressions, as well as in suggesting suitable titles.

{% raw %}

```html
@misc{Cai2025odelearningnotes-2,
    author = {Xin Cai},
    title = {Reconstructing the Scaffolding A Learner’s Notes on Ordinary Differential Equations: Part 2 Convergence and Order in One-Step (Runge–Kutta) Methods},
    howpublished = {\url{https://totalvariation.github.io/blog/2025/concise-intro-odes-part2/}},
    note = {Accessed: 2025-10-28},
    year = {2025}
}
```

{% endraw %}

