Instrumental Variables Estimation
**************************************

Introduction
===============

If we think an explanatory variable in a model is endogenous, and we have cross-sectional data, we
have basically two choices.

* Collect good controls in the hope that the endogenous explanatory variable becomes exogenous.
  (This is one approach to treatment effects estimation of policy interventions.)

* Find one or more instrumental variables for the endogenous explanatory variable.

Coming up with convincing instruments is difficult.

The problem of requiring instrumental variables becomes even more difficult when the coefficient on
the EEV changes in unobserved ways. (So, for example, the treatment effect changes across individual
units.)

Modern approach to IV gives a local average treatment effect interpretation.


Instrumental Variable
=========================

Many data sets do not have good proxies for unobservables, or one loses lots of data in using them.

Very common instead to use *instrumental variables* estimation to allow a RHS variable to be
correlated with the unobservables.

.. admonition:: Example: Effects of Class Size on Student Performance

    Consider a model in the population:

    .. math:: score=\beta _{0}+\beta _{1}classize+u

    where we think :math:`classize` is endogenous:

    .. math:: Cov(z,u)\neq 0.

    We could try to proxy for the omitted variables in :math:`u`, perhaps using family background and
    socioeconomic variables. But we may not be able to sufficiently capture everything in :math:`u` that
    affects :math:`score`.

    Collecting panel data on students can be helpful but is more difficult to get. Plus,
    :math:`classize` may not change much over time. (Better funded schools tend to have smaller classes
    in all grades.)

We can solve the endogeneity problem if we can collect data on a variable, :math:`z`, that satisfies
two restrictions in the linear model :math: `y=\beta _{0}+\beta _{1}x+u`:

1. :math:`z` is **exogenous** to the equation:

   .. math:: Cov(z,u)=0

2. :math:`z` is **relevant** for explaining :math:`x`:

   .. math:: Cov(z,x)\neq 0

Important difference between these two requirements: We cannot test (1), but we can determine
(usually with high confidence) whether (2) is true.

How can we use a variable :math:`z` satisfying these two requirements?

Take the covariance of :math:`z` with both sides of the equation

.. math:: y=\beta _{0}+\beta _{1}x+u

to get

.. math:: Cov(z,y)=\beta _{1}Cov(z,x)+Cov(z,u).

Now use :math:`Cov(z,u)=0` (exogeneity) to get

.. math:: Cov(z,y)=\beta _{1}Cov(z,x).

Next, use :math:`Cov(z,x)\neq 0` (relevance) to get

.. math:: \beta _{1}=\frac{Cov(z,y)}{Cov(z,x)}

We have written :math:`\beta _{1}` as two population moments in observable variables.

Given a random sample :math:`\{(y_{i},x_{i},z_{i}):i=1,...,n\}`, use the sample covariances to
estimate the population covariances. (Method of moments estimation).

This gives the IV estimator of :math:`\beta _{1}`.

.. math::

   \hat{\beta}_{1,IV}=\frac{n^{-1}\sum_{i=1}^{n}(z_{i}-\bar{z})(y_{i}-\bar{y})}{
   n^{-1}\sum_{i=1}^{n}(z_{i}-\bar{z})(x_{i}-\bar{x})}

IV estimator is consistent, not unbiased.  (Bias can be large if the correlation beween :math:`z`
and :math:`x` is small.)

The variance of the IV estimator can be large.  Under homoskedasticity of :math:`u`,

.. math::

   Var(\hat{\beta}_{1,IV})\approx \frac{\sigma _{u}^{2}}{n\sigma _{x}^{2}\rho
   _{x,z}^{2}}

with :math:`\sigma _{u}^{2}=Var(u)`, :math:`\sigma _{x}^{2}=Var(x)` and :math:`\rho
_{x,z}=Corr(x,z)`.

Comparable formula for OLS (when OLS is consistent):

.. math:: Var(\hat{\beta}_{1,OLS})\approx \frac{\sigma _{u}^{2}}{n\sigma _{x}^{2}}

So, as a rough rule of thumb, the standard error of the IV estimator is about

.. math:: \frac{1}{r_{xz}}

larger than that for OLS, where :math:`r_{xz}` is the sample correlation between :math:`x_{i}` and
:math:`z_{i}`.

Can think of this factor as the cost of doing IV when we could be doing OLS. (If OLS is inconsitent,
the variance compariso makes little sense.)

Often :math:`r_{xz}` is small, so IV standard error is large. A large :math:`n` can help offset.

Can compute a heteroskedasticity robust or nonrobust standard error and conduct large-sample
inference using :math:`t` statistics and confidence intervals.

No restrictions on the nature of :math:`x_{i}` or :math:`z_{i}`. For example, each could be binary,
or just one of them.

In Stata the command is ``ivreg`` ::

    ivreg y (x = z)

    ivreg y (x = z), robust

To even proceed with IV, we need to first demonstrate that :math:`z_{i}` helps to predict
:math:`x_{i}` (and in the direction suggested by economics or common sense). Easiest way is to just
regress :math:`x_{i}` on :math:`z_{i}` and do a robust :math:`t` test.

Actually, recent research (Staiger and Stock, 1997, *Econometrica*) on so-called weak
instruments says that, in this simple case, the :math:`t` statistic from this regression should be
at least :math:`3.2\approx \sqrt{10}` -- much higher than just a rejection at the standard 5% level.

Where might instrumental variables come from?  Randomized eligibility can work well as an IV for
participation in a program. So :math:`x_{i}=1` if person actually participates. :math:`z_{i}=1` if
the person was made eligible.

In the Tennessee STAR program, some students were randomly made eligible for smaller class sizes. So
:math:`x_{i}` can be the actual class size, :math:`z_{i}=1` if student :math:`i` was made eligible
for a small class size.

Caution: Just because a variable is randomized does not make it exogenous to a model. Economic
agents can change their behavior!

Example due to Angrist and Evans (1998, *American Economic Review*). Weekly hours equation

.. math:: hours=\beta _{0}+\beta _{1}kids+u

for the population of women with at least two children (so :math:`kids\geq 2`). One proposed IV is
:math:`samesex`, equal to one of the first two children have the same gender.

Even if gender is exogenous, the family’s budget constraint is subsequently affected. (Kids of the
same gender can more easily share a room, clothes, and toys.)

Following uses a (small!) subset of data from Angrist and Evans. Note how large the sample size is,
yet IV estimator is barely statistically significant.

::

    . use labsup
     
    . des hours kids samesex
     
                  storage  display     value
    variable name   type   format      label      variable label
    --------------------------------------------------------------------------------------------------
    hours           byte   %8.0g                  hours of work per week, mom
    kids            byte   %8.0g                  number of kids
    samesex         byte   %8.0g                  first two kids are of same sex
     
    . sum hours kids samesex
     
        Variable |       Obs        Mean    Std. Dev.       Min        Max
    -------------+--------------------------------------------------------
           hours |     31857    21.22011    19.49892          0         99
            kids |     31857    2.752237    .9771916          2         12
         samesex |     31857     .502778    .5000001          0          1
     
    . * Get roughly 50% have both boys or both girls, as we should expect.

::

    . * OLS, first:
     
    . reg hours kids, robust
     
    Linear regression                                      Number of obs =   31857
                                                           F(  1, 31855) =  585.25
                                                           Prob > F      =  0.0000
                                                           R-squared     =  0.0178
                                                           Root MSE      =  19.325
     
    ------------------------------------------------------------------------------
                 |               Robust
           hours |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
            kids |  -2.664309   .1101318   -24.19   0.000    -2.880171   -2.448446
           _cons |   28.55292   .3200455    89.22   0.000     27.92562    29.18022
    ------------------------------------------------------------------------------
     
    . * So each child (above two) decreases weekly hours, on average, by 2.66.

::

    . * Now do IV. Need to first check that samesex is relevant for kids:
     
    . reg kids samesex, robust
     
    Linear regression                                      Number of obs =   31857
                                                           F(  1, 31855) =   40.90
                                                           Prob > F      =  0.0000
                                                           R-squared     =  0.0013
                                                           Root MSE      =  .97658
     
    ------------------------------------------------------------------------------
                 |               Robust
            kids |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
         samesex |   .0699933   .0109439     6.40   0.000     .0485429    .0914437
           _cons |   2.717045    .007806   348.07   0.000     2.701745    2.732346
    ------------------------------------------------------------------------------
     
    . * t statistic is above six, so okay to proceed (assuming samesex is
    . * exogenous!)

