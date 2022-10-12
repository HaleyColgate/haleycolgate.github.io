---
layout: default
title: Identification 
--- 

Note: $$Y_{i}(m_{\leq j}) := Y_i(m_1,\dots,m_j)$$.

We intervene on each MDP independently, leading to the following SWIG:

![Directed Acyclic Graph](/assets/images/swig.png)

For $$i\in[T]$$, we need a positivity assumption that $$P(M_i=m_i)>0$$ and $$P(M_i=m_i')>0$$, a consistency assumption that $$Y_i(M_{\leq i}) = Y_i$$, and the standard SUTVA assumption that the mapping $$(m_{\leq i}) \mapsto Y_i(m_{\leq i})$$ is well defined.


This allows us to conclude that $$M_i\perp \!\!\! \perp Y_j(m_{\leq j})$$, and more importantly, $$M_i\perp \!\!\! \perp Y_j(M_{\leq j-1}, m_j)$$ for any $$i,j\in[T]$$.

We therefore get via linearity

$$\mathbb{E}[Y_T(M_{\leq T-1}, m_T) - Y_T(M_{\leq T-1}, m_T')] = \mathbb{E}[Y_T(M_{\leq T-1}, m_T)] - \mathbb{E}[Y_T(M_{\leq T-1}, m_T')].$$

Then, since $$M_T\perp \!\!\! \perp Y_T(M_{\leq T-1}, m_T)$$ and $$M_T\perp \!\!\! \perp Y_T(M_{\leq T-1}, m_T')$$ we get

$$ = \mathbb{E}[Y_T(M_{\leq T-1}, m_T) \mid M_T=m_t] - \mathbb{E}[Y_T(M_{\leq T-1}, m_T') \mid M_T=m_t'] .$$

Note these conditionals are well defined via our positivity assumption. This conditioning gives us

$$ = \mathbb{E}[Y_T(M_{\leq T}) \mid M_T=m_t] - \mathbb{E}[Y_T(M_{\leq T}) \mid M_T=m_t'].$$

So by consistency we arrive at

$$= \mathbb{E}[Y_T \mid M_T=m_t] - \mathbb{E}[Y_T \mid M_T=m_t']$$

which is a statistical estimand we can estimate using the data.
