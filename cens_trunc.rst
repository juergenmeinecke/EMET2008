Censoring and Truncation
****************************


A Censored Dependent Variable
================================

Data censoring (of the dependent variable) occurs when we do not see the entire range of :math:`y`.

With censoring we still draw a random sample of units from the population; however, at least for
some units, we do not observe :math:`y_{i}`.

But we know something about :math:`y_{i}`.  (The case where :math:`y_{i}` is not reported and we
know nothing about its value falls under the topic of sample selection.)

There are various kinds of data censoring. An extreme example is *binary censoring*: We know only
whether :math:`y_{i}` is above or below a known value. This kind of censoring is used in studies to
elicit willingness to pay.

*Interval censoring* is common in surveys.  Reponsdents are asked to select an interval for, say,
wealth, rather than report actual wealth. *Interval coding* is another name for interval censoring.

Censoring from above and below are also common in surveys. An example of censoring from above is
*top coding*. For example, a respondent is asked about wealth up to a certain amount -- say, $500,000
-- but if it is more than that then that is all that is reported.

Censoring from above is also used in *duration analysis*, where the dependent variable is the time
before an event occurs. (For example, time until an unemployed person finds a job.) Typically,
duration data are *right censored*: each individual is follow only a certain amount of time.

Censoring from below, or *left censoring*, arises when there are floors, such as minimum wages, and
our desire is to analyze wage as determined by the value of marginal product.

We have already covered statistical methods that are similar to those used for censored data. Probit
and Tobit models have a similar statistical structure. But here we are applying them not to explain
the censored outcome but an underlying outcome.

For example, if we collect information on charitable contributions and see lots of zeros, we assume
the zeros are legitimate responses. By contrast, if we collect information on wealth and see lots of
values at $500,000, then we only know actual wealth is at least $500,000.

Binary Censory 
--------------

Start with a standard linear model for the population:

.. math::

   \begin{aligned}
        y &=&\beta _{0}+\beta _{1}x_{1}+...+\beta _{k}x_{k}+u \\
        E(u\mathbf{x}) &=&0
   \end{aligned}

The variable :math:`y` might be willingness to pay (WTP) for a proposed public project, such as a
park: :math:`y=wtp`.  When we draw (say) family :math:`i` from the population, we would like to
observe :math:`(\mathbf{x}_{i},wtp_{i})`; if we did for all :math:`i`, we would estimate the
:math:`\beta _{j}` by OLS.

WTP can be difficult to elicit, and reported amounts might be noisy. Instead, suppose that each
family is presented with a cost of the project, :math:`c_{i}`. The household either says it is in
favor of the project or not at the presented cost.

Along with :math:`\mathbf{x}_{i}` and :math:`c_{i}`, we observe the binary response

.. math:: w_{i}=1[y_{i}>c_{i}].

Usually we assume that the chance that :math:`y_{i}` equals :math:`c_{i}` is zero.

Importantly, we are interested directly in the :math:`\beta _{j}` because
:math:`E(wtp|\mathbf{x})=\mathbf{x\beta }`, but :math:`\mathbf{\beta }` cannot be estimated by OLS
because :math:`wtp` is not observed.

If we impose some (strong) assumptions on the underlying population and the nature of :math:`c_{i}`,
then we can proceed with maximum likelihood. Assume that

.. math::

    \begin{aligned} &&u_{i}\text{ is independent of }(\mathbf{x}_{i},c_{i}) \\ u_{i} &\sim
    &Normal(0,\sigma ^{2})\text{.}\end{aligned}

So the underlying population model satisfies the classical linear model (CLM) assumptions.

The assumption that :math:`c_{i}` is independent of :math:`u_{i}` means that the cost presented to
each family does not depend on unobserved factors affecting WTP.

The cost :math:`c_{i}` is allowed to be correlated with :math:`\mathbf{x}_{i}`. (So, for example,
present families with higher incomes higher :math:`c_{i}`.)

As it turns out, the binary response :math:`w_{i}` follows a probit model:

.. math::

   P(w_{i}=1|\mathbf{x}_{i},c_{i})=\Phi \lbrack (\beta _{0}/\sigma )+(\beta
   _{1}/\sigma )x_{i1}+...+(\beta _{K}/\sigma )x_{iK}+(-1/\sigma )c_{i}\mathbf{].}