::

    . * Now IV with heteroskedasticity-robust standard errors:
     
    . ivreg hours (kids = samesex), robust
     
    Instrumental variables (2SLS) regression               Number of obs =   31857
                                                           F(  1, 31855) =    3.19
                                                           Prob > F      =  0.0743
                                                           R-squared     =       .
                                                           Root MSE      =  19.534
     
    ------------------------------------------------------------------------------
                 |               Robust
           hours |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
            kids |   -5.58186   3.127136    -1.78   0.074    -11.71117    .5474471
           _cons |   36.58271   8.606509     4.25   0.000     19.71362    53.45179
    ------------------------------------------------------------------------------
    Instrumented:  kids
    Instruments:   samesex
     
    . * More than twice as large in magnitude, but 95% CI actually contains zero
    . * (contrast for OLS).

::

    . * Correlation between kids and samesex is small:
     
    . corr kids samesex
    (obs=31857)
     
                 |     kids  samesex
    -------------+------------------
            kids |   1.0000
         samesex |   0.0358   1.0000
    ------------------------------------------------------------------------------
     
    . * Actually ration of IV se to OLS se:
     
    . di 3.127/.111
    28.171171
     
    . * Ratio from rule-of-thumb:
     
    . di 1/0.0358
    27.932961

In the previous example, there is no way to test whether :math:`samesex` is exogenous. We must
assume it is in order to trust IV to be consistent.

In some cases, we can use other information to determine whether an IV is exogenous.

.. admonition:: Example: Estimating the Return to Schooling Using CARD.DTA

    A binary indicator, :math:`nearc4_{i}`, equal to one if the man was near a four-year college in high
    school can be used as an IV. Would expect :math:`x_{i}=educ_{i}` and :math:`z_{i}=nearc4_{i}` to be
    positively related.

::

    . use card
     
    . sum educ nearc4
     
        Variable |       Obs        Mean    Std. Dev.       Min        Max
    -------------+--------------------------------------------------------
            educ |      3010    13.26346    2.676913          1         18
          nearc4 |      3010    .6820598    .4657535          0          1
     
    . reg educ nearc4, robust
     
    Linear regression                                      Number of obs =    3010
                                                           F(  1,  3008) =   60.37
                                                           Prob > F      =  0.0000
                                                           R-squared     =  0.0208
                                                           Root MSE      =  2.6494
     
    ------------------------------------------------------------------------------
                 |               Robust
            educ |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
          nearc4 |    .829019   .1066941     7.77   0.000     .6198182     1.03822
           _cons |   12.69801   .0902199   140.75   0.000     12.52112    12.87491
    ------------------------------------------------------------------------------
     
    . * educ and nearc4 are strongly enough related: being near a 4-year college
    . * increases educ by almost a year. t statistic is pretty large.
      

::

    . ivreg lwage (educ = nearc4), robust
     
    Instrumental variables (2SLS) regression               Number of obs =    3010
                                                           F(  1,  3008) =   51.75
                                                           Prob > F      =  0.0000
                                                           R-squared     =       .
                                                           Root MSE      =  .55686
     
    ------------------------------------------------------------------------------
                 |               Robust
           lwage |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
            educ |   .1880626   .0261426     7.19   0.000     .1368035    .2393217
           _cons |   3.767472    .346742    10.87   0.000     3.087596    4.447347
    ------------------------------------------------------------------------------
    Instrumented:  educ
    Instruments:   nearc4
    ------------------------------------------------------------------------------
     
    . * Note that the list of exogenous variables in the lwage equation is empty.
    . * Estimated return to education seems too large. CI is wide, but lower
    . * bound is still 13.7%.

::

    . * For comparison, OLS:
     
    . reg lwage educ, robust
     
    Linear regression                                      Number of obs =    3010
                                                           F(  1,  3008) =  321.16
                                                           Prob > F      =  0.0000
                                                           R-squared     =  0.0987
                                                           Root MSE      =  .42139
     
    ------------------------------------------------------------------------------
                 |               Robust
           lwage |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
            educ |   .0520942   .0029069    17.92   0.000     .0463946    .0577939
           _cons |   5.570882   .0390935   142.50   0.000      5.49423    5.647535
    ------------------------------------------------------------------------------
     
    . * 18.8% versus 5.2%!

Why are OLS and IV so different in the Card case? A common explanation is that :math:`educ` is
measured with error, so there is attenuation bias with OLS. (Seems implausible that measurement
error could account for such a large difference.) Another explanation (considered later) is that the
return to schooling is not constant and IV is picking up the effect for a certain subgroup.

Should not ignore the possibility that the instrument is somewhat endogenous. From Wooldridge (5e,
Chapter 15):

.. math::

   \begin{aligned}
   plim(\hat{\beta}_{1,OLS}) &=&\beta _{1}+\frac{\sigma _{u}}{\sigma _{x}}\cdot
   Corr(x,u) \\
   plim(\hat{\beta}_{1,IV}) &=&\beta _{1}+\frac{\sigma _{u}}{\sigma _{x}}\cdot 
   \frac{Corr(z,u)}{Corr(z,x)}\end{aligned}

So even if :math:`Corr(z,u)` is smaller than :math:`Corr(x,u)`, the bias in IV can be much larger
because :math:`Corr(z,u)` is blown up by

.. math:: \frac{1}{Corr(z,x)}

Having :math:`Corr(z,x)` on the order or :math:`.10` or smaller is not unusual. (In the Angrist and
Evans example above the correlation was less than :math:`.04`.)

In the Card data set, suppose :math:`IQ` is essentially :math:`u`. Then we can approximate
:math:`Corr(x,u)` and :math:`Corr(z,u)` by using using :math:`IQ` in place of :math:`u`.

::

    . corr nearc4 educ
    (obs=3010)
     
                 |   nearc4     educ
    -------------+------------------
          nearc4 |   1.0000
            educ |   0.1442   1.0000
     
    . corr nearc4 IQ
    (obs=2061)
     
                 |   nearc4       IQ
    -------------+------------------
          nearc4 |   1.0000
              IQ |   0.0765   1.0000
     
    . corr educ IQ
    (obs=2061)
     
                 |     educ       IQ
    -------------+------------------
            educ |   1.0000
              IQ |   0.5103   1.0000
     
    . di .0765/.1442
    .53051318

If we assume :math:`u=IQ`, the bias terms are essentially the same: :math:`.51` for OLS and
:math:`.53` for IV. So maybe there is measurement error in :math:`educ` or we need to control for
more factors.

The IV standard error is :math:`.0261` compared with :math:`.0029` for OLS, or a factor of 9. The
rough rule-of-thumb for the blowing up factor gives

.. math:: \frac{1}{\widehat{Corr}(z,x)}\approx \frac{1}{.1442}\approx 7

Sometimes a potential instrument is exogenous only when other factors are controlled for. Let \
:math:`\mathbf{r}_{i}` be another vector of regressors and consider

.. math:: y_{i}=\beta _{0}+\beta _{1}x_{i}+\mathbf{r}_{i}\mathbf{\gamma }+u_{i}

(where the new error :math:`u_{i}` is really different from the old one because
:math:`\mathbf{r}_{i}` has been taken out). Assume elements of :math:`\mathbf{r}_{i}` are exogenous
and we still have an exogenous instrument :math:`z_{i}` for :math:`x_{i}`:

.. math::

   \begin{aligned}
   Cov(\mathbf{r}_{i},u_{i}) &=&\mathbf{0} \\
   Cov(z_{i},u_{i}) &=&0\end{aligned}

[and still :math:`E(u_{i})=0`].

Now, :math:`z_{i}` must be *partially* correlated with :math:`x_{i}`. Easiest to test with the
regression

.. math:: x_{i}\text{ on }z_{i},\text{ }\mathbf{r}_{i}

and reject the coefficient on :math:`z_{i}` is equal to zero. Called the
*first-stage regression*.

Card argues that, while :math:`nearc4` is not uncorrelated with ability (:math:`IQ`), it is after
controlling for region of the U.S. (where the man lived at age 16). He also includes a race
indicator, living in an SMSA (both currently and at age 16), and living in the south (currently).
Experience is included as in the usual Mincer equation.

::

    . reg educ nearc4 exper expersq black smsa south smsa66 reg662-reg669, robust
     
    ------------------------------------------------------------------------------
                 |               Robust
            educ |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
          nearc4 |   .3198989   .0850763     3.76   0.000      .153085    .4867128
           exper |  -.4125334   .0320751   -12.86   0.000    -.4754249   -.3496418
         expersq |   .0008686   .0017076     0.51   0.611    -.0024795    .0042167
           black |  -.9355287   .0925281   -10.11   0.000    -1.116954   -.7541037
            smsa |   .4021825   .1112278     3.62   0.000     .1840918    .6202731
           south |  -.0516126   .1419604    -0.36   0.716    -.3299623    .2267371
          smsa66 |   .0254805   .1106315     0.23   0.818    -.1914409    .2424019
          reg662 |  -.0786363   .1858739    -0.42   0.672    -.4430898    .2858171
          reg663 |   -.027939   .1793411    -0.16   0.876    -.3795833    .3237053
          reg664 |    .117182   .2075839     0.56   0.572    -.2898395    .5242035
          reg665 |  -.2726165   .2243154    -1.22   0.224    -.7124443    .1672114
          reg666 |  -.3028147   .2367287    -1.28   0.201     -.766982    .1613526
          reg667 |  -.2168177   .2394968    -0.91   0.365    -.6864128    .2527773
          reg668 |   .5238914   .2568717     2.04   0.041     .0202284    1.027554
          reg669 |    .210271   .1993703     1.05   0.292    -.1806456    .6011876
           _cons |   16.63825   .2153815    77.25   0.000     16.21594    17.06056
    ------------------------------------------------------------------------------
     
    . * So nearc4 is still partially correlated with educ and can be used
    . * as an IV. (Partial correlation not as strong as simple correlation.)

