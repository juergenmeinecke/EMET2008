Binary Response Models
**************************

Introduction
===============

Often we want to explain a qualitative outcome. For example, suppose we want to study the question
of married women’s labor force participation (in the labor force or out). Or, we want to know
whether a young man is arrested for a crime during a certain period of time. Or, we want to know if
an employee participates in a 401(k) pension plan.

In these cases, the variable :math:`y` we want to explain is a binary (zero-one) variable.

Sometimes we say we have a dummy dependent variable.

The most we can know about :math:`y` in terms of explanatory variables :math:`x_{1}`, :math:`x_{2}`,
..., :math:`x_{k}` is

.. math:: P(y=1|x_{1},x_{2},...,x_{k})=p(x_{1},x_{2},...,x_{k})=p(\mathbf{x})\mathbf{,}

which is called the **response probability**. (:math:`\mathbf{x}` is shorthand for all of the
explanatory variables). It is the probability of a success (used loosely), that is, :math:`y=1`.

Of course :math:`P(y=0|\mathbf{x})=1-P(y=1|\mathbf{x})=1-p(\mathbf{x}).`

As in regression, we are interested in the partial effects of the :math:`x_{j}` on
:math:`p(\mathbf{x})`. For continuous :math:`x_{j}`, these are usually the partial derivatives.

.. math:: \frac{\partial p(\mathbf{x)}}{\partial x_{j}}.

For discrete :math:`x_{j}`, look at changes in the response probability (usually holding other
variables fixed). For example, if :math:`x_{K}=train` (job training indicator) and :math:`y` is an
employment indicator,

.. math:: p(x_{1},...,x_{K-1},1)-p(x_{1},...,x_{K-1},0)

is the effect of job training on the employment probability, at given values for the other
covariates.

In nonlinear models generally, and binary response models specifically, it is often useful to have a
single number to summarize the relationship between :math:`P(y=1|\mathbf{x})` and :math:`x_{j}`. In
a linear model that is simply the coefficient.

Generally, we might report an estimated *average partial effect (APE)* (also called the *average
marginal effect*). The APE for a continuous :math:`x_{j}` is

.. math:: E_{\mathbf{x}}\left[ \frac{\partial p(\mathbf{x)}}{\partial x_{j}}\right] ,

which means we average the partial effect across the population
distribution of :math:`\mathbf{x}`. This is a weighted average of the
partial effects at each outcome :math:`\mathbf{x}`.

Suppose :math:`x_{K}` is a binary variable. Then its APE is

.. math:: E_{\mathbf{x}_{(K)}}[p(\mathbf{x}_{(K)},1)-p(\mathbf{x}_{(K)},0)]

where :math:`\mathbf{x}_{(K)}` is the :math:`1\times K` vector with :math:`x_{K}` excluded.

Another partial effect that has been reported in empirical work is the *partial effect at the
average (PEA)*. For a continous variable :math:`x_{j}`,

.. math:: \frac{\partial p(\mathbf{\mu }_{\mathbf{x}}\mathbf{)}}{\partial x_{j}}\text{.}

In nonlinear models, the APE and PEA can be very different: the expected value does not pass through
nonlinear functions.

Because :math:`\mathbf{\mu }_{\mathbf{x}}` might not even represent a population unit – for example,
if :math:`\mathbf{x}` includes discrete variables, such as dummy variables – the PEA might not be
especially interesting.

Some simple, useful facts about binary (zero-one) random variables are

.. math::

   \begin{aligned} E(y|\mathbf{x}) &=&P(y=1|\mathbf{x})=p(\mathbf{x)} \\ Var(y|\mathbf{x})
   &=&p(\mathbf{x})[1-p(\mathbf{x})]\end{aligned}

So a binary variable has natural heteroskedasticity except in the special (and not very interesting)
case where :math:`p(\mathbf{x})` does not depend on :math:`\mathbf{x}`.

Review of the Linear Probability Model
=========================================

The linear probability model is just a standard linear model where :math:`y` happens to be binary.
If we write down the model

.. math:: y=\beta _{0}+\beta _{1}x_{1}+\beta _{2}x_{2}+...+\beta _{k}x_{k}+u

when :math:`y` is binary, how can we interpret the parameters

:math:`y` can only change from 0 to 1 or 1 to 0. Suppose :math:`\beta _{1}=.035` and
:math:`x_{1}=educ`. What does it mean for a one year increased in :math:`educ` to increase :math:`y`
by :math:`.035?`

We must rely on the expected value formulation of linear regression. Remember we can interepret the
regression model as

.. math::

   E(y|\mathbf{x})=\beta _{0}+\beta _{1}x_{1}+\beta _{2}x_{2}+...+\beta _{k}x_{k}

We can interpret :math:`\beta _{j}` as

.. math::

   \Delta E(y|\mathbf{x})=\beta _{j}\Delta x_{j}\text{ holding other explanatory variables fixed}

Therefore,

.. math::

   \beta _{j}=\Delta E(y|\mathbf{x})\text{ when }\Delta x_{j}=1,\text{ holding other explanatory
   variables fixed}

Key relationship when :math:`y` is binary:

.. math:: E(y|\mathbf{x})=P(y=1|\mathbf{x})