Because we observe :math:`(w_{i},\mathbf{x}_{i},c_{i})` for random draws from the population,
probit of :math:`w_{i}` on :math:`\mathbf{x}_{i},c_{i}` consistently estimates the :math:`\beta
_{j}/\sigma ` as the vector of slope coefficients on the :math:`x_{ij}` and :math:`-1/\sigma ` as
the coefficient on :math:`c_{i}`.

Let :math:`\gamma _{j}=\beta _{j}/\sigma ` and :math:`\alpha =-1/\sigma `. After probit,
:math:`\hat{\beta}_{j}=-\hat{\gamma}_{j}/\hat{\alpha}`. (Note: It makes sense that
:math:`\hat{\alpha}<0`: as cost increases, less likely to favor the project.)

An important special case is when there are no explanatory variables. Instead, we assume

.. math:: wtp\sim Normal(\mu ,\sigma ^{2})

and the goal is to estimate :math:`\mu` -- average willingness to pay.

If :math:`w_{i}` is the binary response as before, then

.. math:: P(w_{i}=1|c_{i})=\Phi \lbrack (\mu /\sigma )+(-1/\sigma )c_{i}]

So, after probit of :math:`w_{i}` on :math:`c_{i}`, to get the intercept :math:`\hat{\gamma}` and
slope :math:`\hat{\alpha}`, average WTP in the population is estimated as

.. math:: \hat{\mu}=-\hat{\gamma}/\hat{\alpha}

Stata can be used to obtain a valid standard error for :math:`\hat{\mu}`, which is a nonlinear
function of :math:`\hat{\gamma}` and :math:`\hat{\alpha} `. (If we could observe the :math:`wtp_{i}`
we could just use the sample average.)

Costs of binary censoring can be severe. If we could observe :math:`y_{i}`, specifying

.. math:: E(y|\mathbf{x})=\beta _{0}+\beta _{1}x_{1}+\cdots +\beta _{k}x_{k}

would suffice for consistent estimation of the :math:`\beta _{j}`. We could just use OLS.

With binary censoring, heteroskedasticity and nonnormality cause bias and inconsistency because
probit relies on these assumptions.

If :math:`y=wtp` is always nonnegative, assuming it follows something like a Tobit model would be
better. This allows :math:`wtp=0` but not negative WTP.

Estimation is the same as the normal case, but we compute the mean from the Tobit model.

If :math:`wtp>0`, a lognormal distribution can be used, although it has fat tails.

With explanatory variables, we can write this as

.. math:: \log (y)=\beta _{0}+\beta _{1}x_{1}+\cdots +\beta _{k}x_{k}+u

where :math:`u\sim Normal(0,\sigma ^{2})` (and still independent of the :math:`x_{j}`).

To estimate, we just replace :math:`c_{i}` with :math:`\log (c_{i})` in the probit analysis. If an
:math:`x_{j}` is a natural log then we can obtain an elasticity.

.. admonition:: Example: Estimating Willingness to Pay for a New Park

    Data are in WTP\_PARK.DTA. Each person (family representative) is presented with an annual cost,
    and is asked if he/she would favor the park at that cost. The costs are assigned from four values
    independent of family characteristics.

    Family income is also asked.

    Note: These data are fictitious.

::

    . use wtp_park
     
    . des
     
    Contains data from wtp_park.dta
      obs:           826                          
     vars:             5                          5 Dec 2012 14:21
     size:        12,390                          
    --------------------------------------------------------------------------------------------------
                  storage  display     value
    variable name   type   format      label      variable label
    --------------------------------------------------------------------------------------------------
    favor           byte   %9.0g                  =1 if in favor of park
    cost            int    %9.0g                  cost amount presented to individual
    income          float  %9.0g                  family income, $1000s
    lcost           float  %9.0g                  log(cost)
    lincome         float  %9.0g                  log(income)
    --------------------------------------------------------------------------------------------------
    Sorted by:
     
    . tab cost
     
    cost amount |
      presented |
             to |
     individual |      Freq.     Percent        Cum.
    ------------+-----------------------------------
             15 |        210       25.42       25.42
             35 |        209       25.30       50.73
             65 |        230       27.85       78.57
            105 |        177       21.43      100.00
    ------------+-----------------------------------
          Total |        826      100.00