::

    . ivreg lwage exper expersq black smsa south smsa66 reg662-reg669 
           (educ = nearc4), robust
     
    Instrumental variables (2SLS) regression               Number of obs =    3010
     
    ------------------------------------------------------------------------------
                 |               Robust
           lwage |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
            educ |   .1315038   .0541436     2.43   0.015     .0253414    .2376663
           exper |   .1082711   .0234089     4.63   0.000      .062372    .1541702
         expersq |  -.0023349   .0003488    -6.69   0.000    -.0030188   -.0016511
           black |  -.1467757   .0525019    -2.80   0.005    -.2497193   -.0438322
            smsa |   .1118083   .0311448     3.59   0.000     .0507409    .1728757
           south |  -.1446715   .0291429    -4.96   0.000    -.2018136   -.0875294
          smsa66 |   .0185311   .0205651     0.90   0.368     -.021792    .0588542
          reg662 |   .1007678   .0365488     2.76   0.006     .0291045    .1724311
          reg663 |   .1482588   .0355971     4.16   0.000     .0784615     .218056
          reg664 |   .0498971   .0436162     1.14   0.253    -.0356238    .1354179
          reg665 |   .1462719   .0492259     2.97   0.003      .049752    .2427919
          reg666 |   .1629029   .0517655     3.15   0.002     .0614034    .2644025
          reg667 |   .1345722   .0505568     2.66   0.008     .0354427    .2337017
          reg668 |   -.083077   .0572432    -1.45   0.147     -.195317     .029163
          reg669 |   .1078142   .0410761     2.62   0.009     .0272739    .1883545
           _cons |   3.666151     .91096     4.02   0.000      1.87998    5.452322
    ------------------------------------------------------------------------------
    Instrumented:  educ
    Instruments:   exper expersq black smsa south smsa66 reg662 reg663 reg664
                   reg665 reg666 reg667 reg668 reg669 nearc4
    ------------------------------------------------------------------------------

::

    . reg lwage educ exper expersq black smsa south smsa66 reg662-reg669, robust
     
    Linear regression                                      Number of obs =    3010
     
    ------------------------------------------------------------------------------
                 |               Robust
           lwage |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
            educ |   .0746933   .0036462    20.48   0.000     .0675439    .0818427
           exper |    .084832   .0067548    12.56   0.000     .0715875    .0980765
         expersq |   -.002287   .0003194    -7.16   0.000    -.0029133   -.0016608
           black |  -.1990123   .0181644   -10.96   0.000    -.2346282   -.1633964
            smsa |   .1363845   .0192172     7.10   0.000     .0987042    .1740648
           south |   -.147955   .0280346    -5.28   0.000     -.202924    -.092986
          smsa66 |   .0262417   .0185908     1.41   0.158    -.0102102    .0626937
          reg662 |   .0963672   .0350964     2.75   0.006     .0275518    .1651826
          reg663 |     .14454   .0338217     4.27   0.000      .078224     .210856
          reg664 |   .0550756    .041204     1.34   0.181    -.0257154    .1358665
          reg665 |   .1280248    .042915     2.98   0.003     .0438789    .2121707
          reg666 |   .1405174   .0451252     3.11   0.002     .0520378     .228997
          reg667 |    .117981    .045614     2.59   0.010      .028543     .207419
          reg668 |  -.0564361   .0505995    -1.12   0.265    -.1556494    .0427773
          reg669 |   .1185698   .0387784     3.06   0.002     .0425347    .1946048
           _cons |   4.620807    .074229    62.25   0.000     4.475262    4.766352
    ------------------------------------------------------------------------------
     
    . * Discrepancy is smaller now, but still large: 13.2% for IV, 7.5% for OLS.



.. admonition:: Example: Effect of District-Level Spending on Fourth Grade Math Pass Rates (MEAP)

    Data from Papke (2008, *Public Finance Review*). Using only 1995, the year after passage of Proposal
    a. But spending is an averge of current and past three years. In MEAP92\_01.DTA.

    District-Level Data. Instrumental Variable is Foundation Allowance.

    Will use later for panel data.

::

    . use meap92_01
     
    . replace math4 = math4*100
    (5010 real changes made)
     
    . replace math4_94 = math4_94*100
    (3507 real changes made)
     
    . sum math4 lavgrexp lfound if y95
     
        Variable |       Obs        Mean    Std. Dev.       Min        Max
    -------------+--------------------------------------------------------
           math4 |       501    61.75249    13.05005       17.9       98.6
        lavgrexp |       501    8.629719    .1603608   8.313771   9.377905
          lfound |       501    8.530368    .1532953    8.34284    9.25474
     

::

    . reg math4 lavgrexp math4_94 if y95, robust
     
    Linear regression                                      Number of obs =     501
                                                           F(  2,   498) =  223.32
                                                           Prob > F      =  0.0000
                                                           R-squared     =  0.4760
                                                           Root MSE      =  9.4652
     
    ------------------------------------------------------------------------------
                 |               Robust
           math4 |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
        lavgrexp |   4.434022   2.720913     1.63   0.104    -.9118611    9.779905
        math4_94 |   .6430129   .0327015    19.66   0.000      .578763    .7072629
           _cons |  -8.182627    23.0801    -0.35   0.723    -53.52901    37.16375
    ------------------------------------------------------------------------------
     
    . * A 10 percent increase in spending (lavgrexp up by .1) 
    . * increases the pass rate by about .44 percentage points.

::

    . * IV estimate is somewhat larger:
     
    . ivreg math4 math4_94 (lavgrexp = lfound) if y95, robust
     
    Instrumental variables (2SLS) regression               Number of obs =     501
                                                           F(  2,   498) =  230.08
                                                           Prob > F      =  0.0000
                                                           R-squared     =  0.4753
                                                           Root MSE      =  9.4716
     
    ------------------------------------------------------------------------------
                 |               Robust
           math4 |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
        lavgrexp |   6.639055   2.709613     2.45   0.015     1.315373    11.96274
        math4_94 |   .6392136   .0328142    19.48   0.000     .5747422     .703685
           _cons |  -27.02431    22.9201    -1.18   0.239    -72.05632     18.0077
    ------------------------------------------------------------------------------
    Instrumented:  lavgrexp
    Instruments:   math4_94 lfound
    ------------------------------------------------------------------------------

::

    . * No question that lavgrexp and lfound are partially correlated:
     
    . reg lavgrexp lfound math4_94 if y95, robust
     
    Linear regression                                      Number of obs =     501
                                                           F(  2,   498) =  913.37
                                                           Prob > F      =  0.0000
                                                           R-squared     =  0.8558
                                                           Root MSE      =  .06102
     
    ------------------------------------------------------------------------------
                 |               Robust
        lavgrexp |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
          lfound |    .976786   .0235874    41.41   0.000      .930443    1.023129
        math4_94 |  -.0005443   .0002156    -2.53   0.012    -.0009678   -.0001208
           _cons |   .3241837   .1976956     1.64   0.102    -.0642365    .7126039
    ------------------------------------------------------------------------------

::

    . * If we just use simple regression and do not control for lagged score
    . * we get a much larger estimate:
     
    . reg math4 lavgrexp if y95, robust
     
    Linear regression                                      Number of obs =     501
                                                           F(  1,   499) =    8.99
                                                           Prob > F      =  0.0028
                                                           R-squared     =  0.0241
                                                           Root MSE      =  12.905
     
    ------------------------------------------------------------------------------
                 |               Robust
           math4 |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
        lavgrexp |    12.6344   4.213412     3.00   0.003     4.356189    20.91262
           _cons |  -47.27885   36.21994    -1.31   0.192    -118.4412    23.88353
    ------------------------------------------------------------------------------
     
    . * IV with math4_94 as an additional control gives an estimate in between
    . * the two OLS estimates.
     
    . * Later we will use full panel data and combine with IV methods.



Two Stage Least Squares (2SLS)
===================================

When we have more instruments than necessary, IV become **two stage least squares (2SLS)**.