Thus, when we apply the linear model to binary :math:`y` we are really saying

.. math::

   P(y=1|\mathbf{x})=\beta _{0}+\beta _{1}x_{1}+\beta _{2}x_{2}+...+\beta _{k}x_{k},

so that the response probability is linear (in the parameters).  Therefore, we call a linear model
for binary :math:`y` a *linear probability model (LPM)*.

The important point is that all partial effects are effects on the probability that :math:`y=1`
(sometimes called a success). Since :math:`P(y=0|\mathbf{x})=1-P(y=1|\mathbf{x})` it is the only
probability we need.

.. math::

   \Delta P(y=1|\mathbf{x})=\beta _{j}\Delta x_{j}\text{ holding other explanatory variables fixed}

and so

.. math::

   \beta _{j}=\Delta P(y=1|\mathbf{x})\text{ when }\Delta x_{j}=1,\text{ holding other explanatory
   variables fixed}

The sample analog holds as well. When we have the OLS regression line

.. math::
   \hat{y}=\hat{\beta}_{0}+\hat{\beta}_{1}x_{1}+\hat{\beta}_{2}x_{2}+...+\hat{\beta}_{k}x_{k},

:math:`\hat{y}` is now the predicted probability :math:`P(y=1|\mathbf{x})`. So, the intercept is the
predicted probability when each :math:`x_{j}` is set to zero (which, as with other regression
applications, may not make sense).

:math:`\hat{\beta}_{j}` measures the change in the estimated probability of a success when
:math:`\Delta x_{j}=1`, other factors held fixed.

Just like in any regression application, we can have explanatory variables in logs, quadratics, and
interactions as well as binary regressors.

.. admonition:: Example: Married Women’s Labor Force Participation (MROZ.DTA)

    The variable :math:`inlf` is one if a woman worked for a wage during a certain year, and zero if
    not. We estimate a linear probability model to see the effects of variables on the probability of
    being in the labor force.

::

     
    . des inlf nwifeinc educ exper age kidslt6 kidsge6
     
                  storage  display     value
    variable name   type   format      label      variable label
    -------------------------------------------------------------------------------------------------------------------------
    inlf            byte   %9.0g                  =1 if in lab frce, 1975
    nwifeinc        float  %9.0g                  (faminc - wage*hours)/1000
    educ            byte   %9.0g                  years of schooling
    exper           byte   %9.0g                  actual labor mkt exper
    age             byte   %9.0g                  woman's age in yrs
    kidslt6         byte   %9.0g                  # kids < 6 years
    kidsge6         byte   %9.0g                  # kids 6-18
     
    . sum inlf nwifeinc educ exper age kidslt6 kidsge6
     
        Variable |       Obs        Mean    Std. Dev.       Min        Max
    -------------+--------------------------------------------------------
            inlf |       753    .5683931    .4956295          0          1
        nwifeinc |       753    20.12896     11.6348  -.0290575         96
            educ |       753    12.28685    2.280246          5         17
           exper |       753    10.63081     8.06913          0         45
             age |       753    42.53785    8.072574         30         60
    -------------+--------------------------------------------------------
         kidslt6 |       753    .2377158     .523959          0          3
         kidsge6 |       753    1.353254    1.319874          0          8

::

    . reg inlf nwifeinc educ exper expersq age kidslt6 kidsge6
     
          Source |       SS       df       MS              Number of obs =     753
    -------------+------------------------------           F(  7,   745) =   38.22
           Model |  48.8080578     7  6.97257969           Prob > F      =  0.0000
        Residual |  135.919698   745  .182442547           R-squared     =  0.2642
    -------------+------------------------------           Adj R-squared =  0.2573
           Total |  184.727756   752  .245648611           Root MSE      =  .42713
     
    ------------------------------------------------------------------------------
            inlf |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
        nwifeinc |  -.0034052   .0014485    -2.35   0.019    -.0062488   -.0005616
            educ |   .0379953    .007376     5.15   0.000      .023515    .0524756
           exper |   .0394924   .0056727     6.96   0.000     .0283561    .0506287
         expersq |  -.0005963   .0001848    -3.23   0.001    -.0009591   -.0002335
             age |  -.0160908   .0024847    -6.48   0.000    -.0209686    -.011213
         kidslt6 |  -.2618105   .0335058    -7.81   0.000    -.3275875   -.1960335
         kidsge6 |   .0130122    .013196     0.99   0.324    -.0128935    .0389179
           _cons |   .5855192    .154178     3.80   0.000     .2828442    .8881943
    ------------------------------------------------------------------------------

The estimated equation is

.. math::

   \widehat{inlf} &= \underset{{(.154)}}{.586}-\underset{{(.0014)}}{.0034}nwifeinc+\underset{{(.007)}}{.038}educ+\underset{{(.006)}}{.039}\text{\textit{exper}}-\underset{{(.00018)}}{.00060}\text{\textit{exper}}^{2} \\
   & -\underset{{(.002)}}{.016}age-\underset{{(.034)}}{.262}kidslt6+\underset{{(.013)}}{.013}kidsge6 \\
   n &= 753,\text{ }R^{2}=.264
These are the usual OLS :math:`t` statistics, even though they are not quite valid due to
heteroskedasticity. So, we should use heteroskedasticity-robust standard errors in general.

