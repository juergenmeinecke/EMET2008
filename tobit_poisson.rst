The Tobit Model and the Poisson Regression Model
***************************************************

Introduction
===============

Consider special models for two kinds of nonnegative response variables, where the value zero is
important.

The first is sometimes incorrectly called a censored variable. The name corner solution is
preferred. It is a variable that takes on the value zero with some regularity but otherwise has a
(roughly) continuous distribution over positive values.

Examples of corner solution responses include annual hours worked, annual charitable contributions,
amount of life insurance, consumption of alcohol. In each case, :math:`y\geq 0` and
:math:`P(y=0)>0`: a nontrivial fraction of the population is at zero.

The Tobit model has been used for decades to model corner solution responses. It accounts for a
pile up at zero. It tends to provide better fit to corner solution responses with more satisfying
partial effects.

Later we will see what happens when data a truly censored (for example, wealth is top coded).

Count response variables take on nonnegative integer values, in many cases a small number. Examples
include number of crimes committed in a year, annual medical appointments, patents awarded to a
firm.

As with corner solution responses, linear functional forms are often not the best. We will cover an
exponential response function as an alternative.

Simple Strategies for Corner Solutions and Counts
====================================================

Let :math:`y\geq 0` be the response variable and assume we are able to collect a random sample on
:math:`y` along with explanatory variables :math:`x_{1},...,x_{k}`.

If we use a linear model we are (perhaps) thinking

.. math:: E(y|x_{1},...,x_{k})=\beta _{0}+\beta _{1}x_{1}+...+\beta _{k}x_{k}

But the linear model is at best an approximation. For one, linear models cannot ensure
nonnegativity.

As an approximation, there is nothing wrong with using a linear model estimated by OLS.

**Pros of Linear Model**

1. Easy to estimate and perform inference.

2. East to interpret: directly get partial effects on mean value, as always.

**Cons of Linear Model**

1. Possibility of negative fitted values. (Similar to linear model for binary response.) Not
   especially important if goal is mainly to obtain partial effects.

2. There is almost certainly heteroskedasticity. Easy to use robust inference with OLS.

3. Constant partial effects.

Provided :math:`y` is the variable we want to explain – that is, it is not censored in some way –
makes sense to start with a linear model estimated by OLS. Use robust inference. Can check fitted
values. Usual :math:`R`-squared measures are valid.


Tobit Model
==============

For corner solution responses, the most popular model is the **Tobit model** (named for James
Tobin).

One way to define the model is to use an underlying *latent variable*, which we call :math:`y^{\ast
}`. This variable follows the classical linear model:

.. math::

   \begin{aligned} y^{\ast } &=&\beta _{0}+\beta _{1}x_{1}+...+\beta _{k}x_{k}+u \\ u|\mathbf{x}
   &\sim &Normal(0,\sigma ^{2})\end{aligned}

The error term :math:`u` is independent of the explanatory variables (so the :math:`x_{j}` are
exogenous) and normally distributed.

The observed outcome, :math:`y`, depends on :math:`y^{\ast }`:

.. math::

   \begin{aligned} y &=&y^{\ast }\text{ if }y^{\ast }\geq 0 \\ y &=&0\text{ if }y^{\ast
   }<0\end{aligned}

A more concise expression:

.. math:: y=\max (0,y^{\ast })

Because :math:`y^{\ast }` has a normal distribution (conditional on :math:`\mathbf{x}`), it takes
on negative as well as positive values. So :math:`P(y=0)>0` but :math:`y` has a continuous
distribution over strictly positive values.

Drawback to the latent variable formulation: it makes us think the :math:`\beta _{j}` tell us what
we want to know.  But the :math:`\beta _{j}` are the partial effects on :math:`y^{\ast }`, not on
:math:`y`.

We an avoid :math:`y^{\ast }` entirely and write

.. math::

   \begin{aligned} y &=&\max (0,\beta _{0}+\beta _{1}x_{1}+...+\beta _{k}x_{k}+u) \\ u|\mathbf{x}
   &\sim &Normal(0,\sigma ^{2})\end{aligned}

But the latent variable formulation is useful for calculations.

**Expected Value and Partial Effects**

The Tobit model fully characterizes the distribution of :math:`y` given :math:`\mathbf{x}`. In
principle, we can find any conditional probability.

We can relate the Tobit and Probit models. If we define a binary variable

.. math::

  \begin{aligned} w &=&1\text{ if }y>0 \\ w &=&0\text{ if }y=0\end{aligned}

then it can be shown that

.. math::

  P(w=1|\mathbf{x})=\Phi (\mathbf{x\beta }/\sigma )=\Phi \left( \frac{\beta _{0}+\beta
  _{1}x_{1}+...+\beta _{k}x_{k}}{\sigma }\right)

**EXAMPLE:** Suppose that :math:`y` is annual hours worked (for a wage).  Then :math:`w` is a binary
variable indicating whether someone worked at all (participated in the labor force). Then we can
estimate the scaled parameters

.. math:: \frac{\beta _{0}}{\sigma },\frac{\beta _{1}}{\sigma },...,\frac{\beta _{k}}{\sigma }

from a probit of :math:`w` on the explanatory variables. This is often useful as a check because
taking the ratio of any two scaled coefficients eliminates :math:`\sigma`.

Usually we want the partial effects on the expected value of :math:`y` (not :math:`y^{\ast }`). For
example, if number if children increases, what happens to hours worked, on average?  As income
increases, what happens to charitable contributions on average.