``ivreg`` in Stata works the same. We list all outside instruments for the endogenous explanatory
variable (such as :math:`educ`).

Trying to do 2SLS by hand can lead to mistakes -- at a minimum, wrong standard errors.

In the Card data set, there is also an indicator for being near a two-year college, which we can use
along with :math:`nearc4\,`\ as an IV for :math:`nearc2`.

The Staiger-Stock rule-of-thumb for whether we have strong enough IVs is that, in the first stage
regression, the joint :math:`F` for significance of IVs should be above :math:`10`. (The :math:`t`
statistic rule is a special case: :math:`\sqrt{10}\approx 3.2`.)

::

    . reg educ nearc2 nearc4 exper expersq black smsa south smsa66 reg662-reg669, 
          robust
     
    Linear regression                                      Number of obs =    3010
                                                           F( 16,  2993) =  230.04
                                                           Prob > F      =  0.0000
                                                           R-squared     =  0.4776
                                                           Root MSE      =    1.94
     
    ------------------------------------------------------------------------------
                 |               Robust
            educ |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
          nearc2 |   .1229986     .07763     1.58   0.113    -.0292149    .2752121
          nearc4 |   .3205819   .0850041     3.77   0.000     .1539096    .4872542
           exper |  -.4122915   .0319908   -12.89   0.000    -.4750177   -.3495654
         expersq |   .0008479   .0017026     0.50   0.619    -.0024905    .0041863
           black |  -.9451729   .0925529   -10.21   0.000    -1.126647   -.7636992
            smsa |   .4013708    .111281     3.61   0.000     .1831757    .6195658
           south |  -.0419115   .1416803    -0.30   0.767    -.3197122    .2358891
          smsa66 |   .0000782   .1117998     0.00   0.999    -.2191339    .2192904
          reg662 |  -.1002481   .1855985    -0.54   0.589    -.4641617    .2636655
          reg663 |  -.0214286   .1787247    -0.12   0.905    -.3718642    .3290071
          reg664 |   .1310678   .2071421     0.63   0.527    -.2750876    .5372232
          reg665 |  -.2683558   .2234419    -1.20   0.230     -.706471    .1697594
          reg666 |  -.3334436   .2367974    -1.41   0.159    -.7977457    .1308585
          reg667 |  -.2087488   .2388692    -0.87   0.382    -.6771132    .2596155
          reg668 |   .5507871   .2570126     2.14   0.032     .0468479    1.054726
          reg669 |   .1687829   .2008521     0.84   0.401    -.2250392    .5626049
           _cons |   16.60428   .2163869    76.73   0.000     16.17999    17.02856
    ------------------------------------------------------------------------------

::

    . * Adding nearc2 doesn't actually add much predictive power for educ.
     
    . test nearc2 nearc4
     
     ( 1)  nearc2 = 0
     ( 2)  nearc4 = 0
     
           F(  2,  2993) =    8.32
                Prob > F =    0.0002
     
    . * So the IVs are not quite strong enough using the ROT. Can still try IV, 
    . * of course.

::

    . ivreg lwage exper expersq black smsa south smsa66 reg662-reg669 
            (educ = nearc4 nearc2), robust
     
    Instrumental variables (2SLS) regression               Number of obs =    3010
    ------------------------------------------------------------------------------
                 |               Robust
           lwage |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
            educ |   .1570594   .0525526     2.99   0.003     .0540166    .2601021
           exper |   .1188149   .0229516     5.18   0.000     .0738125    .1638173
         expersq |  -.0023565   .0003684    -6.40   0.000    -.0030788   -.0016342
           black |  -.1232778   .0516278    -2.39   0.017    -.2245074   -.0220482
            smsa |    .100753   .0314458     3.20   0.001     .0390955    .1624105
           south |  -.1431945   .0302678    -4.73   0.000    -.2025423   -.0838466
          smsa66 |   .0150626   .0211683     0.71   0.477    -.0264434    .0565685
          reg662 |   .1027473   .0379324     2.71   0.007     .0283712    .1771235
          reg663 |   .1499316    .037123     4.04   0.000     .0771424    .2227209
          reg664 |   .0475676   .0456953     1.04   0.298    -.0420298     .137165
          reg665 |   .1544801   .0509365     3.03   0.002     .0546061    .2543542
          reg666 |   .1729728    .053543     3.23   0.001      .067988    .2779576
          reg667 |   .1420356   .0523988     2.71   0.007     .0392943    .2447769
          reg668 |  -.0950611   .0587578    -1.62   0.106    -.2102708    .0201486
          reg669 |    .102976   .0426891     2.41   0.016      .019273     .186679
           _cons |   3.236711   .8842789     3.66   0.000     1.502855    4.970567
    ------------------------------------------------------------------------------
    Instrumented:  educ
    Instruments:   exper expersq black smsa south smsa66 reg662 reg663 reg664
                   reg665 reg666 reg667 reg668 reg669 nearc4 nearc2
    ------------------------------------------------------------------------------

Testing Whether a Variable is Endogenous
===========================================

Consider the model

.. math:: y_{1}=\alpha _{1}y_{2}+\mathbf{z}_{1}\mathbf{\delta }_{1}+u_{1},

where :math:`\mathbf{z}_{1}\mathbf{\delta }_{1}` represents a set of exogenous explanatory variables
with coefficients. This includes an
intercept.

We want to test whether :math:`y_{2}` and :math:`u_{1}` are uncorrelated – that is, the null
hypothesis is that :math:`y_{2}` is exogenous and so we can use OLS rather than IV.

In addition to assuming the elements of :math:`\mathbf{z}_{1}` are exogenous – so they act as their
own IVs – we need at least one outside exogenous variable. We might have more than one, collected in
:math:`\mathbf{z}_{2}`.

Write the so-called *reduced form* for :math:`y_{2}`:

.. math:: y_{2}=\mathbf{z\pi }_{2}+v_{2}

where :math:`\mathbf{z\pi }_{2}` denotes a linear function of all exogenous variables.

With :math:`y_{2}` written this way, it is endogenous if and only if

.. math:: Cov(v_{2},u_{1})\neq 0\text{.}

To test the null that :math:`Cov(v_{2},u_{1})=0`, we can write

.. math:: u_{1}=\rho _{1}v_{2}+e_{1}

where the new error :math:`e_{1}` is uncorrelated with
:math:`\mathbf{z}` and :math:`v_{2}`, and therefore :math:`y_{2}`.

Plug in for :math:`u_{1}` into the original equation:

.. math::

   \begin{aligned}
   y_{1} &=&\alpha _{1}y_{2}+\mathbf{z}_{1}\mathbf{\delta }_{1}+u_{1} \\
   &=&\alpha _{1}y_{2}+\mathbf{z}_{1}\mathbf{\delta }_{1}+\rho _{1}v_{2}+e_{1}\end{aligned}

which is an equation that, if we observed :math:`v_{2}`, could be
estimated by OLS.

In the equation

.. math::

   y_{1}=\alpha _{1}y_{2}+\mathbf{z}_{1}\mathbf{\delta }_{1}+\rho
   _{1}v_{2}+e_{1},

:math:`e_{1}` is uncorrelated with :math:`y_{2}`, :math:`\mathbf{z}_{1}`, and :math:`v_{2}`. And we
can estimate :math:`v_{2}` using the first-stage regression.

The two-step testing procedure is

1.  Regress :math:`y_{i2}` on :math:`\mathbf{z}_{i}` to obtain the residuals, :math:`\hat{v}_{i2}`
    (one for each observation :math:`i`): :math:`\hat{v}_{i2}=y_{i2}-\mathbf{z}_{i}
    \mathbf{\hat{\pi}}_{2}` for :math:`i=1,...,n`.

2.  Run the regression (using :math:`n` observations)

.. math:: y_{i1}\text{ on }y_{i2}\text{, }\mathbf{z}_{i1},\hat{v}_{i2}

and use a (robust) :math:`t` test on :math:`\hat{v}_{i2}`.

If we do not have an instrument for :math:`y_{2}`, this regression has perfect collinearity:
:math:`\hat{v}_{i2}` would be an exact linear function of :math:`y_{i2}` and
:math:`\mathbf{z}_{i1}`.

Interestingly, the OLS estimates from step (2) for the coefficients on :math:`y_{i2}` and
:math:`\mathbf{z}_{i1}` are always numerically identical to the 2SLS estimates of the structural
equation. Including the first-stage residuals controls for the endogeneity of :math:`y_{i2}`.

.. admonition:: EXAMPLE: MEAP Scores and Spending

    Do first-stage regression to get residuals.