The intercept is not of interest here. The coefficient on :math:`nwifeinc` (other sources of income)
shows a modest effect: if it increases by 20 :math:`(\$20,000`, about one standard deviation), the
probability of being in the labor force falls by :math:`.068`, or 6.8 percentage points. The
:math:`t` statistic shows it is statistically significant at the 2% level.

Each year of education increases the probability of LFP by an estimated :math:`.038`, or 3.8
percentage points.

Past workforce experience has a positive but diminishing effect. The effect of the first year is
about :math:`.039`, and this diminishes to zero at *exper* :math:`=.039/(2\cdot .0006)=32.5`. (Only
13 women have experience 32.)

Having young children has a very large negative effect: being in the labor force falls by
:math:`.262` for each young child. Almost all of the action is between 0 and 1, with a little from 1
to 2.

It is unwise to extrapolate to extreme values when using any linear model, including this one.

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


Shortcomings of the LPM
-------------------------

Using a linear model for a binary outcome is
convenient because estimation is easy and so is interpretation.

But the LPM does have some shortcomings:

1.  The fitted values from an OLS regression are never guaranteed to be between zero and one, yet these
    fitted values are estimated probabilities.

    This can be slightly embarassing but is rarely a big deal. We usually use the LPM to estimate
    partial effects, not to make predictions. Further, a natural predictor (see below) is not sensitive
    to negative fitted values or fitted values above one.

    ::

        . qui reg inlf nwifeinc educ exper expersq age kidslt6 kidsge6
     
        . predict inlfh
        (option xb assumed; fitted values)
     
        . sum inlfh
     
            Variable |       Obs        Mean    Std. Dev.       Min        Max
        -------------+--------------------------------------------------------
               inlfh |       753    .5683931    .2547633  -.3451103   1.127151
     
        . count if inlfh < 0
           16
     
        . count if inlfh > 1
           17

    Only 33 of 753 fitted values are not in :math:`[0,1]`.

2.  The estimated partial effects are constant throughout the range of the explanatory variables,
    possibly leading to silly estimated effects for large changes. (This is related to predicted
    probabilities possibly being negative or greater than one.)

    This is more of a problem because we know that, say, a variable with a positive effect on
    :math:`P(y=1|\mathbf{x})` must eventually have a diminishing effect. But the linear model implies a
    constant effect (when the variable appears by itself).

    For example, take a woman who has no other source of income, 25 years of prior work experience, no
    children, who is 48 years old. As a function of :math:`educ` the equation looks like

    .. math:: \widehat{inlf}=.417+.038\text{ }educ

    At :math:`educ=12`, the predicted probability is :math:`.873`, at :math:`educ=14` it is
    :math:`.949`, and at :math:`educ=16`, :math:`\widehat{inlf}=1.025`. For the estimated model to truly
    represent a probability, the effect of education should be diminishing – that is, the next year of
    education should increase the probability by less than the previous year so that the estimated
    probability never goes above one.

    Using logarithms does not bound the effect, and using quadratics often does not help. (In this
    example, a quadratic in :math:`educ` gives an estimated *increasing* effect, not a diminishing
    effect.)

    But the LPM does a good job of approximating partial effects if we do not look at extreme values of
    the explanatory variables.

3.  Because :math:`y` is binary – and this really has nothing to do with the LPM per se – the LPM must
    exhibit heteroskedasticity except in the one case where no :math:`x_{j}` affects
    :math:`P(y=1|\mathbf{x})`.  This follows because for a binary variable,

    .. math:: Var(y|\mathbf{x})=p(\mathbf{x})[1-p(\mathbf{x})]

    where :math:`p(\mathbf{x})=\beta _{0}+\beta _{1}x_{1}+\beta _{2}x_{2}+...+\beta _{k}x_{k}` is the
    linear response probability.

    This is a case where we *know* MLR.5 must fail, and we know how. So, currently, we treat the usual
    :math:`t` and :math:`F` tests with suspicion, and the confidence intervals. (Turns out they are
    often pretty reliable in LPMs.)


Goodness of Fit in LPMs
-------------------------

The :math:`R`-squared and adjusted :math:`R`-squared can be used for goodness of fit, but other
quantities are used, too.

The *percent correctly predicted* is based on a prediction exercise, where we predict a 0 or 1
outcome depending on the estimated probabilities.

Let :math:`\hat{y}_{i}` be the OLS fitted values. Define a predictor of :math:`y_{i}` as

.. math::

   \begin{aligned} \tilde{y}_{i} &=&1\text{ if }\hat{y}_{i}\geq .5 \\ &=&0\text{ if
   }\hat{y}_{i}<.5\end{aligned}

Like :math:`y_{i}` (and unlike :math:`\hat{y}_{i}`), :math:`\tilde{y}_{i}` is a binary variable.

::

    . list inlfh inlft inlf in 421/435
     
         +-------------------------+
         |    inlfh   inlft   inlf |
         |-------------------------|
    421. | .6425118       1      1 |
    422. | .2949755       0      1 |
    423. | .5775109       1      1 |
    424. |   .46589       0      1 |
    425. | .9001129       1      1 |
         |-------------------------|
    426. | .9201074       1      1 |
    427. | .8777708       1      1 |
    428. | .7644956       1      1 |
    429. | .2710314       0      0 |
    430. | .2892911       0      0 |
         |-------------------------|
    431. | .6073301       1      0 |
    432. | .3504052       0      0 |
    433. | .6445364       1      0 |
    434. | .4887369       0      0 |
    435. | .6182265       1      0 |
         +-------------------------+

