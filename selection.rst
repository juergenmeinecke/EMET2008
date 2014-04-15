Sample Selection
********************

The General Sample Selection Problem
======================================

Truncated sampling is a special case of a *selected sample*.

Nonrandom sample selection can occur when we have missing data, often due to nonreporting. For
example, we may initial collect a random sample from the population, but some people may refuse to
report their incomes.

If data are missing on some variables the resulting data set may effectively be a nonrandom sample.
Might someone’s refusable to report income be systematically related to other individual
characteristics (observed and unobserved)?

Sometimes economic agents self select into or out of certain behaviors – such as labor force
participation – and this results in unobservable variables (such as hourly wage).

We discuss the general problem of sample selection in the context of the population model

.. math::

   \begin{aligned} y &=&\beta _{0}+\beta _{1}x_{1}+\beta _{2}x_{2}+\cdots +\beta _{k}x_{k}+u \\
   E(u|x_{1},x_{2},...,x_{k}) &=&0\end{aligned}

We know that if the explanatory variables do not suffer from perfect collinearity, and we have
access to a random sampling, then OLS on the random sampling is generally unbiased.

If we assume only :math:`Cov(x_{j},u)=0`, :math:`j=1,...,k`, then OLS is still consistent.

Consider a general situation where we do not have a complete set of data for some units. Truncation
is an extreme case: We have no data for some units and so those units do not even appear in the data
set.

Often the missing data problem is less extreme.

Packages such as Stata recognize incomplete observations in all commands. If data are missing on one
or more variables, those units are simply dropped.

.. admonition:: Example: Economic Performance and Math Aptitude

    The data set in ECONMATH.DTA allows us to look for a link between performance in introductory
    economics and math aptitude and preparation.

    The sample is from students at Michigan State University.

    Notice that 42 observations are missing on the ACT math score (:math:`actmth`). These are simply
    dropped from the regression, even though we have data on the other variables.

::

    . use econmath
     
    . des score econhs hsgpa actmth calculus
     
                  storage  display     value
    variable name   type   format      label      variable label
    --------------------------------------------------------------------------------
    score           float  %9.0g                  course total, as a percent
    econhs          byte   %8.0g                  =1 if economics in high school
    hsgpa           float  %9.0g                  high school GPA
    actmth          byte   %8.0g                  ACT math score
    calculus        byte   %9.0g                  =1 if taken calculus course
     
    . sum score econhs hsgpa actmth calculus
     
        Variable |       Obs        Mean    Std. Dev.       Min        Max
    -------------+--------------------------------------------------------
           score |       856    72.59945    13.40065   19.53125    98.4375
          econhs |       856    .3703271    .4831746          0          1
           hsgpa |       856     3.34243    .3434283      2.355       4.26
          actmth |       814     23.2113    3.773354         12         36
        calculus |       856    .6764019    .4681222          0          1

::

    . reg score econhs hsgpa actmth calculus, robust
     
    Linear regression                                      Number of obs =     814
                                                           F(  4,   809) =   60.49
                                                           Prob > F      =  0.0000
                                                           R-squared     =  0.2444
                                                           Root MSE      =  11.594
     
    ------------------------------------------------------------------------------
                 |               Robust
           score |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
          econhs |  -.8678812    .832972    -1.04   0.298    -2.502923    .7671602
           hsgpa |   10.40029    1.23233     8.44   0.000     7.981351    12.81923
          actmth |   .8927088   .1259602     7.09   0.000     .6454614    1.139956
        calculus |   3.868867   .9749062     3.97   0.000     1.955223    5.782511
           _cons |   14.80766   4.187566     3.54   0.000     6.587888    23.02744
    ------------------------------------------------------------------------------