The formula for :math:`E(y|\mathbf{x})` is somewhat complicated. See Wooldridge (5e, Chapter 17)
for derivation.

Again use the shorthand :math:`\mathbf{x\beta }=\beta _{0}+\beta _{1}x_{1}+...+\beta _{k}x_{k}`. It
can be shown that

.. math::

   \begin{aligned} E(y|\mathbf{x}) &=&P(y>0|\mathbf{x})E(y|y>0,\mathbf{x}) \\ &=&\Phi
   (\mathbf{x\beta }/\sigma )\left( \mathbf{x\beta }+\sigma \frac{\phi (\mathbf{x\beta }/\sigma
   )}{\Phi (\mathbf{x\beta }/\sigma )}\right) \\ &=&\Phi (\mathbf{x\beta }/\sigma )\mathbf{x\beta
   }+\sigma \phi (\mathbf{x\beta }/\sigma ),\end{aligned}

where :math:`\Phi (\cdot )` is the standard normal cdf and :math:`\phi (\cdot )` is the standard
normal pdf.

This is called the unconditional expectation because it does not condition on :math:`y>0` (but we
are conditioning on :math:`\mathbf{x}`).

The conditionalexpectation is

.. math:: E(y|y>0,\mathbf{x})=\mathbf{x\beta }+\sigma \frac{\phi (\mathbf{x\beta }/\sigma )}{\Phi
  (\mathbf{x\beta }/\sigma )},

and so we look at the effect on the mean of :math:`y` only for the subpopulation with :math:`y>0`.

Typically :math:`E(y|\mathbf{x})` is of more interest.

Amazingly, the partial effects based on calculus are very simple:

.. math:: \frac{\partial E(y|\mathbf{x})}{\partial x_{j}}=\Phi (\mathbf{x\beta }/\sigma )\beta
  _{j}=P(y>0|\mathbf{x})\beta _{j}.

So, to get partial effects on the unconditional mean, the :math:`\beta _{j}` are scaled by a
function between :math:`0` and :math:`1` (which depends on :math:`\mathbf{x}`).

As :math:`P(y>0|\mathbf{x})\rightarrow 1`, :math:`\beta _{j}` becomes close to the actual partial
effect. If :math:`P(y=0|\mathbf{x})` is large, :math:`P(y>0|\mathbf{x})` is small, so the scale
factor is small.

The scale factor depends on :math:`x_{1},x_{2},...,x_{k}`. We can get a common scale factor by
focusing on the average partial (marginal) effect.

The average of :math:`P(y>0|\mathbf{x})` across the distribution of :math:`\mathbf{x}` is simply
the unconditional probability :math:`P(y>0)`. Therefore, the APE for a continuous variable
:math:`x_{j}` is just

.. math:: APE_{j}=P(y>0)\beta _{j}.

We can estimate :math:`P(y>0)` simply as the fraction of nonzero outcomes in the sample and
multiple the coefficients (on continuous explanatory variables) by this fraction. Or, we can
average the function :math:`\Phi (\mathbf{x}_{i}\mathbf{\hat{\beta}}/\hat{\sigma})` across
:math:`i`. We can see how much this scale factor changes across :math:`i`.

Partial effects for discrete changes are harder to obtain. Need to evaluate :math:`E(y|\mathbf{x})`
at different values of the :math:`x_{j}` and take differences.

**Estimation of Parameters**

As with logit and probit, the estimation method is maximum likelihood. Easily done, even with large
data sets and many explanatory variables, using modern packages. (See Wooldridge, 5e, Chapter 17,
for likelihood function.)

In Stata: ::

    tobit y x1 x2 ... xK, ll(0)

where the option ``ll(0)`` means that the lower limit is at zero.

There are upper limit options, too, leading to two-limit Tobit models. These are for variables with
two corners, say zero and one or zero and 100. They are harder to use.

It is possible to use a ``robust`` option in Stata. This changes calculation of the standard errors
(not the coefficients) and is an admission that the Tobit model is incorrect.  (This may be a
sensible perspective.)

**Reporting the Results**

We obtain estimates :math:`\hat{\beta}_{j}` along with standard errors. We also obtain
:math:`\hat{\sigma}` and its standard error. The latter parameter shows up in partial effects, so
it is not ancillary.

We can report estimated partial effects on the average as

.. math:: \Phi (\mathbf{\bar{x}\hat{\beta}}/\hat{\sigma})\hat{\beta}_{j}

for continuous variables. (Unfortunately, the ``mfx`` command in Stata simply reports the
:math:`\hat{\beta}_{j}.`) Or, plug in other interesting values.

The estimated APEs are

.. math:: \left[ N^{-1}\sum_{i=1}^{N}\Phi (\mathbf{x}_{i}\mathbf{\hat{\beta}}/\hat{\sigma})\right]
  \hat{\beta}_{j}.

The scale factor is the average of :math:`\hat{P}(y>0|\mathbf{x})` across the sample.
(Unfortunately, the ``margins`` and ``marfeff`` commands in Stata report the :math:`\hat{\beta}_{j}`
and not the APEs.)

Can compare the Tobit APEs on :math:`E(y|\mathbf{x})` to OLS estimates.