::

     
     
    . qui reg lavgrexp lfound math4_94 if y95
     
    . predict v2h, resid
     
    . reg math4 lavgrexp math4_94 v2h if y95, robust
     
    Linear regression                                      Number of obs =     501
                                                           F(  3,   497) =  156.02
                                                           Prob > F      =  0.0000
                                                           R-squared     =  0.4802
                                                           Root MSE      =  9.4371
     
    ------------------------------------------------------------------------------
                 |               Robust
           math4 |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
        lavgrexp |   6.639055    2.71628     2.44   0.015     1.302246    11.97586
        math4_94 |   .6392136   .0324096    19.72   0.000     .5755368    .7028904
             v2h |  -14.95647   8.179467    -1.83   0.068    -31.02707    1.114127
           _cons |  -27.02431   23.00407    -1.17   0.241    -72.22152     18.1729
    ------------------------------------------------------------------------------
     
    . * Reject that lavgrexp is exogenous at the 10%, but not the 5%, level
    . * (p-value = .068). Just report both findings.


    Can verify adding v2h and doing OLS produces the 2SLS estimates:

::

    . ivreg math4 math4_94 (lavgrexp = lfound) if y95, robust
     
    Instrumental variables (2SLS) regression               Number of obs =     501
                                                           F(  2,   498) =  230.08
                                                           Prob > F      =  0.0000
                                                           R-squared     =  0.4753
                                                           Root MSE      =  9.4716
     
    ------------------------------------------------------------------------------
                 |               Robust
           math4 |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
        lavgrexp |   6.639055   2.709613     2.45   0.015     1.315373    11.96274
        math4_94 |   .6392136   .0328142    19.48   0.000     .5747422     .703685
           _cons |  -27.02431    22.9201    -1.18   0.239    -72.05632     18.0077
    ------------------------------------------------------------------------------
    Instrumented:  lavgrexp
    Instruments:   math4_94 lfound
    ------------------------------------------------------------------------------
     
    . * The standard errors are different. Should use the ones from ivreg.


Measurement Error and Multiple Indicators
============================================

Suppose we have two test scores to measure unobserved ability. We can use these in an IV strategy to
allow :math:`educ`, say, to be endogenous in a wage equation.

Suppose we think :math:`IQ` and :math:`KWW` (knowledge of the world of work) are indicators of
ability:

.. math::

   \begin{aligned}
   lwage_{i} &=&\beta _{0}+\beta _{1}educ_{i}+\mathbf{r}_{i}\mathbf{\gamma }
   +ability_{i}+e_{i} \\
   IQ_{i} &=&\alpha _{0}+\alpha _{1}ability_{i}+v_{i1} \\
   KWW_{i} &=&\eta _{0}+\eta _{1}ability+v_{i2}\end{aligned}

Have to assume :math:`\alpha _{1}\neq 0` and :math:`\eta _{1}\neq 0` so that :math:`IQ` and
:math:`KWW` are truly indicators of ability.

We normalize the coefficient on :math:`ability` to be one because we cannot estimate its
coefficient, anyway.

Write the setup generally as

.. math::

   \begin{aligned}
   y_{i} &=&\beta _{0}+\beta _{1}x_{i}+\mathbf{r}_{i}\mathbf{\gamma }
   +a_{i}+e_{i} \\
   q_{i1} &=&\alpha _{1}a_{i}+v_{i1} \\
   q_{i2} &=&\eta _{1}a_{i}+v_{i2}\end{aligned}

Drop the intercepts in :math:`q_{i1}` and :math:`q_{i2}` for notational simplicity. With intercepts,
won’t be able to identify the original intercept :math:`\beta _{0}`.

What do we need to assume? That :math:`q_{i1}` and :math:`q_{i2}` are redundant in the equation for
:math:`y_{i}` once we control for :math:`a_{i}`. In conditional expectation terms:

.. math::

   E(y_{i}|x_{i},\mathbf{r}_{i},a_{i},q_{i1},q_{i2})=E(y_{i}|x_{i},\mathbf{r}_{i},a_{i})

so :math:`e_{i}` is uncorrelated with :math:`x_{i}`, :math:`\mathbf{r}_{i},a_{i}`, :math:`q_{i1},`
and :math:`q_{i2}`.  Equivalently, :math:`e_{i}` is uncorrelated with :math:`x_{i}`,
:math:`\mathbf{r}_{i},a_{i}`, :math:`v_{i1},` and :math:`v_{i2}`. This assumption essentially
holds by definition: :math:`a_{i}` is the omitted factor that matters, and :math:`q_{i1}` and
:math:`q_{i2}` are only noisy indicators.

We need some more assumptions.

.. math:: v_{i1}\text{, }v_{i2}\text{ uncorrelated with }(x_{i},\mathbf{r}_{i},a_{i}).

So :math:`v_{i1}`, :math:`v_{i2}` have same properties as classical measurement error.

This is not classical errors-in-variables because we allow :math:`\alpha _{1}`, :math:`\eta _{1}\neq
1`. And we are not interested in a coefficient on :math:`a_{i}`.

Also, :math:`q_{i1}` and :math:`q_{i2}` are not valid proxy variables in the sense that adding them
to an equation estimated by OLS does not solve the endogeneity of :math:`x_{i}`.

Finally, a critical assumption is

.. math:: Cov(v_{i1},v_{i2})=0.

Implies that the two indicators are correlated only through their common dependence on
:math:`a_{i}`:

.. math:: Cov(q_{i1},q_{i2})=\alpha _{1}\eta _{1}\sigma _{a}^{2}>0

Approach: Use :math:`\alpha _{1}\neq 0` to write

.. math::

   a_{i}=(1/\alpha _{1})q_{i1}-(1/\alpha _{1})v_{i1}\equiv \theta _{1}q_{i1}-\theta _{1}v_{i1}

and plug into :math:`y_{i}=\beta _{0}+\beta _{1}x_{i}+\mathbf{r}_{i}\mathbf{\gamma }+a_{i}+e_{i}`:

.. math::

   \begin{aligned}
   y_{i} &=&\beta _{0}+\beta _{1}x_{i}+\mathbf{r}_{i}\mathbf{\gamma }+\theta
   _{1}q_{i1}+e_{i}-\theta _{1}v_{i1} \\
   &\equiv &\beta _{0}+\beta _{1}x_{i}+\mathbf{r}_{i}\mathbf{\gamma }+\theta
   _{1}q_{i1}+c_{i}\end{aligned}

In this estimating equation, only :math:`q_{i1}` is endogenous: it must be correlated with
:math:`v_{i1}` since :math:`q_{i1}=\alpha _{1}a_{i}+v_{i1}` and we assume
:math:`Cov(a_{i},v_{i1})=0`. That’s what makes OLS is inconsistent and why this is different from
the simple proxy variable solution.

Key point: :math:`x_{i}`, and :math:`\mathbf{r}_{i}`, are exogenous in

.. math::

   y_{i}=\beta _{0}+\beta _{1}x_{i}+\mathbf{r}_{i}\mathbf{\gamma }+\theta _{1}q_{i1}+c_{i}

and so they act as their own instruments. We only need an IV for :math:`q_{i1}`. Use :math:`q_{i2}`.

:math:`q_{i2}=\eta _{1}a_{i}+v_{i2}` is uncorrelated with :math:`e_{i}` -- it is redundant in the
structural equation – and we assume it is uncorrelated with :math:`v_{i1}` because :math:`v_{i1}`,
:math:`v_{i2}` are uncorrelated.

Already showed :math:`q_{i2}` and :math:`q_{i1}` are correlated because
:math:`Cov(q_{i1},q_{i2})=\alpha _{1}\eta _{1}\sigma _{a}^{2}`. They are generally partially
correlated, too.

Anyway, we should check the first-stage regression,

.. math:: q_{i1}\text{ on }x_{i}\text{, }\mathbf{r}_{i}\text{, }q_{i2}

to be sure the coefficient on :math:`q_{i2}` has the expected sign and
is statistically different from zero.

Note that :math:`x_{i}` is in this regression because it is exogenous in the estimating equation.

The approach is (1) Check that :math:`q_{i2}` is partially correlated with :math:`q_{i1}`. (2) Add
:math:`q_{i1}` to the original equation and use :math:`q_{i2}` as its IV. No other variable,
including :math:`x_{i}`, needs an IV.

Of course, can reverse the roles of :math:`q_{i1}` and :math:`q_{i2}`. And neither estimator is
efficient.  One can get an efficient estimator using a system method.

Note: If we set :math:`\alpha _{1}=1` and put a coefficient :math:`\delta` on :math:`a_{i}`, then
:math:`\theta _{1}=\delta`, and we can consistently estimate a coefficient on :math:`a_{i}`, too.
That takes us back to Bishop (1989), where his main interest is an estimate of the return to general
intel

Apply this to the Card (1995) data set. Add :math:`IQ` to the equation, use :math:`KWW` as its
instrument.