::

    . reg score econhs hsgpa calculus, robust
     
    Linear regression                                      Number of obs =     856
                                                           F(  3,   852) =   61.57
                                                           Prob > F      =  0.0000
                                                           R-squared     =  0.1778
                                                           Root MSE      =  12.172
     
    ------------------------------------------------------------------------------
                 |               Robust
           score |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
          econhs |  -1.305948   .8608793    -1.52   0.130     -2.99564     .383745
           hsgpa |   13.32902   1.180634    11.29   0.000     11.01173    15.64631
        calculus |   5.780191   .9497385     6.09   0.000      3.91609    7.644293
           _cons |   24.62204   4.036841     6.10   0.000     16.69872    32.54536
    ------------------------------------------------------------------------------
     
    . reg score econhs hsgpa calculus if actmth != ., robust
     
    Linear regression                                      Number of obs =     814
                                                           F(  3,   810) =   63.66
                                                           Prob > F      =  0.0000
                                                           R-squared     =  0.1958
                                                           Root MSE      =  11.953
     
    ------------------------------------------------------------------------------
                 |               Robust
           score |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
          econhs |  -1.369206    .874339    -1.57   0.118    -3.085443    .3470315
           hsgpa |   13.93896   1.214955    11.47   0.000     11.55412    16.32379
        calculus |   5.892206   .9701851     6.07   0.000     3.987832    7.796579
           _cons |    22.5113   4.146497     5.43   0.000     14.37215    30.65045
    ------------------------------------------------------------------------------
     
    . * Most of the change in coefficients is from dropping actmth, not from
    . * the change in samples.

In the previous example, the fraction of missing data is fairly small – less than 5%. But this
sample has already been reduced by dropping some students with too much missing information to be
useful.

The general question is: When does dropping units due to missing data systematically bias the OLS
estimators?

To study this question it is helpful to define a :math:`s_{i}` be a binary variable indicating a
complete set: :math:`s_{i}=1` if we observe a full set of data (on the :math:`x_{j}` and :math:`y`)
and :math:`s_{i}=0` otherwise. :math:`s_{i}` is sometimes called a selection indicator.

The following thought experiment is useful: We randomly draw unit :math:`i` from the population and
we use that unit in the estimation if and only if :math:`s_{i}=1` (by definition).

The key is: How is the outcome on :math:`s_{i}` determined?

For random draw :math:`i` from the population, we can write

.. math::

   y_{i}=\beta _{0}+\beta _{1}x_{i1}+\beta _{2}x_{i2}+\cdots +\beta _{k}x_{ik}+u_{i}

where we absorb the intercept into :math:`\mathbf{x}_{i}`.

Multiplying through by :math:`s_{i}` gives

.. math::

   s_{i}y_{i}=\beta _{0}s_{i}+\beta _{1}s_{i}x_{i1}+\cdots +\beta
   _{k}s_{i}x_{ik}+s_{i}u_{i}

If :math:`s_{i}=0` this equation just says :math:`0=0+0`. When :math:`s_{i}=1` we get the original
equation.

OLS on the *selected sample* is just the OLS regression

.. math:: s_{i}y_{i}\text{ on }s_{i}\text{, }s_{i}x_{i1}\text{, }\ldots s_{i}x_{ik}\text{, }i=1,\ldots ,n.

Mechanically, this is the same as what Stata does when it encounters any missing data.

Because we are only using units with a full set of variables, the OLS estimator on the selected
sample is sometimes called the *complete cases estimator*.

We can study the unbiasedness and consistency of OLS on the selected sample by looking at its
population analog. Let :math:`s` denote a binary random variable with a distribution determining
selection.

.. math:: sy=\beta _{0}s+\beta _{1}sx_{1}+\beta _{2}sx_{2}+\cdots +\beta _{k}sx_{k}+su

Now, we need :math:`su` to have mean zero and, more importantly, to be uncorrelated with each of the
explanatory variables, :math:`sx_{j}`:

.. math::

   \begin{aligned} E(su) &=&0 \\ E(sx_{j}su) &=&E(sx_{j}u)=0\text{, }j=1,...,k\end{aligned}

where we use :math:`s^{2}=s`.