For discrete explanatory variables or for large changes in continuous ones, we can compute the
difference in :math:`E(y|\mathbf{x})` at different values of :math:`\mathbf{x}`.  Suppose
:math:`x_{k}` is a binary variable (such as a policy indicator), and define, for each observation
:math:`i`, the two indices
:math:`\hat{w}_{i1}=\mathbf{x}_{i(k)}\mathbf{\hat{\beta}}_{(k)}+\hat{\beta}_{k}` and
:math:`\hat{w}_{i0}=\mathbf{x}_{i(k)}\mathbf{\hat{\beta}}_{(k)}`, where :math:`\mathbf{x}_{i(k)}`
is the :math:`1\times (k-1)` row vector with :math:`x_{ik}` dropped.Then, the average difference

.. math:: N^{-1}\sum_{i=1}^{N}\{[\Phi (\hat{w}_{i1}/\hat{\sigma})\hat{w}_{i1}+\hat{\sigma}\phi
  (\hat{w}_{i1}/\hat{\sigma})]-[\Phi (\hat{w}_{i0}/\hat{\sigma})\hat{w}_{i0}+\hat{\sigma}\phi
  (\hat{w}_{i0}/\hat{\sigma})]\}

is the APE of :math:`x_{k}`.

Different ways to define goodness-of-fit statistics. If we focus on :math:`E(y|\mathbf{x})`, a
simple one is the squared correlation between :math:`y_{i}` and
:math:`\hat{E}(y_{i}|\mathbf{x}_{i})=\Phi
(\mathbf{x}_{i}\mathbf{\hat{\beta}}/\hat{\sigma})\mathbf{x}_{i}\mathbf{\hat{\beta}}+\hat{\sigma}\phi
(\mathbf{x}_{i}\mathbf{\hat{\beta}}/\hat{\sigma})`.  This can be compared with the OLS
:math:`R`-squared (which is equal to the squared correlation between :math:`y_{i}` and the OLS
fitted values).

.. admonition:: Example: Married Women’s Annual Hours Worked

    Data set is MROZ.DTA. :math:`y=hours` and there are several explanatory variables.

    Cannot include the hourly wage using the usual Tobit model, for two reasons. (1) Observed wage is
    endogenous – this is like a supply and demand system. (2) The wage at which someone can work is
    observed only if the person is working. The wage offer is not observed if :math:`hours=0`.


::

    . use mroz
     
    . des inlf hours kidslt6 kidsge6 age educ wage exper nwifeinc
     
                  storage  display     value
    variable name   type   format      label      variable label
    --------------------------------------------------------------------------------------------------
    inlf            byte   %9.0g                  =1 if in lab frce, 1975
    hours           int    %9.0g                  hours worked, 1975
    kidslt6         byte   %9.0g                  # kids < 6 years
    kidsge6         byte   %9.0g                  # kids 6-18
    age             byte   %9.0g                  woman's age in yrs
    educ            byte   %9.0g                  years of schooling
    wage            float  %9.0g                  est. wage from earn, hrs
    exper           byte   %9.0g                  actual labor mkt exper
    nwifeinc        float  %9.0g                  (faminc - wage*hours)/1000

::

    . sum inlf hours kidslt6 kidsge6 age educ exper nwifeinc wage
     
        Variable |       Obs        Mean    Std. Dev.       Min        Max
    -------------+--------------------------------------------------------
            inlf |       753    .5683931    .4956295          0          1
           hours |       753    740.5764    871.3142          0       4950
         kidslt6 |       753    .2377158     .523959          0          3
         kidsge6 |       753    1.353254    1.319874          0          8
             age |       753    42.53785    8.072574         30         60
    -------------+--------------------------------------------------------
            educ |       753    12.28685    2.280246          5         17
           exper |       753    10.63081     8.06913          0         45
        nwifeinc |       753    20.12896     11.6348  -.0290575         96
            wage |       428    4.177682    3.310282      .1282         25

::

    . count if hours == 0
      325
     
    . tab inlf
     
       =1 if in |
      lab frce, |
           1975 |      Freq.     Percent        Cum.
    ------------+-----------------------------------
              0 |        325       43.16       43.16
              1 |        428       56.84      100.00
    ------------+-----------------------------------
          Total |        753      100.00

::

    . reg hours nwifeinc educ exper expersq age kidslt6 kidsge6, robust
     
    Linear regression                                      Number of obs =     753
                                                           F(  7,   745) =   45.81
                                                           Prob > F      =  0.0000
                                                           R-squared     =  0.2656
                                                           Root MSE      =  750.18
     
    ------------------------------------------------------------------------------
                 |               Robust
           hours |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
        nwifeinc |  -3.446636   2.240662    -1.54   0.124    -7.845398    .9521268
            educ |   28.76112   13.03905     2.21   0.028     3.163468    54.35878
           exper |   65.67251   10.79419     6.08   0.000     44.48186    86.86316
         expersq |  -.7004939   .3720129    -1.88   0.060    -1.430812    .0298245
             age |  -30.51163   4.244791    -7.19   0.000    -38.84481   -22.17846
         kidslt6 |  -442.0899   57.46384    -7.69   0.000    -554.9002   -329.2796
         kidsge6 |  -32.77923   22.80238    -1.44   0.151     -77.5438    11.98535
           _cons |   1330.482   274.8776     4.84   0.000     790.8556    1870.109
    ------------------------------------------------------------------------------
     
    . predict hoursh_l
    (option xb assumed; fitted values)
     
    . sum hoursh_l
     
        Variable |       Obs        Mean    Std. Dev.       Min        Max
    -------------+--------------------------------------------------------
        hoursh_l |       753    740.5764    449.0646  -719.7679   1614.693
     
    . count if hoursh_l < 0
       39