::

    . * Multiple indicator estimation. First plug in IQ, use KWW as IV.
     
    . * IQ and KWW are clearly correlated:
     
    . corr IQ KWW
    (obs=2040)
     
                 |       IQ      KWW
    -------------+------------------
              IQ |   1.0000
             KWW |   0.4314   1.0000
     
    . * IQ and KWW have a strong, positive partial correlation, too, as
    . * can be seen from the first-stage regression:

::

    . reg IQ KWW educ exper expersq black smsa south smsa66 reg662-reg669, robust
     
    Linear regression                                      Number of obs =    2040
     
    ------------------------------------------------------------------------------
                 |               Robust
              IQ |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
             KWW |   .5378006   .0441837    12.17   0.000     .4511502    .6244509
            educ |    1.48248    .172549     8.59   0.000     1.144088    1.820872
           exper |    -1.7292   .3016272    -5.73   0.000    -2.320733   -1.137668
         expersq |   .0440697   .0153579     2.87   0.004     .0139507    .0741887
           black |  -11.37493    .881554   -12.90   0.000    -13.10377   -9.646077
            smsa |   .4931825   .7445858     0.66   0.508    -.9670525    1.953417
           south |  -.1060845   1.060138    -0.10   0.920     -2.18516    1.972991
          smsa66 |   .6153939   .7117567     0.86   0.387    -.7804588    2.011247
          reg662 |   .5987404   1.171653     0.51   0.609    -1.699032    2.896513
          $\vdots $
          reg669 |  -2.402759   1.347505    -1.78   0.075    -5.045401    .2398842
           _cons |   75.77933   2.978112    25.45   0.000     69.93885    81.61982
    ------------------------------------------------------------------------------

::

    . * Plug in IQ, use KWW as its IV:
     
    . ivreg lwage educ exper expersq black smsa south smsa66 reg662-reg669 
            (IQ = KWW), robust
     
    Instrumental variables (2SLS) regression               Number of obs =    2040
     
    ------------------------------------------------------------------------------
                 |               Robust
           lwage |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
              IQ |   .0134051   .0028071     4.78   0.000        .0079    .0189103
            educ |   .0418418   .0089349     4.68   0.000     .0243192    .0593644
           exper |   .1037625   .0101747    10.20   0.000     .0838086    .1237164
         expersq |  -.0029772   .0005051    -5.89   0.000    -.0039677   -.0019867
           black |   .0102046   .0497352     0.21   0.837    -.0873331    .1077422
            smsa |   .1113983   .0245274     4.54   0.000     .0632968    .1594998
           south |  -.1007785     .03893    -2.59   0.010    -.1771256   -.0244314
          smsa66 |   .0266077   .0238009     1.12   0.264    -.0200693    .0732846
          reg662 |    .106505   .0445429     2.39   0.017     .0191501    .1938598
          $\vdots $
          reg669 |    .159497   .0504051     3.16   0.002     .0606457    .2583482
           _cons |    3.55885   .2306165    15.43   0.000     3.106579    4.011121
    ------------------------------------------------------------------------------
    Instrumented:  IQ
    Instruments:   educ exper expersq black smsa south smsa66 reg662 reg663
                   reg664 reg665 reg666 reg667 reg668 reg669 KWW
    ------------------------------------------------------------------------------

::

    . * Return to schooling is even much lower than OLS (using IQ as a proxy):
     
    . reg lwage educ exper expersq black smsa south smsa66 reg662-reg669 IQ, robust
     
    Linear regression                                      Number of obs =    2061
     
    ------------------------------------------------------------------------------
                 |               Robust
           lwage |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
            educ |   .0698597   .0050796    13.75   0.000     .0598979    .0798215
           exper |   .0946117   .0091788    10.31   0.000     .0766109    .1126125
         expersq |  -.0027179   .0004627    -5.87   0.000    -.0036253   -.0018104
           black |  -.1477184   .0275555    -5.36   0.000    -.2017582   -.0936786
            smsa |   .1230324   .0231002     5.33   0.000       .07773    .1683348
           south |  -.1000138   .0354608    -2.82   0.005    -.1695569   -.0304708
          smsa66 |   .0359497   .0221871     1.62   0.105    -.0075621    .0794614
          reg662 |   .1031028   .0411869     2.50   0.012       .02233    .1838755
          ...
          reg669 |   .1217886   .0460766     2.64   0.008     .0314266    .2121507
              IQ |   .0024693   .0007561     3.27   0.001     .0009866     .003952
           _cons |   4.379296    .111868    39.15   0.000     4.159909    4.598683
    ------------------------------------------------------------------------------

::

    . * Reverse KWW and IQ:
     
    . ivreg lwage educ exper expersq black smsa south smsa66 reg662-reg669 
            (KWW = IQ), robust
     
    Instrumental variables (2SLS) regression               Number of obs =    2040
     
    ------------------------------------------------------------------------------
                 |               Robust
           lwage |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
             KWW |   .0189048   .0058152     3.25   0.001     .0075005    .0303091
            educ |   .0375327   .0129468     2.90   0.004     .0121423    .0629231
           exper |   .0616169    .013281     4.64   0.000     .0355709    .0876628
         expersq |  -.0019635    .000514    -3.82   0.000    -.0029715   -.0009555
           black |  -.0752119   .0424618    -1.77   0.077    -.1584854    .0080615
            smsa |   .1087298   .0239493     4.54   0.000      .061762    .1556976
           south |  -.1014312   .0366071    -2.77   0.006    -.1732228   -.0296396
          smsa66 |   .0269149   .0231848     1.16   0.246    -.0185537    .0723835
          reg662 |    .133262   .0416783     3.20   0.001     .0515252    .2149988
          ...
          reg669 |   .1595275   .0478903     3.33   0.001     .0656081    .2534468
           _cons |   4.601024   .1004687    45.80   0.000     4.403991    4.798057
    ------------------------------------------------------------------------------
    Instrumented:  KWW
    Instruments:   educ exper expersq black smsa south smsa66 reg662 reg663
                   reg664 reg665 reg666 reg667 reg668 reg669 IQ
    ------------------------------------------------------------------------------

If we think the measurement errors :math:`v_{i1}` and :math:`v_{i2}` are correlated with
:math:`x_{i}`, then we need an IV for :math:`x_{i}`, too. Could plug in :math:`IQ` and treat
:math:`educ` and :math:`IQ` exogenous. Use :math:`nearc4` and :math:`KWW` as IVs.



Heterogeneous Return to Education
====================================

Random Coefficient Models and Exogenous Covariates
-------------------------------------------------------

A model with only schooling is

.. math:: lwage_{i}=\alpha +\beta _{i}educ_{i}+u_{i}.

Now :math:`\beta _{i}` is the return to education for person :math:`i`.  It does not depend on the
amount of education, but could depend on other observables and unobservables.

This is generally called a *random coefficient* model. (More precise would be a *random slope*
model.)

Of course, we can’t estimate a different :math:`\beta _{i}` for each person. Instead, hope to learn
something about the distribution (or conditional distribution) of :math:`\beta _{i}`. For example,
does :math:`\beta _{i}` depend on socioeconomic factors or native intelligence? Are some of these
factors unobserved?

Consider a general formulation of the model:

.. math:: y_{i}=\alpha +\beta _{i}x_{i}+u_{i}

:math:`u_{i}` and :math:`\beta _{i}` are two sources of unobservables.

How should we summarize the partial effects :math:`\beta _{i}`? :math:`\beta =E(\beta _{i})` is the
average slope in the population, also called the *average partial effect* *(APE)* or the *population
average effect*. If :math:`x_{i}` is binary, :math:`\beta` is the *average treatment effect (ATE)*.

Or, we might want to know the average value of :math:`\beta _{i}` as a function of other covariates,
:math:`E(\beta _{i}|\mathbf{r}_{i})`. Start with APE.

What would :math:`x_{i}` exogenous entail in this model? Zero correlation does not quite work
because of the interaction :math:`\beta _{i}x_{i}`. *Mean independence* is enough:

.. math::

   \begin{aligned} E(u_{i}|x_{i}) &=&E(u_{i})=0 \\ E(\beta _{i}|x_{i}) &=&E(\beta
   _{i})=\beta\end{aligned}

:math:`E(u_{i})=0` is without loss of generality with :math:`\alpha` in the model.

Suppose that :math:`x_{i}` is exogenous in the way stated. Then an important result is that OLS of
:math:`y_{i}` on :math:`x_{i}` – that is, acting as if the slope is constant – consistently
estimates :math:`\beta`, the APE.

Argument is simple:

.. math::

   \begin{aligned}
   E(y_{i}|x_{i}) &=&\alpha +E(\beta _{i}x_{i}|x_{i})+E(u_{i}|x_{i}) \\
   &=&\alpha +E(\beta _{i}|x_{i})x_{i}+0 \\
   &=&\alpha +\beta x_{i}\end{aligned}

