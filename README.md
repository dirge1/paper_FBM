# paper_FBM
Reliability modeling and statistical analysis of accelerated degradation testing with memory effects and unit-to-unit variability

Abstract

A reasonable description of the degradation process is essential for credible reliability assessment in accelerated degradation testing. Existing methods usually use Markovian stochastic processes to describe the degradation process. However, degradation processes of some products are non-Markovian due to the interaction with environments. Misinterpretation of the degradation pattern may lead to biased reliability evaluations. Besides, owing to the differences in materials and manufacturing processes, products from the same population exhibit diverse degradation paths, further increasing the difficulty of accurate reliability estimation. To address the above issues, this paper proposes an accelerated degradation model incorporating memory effects and unit-to-unit variability. The memory effect in the degradation process is captured by the fractional Brownian motion, which reflects the non-Markovian characteristic of degradation. The unit-to-unit variability is considered in the acceleration model to describe diverse degradation paths. Then, lifetime and reliability under normal operating conditions are presented. Furthermore, to give an accurate estimation of the memory effect, a new statistical analysis method based on the expectation maximization algorithm is devised. The effectiveness of the proposed method is verified by a simulation case and a real-world tuner reliability analysis case. The simulation case shows that the estimation of the memory effect obtained by the proposed statistical analysis method is much more accurate than the traditional one. Moreover, ignoring unit-to-unit variability can lead to a highly biased estimation of the memory effect and reliability. From the tuner reliability analysis case, the proposed model is superior in both deterministic degradation trend predictions and degradation boundary quantification compared to existing models, which can provide more credible reliability assessment. The code for the simulation case is publicly available at https://github.com/dirge1/FBM_ADT.


Keywords: Accelerated degradation testing; Fractional Brownian motion; Unit-to-unit variability; EM algorithm; Memory effect; Tuner reliability analysis.

Highlights

A non-Markovian accelerated degradation model with memory effect is developed.
Unit-to-unit variability is considered in the accelerated degradation model.
Ignoring unit-to-unit variability leads to biased estimation of the memory effect.
A statistical analysis method is proposed via expectation maximization algorithm.
The proposed statistical analysis method gives a more accurate estimation.

 
Introduction


With the continuous progress of design and manufacturing technology, modern products tend to have extremely high reliability and long lifetime. At this point, accelerated degradation testing (ADT) is always employed, in which degradation data are obtained under more severe stress conditions. By modeling ADT data, the lifetime and reliability of products under normal conditions can be extrapolated and the saving of cost and time can be achieved. In ADT modeling, a reasonable description of the degradation process is vital to support credible lifetime and reliability assessments [1]. Most existing ADT models hold a general assumption that performance degradation is a memoryless Markovian process with independent increments [2-4]. However, for real engineering products, such as batteries or blast furnace walls [5], degradation typically exhibits non-Markovian properties due to their interaction with environments [6], i.e., the future degradation increment is influenced by the current or historical states. Since traditional Markovian models are inherently memoryless, they fail to describe the degradation of such dynamic systems, leading to an imprecise lifetime and reliability assessments [7]. Hence, building up an ADT model considering the non-Markovian degradation is essential for enabling accurate extrapolation of lifetime and reliability.


To describe the non-Markovian property of a degradation process, some research introduced a state-dependent function to quantify the influence of the current state on the degradation rate and established different transformed stochastic processes. For instance, Giorgio et al. [8] established the transformed gamma process and derived the conditional distribution of the first passage time. Following this, Giorgio and Pulcini [9] further proposed the transformed Wiener process to describe the non-Markovian degradation process with possibly negative increments. Also, the transformed inverse Gaussian process [10] and the transformed exponential dispersion process [11] were presented and the corresponding parameter estimation methods based on the Bayesian framework were developed. However, for the above transformed stochastic processes, selecting a sensible form of the state-dependent function is challenging in the absence of prior knowledge. Besides, these models only focus on the influence of the current state on the future degradation increment, which may not be sufficient for describing the memory effects of the entire degradation path.


Different from the state-dependent function, fractional Brownian motion (FBM) serves as a proficient mathematical tool for modeling non-Markovian stochastic processes incorporating memory effects [12, 13]. In an FBM, memory effects can be simply quantified by the Hurst exponent. To this end, FBM has drawn much attention in degradation modeling and had widespread applications in remaining useful life (RUL) prediction [14]. Xi et al. [15] first introduced the FBM to capture the memory effects in the degradation process and predicted the RUL of a turbofan engine. Afterward, Xi et al. [16] proposed an improved degradation model by considering both the memory effect and unit-to-unit variability (UtUV). Shao and Si [17] extended the degradation model based on the FBM by considering measurement errors. Xi et al. [18] further developed a multivariate degradation model based on the FBM to describe multivariate stochastic degradation systems with memory effects. The above studies found that compared to the Wiener process-based Markovian degradation models, the FBM-based non-Markovian ones can help for obtaining more accurate RUL predictions when facing degradation with memory effects.


Despite substantial progress in degradation modeling achieved by introducing FBM, their studies ignore the effect of external stresses which are key factors influencing performance degradation. As a result, their modeling is limited to degradation under constant stress levels. In an ADT, degradation data are collected from different stress levels. In order to extrapolate reliability under the normal stress level, the effect of external stresses on performance degradation needs to be quantified accurately. For this purpose, Si et al. [7] developed an ADT model based on the FBM, which incorporated external stresses into degradation modeling. However, there still exists research gaps. In general, the reliability evaluation accuracy of the ADT model depends not only on the precise description of the degradation process, but also on proper uncertainty quantification. In an ADT, we can observe that different items exhibit diverse degradation paths, denoted as UtUV. UtUV stems from variations in materials and manufacturing among different products in the population. Unfortunately, the existing FBM-based ADT model ignores UtUV. Therefore, their model can only represent the degradation paths of the observed samples, rather than the overall degradation pattern of the population. The author [19] and other scholars [20, 21] have also shown that ignoring UtUV in ADT modeling results in unreliable reliability assessments in practical applications.