The first 8 women listed were in the labor force, and we correctly predict this for 6 of 8. The last
7 were out of the labor force, and our model predicts this correctly for 4 out of 7.

We can get the percent correctly predicted for each outcome, and then the overall percent correctly
predicted, but tabulating :math:`inlf` and :math:`\widetilde{inlf}`.

::

     
    . tab inlf inlft
     
      =1 if in |
     lab frce, |         inlft
          1975 |         0          1 |     Total
    -----------+----------------------+----------
             0 |       203        122 |       325 
             1 |        78        350 |       428 
    -----------+----------------------+----------
         Total |       281        472 |       753 

Some people call this the confusion matrix.

Of the 428 women in the labor force, we predict 350 correctly, or about 81.8%. For the 325 not in
the labor force, we predict 203 correctly, or about 62.5%.

The overall proportion correctly predicted is :math:`(203+350)/753\approx .734`, or 73.4%. This is a
weighted average of the other two:

.. math::

   \left( \frac{325}{753}\right) 62.5+\left( \frac{428}{753}\right) 81.8\approx
   73.5

(difference due to rounding error).

Should not dwell too much on goodness of fit.  Because the most common outcome gets the most weight,
the overall percent correctly predicted can be high even when we do a terrible job predicting the
less likely outcome.

Further, we do not need high goodness of fit for an estimated equation to be useful.

.. admonition:: Example: Effects of Job Training on Unemployment Status

    The data in JTRAIN2.DTA are from a job training experiment, where training status was randomly
    assigned during 1976. The outcome is :math:`unem78`, a dummy variable equal to one if the man was
    unemployed during all of 1978. The variable :math:`train` is the binary policy variable.

    Because :math:`train\,`\ was randomly assigned, we do not need to control for other factors. But we
    might want to for predictive purposes.

::

    . des train unem78 unem74 unem75 re74 re75 black
     
                  storage  display     value
    variable name   type   format      label      variable label
    -------------------------------------------------------------------------------------------------------------------------
    train           byte   %9.0g                  =1 if assigned to job training
    unem78          byte   %9.0g                  =1 if unem. all of 1978
    unem74          byte   %9.0g                  =1 if unem. all of 1974
    unem75          byte   %9.0g                  =1 if unem. all of 1975
    re74            float  %9.0g                  real earns., 1974, $1000s
    re75            float  %9.0g                  real earns., 1975, $1000s
    black           byte   %9.0g                  =1 if black
     
    . sum unem78 train
     
        Variable |       Obs        Mean    Std. Dev.       Min        Max
    -------------+--------------------------------------------------------
          unem78 |       427    .3138173    .4645874          0          1
           train |       427    .4192037    .4940076          0          1

The unemployment rate is very high for these men: 31.4%. About 42% were assigned to job training.

::

     
    . reg unem78 train
     
          Source |       SS       df       MS              Number of obs =     427
    -------------+------------------------------           F(  1,   425) =    5.62
           Model |  1.20084304     1  1.20084304           Prob > F      =  0.0182
        Residual |  90.7476347   425  .213523846           R-squared     =  0.0131
    -------------+------------------------------           Adj R-squared =  0.0107
           Total |  91.9484778   426  .215841497           Root MSE      =  .46209
     
    ------------------------------------------------------------------------------
          unem78 |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
           train |  -.1074743   .0453195    -2.37   0.018    -.1965525   -.0183961
           _cons |    .358871   .0293425    12.23   0.000     .3011964    .4165455
    ------------------------------------------------------------------------------

So the training program is estimated to reduce the probability of unemployment by :math:`.107`. The
estimated unemployment probability without traing is about :math:`.359`.

Adding a bunch of controls changes the estimate very little – as should be the case because
:math:`train` is (roughly) uncorrelated with the controls.

Only the binary variable *black* is statistically significant (and it is a large effect).

::

    . reg unem78 train unem74 unem75 re74 re75 black hisp married age educ
     
          Source |       SS       df       MS              Number of obs =     427
    -------------+------------------------------           F( 10,   416) =    1.95
           Model |  4.11459586    10  .411459586           Prob > F      =  0.0375
        Residual |  87.8338819   416  .211139139           R-squared     =  0.0447
    -------------+------------------------------           Adj R-squared =  0.0218
           Total |  91.9484778   426  .215841497           Root MSE      =   .4595
     
    ------------------------------------------------------------------------------
          unem78 |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
           train |  -.1086415   .0456392    -2.38   0.018    -.1983537   -.0189292
          unem74 |   .0510057   .0948163     0.54   0.591     -.135373    .2373845
          unem75 |   .0043137   .0781801     0.06   0.956    -.1493635    .1579909
            re74 |    .009118   .0117062     0.78   0.436    -.0138927    .0321288
            re75 |  -.0090693   .0166488    -0.54   0.586    -.0417956     .023657
           black |   .1807529   .0846892     2.13   0.033     .0142808     .347225
            hisp |   -.042662   .1131848    -0.38   0.706    -.2651473    .1798234
         married |   -.000157   .0639208    -0.00   0.998     -.125805    .1254911
             age |   .0007414   .0032595     0.23   0.820    -.0056658    .0071486
            educ |  -.0020509   .0127623    -0.16   0.872    -.0271375    .0230358
           _cons |   .1699241   .1941694     0.88   0.382    -.2117513    .5515995
    ------------------------------------------------------------------------------