We already know OLS is consistent for :math:`\alpha ` and :math:`\beta`.

If we write :math:`c_{i}=\beta _{i}-\beta` as the individual-specific deviation from the average,

.. math:: y_{i}=\alpha +\beta x_{i}+u_{i}+c_{i}x_{i}

and the presence of :math:`c_{i}x_{i}` causes the error :math:`u_{i}+c_{i}x_{i}` to be
heteroskedastic (even if :math:`u_{i}` and :math:`c_{i}` are not).

More generally, OLS is consistent for the APEs in the more general model

.. math::

   \begin{aligned} y_{i} &=&\alpha +\mathbf{x}_{i}\mathbf{\beta }_{i}+u_{i} \\
   E(u_{i}|\mathbf{x}_{i}) &=&0 \\ E(\mathbf{\beta }_{i}|\mathbf{x}_{i}) &=&\mathbf{\beta
   }\end{aligned}

Correlated Random Coefficients 
-----------------------------------

Real interest centers on allowing :math:`x_{i}` to be endogenous with respect to both :math:`u_{i}`
(the additive error) and :math:`\beta _{i}`.

When :math:`\beta _{i}` is allowed to be correlated with :math:`x_{i}`, the model

.. math:: y_{i}=\alpha +\beta _{i}x_{i}+u_{i}=\alpha _{i}+\beta _{i}x_{i}

is called a *correlated random coefficient (CRC)* model. Can think of the model as having an
individual-specific intercept (:math:`\alpha _{i}=\alpha+u_{i}`) and slope.

In wage-education case, expect :math:`\beta _{i}\geq 0` for all or most :math:`i`. Makes sense that
those with higher :math:`\beta _{i}` will tend to choose higher :math:`x_{i}`. Another source of
self-selection.

Lots of other examples, too. If :math:`y_{i}=score_{i}` and :math:`x_{i}=classize_{i}`, expect
:math:`\beta _{i}\leq 0` for most children.  Could be that kids with low SES benefit more from
smaller classes – :math:`\beta _{i}<0` and larger in magnitude – but are more likely to be in larger
classes – :math:`Cov(\beta _{i},x_{i})<0`.

Even if have an IV for :math:`x_{i}`, generally cannot estimate :math:`\beta =E(\beta _{i})`. But
sometimes we can just use usual IV.

Write

.. math:: \beta _{i}=\beta +c_{i}\text{, }E(c_{i})=0

:math:`\beta` is the average return in the population and :math:`c_{i}` is the individual-specific
deviation from the average.

Suppose we have :math:`z_{i}` such that

.. math:: E(u_{i}|z_{i})=E(c_{i}|z_{i})=0

so :math:`x_{i}` is exogenous with respect to both forms of heterogeneity.

Write

.. math::

   \begin{aligned} y_{i} &=&\alpha +\beta x_{i}+u_{i}+c_{i}x_{i} \\ &=&\alpha +\beta
   x_{i}+e_{i}\end{aligned}

Is :math:`z_{i}` uncorrelated with composite error :math:`e_{i}=u_{i}+c_{i}x_{i}`? :math:`z_{i}`
is uncorrelated with :math:`u_{i}`, but :math:`c_{i}x_{i}` is the problem because :math:`x_{i}` is
endogenous.

Wooldridge (2003, *Economics Letters*):

1.  :math:`E(c_{i}x_{i})` is not usually zero because we think :math:`x_{i}` is endogenous, but this in
    itself only affects the intercept.

2.  Assume the conditional covariance between :math:`\beta _{i}` and :math:`x_{i}` does not depend on
    :math:`z_{i}`:

.. math:: Cov(\beta _{i},x_{i}|z_{i})=Cov(\beta _{i},x_{i})

Under this assumption, can show the usual IV estimator (2SLS more generally) is consistent for
:math:`\beta`, the average effect.

* Key condition can hold when :math:`x_{i}` is continuous – maybe education is close enough – but
  not usually when :math:`x_{i}` is discrete (say, binary).

Same conclusions hold if we allow :math:`\beta _{i}` to depend on observables. Let
:math:`\mathbf{r}_{i}` be a vector of observable variables with mean :math:`\mathbf{\mu
}_{\mathbf{r}}`. Write

.. math::

   \beta _{i}=\beta +\mathbf{(r}_{i}-\mathbf{\mu }_{\mathbf{r}})\mathbf{\delta } +c_{i}

and also add :math:`\mathbf{r}_{i}` directly to the equation:

.. math::

   y_{i}=\alpha +\beta x_{i}+\mathbf{r}_{i}\mathbf{\gamma }+x_{i}\mathbf{(r}
   _{i}-\mathbf{\mu }_{\mathbf{r}})\mathbf{\delta }+u_{i}+c_{i}x_{i}

Subtracting off :math:`\mathbf{\mu }_{\mathbf{r}}` in the interaction ensures the coefficient on
:math:`x_{i}` is the APE:

.. math:: E(\beta _{i})=\beta \text{.}

If :math:`(x_{i},\mathbf{r}_{i})` are exogenous in the sense :math:`E(u_{i}|x_{i},\mathbf{r}_{i})=E(c_{i}|x_{i},\mathbf{r}_{i})=0`, can use OLS.

In practice, run the regression

.. math::

   y_{i}\text{ on }x_{i},\text{ }\mathbf{r}_{i}\text{, }x_{i}(\mathbf{r}_{i}-
   \mathbf{\bar{r}})\text{, }i=1,...,n

where :math:`\mathbf{\bar{r}}` is the sample average.

Saw this already when the only interaction was between schooling and IQ.

Again, suppose we have an exogenous IV \ :math:`z_{i}` for :math:`x_{i}`. In

.. math::

   y_{i}=\alpha +\beta x_{i}+\mathbf{r}_{i}\mathbf{\gamma }+x_{i}\mathbf{(r}
   _{i}-\mathbf{\mu }_{\mathbf{r}})\mathbf{\delta }+u_{i}+c_{i}x_{i}

:math:`x_{i}` and all interactions :math:`x_{i} (\mathbf{r}_i-\mathbf{\mu}_r)` are
endogenous.

Natural instruments:

.. math:: \lbrack z_{i},z_{i}\mathbf{(r}_{i}-\mathbf{\mu }_{\mathbf{r}})]

In practice, replace :math:`\mathbf{\mu }_{\mathbf{r}}` with :math:`\mathbf{\bar{r}}`, as usual.

Wooldridge results apply to this case.

What happens if the constant conditional covariance assumption fails? Card (2001) argues that, even
in the case of schooling (treated as roughly continuous), the constant conditional covariance
assumption might fail.

Will consider the case of binary :math:`x_{i}` -- where the constant covariance assumption must fail
– later under treatment effects.

A Control Function Approach to CRC Models
----------------------------------------------

A different approach to estimating the mean effect :math:`E(\beta _{i})` or the conditional mean
:math:`E(\beta _{i}|\mathbf{r}_{i})` is to apply a *control function* method. This approach is due
to Garen (1984, *Econometrica*).

We still need at least on instrument for :math:`x_{i}`, but we add a reduced form residual to an
estimating equation.

Start with the equation, derived earlier:

.. math::

   y_{i}=\alpha +\beta x_{i}+\mathbf{r}_{i}\mathbf{\gamma }+x_{i}\mathbf{(r}
   _{i}-\mathbf{\mu }_{\mathbf{r}})\mathbf{\delta }+u_{i}+c_{i}x_{i}

where

.. math::

   \beta _{i}=\beta +\mathbf{(r}_{i}-\mathbf{\mu }_{\mathbf{r}})\mathbf{\delta }
   +c_{i}

Write a reduced form for :math:`x_{i}` as

.. math:: x_{i}=\eta +\theta z_{i}+\mathbf{r}_{i}\mathbf{\pi }+v_{i}

where, by construction, :math:`v_{i}` has zero mean and is uncorrelated with :math:`z_{i} ` and
:math:`\mathbf{r}_{i}`.

:math:`v_{i}` is the part of :math:`x_{i}` that is endogenous to :math:`(u_{i},c_{i})`.

Assume that :math:`(u_{i},c_{i},v_{i})` are actually independent of :math:`(z_{i},\mathbf{r}_{i})`.
Then

.. math::

   \begin{aligned} E(u_{i}|z_{i},\mathbf{r}_{i},v_{i}) &=&E(u_{i}|v_{i}) \\
   E(c_{i}|z_{i},\mathbf{r}_{i},v_{i}) &=&E(c_{i}|v_{i})\end{aligned}

So we need to model the relationship between the unobservable heterogeneities :math:`(u_{i},c_{i})`
and :math:`v_{i}`. Assume the two expectations are linear

.. math::

   \begin{aligned} E(u_{i}|v_{i}) &=&\rho v_{i} \\ E(c_{i}|v_{i}) &=&\xi v_{i}\end{aligned}