::

    . tobit hours nwifeinc educ exper expersq age kidslt6 kidsge6, ll(0)
     
    Tobit regression                                  Number of obs   =        753
                                                      LR chi2(7)      =     271.59
                                                      Prob > chi2     =     0.0000
    Log likelihood = -3819.0946                       Pseudo R2       =     0.0343
     
    ------------------------------------------------------------------------------
           hours |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
        nwifeinc |  -8.814243   4.459096    -1.98   0.048    -17.56811   -.0603724
            educ |   80.64561   21.58322     3.74   0.000     38.27453    123.0167
           exper |   131.5643   17.27938     7.61   0.000     97.64231    165.4863
         expersq |  -1.864158   .5376615    -3.47   0.001    -2.919667   -.8086479
             age |  -54.40501   7.418496    -7.33   0.000    -68.96862    -39.8414
         kidslt6 |  -894.0217   111.8779    -7.99   0.000    -1113.655   -674.3887
         kidsge6 |    -16.218   38.64136    -0.42   0.675    -92.07675    59.64075
           _cons |   965.3053   446.4358     2.16   0.031     88.88528    1841.725
    -------------+----------------------------------------------------------------
          /sigma |   1122.022   41.57903                      1040.396    1203.647
    ------------------------------------------------------------------------------
      Obs. summary:        325  left-censored observations at hours<=0
                           428     uncensored observations
                             0 right-censored observations

::

    . * Get the estimates of xb for each i:
    . predict xbh, xb
     
    . * Generate predicted values for Tobit:
     
    . gen hoursh_t = normal(xbh/_b[/sigma])*xbh + _b[/sigma]*normalden(xbh/_b[/sigma])
     
    . sum hours hoursh_t
     
        Variable |       Obs        Mean    Std. Dev.       Min        Max
    -------------+--------------------------------------------------------
           hours |       753    740.5764    871.3142          0       4950
        hoursh_t |       753    721.4201    473.6053   3.496456   1993.885
     
    . corr hours hoursh_t
    (obs=753)
     
                 |    hours   hoursh
    -------------+------------------
           hours |   1.0000
        hoursh_t |   0.5237   1.0000
     
    . di .5237^2
    .27426169
     
    . * This squared correlation can be compared with the linear model R-squared,
    . * .266. So Tobit fits a bit better.
     

::

    . * Obtain some partial effects.
     
    . * Generate scale factor for each i:
     
    . gen scale = normal(xbh/_b[/sigma])
     
    . sum scale
     
        Variable |       Obs        Mean    Std. Dev.       Min        Max
    -------------+--------------------------------------------------------
           scale |       753    .5886634    .2426614   .0092704    .960908
     
    . * Partial effect for educ:
    . gen pe_educ = scale*_b[educ]
    . gen pe_nwifeinc = scale*_b[nwifeinc]
     
    . sum pe_educ pe_nwifeinc
     
        Variable |       Obs        Mean    Std. Dev.       Min        Max
    -------------+--------------------------------------------------------
         pe_educ |       753    47.47311    19.56957   .7476184     77.493
     pe_nwifeinc |       753   -5.188622    2.138876  -8.469676  -.0817117

::

    . histogram pe_educ, bin(30) fraction
     

::

    . * Evaluate xb at 0, 1, and 2 young kids, for each i
     
    . gen xbh0 = xbh - _b[kidslt6]*kidslt6
    . gen xbh1 = xbh0 + _b[kidslt6]
    . gen xbh2 = xbh0 + _b[kidslt6]*2
     
    . * Generate estimated mean for each woman at each value of young kids:
    . gen m0 = normal(xbh0/_b[/sigma])*xbh0 + _b[/sigma]*normalden(xbh1/_b[/sigma])
    . gen m1 = normal(xbh1/_b[/sigma])*xbh1 + _b[/sigma]*normalden(xbh1/_b[/sigma])
    . gen m2 = normal(xbh2/_b[/sigma])*xbh2 + _b[/sigma]*normalden(xbh2/_b[/sigma])
    . gen pe0_1 = m1 - m0
    . gen pe1_2 = m2 - m1
     
    . sum pe0_1 pe1_2
     
        Variable |       Obs        Mean    Std. Dev.       Min        Max
    -------------+--------------------------------------------------------
           pe0_1 |       753    -487.213     338.899  -999.4987   105.9935
           pe1_2 |       753   -246.1717    159.1696  -649.9704  -2.305203
     
    . * Linear model estimate: -442.09 for each additional kid.

::

    . tab kidslt6
     
     # kids < 6 |
          years |      Freq.     Percent        Cum.
    ------------+-----------------------------------
              0 |        606       80.48       80.48
              1 |        118       15.67       96.15
              2 |         26        3.45       99.60
              3 |          3        0.40      100.00
    ------------+-----------------------------------
          Total |        753      100.00
     
    . * Suppose we weight pe0_1 and pe1_2 by the shares of women in the sample.
    . * 147 women have at least one child. About 80% have only one child.
     
    . di 29 + 118
    147
     
    . di 118/147
    .80272109
     
    . di .8*(-487.2) + .2*(-246.2)
    -439
     
    . * Linear regression coefficient on kidslt6 is about -442, very close 
    . * to the weighted average of the Tobit effects.
     
    . * The linear regression coefficient for educ (28.8) is well below the APE
    . * from the Tobit model (47.5).



Poisson Regression Model 
===========================