::

    . * More than half of the people favor the park:
     
    . tab favor
     
       =1 if in |
       favor of |
           park |      Freq.     Percent        Cum.
    ------------+-----------------------------------
              0 |        397       48.06       48.06
              1 |        429       51.94      100.00
    ------------+-----------------------------------
          Total |        826      100.00
     
    . * Not suprisingly, the fraction favoring
    . * falls as the cost increases.
     
    . tab cost favor, row
     
          cost |
        amount |
     presented |   =1 if in favor of
            to |         park
    individual |         0          1 |     Total
    -----------+----------------------+----------
            15 |        69        141 |       210 
               |     32.86      67.14 |    100.00 
    -----------+----------------------+----------
            35 |        94        115 |       209 
               |     44.98      55.02 |    100.00 
    -----------+----------------------+----------
            65 |       122        108 |       230 
               |     53.04      46.96 |    100.00 
    -----------+----------------------+----------
           105 |       112         65 |       177 
               |     63.28      36.72 |    100.00 
    -----------+----------------------+----------
         Total |       397        429 |       826 
               |     48.06      51.94 |    100.00 
          Total |      1,103      100.00

::

    . probit favor cost
     
    Probit regression                                 Number of obs   =        826
                                                      LR chi2(1)      =      37.78
                                                      Prob > chi2     =     0.0000
    Log likelihood = -553.03073                       Pseudo R2       =     0.0330
     
    ------------------------------------------------------------------------------
           favor |      Coef.   Std. Err.      z    P>|z|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
            cost |  -.0083184   .0013655    -6.09   0.000    -.0109948    -.005642
           _cons |   .4925274   .0851899     5.78   0.000     .3255582    .6594965
    ------------------------------------------------------------------------------
     
    . margeff
     
    Average partial effects after probit
          y  = Pr(favor) 
     
    ------------------------------------------------------------------------------
        variable |      Coef.   Std. Err.      z    P>|z|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
            cost |  -.0031957   .0004868    -6.56   0.000    -.0041499   -.0022415
    ------------------------------------------------------------------------------
     
     
    . * So every $10 of cost reduces probability of favoring by about .032 or 3.2
    . * percentage points.

::

    . * Estimate average WTP:
     
    . nlcom -_b[_cons]/_b[cost]
     
           _nl_1:  -_b[_cons]/_b[cost]
     
    ------------------------------------------------------------------------------
           favor |      Coef.   Std. Err.      z    P>|z|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
           _nl_1 |   59.20908   5.402223    10.96   0.000     48.62092    69.79724
    ------------------------------------------------------------------------------
     
    . * Estimated average WTP is about $59. This 95% confidence interval is valid.
     
    . * Get the standard deviation of WTP:
     
    . nlcom -1/_b[cost]
     
           _nl_1:  -1/_b[cost]
     
    ------------------------------------------------------------------------------
           favor |      Coef.   Std. Err.      z    P>|z|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
           _nl_1 |   120.2148   19.73417     6.09   0.000     81.53655    158.8931
    ------------------------------------------------------------------------------
     
    . * If the mean is 59 and SD is 120 and WTP is normally distributed, lots
    . * of people in the population have a negative WTP.
    . * This may be true: some people, especially those living near the site,
    . * might find a new park to be a nuisance.
    . * But the result also could be an artifact of the normality assumption.

::

    . * If we insist WTP >= 0 but allow zero, we can compute the mean WTP
    . * acting as if WTP follows a Tobit model. Of course this gives a
    . * larger mean WTP:
     
    . nlcom normal(_b[_cons])*(-_b[_cons]/_b[cost]) 
               + (-1/_b[cost])*normalden(_b[_cons])
     
    \qquad \qquad \qquad _nl_1:  normal(_b[_cons])*(-_b[_cons]/_b[cost]) 
                    + (-1/_b[cost])*normalden(_b[_cons])
     
    ------------------------------------------------------------------------------
           favor |      Coef.   Std. Err.      z    P>|z|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
           _nl_1 |   83.26551   8.470907     9.83   0.000     66.66283    99.86818
    ------------------------------------------------------------------------------
     
    . * P(WTP = 0) can be estimated, too:
     
    . nlcom normal(-_b[_cons])
     
           _nl_1:  normal(-_b[_cons])
     
    ------------------------------------------------------------------------------
           favor |      Coef.   Std. Err.      z    P>|z|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
           _nl_1 |   .3111733   .0301038    10.34   0.000     .2521708    .3701758
    ------------------------------------------------------------------------------
     
    . * We estimate about 31.1% of the population is willing to pay nothing.