Now, note that :math:`x_{i}` is a function of :math:`(z_{i},\mathbf{r}_{i},v_{i})`. Take the
expectation of

.. math::

   y_{i}=\alpha +\beta x_{i}+\mathbf{r}_{i}\mathbf{\gamma }+x_{i}\mathbf{(r}
   _{i}-\mathbf{\mu }_{\mathbf{r}})\mathbf{\delta }+u_{i}+c_{i}x_{i}

conditional on :math:`(z_{i},\mathbf{r}_{i},v_{i})`:

.. math::

   \begin{aligned}
   E(y_{i}|z_{i},\mathbf{r}_{i},v_{i}) &=&\alpha +\beta x_{i}+\mathbf{r}_{i}
   \mathbf{\gamma }+x_{i}\mathbf{(r}_{i}-\mathbf{\mu }_{\mathbf{r}})\mathbf{
   \delta } \\
   &&+E(u_{i}|z_{i},\mathbf{r}_{i},v_{i})+E(c_{i}|z_{i},\mathbf{r}
   _{i},v_{i})x_{i}\end{aligned}

and use the assumptions on :math:`E(u_{i}|z_{i},\mathbf{r}_{i},v_{i})`
and :math:`E(c_{i}|z_{i},\mathbf{r}_{i},v_{i})`:

.. math::

   E(y_{i}|z_{i},\mathbf{r}_{i},v_{i})=\alpha +\beta x_{i}+\mathbf{r}_{i}
   \mathbf{\gamma }+x_{i}\mathbf{(r}_{i}-\mathbf{\mu }_{\mathbf{r}})\mathbf{
   \delta }+\rho v_{i}+\xi v_{i}x_{i}

The variable :math:`v_{i}` – the reduced form error for :math:`x_{i}` -- is the control
function.Because of the random coefficient model, :math:`v_{i}` appears interacted with :math:`x_{i}`, too.

Note how :math:`x_{i}` is exogenous in the CF equation. If we had data on :math:`v_{i}`, we could
just use OLS.  Instead, write :math:`v_{i}=x_{i}-\eta -\theta z_{i}-\mathbf{r}_{i}\mathbf{\pi }` and
we can estimate the parameters by the first-stage regression.

Garen’s Procedure:

1.  Estimate the reduced form (first-stage regression) for :math:`x_{i}`.
    Obtain the residuals, :math:`\hat{v}_{i}`.

2.  Run the OLS regression

    .. math::

       y_{i}\text{ on }x_{i},\text{ }\mathbf{r}_{i}\mathbf{\gamma }\text{, }x_{i}
       \mathbf{(r}_{i}-\mathbf{\bar{r}})\text{, }\hat{v}_{i}\text{, }\hat{v}
       _{i}x_{i}

    to consistently estimate all parameters.

Without the interactions :math:`x_{i}\mathbf{(r}_{i}-\mathbf{\bar{r}})` and
:math:`\hat{v}_{i}x_{i}`, method would be identical to 2SLS. That is, all coefficients (other than
those on :math:`\hat{v}_{i}`) would be identical to 2SLS.

Usual (or robust) OLS inference is not valid because the :math:`\hat{v}_{i}` have been estimated
using the same data.  So, it is a little harder to use than just 2SLS. Can use a
bootstrapping procedure (resampling method) and let the computer work.

Garen’s procedure uses more assumptions than 2SLS, so it is less robust. But can look at the
coefficient on :math:`\hat{v}_{i}x_{i}` to see if the interaction might be important.
(Unfortunately, :math:`t` test not strictly valid; does not account for first-stage estimation.)

If the null hypothesis is that :math:`x_{i}` is exogenous, then do a joint :math:`F` test of
:math:`\hat{v}_{i}` and :math:`\hat{v}_{i}x_{i}` (two restrictions). This *is* valid (but may want
to make it robust to heteroskedasticity).

If :math:`\hat{v}_{i}x_{i}` is not included the CF regression is

.. math::

   y_{i}\text{ on }x_{i},\text{ }\mathbf{r}_{i}\mathbf{\gamma }\text{, }x_{i}
   \mathbf{(r}_{i}-\mathbf{\bar{r}})\text{, }\hat{v}_{i}

Can use a :math:`t` statistic on :math:`\hat{v}_{i}` to test :math:`H_{0}:\rho =0`, which means
:math:`x_{i}` is exogenous.  (Estimates of other coefficients are 2SLS.)

Apply to Card (1995) data without interactions with exogenous variables.

::

    . qui reg educ nearc4 exper expersq black smsa south smsa66 reg662-reg669,
          robust
     
    . predict vh, resid
     
    . gen vheduc = vh*educ
     
    . reg lwage educ exper expersq black smsa south smsa66 reg662-reg669 
          vh vheduc, robust
     
    Linear regression                                      Number of obs =    3010
     
    ------------------------------------------------------------------------------
                 |               Robust
           lwage |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
            educ |   .1309872   .0515575     2.54   0.011     .0298955    .2320789
           exper |   .1080622   .0221909     4.87   0.000     .0645512    .1515731
         expersq |  -.0023443   .0003195    -7.34   0.000    -.0029707   -.0017179
           black |  -.1459074   .0501094    -2.91   0.004    -.2441597    -.047655
            smsa |   .1119439   .0293265     3.82   0.000     .0544419     .169446
           south |   -.144428   .0281972    -5.12   0.000    -.1997158   -.0891402
          smsa66 |   .0191529   .0199871     0.96   0.338     -.020037    .0583427
          reg662 |   .0990291   .0353443     2.80   0.005     .0297274    .1683308
          $\vdots $
          reg669 |   .1077781   .0396145     2.72   0.007     .0301037    .1854525
              vh |  -.0768732   .0536161    -1.43   0.152    -.1820014     .028255
          vheduc |   .0015117   .0010059     1.50   0.133    -.0004607    .0034841
           _cons |   3.670462   .8672803     4.23   0.000     1.969935    5.370988
    ------------------------------------------------------------------------------
     
    . test vh vheduc
     
     ( 1)  vh = 0
     ( 2)  vheduc = 0
     
           F(  2,  2992) =    1.72
                Prob > F =    0.1801
     
    . reg lwage educ exper expersq black smsa south smsa66 reg662-reg669 vh, robust
     
    Linear regression                                      Number of obs =    3010
     
    ------------------------------------------------------------------------------
                 |               Robust
           lwage |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
            educ |   .1315038   .0515908     2.55   0.011     .0303467    .2326609
           exper |   .1082711   .0222338     4.87   0.000     .0646761    .1518661
         expersq |  -.0023349   .0003223    -7.24   0.000    -.0029669    -.001703
           black |  -.1467758   .0501315    -2.93   0.003    -.2450714   -.0484802
            smsa |   .1118083   .0293274     3.81   0.000     .0543045    .1693122
           south |  -.1446715   .0281784    -5.13   0.000    -.1999226   -.0894204
          smsa66 |   .0185311   .0200058     0.93   0.354    -.0206953    .0577576
          reg662 |   .1007678   .0353768     2.85   0.004     .0314025    .1701331
          $\vdots $
          reg669 |   .1078142   .0396825     2.72   0.007     .0300065     .185622
              vh |  -.0570621   .0518297    -1.10   0.271    -.1586874    .0445633
           _cons |   3.666152   .8678994     4.22   0.000     1.964412    5.367892
    ------------------------------------------------------------------------------
     
     
     
    . reg lwage educ exper expersq black smsa south smsa66 reg662-reg669, robust
     
    Linear regression                                      Number of obs =    3010
                                                           F( 15,  2994) =   91.31
                                                           Prob > F      =  0.0000
                                                           R-squared     =  0.2998
                                                           Root MSE      =  .37228
     
    ------------------------------------------------------------------------------
                 |               Robust
           lwage |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
            educ |   .0746933   .0036462    20.48   0.000     .0675439    .0818427
           exper |    .084832   .0067548    12.56   0.000     .0715875    .0980765
         expersq |   -.002287   .0003194    -7.16   0.000    -.0029133   -.0016608
           black |  -.1990123   .0181644   -10.96   0.000    -.2346282   -.1633964
            smsa |   .1363845   .0192172     7.10   0.000     .0987042    .1740648
           south |   -.147955   .0280346    -5.28   0.000     -.202924    -.092986
          smsa66 |   .0262417   .0185908     1.41   0.158    -.0102102    .0626937
          reg662 |   .0963672   .0350964     2.75   0.006     .0275518    .1651826
          $\vdots $
          reg669 |   .1185698   .0387784     3.06   0.002     .0425347    .1946048
           _cons |   4.620807    .074229    62.25   0.000     4.475262    4.766352
    ------------------------------------------------------------------------------
     