We consider the case where the count variable does not have a natural upper bound. So possible
values of :math:`y` are :math:`\{0,1,2,...\}`. In other cases, there is an upper bound, and it can
even change by individual: :math:`(n_{i},y_{i})` is drawn with :math:`n_{i}` a positive integer and
then :math:`y_{i}\in \{0,1,...,n_{i}\}`. (For example, :math:`n_{i}` is number of children in a
family and :math:`y_{i}` is the number of children with a college degree.)

Count data can be analyzed from one of two perspectives:

1.  We are mainly interested in :math:`E(y|\mathbf{x})` – and so we want consistent estimators of
    the mean parameters without additional assumptions – but we would like our estimators to at
    least recognize the count nature of :math:`y` and be efficient in some situations.

2.  We are interested in other features of the distribution of :math:`y` given :math:`\mathbf{x}`.

Our focus here is on :math:`E(y|\mathbf{x})` (just as with linear regression).

Remember, linear regression is a perfectly fine place to start. As with a corner solution response,
a linear model might give negative predicted values and are that it will not ensure
:math:`\hat{E}(y|\mathbf{x})\geq 0` for all relevant vectors :math:`\mathbf{x}` and it may not give
sensible partial effects for extreme values of :math:`\mathbf{x}`. Heteroskedasticity-robust
inference should be used.

We cannot use :math:`\log (y)` in interesting applications because :math:`y_{i}` is typically zero
for a nontrivial fraction of the observations. Using :math:`\log (1+y)` is not recommended. It does
not help with the pile-up-at-zero problem because :math:`\log (1)=0` and we cannot recover partial
effects on :math:`E(y|\mathbf{x})`.

A better approach is to directly model :math:`E(y|\mathbf{x})`. An exponential function is very
convenient and flexible:

.. math::

    E(y|x_{1},x_{2},...,x_{k})=\exp (\beta _{0}+\beta _{1}x_{1}+...+\beta _{k}x_{k})\equiv \exp
    (\mathbf{x\beta })

Recall the function :math:`\exp (\cdot )` is always positive, so predicted values of :math:`y` will
be positive.

:math:`\exp (\cdot )` is strictly increasing and its slope increases.

We loosely refer to :math:`\beta _{0}` as the intercept, which is automatically included in
estimation – just as with a linear model.

It is easy to intepret the slope parameters because we can write

.. math:: \log [E(y|\mathbf{x})]=\beta _{0}+\beta _{1}x_{1}+...+\beta _{k}x_{k}

This looks a lot like a linear model for :math:`\log (y)` but we do not have to worry about
:math:`y=0`.

Parameters are interpreted *as if* we can estimate a linear model for :math:`\log (y)`.

1.  :math:`100\cdot \beta _{1}` is the percentage change in :math:`E(y|\mathbf{x})` when
    :math:`\Delta x_{1}=1`, other factors fixed. (So we say that :math:`100\cdot \beta _{1}` is the
    semi-elasticity of :math:`y` with respect to :math:`x_{1}`.)

2.  If :math:`x_{2}=\log (z_{2})`, :math:`\beta _{2}` is the percentage change in
    :math:`E(y|\mathbf{x})` given a 1% increase in :math:`z_{2}`.  (So we say that :math:`\beta _{2}`
    is the elasticity of :math:`y` with respect to :math:`z_{2}`.)

3.  If :math:`x_{3}` is a dummy variable then :math:`100\cdot \beta _{3}` is the percentage change in
    :math:`E(y|\mathbf{x})` when :math:`x_{3}` changes from zero to one. (This approximation might be
    poor if :math:`\beta _{3}` is large in magnitude.)

Other functional forms – such as quadratics and interactions – are allowed, just as with linear
models.

**Estimation**

Nominally, the estimation is by maximum likelihood assuming that :math:`y` given :math:`\mathbf{x}`
follows a Poisson distribution with mean :math:`\exp (\mathbf{x\beta })`.

For many applications the Poisson distributional assumption is too restrictive. One important
restriction is that the variance is equal to the mean:

.. math:: Var(y|\mathbf{x})=E(y|\mathbf{x})

In economic applications, *overdispersion* is common, that is,
:math:`Var(y|\mathbf{x})>E(y|\mathbf{x})`. But *underdispersion* can also occur.

If the Poisson distribution is so restrictive, why do we use it?

1.  It is a standard count distribution, much like the normal distribution is the starting point for
    continuous responses.

2.  Much more importantly, it turns out that the resulting estimator is fully robust to
    distributional misspecification. We only need :math:`E(y|\mathbf{x})=\exp (\mathbf{x\beta })` for
    the estimators to be consistent for :math:`\mathbf{\beta }` and approximately normally
    distributed. In particular, we need make no assumption about :math:`Var(y|\mathbf{x})`.

In other words, the so called *Poisson regression estimator* is a *quasi*-maximum likelihood
estimator with very satisfying robustness properties. It has the same properties as OLS estimation
of a linear model (where neither normality nor homoskedasticity are needed for consistent
estimation).

As with OLS in the presence of heteroskedasticity, we need to use robust standard errors for
Poisson regression when :math:`Var(y|\mathbf{x})\neq E(y|\mathbf{x})`.

The first-order conditions solved by the Poisson regression estimator look similar to those for
OLS:

.. math::

   \begin{aligned} \sum_{i=1}^{n}[y_{i}-\exp (\mathbf{x}_{i}\mathbf{\hat{\beta}})] &=&0 \\
   \sum_{i=1}^{n}x_{i1}[y_{i}-\exp (\mathbf{x}_{i}\mathbf{\hat{\beta}})] &=&0 \\ &&\vdots \\
   \sum_{i=1}^{n}x_{ik}[y_{i}-\exp (\mathbf{x}_{i}\mathbf{\hat{\beta}})] &=&0\end{aligned}