::

    . * Does income affect WTP?
     
    . probit favor cost income
     
    Probit regression                                 Number of obs   =        826
                                                      LR chi2(2)      =      55.38
                                                      Prob > chi2     =     0.0000
    Log likelihood = -544.23201                       Pseudo R2       =     0.0484
     
    ------------------------------------------------------------------------------
           favor |      Coef.   Std. Err.      z    P>|z|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
            cost |  -.0083046   .0013767    -6.03   0.000    -.0110028   -.0056064
          income |   .0035519   .0009026     3.94   0.000     .0017829     .005321
           _cons |   .3081706   .0969558     3.18   0.001     .1181408    .4982003
    ------------------------------------------------------------------------------
     
    . nlcom -_b[income]/_b[cost]
     
           _nl_1:  -_b[income]/_b[cost]
     
    ------------------------------------------------------------------------------
           favor |      Coef.   Std. Err.      z    P>|z|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
           _nl_1 |   .4277049   .1294219     3.30   0.001     .1740426    .6813673
    ------------------------------------------------------------------------------
     
    . * Every $1000 in income increases mean WTP by about 43 cents.

::

    . * log(income) works a bit better in terms of fit, but the coefficient
    . * is harder to interpret:
     
    . probit favor cost lincome
     
    Probit regression                                 Number of obs   =        826
                                                      LR chi2(2)      =      57.07
                                                      Prob > chi2     =     0.0000
    Log likelihood = -543.38662                       Pseudo R2       =     0.0499
     
    ------------------------------------------------------------------------------
           favor |      Coef.   Std. Err.      z    P>|z|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
            cost |  -.0083663   .0013788    -6.07   0.000    -.0110687    -.005664
         lincome |   .2235411   .0513154     4.36   0.000     .1229647    .3241174
           _cons |  -.3011861   .2004792    -1.50   0.133    -.6941182    .0917459
    ------------------------------------------------------------------------------

::

    . * A constant elasticity model is appealing but it does assume that
    . * WTP has a lognormal distribution, which means WTP > 0 and that
    . * the distribution has a fat right tail.
     
    . probit favor lcost lincome
     
    Probit regression                                 Number of obs   =        826
                                                      LR chi2(2)      =      58.78
                                                      Prob > chi2     =     0.0000
    Log likelihood = -542.53053                       Pseudo R2       =     0.0514
     
    ------------------------------------------------------------------------------
           favor |      Coef.   Std. Err.      z    P>|z|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
           lcost |   -.392113   .0632509    -6.20   0.000    -.5160826   -.2681435
         lincome |   .2255349   .0513472     4.39   0.000     .1248963    .3261735
           _cons |    .717898   .2994621     2.40   0.017      .130963    1.304833
    ------------------------------------------------------------------------------
     
    . nlcom -_b[lincome]/_b[lcost]
     
           _nl_1:  -_b[lincome]/_b[lcost]
     
    ------------------------------------------------------------------------------
           favor |      Coef.   Std. Err.      z    P>|z|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
           _nl_1 |   .5751783   .1585236     3.63   0.000     .2644777    .8858789
    ------------------------------------------------------------------------------
     
    . * It fits a bit better; see log likelihood values.
     
    . * The estimated elasticity of WTP with respect to income is about .58, and
    . * it is statistically different from zero.



Interval Censoring (or Interval Coding)
=======================================

Binary censoring is a special case of interval censoring.

Again assume that the underlying population model is

.. math:: y=\beta _{0}+\beta _{1}x_{1}+...+\beta _{k}x_{k}+u

For random draw :math:`i`, let :math:`c_{i1}<c_{i2}<...<c_{iM}` denote the *known* interval limits.
These are specified as part of the survey design. For example, rather than asking individuals to
report actual annual income, they report the interval that their income falls into.

Often the :math:`c_{im}` do not depend on :math:`i`, but sometimes they do. (Extensions of
WTP example.) If the :math:`c_{im}` change with :math:`i`, the should be independent of the
unobervables, :math:`u_{i}`.

Estimation proceeds by defining a variable denoting the interval that :math:`y_{i}` falls into:

.. math::

   \begin{aligned}
   w_{i} &=&0\text{ \ \ if }y_{i}\leq c_{i1} \\
   w_{i} &=&1\text{ \ \ if }c_{i1}<y_{i}\leq c_{i2} \\
   &&\vdots \\
   w_{i} &=&M\text{ \ \ if }y>c_{iM}\end{aligned}

Remember, we observe only :math:`w_{i}`, not :math:`y_{i}`.

If the linear model for :math:`y` satisfies the classical linear model assumptions (including
homosedkasticity and normality), the :math:`\beta _{j}` and :math:`\sigma ^{2}` can be estimated –
just as if we could observe :math:`y_{i}` and use OLS.