Apart from ADT modeling, performing statistical analysis on the ADT data to estimate unknown parameters is also a crucial step. When estimating the unknown parameters in ADT models with UtUV, it is always challenging to get a solution by direct constrained optimization of the overall likelihood function since the source of effect on degradation is diverse (i.e., external stresses, time and uncertain degradation rate). In the literature, a two-step maximum likelihood estimation (MLE) method was generally used to estimate unknown parameters in the Wiener-process based ADT models with UtUV [22, 23]. However, the two-step MLE method maximizes two partial likelihood functions separately and fails to guarantee the maximization of the overall likelihood function. This exists a risk of deviating from the real law of actual degradation data when dealing with ADT models with UtUV, especially when the memory effect is incorporated. As demonstrated in the following Section 4.3.1, the estimation of the memory effect obtained by the two-step MLE method is highly biased, which leads to misjudgment of the degradation law and significant deviations in reliability evaluations.


Motivated by the above limitations, a comprehensive degradation model considering memory effects, UtUV and external stresses simultaneously is presented. In addition, a new statistical analysis method is proposed based on the expectation maximization (EM) algorithm [24], where the maximization of the overall likelihood function can be implemented. Compared to existing studies, although Xi et al. [16] has proposed a FBM-based degradation model incorporating UtUV, their model ignored the effect of external stresses on degradation. Therefore, extrapolation of the product reliability under other stress levels cannot be achieved. For the ADT model proposed in [7], UtUV was ignored in their model, which could result in biased reliability evaluations in practical applications. Moreover, when external stresses and UtUV are both considered in the FBM-based degradation model, the statistical analysis method used in [16] and [7] has difficulty in obtaining a solution, requiring exploration of new statistical analysis methods.


The main contributions of this paper are summarized as follows:
	An improved FBM-based degradation model is proposed for reliability estimation of ADT data with memory effects, in which the influence of external stresses are incorporated and the UtUV is quantified.
	A statistical analysis method is presented based on the EM algorithm, in which the accurate estimation of the memory effect is achieved by maximizing the overall likelihood function rather than maximizing two partial likelihood functions separately.
	The superiority of the proposed ADT model and statistical analysis method is verified by comparing existing methods in a numerical case and a real-world tuner reliability analysis case.

 
The organization of the paper is as follows. Firstly, Section 2 gives the preliminary about the FBM. Then, in Section 3, an ADT model with memory effects and UtUV is proposed, and a statistical analysis method based on the EM algorithm is presented. After that, in Section 4 and Section 5, the practicability and the superiority of the proposed methodology are verified by a simulation case and an engineering case, respectively. Finally, we conclude our work in Section 6.


Preliminary to fractional Brownian motion
Let a standard FBM denoted by  , for a given time "t ≥ 0" ,   can be defined as [25]:
	 	(1)
where
	 	(2)
Γ(•) is the gamma function formulated by
	 	(3)
B(•) represents the standard Brownian motion; and H is the Hurst exponent with the range of (0, 1). For the standard FBM shown in Eq. (1), the autocorrelation function can be expressed by
	 	(4)
It can be seen from Eq. (4) that the memory effect is determined by H. Depending on the value of H, there are three different cases [16]:
	When "0 < H < 0.5" , the increments of FBM are negatively correlated, which implies that future increments tend to an opposite tendency of the previous direction and the increment process has short-term memory effect.
	When "H = 0.5" , the FBM degenerates into the BM, and there is no memory effect in the increment process.
	When "0.5 < H < 1" , the increments of FBM are positively correlated, which implies that future increments tend to follow the previous direction and the increment process has long-term memory effect.
We can see that the standard BM is a special case of the standard FBM. In addition, for "t ≥ 0" , the standard FBM exhibits the following properties:
	 ;
	  and  ;
	  for any "r < t" ;
	  for "a > 0" .
Methodology
Model construction
In order to describe the non-Markovian degradation process with memory effects, a standard FBM shown in Eq. (1) is introduced to construct the ADT model as follows:
	 	(5)
where X(•) denotes the performance degradation amount; f (•) is a function of stress s and time t, which depicts the deterministic performance degradation trend; σ represents the diffusion coefficient.
Generally speaking, f(•) in Eq. (5) can be decoupled as a performance degradation rate function e(s) multiplied by a time scale function τ(t) [19], i.e.:
	 	(6)
where "τ(t) = " "t" ^"β" , "β > 0"  is a common assumption for the timescale function [23]. If "β = 1" , then it represents a linear degradation process, otherwise a nonlinear degradation process; e(s) indicates the relationship between degradation rate and external stress (also known as the acceleration model) [26].
In general, e(s) is usually assumed as a log-linear function as follows [27]:
	 	(7)
where "α" _"0"  and "α" _"1"  are constant unknown parameters; s* is the standardized stress. For different type of acceleration models, s* can be calculated as [28]:
	 	(8)
where s0 denotes the normal stress level; sH denotes the highest stress levels. The choice of acceleration models mainly depends on the type of accelerated stress. For instance, the Arrhenius model is adopted if the accelerated stress is temperature; if the accelerated stress is humidity, the Power law model is employed [29].
As mentioned in the introduction, UtUV plays a significant role in ADT modeling. To be specific, for items under the same stress level, each item may have its own performance degradation rate due to manufacturing imperfections. As a result, it is reasonable to deduce that each item has unique e(s). Without loss of generality, we make the assumption that the degradation rates of various items adhere to a normal probability distribution. Thus, Eq. (6) can be transformed as:
	 	(9)
Given that e(s) is a random variable, the degradation rate function in Eq. (7) can be reformulated, as indicated by
	 	(10)
where   is a random variable with a normal probability distribution,  . Then, the distribution of e(s) in Eq. (9) can be derived as:
	 	(11)
Lifetime distribution and reliability analysis
The main purpose of ADT is to extrapolate the lifetime and reliability of the product at normal stress s0 by using the degradation data subjected to high stress levels. In accordance with the definition of reliability within belief reliability theory [30], a product is considered to fail when its performance margin less than 0. Since X denotes the performance degradation amount, the performance margin M can be defined as:
	 	(12)
where Xth denotes the critical threshold of the degradation process. Then, the failure time T of a product can be expressed as:
	 	(13)
and the reliability of a product R can be expressed as:
	 	(14)