One possibility is that selection is entirely random, in which case :math:`s` is independent of
:math:`(\mathbf{x},u)`. The probability of keeping an observations, say :math:`\rho =P(s=1)`, means
that we effectively flip a biased coin that has probability :math:`\rho ` of turning up heads.
:math:`s_{i}=1` if the coin flip for unit :math:`i` comes up heads.

If :math:`s` is indepenent of :math:`(\mathbf{x},u)` (and therefore :math:`y`),

.. math::

   \begin{aligned}
   E(su) &=&E(s)E(u)=0 \\
   E(sx_{j}u) &=&E(s)E(x_{j}u)=0\end{aligned}

where we impose the population assumptions

.. math:: E(u)=0,\text{ }E(x_{j}u)=Cov(x_{j},u)=0

It makes sense that randomly dropping observations should not cause systematic bias, and the above
calculations show that.

We must rule out perfect collinearity in the selected subpopulation. If the :math:`x_{j}` do not
vary enough when :math:`s=1`, estimation of all :math:`\beta _{j}` may not be possible.

The key condition for unbiasedness of OLS is

.. math:: E(su|sx_{1},...,sx_{k})=0.

This condition holds if the zero conditional mean assumption holds *in the population*,

.. math:: E(u|x_{1},...,x_{k})=0,

and :math:`s` is a function of :math:`(x_{1},...,x_{k})`.

The condition :math:`E(su|sx_{1},...,sx_{k})=0` also generally holds if :math:`s` is a function of
:math:`(x_{1},...,x_{k})` and an unobserved variable, say :math:`v`, where :math:`u` is independent
of :math:`(x_{1},...,x_{k},v)`.

Situations where selection is a function of the exogenous variables (and maybe independent
unobservables) is called *exogenous selection*.

Consistency of 2SLS on the selected sample can also be shown to hold if selection is exogenous, but
in this case selection cannot be based on endogenous explanatory variables. It can only be based on
the exogenous variables (including the IVs).

For panel data, one advantage of fixed effects over random effects comes with unbalanced panels.
Fixed effects allows selection to be arbitrarily correlated with the unobserved effect whereas
random effects does not. Both require selection to be uncorrelated with the idiosyncratic error.

In general it is hard to detect selection bias. If data are missing on one or more variables we can
rarely know if the missingness is correlated with :math:`u`.

Methods to deal with missing data usually assume that the missingness is a function of exogenous
variables. Then, there are various ways to fill in the missing data (known as imputation). Just
using the complete cases does not cause systematic bias (but it may be less efficient than some
imputation methods).

Incidental Truncation 
========================

In some cases, we have missing data only on :math:`y`, and we have exogenous variables that we think
predict selection. Often, whether we oberve :math:`y` is associated with some economic behavior (as
opposed to simply not reporting a variable).

For example, labor economists are interested in estimating a wage offer equation, which (indirectly)
measures productivity for anyone of working age.

The equation describing the working-age population is

.. math::

   lwage^{o}=\beta _{0}+\beta _{1}x_{1}+\beta _{2}x_{2}+\ldots +\beta _{k}x_{k}+u,

where :math:`lwage^{o}` is the log of of the wage offer.

We only observe :math:`lwage^{o}` if someone is working.

The selection indicator, :math:`s`, equals one if a woman is in the labor force. Call this variable
:math:`inlf`. If :math:`inlf=1` then the observed wage is :math:`lwage^{o}`; otherwise this variable
is missing (not zero!).

The decision to enter the labor market may be correlated with some of the :math:`x_{j}` but also
with :math:`u`. This is an example of a self-selection problem.

Consider the general equation

.. math::

   \begin{aligned} y &=&\beta _{0}+\beta _{1}x_{1}+\beta _{2}x_{2}+\cdots +\beta _{k}x_{k}+u \\
   E(u|x_{1},x_{2},...,x_{k}) &=&0\end{aligned}

where :math:`y` is observed only if :math:`s=1`.