As in the case of binary censoring, when we obtain the interval regression estimates we interpret
the :math:`\hat{\beta}_{j}` *as if* we had been able to run the regression :math:`y_{i}` on
:math:`x_{i1},...,x_{ik}`, :math:`i=1,...,n`.

In Stata, for each observation we specify variables :math:`lower` and :math:`upper`, and these
determine the interval that :math:`y_{i}` falls into. If :math:`y_{i}` is below the smallest
interval value, :math:`c_{i1}`, :math:`lower_{i}` is set to missing. If :math:`y_{i}` is above the
largest interval value, :math:`c_{iM}`, :math:`upper_{i}` is set to missing. ::

    intreg lower upper x1 x2 ... xk

The next example uses data that were interval coded based on :math:`y=nettfa`. Normally we would not
know the :math:`y_{i}`.


.. admonition:: Example: Interval Coding for Net Financial Wealth

    Data in 401KSUBS\_INTCODE.DTA.

::

     
    . use 401ksubs_intcode
    . sum nettfa
     
        Variable |       Obs        Mean    Std. Dev.       Min        Max
    -------------+--------------------------------------------------------
          nettfa |       975    14.87889    57.24609     -59.97   1134.098
     
    . list nettfa lower upper in 1/10
     
         +------------------------+
         | nettfa   lower   upper |
         |------------------------|
      1. |  4.575       0       5 |
      2. |    154      25       . |
      3. |  18.45      10      25 |
      4. |   29.6      25       . |
      5. |      0       .       0 |
         |------------------------|
      6. |  9.687       5      10 |
      7. |    .13       0       5 |
      8. | -21.02       .       0 |
      9. | 24.999      10      25 |
     10. |  2.999       0       5 |
         +------------------------+
     
    . * The intervals are y <= 0, 0 < y <= 5, 5 < y <= 10, 10 < y <= 25, y > 25.

::

    . tab lower
     
          lower |
       interval |
      limit for |
         nettfa |      Freq.     Percent        Cum.
    ------------+-----------------------------------
              0 |        264       41.31       41.31
              5 |         90       14.08       55.40
             10 |        133       20.81       76.21
             25 |        152       23.79      100.00
    ------------+-----------------------------------
          Total |        639      100.00
     
    . * 336 observations have nettfa <= 0

::

    . intreg lower upper inc incsq age agesq male e401k
     
    Interval regression                               Number of obs   =        975
                                                      LR chi2(6)      =     274.90
    Log likelihood = -1446.7593                       Prob > chi2     =     0.0000
     
    ------------------------------------------------------------------------------
                 |      Coef.   Std. Err.      z    P>|z|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
             inc |   .6263831   .0885805     7.07   0.000     .4527684    .7999977
           incsq |  -.0028399   .0008818    -3.22   0.001    -.0045683   -.0011116
             age |    -.13666   .3724458    -0.37   0.714    -.8666404    .5933204
           agesq |   .0056804   .0043551     1.30   0.192    -.0028554    .0142163
            male |  -.6346241    1.00675    -0.63   0.528    -2.607818     1.33857
           e401k |   6.577516   1.044148     6.30   0.000     4.531023     8.62401
           _cons |  -16.37731   7.627359    -2.15   0.032    -31.32666   -1.427963
    -------------+----------------------------------------------------------------
        /lnsigma |   2.626524   .0368932    71.19   0.000     2.554215    2.698834
    -------------+----------------------------------------------------------------
           sigma |   13.82563   .5100724                       12.8612    14.86239
    ------------------------------------------------------------------------------
     
      Observation summary:       336  left-censored observations
                                   0     uncensored observations
                                 152 right-censored observations
                                 487       interval observations

::

    . * How does this compare if we use the uncensored data in standard OLS
    . * regression?
     
    . reg nettfa inc incsq age agesq male e401k
     
          Source |       SS       df       MS              Number of obs =     975
    -------------+------------------------------           F(  6,   968) =   15.74
           Model |  283681.821     6  47280.3036           Prob > F      =  0.0000
        Residual |  2908228.37   968  3004.36815           R-squared     =  0.0889
    -------------+------------------------------           Adj R-squared =  0.0832
           Total |  3191910.19   974  3277.11518           Root MSE      =  54.812
     
    ------------------------------------------------------------------------------
          nettfa |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
             inc |   1.109313   .3183536     3.48   0.001     .4845707    1.734056
           incsq |  -.0041516   .0031799    -1.31   0.192    -.0103919    .0020888
             age |  -1.946907   1.337097    -1.46   0.146    -4.570849    .6770356
           agesq |   .0335103   .0156526     2.14   0.033     .0027935    .0642272
            male |   2.754853   3.618632     0.76   0.447    -4.346415    9.856121
           e401k |    7.51211   3.778518     1.99   0.047     .0970803    14.92714
           _cons |    3.15536   27.31521     0.12   0.908    -50.44849    56.75921
    ------------------------------------------------------------------------------