where Pr{•} denotes the probability measure.
For the FBM-based degradation model with UtUV, Xi et al. [16] have developed an approximate analytical lifetime distribution by utilizing weighted random sums. However, the approximate lifetime distribution is only suitable for the degradation process with long-term memory effect, i.e., H>0.5. When the degradation process exhibits short-term dependency, the approximate lifetime distribution may lead to biased reliability evaluations. Therefore, we employ the Monte Carlo (MC) method to calculate the lifetime distribution and reliability under specified operating conditions. This approach is not restricted by the type of memory effect. Algorithm 1 gives the detailed procedures of the MC method.

Algorithm 1: The MC method to calculate the product lifetime distribution and reliability under specified operating conditions
Step 1. Determine the model parameters   based on the EM algorithm in the subsequent Section 3.3;
Step 2. According to  , simulate N degradation trajectories under specified stress sp. The qth degradation trajectory can be obtained as:
2.1 Simulate the qth standard FBM   using the FFT algorithm [31] (Detailed steps of the FFT algorithm can be found in [7]);
2.2 Generate a unit-specific aq according to its distribution  ;
2.3 Acquire the degradation trajectory as  .
Step 3. Calculate the product lifetime distribution and reliability under specified stress sp:
3.1 Calculate the failure time of the N simulated degradation trajectories under stress sp by applying Eq. (13), denoted as "T" _"q"  " (1 ≤ q ≤ N)" ;
3.2 Derive the empirical lifetime distribution under stress sp as ; ;
3.3 Calculate the product reliability at the specific time t using Eq. (14).

Statistical analysis
In this paper, we consider a constant stress ADT, which is generally used in practice. The observed ADT data xlij is denoted as the jth degradation value of unit i subjected to the lth stress level, l = 1, 2, ..., k, i = 1, 2, ..., nl, j = 1, 2, ..., mli, where k is the number of stress levels, nl is the number of test items under the lth stress level, and mli denotes the number of measurements for unit i subjected to the lth stress level. The corresponding measurement time is represented by tlij. Hereby, the degradation observations of the ith item tested at lth stress level can be denoted as  . Furthermore, we define   and  .
According to Eqs. (9) and (10), the unknown parameters need to be determined are  . In this section, a statistical analysis approach based on the EM algorithm is presented.
Since each item has a unique degradation rate, for the ith item tested at lth stress level, we have
	 	(15)
where
	 	(16)
The basic idea of the EM algorithm involves replacing unobservable variables with their conditional expectations, where parameter updates at each step can be derived in a closed or simple form [32]. The EM algorithm mainly comprises two steps. Firstly, the expectation of the log-likelihood is calculated with respect to the latent variables, which is called the expectation-step (E-step). Then, the maximizer of this expected likelihood is identified, which is called the maximization step (M-step). These two steps are iteratively repeated until reaching a satisfactory level of convergence.
In Eq. (15), ali is the unobservable variable and needs to be replaced. We denote  . Subsequently, when provided with the complete data including x and Ω, we can formulate the complete log-likelihood function as:
	 	(17)
where   denotes the probability density function (PDF) of xli given ali and θ; and   denotes the PDF of ali given θ. According to the ADT models Eqs. (9) and (11), Eq. (17) can be extended as:
	 	(18)
where Qli is a Kli × Kli dimensional covariance matrix and its (u, v)th entry can be calculated by
	 	(19)
To facilitate the following statistical analysis, a re-parameterization of the unknown parameter is performed by  . Then, Eq. (18) can be rewritten as:
	 	(20)
Considering the update of θ during the iterations in the EM algorithm, let  represent the estimations in the pth step. To calculate the expectation of the log-likelihood function Eq. (20), it is necessary to derive the first and second moments of ali conditional on   and  . Notably, it is evident that the distribution of ali conditional on   and   maintains a normal distribution [33]. Within the Bayesian framework, the posterior distribution of ali can be acquired as:
	 		(21)
with
	 	(22)
	 	(23)
Subsequently, we employ the EM algorithm to estimate θ iteratively. Firstly, the expectation of   with regard to Ω is computed in the E-step. According to Eqs. (20), (22) and (23), we can get
	 	(24)
In the M-step, the first partial derivatives of   with respect to  ,   and   can be derived, respectively. Then, by equating each derivative to zero, we can obtain
	 	(25)
	 	(26)
	 	(27)
Note that   and   can be directly determined based on Eqs. (22) and (23), while   depends on the values of  ,   and  . Upon substitution of  ,   and   into Eq. (24), we get the profile likelihood function as:
	 	(28)
Then, by maximizing the profile likelihood function, the values of  ,   and   can be obtained. Subsequently, the value of   can be determined by substituting  ,   and   into Eq. (27). The iterations are typically stopped when the relative change in parameter estimations drops below a predefined threshold. The complete algorithm is outlined in Algorithm 2.

Algorithm 2: Parameter Estimations Using the EM Algorithm
Step 1. Initialize   and  . Set "p = 0" .
Step 2. Get   by using the EM algorithm
E-step:
2.1 Calculate   and   by using Eqs. (22) and (23);
M-step:
2.2 Update   and   by using Eqs. (25) and (26);
2.3 Update  ,   and   by maximizing the profile likelihood function Eq. (28) through a three-dimensional search;
2.4 Update   by substituting  ,   and   into Eq. (27).
Step 3. If  , obtain  . Otherwise, go to Step 2.

In addition to the point estimation of θ derived from Algorithm 2, it is equally important to consider the interval estimation of θ. Interval estimation provides a range of plausible values within which the true parameter is likely to lie, offering a complete understanding of the estimation uncertainty. While the asymptotic normality of maximum likelihood function is commonly used for interval estimation, deriving the expected Fisher information matrix of the proposed model is challenging due to the existence of memory effects, stress acceleration, and UtUV. Thus, non-parametric bootstrap method is employed to acquire the interval estimation. Since the degradation processes of different items are independent, a resampling procedure with replacement is carried out on the degradation processes of items under each stress condition. Then,   of the resampled data is obtained based on the EM algorithm. After repeating this procedure M times, we get M bootstrap estimates for different resampled data, denoted as  . Then, the interval estimation at a given confidence level (CL) for each parameter can be derived by calculating the percentiles from  . Besides, the interval estimation of reliability can be derived from  . The complete procedure for interval estimation is outlined in Algorithm 3.