Define the Poisson residuals fitted values and residuals as

.. math::

   \begin{aligned} \hat{y}_{i} &=&\exp (\mathbf{x}_{i}\mathbf{\hat{\beta}})=\exp
   (\hat{\beta}_{0}+\hat{\beta}_{1}x_{i1}+...+\hat{\beta}_{k}x_{ik}) \\ \hat{u}_{i}
   &=&y_{i}-\hat{y}_{i}=\exp (\mathbf{x}_{i}\mathbf{\hat{\beta}})\end{aligned}

From the first FOC it follows that the residuals always add up to zero in the sample (just like
with OLS).  Consequently, the average of :math:`y_{i}` in the sample equals the average of
:math:`\hat{y}_{i}`.

The FOCs also show each explanatory variable is uncorrelated with the residuals in the sample (just
like with OLS).

Stata has a couple of different ways to estimate Poisson regression models. With robust standard
errors: :: 

    poisson y x1 ... xk, robust

The command without the ``robust`` option, ::

    poisson y x1 ... xk

only produces valid inference when the variance equals the mean.

A generalized linear models (GLM) command can be used, too, with a ``robust`` option. This is
needed for earlier versions of Stata (where ``poisson`` did not allow a ``robust`` option): ::

    glm y x1 ... xk, fam(poisson) robust

The robust standard errors are valid for any variance. Sometimes standard errors are be computed
under the assumption that the variance is proportional to the mean:

.. math:: Var(y|\mathbf{x})=\sigma ^{2}E(y|\mathbf{x}),\text{ some }\sigma ^{2}>0.

The Stata command is ::

    glm y x1 ... xk, fam(poisson) scale(x2)

The ``scale(x2)`` option says to use this form of the variance, to estimate :math:`\sigma ^{2}`,
and report the estimate. Typically use the Pearson estimate, which can be used to see whether there
is underdispersion or overdispersion.

**Average Partial Effects and Goodness-of-Fit**

Remember, the coefficients can be interpreted as percentage changes (semi-elasticities or
elasticities).

We can also directly estimate the change in the explanatory variables on the expected level of
:math:`y`. Remember that the derivative of :math:`\exp (\cdot )` is just :math:`\exp (\cdot )`. So,
by the chain rule, for continuous :math:`x_{j}`,

.. math:: \frac{\partial E(y|\mathbf{x})}{\partial x_{j}}=\beta _{j}\exp (\mathbf{x\beta }).

The APE is consistently estimated as

.. math:: \hat{\beta}_{j}\left[ N^{-1}\sum_{i=1}^{N}\exp (\mathbf{x}_{i}\mathbf{\hat{\beta}})\right]
   =\hat{\beta}_{j}\left( N^{-1}\sum_{i=1}^{N}\hat{y}_{i}\right) =\hat{\beta}_{j}\bar{y}

because as shown earlier (from the first-order condition), :math:`\overline{\hat{y}}=\bar{y}`.

Consequently for (roughly) continuous :math:`x_{j}`, the Poisson coefficients multiplied by the
sample average :math:`\bar{y}` are comparable to the OLS estimates from :math:`y_{i}` on
:math:`x_{i1},x_{i2},...,x_{iK}`.

Goodness-of-Fit: To measure how well the mean predicts :math:`y_{i}`, can use the squared
correlation between :math:`y_{i}` and :math:`\hat{y}_{i}` as an :math:`R`-squared. This can be
compared directly to the OLS.

**Summary of Statistical Properties of Poisson Regression**

The Poisson QMLE is consistent provided the mean is correctly specified. No other features of the
Poisson distribution need to be correct. (Aside: This estimator can even be applied to corner
solution responses if we prefer :math:`E(y|\mathbf{x})=\exp (\mathbf{x\beta })` to the more
complicated Tobit mean.)

The estimators are asymptotically normal and robust standard errors, :math:`t` statistics, and
confidence intervals are easily obtained.

It turns out the Poisson QMLE is asymptotically efficient among all estimators using just
:math:`E(y|\mathbf{x})=\exp (\mathbf{x\beta })` for consistency if

.. math:: Var(y|\mathbf{x})=\sigma ^{2}E(y|\mathbf{x})\text{, some }\sigma ^{2}>0.

There are ways to test whether the variance is proportional to the mean but it is rarely done.

.. admonition:: Example: Use the number of arrests in CRIME1.DTA 
   
    Data on men in California born in 1960 or 1961. All had been arrested at least once previously.
    Data from 1986.

    :math:`narr86` is the number of times the man was arrested during 1986. Are there deterrent effects
    to prior convictions or sentence lengths? What is the effect of incarceration?  What about labor
    market opportunities?