::

    . predict unem78h
    (option xb assumed; fitted values)
     
    . sum unem78h
     
        Variable |       Obs        Mean    Std. Dev.       Min        Max
    -------------+--------------------------------------------------------
         unem78h |       427    .3138173    .0982786  -.0493371    .496942

The maximum predicted probability is :math:`.497`, so we *never* predict that someone is unemployed.
This is a case where we get every prediction for :math:`unem78=1` wrong. And yet we know
:math:`-.109` is a good estimate of the effect of job training.  We might like to predict better,
but it is unimportant for the policy analysis.

::

    . tab unem78 unem78t
     
         =1 if |
     unem. all |  unem78t
       of 1978 |         0 |     Total
    -----------+-----------+----------
             0 |       293 |       293 
             1 |       134 |       134 
    -----------+-----------+----------
         Total |       427 |       427 
     
     
    . di 293/427
    .68618267
     

We still get 68.6% of the predictions correct because all of the :math:`unem78=0` cases are
correctly predicted.

This example demonstrates the arbitrariness of the rule that we set :math:`\tilde{y}_{i}=1` when
:math:`\hat{y}_{i}\geq .5`. Some prefer to use the rule

.. math:: \tilde{y}_{i}=1\text{ when }\hat{y}_{i}\geq \bar{y}

where :math:`\bar{y}` is the fraction of ones in the sample. In the job training example,
:math:`\bar{y}=.314`. We will get more predictions correct when :math:`unem78=1`, but make more
mistakes when :math:`unem78=0`.

Now our proportion corrected predicted is only :math:`163/293=.556` of the :math:`unem78=0` cases
but we do much better with the :math:`unem78=1` case: :math:`83/134=.619`. Overall, we are doing
worse: :math:`(163+83)/427=.576`.

::

     
     drop unem78t
     
    . gen unem78t = unem78h >= .314
     
    . tab unem78 unem78t
     
         =1 if |
     unem. all |        unem78t
       of 1978 |         0          1 |     Total
    -----------+----------------------+----------
             0 |       163        130 |       293 
             1 |        51         83 |       134 
    -----------+----------------------+----------
         Total |       214        213 |       427 
     

Logit and Probit
===================

A general model that ensures probabilities are between zero and one has the form

.. math::

   P(y=1|\mathbf{x})=G(\beta _{0}+\beta _{1}x_{1}+\beta _{2}x_{2}+...+\beta _{k}x_{k})

for some function :math:`G` that takes values between zero and one.

In most cases, :math:`G(\cdot )` is actually a cumulative distribution function (cdf) for a
continuous random variable with probability density function :math:`g(\cdot )`. Then, :math:`G(\cdot
)` is strictly increasing, and the estimates are easier to interpret.

The leading cases are

.. math::

   \begin{aligned} G(z) &=&\Lambda (z)=\frac{\exp (z)}{[1+\exp (z)]}\text{ \ \ (logit)} \\ G(z)
   &=&\Phi (z)=\int_{-\infty }^{z}\frac{1}{\sqrt{2\pi }}\exp (-u^{2}/2)du\text{ \ \
   (probit)}\end{aligned}

These functions have similar shapes but the logistic is more spread out.

::

    . range z -4 4 1000
    obs was 0, now 1000
     
    . gen PHIz = normal(z)
     
    . gen LAMz = exp(z)/(1 + exp(z))
     
    . twoway (line PHIz z) (line LAMz z)
     

The estimation method for the :math:`\beta _{j}` is maximum likelihood. Computationally
straightforward, and runs quickly. 

::

    probit y x1 x2 ... xk

    logit y x1 x2 ... xk

(Asymptotic) standard errors are reported, as are :math:`t` statistics and confidence intervals.

We can test multiple exclusion restrictions using a ``test`` command, just as with linear
regression.

Estimating Partial Effects
----------------------------

What do we do with the estimates? Let :math:`x_{j}` be continuous. Then the partial effect is

.. math:: \frac{\partial p(\mathbf{x)}}{\partial x_{j}}=\beta _{j}g(\mathbf{x\beta })

where

.. math::

   \begin{aligned}
   g(z) &=&\phi (z)=\frac{1}{\sqrt{2\pi }}\exp (-z^{2}/2)\text{ for probit} \\
   g(z) &=&\exp (z)/[1+\exp (z)]^{2}\text{ for logit}\end{aligned}

and, because :math:`g(z)>0`, :math:`\beta _{j}` gives the direction of the partial effect. But its
magnitude depends on :math:`g(\mathbf{x\beta }).`

For probit, the largest value of the scale factor is about :math:`.4=g(0)`. For logit, it is
:math:`g(0)=.25`.