Algorithm 3: Interval Estimation of Parameters and Reliability Using the Non-parametric Bootstrap
Step 1. Obtain M bootstrap estimates of θ based on the original ADT data. The qth estimate   can be obtained as:
1.1 Get the resampled degradation data across all the stress levels. For the lth stress level, randomly sample nl items with replacement from the original items under the lth stress level.
1.2 Acquire the estimate   of the resampled data based on Algorithm 2.
Step 2. Obtain the interval estimation of each parameter in θ at a specific (1−α) CL. For the pth parameter in θ, which is denoted as  , its interval estimation can be derived as:
2.1 Sort all the estimates of   from smallest to largest based on  .
2.2 The interval estimation of   at (1−α) CL is given by  , where   and   are the (100α/2)th and [100(1−α/2)]th percentiles of the sorted parameter estimate, respectively.
Step 3. Calculate the interval estimation of reliability at a specific (1−α) CL:
3.1 Obtain M reliability estimations according to Algorithm 1 based on M bootstrap estimates of θ, denoted as  .
3.2 Sort the reliability estimation from smallest to largest based on  .
3.3 The interval estimation of reliability at (1−α) CL is given by  , where   and   are the (100α/2)th and [100(1−α/2)]th percentiles of the sorted reliability estimation, respectively.
Simulation study
In this section, the proposed method is illustrated through a numerical simulation case. Furthermore, the superiority of the proposed parameter estimation method and the significance of the proposed ADT model are demonstrated.
Simulation settings
Detailed information on the constant-stress ADT simulation case is shown in Table 1. The degradation process exhibits short-term dependency because "H = 0.1" . Specifically, the sample sizes for each stress level (N) are set as 6 and the measurements for each sample (M) are set as 10.

Table 1  Information for simulation configuration.
Content	Values
Accelerated stress level (Temperature/°C)	80, 100, 120
Normal stress level (°C)	40
Degradation model	 

Acceleration model	Arrhenius model
Parameter value:  
1e-5, 2e-6, 2.5, 1.5, 0.1, 0.1
Inspection interval (h)	100
Failure threshold	5
Results and analysis
In this case, the initial value of θ is taken from the estimations of the two-step MLE method. The implementation details of the two-step MLE method are given in the Appendix. The terminated threshold   for the EM algorithm iteration is set as 0.01, and the number of bootstrap estimates M is set as 1000. Then, according to Algorithm 2 and Algorithm 3, we obtain the point and interval estimations of unknown parameters at 90% CL as shown in Table 2. It can be seen that the point estimations are basically close to the true values and the true values lie within the region of interval, which shows the effectiveness of the proposed statistical analysis method.

Table 2  Point and interval estimation results of θ in the simulation case.
Parameters	Point estimations	90% confidence interval estimations
μa	1.293e-5	(7.761e-6, 2.049e-5)
σa	2.269e-6	(1.007e-6, 3.826e-6)
α1	2.700	(2.333, 3.052)
β	1.452	(1.398, 1.513)
σ	0.081	(0.058, 0.133)
H	0.117	(0.012, 0.181)

Furthermore, to show the effectiveness of the ADT model, the predicted deterministic degradation trends based on the point estimations, along with the upper and lower boundaries at 90% CL derived from 10000 simulated degradation paths are compared with the observed data. As depicted in Fig. 1, the predicted deterministic degradation trends primarily lie in the observed data across all accelerated stress levels, and the boundaries nicely envelope the observations. This indicates that the proposed ADT model is capable of describing the degradation law under different stress levels.
  
                                          (a)                                                                             (b)
 
                                                                                    (c) 
Fig. 1  Deterministic degradation trend, boundaries and observations: (a) 80°C (b) 100°C (c) 120°C.

Next, when the critical threshold Xth is 5, we can calculate the point and interval estimation of reliability under the normal use condition (40°C) according to Algorithm 1 and Algorithm 3, as shown in Fig. 2. From Fig. 2, the reliability of the product for a given operating time can be obtained. For example, when the product is operated for 4200 hours, the lower boundary of reliability at 90% CL is 0.99. This result can provide guidance for the development of maintenance and warranty strategies.
 
Fig. 2  Reliability of the product under normal stress level (40°C).

Remark 1: It is worth mentioning that the sources of uncertainty for the degradation and reliability confidence intervals in Figs. 1 and 2 are different. For the degradation prediction in Fig. 1, the uncertainty is originated from FBM and UtUV in the ADT model, which describes the inherent uncertainty of the degradation process for a batch of products. This type of uncertainty is objective and cannot be eliminated. On the other hand, the uncertainty of reliability estimation in Fig. 2 is originated from the uncertainty in parameter estimations. This type of uncertainty is mainly caused by insufficient sample size. In general, increasing sample size will narrow the range of confidence intervals. In Fig. 1, the goal of quantifying the inherent uncertainty for a batch of products is to compare it with the dispersion of the real data, thereby validating the effectiveness of the ADT model in uncertainty quantification. Whereas in Fig. 2, quantifying the uncertainty of the parameter estimates is chosen in order to prevent the risk of decision making due to estimation bias.
Discussions
Comparison of statistical analysis methods
In order to illustrate the superiority of the proposed statistical analysis method compared to the existing two-step MLE method, in this subsection, nine combinations of (N,M) are chosen to generate simulated ADT data. Other simulation settings are the same as those in Table 1. Considering the randomness of data generation, we simulated ADT data 1000 times for each combination and obtain the mean values of parameter estimations. Then, the relative error (RE) between the estimations and the true values is adopted as a metric to compare the performance of both methods. The RE of the parameter estimation is given by
	 	(29)
Table 3 gives the mean values of unknown parameter estimations obtained by two methods under nine combinations of sample sizes and measurements, and Fig. 3 illustrates the RE for two methods under different combinations. From Table 3 and Fig. 3, we can get the following results:
	For the two-step MLE method, when the number of measurements is limited, the estimation bias of H is significantly high, reaching nearly 100%. As the number of measurements increases, the estimate of H approaches the true value but remains unsatisfactory, with a bias around 40%. On the other hand, for the proposed statistical analysis method based on the EM algorithm, even with a limited number of measurements, the estimated value of H remains very close to the true value, with a bias of less than 5%. This demonstrates the crucial role of maximizing the overall likelihood function for estimating the memory effect accurately, particularly when measurements are scarce.
	With the increase in sample size and the number of measurements, the RE obtained by the EM algorithm decreases, demonstrating that increasing information in ADT experiments can enhance the precision of parameter estimation effectively.
	In all combinations, the RE obtained by the EM algorithm is much better than those of the two-step MLE method, proving its superiority in estimating the parameters of the ADT model with memory effects and UtUV.