::

    . des
     
    Contains data from crime1.dta
      obs:         2,725                          
     vars:            19                          6 Nov 1996 10:54
     size:       155,325 (98.5% of memory free)
    -------------------------------------------------------------------------------
                  storage  display     value
    variable name   type   format      label      variable label
    -------------------------------------------------------------------------------
    narr86          byte   %9.0g                  # times arrested, 1986
    nfarr86         byte   %9.0g                  # felony arrests, 1986
    nparr86         byte   %9.0g                  # property crme arr., 1986
    pcnv            float  %9.0g                  proportion of prior convictions
    avgsen          float  %9.0g                  avg sentence length, mos.
    tottime         float  %9.0g                  time in prison since 18 (mos.)
    ptime86         byte   %9.0g                  mos. in prison during 1986
    qemp86          float  %9.0g                  # quarters employed, 1986
    inc86           float  %9.0g                  legal income, 1986, $100s
    durat           float  %9.0g                  recent unemp duration
    black           byte   %9.0g                  =1 if black
    hispan          byte   %9.0g                  =1 if Hispanic
    born60          byte   %9.0g                  =1 if born in 1960
    pcnvsq          float  %9.0g                  pcnv^2
    pt86sq          int    %9.0g                  ptime86^2
    inc86sq         float  %9.0g                  inc86^2

::

    . tab narr86
     
        # times |
      arrested, |
           1986 |      Freq.     Percent        Cum.
    ------------+-----------------------------------
              0 |      1,970       72.29       72.29
              1 |        559       20.51       92.81
              2 |        121        4.44       97.25
              3 |         42        1.54       98.79
              4 |         12        0.44       99.23
              5 |         13        0.48       99.71
              6 |          4        0.15       99.85
              7 |          1        0.04       99.89
              9 |          1        0.04       99.93
             10 |          1        0.04       99.96
             12 |          1        0.04      100.00
    ------------+-----------------------------------
          Total |      2,725      100.00
     
    . sum pcnv avgsen ptime86 inc86
     
        Variable |       Obs        Mean    Std. Dev.       Min        Max
    -------------+--------------------------------------------------------
            pcnv |      2725    .3577872     .395192          0          1
          avgsen |      2725    .6322936    3.508031          0       59.2
         ptime86 |      2725     .387156    1.950051          0         12
           inc86 |      2725    54.96705    66.62721          0        541

::

    . reg narr86 pcnv avgsen tottime ptime86 inc86 qemp86 black hispan born60, 
          robust
     
    Linear regression                                      Number of obs =    2725
                                                           F(  9,  2715) =   25.93
                                                           Prob > F      =  0.0000
                                                           R-squared     =  0.0725
                                                           Root MSE      =  .82873
     
    ------------------------------------------------------------------------------
                 |               Robust
          narr86 |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
            pcnv |   -.131886   .0335876    -3.93   0.000    -.1977458   -.0660262
          avgsen |  -.0113316   .0141409    -0.80   0.423    -.0390595    .0163963
         tottime |   .0120693   .0131776     0.92   0.360    -.0137699    .0379084
         ptime86 |  -.0408735   .0067985    -6.01   0.000    -.0542043   -.0275426
           inc86 |  -.0014617   .0002289    -6.38   0.000    -.0019106   -.0010128
          qemp86 |  -.0513099    .014205    -3.61   0.000    -.0791636   -.0234562
           black |   .3270097   .0584381     5.60   0.000     .2124221    .4415973
          hispan |   .1938094   .0401625     4.83   0.000     .1150572    .2725616
          born60 |   -.022465    .032094    -0.70   0.484    -.0853961    .0404661
           _cons |    .576566   .0426021    13.53   0.000     .4930302    .6601019
    ------------------------------------------------------------------------------
     
    . * As the proportion of prior convictions goes from 0 to 1, expect arrests
    . * to fall by .132. So out of 100 men, about 13 fewer arrests.
    . * An Hispanic man is expected to have about .194 more arrests, so 100
    . * Hispanic men will have about 19 more arrests than 100 white men
    . * (other things in the equation equal).