The :math:`g(\cdot )` functions both have a bell shape and are maximized at zero. As
:math:`z\rightarrow \infty `, :math:`g(z)\rightarrow 0`, which ensures that the partial effects head
to zero for any :math:`x_{j}` with :math:`\beta _{j}>0`.

For two continous covariates, the ratio of the coefficients give the ratio of the partial effects,
independent of :math:`\mathbf{x}`.

.. math::

   \frac{\partial p(\mathbf{x})/\partial x_{j}}{\partial p(\mathbf{x})/\partial x_{h}}=\frac{\beta
   _{j}g(\mathbf{x\beta })}{\beta _{h}g(\mathbf{x\beta })}=\beta _{j}/\beta _{h}\text{.}

No simple relationship exists for discrete variables or changes.

In any case, we would like the magnitude of the effect.

Two common summary measures are the estimated PEAs and APEs. The estimated PEA for a continuous
variable is

.. math:: \widehat{PEA}_{j}=\hat{\beta}_{j}g(\mathbf{\bar{x}\hat{\beta}})

As discussed earlier, putting in averages for discrete covariates might not be especially
interesting.

When :math:`\mathbf{x}` includes nonlinear functions, such as :math:`age^{2}`, probably makes more
sense to use :math:`(\overline{age})^{2}` rather than average :math:`age_{i}^{2}`.

A standard error for :math:`\widehat{PEA}_{j}` is obtained using Stata and the ``mfx`` command. Even
newer is the ``margeff`` command.

The APE has more appeal, as we are averaging partial effects for actual units:

.. math:: \widehat{APE}_{j}=\hat{\beta}_{j}\left[
   N^{-1}\sum_{i=1}^{N}g(\mathbf{x}_{i}\mathbf{\hat{\beta}})\right]

To use the delta method, must account for randomness in :math:`\mathbf{x}_{i}`, too. Bootstrap makes
that easy.

Whether we use the PEA or APE, the scale factor multiplying :math:`\hat{\beta}_{j}` is below one,
and sometimes well below one.

It makes no sense to compare magnitudes of coefficients across probit, logit, LPM. Comparing APEs is
preferred.

In particular, if :math:`\hat{\gamma}_{j}` is the linear regression coefficient on :math:`x_{j}`
from estimating an LPM, it can be compared with :math:`\widehat{APE}_{j}` (provided no other
function of :math:`x_{j}` appears in the regressors).

Suppose :math:`x_{K}` is a binary variable.  Then its APE is estimated as

.. math::
   \widehat{APE}_{K}=N^{-1}\sum_{i=1}^{N}[G(\mathbf{x}_{i(K)}\mathbf{\hat{\beta}}_{(K)}+\hat{\beta}_{K})-G(\mathbf{x}_{i(K)}\mathbf{\hat{\beta}}_{(K)})],

where :math:`\mathbf{x}_{i(K)}` is :math:`\mathbf{x}_{i}` but without :math:`x_{iK}`.

The APE has a nice counterfactual interpretation that is especially useful in policy analysis.
Called the *average treatment effect (ATE)* in the treatment effect literature with a binary
outcome. (The treatment, :math:`x_{K}`, is binary.)

Can average the individual treatment effects across subgroups, too, or insert fixed values for some
of the other covariates.

Stata, with its ``margeff`` (marginal effects) command can report at PEA or APE. For a discrete
:math:`x_{K}`, the estimated PEA is

.. math::

   \widehat{PEA}_{K}=G(\mathbf{\bar{x}}_{(K)}\mathbf{\hat{\beta}}_{(K)}+\hat{\beta}_{K})-G(\mathbf{\bar{x}}_{(K)}\mathbf{\hat{\beta}}_{(K)})

Again, this might correspond to a weird population unit, or might not be representative of the
population.

To obtain standard errors of APEs and PEAs, we can use the delta method or bootstrap.

Stata reports proper standard errors using the ``margeff`` command.

::

    . probit inlf nwifeinc educ exper expersq age kidslt6 kidsge6
     
    Probit regression                                 Number of obs   =        753
                                                      LR chi2(7)      =     227.14
                                                      Prob > chi2     =     0.0000
    Log likelihood = -401.30219                       Pseudo R2       =     0.2206
     
    ------------------------------------------------------------------------------
            inlf |      Coef.   Std. Err.      z    P>|z|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
        nwifeinc |  -.0120237   .0048398    -2.48   0.013    -.0215096   -.0025378
            educ |   .1309047   .0252542     5.18   0.000     .0814074     .180402
           exper |   .1233476   .0187164     6.59   0.000     .0866641    .1600311
         expersq |  -.0018871      .0006    -3.15   0.002     -.003063   -.0007111
             age |  -.0528527   .0084772    -6.23   0.000    -.0694678   -.0362376
         kidslt6 |  -.8683285   .1185223    -7.33   0.000    -1.100628    -.636029
         kidsge6 |    .036005   .0434768     0.83   0.408     -.049208    .1212179
           _cons |   .2700768    .508593     0.53   0.595    -.7267473    1.266901
    ------------------------------------------------------------------------------

 