Table 3  Mean value of parameter estimations for two methods under nine combinations of sample sizes and measurements.
(N,M)	Method	μa	σa	α1	β	σ	H	RE
(6,10)	S1	1.100e-5	2.221e-6	2.469	1.5	0.157	0.001	1.779
	S2	1.102e-5	1.998e-6	2.483	1.498	0.106	0.095	0.235
(6,20)	S1	1.041e-5	1.938e-6	2.493	1.5	0.144	0.03	1.209
	S2	1.041e-5	1.918e-6	2.493	1.5	0.103	0.095	0.168
(6,30)	S1	1.027e-5	2.553e-6	2.493	1.5	0.124	0.059	0.959
	S2	1.027e-5	1.925e-6	2.493	1.5	0.102	0.097	0.116
(12,10)	S1	1.068e-5	2.257e-6	2.477	1.499	0.158	4.484e-5	1.790
	S2	1.070e-5	2.049e-6	2.489	1.497	0.105	0.095	0.134
(12,20)	S1	1.016e-5	1.848e-6	2.497	1.5	0.142	0.032	1.192
	S2	1.016e-5	1.959e-6	2.497	1.5	0.102	0.098	0.074
(12,30)	S1	1.016e-5	2.288e-6	2.494	1.5	0.122	0.061	0.772
	S2	1.016e-5	1.968e-6	2.494	1.5	0.101	0.099	0.055
(18,10)	S1	1.064e-5	2.270e-6	2.466	1.499	0.159	1.531e-8	1.802
	S2	1.067e-5	2.066e-6	2.481	1.497	0.102	0.098	0.151
(18,20)	S1	1.013e-5	2.007e-6	2.497	1.5	0.141	0.033	1.101
	S2	1.013e-5	1.981e-6	2.497	1.5	0.101	0.099	0.049
(18,30)	S1	1.003e-5	1.968e-6	2.506	1.5	0.122	0.061	0.624
	S2	1.003e-5	1.960e-6	2.506	1.5	0.101	0.099	0.044
Note: S1 represents the two-step MLE method, and S2 represents the proposed statistical analysis method based on the EM algorithm.
 
Fig. 3  RE for parameter estimations of two statistical analysis methods under nine combinations of sample sizes and measurements.
Comparison of ADT models
The proposed ADT model is described by Eqs. (8), (9), and (11), which is denoted as M0. It should be noted that the proposed ADT model has three special cases as follows:
	When the UtUV is not considered, i.e., a is a constant, it degenerates into the model in [7], which is denoted as M1.
	 	(30)
	When the memory effects in the degradation process are not considered, i.e., "H = 0.5" , it degenerates into the model in [22], which is denoted as M2.
	 	(31)
	When the UtUV and the memory effects are both not considered, i.e., a is a constant and "H = 0.5" , it degenerates into the model in [34], which is denoted as M3.
	 	(32)
In order to illustrate the superiority of the proposed ADT model compared to existing ADT models, in this subsection, we set the combination of (N,M) as (12,30) to generate ADT simulation data. Other simulation settings are the same as those in Table 1. Considering the randomness of data generation, we simulated ADT data 1000 times for each combination and obtain the mean values of parameter estimations. For the model M0 and M2, the unknown parameters are estimated based on the EM algorithm. For the model M1 and M3, the parameters are obtained by directly maximizing the log-likelihood function. Then, we calculate the maximum value of the log-likelihood function lmax and the Akaike information criterion (AIC) for each ADT model. The AIC is expressed as [35]
	 	(33)
where np is the number of unknown parameters. The smaller the AIC, the better the model fits.
Table 4 gives the mean values of unknown parameter estimations for the four ADT models. From lmax and AIC values, M0 is the most effective model for describing the ADT data, followed by model M2 and M1, and model M3 is the worst. Analyzing the reasons, the model M2 and M1 partially consider the UtUV and the memory effects, respectively. Therefore, they both perform worse than the model M0, which considers the UtUV and the memory effects simultaneously. Moreover, model M1 even gets wrong degradation law, i.e., the degradation process exhibits long-term dependency, which is due to the lack of consideration of UtUV. As for the model M3, both the UtUV and the memory effects are not considered, which leads to the poorest fitting results.

Table 4  Mean value of parameter estimations for the four ADT models.
Model	μa	σa	α1	β	σ	H	lmax	AIC
M0	1.003e-5	1.960e-6	2.506	1.5	0.101	0.099	811.827	-1611.65
M1	9.018e-6	/	2.522	1.512	0.013	0.583	465.075	-920.15
M2	1.032e-5	1.851e-6	2.501	1.497	0.016	/	605.479	-1200.96
M3	8.598e-6	/	2.534	1.522	0.019	/	430.897	-853.79

In addition, the product reliability under normal stress level (40°C) obtained by the four ADT models are compared with the real values as illustrated in Fig. 4. From Fig. 4, the model M0 gives an accurate reliability assessment while the other three ADT models show significant bias, proving the significance of the proposed ADT model.
 
Fig. 4  Reliability of the product under normal stress level (40°C) for the four ADT models with the real values.
Engineering application
In this section, we take the accelerated degradation data of a tuner to show the superiority of the proposed ADT model compared to other ADT models.
Data description
A tuner is a long-lifespan microwave electronic assembly designed primarily for reception, filtering, amplification and gain control of cable signals. Previous investigations into tuner failures have shown the significant impact of temperature on the degradation process [36]. To evaluate the reliability of the tuner, a constant-stress ADT based on temperature was performed under 4 stress levels. And, the key performance parameter, noise, was measured every ten hours by a computerized measuring system. Fig. 5 illustrates detailed degradation data, which is sourced from Fig. 6 in [36].
 
Fig. 5  Accelerated degradation data of a tuner under 4 stress levels.
Modeling and analysis
The Arrhenius model is adopted to build up the acceleration model since the applied accelerated stress is temperature. Then, stress normalization is employed according to Eq. (8), where the normal stress level is 20°C. Next, the initial values of the unknown parameters are set based on the result of the two-step MLE method, and the terminated threshold   for the EM algorithm iteration is set as 0.01. Subsequently, according to Algorithm 2, we obtain the estimations of unknown parameters as shown in Table 5.