If :math:`s` is correlated with :math:`u` we usually say there is **endogenous selection**.
(Technically, what matters is whether :math:`s` is correlated with :math:`u` after we partial out
the :math:`x_{j}`.)

If in addition to always observing the :math:`x_{j}` we always observe at least one other exogenous
variable that predicts selection, we can obtain consistent estimators of the :math:`\beta _{j}`.

We model selection as depending on exogenous variables and an unobservable. Write

.. math::

   \begin{aligned} y &=&\beta _{0}+\beta _{1}x_{1}+\beta _{2}x_{2}+\cdots +\beta _{k}x_{k}+u \\ s
   &=&1[\gamma _{0}+\gamma _{1}z_{1}+\cdots \gamma _{m}z_{m}+v\geq 0]\end{aligned}

where the :math:`z_{h}` are variables that predict selection.

We assume, at a minimum,

.. math:: E(u|\mathbf{x},\mathbf{z})=0.

We should view the selection equation the same way we view a reduced form equation for an endogenous
explanatory variable in the context of two stage least squares.

In particular, because all of the :math:`x_{j}` are exogenous, they should all be included among the
:math:`z_{h}`. Some authors disagree about this. But leaving an :math:`x_{j}` out of
:math:`\mathbf{z}` generally results in inconsistency if :math:`x_{j}` is partially correlated with
selection.

In what follows, take :math:`\mathbf{x}` to be a subset of :math:`\mathbf{z}`. Again, this is the
safe choice.

As with instrumental variables, we need at least one element in :math:`\mathbf{z}` that is not also
in :math:`\mathbf{x}`. So :math:`\mathbf{x}` is a *strict* subset of :math:`\mathbf{z}`. In other
words, we need an **exclusion restriction**: There must be an exogenous variable that influences
:math:`s` but does not affect :math:`y`.

How can we use the selection equation to get consistent estimators of the :math:`\beta _{j}`? Assume
that :math:`(u,v)` has a jointly normal distribution independent of :math:`\mathbf{z}` (and
therefore of :math:`\mathbf{x}`). In fact, assume :math:`v` is standard normal. Then

.. math:: E(u|\mathbf{z},v)=E(u|v)=\rho v

for some parameter :math:`\rho`.

Then

.. math:: E(y|\mathbf{z},v)=\mathbf{x\beta }+\rho v.

What we need is

.. math:: E(y|\mathbf{z},s=1)

because we observe :math:`y` only when :math:`s=1`.

It can be shown that

.. math::

   E(y|\mathbf{z},s=1)=\mathbf{x\beta }+\rho E(v|\mathbf{z},s=1)=\mathbf{x\beta 
   }+\rho \lambda (\mathbf{z\gamma )}

where :math:`\lambda (\cdot )=\phi (\cdot )/\Phi (\cdot )` is the
inverse Mills ratio (IMR).

The sample selection problem can be solved by
including :math:`\lambda (\mathbf{z\gamma )}` as an additional
explanatory variable using only the selected sample.

One problem is we do not know
:math:`\mathbf{\gamma }`. Nevertheless, we can estimate it using data on
:math:`(s,\mathbf{z})`. Because :math:`v` follows a standard normal
distribution, :math:`s` follows a probit model:

.. math:: P(s=1|\mathbf{z})=\Phi (\gamma _{0}+\gamma _{1}z_{1}+...+\gamma _{m}z_{m}).

Remember, we are assuming all :math:`x_{j}`
are in this model.

Two-Step Sample Selection Correction
---------------------------------------

1.  Using the random sample :math:`\{(s_{i},\mathbf{z}_{i}):i=1,2,...,n\}`, do probit of :math:`s_{i}`
    on the :math:`z_{ih}`. Obtain the estimated IMRs,

    .. math:: \hat{\lambda}_{i}=\lambda
       (\hat{\gamma}_{0}+\hat{\gamma}_{1}z_{i1}+...+\hat{\gamma}_{m}z_{im});

    we only need these for the :math:`s_{i}=1` subsample.