::

    . * The OLS estimates are not especially close to the interval regression
    . * estimates, quite likely because nettfa is 
    . * neither homoskedastic nor conditionally normally distributed. Of course,
    . * the conditional mean may be misspecified, too. And it is just one sample of
    . * data.
    . * The interval regression estimates are less sensitive to extreme
    . * values of nettfa. But then we are admitting the underlying distribution
    . * cannot be homoskedastic normal (because then there would not really be
    . * "outliers")

**Violations of the Assumptions**

Unfortunately, heteroskedasticity and nonnormality cause interval regression estimators to be
unreliable (inconsistent). So, like binary censoring, interval censoring is costly.

One way to check robustness of the results: with many intervals, can combine some intervals and
reestimate the parameters using interval regression. If the underlying CLM holds, the estimates
should differ only by sampling error.

Censoring from Above and from Below 
-----------------------------------

Again start with the linear population model

.. math:: y=\beta _{0}+\beta _{1}x_{1}+...+\beta _{k}x_{k}+u

and let :math:`i` denote a random draw. Then we only observe

.. math:: w_{i}=\min (y_{i},c_{i})

where :math:`c_{i}` is the right censoring variable that may change with :math:`i` (but sometimes it
does not).

**Top coding** is a common example of right censoring, where, say, income or wealth is recorded up
to a certain amount, and then we just know whether the value is above the amount.

Censoring of duration data is also common.

Left censoring or censoring from below occurs with minimum wages when we are interested in the value
of marginal product.