Table 5  Point and interval estimation results of θ in the tuner case.
Parameters	Point estimations	90% confidence interval estimations
μa	1.073e-6	(6.623e-7, 1.856e-6)
σa	1.894e-7	(8.463e-8, 3.399e-7)
α1	4.667	(4.304, 5.021)
β	1.691	(1.642, 1.725)
σ	0.073	(0.069, 0.077)
H	2.497e-8	(1.307e-9, 0.009)

From Table 5, it can be deduced that the degradation process of the tuner exhibits short-term dependency because "H < 0.5" . The short-term memory effect in the degradation process is also reported in [37]. Besides, it can be found that H is close to zero in our case. At this point, the FBM is close to a logarithmically correlated Gaussian random process [38], which has been studied and employed in financial mathematics [39].
Then, we generate 10000 degradation paths according to the point estimations in Table 5. The predicted deterministic degradation trends and boundaries of performance with temperature and time are plotted in Fig. 6 (a). When the critical threshold Xth is 7, the performance margin can be calculated according to Eq. (12). Fig. 6 (b) illustrates the predicted deterministic degradation trends and boundaries of margin with temperature and time. It can be seen from the above figures that the degradation and corresponding boundary width increase with the increase of time and temperature.
   
                                        (a)                                                                             (b)
Fig. 6  Deterministic degradation trends and boundaries of tuner performance and margin with temperature and time.

Next, according to Algorithm 1, we can deduce the reliability under different temperature and time as shown in Fig. 7 (a). Specially, the reliability of the tuner under the normal use condition (20°C) with 90% confidence interval is illustrated in Fig. 7 (b). From Fig. 7 (b), we can get some valuable information for the development of maintenance and warranty strategies. For example, when the tuner is operated for 6000 hours, the lower boundary of reliability of the tuner at 90% CL is nearly 0.99.
   
                                            (a)                                                                       (b)
Fig. 7  Reliability of tuner under (a) different temperature and time (b) normal stress level (20°C) with 90% confidence interval.
Discussions
In general, the better the model fits the degradation data, the greater the potential for the model to reflect the degradation law. Besides, it should be recalled that the ultimate goal of ADT modeling is to extrapolate the lifetime and reliability of the product under the normal use condition, so it is crucial to ensure the accuracy of the degradation prediction for unseen stress levels. To this end, we will discuss the fitting and extrapolation capabilities of the proposed model M0 compared to other existing ADT models mentioned in Section 4.3.2.
Comparison of model fitting
In this subsection, we first calculate the lmax and the AIC to compare the effectiveness of different ADT models in fitting the degradation data. Table 6 gives the estimations of unknown parameters for the four ADT models. From the lmax and AIC values in Table 6, M0 is the most suitable model compared to other ADT models.

Table 6  Parameter estimations for the four ADT models in the tuner case.
Model	μa	σa	α1	β	σ	H	lmax	AIC
M0	1.073e-6	1.894e-7	4.667	1.691	0.073	2.497e-8	579.444	-1146.888
M1	1.349e-6	/	4.518	1.674	0.056	0.1113	551.784	-1093.568
M2	1.072e-6	2.113e-8	4.473	1.723	0.023	/	479.348	-948.696
M3	1.601e-6	/	4.280	1.684	0.023	/	479.483	-950.966

In addition to evaluating the model's goodness of fit, we are more concerned with the capacity of the model to predict actual degradation processes, which involves deterministic degradation trend predictions and uncertainty quantification under all stress levels. Credible reliability assessments can only be made if the model performs well in both aspects. In order to quantitatively compare the performance of different ADT models in degradation prediction, we set up several indices formulated by Eq. (34) to assess the precision of predicted deterministic degradation trend and uncertainty quantification [40]. Specifically, for each ADT model, we generate 1000 degradation paths according to the estimations in Table 6 and calculate the predicted deterministic degradation trend  , predicted upper boundary   and predicted lower boundary   at tlij under the lth stress level. Then, the three comparison indices can be calculated by Eq. (34), where  ,   and   denote the mean value, upper 5% quantile and lower 5% quantile for the 1000 simulated degradation paths, respectively. The closer   is to 0, the more accurately the model predicts the deterministic degradation trend under the lth stress level. The closer   or   is to 0, the better the predicted boundary can describe the uncertainty of the real data under the lth stress level. Moreover,  ,  , and   reflect the predicted performance of an ADT model under all stress levels.
	 	(34)
Table 7 gives the quantitative indices for the four ADT models under all stress levels. The visualization results of the deterministic degradation trend and the degradation boundary are shown in Fig. 8 and Fig. 9.

Table 7  Quantitative indices of the tuner case under all stress levels.
Model	 
 
 

M0	0.1335	0.9389	3.7036
M1	0.1490	1.1209	3.8215
M2	0.1429	2.3264	12.3555
M3	0.1849	2.4501	11.3810
Note: The bolded results depict the best ones.
 
Fig. 8  Prediction for the deterministic degradation trend under all stress levels.
 
Fig. 9  Prediction for the degradation boundary at 90% CL under all stress levels.

According to the above comparative tables and figures, compared with the other models, model M0 is superior not only in predicting the deterministic degradation trend, but also in uncertainty quantification, which demonstrates the superiority of the proposed model. Additionally, the uncertainty quantification of M0 and M1 is considerably superior to that of models M2 and M3, demonstrating the necessity of considering memory effects in the degradation process.
Comparison of model extrapolation 
In this subsection, we develop a cross-validation scheme [41] shown in Table 8 to explore the extrapolation capability of the four ADT models. The results of quantitative indices for the cross-validation 1 and 2 are listed in Table 9 and Table 10, respectively. The visualization results are shown in Fig. 10 and Fig. 11, respectively.

Table 8  Plans of cross-validation
Projects	Training data	Testing data
Cross-validation 1	The data under all stress levels except the lowest stress level	The data under the lowest stress level (55°C)
Cross-validation 2	The data under all stress levels except the highest stress level	The data under the highest stress level (85°C)

Table 9  Quantitative indices of the tuner case for the cross-validation 1.
Model	 
 
 

