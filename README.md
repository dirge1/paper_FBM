# Reliability modeling and statistical analysis of accelerated degradation process with memory effects and unit-to-unit variability

\**Shi-Shun Chen a, Xiao-Yang Li a, ***, Wen-Rui Xie b**

a School of Reliability and Systems Engineering, Beihang University, Beijing 100191, China\
b School of Mathematics, Jilin University, Changchun 130012, China

Email: [css1107@buaa.edu.cn](mailto:css1107@buaa.edu.cn), [leexy@buaa.edu.cn](mailto:leexy@buaa.edu.cn), [xiewr22@mails.jlu.edu.cn](mailto:xiewr22@mails.jlu.edu.cn)

*Corresponding author*: Xiao-Yang Li

---

## Abstract

A reasonable description of the degradation process is essential for credible reliability assessment in accelerated degradation testing. Existing methods usually use Markovian stochastic processes to describe the degradation process. However, degradation processes of some products are non-Markovian due to the interaction with environments. Misinterpretation of the degradation pattern may lead to biased reliability evaluations. Besides, owing to the differences in materials and manufacturing processes, products from the same population exhibit diverse degradation paths, further increasing the difficulty of accurate reliability estimation.

To address the above issues, this paper proposes an accelerated degradation model incorporating memory effects and unit-to-unit variability. The memory effect in the degradation process is captured by the fractional Brownian motion, which reflects the non-Markovian characteristic of degradation. The unit-to-unit variability is considered in the acceleration model to describe diverse degradation paths. Then, lifetime and reliability under normal operating conditions are presented. Furthermore, to give an accurate estimation of the memory effect, a new statistical analysis method based on the expectation maximization algorithm is devised. The effectiveness of the proposed method is verified by a simulation case and a real-world tuner reliability analysis case.

The simulation case shows that the estimation of the memory effect obtained by the proposed statistical analysis method is much more accurate than the traditional one. Moreover, ignoring unit-to-unit variability can lead to a highly biased estimation of the memory effect and reliability. From the tuner reliability analysis case, the proposed model is superior in both deterministic degradation trend predictions and degradation boundary quantification compared to existing models, which can provide more credible reliability assessment.

The code for the simulation case is publicly available at [GitHub](https://github.com/dirge1/FBM_ADT).

**Keywords:** Accelerated degradation testing; Fractional Brownian motion; Unit-to-unit variability; EM algorithm; Memory effect; Tuner reliability analysis.

---

## Highlights

- A non-Markovian accelerated degradation model with memory effect is developed.
- Unit-to-unit variability is considered in the accelerated degradation model.
- Ignoring unit-to-unit variability leads to biased estimation of the memory effect.
- A statistical analysis method is proposed via expectation maximization algorithm.
- The proposed statistical analysis method gives a more accurate estimation.

---

## Introduction

With the continuous progress of design and manufacturing technology, modern products tend to have extremely high reliability and long lifetime. At this point, accelerated degradation testing (ADT) is always employed, in which degradation data are obtained under more severe stress conditions. By modeling ADT data, the lifetime and reliability of products under normal conditions can be extrapolated and the saving of cost and time can be achieved.

In ADT modeling, a reasonable description of the degradation process is vital to support credible lifetime and reliability assessments [1]. Most existing ADT models hold a general assumption that performance degradation is a memoryless Markovian process with independent increments [2-4]. However, for real engineering products, such as batteries or blast furnace walls [5], degradation typically exhibits non-Markovian properties due to their interaction with environments [6], i.e., the future degradation increment is influenced by the current or historical states.

Since traditional Markovian models are inherently memoryless, they fail to describe the degradation of such dynamic systems, leading to an imprecise lifetime and reliability assessments [7]. Hence, building up an ADT model considering the non-Markovian degradation is essential for enabling accurate extrapolation of lifetime and reliability.

## Preliminary to Fractional Brownian Motion

Let a standard FBM denoted by \( B_H(t) \), for a given time \( t \), \( B_H(t) \) can be defined as:

\[ B_H(t) = \frac{1}{Γ(H+1/2)} \int_0^t (t-s)^{H-1/2} dB(s) \]

where

- \( Γ(\cdot) \) is the gamma function formulated by
  
  \[ Γ(x) = \int_0^\infty t^{x-1} e^{-t} dt \]

- \( B(\cdot) \) represents the standard Brownian motion;