In the leading case, :math:`y` follows a classical linear model in the population of interest, that
is, :math:`u` is independent of :math:`\mathbf{x}` and normally distributed. Plus we assume that, if
:math:`c_{i}` changes across :math:`i`, it is independent of :math:`u_{i}` (but may depend on
:math:`\mathbf{x}_{i}).`

When :math:`y` follows a CLM but is censored from above or below, we have the *censored normal
regression model*.

The likelihood function is very similar to that for a Tobit model, but the models have different
purposes. The Tobit model is for corner solution responses without data censoring.

Censored normal regression is to model an underlying linear relationship but account for censoring.

In Stata, the variable that is used is :math:`w_{i}=\min (y_{i},c_{i})`, and an indicator is needed
for whether the data are censored or not. ::

    cnreg w x1 x2 ... xK, cen(cens)

where ``cens`` is the dummy variable equal to one of the observation is censored.

For left censoring, :math:`w_{i}=\max (y_{i},c_{i})` and ``cens`` is :math:`-1` for censored, zero
for uncensored.


.. admonition:: Example: Top Coding of Net Financial Wealth

    Data in 401KSUBS\_TOPCODE.DTA

::

     
    . use 401ksubs_topcode
     
    . * Censoring is at nettfa >= 50.
     
    . sum nettfa nettfac
     
        Variable |       Obs        Mean    Std. Dev.       Min        Max
    -------------+--------------------------------------------------------
          nettfa |       975    35.55443      81.435       -409   1003.126
         nettfac |       975    17.80794    26.73529       -409         50
     
    . tab cens
     
          =1 if |
         nettfa |
       censored |      Freq.     Percent        Cum.
    ------------+-----------------------------------
              0 |        751       77.03       77.03
              1 |        224       22.97      100.00
    ------------+-----------------------------------
          Total |        975      100.00
     
    . * So about 23% of the observations are right censored.

::

    . * First, linear regression using the actual data on nettfa. In real 
    . * applications, we would not be able to do this:
     
    . reg nettfa inc incsq age agesq male e401k, robust
     
    Linear regression                                      Number of obs =     975
                                                           F(  6,   968) =   38.66
                                                           Prob > F      =  0.0000
                                                           R-squared     =  0.2588
                                                           Root MSE      =  70.326
     
    ------------------------------------------------------------------------------
                 |               Robust
          nettfa |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
             inc |  -.4726794   .6361378    -0.74   0.458    -1.721048    .7756887
           incsq |   .0116774   .0054104     2.16   0.031     .0010599    .0222948
             age |  -1.527668   1.838973    -0.83   0.406      -5.1365    2.081165
           agesq |   .0354728   .0221191     1.60   0.109    -.0079341    .0788797
            male |  -9.332761   4.586691    -2.03   0.042    -18.33377   -.3317568
           e401k |   10.70226   4.673405     2.29   0.022     1.531087    19.87343
           _cons |   8.342726   33.27078     0.25   0.802    -56.94843    73.63389
    ------------------------------------------------------------------------------

::

    . cnreg nettfac inc incsq age agesq male e401k, cen(cens)
     
    Censored-normal regression                        Number of obs   =        975
                                                      LR chi2(6)      =     301.64
                                                      Prob > chi2     =     0.0000
    Log likelihood = -3774.6932                       Pseudo R2       =     0.0384
     
    ------------------------------------------------------------------------------
         nettfac |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
             inc |   .7225285   .1192285     6.06   0.000     .4885527    .9565043
           incsq |  -.0018362   .0008255    -2.22   0.026    -.0034562   -.0002162
             age |  -.1480192   .7230439    -0.20   0.838    -1.566932    1.270893
           agesq |   .0122743   .0081677     1.50   0.133    -.0037542    .0283028
            male |  -2.032747   3.123538    -0.65   0.515    -8.162425    4.096931
           e401k |   7.496106    2.00374     3.74   0.000     3.563936    11.42828
           _cons |  -31.34548   15.02683    -2.09   0.037    -60.83437   -1.856601
    -------------+----------------------------------------------------------------
          /sigma |   28.67045   .7756753                      27.14825    30.19264
    ------------------------------------------------------------------------------
      Observation summary:         0  left-censored observations
                                 751     uncensored observations
                                 224 right-censored observations
     
    . * Estimates are, unfortunately, very different. Perhaps because nettfa
    . * is not normally distributed and exhibits heteroskesdasticity. But the 
    . * right censoring is more resilient to large values of nettfa.



A Truncated Sample Based on the Dependent Variable
=====================================================

With *truncated sampling* we ignore part of the population based on the dependent variable
:math:`y`. Thus, we have less information than in a censored sample because we never observe
anything about a certain subset of the population.

Suppose in the financial wealth example we simply ignore the population with :math:`nettfa>50`, that
is, $50,000.  Our sample will not be representative of the entire population.

In general, suppose we ignore the population with :math:`y>c`, where :math:`c` is some fixed value.
Let :math:`f(y|\mathbf{x})` denote the probability density of :math:`y` given :math:`\mathbf{x}` for
the entire population.

When we only sample from the subet with :math:`y\leq c`, the density we are sampling from is

.. math:: \frac{f(y|\mathbf{x})}{P(y\leq c|\mathbf{x})}=\frac{f(y|\mathbf{x})}{F(c|\mathbf{x})}

where :math:`F(\cdot |\mathbf{x})` is the cumulative distribution function associated with
:math:`f(\cdot |\mathbf{x})`.

The rescaling of the density ensures that the the total probability is one.

We can see the consequences of obtaining a truncated sample on the regression line.

Suppose in the population that income is a linear function of education, but we only observe people
if their incomes are less than $50,000.

The *truncated normal regression model* has a normal population distribution.

With truncation on the right at the value 50, the Stata command is ::

    truncreg y x1 ... xK, ul(50)

where ``ul(50)`` means the upper limit is 50.

As with censoring, truncated the sample is costly. We are interested in :math:`E(y|\mathbf{x})=\beta
_{0}+\mathbf{x\beta }` in the entire population, but because of the truncated sampling, we have to
specify an entire population distribution (homoskedastic normal).

Again, differs from the censored normal regression model in that we observe no information on units
in the subpopulation with :math:`y_{i}>c `. In the censored case, we have a random sample of units,
which means we observe :math:`\mathbf{x}_{i}`, and we can use that in estimation.

Because a censored data set contains more information, it is more efficient to use the censored
observations along with uncensored observations. But you can also analyze the data set as a
truncated data set by dropping the censored observations.

If you have a choice, you should use censored regression, not truncated regression.

As with censored regression, we interpret the results *as if* we had been able to run a linear
regression using a random sample from the entire population.

We can have a lower limit instead of an upper limit, and we can also have both.

Sometimes the limits change with :math:`i`.  Hausman and Wise (1974):math:`\ `\ analysis data from a
negative income tax experiment where eligibility depended on family size in addition to income
(where :math:`y=income`).

The general Stata command is ::

    truncreg y x1 ... xK, ll(lower) ul(upper)

where ``lower`` and ``upper`` are variables defined in the data set.

.. admonition:: Example: Truncating the Wealth Distribution

   Now act as if we only sampled people with nettfa < 50

::

     
    . use 401ksubs_topcode 
     
    . truncreg nettfa inc incsq age agesq male e401k, ul(50)
    (note: 224 obs. truncated)
     
    Truncated regression
    Limit:   lower =       -inf                             Number of obs =    751
             upper =         50                             Wald chi2(6)  =  57.16
    Log likelihood = -3351.6879                             Prob > chi2   = 0.0000
     
    ------------------------------------------------------------------------------
          nettfa |      Coef.   Std. Err.      z    P>|z|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
             inc |   .6447338   .1412821     4.56   0.000      .367826    .9216416
           incsq |  -.0034965   .0010597    -3.30   0.001    -.0055735   -.0014194
             age |   .1806256   .7896731     0.23   0.819    -1.367105    1.728356
           agesq |   .0032957   .0090657     0.36   0.716    -.0144727    .0210641
            male |   .1300546   3.379858     0.04   0.969    -6.494346    6.754455
           e401k |    4.09938   2.224616     1.84   0.065    -.2607873    8.459548
           _cons |  -24.23088   16.15679    -1.50   0.134    -55.89761    7.435844
    -------------+----------------------------------------------------------------
          /sigma |   25.12179   .9167748    27.40   0.000     23.32494    26.91863
    ------------------------------------------------------------------------------

::

    . * If underlying CLM is correct, truncated and censored regression should
    . * give similar answers, with censored more efficient.
     
    . cnreg nettfac inc incsq age agesq male e401k, cen(cens)
     
    Censored-normal regression                        Number of obs   =        975
                                                      LR chi2(6)      =     301.64
                                                      Prob > chi2     =     0.0000
    Log likelihood = -3774.6932                       Pseudo R2       =     0.0384
     
    ------------------------------------------------------------------------------
         nettfac |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
             inc |   .7225285   .1192285     6.06   0.000     .4885527    .9565043
           incsq |  -.0018362   .0008255    -2.22   0.026    -.0034562   -.0002162
             age |  -.1480192   .7230439    -0.20   0.838    -1.566932    1.270893
           agesq |   .0122743   .0081677     1.50   0.133    -.0037542    .0283028
            male |  -2.032747   3.123538    -0.65   0.515    -8.162425    4.096931
           e401k |   7.496106    2.00374     3.74   0.000     3.563936    11.42828
           _cons |  -31.34548   15.02683    -2.09   0.037    -60.83437   -1.856601
    -------------+----------------------------------------------------------------
          /sigma |   28.67045   .7756753                      27.14825    30.19264
    ------------------------------------------------------------------------------
      Observation summary:         0  left-censored observations
                                 751     uncensored observations
                                 224 right-censored observations
     
    . * Not terrible. Censored regression does have smaller standard errors, 
    . * as the theory predicts. But the e401k variable has a notably larger effect 
    . * using cnreg.

::

    . * What would happen if we use regression on the truncated sample?
     
    . reg nettfa inc incsq age agesq male e401k if nettfa < 50
     
          Source |       SS       df       MS              Number of obs =     751
    -------------+------------------------------           F(  6,   744) =   10.06
           Model |  29631.6853     6  4938.61422           Prob > F      =  0.0000
        Residual |  365182.579   744    490.8368           R-squared     =  0.0751
    -------------+------------------------------           Adj R-squared =  0.0676
           Total |  394814.265   750  526.419019           Root MSE      =  22.155
     
    ------------------------------------------------------------------------------
          nettfa |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
             inc |   .5094416   .1113386     4.58   0.000     .2908665    .7280167
           incsq |  -.0027969   .0008422    -3.32   0.001    -.0044503   -.0011434
             age |   .1964479   .6107746     0.32   0.748    -1.002599    1.395495
           agesq |   .0019018   .0069766     0.27   0.785    -.0117944    .0155981
            male |   .1273561   2.616432     0.05   0.961    -5.009113    5.263825
           e401k |   3.121054   1.707491     1.83   0.068    -.2310204    6.473127
           _cons |  -21.14946   12.57893    -1.68   0.093    -45.84388    3.544962
    ------------------------------------------------------------------------------
     
    . * The e401k coefficient is even smaller, and the income coefficients
    . * differ by a lot from truncated regression.