M0	0.2937	0.7971	3.3203
M1	0.4637	1.2940	1.3456
M2	0.4973	3.2261	27.1602
M3	0.3621	3.2290	28.5961
Note: The bolded results depict the best ones.

Table 10  Quantitative indices of the tuner case for the cross-validation 2.
Model	 
 
 

M0	0.1249	0.4198	9.8124
M1	0.2167	0.4468	9.9591
M2	0.1617	0.8326	11.8889
M3	0.2153	0.7809	12.5949
Note: The bolded results depict the best ones.

  
                                            (a)                                                                         (b)
Fig. 10  Prediction for the cross-validation 1: (a) deterministic degradation trend (b) degradation boundary at 90% CL.
  
                                             (a)                                                                       (b)
Fig. 11  Prediction for the cross-validation 2: (a) deterministic degradation trend (b) degradation boundary at 90% CL.

From the above comparative tables and figures, the following results can be obtained:
	According to Table 9 and Table 10, for both cross-validations, the predicted deterministic degradation trends based on M0 consistently outperform others significantly. In terms of uncertainty quantification, the upper and lower boundary predictions based on M0 are both the best for cross-validation 2, while the lower boundary predictions based on M0 are worse than those of M1 for cross-validation 1. Overall, model M0 has a superior capacity for uncertainty quantification.
	From Fig. 10 and Fig. 11, for both cross-validations, M0 best describes the deterministic degradation trend and envelopes the real data, proving the superiority of the model M0.
	By analyzing the predictions of these four ADT models for two cross-validations, we can deduce that M0 is more suitable for lifetime and reliability assessment of this tuner than other ADT models.
Conclusion
This paper seeks to tackle the challenges of reliability modeling and statistical analysis of ADT with memory effects and UtUV. The following conclusions are drawn from this paper:
	This paper proposes a comprehensive degradation model considering the influence of external stresses, memory effects and UtUV. The memory effects are quantified by the Hurst exponent in the FBM and the UtUV is quantified in the acceleration model. Besides, a statistical analysis method based on the EM algorithm is devised to ensure the maximization of the overall likelihood function. 
	The results of the simulation case illustrate that the memory effect estimation of the proposed statistical analysis method is more accurate than the two-step MLE method, especially when the number of measurements is small. Moreover, ignoring UtUV will lead to highly biased memory effect estimation and reliability evaluations.
	A tuner case is utilized to demonstrate the efficacy of the proposed methodology. Findings show that the proposed ADT model gives superior predictions in both deterministic degradation trends and uncertainty quantification compared to the existing ADT models, which is more suitable for reliability assessment.
Beyond the work of this study, there exists additional issues that warrant investigation in future research. For example, we only focus on modeling constant-stress ADT data in this work. Constructing an ADT model considering memory effects for the step-stress ADT data is another interesting topic.
Declarations of interest
None.
Acknowledgements
This work was supported by the National Natural Science Foundation of China [grant number 51775020], the Science Challenge Project [grant number. TZ2018007], the National Natural Science Foundation of China [grant numbers 62073009].
Appendix
In this section, we briefly introduce the two-step MLE method [23] for parameter estimations of ADT models with UtUV. The variables used are consistent with those in Section 3.3. In the first step, the parameters associated with the degradation process model are estimated, denoted as  . Subsequently, the remaining parameters relevant to the acceleration model are determined in the second step, denoted as  .
In the first step, based on the property of FBM, for the ith item tested at lth stress level, we have
	 	(35)
where
	 	(36)
Based on Eq. (35), the log-likelihood function of the ADT data can be derived as:
	 	(37)
Then, the first partial derivatives of   with respect to   and   can be derived, respectively. By equating each derivative to zero, we can obtain
	 	(38)
	 	(39)
Upon substituting Eqs (38) and (39) into Eq. (37), the log-likelihood function depends solely on β and H. Thus,   and   can be obtained by a two-dimensional search for the maximum value of Eq. (37). Then,   and   can calculated by substituting   and   into Eqs (38) and (39).
In the second step, according to Eq. (11), we can get
	 	(40)
Based on Eq. (40), the log-likelihood function for   can be derived as:
	 	(41)
Specifically,   and   can be computed as:
	 	(42)
	 	(43)