::

    . * Poisson regression with nonrobust standard errors:
     
    . glm narr86 pcnv avgsen tottime ptime86 inc86 qemp86 black hispan born60, 
          fam(poisson)
     
    Generalized linear models                          No. of obs      =      2725
    Optimization     : ML                              Residual df     =      2715
                                                       Scale parameter =         1
    Deviance         =  2822.184873                    (1/df) Deviance =  1.039479
    Pearson          =  4118.079859                    (1/df) Pearson  =  1.516788
     
    Variance function: V(u) = u                        [Poisson]
    Link function    : g(u) = ln(u)                    [Log]
     
                                                       AIC             =  1.657806
    Log likelihood   = -2248.761092                    BIC             = -18654.07
     
    ------------------------------------------------------------------------------
                 |                 OIM
          narr86 |      Coef.   Std. Err.      z    P>|z|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
            pcnv |  -.4015713   .0849712    -4.73   0.000    -.5681117   -.2350308
          avgsen |  -.0237723    .019946    -1.19   0.233    -.0628658    .0153212
         tottime |   .0244904   .0147504     1.66   0.097    -.0044199    .0534006
         ptime86 |  -.0985584   .0206946    -4.76   0.000    -.1391192   -.0579977
           inc86 |  -.0080807    .001041    -7.76   0.000     -.010121   -.0060404
          qemp86 |  -.0380187   .0290242    -1.31   0.190    -.0949051    .0188677
           black |   .6608376   .0738342     8.95   0.000     .5161252      .80555
          hispan |   .4998133   .0739267     6.76   0.000     .3549196     .644707
          born60 |  -.0510286   .0640518    -0.80   0.426    -.1765678    .0745106
           _cons |  -.5995888   .0672501    -8.92   0.000    -.7313966    -.467781
    ------------------------------------------------------------------------------
     
     
     
    . * With robust standard errors. Somewhat larger.
     
    . glm narr86 pcnv avgsen tottime ptime86 inc86 qemp86 black hispan born60, 
          fam(poisson) robust
     
    Generalized linear models                          No. of obs      =      2725
    Optimization     : ML                              Residual df     =      2715
                                                       Scale parameter =         1
    Deviance         =  2822.184873                    (1/df) Deviance =  1.039479
    Pearson          =  4118.079859                    (1/df) Pearson  =  1.516788
     
                                                       AIC             =  1.657806
    Log pseudolikelihood = -2248.761092                BIC             = -18654.07
     
    ------------------------------------------------------------------------------
                 |               Robust
          narr86 |      Coef.   Std. Err.      z    P>|z|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
            pcnv |  -.4015713   .1011619    -3.97   0.000    -.5998449   -.2032976
          avgsen |  -.0237723   .0236078    -1.01   0.314    -.0700427    .0224981
         tottime |   .0244904   .0205023     1.19   0.232    -.0156934    .0646741
         ptime86 |  -.0985584   .0223035    -4.42   0.000    -.1422724   -.0548445
           inc86 |  -.0080807   .0012276    -6.58   0.000    -.0104867   -.0056747
          qemp86 |  -.0380187   .0341509    -1.11   0.266    -.1049532    .0289158
           black |   .6608376   .0994572     6.64   0.000     .4659051      .85577
          hispan |   .4998133   .0923874     5.41   0.000     .3187374    .6808892
          born60 |  -.0510286   .0811403    -0.63   0.529    -.2100606    .1080034
           _cons |  -.5995888   .0893463    -6.71   0.000    -.7747044   -.4244732
    ------------------------------------------------------------------------------
     
    . * If pcnv increases by .1, expected arrests falls by about 4%: -.40(.1) =
    . * -.04.
    . * If ptime86 increases by 1 (one month), expected arrests falls by about
    . * 9.9%
    . * blacks are about 66% more likely to be arrested than whites.
     
    . glm narr86 pcnv avgsen tottime ptime86 inc86 qemp86 black hispan born60, 
          fam(poisson) sca(x2)
     
    Generalized linear models                          No. of obs      =      2725
    Optimization     : ML                              Residual df     =      2715
                                                       Scale parameter =         1
    Deviance         =  2822.184873                    (1/df) Deviance =  1.039479
    Pearson          =  4118.079859                    (1/df) Pearson  =  1.516788
     
                                                       AIC             =  1.657806
    Log likelihood   = -2248.761092                    BIC             = -18654.07
     
    ------------------------------------------------------------------------------
                 |                 OIM
          narr86 |      Coef.   Std. Err.      z    P>|z|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
            pcnv |  -.4015713   .1046488    -3.84   0.000    -.6066791   -.1964634
          avgsen |  -.0237723   .0245651    -0.97   0.333    -.0719191    .0243745
         tottime |   .0244904   .0181663     1.35   0.178    -.0111149    .0600957
         ptime86 |  -.0985584   .0254871    -3.87   0.000    -.1485122   -.0486047
           inc86 |  -.0080807   .0012821    -6.30   0.000    -.0105935   -.0055679
          qemp86 |  -.0380187   .0357456    -1.06   0.288    -.1080788    .0320414
           black |   .6608376   .0909327     7.27   0.000     .4826127    .8390624
          hispan |   .4998133   .0910466     5.49   0.000     .3213652    .6782614
          born60 |  -.0510286   .0788849    -0.65   0.518    -.2056401     .103583
           _cons |  -.5995888   .0828238    -7.24   0.000    -.7619206    -.437257
    ------------------------------------------------------------------------------
    (Standard errors scaled using square root of Pearson X2-based dispersion)
     
    . * Estimate of variance-mean ratio (sigmasq) is about 1.52. So overdispersion.
     
     
     
     
     
     
    . predict narr86h_p
    (option mu assumed; predicted mean narr86)
     
    . corr narr86 narr86h_p
    (obs=2725)
     
                 |   narr86 narr86~p
    -------------+------------------
          narr86 |   1.0000
       narr86h_p |   0.2775   1.0000
     
    . di .2775^2
    .07700625
     
    . * A little better fit than linear model, which had R-squared = .0725.

::

    . * Average marginal (or partial) effects:
     
    . margins, dydx(pcnv avgsen tottime ptime86 inc86 qemp86 black hispan born60)
     
    Average marginal effects                          Number of obs   =       2725
    Model VCE    : Robust
     
    Expression   : Predicted mean narr86, predict()
    dy/dx w.r.t. : pcnv avgsen tottime ptime86 inc86 qemp86 black hispan born60
     
    ------------------------------------------------------------------------------
                 |            Delta-method
                 |      dy/dx   Std. Err.      z    P>|z|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
            pcnv |  -.1623969   .0404199    -4.02   0.000    -.2416184   -.0831754
          avgsen |  -.0096136   .0095756    -1.00   0.315    -.0283815    .0091543
         tottime |    .009904   .0083122     1.19   0.233    -.0063877    .0261957
         ptime86 |  -.0398574   .0091649    -4.35   0.000    -.0578202   -.0218946
           inc86 |  -.0032679   .0005148    -6.35   0.000    -.0042769   -.0022588
          qemp86 |  -.0153749   .0139299    -1.10   0.270    -.0426771    .0119272
           black |   .2672451   .0420561     6.35   0.000     .1848168    .3496735
          hispan |   .2021263   .0382924     5.28   0.000     .1270745    .2771781
          born60 |  -.0206361   .0328797    -0.63   0.530    -.0850793     .043807
    ------------------------------------------------------------------------------
     
    . * These are pretty similar to the OLS estimates of the linear model.