::

    . * Compute partial effects at the averages (not meaningful for 
    . * exper and expersq):
     
    . mfx
     
    Marginal effects after probit
          y  = Pr(inlf) (predict)
             =  .58154201
    ------------------------------------------------------------------------------
    variable |      dy/dx    Std. Err.     z    P>|z|  [    95% C.I.   ]      X
    ---------+--------------------------------------------------------------------
    nwifeinc |  -.0046962      .00189   -2.48   0.013  -.008401 -.000991    20.129
        educ |   .0511287      .00986    5.19   0.000   .031805  .070452   12.2869
       exper |   .0481771      .00733    6.57   0.000   .033815  .062539   10.6308
     expersq |  -.0007371      .00023   -3.14   0.002  -.001197 -.000277   178.039
         age |  -.0206432      .00331   -6.24   0.000  -.027127  -.01416   42.5378
     kidslt6 |  -.3391514      .04636   -7.32   0.000  -.430012 -.248291   .237716
     kidsge6 |   .0140628      .01699    0.83   0.408  -.019228  .047353   1.35325
    ------------------------------------------------------------------------------

::

    . * Now the APEs. Not meaningful for the experience variables.
     
    . margeff
     
    Average partial effects after probit
          y  = Pr(inlf) 
     
    ------------------------------------------------------------------------------
        variable |      Coef.   Std. Err.      z    P>|z|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
        nwifeinc |  -.0036162   .0014414    -2.51   0.012    -.0064413   -.0007911
            educ |   .0393088   .0071877     5.47   0.000     .0252212    .0533964
           exper |    .037046    .005131     7.22   0.000     .0269893    .0471026
         expersq |  -.0005675   .0001771    -3.20   0.001    -.0009146   -.0002204
             age |  -.0158917   .0023569    -6.74   0.000     -.020511   -.0112723
         kidslt6 |  -.2441788   .0258995    -9.43   0.000    -.2949409   -.1934167
         kidsge6 |   .0108274   .0130538     0.83   0.407    -.0147576    .0364124
    ------------------------------------------------------------------------------

::

    . logit inlf nwifeinc educ exper expersq age kidslt6 kidsge6
     
    Logistic regression                               Number of obs   =        753
                                                      LR chi2(7)      =     226.22
                                                      Prob > chi2     =     0.0000
    Log likelihood = -401.76515                       Pseudo R2       =     0.2197
     
    ------------------------------------------------------------------------------
            inlf |      Coef.   Std. Err.      z    P>|z|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
        nwifeinc |  -.0213452   .0084214    -2.53   0.011    -.0378509   -.0048394
            educ |   .2211704   .0434396     5.09   0.000     .1360303    .3063105
           exper |   .2058695   .0320569     6.42   0.000     .1430391    .2686999
         expersq |  -.0031541   .0010161    -3.10   0.002    -.0051456   -.0011626
             age |  -.0880244    .014573    -6.04   0.000     -.116587   -.0594618
         kidslt6 |  -1.443354   .2035849    -7.09   0.000    -1.842373   -1.044335
         kidsge6 |   .0601122   .0747897     0.80   0.422     -.086473    .2066974
           _cons |   .4254524   .8603696     0.49   0.621    -1.260841    2.111746
    ------------------------------------------------------------------------------

::

    . margeff
     
    Average partial effects after logit
          y  = Pr(inlf) 
     
    ------------------------------------------------------------------------------
        variable |      Coef.   Std. Err.      z    P>|z|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
        nwifeinc |  -.0038118   .0014824    -2.57   0.010    -.0067172   -.0009064
            educ |   .0394323   .0072593     5.43   0.000     .0252044    .0536602
           exper |   .0367123   .0051289     7.16   0.000     .0266598    .0467648
         expersq |  -.0005633   .0001774    -3.18   0.001    -.0009109   -.0002156
             age |  -.0157153   .0023789    -6.61   0.000    -.0203779   -.0110527
         kidslt6 |   -.240805   .0259425    -9.28   0.000    -.2916515   -.1899585
         kidsge6 |   .0107335   .0133282     0.81   0.421    -.0153893    .0368564
    ------------------------------------------------------------------------------

Reporting the Results
========================

Start with an LPM and report the coefficients along with robust standard errors.

With logit and probit, one typically reports the estimated coefficients along with standard errors.
The value of the log likelihood is sometimes reported, too (as it can be used to compute tests of
exclusion restrictions).

It is important to report APEs and their standard errors for logit and probit (maybe even in place
of coefficients). Sometimes a key policy variable is of interest, and it is important to estimate
the magnitude of the effect.

If the variable of interest is continuous, or at least takes on lots of values, might compute the
partial effects at different values (and average out the other explanatory variables).

For example, in estimating the effects of young children on married women’s labor force
participation, we can estimate the partial effect in going from 0 to 1 and 1 to 2 children.

Goodness-of-fit is not as important, but the percent correctly predicted for each outcome, and
overall, is of some interest.

.. admonition:: Example: Effects of Job Training on Unemployment with Nonexperimental Data (JTRAIN3.DTA)

    These are nonexperimental data in the sense that men can self-select into job training.
    Consequently, to estimate the causal effect of the program on unemployment status, we should control
    for other factors, including unemployment status in the early years and demographics.