By substituting Eqs (42) and (43) into Eq. (41), the log-likelihood function depends solely on  .Therefore,   can be obtained by a one-dimensional search for the maximum value of Eq. (41). Then,   and   can calculated by substituting   into Eqs (42) and (43).
References
[1]	S. Li, Z. Chen, Q. Liu, et al. Modeling and analysis of performance degradation data for reliability assessment: A review. IEEE Access. 2020, 8: 74648-74678.
[2]	Y. Li, H. Gao, H. Chen, et al. Accelerated degradation testing for lifetime analysis considering random effects and the influence of stress and measurement errors. Reliab. Eng. Syst. Saf. 2024, 247: 110101.
[3]	Y. Li, S. Xu, H. Chen, et al. A general degradation process of useful life analysis under unreliable signals for accelerated degradation testing. IEEE Trans. Ind. Informatics. 2022, 19(6): 7742-7750.
[4]	R. Tian, F. Zhang, H. Du, et al. Optimization design method for multi-stress accelerated degradation test based on Tweedie exponential dispersion process. Appl. Math. Model. 2024, 135: 684-707.
[5]	H. Zhang, D. Zhou, M. Chen, et al. FBM-Based Remaining Useful Life Prediction for Degradation Processes With Long-Range Dependence and Multiple Modes. IEEE Trans. Reliab. 2018.
[6]	T. Guérin, N. Levernier, O. Bénichou, et al. Mean first-passage times of non-Markovian random walkers in confinement. Nature. 2016, 534(7607): 356-359.
[7]	W. Si, Y. Shao, W. Wei. Accelerated Degradation Testing with Long-Term Memory Effects. IEEE Trans. Reliab. 2020, 69(4): 1254-1266.
[8]	M. Giorgio, M. Guida, G. Pulcini. A new class of Markovian processes for deteriorating units with state dependent increments and covariates. IEEE Trans. Reliab. 2015, 64(2): 562-578.
[9]	M. Giorgio, G. Pulcini. A new age- and state-dependent degradation process with possibly negative increments. Qual. Reliab. Eng. Int. 2019, 35(5): 1476-1501.
[10]	W. Peng, S.-P. Zhu, L. Shen. The transformed inverse Gaussian process as an age-and state-dependent degradation model. Appl. Math. Model. 2019, 75: 837-852.
[11]	F. Duan, G. Wang. Bayesian analysis for the transformed exponential dispersion process with random effects. Reliab. Eng. Syst. Saf. 2022, 217: 108104.
[12]	T. Sottinen. Fractional Brownian motion, random walks and binary market models. Finance Stoch. 2001, 5(3): 343-355.
[13]	K. Burnecki, E. Kepten, J. Janczura, et al. Universal algorithm for identification of fractional Brownian motion. A case of telomere subdiffusion. Biophys. J. 2012, 103(9): 1839-1847.
[14]	H. Zhang, M. Chen, J. Shang, et al. Stochastic process-based degradation modeling and RUL prediction: from Brownian motion to fractional Brownian motion. Sci. China Inf. Sci. 2021, 64(7).
[15]	X. Xi, M. Chen, D. Zhou. Remaining Useful Life Prediction for Degradation Processes with Memory Effects. IEEE Trans. Reliab. 2017, 66(3): 751-760.
[16]	X. Xi, M. Chen, H. Zhang, et al. An improved non-Markovian degradation model with long-term dependency and item-to-item uncertainty. Mech. Syst. Signal Process. 2018, 105: 467-480.
[17]	Y. Shao, W. Si. Degradation Modeling With Long-Term Memory Considering Measurement Errors. IEEE Trans. Reliab. 2021.
[18]	X. Xi, D. Zhou, M. Chen, et al. Remaining useful life prediction for multivariable stochastic degradation systems with non-Markovian diffusion processes. Qual. Reliab. Eng. Int. 2020, 36(4): 1402-1421.
[19]	J.-P. Wu, R. Kang, X.-Y. Li. Uncertain accelerated degradation modeling and analysis considering epistemic uncertainties in time and unit dimension. Reliab. Eng. Syst. Saf. 2020, 201: 106967.
[20]	P. Jiang. Statistical Inference of Wiener Constant-Stress Accelerated Degradation Model with Random Effects. Mathematics. 2022, 10(16): 2863.
[21]	H. Zheng, J. Yang, W. Kang, et al. Accelerated degradation data analysis based on inverse Gaussian process with unit heterogeneity. Appl. Math. Model. 2024, 126: 420-438.
[22]	S. Tang, X. Guo, C. Yu, et al. Accelerated degradation tests modeling based on the nonlinear wiener process with random effects. Math. Probl. Eng. 2014, 2014.
[23]	L. Liu, X. Li, F. Sun, et al. A general accelerated degradation model based on the wiener process. Materials. 2016, 9(12).
[24]	A.P. Dempster, N.M. Laird, D.B. Rubin. Maximum likelihood from incomplete data via the EM algorithm. J. R. Stat. Soc. Ser. B (Methodol.). 1977, 39(1): 1-22.
[25]	B.B. Mandelbrot, J.W. Van Ness. Fractional Brownian motions, fractional noises and applications. SIAM Rev. 1968, 10(4): 422-437.
[26]	L.A. Escobar, W.Q. Meeker. A review of accelerated test models. Stat. Sci. 2006. 552-577.
[27]	W.-B. Chen, X.-Y. Li, J.-P. Wu, et al. Uncertain random accelerated degradation modelling and statistical analysis with aleatory and epistemic uncertainties from multiple dimensions. Reliab. Eng. Syst. Saf. 2024, 243: 109906.
[28]	Z.S. Ye, L.P. Chen, L.C. Tang, et al. Accelerated degradation test planning using the inverse gaussian process. IEEE Trans. Reliab. 2014, 63(3): 750-763.
[29]	X. Ye, Y. Hu, B. Zheng, et al. A new class of multi-stress acceleration models with interaction effects and its extension to accelerated degradation modelling. Reliab. Eng. Syst. Saf. 2022, 228.
[30]	R. Kang. Belief reliability theory and methodology. Singapore: Springer, 2021.
[31]	A.T. Wood, G. Chan. Simulation of stationary Gaussian processes in [0, 1] d. J. Comput. Graph. Stat. 1994, 3(4): 409-432.
[32]	H. Li, D. Pan, C.L.P. Chen. Reliability modeling and life estimation using an expectation maximization based wiener degradation model for momentum wheels. IEEE Trans. Cybern. 2015, 45(5): 969-977.
[33]	D. Pan, Y. Wei, H. Fang, et al. A reliability estimation approach via Wiener degradation model with measurement errors. Appl. Math. Comput. 2018, 320: 131-141.
[34]	C.M. Liao, S.T. Tseng. Optimal design for step-stress accelerated degradation tests. IEEE Trans. Reliab. 2006, 55(1): 59-66.
[35]	J.E. Cavanaugh, A.A. Neath. The Akaike information criterion: Background, derivation, properties, application, interpretation, and refinements. Wiley Interdiscip. Rev.: Comput. Stat. 2019, 11(3): e1460.
[36]	F. Sun, F. Fu, H. Liao, et al. Analysis of multivariate dependent accelerated degradation data using a random-effect general Wiener process and D-vine Copula. Reliab. Eng. Syst. Saf. 2020, 204: 107168.
[37]	H. Zhang, C. Jia, M. Chen. Remaining Useful Life Prediction for Degradation Processes with Dependent and Nonstationary Increments. IEEE Trans. Instrum. Meas. 2021, 70.
[38]	Y. Fyodorov, B. Khoruzhenko, N. Simm. Fractional Brownian motion with Hurst index H=0 and the Gaussian unitary ensemble. Ann. Probab. 2016, 44(4): 2980-3031.
[39]	Y.V. Fyodorov, P.L. Doussal. Moments of the position of the maximum for GUE characteristic polynomials and for log-correlated Gaussian processes. J. Stat. Phys. 2016, 164(1): 190-240.
[40]	X.Y. Li, D.Y. Chen, J.P. Wu, et al. 3-Dimensional general ADT modeling and analysis: Considering epistemic uncertainties in unit, time and stress dimension. Reliab. Eng. Syst. Saf. 2022, 225.
[41]	J. Shao. Linear model selection by cross-validation. J. Am. Stat. Assoc. 1993, 88(422): 486-494.