2.  Run the OLS regression, on the selected sample (:math:`s_{i}=1`),

    .. math:: y_{i}\text{ on }\mathbf{x}_{i}\text{, }\hat{\lambda}_{i}

This two-step method produces consistent and asymptotically normal estimators of the :math:`\beta
_{j}` and :math:`\rho`.

In general, the first-stage estimation of the IMRs needs to be accounted for in obtaining standard
errors in the second stage. This is hard theoretically but has been programmed in Stata.

If :math:`\rho =0` then :math:`u` and :math:`v` are uncorrelated, and there is no sample selection
problem. A standard :math:`t` test on the IMR is a valid test of the null of *no* selection bias.

The ``heckman`` command in Stata expects the regression equation followed by the selection equation.
We have discussed what is called **Heckman’s two-step method**. There is also a joint maximum
likelihood procedure that does everything in one step. It is asymptotically more efficient but is
somewhat less robust.

The Heckman procedure in Stata reports an estimate of the correlation between :math:`u` and
:math:`v`. (Unfortunately the text notation is in conflict with Stata, which uses rho as the
correlation. In our notation, :math:`\rho =Cov(v,u)`; it is not the correlation.). Looking at the
rhoreported by Stata can be informative.

.. admonition:: Example: Wage Offer for Married Women (MROZ.DTA)

   Estimating a wage offer equation:

::

     
    . use mroz
     
    . des lwage inlf nwifeinc
     
                  storage  display     value
    variable name   type   format      label      variable label
    -------------------------------------------------------------------------------
    lwage           float  %9.0g                  log(wage)
    inlf            byte   %9.0g                  =1 if in lab frce, 1975
    nwifeinc        float  %9.0g                  (faminc - wage*hours)/1000
    . sum lwage inlf educ kidslt6 nwifeinc
     
        Variable |       Obs        Mean    Std. Dev.       Min        Max
    -------------+--------------------------------------------------------
           lwage |       428    1.190173    .7231978  -2.054164   3.218876
            inlf |       753    .5683931    .4956295          0          1
            educ |       753    12.28685    2.280246          5         17
         kidslt6 |       753    .2377158     .523959          0          3
        nwifeinc |       753    20.12896     11.6348  -.0290575         96

::

    . reg lwage educ exper expersq
     
          Source |       SS       df       MS              Number of obs =     428
    -------------+------------------------------           F(  3,   424) =   26.29
           Model |  35.0222967     3  11.6740989           Prob > F      =  0.0000
        Residual |  188.305144   424  .444115906           R-squared     =  0.1568
    -------------+------------------------------           Adj R-squared =  0.1509
           Total |  223.327441   427  .523015084           Root MSE      =  .66642
     
    ------------------------------------------------------------------------------
           lwage |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
            educ |   .1074896   .0141465     7.60   0.000     .0796837    .1352956
           exper |   .0415665   .0131752     3.15   0.002     .0156697    .0674633
         expersq |  -.0008112   .0003932    -2.06   0.040    -.0015841   -.0000382
           _cons |  -.5220406   .1986321    -2.63   0.009    -.9124667   -.1316144
    ------------------------------------------------------------------------------

::

    . heckman lwage educ exper expersq, select(inlf = educ exper expersq nwifeinc 
              age kidslt6 kidsge6) twostep
     
    Heckman selection model -- two-step estimates   Number of obs      =       753
    (regression model with sample selection)        Censored obs       =       325
                                                    Uncensored obs     =       428
     
                                                    Wald chi2(6)       =    180.10
                                                    Prob > chi2        =    0.0000
     
    ------------------------------------------------------------------------------
                 |      Coef.   Std. Err.      z    P>|z|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
    lwage        |
            educ |   .1090655    .015523     7.03   0.000     .0786411      .13949
           exper |   .0438873   .0162611     2.70   0.007     .0120163    .0757584
         expersq |  -.0008591   .0004389    -1.96   0.050    -.0017194    1.15e-06
           _cons |  -.5781032   .3050062    -1.90   0.058    -1.175904     .019698
    -------------+----------------------------------------------------------------