::

    . use jtrain3
     
    . tab train
     
       =1 if in |
            job |
       training |      Freq.     Percent        Cum.
    ------------+-----------------------------------
              0 |        586       76.60       76.60
              1 |        179       23.40      100.00
    ------------+-----------------------------------
          Total |        765      100.00
     
    . reg unem78 train, robust
     
    Linear regression                                      Number of obs =     765
                                                           F(  1,   763) =    5.62
                                                           Prob > F      =  0.0180
                                                           R-squared     =  0.0067
                                                           Root MSE      =  .46563
     
    ------------------------------------------------------------------------------
                 |               Robust
          unem78 |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
           train |  -.0899003   .0379312    -2.37   0.018    -.1643622   -.0154383
           _cons |   .3412969   .0196124    17.40   0.000     .3027963    .3797976
    ------------------------------------------------------------------------------

::

    . probit unem78 train
     
    Probit regression                                 Number of obs   =        765
                                                      LR chi2(1)      =       5.25
                                                      Prob > chi2     =     0.0219
    Log likelihood = -477.08041                       Pseudo R2       =     0.0055
     
    ------------------------------------------------------------------------------
          unem78 |      Coef.   Std. Err.      z    P>|z|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
           train |   -.261175   .1148891    -2.27   0.023    -.4863535   -.0359966
           _cons |  -.4089262   .0533782    -7.66   0.000    -.5135454   -.3043069
    ------------------------------------------------------------------------------
     
    . margeff
     
    Average marginal effects on Prob(unem78==1) after probit
     
    ------------------------------------------------------------------------------
          unem78 |      Coef.   Std. Err.      z    P>|z|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
           train |  -.0899003   .0378816    -2.37   0.018    -.1641469   -.0156537
    ------------------------------------------------------------------------------

::

    . logit unem78 train
     
    Logistic regression                               Number of obs   =        765
                                                      LR chi2(1)      =       5.25
                                                      Prob > chi2     =     0.0219
    Log likelihood = -477.08041                       Pseudo R2       =     0.0055
     
    ------------------------------------------------------------------------------
          unem78 |      Coef.   Std. Err.      z    P>|z|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
           train |  -.4336573   .1930689    -2.25   0.025    -.8120653   -.0552493
           _cons |    -.65752   .0871245    -7.55   0.000    -.8282808   -.4867592
    ------------------------------------------------------------------------------
     
    . margeff
     
    Average marginal effects on Prob(unem78==1) after logit
     
    ------------------------------------------------------------------------------
          unem78 |      Coef.   Std. Err.      z    P>|z|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
           train |  -.0899003   .0378816    -2.37   0.018    -.1641469   -.0156537
    ------------------------------------------------------------------------------
     
    . * With a single binary explanatory variable, the APEs are identical across
    . * all methods, even though coefficients are very different!

::

    . logit unem78 train unem74 unem75 age educ black hisp married
     
    Logistic regression                               Number of obs   =        765
                                                      LR chi2(8)      =     246.49
                                                      Prob > chi2     =     0.0000
    Log likelihood =  -356.4631                       Pseudo R2       =     0.2569
     
    ------------------------------------------------------------------------------
          unem78 |      Coef.   Std. Err.      z    P>|z|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
           train |  -1.356781   .3361003    -4.04   0.000    -2.015526   -.6980365
          unem74 |   1.349475   .2528146     5.34   0.000      .853967    1.844982
          unem75 |   1.294705   .2342518     5.53   0.000     .8355803    1.753831
             age |   .0482808    .010451     4.62   0.000     .0277973    .0687643
            educ |   .0076614   .0346233     0.22   0.825    -.0601991    .0755218
           black |  -.0552568   .2420343    -0.23   0.819    -.5296354    .4191218
            hisp |  -.9161793   .5100568    -1.80   0.072    -1.915872    .0835136
         married |  -.6571078   .2524022    -2.60   0.009    -1.151807   -.1624086
           _cons |  -3.025995    .650053    -4.65   0.000    -4.300076   -1.751915
    ------------------------------------------------------------------------------

::

    . margeff
     
    Average marginal effects on Prob(unem78==1) after logit
     
    ------------------------------------------------------------------------------
          unem78 |      Coef.   Std. Err.      z    P>|z|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
           train |  -.1891976   .0406958    -4.65   0.000    -.2689599   -.1094353
          unem74 |   .2346285   .0468234     5.01   0.000     .1428564    .3264006
          unem75 |   .2243058   .0431044     5.20   0.000     .1398227    .3087888
             age |   .0072676   .0015025     4.84   0.000     .0043228    .0102125
            educ |   .0011533   .0052108     0.22   0.825    -.0090597    .0113662
           black |  -.0083453   .0366672    -0.23   0.820    -.0802118    .0635211
            hisp |  -.1240361   .0605661    -2.05   0.041    -.2427435   -.0053287
         married |  -.0974535   .0363494    -2.68   0.007     -.168697     -.02621
    ------------------------------------------------------------------------------
     
    . * Effect of job training program now, on average, is that it reduces the 
    . * probability of unemployement by about .19 --- a pretty large effect. This is
    . * more than double the estimate without the controls (about .09).
     
    . test educ black hisp
     
     ( 1)  [unem78]educ = 0
     ( 2)  [unem78]black = 0
     ( 3)  [unem78]hisp = 0
     
               chi2(  3) =    3.42
             Prob > chi2 =    0.3319
     