::

    inlf         |
            educ |   .1309047   .0252542     5.18   0.000     .0814074     .180402
           exper |   .1233476   .0187164     6.59   0.000     .0866641    .1600311
         expersq |  -.0018871      .0006    -3.15   0.002     -.003063   -.0007111
        nwifeinc |  -.0120237   .0048398    -2.48   0.013    -.0215096   -.0025378
             age |  -.0528527   .0084772    -6.23   0.000    -.0694678   -.0362376
         kidslt6 |  -.8683285   .1185223    -7.33   0.000    -1.100628    -.636029
         kidsge6 |    .036005   .0434768     0.83   0.408     -.049208    .1212179
           _cons |   .2700768    .508593     0.53   0.595    -.7267473    1.266901
    -------------+----------------------------------------------------------------
    mills        |
          lambda |   .0322619   .1336246     0.24   0.809    -.2296376    .2941613
    -------------+----------------------------------------------------------------
             rho |    0.04861
           sigma |  .66362875
          lambda |  .03226186   .1336246
    ------------------------------------------------------------------------------
     
    . * The IMR is very statistically insignificant, so there is no evidence of
    . * sample selection bias. The estimates in the wage equation are 
    . * very similar without the selection correction.

::

    . * The joint MLE estimates are very similar:
     
    . heckman lwage educ exper expersq, select(inlf =educ exper expersq nwifeinc age 
              kidslt6 kidsge6)
     
    Heckman selection model                         Number of obs      =       753
    (regression model with sample selection)        Censored obs       =       325
                                                    Uncensored obs     =       428
     
                                                    Wald chi2(3)       =     59.67
    Log likelihood = -832.8851                      Prob > chi2        =    0.0000
     
    ------------------------------------------------------------------------------
                 |      Coef.   Std. Err.      z    P>|z|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
    lwage        |
            educ |   .1083502   .0148607     7.29   0.000     .0792238    .1374767
           exper |   .0428369   .0148785     2.88   0.004     .0136755    .0719983
         expersq |  -.0008374   .0004175    -2.01   0.045    -.0016556   -.0000192
           _cons |  -.5526973   .2603784    -2.12   0.034     -1.06303   -.0423651
    -------------+----------------------------------------------------------------

::

    inlf         |
            educ |   .1313415   .0253823     5.17   0.000     .0815931    .1810899
           exper |   .1232818   .0187242     6.58   0.000     .0865831    .1599806
         expersq |  -.0018863   .0006004    -3.14   0.002     -.003063   -.0007095
        nwifeinc |  -.0121321   .0048767    -2.49   0.013    -.0216903    -.002574
             age |  -.0528287   .0084792    -6.23   0.000    -.0694476   -.0362098
         kidslt6 |  -.8673988   .1186509    -7.31   0.000     -1.09995   -.6348472
         kidsge6 |   .0358723   .0434753     0.83   0.409    -.0493377    .1210824
           _cons |   .2664491   .5089578     0.52   0.601    -.7310898    1.263988
    -------------+----------------------------------------------------------------
         /athrho |    .026614    .147182     0.18   0.857    -.2618573    .3150854
        /lnsigma |  -.4103809   .0342291   -11.99   0.000    -.4774687   -.3432931
    -------------+----------------------------------------------------------------
             rho |   .0266078   .1470778                     -.2560319    .3050564
           sigma |   .6633975   .0227075                      .6203517    .7094303
          lambda |   .0176515   .0976057                     -.1736521    .2089552
    ------------------------------------------------------------------------------
    LR test of indep. eqns. (rho = 0):   chi2(1) =     0.03   Prob > chi2 = 0.8577
    ------------------------------------------------------------------------------

