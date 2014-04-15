Panel Data Methods
***********************

What is a Panel Data Set?
============================

With a panel data set, the same units are sampled in two or more time periods. For each unit
(individual, school, city, and so on) :math:`i` we have multiple years of data.

At a minimum, statistical methods must recognize that the outcomes for a unit will be correlated
over time.

By contrast, with pooled cross sections we have different units sampled in each period. If there is
some overlap, we ignore it (and usually would not know there is overlap).

Main benefit of panel data: with multiple years of data we can control for unobserved
characteristics that do not change (or change slowly) over time. Very useful for policy analysis.

A *balanced panel* is one where we observe the same time periods for each unit. Easier to achieve
for larger units (such as schools and cities). At a disaggregated level – such as individuals and
families – following the same units over time can be challenging. (Attrition can be a serious
problem.)

Notation here assumes a balanced panel.  Econometric methods extend to unbalanced panels, and
software takes care of algebraic details. But one should ask: Why are some periods missing for some
units? (For example, is reporting achievement scores by schools optional or not enforced?)

The notation we use is the following. For each cross-sectional unit :math:`i` at time :math:`t` the
response variable is :math:`y_{it}`. An explanatory variable is :math:`x_{it}`. With more than one
explanatory variable we have :math:`x_{it1}`, :math:`x_{it2},` ..., :math:`x_{itk}`.

We will start with the case of two periods, so :math:`t=1`, :math:`2` and (hopefully) many cross
section observations, :math:`i=1,2,...,n`.

Along with the observed data :math:`(x_{it1},x_{it2},...,x_{itk},y_{it})` we draw unobserved
factors.  Put these into two categories. (1) A component that does not change over time,
:math:`a_{i}`. Called an *unobserved effect* or *unobserved heterogeneity*. It varies by individual
but not by time.

At the invididual level, can think of :math:`a_{i}` as ability – something innate and not subject to
change.  Generally, :math:`a_{i}\,`\ contains unobserved attributes.

There are also unobservables that change across time, :math:`u_{it}`. These are sometimes called
shocks; we will call them *idiosyncratic errors*. They are specific to unit :math:`i` but vary over
time, and they affect the outcome, :math:`y_{it}`.

Formats for Storing Panel Data
=================================

The best way to store panel data is to stack the time periods for each :math:`i` on top of each
other. In particular, the time periods for each unit should be adjacent, and stored in chronological
order (from earliest period to the most recent). This is sometimes called the long storage format.
It is by far the most common.

::

    . use gpa3
     
    . des id term trmgpa sat season
     
                  storage  display     value
    variable name   type   format      label      variable label
    -------------------------------------------------------------------------------------------------------------------------------
    id              float  %9.0g                  student identifier
    term            int    %4.0f                  fall = 1, spring = 2
    trmgpa          float  %9.0g                  term GPA
    sat             int    %4.0f                  SAT score
    season          byte   %8.0g                  =1 if in season
     
    . list id term trmgpa sat season in 1/10
     
         +-------------------------------------+
         |  id   term   trmgpa    sat   season |
         |-------------------------------------|
      1. |  22      1      1.5    920        0 |
      2. |  22      2     2.25    920        1 |
      3. |  35      1      2.2    780        0 |
      4. |  35      2      1.6    780        1 |
      5. |  36      1      1.6    810        0 |
         |-------------------------------------|
      6. |  36      2     1.29    810        1 |
      7. | 156      1        2   1080        1 |
      8. | 156      2     2.73   1080        0 |
      9. | 246      1      2.8    960        1 |
     10. | 246      2      2.6    960        0 |
         +-------------------------------------+

While not absolutely necessary for some procedures, it is best to tell Stata that you have a panel
data set. In particular, what are :math:`i` and :math:`t`? In GPA3.DTA, :math:`i=id` and
:math:`t=term`.

::

     
     
    . xtset id term
           panel variable:  id (strongly balanced)
            time variable:  term, 1 to 2
                    delta:  1 unit
     
    . tab term
     
      fall = 1, |
     spring = 2 |      Freq.     Percent        Cum.
    ------------+-----------------------------------
              1 |        366       50.00       50.00
              2 |        366       50.00      100.00
    ------------+-----------------------------------
          Total |        732      100.00

The same data structure is convenient for more than two years.

::

     
    . use mathpnl
     
    . xtset distid year
           panel variable:  distid (strongly balanced)
            time variable:  year, 1992 to 1998
                    delta:  1 unit
     
    . des distid year math4 expp found lunch
     
                  storage  display     value
    variable name   type   format      label      variable label
    -------------------------------------------------------------------------------------------------------------------------------
    distid          float  %9.0g                  district identifier
    year            int    %9.0g                  1992-1998
    math4           float  %9.0g                  % satisfactory, 4th grade math
    expp            int    %9.0g                  expenditure per pupil
    found           int    %9.0g                  foundation grant, $:  1995-98
    lunch           float  %9.0g                  % eligible for free lunch
     

::

    . list distid year math4 expp found lunch in 1/21
     
         +-----------------------------------------------+
         | distid   year   math4    expp   found   lunch |
         |-----------------------------------------------|
      1. |   1010   1992    28.8    4227       .    36.3 |
      2. |   1010   1993    32.3    4809       .    39.2 |
      3. |   1010   1994    39.1    5214       .    38.6 |
      4. |   1010   1995      68    6019    5245   37.41 |
      5. |   1010   1996    68.4    6155    5398   40.79 |
         |-----------------------------------------------|
      6. |   1010   1997      49    6134    5553   43.84 |
      7. |   1010   1998      75    6476    5707   42.61 |
      8. |   2010   1992      30    3445       .    36.4 |
      9. |   2010   1993    28.6    3446       .    38.6 |
     10. |   2010   1994    37.5    3583       .    52.6 |
         |-----------------------------------------------|
     11. |   2010   1995      40    8525    5581   51.52 |
     12. |   2010   1996    83.3    8632    5734    47.3 |
     13. |   2010   1997      90    8854    5889   52.24 |
     14. |   2010   1998      67    9424    6043    56.6 |
     15. |   2020   1992      20    6379       .    59.3 |
         |-----------------------------------------------|
     16. |   2020   1993    37.5    7600       .    59.2 |
     17. |   2020   1994     100    7412       .      50 |
     18. |   2020   1995    57.1    8041    8588   51.22 |
     19. |   2020   1996      50    9102    8741    62.5 |
     20. |   2020   1997      40    9942    8896   48.81 |
         |-----------------------------------------------|
     21. |   2020   1998      50   10539    9050   56.41 |
         +-----------------------------------------------+

Initially using ``xtset`` heads off most problems, but it is nice to have the data appropriately
sorted. Old versions of Stata had commands that would jumble the data. To get it sorted again, use

``. sort distid year``

You do not want to sort by year and then district ID. (That would make the data set look more like
independently pooled cross sections, and mask the panel structure.)

Sometimes panel data sets (especially with two years) will be stored as having only :math:`n`
records (rather than :math:`2n`, as above), with the variables from the different years given
different suffixes (to distinguish the years). Generally, this makes the data harder to work with,
especially if there are more than two years.  It is sometimes called the wide storage method; the
above is called the long storage method.

State has a command, ``reshape``, that allows one to go from wide to long, and vice versa.

VOTE2.DTA is a two-year panel data set stored in the wide format.

::

    . use vote2
     
    . des state district vote90 vote88 inexp90 chexp90 inexp88 chexp88
     
                  storage  display     value
    variable name   type   format      label      variable label
    -------------------------------------------------------------------------------------------------------------------------------
    state           str2   %9s                    state postal code
    district        byte   %8.0g                  U.S. Congressional district
    vote90          byte   %8.0g                  inc. share two-party vote, 1990
    vote88          byte   %8.0g                  inc. share two-party vote, 1988
    inexp90         float  %9.0g                  inc. camp. expends., 1990
    chexp90         float  %9.0g                  chl. camp. expends., 1990
    inexp88         float  %9.0g                  inc. camp. expends., 1988
    chexp88         float  %9.0g                  chl. camp. expends., 1988
     
    . list state district vote90 vote88 inexp90 chexp90 inexp88 chexp88 in 1/10
     
         +----------------------------------------------------------------------------+
         | state   district   vote90   vote88   inexp90   chexp90   inexp88   chexp88 |
         |----------------------------------------------------------------------------|
      1. |    AL          2       51       94    596096    163663    234923         . |
      2. |    AL          3       74       65    176550     22989    679297    443927 |
      3. |    AL          7       71       68    238446     58952    328296      8737 |
      4. |    AK          1       52       62    564759    164732    626377    402477 |
      5. |    AZ          2       66       73    112373      1445     99607      3065 |
         |----------------------------------------------------------------------------|
      6. |    AZ          3       57       69    225149      9353    319690     26281 |
      7. |    AZ          4       61       87    442366     38851    316476         . |
      8. |    AR          3       71       75    105354     15730    159221     60054 |
      9. |    AR          4       72       69    480853       511    570155     21393 |
     10. |    CA          2       64       59    515020      5951    696748    193915 |
         +----------------------------------------------------------------------------+

Analysis with Two-Periods of Panel Data
==========================================

Assume a balanced panel for units :math:`i`.  The units can be aggregated (schools or cities) or
disaggregated (students or teachers).

We have time periods :math:`t=1` and :math:`t=2` for each unit :math:`i`. These periods do not have
to be, say, adjacent years. They could be periods far apart in time. Or, they could be close
together.

First consider the case with a single explanatory variable, :math:`x_{it}`.

The equation is

.. math:: y_{it}=\beta _{0}+\delta _{0}d2_{t}+\beta _{1}x_{it}+a_{i}+u_{it}\text{, }t=1,2.

We observe :math:`(x_{it},y_{it})` for each of the two time periods. The variable :math:`d2_{t}` is
a constructed time dummy for the second time period: :math:`d2_{t}=1` if :math:`t=2` and
:math:`d2_{t}=0` if :math:`t=1`.

The variable :math:`a_{i}` is the unobserved unit effect (or heterogeneity). :math:`u_{it}` is the
unobserved idiosyncratic error.

We are interested in estimating :math:`\beta _{1}`, the partial effect of :math:`x` on :math:`y`.
Note that the model assumes this effect is constant over time.

.. math:: y_{it}=\beta _{0}+\delta _{0}d2_{t}+\beta _{1}x_{it}+a_{i}+u_{it}\text{, }t=1,2.

The intercept in the first (base) period is :math:`\beta _{0}`, and that for the second period is
:math:`\beta _{0}+\delta _{0}`. It can be very important to allow changing intercepts to get a good
estimate of a causal effect. (For example, a policy, as measured by :math:`x_{it}`, might be
implemented just as the aggregate economy is turning up or down – as captured by :math:`\delta
_{0}d2_{t}`.)

For policy analysis, :math:`x_{it}` is often a dummy variable. Was zip code :math:`i` in year
:math:`t` designated an empowerment zone?

Occasionally :math:`x_{it}` does not change over time for *any* unit. (Generally, we would expect
:math:`x_{it}` to be constant across time for *some* units.) For example, :math:`x_{it}` could be
gender, or years of schooling for people who have completed their schooling. We will be limited in
what we can learn in that case.

How should we estimate the slope :math:`\beta _{1}` (and :math:`\beta _{0}`, :math:`\delta _{0}`
along with it)? One possibility is to just use a pooled OLS analysis.  Effectively, define the
*composite error* as

.. math:: v_{it}=a_{i}+u_{it}\text{, }t=1,2

and write

.. math:: y_{it}=\beta _{0}+\delta _{0}d2_{t}+\beta _{1}x_{it}+v_{it}\text{, }t=1,2.

Applying OLS we obtain the *pooled OLS estimator*. We simply regress :math:`y` on :math:`d2` and
:math:`x`. (Stata need not even know we have a panel data set; it looks like a regression with one
long cross section.)

A few important issues arise with the POLS estimator.

1.  Even if we assume random sampling across :math:`i` – which we do – we cannot reasonably assume the
    observations for :math:`i` across :math:`t=1,2` are independent. In fact,

    .. math::

        \begin{aligned} v_{i1} &=&a_{i}+u_{i1} \\ v_{i2} &=&a_{i}+u_{i2}\end{aligned}

    must be correlated because of the presence of :math:`a_{i}`.

    Correlation of :math:`v_{i1}` and :math:`v_{i2}` causes the usual OLS standard errors to be invalid.
    And using heteroskedasticity-robust standard errors does not solve the problem. This is a problem of
    **serial correlation** or **cluster correlation**. (Each unit :math:`i` is a cluster of two time
    periods.)

    Obtaining cluster-robust standard errors and test statistics is very easy these days.

2.  A more serious issue is that consistency of OLS (as :math:`n` gets large, as usual) requires that
    :math:`x_{it}` and :math:`v_{it}` are uncorrelated. Because :math:`v_{it}=a_{i}+u_{it}`, we need

    .. math::
        
        \begin{aligned} Cov(x_{it},a_{i}) &=&0 \\ Cov(x_{it},u_{it}) &=&0\end{aligned}

    Suppose we are willing to assume the second of these. The first might be violated if :math:`x_{it}`
    is determined based on systematic differences in units. For example, if :math:`y_{it}` is an
    employment rate, EZ designation might depend partly on historical economic conditions of an area,
    captured by :math:`a_{i}`, but not on contemporaneous shocks to employment (in :math:`u_{it}`)\
    :math:`.`

    When :math:`Cov(x_{it},a_{i})\neq 0` it is often said that (pooled) OLS suffers from *heterogeneity
    bias*.

    If the explanatory variable changes over time – at least for some units in the population –
    heterogeneity bias can be solved by differencing away :math:`a_{i}`.

**Differencing the Two Years**

To remove the source of bias in POLS, :math:`a_{i}`, write the time periods in reverse order for any
unit :math:`i`:

.. math::

    \begin{aligned} y_{i2} &=&(\beta _{0}+\delta _{0})+\beta _{1}x_{i2}+a_{i}+u_{i2} \\ y_{i1} &=&\beta
    _{0}+\beta _{1}x_{i1}+a_{i}+u_{i1}\end{aligned}

Subtract time period one from time period two to get

.. math:: y_{i2}-y_{i1}=\delta _{0}+\beta _{1}(x_{i2}-x_{i1})+(u_{i2}-u_{i1})

If we define :math:`\Delta y_{i}=y_{i2}-y_{i1}`, where :math:`\Delta =change` – and similarly for
:math:`\Delta y_{i}` and :math:`\Delta y_{i}` – we can write the cross-sectional equation as

.. math:: \Delta y_{i}=\delta _{0}+\beta _{1}\Delta x_{i}+\Delta u_{i}

or

.. math:: cy_{i}=\delta _{0}+\beta _{1}cx_{i}+cu_{i}

Important: :math:`\beta _{1}` is the original coefficient we are interested in. We have obtained an
estimating equation by taking changes or *differencing*.

Notice that the intercept in the differenced equation,

.. math:: \Delta y_{i}=\delta _{0}+\beta _{1}\Delta x_{i}+\Delta u_{i}

is the *change* in the intercept over the two time periods. It is sometimes interesting to study
this change.

Differencing away the unobserved effect, :math:`a_{i}`, is simple but can be very powerful for
isolating causal effects.

If :math:`\Delta x_{i}=0` for all :math:`i`, or even if :math:`\Delta x_{i}` is the same nonzero
constant, this strategy does not work. We need some variation in :math:`\Delta x_{i}` across
:math:`i`.

The OLS estimator applied to

.. math:: \Delta y_{i}=\delta _{0}+\beta _{1}\Delta x_{i}+\Delta u_{i}

is often called the *first-difference estimator*. (With more than two time periods, other orders of
differencing are possible; hence the qualifier first.) We will refer to the *FD estimator*.

.. admonition:: Example: Effects of City Unemployment Rates on Crime Rates

    CRIME2.DTA: :math:`n=46` cities over the two years 1982 and 1987. No city identifier, so cannot
    cluster the standard errors when using POLS.

    :math:`crmrte` is the number of crimes per 1,000 people. :math:`unem` is the unemployment rate, in
    percent.

    If ``xtset`` has been used can use the difference operator to create the changes, but that requires
    a city identifier. Instead, changes were created as

    ``. gen ccrmrte = crmrte - crmrte[_n-1] if year == 87``

    ``. gen cunem = unem - unem[_n-1] if year == 87``

    Here, ``_n`` means the current observation, so ``_n-1`` is the previous observation.

    Need to include ``if year == 87`` or 46 bogus observations will be created.

::

    . des crmrte unem year
     
                  storage  display     value
    variable name   type   format      label      variable label
    -------------------------------------------------------------------------------------------------------------------------------
    crmrte          float  %9.0g                  crimes per 1000 people
    unem            float  %9.0g                  unemployment rate
    year            byte   %9.0g                  82 or 87
     
    . tab year
     
       82 or 87 |      Freq.     Percent        Cum.
    ------------+-----------------------------------
             82 |         46       50.00       50.00
             87 |         46       50.00      100.00
    ------------+-----------------------------------
          Total |         92      100.00

::

    . * Pooled OLS. Standard errors probably not correct because of correlation
    . * across time.
     
    . reg crmrte d87 unem
     
          Source |       SS       df       MS              Number of obs =      92
    -------------+------------------------------           F(  2,    89) =    0.55
           Model |  989.717223     2  494.858612           Prob > F      =  0.5788
        Residual |  80055.7995    89  899.503365           R-squared     =  0.0122
    -------------+------------------------------           Adj R-squared = -0.0100
           Total |  81045.5167    91  890.610074           Root MSE      =  29.992
     
    ------------------------------------------------------------------------------
          crmrte |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
             d87 |   7.940416   7.975325     1.00   0.322    -7.906385    23.78722
            unem |   .4265473   1.188279     0.36   0.720    -1.934538    2.787633
           _cons |   93.42025   12.73947     7.33   0.000     68.10719    118.7333
    ------------------------------------------------------------------------------

::

    . * Now the first-difference regression:
     
    . des ccrmrte cunem
     
                  storage  display     value
    variable name   type   format      label      variable label
    -------------------------------------------------------------------------------------------------------------------------------
    ccrmrte         float  %9.0g                  change in crmrte
    cunem           float  %9.0g                  change in unem
     
    . reg ccrmrte cunem
     
          Source |       SS       df       MS              Number of obs =      46
    -------------+------------------------------           F(  1,    44) =    6.38
           Model |  2566.43744     1  2566.43744           Prob > F      =  0.0152
        Residual |  17689.5497    44  402.035219           R-squared     =  0.1267
    -------------+------------------------------           Adj R-squared =  0.1069
           Total |  20255.9871    45  450.133047           Root MSE      =  20.051
     
    ------------------------------------------------------------------------------
         ccrmrte |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
           cunem |   2.217999   .8778658     2.53   0.015     .4487771    3.987222
           _cons |    15.4022   4.702117     3.28   0.002      5.92571     24.8787
    ------------------------------------------------------------------------------

::

    . reg ccrmrte cunem, robust
     
    Linear regression                                      Number of obs =      46
                                                           F(  1,    44) =    7.40
                                                           Prob > F      =  0.0093
                                                           R-squared     =  0.1267
                                                           Root MSE      =  20.051
     
    ------------------------------------------------------------------------------
                 |               Robust
         ccrmrte |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
           cunem |   2.217999   .8155056     2.72   0.009     .5744559    3.861543
           _cons |    15.4022   5.178907     2.97   0.005     4.964803     25.8396
    ------------------------------------------------------------------------------
     
    . * The intercept means that, if the unemployment rate did not change, the
    . * crime rate would be predicted to increase by about 15 crimes per 1,000
    . * people.
     
    . * The coefficient on cunem is statistically significant and of the sign we
    . * might expect: a one percentage point increase in the unemployment rate
    . * increases the crime rate by about 2.2 crimes per 1,000 people.
     
    . * Making standard errors robust to heteroskedasticity does not change much.

The same differencing strategy works if :math:`x_{it}` is a binary program indicator. The
differenced equation is the same:

.. math:: \Delta y_{i}=\delta _{0}+\beta _{1}\Delta x_{i}+\Delta u_{i}

In particular, differencing a dummy variable is fine. We interpret the model as if we have estimated
it in levels and controlled for :math:`a_{i}`.

In many program evaluation settings no units are treated at :math:`t=1` and some are treated at
:math:`t=2`, in which case :math:`\Delta x_{i}=x_{i2}-x_{i1}=x_{i2}`, so putting in :math:`\Delta
x_{i}` is the same as putting in the treatment dummy for the second time period.

.. admonition:: Example: Effects of Job Training Grant on Scrap Rates (JTRAIN.DTA)

    Same data we used before, but now use use a differencing strategy. The years we use are 1987 and
    1988.

    .. math::

        \begin{aligned}
        lscrap_{it} &=&\beta _{0}+\delta _{0}d88_{t}+\beta
        _{1}grant_{it}+a_{i}+u_{it} \\
        \Delta lscrap_{i} &=&\delta _{0}+\beta _{1}\Delta grant_{i}+\Delta u_{i} \\
        &=&\delta _{0}+\beta _{1}grant_{i,1988}+\Delta u_{i}\end{aligned}

::

    . use jtrain
     
    . des fcode year scrap grant
     
                  storage  display     value
    variable name   type   format      label      variable label
    -------------------------------------------------------------------------------------------------------------------------------
    fcode           float  %9.0g                  firm code number
    year            int    %9.0g                  1987, 1988, or 1989
    scrap           float  %9.0g                  scrap rate (per 100 items)
    grant           byte   %9.0g                  = 1 if received grant
     
    . keep if year <= 1988
    (157 observations deleted)
     
    . tab year if scrap != .
     
    1987, 1988, |
        or 1989 |      Freq.     Percent        Cum.
    ------------+-----------------------------------
           1987 |         54       50.00       50.00
           1988 |         54       50.00      100.00
    ------------+-----------------------------------
          Total |        108      100.00

::

    .* POLS gives essentially a zero effect. Sign is actually positive:
     
    . reg lscrap d88 grant
     
          Source |       SS       df       MS              Number of obs =     108
    -------------+------------------------------           F(  2,   105) =    0.18
           Model |  .810536068     2  .405268034           Prob > F      =  0.8378
        Residual |  240.098947   105  2.28665664           R-squared     =  0.0034
    -------------+------------------------------           Adj R-squared = -0.0156
           Total |  240.909484   107   2.2514905           Root MSE      =  1.5122
     
    ------------------------------------------------------------------------------
          lscrap |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
             d88 |  -.1889081   .3281441    -0.58   0.566    -.8395572     .461741
           grant |   .0566004     .43091     0.13   0.896    -.7978145    .9110152
           _cons |   .5974341   .2057802     2.90   0.005     .1894099    1.005458
    ------------------------------------------------------------------------------
     

::

    . * Correcting the standard errors for clustering and heteroskedasticity
    . * changes nothing.
     
    . reg lscrap d88 grant, cluster(fcode)
     
    Linear regression                                      Number of obs =     108
                                                           F(  2,    53) =    2.28
                                                           Prob > F      =  0.1124
                                                           R-squared     =  0.0034
                                                           Root MSE      =  1.5122
     
                                     (Std. Err. adjusted for 54 clusters in fcode)
    ------------------------------------------------------------------------------
                 |               Robust
          lscrap |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
             d88 |  -.1889081   .1400111    -1.35   0.183    -.4697349    .0919187
           grant |   .0566004   .3708021     0.15   0.879    -.6871345    .8003353
           _cons |   .5974341   .2190626     2.73   0.009     .1580501    1.036818
    ------------------------------------------------------------------------------

::

    . des clscrap cgrant
     
                  storage  display     value
    variable name   type   format      label      variable label
    -------------------------------------------------------------------------------------------------------------------------------
    clscrap         float  %9.0g                  lscrap - lscrap_1; year > 1987
    cgrant          byte   %9.0g                  grant - grant_1
     
    . * Using differences, the estimated effect is much different:
     
    . reg clscrap cgrant
     
          Source |       SS       df       MS              Number of obs =      54
    -------------+------------------------------           F(  1,    52) =    3.74
           Model |  1.23795567     1  1.23795567           Prob > F      =  0.0585
        Residual |  17.1971851    52  .330715099           R-squared     =  0.0672
    -------------+------------------------------           Adj R-squared =  0.0492
           Total |  18.4351408    53  .347832845           Root MSE      =  .57508
     
    ------------------------------------------------------------------------------
         clscrap |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
          cgrant |  -.3170579   .1638751    -1.93   0.058    -.6458974    .0117816
           _cons |  -.0574357    .097206    -0.59   0.557    -.2524938    .1376224
    ------------------------------------------------------------------------------

::

    . reg clscrap cgrant, robust
     
    Linear regression                                      Number of obs =      54
                                                           F(  1,    52) =    3.42
                                                           Prob > F      =  0.0701
                                                           R-squared     =  0.0672
                                                           Root MSE      =  .57508
     
    ------------------------------------------------------------------------------
                 |               Robust
         clscrap |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
          cgrant |  -.3170579   .1714224    -1.85   0.070    -.6610422    .0269264
           _cons |  -.0574357    .091606    -0.63   0.533    -.2412566    .1263852
    ------------------------------------------------------------------------------
     
    . * Suppose the changes did not already exist.
     
    . drop clscrap cgrant

::

    . xtset fcode year
           panel variable:  fcode (strongly balanced)
            time variable:  year, 1987 to 1988
                    delta:  1 unit
     
    . gen clscrap = d.lscrap
    (260 missing values generated)
     
    . gen cgrant = d.grant
    (157 missing values generated)
     
    . reg clscrap cgrant, robust
     
    Linear regression                                      Number of obs =      54
                                                           F(  1,    52) =    3.42
                                                           Prob > F      =  0.0701
                                                           R-squared     =  0.0672
                                                           Root MSE      =  .57508
     
    ------------------------------------------------------------------------------
                 |               Robust
         clscrap |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
          cgrant |  -.3170579   .1714224    -1.85   0.070    -.6610422    .0269264
           _cons |  -.0574357    .091606    -0.63   0.533    -.2412566    .1263852
    ------------------------------------------------------------------------------

::

    * Estimate is pretty similar to when we controlled for the lagged scrap rate:
     
    . reg lscrap grant lscrap_1, robust
     
    Linear regression                                      Number of obs =      54
                                                           F(  2,    51) =   77.79
                                                           Prob > F      =  0.0000
                                                           R-squared     =  0.8728
                                                           Root MSE      =  .51267
     
    ------------------------------------------------------------------------------
                 |               Robust
          lscrap |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
           grant |  -.2539697   .1463727    -1.74   0.089    -.5478251    .0398857
        lscrap_1 |   .8311606   .0735407    11.30   0.000     .6835215    .9787996
           _cons |    .021237   .0998451     0.21   0.832    -.1792103    .2216843
    ------------------------------------------------------------------------------
     
    . * In this case, differencing and including a lagged dependent variable
    . * are similar.

In applications where :math:`x_{it}` is a dummy variable with no assignment in the first period, the
FD estimator has a simple interpretation. It is the same as applying OLS to

.. math:: \Delta y_{i}=\delta _{0}+\beta _{1}x_{i2}+\Delta u_{i}

where :math:`x_{i2}` is the second-period program participation (zero or one).

Regression on a single dummy variable is easy to characterize: the estimate of :math:`\beta _{1}` is
just the difference in means between the treated group and the control group:

.. math:: \hat{\beta}_{FD}=\overline{\Delta y}_{treat}-\overline{\Delta y}_{control}.

This has also been called a difference-in-differences estimator. Here, unlike in the case of pooled
cross sections, the differences are within the same unit.

The FD estimator is useful when treatment assignment depends on :math:`a_{i}`. If we think
assignment in period two (still taking :math:`x_{i1}=0` for all :math:`i`) is determined largely by
:math:`y_{i1}` (first-period response), it is common to add :math:`y_{i1}` as a regressor:

.. math:: \Delta y_{i}=\delta _{0}+\beta _{1}x_{i2}+\rho y_{i1}+e_{i1}

and use OLS. (This is the same as regressing :math:`y_{i2}` on :math:`x_{i2}` and :math:`y_{i1}`.)

Adding :math:`y_{i1}` seems more general, but it can actually cause inconsistency if :math:`x_{i2}`
is determined by :math:`a_{i}`, not :math:`y_{i1}`. The argument is somewhat subtle.

The treatment effects literature is more about adding observed controls to make assignment exogenous
and prefers to add :math:`y_{i1}`. The unobserved effects approach is used more by economists but
the treatment effects approach is gaining.

See Wooldridge (2010, MIT Press) for more discussion.

**More Details: The Strict Exogeneity Assumption**

Recall that the estimating equation in changes or differences is

.. math:: y_{i2}-y_{i1}=\delta _{0}+\beta _{1}(x_{i2}-x_{i1})+(u_{i2}-u_{i1})

We emphasized that this equation is free of :math:`a_{i}`. But we still need the error in this
equation to be uncorrelated with the explanatory variable for OLS to be consistent:

.. math:: Cov(x_{i2}-x_{i1},u_{i2}-u_{i1})=Cov(\Delta x_{i},\Delta u_{i})=0.

When does this condition hold?

Algebra gives

.. math::

    \begin{aligned} Cov(x_{i2}-x_{i1},u_{i2}-u_{i1}) &=&Cov(x_{i2},u_{i2})-Cov(x_{i1},u_{i2}) \\
    &&-Cov(x_{i2},u_{i1})+Cov(x_{i1},u_{i2})\end{aligned}

Except by fluke, we need the explanatory variable in *both* time periods to be uncorrelated with the
error in *both* time periods:

.. math:: Cov(x_{is},u_{it})=0\text{, }s,t=1,2.

Note that it is not enough to just assume

.. math:: Cov(x_{i1},u_{i1})=Cov(x_{i2},u_{i2})=0

or

.. math:: Cov(x_{it},u_{it})=0\text{, }t=1,2,

which is known as *contemporaneous exogeneity*.

The stronger assumption

.. math:: Cov(x_{is},u_{it})=0\text{, }s,t=1,2.

is known as *strict exogeneity*. It requires that there are no lagged effects of :math:`x`, and it
rules out feedback from :math:`u_{i1}` to :math:`x_{i2}`.

Remember, if we difference we do not have to worry about :math:`Cov(x_{it},a_{i})\neq 0`. But if
:math:`x_{it}` is changing over time -- as it must to apply the differencing solution -- we should ask
why. Might :math:`x_{it}` be chosen in part based on past shocks?

.. admonition:: Example: Test Sores and Class Size

   .. math:: score_{it}=\beta _{0}+\delta _{0}d2_{t}+\beta _{1}size_{it}+a_{i}+u_{it}

   A bad shock to the student score at :math:`t-1` – negative :math:`u_{i,t-1}` – might lead a
   principle to put the student in a smaller class the next year. Then :math:`u_{i,t-1}` and
   :math:`size_{it}` would be postively correlated, violating strict exogeneity.

   Might class size have a lasting effect? If so, last years size, :math:`size_{i,t-1}` is incorrectly
   omitted from the equation – also a violation of strict exogeneity. (With two years of data, not much
   we can do about that.)

   Hard to solve violations of strict exogeneity; at a minimum, need more than two time periods.

   For policy analysis, hope that designation does not react to shocks. Designation largely depends on
   historical factors in :math:`a_{i}`, and those get differenced away.

**Multiple Explanatory Variables**

Having more than one explanatory variable causes no problems. A general two-period model is

.. math::

    y_{it}=\beta _{0}+\delta _{0}d2_{t}+\beta _{1}x_{it1}+\beta _{2}x_{it2}+...+\beta
    _{k}x_{itk}+a_{i}+u_{it}\text{, }t=1,2,

Now the first-difference equation looks like

.. math::

  \Delta y_{i}=\delta _{0}+\beta _{1}\Delta x_{i1}+\beta _{2}\Delta x_{i2}+...+\beta _{k}\Delta
  x_{ik}+\Delta u_{i},

and we just estimate this by OLS. We might use heteroskedasticity-robust inference.

We want to interpret the :math:`\beta _{j}` as in the original equation: What is the effect of
changing :math:`x_{j}` on :math:`y`, holding fixed the other explanatory variables *and* the
unobserved heterogeneity.

All explanatory variables get differenced, whether they are dummy variables, squares, interactions,
and so on. The FD equation is an estimating equation.

.. admonition:: Example:: In-Season GPA Effects on Student-Athletes

    GPA3.DTA: Two terms of data, fall and spring. Some sports are entirely in the fall, others entirely
    in the spring, others spill over into both.

    The key variable, :math:`season`, is binary.  But it can be switched on in either the fall or spring
    or both.

    Without differencing, it makes sense to control for time-constant variables, such as SAT score and
    high school percentile.

    The equation we start with is

    .. math::

        \begin{aligned} trmgpa_{it} &=&\beta _{0}+\delta _{0}spring_{t}+\beta _{1}season_{it}+\beta
        _{2}crsgpa_{it}+\beta _{2}sat_{i} \\ &&+\beta _{4}hsperc_{i}+a_{i}+u_{it}\end{aligned}

    for :math:`t=fall`, :math:`spring`.

    :math:`sat_{i}` and :math:`hsperc_{i}` are recorded in high school. Like :math:`a_{i}`, they get
    differenced away:

    .. math::

        \Delta trmgpa_{i}=\delta _{0}+\beta _{1}\Delta season_{i}+\beta _{2}\Delta crsgpa_{i}+\Delta u_{i}

    Note that the variable measuring difficulty of the course load, :math:`crsgpa_{it}`, varies across
    semester.

::

    . use gpa2
     
    . des trmgpa crsgpa spring season sat hsperc
     
                  storage  display     value
    variable name   type   format      label      variable label
    -------------------------------------------------------------------------------------------------------------------------------
    trmgpa          float  %9.0g                  term GPA
    crsgpa          float  %9.0g                  weighted course GPA
    spring          byte   %9.0g                  =1 if spring term
    season          byte   %8.0g                  =1 if in season
    sat             int    %4.0f                  SAT score
    hsperc          float  %9.0g                  percentile in h.s.
     
    . reg trmgpa spring season crsgpa sat hsperc, cluster(id)
     
    Linear regression                                      Number of obs =     732
                                                           F(  5,   365) =  102.48
                                                           Prob > F      =  0.0000
                                                           R-squared     =  0.4276
                                                           Root MSE      =  .57563
     
                                       (Std. Err. adjusted for 366 clusters in id)
    ------------------------------------------------------------------------------
                 |               Robust
          trmgpa |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
          spring |  -.0281825   .0341857    -0.82   0.410    -.0954081    .0390432
          season |   .0605399   .0433015     1.40   0.163    -.0246117    .1456916
          crsgpa |   1.145918   .0993488    11.53   0.000     .9505498    1.341286
             sat |    .001946   .0001694    11.49   0.000     .0016128    .0022791
          hsperc |  -.0099631   .0013422    -7.42   0.000    -.0126025   -.0073236
           _cons |  -2.281623   .3353073    -6.80   0.000       -2.941   -1.622247
    ------------------------------------------------------------------------------

::

    . * The in-season effect from pooled OLS is actually positive and almost
    . * statistically significant. Might this just be picking up that certain
    . * sports (such as football) are played in one season (fall), and football
    . * players are different academically from other athletes?
     
    . reg ctrmgpa cseason ccrsgpa, robust
     
    Linear regression                                      Number of obs =     366
                                                           F(  2,   363) =   38.16
                                                           Prob > F      =  0.0000
                                                           R-squared     =  0.2064
                                                           Root MSE      =   .5774
     
    ------------------------------------------------------------------------------
                 |               Robust
         ctrmgpa |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
         cseason |  -.0572418   .0405979    -1.41   0.159    -.1370784    .0225948
         ccrsgpa |   1.139213   .1305734     8.72   0.000     .8824375    1.395988
           _cons |  -.0693142   .0338591    -2.05   0.041    -.1358989   -.0027295
    ------------------------------------------------------------------------------
     
    . * Differencing away the student ``ability'' gives a negative effect, and
    . * close to statistical significance.

In cases where a policy variable is of interest, it is important not to include regressors that are
themselves influence by the policy intervention! This can lead to overcontrolling.

For example, in studying the effects of class size (say, small or large) on math performance, one
would not include a science test score among the explanatory variables (unless it is from a previous
year).

Differencing with More than Two Years of Data 
================================================

First Differencing can be used with more than two years of panel data, but we must be careful to
account for serial correlation (and, as usual, possibly heteroskedasticity) in the FD equation. This
is because the FD equation is no longer just a single cross section.

Generally, we should also include a full set of time dummies for a convincing analysis.

With :math:`T` time periods, where now :math:`T\geq 2`, we can write

.. math::

    y_{it}=\delta _{1}+\delta _{2}d2_{t}+...+\delta _{T}dT_{t}+\beta _{1}x_{it1}+\beta
    _{2}x_{it2}+...+\beta _{k}x_{itk}+a_{i}+u_{it}

where now :math:`\delta _{1}` denotes the intercept in the first year and :math:`\delta _{t}`,
:math:`t\geq 2`, is the difference between the intercept in period :math:`t` and period :math:`1`.

The model still contains an unobserved effect, :math:`a_{i}`, and idiosyncratic error,
:math:`u_{it}`.

When we difference we lose the first time period, as before, but we are left with a panel data set
if we start with :math:`T\geq 3`:

.. math::

    \Delta y_{it}=\delta _{2}\Delta d2_{t}+...+\delta _{T}\Delta dT_{t}+\beta _{1}\Delta x_{it1}+\beta
    _{2}\Delta x_{it2}+...+\beta _{k}\Delta x_{itk}+\Delta u_{it}

for :math:`t=2,3,...,T`.

With :math:`T\geq 3`, the changes in the time dummies consists of the values :math:`\{-1,0,1\}`. For
example, with :math:`T=3`, :math:`\Delta d2_{2}=1`, :math:`\Delta d2_{3}=-1`, :math:`\Delta
d3_{2}=0`, :math:`\Delta d3_{3}=1`.

Unless we are interested in the original :math:`\delta _{t}`, it is easier to include an overall
intercept and not difference the time dummies. We lose a time dummy because we lose the first time
period:

.. math::

    \Delta y_{it}=\alpha _{0}+\alpha _{3}d3_{t}+...+\alpha _{T}dT_{t}+\beta _{1}\Delta x_{it1}+\beta
    _{2}\Delta x_{it2}+...+\beta _{k}\Delta x_{itk}+\Delta u_{it}

Now we just use POLS on the changes for :math:`t=2,...,T`, and we can use the usual and adjusted
:math:`R`-squareds as goodness-of-fit in the FD equation.

Except for changing how we allow for different time intercepts, this is the same model as before.
Estimates of the :math:`\beta _{j}` are identical.

As before, we have eliminated :math:`a_{i}` – which we think is the main source of correlation
between the :math:`x_{itj}` and the composite error :math:`v_{it}=a_{i}+u_{it}`. But we can see we
need

.. math:: Cov(\Delta x_{itj},\Delta u_{it})=0\text{, }j=1,...,k

and this is a strict exogeneity assumption. Sufficient is for each :math:`j`,

.. math:: Cov(x_{isj},u_{it})=0\text{, all }s,t=1,...,T

If we write the FD equation as

.. math::

\Delta y_{it}=\alpha _{0}+\alpha _{3}d3_{t}+...+\alpha _{T}dT_{t}+\beta _{1}\Delta x_{it1}+\beta
_{2}\Delta x_{it2}+...+\beta _{k}\Delta x_{itk}+e_{it}

with errors

.. math:: e_{it}=\Delta u_{it}\text{, }t=2,3,...T,

then, in general, the :math:`e_{it}` will be correlated across time (relevant only when
:math:`T>2`)\ :math:`.`

In fact, if the original errors :math:`\{u_{it}\}` are uncorrelated with constant variance
:math:`\sigma _{u}^{2}`, it can be shown that

.. math:: Corr(e_{it},e_{i,t+1})=-1/2

Generally, we should expect complicated serial correlation in :math:`\{u_{it}\}`, which results in a
complicated pattern for :math:`\{e_{it}\}`. With a large cross-sectional sample size :math:`N`, and
assuming :math:`T` is not larger than :math:`N`, we can use clustering.

Stata has a few different ways of doing FD.  First, after using ``xtset``, we can construct the
differences ourselves.

The cluster-robuststandard errors and test statistics are also robust to heteroskedasticity of any
kind.

``xtset id year``

``gen cy = D.y``

``gen cx1 = D.x1``

``...``

``gen cxk = D.xk``

``reg cy d3 d4 ... dT cx1 cx2 ... cxk, cluster(id)``

A simpler way is to apply the differencing operator, ``d.``, to the equation all at once:

``reg d.(y d2 d3 ... dT x1 x2 ... xk), cluster(id)``

This gives different time intercepts because these time dummies are differenced. And one will drop
due to losing a time period (because default is to estimate an intercept.)

Can force the intercept to zero to get estimates on the :math:`T-1` dummies:

``reg D.(y d2 d3 ... dT x1 x2 ... xk), cluster(id) nocons``

Reminder: In using FD methods, it is important to remember that an equation such as

.. math::

    \Delta y_{it}=\alpha _{0}+\alpha _{3}d3_{t}+...+\alpha _{T}dT_{t}+\beta _{1}\Delta x_{it1}+\beta
    _{2}\Delta x_{it2}+...+\beta _{k}\Delta x_{itk}+\Delta u_{it}

is an *estimating* equation used to get rid of :math:`a_{i}`. The estimates should be interpreted in
the context of the original levels equation

.. math::

    y_{it}=\delta _{1}+\delta _{2}d2_{t}+...+\delta _{T}dT_{t}+\beta _{1}x_{it1}+\beta
    _{2}x_{it2}+...+\beta _{k}x_{itk}+a_{i}+u_{it}

With the levels equation as the starting point, it is easy to choose explanatory variables for
policy evaluation that allow, say, lagged effects. We can also add quadratics, interactions, and so
on. These are constructed before applying differencing.

.. admonition:: Example:
   
    Suppose that students are tested in 3rd, 4th, and 5th grades, and :math:`y_{it}` is the score of
    student :math:`i` in grade :math:`t`. Let :math:`w_{it}` be a binary variable equal to one if
    student :math:`i` in grade :math:`t` was in a small class, and zero otherwise. Suppose we think
    current class size and previous number of years in a small class matters. Let :math:`n_{i,t-1}` be
    the number of grades in a small class up through the previous grade.

    Letting :math:`\mathbf{r}_{it}` be a vector of other time-varying controls, a suitable model is

    .. math:: y_{it}=\delta _{t}+\beta _{1}w_{it}+\beta _{2}n_{i,t-1}+\mathbf{r}_{it}\mathbf{\gamma
       }+c_{i}+u_{it},\text{ }t=1,2,3

    and the FD estimating equation is

    .. math::

        \Delta y_{it}=\eta _{t}+\beta _{1}\Delta w_{it}+\beta _{2}\Delta n_{i,t-1}+\Delta
        \mathbf{r}_{it}\mathbf{\gamma }+\Delta u_{it},\text{ }t=2,3

    Everything gets differenced in the estimating equation, even policy dummy variables.

    Note that the new intercepts, :math:`\eta _{t}`, are :math:`\eta _{t}=\delta _{t}-\delta _{t-1}`. If
    we don’t care about the :math:`\delta _{t}`, just include a full set of time dummies in the FD
    equation (one fewer than levels equation).

    If, say, we interact :math:`w_{it}` and a variable :math:`r_{it}`, we construct :math:`w_{it}r_{it}`
    and then difference it. (Example later.)

.. admonition:: Example: Effects of Job Training Grants in Michigan

    JTRAIN.DTA: Includes three years of data, 1987, 1988, and 1989. No grants in 1987, if got a grant in
    1988, could not get one in 1989.

    Job training in a previous year could have an effect on this year’s productivity. Omitting the
    previous year’s grant indicator could be a serious misspecification.

    We already saw what happens when using just 1987 and 1988 for a two-period panel diff-of-diffs.

    Remember that response is :math:`lscrap=\log (scrap)`, where :math:`scrap` is the number of items
    out of 100 that must be discarded.

    If the training grant improved productivity, :math:`lscrap` should fall, on average.

    Since we know that :math:`grant` was zero prior to 1987, we have three years of data to add a lagged
    effect:

    .. math::

        lscrap_{it}=\alpha _{0}+\alpha _{1}d88_{t}+\alpha _{2}d89_{t}+\beta _{1}grant_{it}+\beta
        _{2}grant_{i,t-1}+a_{i}+u_{it}\text{, }t=1,2,3

    If job training affects persist for at least a year, :math:`\beta _{2}<0`.

    Can estimate the above equation by pooled OLS if grant designation is uncorrelated with
    :math:`a_{i}` (and :math:`u_{it}`).

    The differenced equation (where the :math:`c` stands for change :math:`d` can be confused with
    dummy variable) is

    .. math::

        clscrap_{it}=\beta _{1}cgrant_{it}+\beta _{2}cgrant_{i,t-1}+\alpha _{1}cd88_{t}+\alpha
        _{2}cd89_{t}+cu_{it}\text{, }t=2,3

    Estimate by pooled OLS (using two years). The firm effect :math:`a_{i}` has been removed.

    Can just use a dummy for 1989 and include a constant.

::

    . use jtrain
     
    . * Only keep firms where scrap is not missing for all three years:
     
    . gen nonmiss = scrap != .
     
    . tab nonmiss
     
        nonmiss |      Freq.     Percent        Cum.
    ------------+-----------------------------------
              0 |        309       65.61       65.61
              1 |        162       34.39      100.00
    ------------+-----------------------------------
          Total |        471      100.00
     
    . egen tobs = sum(nonmiss), by(fcode)
     
    . tab tobs
     
           tobs |      Freq.     Percent        Cum.
    ------------+-----------------------------------
              0 |        309       65.61       65.61
              3 |        162       34.39      100.00
    ------------+-----------------------------------
          Total |        471      100.00
     
    . keep if tobs == 3
    (309 observations deleted)

::

    . tab year
     
    1987, 1988, |
        or 1989 |      Freq.     Percent        Cum.
    ------------+-----------------------------------
           1987 |         54       33.33       33.33
           1988 |         54       33.33       66.67
           1989 |         54       33.33      100.00
    ------------+-----------------------------------
          Total |        162      100.00
     
    . sum scrap grant
     
        Variable |       Obs        Mean    Std. Dev.       Min        Max
    -------------+--------------------------------------------------------
           scrap |       162    3.843642     6.00777        .01         30
           grant |       162    .1790123    .3845514          0          1

::

    . bysort year: sum scrap grant
     
    ----------------------------------------------------------------------
    -> year = 1987
     
        Variable |       Obs        Mean    Std. Dev.       Min        Max
    -------------+--------------------------------------------------------
           scrap |        54    4.611667    6.414963        .01         30
           grant |        54           0           0          0          0
     
    ----------------------------------------------------------------------
    -> year = 1988
     
        Variable |       Obs        Mean    Std. Dev.       Min        Max
    -------------+--------------------------------------------------------
           scrap |        54    3.787778    5.984144        .05         25
           grant |        54    .3518519    .4820322          0          1
     
    ----------------------------------------------------------------------
    -> year = 1989
     
        Variable |       Obs        Mean    Std. Dev.       Min        Max
    -------------+--------------------------------------------------------
           scrap |        54    3.131481    5.617764        .03         30
           grant |        54    .1851852    .3920952          0          1
     
    . * 35.2% of the firms got a grant in 1988. 18.5% got one in 1989. 
    . * There is no overlap across the two years.

::

    . sort fcode year
     
    . * The change in scrap is missing for 1987. We assume there 
    . * were no grants in 1986, so cgrant is zero (even though one
    . * could argue it should be missing).
     
    . list fcode year scrap grant lscrap clscrap cgrant in 43/60
     
         +----------------------------------------------------------------+
         |  fcode   year   scrap   grant      lscrap     clscrap   cgrant |
         |----------------------------------------------------------------|
     43. | 410665   1987     .05       0   -2.995732           .        0 |
     44. | 410665   1988     .08       0   -2.525729    .4700036        0 |
     45. | 410665   1989     .03       0   -3.506558   -.9808292        0 |
     46. | 410685   1987     .14       0   -1.966113           .        0 |
     47. | 410685   1988     .16       0   -1.832582    .1335313        0 |
         |----------------------------------------------------------------|
     48. | 410685   1989     .23       0   -1.469676    .3629056        0 |
     49. | 418011   1987      10       0    2.302585           .        0 |
     50. | 418011   1988       3       1    1.098612   -1.203973        1 |
     51. | 418011   1989       2       0    .6931472   -.4054651       -1 |
     52. | 418021   1987       1       0           0           .        0 |
         |----------------------------------------------------------------|
     53. | 418021   1988       1       1           0           0        1 |
     54. | 418021   1989       1       0           0           0       -1 |
     55. | 418035   1987       6       0    1.791759           .        0 |
     56. | 418035   1988       5       1    1.609438   -.1823215        1 |
     57. | 418035   1989       3       0    1.098612   -.5108256       -1 |
         |----------------------------------------------------------------|
     58. | 418045   1987       2       0    .6931472           .        0 |
     59. | 418045   1988       2       0    .6931472           0        0 |
     60. | 418045   1989       2       0    .6931472           0        0 |
         +----------------------------------------------------------------+

::

    . * Pooled OLS with robust standard errors:
     
    . reg lscrap grant grant_1 d88 d89, cluster(fcode)
     
    Linear regression                                      Number of obs =     162
                                                           F(  4,    53) =    3.90
                                                           Prob > F      =  0.0075
                                                           R-squared     =  0.0173
                                                           Root MSE      =  1.4922
     
                                     (Std. Err. adjusted for 54 clusters in fcode)
    ------------------------------------------------------------------------------
                 |               Robust
          lscrap |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
           grant |   .2000197   .3226246     0.62   0.538    -.4470833    .8471227
         grant_1 |   .0489357   .4720909     0.10   0.918    -.8979588    .9958302
             d88 |  -.2393704   .1258802    -1.90   0.063    -.4918541    .0131133
             d89 |  -.4965236   .2331842    -2.13   0.038    -.9642318   -.0288154
           _cons |   .5974341   .2197527     2.72   0.009      .156666    1.038202
    ------------------------------------------------------------------------------
     
    . * Again, pooled OLS actually shows a positive (but insignificant) effect.

::

    . * Differencing gives much stronger results, but statistical signif. is 
    . * marginal (small sample size).
     
    . reg clscrap cgrant cgrant_1 d89, cluster(fcode)
     
    Linear regression                                      Number of obs =     108
                                                           F(  3,    53) =    1.98
                                                           Prob > F      =  0.1284
                                                           R-squared     =  0.0365
                                                           Root MSE      =  .57672
     
                                     (Std. Err. adjusted for 54 clusters in fcode)
    ------------------------------------------------------------------------------
                 |               Robust
         clscrap |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
          cgrant |   -.222781   .1316461    -1.69   0.096    -.4868297    .0412676
        cgrant_1 |  -.3512459   .2709732    -1.30   0.201    -.8947493    .1922575
             d89 |  -.0962081   .1136492    -0.85   0.401    -.3241596    .1317434
           _cons |  -.0906072   .0901821    -1.00   0.320    -.2714895    .0902751
    ------------------------------------------------------------------------------

::

    . * If we forget the lag, we get a much smaller effect of contemporaneous
    . * grant because grant and grant_1 are negatively correlated:
     
    . reg clscrap cgrant d89, cluster(fcode)
     
    Linear regression                                      Number of obs =     108
                                                           F(  2,    53) =    1.76
                                                           Prob > F      =  0.1826
                                                           R-squared     =  0.0158
                                                           Root MSE      =  .58009
     
                                     (Std. Err. adjusted for 54 clusters in fcode)
    ------------------------------------------------------------------------------
                 |               Robust
         clscrap |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
          cgrant |  -.0830995   .0664392    -1.25   0.217    -.2163597    .0501606
             d89 |  -.1473672   .0943677    -1.56   0.124    -.3366449    .0419105
           _cons |  -.1397544   .0806593    -1.73   0.089    -.3015364    .0220276
    ------------------------------------------------------------------------------

::

    . * If we use the d. operator, we get estimated time effects directly:
     
    . reg d.(lscrap grant grant_1 d88 d89), cluster(fcode)
    note: _delete omitted because of collinearity
     
    Linear regression                                      Number of obs =     108
                                                           F(  3,    53) =    1.98
                                                           Prob > F      =  0.1284
                                                           R-squared     =  0.0365
                                                           Root MSE      =  .57672
     
                                     (Std. Err. adjusted for 54 clusters in fcode)
    ------------------------------------------------------------------------------
                 |               Robust
        D.lscrap |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
           grant |
             D1. |   -.222781   .1316461    -1.69   0.096    -.4868297    .0412676
                 |
         grant_1 |
             D1. |  -.3512459   .2709732    -1.30   0.201    -.8947493    .1922575
                 |
             d88 |
             D1. |   .0481041   .0568246     0.85   0.401    -.0658717    .1620798
                 |
             d89 |
             D1. |          0  (omitted)
                 |
           _cons |  -.1387113   .0953842    -1.45   0.152    -.3300278    .0526053
    ------------------------------------------------------------------------------

::

    . * Instead of dropping d89, force it to drop intercept. Drawback is the
    . * R-squared is ``uncentered'' (no mean subtracted), and so it is
    . * artificially high.
     
    . reg d.(lscrap grant grant_1 d88 d89), cluster(fcode) nocons
     
    Linear regression                                      Number of obs =     108
                                                           F(  4,    53) =    5.85
                                                           Prob > F      =  0.0006
                                                           R-squared     =  0.1601
                                                           Root MSE      =  .57672
     
                                     (Std. Err. adjusted for 54 clusters in fcode)
    ------------------------------------------------------------------------------
                 |               Robust
        D.lscrap |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
           grant |
             D1. |   -.222781   .1316461    -1.69   0.096    -.4868297    .0412676
                 |
         grant_1 |
             D1. |  -.3512459   .2709732    -1.30   0.201    -.8947493    .1922575
                 |
             d88 |
             D1. |  -.0906072   .0901821    -1.00   0.320    -.2714895    .0902751
                 |
             d89 |
             D1. |  -.2774225   .1907685    -1.45   0.152    -.6600556    .1052105
    ------------------------------------------------------------------------------

::

    . * We can force Stata to center the R-squared by telling it to act as if
    . * the equation has an intercept:
     
    . reg d.(lscrap grant grant_1 d88 d89), cluster(fcode) nocons hascons
     
    Linear regression                                      Number of obs =     108
                                                           F(  4,    53) =    5.85
                                                           Prob > F      =  0.0006
                                                           R-squared     =  0.0365
                                                           Root MSE      =  .57672
     
                                     (Std. Err. adjusted for 54 clusters in fcode)
    ------------------------------------------------------------------------------
                 |               Robust
        D.lscrap |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
           grant |
             D1. |   -.222781   .1316461    -1.69   0.096    -.4868297    .0412676
                 |
         grant_1 |
             D1. |  -.3512459   .2709732    -1.30   0.201    -.8947493    .1922575
                 |
             d88 |
             D1. |  -.0906072   .0901821    -1.00   0.320    -.2714895    .0902751
                 |
             d89 |
             D1. |  -.2774225   .1907685    -1.45   0.152    -.6600556    .1052105
    ------------------------------------------------------------------------------

Fixed Effects Estimation
===========================

Differencing is one method of eliminating :math:`a_{i}` (which itself is sometimes called a *fixed
effect*).

Alternatively, can use the fixed effects or withintransformation: remove the within :math:`i` time
averages.

In the simple model with only :math:`x_{it}`:

.. math:: y_{it}=\beta _{0}+\beta _{1}x_{it}+a_{i}+u_{it}

Average this equation across :math:`t` to get

.. math:: \bar{y}_{i}=\beta _{0}+\beta _{1}\bar{x}_{i}+a_{i}+\bar{u}_{i}

where :math:`\bar{y}_{i}=T^{-1}\sum_{t=1}^{T}y_{it}` is a time average for unit :math:`i`. Similarly
for :math:`\bar{x}_{i}` and :math:`\bar{u}_{i}`.

Subtract the time-averaged equation – sometimes called the between equation – from other time
periods:

.. math:: y_{it}-\bar{y}_{i}=\beta _{1}(x_{it}-\bar{x}_{i})+(u_{it}-\bar{u}_{i})

As with the FD equation, this equation is free of :math:`a_{i}`.

We view this time-demeaned (or within) equation as an estimating equation. As with FD, we interpret
:math:`\beta _{1}` in the levels equation

.. math:: y_{it}=\beta _{0}+\beta _{1}x_{it}+a_{i}+u_{it}

We can use pooled OLS on the deviations from time averages to estimate :math:`\beta _{1}`. Called
the fixed effects (FE) estimator or the within estimator.

There is yet another way to compute :math:`\hat{\beta}_{1}`. Keep the original data, :math:`y_{it}`
and :math:`x_{it}`, and run a regression of :math:`y_{it}` on :math:`N` dummy variables (one for
each :math:`i`) and :math:`x_{it}`. Called the dummy variable regression. Often not practical when
:math:`N` is large.

Stata reports a constant, or intercept, with FE estimation. But :math:`\beta _{0}` gets eliminated
by the FE transformation, so what is the estimated intercept? It is the average of the :math:`N`
coefficients on the dummy variables (rarely of interest).

Like FD, FE can be applied with :math:`T\geq 2`. An interesting algebraic fact is that FD and FD are
numerically identical when :math:`T=2`. For :math:`T>2`, they are different. Typically find larger
differences for larger :math:`T`.

Like FD, FE allows arbitrary correlation between :math:`x_{it}` and :math:`a_{i}`, but it requires a
strict exogeneity assumption with respect to :math:`\{u_{it}\}`. As with FD, a sufficient condition
is

.. math:: Cov(x_{is},u_{it})=0\text{, }s,t=1,...,T.

FE has an advantage over FD when strict exogeneity fails: under reasonable assumptions with large
:math:`T`, FE tends to have less bias (Wooldridge, 2010, Chapter 10).

Because :math:`a_{i}` is removed using the FE transformation, it cannot be a source of serial
correlation. But the :math:`u_{it}` might have serial correlation (and heteroskedasticity), and so
use cluster robust inference. (Using robust by itself is never a good idea with FE.)

Stata does FE as a canned program; the command is ``xtreg``. It computes proper standard errors and
test statistics.  But the default is that :math:`\{u_{it}\}` is homoskedastic and serially
uncorrelated, so add a cluster option.

As in other situations, it is easy to include a full set of year dummies and many explanatory
variables.

Computing pooled OLS, FD, and FE estimators can be informative and should be done in applications.
If FD and FE are very different, it is a sign that strict exogeneity fails. If POLS is different
from FE (say), it indicates explanatory variables correlated with :math:`a_{i}`.

Do not worry too much about goodness-of-fit with FE. The within \ :math:`R`-squared is probably most
informative (based on time-demeaned equation)

In Stata, an :math:`F` test for differences in :math:`N` intercepts is computed (when nonrobust
inference is used). It can be used to verify there is unobserved, additive heterogeneity.

::

    . * JTRAIN data.
     
    . * First, POLS with robust inference:
     
    . reg lscrap grant grant_1 d88 d89, cluster(fcode)
     
    Linear regression                                      Number of obs =     162
                                                           F(  4,    53) =    3.90
                                                           Prob > F      =  0.0075
                                                           R-squared     =  0.0173
                                                           Root MSE      =  1.4922
     
                                     (Std. Err. adjusted for 54 clusters in fcode)
    ------------------------------------------------------------------------------
                 |               Robust
          lscrap |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
           grant |   .2000197   .3226246     0.62   0.538    -.4470833    .8471227
         grant_1 |   .0489357   .4720909     0.10   0.918    -.8979588    .9958302
             d88 |  -.2393704   .1258802    -1.90   0.063    -.4918541    .0131133
             d89 |  -.4965236   .2331842    -2.13   0.038    -.9642318   -.0288154
           _cons |   .5974341   .2197527     2.72   0.009      .156666    1.038202
    ------------------------------------------------------------------------------

::

    . * FE with nonrobust standard errors:
     
    . xtreg lscrap grant grant_1 d88 d89, fe
     
    Fixed-effects (within) regression               Number of obs      =       162
    Group variable: fcode                           Number of groups   =        54
     
    R-sq:  within  = 0.2010                         Obs per group: min =         3
           between = 0.0079                                        avg =       3.0
           overall = 0.0068                                        max =         3
     
                                                    F(4,104)           =      6.54
    corr(u_i, Xb)  = -0.0714                        Prob > F           =    0.0001
     
    ------------------------------------------------------------------------------
          lscrap |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
           grant |  -.2523149    .150629    -1.68   0.097    -.5510178    .0463881
         grant_1 |  -.4215895      .2102    -2.01   0.047    -.8384239   -.0047551
             d88 |  -.0802157   .1094751    -0.73   0.465     -.297309    .1368776
             d89 |  -.2472028   .1332183    -1.86   0.066    -.5113797    .0169741
           _cons |   .5974341   .0677344     8.82   0.000     .4631142    .7317539
    -------------+----------------------------------------------------------------
         sigma_u |   1.438982
         sigma_e |  .49774421
             rho |  .89313867   (fraction of variance due to u_i)
    ------------------------------------------------------------------------------
    F test that all u_i=0:     F(53, 104) =    24.66             Prob > F = 0.0000

::

    . * With cluster-robust standard errors:
     
    . xtreg lscrap grant grant_1 d88 d89, fe cluster(fcode)
     
    Fixed-effects (within) regression               Number of obs      =       162
    Group variable: fcode                           Number of groups   =        54
     
    R-sq:  within  = 0.2010                         Obs per group: min =         3
           between = 0.0079                                        avg =       3.0
           overall = 0.0068                                        max =         3
     
                                                    F(4,53)            =      7.07
    corr(u_i, Xb)  = -0.0714                        Prob > F           =    0.0001
     
                                     (Std. Err. adjusted for 54 clusters in fcode)
    ------------------------------------------------------------------------------
                 |               Robust
          lscrap |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
           grant |  -.2523149   .1434399    -1.76   0.084    -.5400188     .035389
         grant_1 |  -.4215895   .2824604    -1.49   0.141    -.9881333    .1449543
             d88 |  -.0802157   .0978408    -0.82   0.416    -.2764594    .1160281
             d89 |  -.2472028   .1967819    -1.26   0.215    -.6418973    .1474917
           _cons |   .5974341   .0638746     9.35   0.000     .4693177    .7255504
    -------------+----------------------------------------------------------------
         sigma_u |   1.438982
         sigma_e |  .49774421
             rho |  .89313867   (fraction of variance due to u_i)
    ------------------------------------------------------------------------------
     
    . * Very similar to FD estimates (not too surprising with T = 3).

The ``areg`` command is another way to obtain the FE estimates in Stata. It is used here to show how
high the :math:`R`-squared is if we allow the firm effects to explainthe scrap rate.

::

     
    . areg lscrap grant grant_1 d88 d89, absorb(fcode)
     
    Linear regression, absorbing indicators           Number of obs   =        162
                                                      F(   4,    104) =       6.54
                                                      Prob > F        =     0.0001
                                                      R-squared       =     0.9276
                                                      Adj R-squared   =     0.8879
                                                      Root MSE        =     0.4977
     
    ------------------------------------------------------------------------------
          lscrap |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
           grant |  -.2523149    .150629    -1.68   0.097    -.5510178    .0463881
         grant_1 |  -.4215895      .2102    -2.01   0.047    -.8384239   -.0047551
             d88 |  -.0802157   .1094751    -0.73   0.465     -.297309    .1368776
             d89 |  -.2472028   .1332183    -1.86   0.066    -.5113797    .0169741
           _cons |   .5974341   .0677344     8.82   0.000     .4631142    .7317539
    -------------+----------------------------------------------------------------
           fcode |        F(53, 104) =     24.661   0.000          (54 categories)

In order not to count the firm fixed effects in the explanatory power, it is better to use the
:math:`R`-squared from the within (time-demeaned) equation. That is about :math:`.201`, not
:math:`.928`.

The :math:`F` statistic reported at the bottom of the ``xtreg`` (and ``areg``) output (but only when
cluster is not used) is a test of whether the :math:`N` intercepts are all the same. So it is
testing :math:`N-1` restrictions (53 in JTRAIN example). Its outcome is usually known ahead of time:
a strong rejection is expected in most cases because unobserved heterogeneity is usually important.

Random Effects Estimation 
============================

Suppose we start with the same equation as before, written in shorthand for a unit :math:`i`:

.. math:: y_{it}=\delta _{t}+\mathbf{x}_{it}\mathbf{\beta }+a_{i}+u_{it}\text{, }t=1,2,...,T

where :math:`\mathbf{x}_{it}\mathbf{\beta }=\beta _{1}x_{it1}+\beta _{2}x_{it2}+...+\beta
_{k}x_{itk}`. The :math:`\delta _{t}` represent different time intercepts. In some of what follows
we drop them for simplicity.

Unlike FD and FE, Random Effects (RE) estimation leaves :math:`a_{i}` in the error term, and then
accounts for the serial correlation over time in :math:`v_{it}=a_{i}+u_{it}` via a *generalized
least squares* (GLS) procedure.

RE estimation allows time-constant explanatory variables in :math:`\mathbf{x}_{it}`, and this is
often important in RE applications. (With FD and FE, we can think of all time-constant variables as
being captured by :math:`a_{i}`.)

How come RE can include time-constant variables (such as gender)? Like pooled OLS, RE estimation
maintains

.. math:: Cov(x_{itj},a_{i})=0\text{, }j=1,...,k

and also strict exogeneity with respect to :math:`\{u_{it}\}`:

.. math:: Cov(x_{isj},u_{it})=0\text{, }s,t=1,...,T

For consistency, RE maintains that the composite error term, :math:`v_{it}=a_{i}+u_{it}`, is
uncorrelated with the explanatory variables in all time periods.

For policy analysis, RE is typically less convincing than FD or FE: we want participation to be
correlated with the time-constant factors in :math:`a_{i}`.

However, with good time-constant controls, RE may be convincing. This is because more is taken out
of :math:`a_{i}` as we add time-constant variables.

When it is consistent, RE is typically more efficient – sometimes much more efficient – than FD or
FE.

The standard RE assumptions also include that

.. math::

   \begin{aligned}
   Cov(a_{i},u_{it}) &=&0\text{ \ (not especially controversial)} \\
   Var(u_{it}) &=&\sigma _{u}^{2}\text{ for all }t\text{ \ (constant variance
   over time)} \\
   Cov(u_{is},u_{it}) &=&0\text{, }t\neq s\text{ \ (no serial correlation)}\end{aligned}

These are assumed to hold for random draws :math:`i` from the population.

The first is uncontroversial because we are just separating the time-constant and time-varying
unobservables.

There are good reasons why the second and third of these fail, and empirically they often do.
Fortunately, the RE estimator does not rely on any of these assumptions for consistency. But if they
fail, we must use cluster-robust inference.

The GLS procedure uses properties of the composite error, :math:`v_{it}=a_{i}+u_{it}` Under the
standard RE assumptions,

.. math::

   \begin{aligned}
   Var(v_{it}) &=&\sigma _{a}^{2}+\sigma _{u}^{2} \\
   Cov(v_{it},v_{is}) &=&Var(a_{i})=\sigma _{a}^{2}\end{aligned}

where the first follows from :math:`Cov(a_{i},u_{it})=0` and the last follows from
:math:`Cov(u_{it},u_{is})=0`, :math:`t\neq s`.

Using these two equations, the serial correlation in :math:`\{v_{it}\}` is easy to characterize:

.. math:: Corr(v_{it},v_{is})=\frac{\sigma _{a}^{2}}{\sigma _{a}^{2}+\sigma _{u}^{2}}\equiv \rho

:math:`\rho` is the share of the total variance, :math:`\sigma _{a}^{2}+\sigma _{u}^{2}`, due to
the variance in :math:`a_{i}`, the unobserved effect, :math:`\sigma _{a}^{2}`.

Note that the correlation is the same no matter how far apart :math:`t` and :math:`s` are. Can be
unrealistic.

In estimation, RE acts as if there is a single correlation parameter, :math:`\rho`.

Full treatment of RE as GLS difficult, but it is not hard to describe RE and relate it to POLS (on
the levels) and FE.

A very useful characterization of RE is in terms of a partially time demeaned equation. Define a
parameter, :math:`\theta`, that is between zero and one as

.. math::

    \theta =1-\left[ \frac{1}{1+T(\sigma _{a}^{2}/\sigma _{u}^{2})}\right] ^{1/2}.

The variances :math:`\sigma _{a}^{2}` and :math:`\sigma _{u}^{2}` can be estimated after POLS (or
FE) estimation, which then allows us to estimate :math:`\theta ` by :math:`\hat{\theta}`. Then RE
estimate can be obtained from the pooled OLS regression

.. math:: y_{it}-\hat{\theta}\bar{y}_{i}\text{ on }\mathbf{x}_{it}-\hat{\theta}\mathbf{\bar{x}}_{i},t=1,...,T;i=1,...,N.

Call :math:`y_{it}-\hat{\theta}\bar{y}_{i}` a partially-time-demeaned variable: only a fraction of
the mean is removed. Same with the :math:`k` elements of
:math:`\mathbf{x}_{it}-\hat{\theta}\mathbf{\bar{x}}_{i}.`

It is easy to see that

.. math::

   \begin{aligned}
   \hat{\theta} &\approx &0\Rightarrow \mathbf{\hat{\beta}}_{RE}\approx \mathbf{\hat{\beta}}_{POLS} \\
   \hat{\theta} &\approx &1\Rightarrow \mathbf{\hat{\beta}}_{RE}\approx \mathbf{\hat{\beta}}_{FE}\end{aligned}

When is :math:`\theta` close to one? (i) :math:`\sigma _{a}^{2}/\sigma _{u}^{2}` is large or (ii)
:math:`T` is large. With large :math:`T`, FE and RE can be similar.

If :math:`\mathbf{x}_{it}` includes time-constant variables :math:`\mathbf{z}_{i}`, then
:math:`(1-\hat{\theta})\mathbf{z}_{i}` appears as a regressor in the RE estimation. If
:math:`\hat{\theta}=1` (FE) are they eliminated.

No need to do the transformation by hand.  Stata does it.

``xtset id year``

``xtreg y x1 x2 ... xK, re``

``xtreg y x1 x2 ... xK, re cluster(id)``

The first ``xtreg`` command produces the usual, nonrobust inference, the second makes the inference
fully robust.

.. admonition:: Example: Return to Union Membership Using Panel Data

    WAGEPAN.DTA: Working men from 1980 to 1987, so eight years.  :math:`N=545`. Use :math:`lwage` as the
    dependent variable and standard panel data methods.

    Union status and marital status change over time. Education does not. Experience does, but if we
    know the experience in 1980 we know it in any other year (increases by one each year).

    Might be worried about strict exogeneity: Why are union status and marital status changing over
    time? Do shocks to wages contribute?

::

    . use wagepan
     
    . xtset nr year
           panel variable:  nr (strongly balanced)
            time variable:  year, 1980 to 1987
                    delta:  1 unit
     
    . tab year
     
        1980 to |
           1987 |      Freq.     Percent        Cum.
    ------------+-----------------------------------
           1980 |        545       12.50       12.50
           1981 |        545       12.50       25.00
           1982 |        545       12.50       37.50
           1983 |        545       12.50       50.00
           1984 |        545       12.50       62.50
           1985 |        545       12.50       75.00
           1986 |        545       12.50       87.50
           1987 |        545       12.50      100.00
    ------------+-----------------------------------
          Total |      4,360      100.00

::

    . * POLS, with and withour robust SEs:
     
    . reg lwage educ exper union married d81-d87
     
          Source |       SS       df       MS              Number of obs =    4360
    -------------+------------------------------           F( 11,  4348) =   87.24
           Model |  223.568256    11  20.3243869           Prob > F      =  0.0000
        Residual |  1012.96139  4348    .2329718           R-squared     =  0.1808
    -------------+------------------------------           Adj R-squared =  0.1787
           Total |  1236.52964  4359  .283672779           Root MSE      =  .48267
     
    ------------------------------------------------------------------------------
           lwage |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
            educ |     .09161   .0051663    17.73   0.000     .0814814    .1017386
           exper |   .0275251   .0054906     5.01   0.000     .0167607    .0382895
           union |    .175343   .0170619    10.28   0.000     .1418931     .208793
         married |   .1254227   .0155471     8.07   0.000     .0949424     .155903
             d81 |   .0792994   .0297503     2.67   0.008     .0209737     .137625
             d82 |   .1005421   .0312085     3.22   0.001     .0393575    .1617268
             d83 |   .1112672   .0335155     3.32   0.001     .0455597    .1769748
             d84 |   .1471347   .0364646     4.03   0.000     .0756454    .2186239
             d85 |   .1684878   .0399488     4.22   0.000     .0901679    .2468078
             d86 |   .1991507   .0438259     4.54   0.000     .1132297    .2850718
             d87 |   .2245449   .0479651     4.68   0.000      .130509    .3185809
           _cons |   .1652045   .0744227     2.22   0.026     .0192982    .3111109
    ------------------------------------------------------------------------------

::

    . reg lwage educ exper union married d81-d87, cluster(nr)
     
    Linear regression                                      Number of obs =    4360
                                                           F( 11,   544) =   54.17
                                                           Prob > F      =  0.0000
                                                           R-squared     =  0.1808
                                                           Root MSE      =  .48267
     
                                       (Std. Err. adjusted for 545 clusters in nr)
    ------------------------------------------------------------------------------
                 |               Robust
           lwage |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
            educ |     .09161   .0108335     8.46   0.000     .0703294    .1128906
           exper |   .0275251   .0112324     2.45   0.015      .005461    .0495893
           union |    .175343   .0277192     6.33   0.000     .1208933    .2297927
         married |   .1254227   .0256243     4.89   0.000      .075088    .1757573
             d81 |   .0792994   .0272923     2.91   0.004     .0256882    .1329105
             d82 |   .1005421    .034222     2.94   0.003     .0333188    .1677655
             d83 |   .1112672    .042406     2.62   0.009     .0279676    .1945668
             d84 |   .1471347   .0543211     2.71   0.007     .0404298    .2538395
             d85 |   .1684878   .0634103     2.66   0.008     .0439288    .2930468
             d86 |   .1991507   .0739624     2.69   0.007     .0538638    .3444377
             d87 |   .2245449   .0838221     2.68   0.008     .0598903    .3891996
           _cons |   .1652045   .1516309     1.09   0.276    -.1326493    .4630584
    ------------------------------------------------------------------------------
     
    . * Robust SEs quite a bit larger, but everything is statistically significant.
    . * Return to schooling is about 9.2%, union wage premium is about 17.5%.
    . * Marriage premium about 12.5%. Is this causal, or is their a matching story?

::

    . * Now RE. The partial-time-demeaning parameter is about .64, so not close 
    . * to zero and pretty far from one.
     
    . * RE estimates of return to schooling, experience are similar to POLS.
    . * But union and marriage premiums are much smaller. Robust SEs
    . * somewhat larger than nonrobust ones.
    . * Valid to compare POLS robust SEs and RE robust SEs. The latter are smaller
    . * on union and married, so RE appears to be more precise.
     
    . xtreg lwage educ exper union married d81-d87, re theta
     
    Random-effects GLS regression                   Number of obs      =      4360
    Group variable: nr                              Number of groups   =       545
     
    R-sq:  within  = 0.1684                         Obs per group: min =         8
           between = 0.1842                                        avg =       8.0
           overall = 0.1762                                        max =         8
     
                                                    Wald chi2(11)      =    891.30
    corr(u_i, X)   = 0 (assumed)                    Prob > chi2        =    0.0000
    theta          = .6432204
     
    ------------------------------------------------------------------------------
           lwage |      Coef.   Std. Err.      z    P>|z|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
            educ |    .093662   .0105791     8.85   0.000     .0729273    .1143967
           exper |   .0309784   .0111915     2.77   0.006     .0090435    .0529133
           union |   .1075549   .0179179     6.00   0.000     .0724363    .1426734
         married |   .0770573   .0167671     4.60   0.000     .0441945    .1099202
             d81 |   .0806913   .0241898     3.34   0.001     .0332801    .1281025
             d82 |   .1023506   .0309691     3.30   0.001     .0416523    .1630489
             d83 |   .1132246   .0397948     2.85   0.004     .0352283    .1912209
             d84 |   .1485855   .0495472     3.00   0.003     .0514747    .2456962
             d85 |   .1665719   .0598006     2.79   0.005     .0493649     .283779
             d86 |    .194597   .0703264     2.77   0.006     .0567598    .3324343
             d87 |   .2218842   .0809955     2.74   0.006     .0631361    .3806324
           _cons |   .1566514   .1479913     1.06   0.290    -.1334062     .446709
    -------------+----------------------------------------------------------------
         sigma_u |  .32718836
         sigma_e |  .35343397
             rho |  .46149606   (fraction of variance due to u_i)
    ------------------------------------------------------------------------------

::

    . xtreg lwage educ exper union married d81-d87, re theta cluster(nr)
     
    Random-effects GLS regression                   Number of obs      =      4360
    Group variable: nr                              Number of groups   =       545
     
    R-sq:  within  = 0.1684                         Obs per group: min =         8
           between = 0.1842                                        avg =       8.0
           overall = 0.1762                                        max =         8
     
                                                    Wald chi2(11)      =    557.36
    corr(u_i, X)   = 0 (assumed)                    Prob > chi2        =    0.0000
    theta          = .6432204
     
                                       (Std. Err. adjusted for 545 clusters in nr)
    ------------------------------------------------------------------------------
                 |               Robust
           lwage |      Coef.   Std. Err.      z    P>|z|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
            educ |    .093662   .0108436     8.64   0.000     .0724089    .1149151
           exper |   .0309784   .0112562     2.75   0.006     .0089166    .0530402
           union |   .1075549   .0209967     5.12   0.000     .0664022    .1487076
         married |   .0770573    .019067     4.04   0.000     .0396867    .1144279
             d81 |   .0806913   .0272814     2.96   0.003     .0272208    .1341619
             d82 |   .1023506   .0342393     2.99   0.003     .0352428    .1694584
             d83 |   .1132246   .0423645     2.67   0.008     .0301918    .1962574
             d84 |   .1485855   .0542884     2.74   0.006     .0421821    .2549888
             d85 |   .1665719   .0635183     2.62   0.009     .0420784    .2910655
             d86 |    .194597   .0740505     2.63   0.009     .0494606    .3397334
             d87 |   .2218842   .0840108     2.64   0.008     .0572261    .3865423
           _cons |   .1566514   .1522947     1.03   0.304    -.1418408    .4551436
    -------------+----------------------------------------------------------------
         sigma_u |  .32718836
         sigma_e |  .35343397
             rho |  .46149606   (fraction of variance due to u_i)
    ------------------------------------------------------------------------------

::

    . * Now do FE. Two variables are dropped: educ does not change over time for
    . * anyone in the sample. exper must also be dropped because it increases
    . * by one for every year in the sample. The starting points for exper
    . * are different across people but not distinguishable from the fixed effect.
    . * Union and marriage premiums drop even further. FE probably the
    . * most reliable estimates.
     
    . xtreg lwage union married d81-d87, fe
     
    Fixed-effects (within) regression               Number of obs      =      4360
    Group variable: nr                              Number of groups   =       545
     
    R-sq:  within  = 0.1689                         Obs per group: min =         8
           between = 0.0789                                        avg =       8.0
           overall = 0.1026                                        max =         8
     
                                                    F(9,3806)          =     85.95
    corr(u_i, Xb)  = 0.0455                         Prob > F           =    0.0000
     
    ------------------------------------------------------------------------------
           lwage |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
           union |   .0833697   .0194393     4.29   0.000     .0452572    .1214821
         married |   .0583372   .0183688     3.18   0.002     .0223235    .0943509
             d81 |   .1135489   .0214936     5.28   0.000     .0714089    .1556889
             d82 |   .1676693   .0216434     7.75   0.000     .1252356    .2101031
             d83 |   .2109386   .0219471     9.61   0.000     .1679093    .2539678
             d84 |   .2784071   .0221814    12.55   0.000     .2349186    .3218956
             d85 |    .327462   .0223974    14.62   0.000     .2835499    .3713742
             d86 |   .3868075   .0226027    17.11   0.000     .3424929     .431122
             d87 |    .447037   .0228157    19.59   0.000     .4023048    .4917692
           _cons |   1.361709   .0162395    83.85   0.000      1.32987    1.393548
    -------------+----------------------------------------------------------------
         sigma_u |  .38216008
         sigma_e |  .35343397
             rho |  .53899212   (fraction of variance due to u_i)
    ------------------------------------------------------------------------------
    F test that all u_i=0:     F(544, 3806) =     9.14           Prob > F = 0.0000

::

    . xtreg lwage union married d81-d87, fe cluster(nr)
     
    Fixed-effects (within) regression               Number of obs      =      4360
    Group variable: nr                              Number of groups   =       545
     
    R-sq:  within  = 0.1689                         Obs per group: min =         8
           between = 0.0789                                        avg =       8.0
           overall = 0.1026                                        max =         8
     
                                                    F(9,544)           =     48.86
    corr(u_i, Xb)  = 0.0455                         Prob > F           =    0.0000
     
                                       (Std. Err. adjusted for 545 clusters in nr)
    ------------------------------------------------------------------------------
                 |               Robust
           lwage |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
           union |   .0833697   .0230603     3.62   0.000     .0380715    .1286679
         married |   .0583372   .0213374     2.73   0.006     .0164234     .100251
             d81 |   .1135489   .0246191     4.61   0.000     .0651887    .1619091
             d82 |   .1676693   .0242752     6.91   0.000     .1199848    .2153539
             d83 |   .2109386   .0249609     8.45   0.000      .161907    .2599701
             d84 |   .2784071   .0276723    10.06   0.000     .2240495    .3327646
             d85 |    .327462   .0270472    12.11   0.000     .2743322    .3805918
             d86 |   .3868075   .0282988    13.67   0.000     .3312191    .4423959
             d87 |    .447037   .0273812    16.33   0.000     .3932512    .5008227
           _cons |   1.361709   .0203774    66.82   0.000     1.321681    1.401737
    -------------+----------------------------------------------------------------
         sigma_u |  .38216008
         sigma_e |  .35343397
             rho |  .53899212   (fraction of variance due to u_i)
    ------------------------------------------------------------------------------

Choosing Among POLS, FD, FE, and RE
======================================

We have covered four estimators:

1.  POLS, which is on the levels.

2.  FD, which is POLS but on the differences (changes)

3.  FE, which is POLS on the time-demeaned variables

4.  RE, which is POLS on the partially time-demeaned variables

POLS on the levels is usually deficient, unless we include things like lagged :math:`y` (not allowed
in the other methods). With good controls and lags of :math:`y`, we might be able to make a
convincing analysis. But many economists prefer unobserved effects models.

**FD versus FE**

If FD and FE are different in important ways, the strict exogeneity assumption may be violated.
Wooldridge (2010, Chapter 10) shows how to test whether shocks :math:`u_{it}` at time :math:`t` are
correlated with future explanatory variables.

We need :math:`T\geq 3` time periods for this test.

Suppose :math:`w_{it}` is the key policy variable. Consider the expanded equation

.. math::

  y_{it}=\delta _{t}+\beta w_{it}+\mathbf{x}_{it}\mathbf{\gamma }+\psi w_{i,t+1}+a_{i}+u_{it}.

If there is no feedback we should have :math:`\psi =0`. We can test this using FE estimation on time
periods :math:`t=1,2,...,T-1`.

This is not an equation that can be used to estimate the cause effect of :math:`w_{it}`. It is just
used for testing. (An example later.)

**RE versus FE**

Time-constant variables drop out of FE estimation and the estimates may be imprecise.

On the time-varying covariates, are FE and RE so different after all? Define the parameter

.. math::

  \theta =1-\left[ \frac{1}{1+T(\sigma _{c}^{2}/\sigma _{u}^{2})}\right] ^{1/2},

which is consistently estimated (for fixed :math:`T`) by :math:`\hat{\theta}`. The RE estimate can
be obtained from the pooled OLS regression

.. math:: y_{it}-\hat{\theta}\bar{y}_{i}\text{ on
   }\mathbf{x}_{it}-\hat{\theta}\mathbf{\bar{x}}_{i},t=1,...,T;i=1,...,N.

Call :math:`y_{it}-\hat{\theta}\bar{y}_{i}` a quasi-time-demeaned variable: only a fraction of the
mean is removed.

.. math::

  \begin{aligned} \hat{\theta} &\approx &0\Rightarrow \mathbf{\hat{\beta}}_{RE}\approx
  \mathbf{\hat{\beta}}_{POLS} \\ \hat{\theta} &\approx &1\Rightarrow
  \mathbf{\hat{\beta}}_{RE}\approx \mathbf{\hat{\beta}}_{FE}\end{aligned}

:math:`\theta ` increases to unity as (i) :math:`\sigma _{c}^{2}/\sigma _{u}^{2}` increases or (ii)
:math:`T` increases. With large :math:`T`, FE and RE are often similar.

If :math:`\mathbf{x}_{it}` includes time-constant variables :math:`\mathbf{z}_{i}`, then
:math:`(1-\hat{\theta})\mathbf{z}_{i}` appears as a regressor.

Recall the key RE assumption is :math:`Cov(\mathbf{x}_{it},a_{i})=0`. With lots of good
time-constant controls (observed heterogeneity, such as industry dummies) might be able to make this
condition roughly true.

But if RE and FE give estimates that differ in economically important ways, we want evidence that RE
should be rejected in favor of FE. (RE usually gives smaller standard errors; sometimes much
smaller. So we would use it if we can.)

There is a way to directly compare the RE and FE estimators on explanatory variables that change
across :math:`i` and :math:`t`. Called the *Hausman Test*. But test can be cumbersome to compute,
and the simple version of the test is not cluster robust. See Wooldridge (2010, Chapter 10).

A variable addition test is easier and can be made robust.

.. math:: y_{it}=\mathbf{g}_{t}\mathbf{\eta }+\mathbf{z}_{i}\mathbf{\delta
   }+\mathbf{w}_{it}\mathbf{\gamma }+a_{i}+u_{it}

where usually :math:`\mathbf{g}_{t}` is a set of time dummies.

Let :math:`\mathbf{\bar{w}}_{i}` contain the time averages on the variables that change across
:math:`i` and :math:`t`. We compute them for each :math:`i`. To test whether :math:`a_{i}` is
correlated with :math:`\mathbf{\bar{w}}_{i}` we add :math:`\mathbf{\bar{w}}_{i}` as a set of
regressors:

.. math:: y_{it}=\mathbf{g}_{t}\mathbf{\theta }+\mathbf{z}_{i}\mathbf{\delta
   }+\mathbf{w}_{it}\mathbf{\gamma }+\psi +\mathbf{\bar{w}}_{i}\mathbf{\xi }+r_{i}+u_{it}

where :math:`r_{i}` is a new unobserved effect: :math:`a_{i}=\psi +\mathbf{\bar{w}}_{i}\mathbf{\xi
}+r_{i}`.

The expanded equation can be estimated by RE and we can test :math:`\mathbf{\xi =0}`. The estimates
of :math:`\mathbf{\gamma }` are the FE estimates! (A useful check on the procedure.) Including
:math:`\mathbf{\bar{w}}_{i}` effectively proxies for :math:`a_{i}` (even though :math:`r_{i}` is
still in error term).

If we find the :math:`\mathbf{\bar{w}}_{i}` are jointly significant -- use a cluster-robust test! --
then we abandon RE in favor of FE.

When FE and RE are similar, it does not really matter which we choose (although statistical
significance can be an issue, in which case we typically go with RE). When they differ by a lot and
in a statistically significant way, RE is likely to be inappropriate. Harder is when they differ by
a lot practically but are not statistically different. Should we really use RE?

.. admonition:: Example: Does Higher Concentration on U.S. Airline Routes Lead to Higher Air Fares?

    AIREFARE.DTA: :math:`N=1,149` routes (each route is an :math:`i`) over four years :math:`(T=4)`.
    Only other useful (but important) control is route length.

::

    . use airfare
     
    . xtset id year
           panel variable:  id (strongly balanced)
            time variable:  year, 1997 to 2000
                    delta:  1 unit
     
    . tab year
     
    1997, 1998, |
     1999, 2000 |      Freq.     Percent        Cum.
    ------------+-----------------------------------
           1997 |      1,149       25.00       25.00
           1998 |      1,149       25.00       50.00
           1999 |      1,149       25.00       75.00
           2000 |      1,149       25.00      100.00
    ------------+-----------------------------------
          Total |      4,596      100.00
     
    . sum fare concen dist
     
        Variable |       Obs        Mean    Std. Dev.       Min        Max
    -------------+--------------------------------------------------------
            fare |      4596    178.7968    74.88151         37        522
          concen |      4596    .6101149     .196435      .1605          1
            dist |      4596     989.745    611.8315         95       2724

::

    . reg lfare concen ldist ldistsq y98 y99 y00
     
          Source |       SS       df       MS              Number of obs =    4596
    -------------+------------------------------           F(  6,  4589) =  523.18
           Model |  355.453858     6  59.2423096           Prob > F      =  0.0000
        Residual |  519.640516  4589  .113236112           R-squared     =  0.4062
    -------------+------------------------------           Adj R-squared =  0.4054
           Total |  875.094374  4595  .190444913           Root MSE      =  .33651
     
    ------------------------------------------------------------------------------
           lfare |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
          concen |   .3601203   .0300691    11.98   0.000     .3011705    .4190702
           ldist |  -.9016004    .128273    -7.03   0.000    -1.153077   -.6501235
         ldistsq |   .1030196   .0097255    10.59   0.000     .0839529    .1220863
             y98 |   .0211244   .0140419     1.50   0.133    -.0064046    .0486533
             y99 |   .0378496   .0140413     2.70   0.007      .010322    .0653772
             y00 |     .09987   .0140432     7.11   0.000     .0723385    .1274015
           _cons |   6.209258   .4206247    14.76   0.000     5.384631    7.033884
    ------------------------------------------------------------------------------
     
    . * If concen increases by .1 --- a 10 percentage point increase -- lfare
    . * goes up by .036. So a 3.6% increase in fairs, holding route distance
    . * (and year) fixed.
    . * As usual, the SEs cannot be trusted until we cluster.

::

    . reg lfare concen ldist ldistsq y98 y99 y00, cluster(id)
     
                                      (Std. Err. adjusted for 1149 clusters in id)
    ------------------------------------------------------------------------------
                 |               Robust
           lfare |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
          concen |   .3601203    .058556     6.15   0.000     .2452315    .4750092
           ldist |  -.9016004   .2719464    -3.32   0.001    -1.435168   -.3680328
         ldistsq |   .1030196   .0201602     5.11   0.000     .0634647    .1425745
             y98 |   .0211244   .0041474     5.09   0.000     .0129871    .0292617
             y99 |   .0378496   .0051795     7.31   0.000     .0276872     .048012
             y00 |     .09987   .0056469    17.69   0.000     .0887906    .1109493
           _cons |   6.209258   .9117551     6.81   0.000     4.420364    7.998151
    ------------------------------------------------------------------------------

::

    . xtreg lfare concen ldist ldistsq y98 y99 y00, re theta
     
    Random-effects GLS regression                   Number of obs      =      4596
    Group variable: id                              Number of groups   =      1149
     
    R-sq:  within  = 0.1348                         Obs per group: min =         4
           between = 0.4176                                        avg =       4.0
           overall = 0.4030                                        max =         4
     
                                                    Wald chi2(6)       =   1360.42
    corr(u_i, X)   = 0 (assumed)                    Prob > chi2        =    0.0000
    theta          = .83550226
     
    ------------------------------------------------------------------------------
           lfare |      Coef.   Std. Err.      z    P>|z|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
          concen |   .2089935   .0265297     7.88   0.000     .1569962    .2609907
           ldist |  -.8520921   .2464836    -3.46   0.001    -1.335191   -.3689931
         ldistsq |   .0974604   .0186358     5.23   0.000     .0609348     .133986
             y98 |   .0224743   .0044544     5.05   0.000     .0137438    .0312047
             y99 |   .0366898   .0044528     8.24   0.000     .0279626    .0454171
             y00 |    .098212   .0044576    22.03   0.000     .0894752    .1069487
           _cons |   6.222005   .8099666     7.68   0.000       4.6345     7.80951
    -------------+----------------------------------------------------------------
         sigma_u |  .31933841
         sigma_e |  .10651186
             rho |  .89988885   (fraction of variance due to u_i)
    ------------------------------------------------------------------------------
     
    . * theta = .836, so we expect RE and FE to be similar.

::

    . * The coefficient on the time-varying variable concen drops quite a bit. 
    . * Notice that the RE and POLS coefficients on the time-constant 
    . * distance variables are pretty similar, something that often occurs.
     
    . xtreg lfare concen ldist ldistsq y98 y99 y00, re cluster(id)
     
                                      (Std. Err. adjusted for 1149 clusters in id)
    ------------------------------------------------------------------------------
                 |               Robust
           lfare |      Coef.   Std. Err.      z    P>|z|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
          concen |   .2089935   .0422459     4.95   0.000      .126193    .2917939
           ldist |  -.8520921   .2720902    -3.13   0.002    -1.385379   -.3188051
         ldistsq |   .0974604   .0201417     4.84   0.000     .0579833    .1369375
             y98 |   .0224743   .0041461     5.42   0.000      .014348    .0306005
             y99 |   .0366898   .0051318     7.15   0.000     .0266317     .046748
             y00 |    .098212   .0055241    17.78   0.000     .0873849     .109039
           _cons |   6.222005   .9144067     6.80   0.000     4.429801    8.014209
    -------------+----------------------------------------------------------------
         sigma_u |  .31933841
         sigma_e |  .10651186
             rho |  .89988885   (fraction of variance due to u_i)
    ------------------------------------------------------------------------------
     
    . * Robust standard error on concen is quite a bit larger.

::

    . * What if we do not control for distance in RE? Should control for important
    . * time-constant variables in RE!
     
    . xtreg lfare concen y98 y99 y00, re cluster(id)
     
    Random-effects GLS regression                   Number of obs      =      4596
    Group variable: id                              Number of groups   =      1149
     
     
                                      (Std. Err. adjusted for 1149 clusters in id)
    ------------------------------------------------------------------------------
                 |               Robust
           lfare |      Coef.   Std. Err.      z    P>|z|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
          concen |   .0468181   .0427562     1.09   0.274    -.0369826    .1306188
             y98 |   .0239229   .0041907     5.71   0.000     .0157093    .0321364
             y99 |   .0354453   .0051678     6.86   0.000     .0253167     .045574
             y00 |   .0964328   .0055197    17.47   0.000     .0856144    .1072511
           _cons |   5.028086   .0285248   176.27   0.000     4.972178    5.083993
    -------------+----------------------------------------------------------------
         sigma_u |  .40942871
         sigma_e |  .10651186
             rho |  .93661309   (fraction of variance due to u_i)
    ------------------------------------------------------------------------------
     
    . * The RE estimate on concen is now much smaller than when ldist 
    . * and ldistsq are controlled for, and much smaller than the FE estimate.
    . * Thus, it can be very harmful to omit time-constant variables in 
    . * RE estimation.

::

    . * See what happens if we leave the distance variables in FE:
     
    . xtreg lfare concen ldist ldistsq y98 y99 y00, fe
     
    Fixed-effects (within) regression               Number of obs      =      4596
    Group variable: id                              Number of groups   =      1149
     
    R-sq:  within  = 0.1352                         Obs per group: min =         4
           between = 0.0576                                        avg =       4.0
           overall = 0.0083                                        max =         4
     
                                                    F(4,3443)          =    134.61
    corr(u_i, Xb)  = -0.2033                        Prob > F           =    0.0000
     
    ------------------------------------------------------------------------------
           lfare |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
          concen |    .168859   .0294101     5.74   0.000     .1111959     .226522
           ldist |  (dropped)
         ldistsq |  (dropped)
             y98 |   .0228328   .0044515     5.13   0.000     .0141048    .0315607
             y99 |   .0363819   .0044495     8.18   0.000     .0276579    .0451058
             y00 |   .0977717   .0044555    21.94   0.000      .089036    .1065073
           _cons |   4.953331   .0182869   270.87   0.000     4.917476    4.989185
    -------------+----------------------------------------------------------------
         sigma_u |  .43389176
         sigma_e |  .10651186
             rho |  .94316439   (fraction of variance due to u_i)
    ------------------------------------------------------------------------------
    F test that all u_i=0:     F(1148, 3443) =    36.90          Prob > F = 0.0000

 

::

    . xtreg lfare concen ldist ldistsq y98 y99 y00, fe cluster(id)
     
                                      (Std. Err. adjusted for 1149 clusters in id)
    ------------------------------------------------------------------------------
                 |               Robust
           lfare |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
          concen |    .168859   .0494587     3.41   0.001     .0718194    .2658985
           ldist |  (dropped)
         ldistsq |  (dropped)
             y98 |   .0228328    .004163     5.48   0.000     .0146649    .0310007
             y99 |   .0363819   .0051275     7.10   0.000     .0263215    .0464422
             y00 |   .0977717   .0055054    17.76   0.000     .0869698    .1085735
           _cons |   4.953331   .0296765   166.91   0.000     4.895104    5.011557
    -------------+----------------------------------------------------------------
         sigma_u |  .43389176
         sigma_e |  .10651186
             rho |  .94316439   (fraction of variance due to u_i)
    ------------------------------------------------------------------------------
     
    . * The FE effect of concen (.169) is less than half of the POLS effect (.360).
    . * The FE estimate is still statistically significant with fully robust SE.

::

    . * RE and FE are not too far apart, but are they statistically different?
    . * We need the time average of concen: concenbar, say.
     
    . egen concenbar = mean(concen), by(id)
     
    . list id year concen concenbar in 1/20
     
         +-------------------------------+
         | id   year   concen   concen~r |
         |-------------------------------|
      1. |  1   1997    .8386    .834825 |
      2. |  1   1998    .8133    .834825 |
      3. |  1   1999    .8262    .834825 |
      4. |  1   2000    .8612    .834825 |
      5. |  2   1997    .5798       .608 |
         |-------------------------------|
      6. |  2   1998    .5817       .608 |
      7. |  2   1999    .7319       .608 |
      8. |  2   2000    .5386       .608 |
      9. |  3   1997     .818    .786175 |
     10. |  3   1998    .8172    .786175 |
         |-------------------------------|
     11. |  3   1999    .7998    .786175 |
     12. |  3   2000    .7097    .786175 |
     13. |  4   1997    .4604      .4317 |
     14. |  4   1998    .4614      .4317 |
     15. |  4   1999    .4334      .4317 |
         |-------------------------------|
     16. |  4   2000    .3716      .4317 |
     17. |  5   1997    .4571      .5102 |
     18. |  5   1998    .5632      .5102 |
     19. |  5   1999    .5008      .5102 |
     20. |  5   2000    .5197      .5102 |
         +-------------------------------+

::

    . xtreg lfare concen concenbar ldist ldistsq y98 y99 y00, re cluster(id)
     
                                      (Std. Err. adjusted for 1149 clusters in id)
    ------------------------------------------------------------------------------
                 |               Robust
           lfare |      Coef.   Std. Err.      z    P>|z|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
          concen |    .168859   .0494749     3.41   0.001       .07189    .2658279
       concenbar |   .2136346   .0816403     2.62   0.009     .0536227    .3736466
           ldist |  -.9089297   .2721637    -3.34   0.001    -1.442361   -.3754987
         ldistsq |   .1038426   .0201911     5.14   0.000     .0642688    .1434164
             y98 |   .0228328   .0041643     5.48   0.000     .0146708    .0309947
             y99 |   .0363819   .0051292     7.09   0.000     .0263289    .0464349
             y00 |   .0977717   .0055072    17.75   0.000     .0869777    .1085656
           _cons |   6.207889   .9118109     6.81   0.000     4.420773    7.995006
    -------------+----------------------------------------------------------------
         sigma_u |  .31933841
         sigma_e |  .10651186
             rho |  .89988885   (fraction of variance due to u_i)
    ------------------------------------------------------------------------------
     
    . * So the robust t statistic is 2.62 --- a rejection of RE.
    . * Coefficient on concenbar is larger than that on concen. Is this picking
    . * up persistent effects?

**Computer Exercises**

Use AIRFARE.DTA to test whether there is feedback from fair shocks to future concentration rate.
Also, allow :math:`concen` to interact with :math:`ldist` in FE estimation.

::

     
     
    . * Create the lead value (next period's value) of concen:
     
    . sort id year
     
    . gen concenp1 = concen[_n+1] if year < 2000
     
    . xtreg lfare concen concenp1 y98 y99 y00, fe cluster(id)
     
    Fixed-effects (within) regression               Number of obs      =      3447
    Group variable: id                              Number of groups   =      1149
     
    R-sq:  within  = 0.0558                         Obs per group: min =         3
           between = 0.0535                                        avg =       3.0
           overall = 0.0347                                        max =         3
     
                                                    F(4,1148)          =     25.63
    corr(u_i, Xb)  = -0.2949                        Prob > F           =    0.0000
     
                                      (Std. Err. adjusted for 1149 clusters in id)
    ------------------------------------------------------------------------------
                 |               Robust
           lfare |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
          concen |   .2983988    .054797     5.45   0.000     .1908854    .4059122
        concenp1 |  -.0659259   .0467578    -1.41   0.159    -.1576663    .0258145
             y98 |   .0205809   .0042341     4.86   0.000     .0122735    .0288883
             y99 |   .0360638   .0050754     7.11   0.000     .0261058    .0460218
             y00 |  (dropped)
           _cons |   4.914953   .0478488   102.72   0.000     4.821072    5.008834
    -------------+----------------------------------------------------------------
     
    . * Lose the last year, that is why y00 dropped. Not statistically significant.
    . * But positive shock to fare might be expected to reduce next year's
    . * concentration ratio (market entry).

::

    . * Let the effect of concen depend on route distance. First center
    . * ldist about its mean so coefficient on concen is effect at 
    . * the mean value of ldist:
     
    . sum ldist if y00
     
        Variable |       Obs        Mean    Std. Dev.       Min        Max
    -------------+--------------------------------------------------------
           ldist |      1149    6.696482    .6595331   4.553877   7.909857
     
    . gen ldistconcen = (ldist - 6.7)*concen

::

    . xtreg lfare concen ldistconcen y98 y99 y00, fe cluster(id)
     
    Fixed-effects (within) regression               Number of obs      =      4596
    Group variable: id                              Number of groups   =      1149
     
                                      (Std. Err. adjusted for 1149 clusters in id)
    ------------------------------------------------------------------------------
                 |               Robust
           lfare |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
          concen |   .1652538   .0482782     3.42   0.001     .0705304    .2599771
     ldistconcen |  -.2498619   .0828545    -3.02   0.003    -.4124251   -.0872987
             y98 |   .0230874   .0041459     5.57   0.000      .014953    .0312218
             y99 |   .0355923   .0051452     6.92   0.000     .0254972    .0456874
             y00 |   .0975745   .0054655    17.85   0.000     .0868511    .1082979
           _cons |    4.93797   .0317998   155.28   0.000     4.875578    5.000362
    -------------+----------------------------------------------------------------
         sigma_u |  .50598296
         sigma_e |  .10605257
             rho |  .95791776   (fraction of variance due to u_i)
    ------------------------------------------------------------------------------

::

    . * Effect at the average of ldist is similar to before: .165. But at 
    . * one standard deviation of ldist above its mean, 
    . * the effect of concen is zero:
     
    . lincom concen + .66*ldistconcen
     
     ( 1)  concen + .66 ldistconcen = 0
     
    ------------------------------------------------------------------------------
           lfare |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
             (1) |   .0003449   .0554442     0.01   0.995    -.1084383    .1091281
    ------------------------------------------------------------------------------
     
    . count if ldist > 6.7 + .66 & y00
      209
     
    . di 209/1149
    .1818973
     
    . * So about 18.2* of the routes of ldist greater than one standard deviation
    . * above the mean.
    . * This is a puzzle.

Use WAGEPAN.DTA to test for interactive effects between union and marital status. Check each for
violation of strict exogeneity.

::

     
    . * No evidence of an interactive effect.
     
    . gen unionmarried = union*married
     
    . xtreg lwage union married unionmarried d81-d87, fe cluster(nr)
     
    Fixed-effects (within) regression               Number of obs      =      4360
    Group variable: nr                              Number of groups   =       545
     
    R-sq:  within  = 0.1691                         Obs per group: min =         8
           between = 0.0826                                        avg =       8.0
           overall = 0.1033                                        max =         8
     
                                                    F(10,544)          =     44.23
    corr(u_i, Xb)  = 0.0470                         Prob > F           =    0.0000
     
                                       (Std. Err. adjusted for 545 clusters in nr)
    ------------------------------------------------------------------------------
                 |               Robust
           lwage |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
           union |   .0963696   .0292893     3.29   0.001     .0388357    .1539035
         married |   .0653697    .023527     2.78   0.006     .0191547    .1115847
    unionmarried |  -.0292845   .0377635    -0.78   0.438    -.1034645    .0448956
             d81 |   .1134412   .0246364     4.60   0.000     .0650471    .1618354
             d82 |   .1681043   .0242194     6.94   0.000     .1205293    .2156793
             d83 |   .2107231   .0249842     8.43   0.000     .1616459    .2598004
             d84 |   .2786594   .0276848    10.07   0.000     .2242771    .3330416
             d85 |   .3277345   .0270551    12.11   0.000     .2745893    .3808797
             d86 |   .3867868   .0282917    13.67   0.000     .3312126    .4423611
             d87 |   .4472058   .0273797    16.33   0.000      .393423    .5009887
           _cons |   1.358749    .020698    65.65   0.000     1.318092    1.399407
    -------------+----------------------------------------------------------------
         sigma_u |  .38194161
         sigma_e |   .3534425
             rho |  .53869593   (fraction of variance due to u_i)
    ------------------------------------------------------------------------------
     
    . gen unioneduc = union*educ
     
    . xtreg lwage union married unionmarried unioneduc d81-d87, fe cluster(nr)
     
    Fixed-effects (within) regression               Number of obs      =      4360
    Group variable: nr                              Number of groups   =       545
     
    R-sq:  within  = 0.1691                         Obs per group: min =         8
           between = 0.0784                                        avg =       8.0
           overall = 0.1024                                        max =         8
     
                                                    F(11,544)          =     40.88
    corr(u_i, Xb)  = 0.0450                         Prob > F           =    0.0000
     
                                       (Std. Err. adjusted for 545 clusters in nr)
    ------------------------------------------------------------------------------
                 |               Robust
           lwage |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
           union |   .1342275   .1649136     0.81   0.416    -.1897179    .4581729
         married |   .0653673   .0235243     2.78   0.006     .0191577    .1115768
    unionmarried |  -.0292705    .037774    -0.77   0.439    -.1034713    .0449302
       unioneduc |  -.0032691   .0144986    -0.23   0.822    -.0317492     .025211
             d81 |   .1135646      .0247     4.60   0.000     .0650457    .1620836
             d82 |   .1681954   .0242755     6.93   0.000     .1205102    .2158807
             d83 |   .2107695   .0250217     8.42   0.000     .1616185    .2599205
             d84 |    .278629   .0276635    10.07   0.000     .2242886    .3329693
             d85 |   .3277924   .0271009    12.10   0.000     .2745571    .3810277
             d86 |   .3868873    .028373    13.64   0.000     .3311532    .4426215
             d87 |   .4472384   .0274036    16.32   0.000     .3934086    .5010682
           _cons |    1.35883   .0206775    65.72   0.000     1.318212    1.399448
    -------------+----------------------------------------------------------------
         sigma_u |  .38225893
         sigma_e |  .35348574
             rho |  .53904787   (fraction of variance due to u_i)
    ------------------------------------------------------------------------------

::

    . * Now test for feedback:
     
    . sort nr year
     
    . gen union_p1 = union[_n+1] if year < 1987
    (545 missing values generated)
     
    . gen married_p1 = married[_n+1] if year < 1987
    (545 missing values generated)
     
    . xtreg lwage union union_p1 married d81-d87, fe cluster(nr)
    note: d87 omitted because of collinearity
     
    Fixed-effects (within) regression               Number of obs      =      3815
    Group variable: nr                              Number of groups   =       545
     
    R-sq:  within  = 0.1373                         Obs per group: min =         7
           between = 0.0786                                        avg =       7.0
           overall = 0.0931                                        max =         7
     
                                                    F(9,544)           =     33.46
    corr(u_i, Xb)  = 0.0559                         Prob > F           =    0.0000
     
                                       (Std. Err. adjusted for 545 clusters in nr)
    ------------------------------------------------------------------------------
                 |               Robust
           lwage |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
           union |    .079371   .0238769     3.32   0.001     .0324689    .1262732
        union_p1 |   .0501971   .0230545     2.18   0.030     .0049103    .0954839
         married |   .0534979   .0238827     2.24   0.025     .0065842    .1004116
             d81 |   .1136704    .024587     4.62   0.000     .0653734    .1619674
             d82 |   .1687102    .024352     6.93   0.000     .1208748    .2165457
             d83 |   .2120942   .0250733     8.46   0.000     .1628418    .2613466
             d84 |   .2812238   .0279498    10.06   0.000     .2263211    .3361266
             d85 |   .3310088   .0271571    12.19   0.000     .2776632    .3843543
             d86 |   .3878926   .0286425    13.54   0.000     .3316292    .4441561
             d87 |          0  (omitted)
           _cons |   1.351084   .0209037    64.63   0.000     1.310023    1.392146
    -------------+----------------------------------------------------------------
         sigma_u |  .38606516
         sigma_e |  .35945821
             rho |  .53564356   (fraction of variance due to u_i)
    ------------------------------------------------------------------------------
     
     * Looks like union status not strictly exogenous.

::

    . * Marital status looks better:
     
    . xtreg lwage union union_p1 married married_p1 d81-d87, fe cluster(nr)
    note: d87 omitted because of collinearity
     
    Fixed-effects (within) regression               Number of obs      =      3815
    Group variable: nr                              Number of groups   =       545
     
    R-sq:  within  = 0.1373                         Obs per group: min =         7
           between = 0.0806                                        avg =       7.0
           overall = 0.0942                                        max =         7
     
                                                    F(10,544)          =     30.18
    corr(u_i, Xb)  = 0.0574                         Prob > F           =    0.0000
     
                                       (Std. Err. adjusted for 545 clusters in nr)
    ------------------------------------------------------------------------------
                 |               Robust
           lwage |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
           union |   .0796327   .0238623     3.34   0.001     .0327593    .1265062
        union_p1 |   .0501839   .0230552     2.18   0.030     .0048958     .095472
         married |   .0496759   .0250147     1.99   0.048     .0005388    .0988131
      married_p1 |   .0093408   .0224252     0.42   0.677    -.0347099    .0533914
             d81 |   .1134124   .0247327     4.59   0.000     .0648291    .1619957
             d82 |   .1678769   .0247866     6.77   0.000     .1191877     .216566
             d83 |   .2111104   .0255618     8.26   0.000     .1608984    .2613223
             d84 |   .2800645   .0284265     9.85   0.000     .2242254    .3359036
             d85 |   .3296851   .0275695    11.96   0.000     .2755293    .3838409
             d86 |   .3863464   .0293591    13.16   0.000     .3286754    .4440174
             d87 |          0  (omitted)
           _cons |   1.349039   .0207109    65.14   0.000     1.308356    1.389723
    -------------+----------------------------------------------------------------
         sigma_u |  .38569545
         sigma_e |  .35950418
             rho |  .53510329   (fraction of variance due to u_i)
    ------------------------------------------------------------------------------
     
    . * So would have to move beyond FE to be completely convincing. More
    . * advanced methods involve FD and IV.

.. admonition:: Example: Effects of Traffic Laws on Driving Fatalities (DRIVING.DTA)

    State-level data over a 25-year period. Only the 48 Continental states.

    Look at blood alcohol limits, administrative per se laws (license can be revoked by an arresting
    officer), speed limit laws. Include various controls but most importantly allow for systematic
    differences across states via FD or FE.

    Probably lots of serial correlation in shocks, so use cluster-robust inference.

::

    . use driving
     
    . xtset state year
           panel variable:  state (strongly balanced)
            time variable:  year, 1980 to 2004
                    delta:  1 unit
     
    . des totfatrte bac08 bac10 perse sbprim sbsecon sl70plus gdl per14_24 unem vehicmiles
     
                  storage  display     value
    variable name   type   format      label      variable label
    -------------------------------------------------------------------------------------------------------------------------
    totfatrte       float  %9.0g                  total fatalities per 100,000 population
    bac08           float  %9.0g                  blood alcohol limit .08
    bac10           float  %9.0g                  blood alcohol limit .10
    perse           float  %9.0g                  administrative license revocation (per se law)
    sbprim          byte   %9.0g                  =1 if primary seatbelt law
    sbsecon         byte   %9.0g                  =1 if secondary seatbelt law
    sl70plus        float  %9.0g                  sl70 + sl75 + slnone
    gdl             float  %9.0g                  graduated drivers license law
    per14_24        float  %9.0g                  percent population aged 14 through 24
    unem            float  %9.0g                  unemployment rate, percent
    vehicmiles      float  %9.0g                  vehicle miles traveled, billions

::

    . tab year
     
           1980 |
        through |
           2004 |      Freq.     Percent        Cum.
    ------------+-----------------------------------
           1980 |         48        4.00        4.00
           1981 |         48        4.00        8.00
           1982 |         48        4.00       12.00
           1983 |         48        4.00       16.00
           1984 |         48        4.00       20.00
           1985 |         48        4.00       24.00
           1986 |         48        4.00       28.00
          .
          .
          .
           1996 |         48        4.00       68.00
           1997 |         48        4.00       72.00
           1998 |         48        4.00       76.00
           1999 |         48        4.00       80.00
           2000 |         48        4.00       84.00
           2001 |         48        4.00       88.00
           2002 |         48        4.00       92.00
           2003 |         48        4.00       96.00
           2004 |         48        4.00      100.00
    ------------+-----------------------------------
          Total |      1,200      100.00

::

    . tab bac08 if year == 2004
     
          blood |
        alcohol |
      limit .08 |      Freq.     Percent        Cum.
    ------------+-----------------------------------
              0 |          1        2.08        2.08
             .5 |          2        4.17        6.25
           .667 |          1        2.08        8.33
              1 |         44       91.67      100.00
    ------------+-----------------------------------
          Total |         48      100.00
     
    . * This is fraction of year the law was in effect.
     
    . tab bac10 if year == 2004
     
          blood |
        alcohol |
      limit .10 |      Freq.     Percent        Cum.
    ------------+-----------------------------------
              0 |         44       91.67       91.67
           .333 |          1        2.08       93.75
             .5 |          2        4.17       97.92
              1 |          1        2.08      100.00
    ------------+-----------------------------------
          Total |         48      100.00
     
    . tab perse if year == 2004
     
    administrat |
    ive license |
     revocation |
        (per se |
           law) |      Freq.     Percent        Cum.
    ------------+-----------------------------------
              0 |          9       18.75       18.75
              1 |         39       81.25      100.00
    ------------+-----------------------------------
          Total |         48      100.00
     
    . tab sl70plus if year == 2004
     
    sl70 + sl75 |
       + slnone |      Freq.     Percent        Cum.
    ------------+-----------------------------------
              0 |         19       39.58       39.58
              1 |         29       60.42      100.00
    ------------+-----------------------------------
          Total |         48      100.00

::

    . * First POLS, with and without cluster-robust inference:
     
    . reg totfatrte bac08 bac10 perse sbprim sbsecon sl70plus gdl per14_24 unem 
          vehicmiles d81-d04
     
          Source |       SS       df       MS              Number of obs =    1200
    -------------+------------------------------           F( 34,  1165) =   16.78
           Model |  15977.3384    34  469.921719           Prob > F      =  0.0000
        Residual |  32634.7683  1165  28.0126766           R-squared     =  0.3287
    -------------+------------------------------           Adj R-squared =  0.3091
           Total |  48612.1067  1199  40.5438755           Root MSE      =  5.2927
     
    ------------------------------------------------------------------------------
       totfatrte |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
           bac08 |  -1.757702   .7088547    -2.48   0.013    -3.148477   -.3669278
           bac10 |  -1.580243   .5186078    -3.05   0.002    -2.597753    -.562733
           perse |   .1628215   .3941308     0.41   0.680     -.610464    .9361071
          sbprim |    .540878   .6863497     0.79   0.431    -.8057417    1.887498
         sbsecon |   .2608077   .5693998     0.46   0.647    -.8563559    1.377971
        sl70plus |   6.681484   .5734641    11.65   0.000     5.556346    7.806622
             gdl |  -1.631779   .6908841    -2.36   0.018    -2.987296   -.2762631
        per14_24 |    .710293    .158478     4.48   0.000     .3993587    1.021227
            unem |   .5086961   .1019875     4.99   0.000     .3085964    .7087957
      vehicmiles |  -.0314786   .0037613    -8.37   0.000    -.0388584   -.0240988
             d81 |  -1.797262   1.082621    -1.66   0.097    -3.921367    .3268441
             d82 |  -5.108976   1.114603    -4.58   0.000     -7.29583   -2.922121
             .
             .
             .
             d03 |  -5.896365   1.705343    -3.46   0.001    -9.242252   -2.550479
             d04 |   -5.53404   1.742148    -3.18   0.002    -8.952139   -2.115941
           _cons |   10.10243   3.220842     3.14   0.002     3.783133    16.42173
    ------------------------------------------------------------------------------
     
    . * bac08, bac10 have right signs and are statistically significant, but 
    . * standard errors cannot be trusted. perse and seat belt laws of "wrong"
    . * signs.
    . * The speed limit variable has the "right" sign and is very significant.

::

    . reg totfatrte bac08 bac10 perse sbprim sbsecon sl70plus gdl per14_24 unem 
          vehicmiles d81-d04, cluster(state)
     
    Linear regression                                      Number of obs =    1200
                                                           F( 34,    47) =   26.18
                                                           Prob > F      =  0.0000
                                                           R-squared     =  0.3287
                                                           Root MSE      =  5.2927
     
                                     (Std. Err. adjusted for 48 clusters in state)
    ------------------------------------------------------------------------------
                 |               Robust
       totfatrte |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
           bac08 |  -1.757702   2.378517    -0.74   0.464    -6.542662    3.027257
           bac10 |  -1.580243   1.958837    -0.81   0.424    -5.520915    2.360429
           perse |   .1628215   1.245421     0.13   0.897    -2.342642    2.668285
          sbprim |    .540878   1.964398     0.28   0.784     -3.41098    4.492737
         sbsecon |   .2608077   1.122508     0.23   0.817    -1.997388    2.519003
        sl70plus |   6.681484   1.142452     5.85   0.000     4.383167      8.9798
             gdl |  -1.631779   1.434345    -1.14   0.261    -4.517309     1.25375
        per14_24 |    .710293   .6244481     1.14   0.261    -.5459347    1.966521
            unem |   .5086961    .286826     1.77   0.083    -.0683234    1.085716
      vehicmiles |  -.0314786    .011718    -2.69   0.010    -.0550522    -.007905
             d81 |  -1.797262   .4748603    -3.78   0.000    -2.752557    -.841966
             d82 |  -5.108976   .8242987    -6.20   0.000    -6.767251   -3.450701
             .
             .
             .
             d03 |  -5.896365   4.296381    -1.37   0.176    -14.53957    2.746838
             d04 |   -5.53404   4.346451    -1.27   0.209    -14.27797    3.209891
           _cons |   10.10243   12.61279     0.80   0.427    -15.27124     35.4761
    ------------------------------------------------------------------------------
     
    . * With robust ses, only sl70plus is significant among policy variables.

::

    . * Now control for state fixed effects:
     
    . xtreg totfatrte bac08 bac10 perse sbprim sbsecon sl70plus gdl per14_24 unem 
            vehicmiles d81-d04, fe
     
    Fixed-effects (within) regression               Number of obs      =      1200
    Group variable: state                           Number of groups   =        48
     
    R-sq:  within  = 0.6026                         Obs per group: min =        25
     
    ------------------------------------------------------------------------------
       totfatrte |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
           bac08 |  -1.098128   .4051397    -2.71   0.007    -1.893048   -.3032085
           bac10 |  -.8347651   .2759032    -3.03   0.003    -1.376112   -.2934186
           perse |  -1.130769   .2420146    -4.67   0.000    -1.605623   -.6559154
          sbprim |  -1.210998   .3562968    -3.40   0.001    -1.910084    -.511912
         sbsecon |  -.2596397   .2600448    -1.00   0.318    -.7698705    .2505911
        sl70plus |   .1997288   .2799845     0.71   0.476    -.3496255    .7490831
             gdl |  -.5626811   .3026503    -1.86   0.063    -1.156508    .0311455
        per14_24 |   .2663101   .0977176     2.73   0.007     .0745796    .4580406
            unem |  -.6915953   .0607696   -11.38   0.000    -.8108305     -.57236
      vehicmiles |  -.0063192   .0065989    -0.96   0.338    -.0192668    .0066284
             d81 |  -1.407696   .4258959    -3.31   0.001    -2.243341   -.5720506
             d82 |  -2.542338   .4525107    -5.62   0.000    -3.430204   -1.654472
             d83 |  -2.877553   .4648788    -6.19   0.000    -3.789687    -1.96542
             .
             .
             .
             d03 |  -5.526686   .8240642    -6.71   0.000    -7.143572   -3.909799
             d04 |  -5.904508   .8470631    -6.97   0.000     -7.56652   -4.242495
           _cons |   25.66702    1.98907    12.90   0.000     21.76429    29.56975
    -------------+----------------------------------------------------------------
         sigma_u |  5.7529904
         sigma_e |  2.0767987
             rho |  .88470746   (fraction of variance due to u_i)
    ------------------------------------------------------------------------------
    F test that all u_i=0:     F(47, 1118) =   137.20            Prob > F = 0.0000
     
    . * Now all policy variables have the "correct" sign and only sl70plus and 
    . * sbsecon are insignificant.
     
    . * But again, we can't really trust the standard errors.

::

    . xtreg totfatrte bac08 bac10 perse sbprim sbsecon sl70plus gdl per14_24 unem 
            vehicmiles d81-d04, fe cluster(state)
     
    Fixed-effects (within) regression               Number of obs      =      1200
    Group variable: state                           Number of groups   =        48
     
    R-sq:  within  = 0.6026                         Obs per group: min =        25
     
                                     (Std. Err. adjusted for 48 clusters in state)
    ------------------------------------------------------------------------------
                 |               Robust
       totfatrte |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
           bac08 |  -1.098128   .8259801    -1.33   0.190    -2.759786    .5635294
           bac10 |  -.8347651   .4880785    -1.71   0.094    -1.816652    .1471222
           perse |  -1.130769   .4229467    -2.67   0.010    -1.981628   -.2799104
          sbprim |  -1.210998   .6192596    -1.96   0.056    -2.456787    .0347919
         sbsecon |  -.2596397   .3874532    -0.67   0.506    -1.039095    .5198157
        sl70plus |   .1997288   .6345107     0.31   0.754    -1.076742      1.4762
             gdl |  -.5626811   .4460789    -1.26   0.213    -1.460076    .3347138
        per14_24 |   .2663101   .2046722     1.30   0.200    -.1454373    .6780575
            unem |  -.6915953   .1158565    -5.97   0.000    -.9246684   -.4585221
      vehicmiles |  -.0063192   .0127209    -0.50   0.622    -.0319103    .0192719
             d81 |  -1.407696   .4526196    -3.11   0.003    -2.318249   -.4971428
             d82 |  -2.542338   .5159949    -4.93   0.000    -3.580386    -1.50429
             d83 |  -2.877553    .552767    -5.21   0.000    -3.989577    -1.76553
             .
             .
             .
             d03 |  -5.526686   1.141777    -4.84   0.000    -7.823644   -3.229728
             d04 |  -5.904508   1.214393    -4.86   0.000    -8.347552   -3.461464
           _cons |   25.66702   3.788567     6.77   0.000      18.0454    33.28863
    -------------+----------------------------------------------------------------
         sigma_u |  5.7529904
         sigma_e |  2.0767987
             rho |  .88470746   (fraction of variance due to u_i)
    ------------------------------------------------------------------------------
     
    . * The standard errors that allow for arbitrary time series correlation are
    . * almost twice as large as usual standard errors. But some things are 
    . * still marginally significant.

::

    . * Now use first differencing. Suffices to put "d." outside the entire
    . * list of variables. A year is dropped and a constant included.
     
    . * Effects are reasonably similar to FE.
     
    . reg d.(totfatrte bac08 bac10 perse sbprim sbsecon sl70plus gdl per14_24 
          unem vehicmiles d81-d04)
    note: _delete omitted because of collinearity
     
          Source |       SS       df       MS              Number of obs =    1152
    -------------+------------------------------           F( 33,  1118) =    7.11
           Model |  797.312107    33  24.1609729           Prob > F      =  0.0000
        Residual |  3797.99123  1118  3.39712991           R-squared     =  0.1735
    -------------+------------------------------           Adj R-squared =  0.1491
           Total |  4595.30334  1151  3.99244426           Root MSE      =  1.8431
     
    ------------------------------------------------------------------------------
     D.totfatrte |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
           bac08 |
             D1. |  -.8612566   .5879443    -1.46   0.143    -2.014855     .292342
                 |
           bac10 |
             D1. |  -1.050576   .4456376    -2.36   0.019    -1.924956   -.1761956
                 |
           perse |
             D1. |  -.6272044   .3903663    -1.61   0.108    -1.393138    .1387288
                 |
          sbprim |
             D1. |   -.346693   .4831841    -0.72   0.473    -1.294743    .6013569
                 |
         sbsecon |
             D1. |  -.3303991   .2961221    -1.12   0.265    -.9114168    .2506186
                 |
        sl70plus |
             D1. |   .3817811   .5650386     0.68   0.499    -.7268744    1.490437
                 |
             gdl |
             D1. |  -.1923185   .3705857    -0.52   0.604    -.9194404    .5348034
                 |
        per14_24 |
             D1. |   .9581163   .3115071     3.08   0.002      .346912    1.569321
                 |
            unem |
             D1. |  -.2338551    .080942    -2.89   0.004    -.3926703   -.0750398
                 |
      vehicmiles |
             D1. |  -.0115343   .0294985    -0.39   0.696     -.069413    .0463443
                 |
             d81 |
             D1. |  -1.246487   .2704865    -4.61   0.000    -1.777205   -.7157687
                 |
             d82 |
             D1. |  -2.848866   .4377456    -6.51   0.000    -3.707761    -1.98997
                 |
             d83 |
             D1. |  -2.744412   .5353472    -5.13   0.000     -3.79481   -1.694013
                 |
             d84 |
             D1. |  -2.236782   .6004855    -3.72   0.000    -3.414988   -1.058577
                 |
             d85 |
             D1. |  -2.076031   .6713311    -3.09   0.002    -3.393242   -.7588204
                 |
             d86 |
             D1. |  -.4884557   .7330563    -0.67   0.505    -1.926777    .9498653
                 |
             d87 |
             D1. |  -.0483105   .8055603    -0.06   0.952    -1.628891     1.53227
                 |
             d88 |
             D1. |   .5572816   .8844013     0.63   0.529    -1.177992    2.292555
                 |
             d89 |
             D1. |  -.0134482   .9545314    -0.01   0.989    -1.886323    1.859427
                 |
             d90 |
             D1. |   .2820852   .9864169     0.29   0.775    -1.653352    2.217522
                 |
             d91 |
             D1. |  -.4587974   1.003981    -0.46   0.648    -2.428697    1.511102
                 |
             d92 |
             D1. |  -.9870113   .9995784    -0.99   0.324    -2.948272    .9742495
                 |
             d93 |
             D1. |  -.8667898   .9797048    -0.88   0.376    -2.789057    1.055477
                 |
             d94 |
             D1. |  -.7772774    .952007    -0.82   0.414    -2.645199    1.090644
                 |
             d95 |
             D1. |  -.1077684   .9247086    -0.12   0.907    -1.922128    1.706591
                 |
             d96 |
             D1. |  -.2884677   .8875639    -0.33   0.745    -2.029946    1.453011
                 |
             d97 |
             D1. |  -.0721104   .8355566    -0.09   0.931    -1.711546    1.567325
                 |
             d98 |
             D1. |   -.415694   .7632714    -0.54   0.586      -1.9133    1.081912
                 |
             d99 |
             D1. |  -.3747216    .677418    -0.55   0.580    -1.703875    .9544322
                 |
             d00 |
             D1. |  -.6760574   .6090511    -1.11   0.267    -1.871069    .5189545
                 |
             d01 |
             D1. |  -.3550926   .5112958    -0.69   0.488      -1.3583    .6481149
                 |
             d02 |
             D1. |   .1955255    .404903     0.48   0.629    -.5989299    .9899809
                 |
             d03 |
             D1. |   .0886873   .2822833     0.31   0.753    -.4651775     .642552
                 |
             d04 |
             D1. |          0  (omitted)
                 |
           _cons |  -.1298422    .096464    -1.35   0.179    -.3191131    .0594287
    ------------------------------------------------------------------------------
     
    . reg d.(totfatrte bac08 bac10 perse sbprim sbsecon sl70plus gdl per14_24 
          unem vehicmiles d81-d04), cluster(state)
    note: _delete omitted because of collinearity
     
    Linear regression                                      Number of obs =    1152
                                                           F( 33,    47) =   28.55
                                                           Prob > F      =  0.0000
                                                           R-squared     =  0.1735
                                                           Root MSE      =  1.8431
     
                                     (Std. Err. adjusted for 48 clusters in state)
    ------------------------------------------------------------------------------
                 |               Robust
     D.totfatrte |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
           bac08 |
             D1. |  -.8612566   .5475202    -1.57   0.122    -1.962725    .2402119
                 |
           bac10 |
             D1. |  -1.050576   .4009937    -2.62   0.012    -1.857271   -.2438806
                 |
           perse |
             D1. |  -.6272044   .3943417    -1.59   0.118    -1.420518    .1661089
                 |
          sbprim |
             D1. |   -.346693   .4722112    -0.73   0.466    -1.296659    .6032734
                 |
         sbsecon |
             D1. |  -.3303991   .3408174    -0.97   0.337    -1.016035    .3552371
                 |
        sl70plus |
             D1. |   .3817811    .516949     0.74   0.464    -.6581861    1.421748
                 |
             gdl |
             D1. |  -.1923185   .2605241    -0.74   0.464    -.7164254    .3317884
                 |
        per14_24 |
             D1. |   .9581163    .329091     2.91   0.005     .2960707    1.620162
                 |
            unem |
             D1. |  -.2338551   .0835355    -2.80   0.007    -.4019068   -.0658033
                 |
      vehicmiles |
             D1. |  -.0115343   .0164127    -0.70   0.486    -.0445525    .0214838
                 |
             d81 |
             D1. |  -1.246487   .4400322    -2.83   0.007    -2.131718   -.3612564
                 |
             d82 |
             D1. |  -2.848866   .4340722    -6.56   0.000    -3.722106   -1.975625
                 |
             d83 |
             D1. |  -2.744412   .3673735    -7.47   0.000    -3.483472   -2.005351
                 |
             d84 |
             D1. |  -2.236782    .400642    -5.58   0.000     -3.04277   -1.430795
                 |
             d85 |
             D1. |  -2.076031   .4010226    -5.18   0.000    -2.882784   -1.269278
                 |
             d86 |
             D1. |  -.4884557   .5121221    -0.95   0.345    -1.518713    .5418012
                 |
             d87 |
             D1. |  -.0483105   .5198629    -0.09   0.926     -1.09414    .9975187
                 |
             d88 |
             D1. |   .5572816   .5535045     1.01   0.319    -.5562259    1.670789
                 |
             d89 |
             D1. |  -.0134482   .6175611    -0.02   0.983    -1.255821    1.228925
                 |
             d90 |
             D1. |   .2820852   .6417303     0.44   0.662     -1.00891     1.57308
                 |
             d91 |
             D1. |  -.4587974    .604231    -0.76   0.451    -1.674353    .7567585
                 |
             d92 |
             D1. |  -.9870113   .5163006    -1.91   0.062    -2.025674    .0516516
                 |
             d93 |
             D1. |  -.8667898   .5394571    -1.61   0.115    -1.952038    .2184579
                 |
             d94 |
             D1. |  -.7772774   .6021292    -1.29   0.203    -1.988605    .4340503
                 |
             d95 |
             D1. |  -.1077684   .5711218    -0.19   0.851    -1.256717     1.04118
                 |
             d96 |
             D1. |  -.2884677   .5621814    -0.51   0.610    -1.419431    .8424955
                 |
             d97 |
             D1. |  -.0721104   .5489352    -0.13   0.896    -1.176426    1.032205
                 |
             d98 |
             D1. |   -.415694   .5295673    -0.78   0.436    -1.481046     .649658
                 |
             d99 |
             D1. |  -.3747216   .5337559    -0.70   0.486      -1.4485    .6990568
                 |
             d00 |
             D1. |  -.6760574   .3918686    -1.73   0.091    -1.464395    .1122805
                 |
             d01 |
             D1. |  -.3550926   .3302952    -1.08   0.288    -1.019561    .3093756
                 |
             d02 |
             D1. |   .1955255   .2962797     0.66   0.513    -.4005124    .7915634
                 |
             d03 |
             D1. |   .0886873   .2630683     0.34   0.738    -.4405378    .6179123
                 |
             d04 |
             D1. |          0  (omitted)
                 |
           _cons |  -.1298422   .0890311    -1.46   0.151    -.3089497    .0492652
    ------------------------------------------------------------------------------

::

    . * Can drop the constant to get effects on all year dummies:
     
    . reg d.(totfatrte bac08 bac10 perse sbprim sbsecon sl70plus gdl per14_24 
        unem vehicmiles d81-d04), nocons cluster(state)
     
    Linear regression                                      Number of obs =    1152
                                                           F( 34,    47) =   31.83
                                                           Prob > F      =  0.0000
                                                           R-squared     =  0.2003
                                                           Root MSE      =  1.8431
     
                                     (Std. Err. adjusted for 48 clusters in state)
    ------------------------------------------------------------------------------
                 |               Robust
     D.totfatrte |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
           bac08 |
             D1. |  -.8612566   .5475202    -1.57   0.122    -1.962725    .2402119
                 |
           bac10 |
             D1. |  -1.050576   .4009937    -2.62   0.012    -1.857271   -.2438806
                 |
           perse |
             D1. |  -.6272044   .3943417    -1.59   0.118    -1.420518    .1661089
                 |
          sbprim |
             D1. |   -.346693   .4722112    -0.73   0.466    -1.296659    .6032734
                 |
         sbsecon |
             D1. |  -.3303991   .3408174    -0.97   0.337    -1.016035    .3552371
                 |
        sl70plus |
             D1. |   .3817811    .516949     0.74   0.464    -.6581861    1.421748
                 |
             gdl |
             D1. |  -.1923185   .2605241    -0.74   0.464    -.7164254    .3317884
                 |
        per14_24 |
             D1. |   .9581163    .329091     2.91   0.005     .2960707    1.620162
                 |
            unem |
             D1. |  -.2338551   .0835355    -2.80   0.007    -.4019068   -.0658033
                 |
      vehicmiles |
             D1. |  -.0115343   .0164127    -0.70   0.486    -.0445525    .0214838
                 |
             d81 |
             D1. |  -1.376329   .4763084    -2.89   0.006    -2.334538   -.4181204
                 |
             d82 |
             D1. |   -3.10855   .4611326    -6.74   0.000    -4.036229   -2.180871
                 |
             d83 |
             D1. |  -3.133938   .5010267    -6.26   0.000    -4.141874   -2.126003
                 |
             d84 |
             D1. |  -2.756151   .5882961    -4.68   0.000     -3.93965   -1.572652
                 |
             d85 |
             D1. |  -2.725242   .6867935    -3.97   0.000    -4.106892   -1.343592
                 |
             d86 |
             D1. |  -1.267509   .9362483    -1.35   0.182    -3.150998    .6159797
                 |
             d87 |
             D1. |   -.957206   1.017576    -0.94   0.352    -3.004305    1.089893
                 |
             d88 |
             D1. |  -.4814561   1.172463    -0.41   0.683    -2.840147    1.877235
                 |
             d89 |
             D1. |  -1.182028    1.30566    -0.91   0.370    -3.808677    1.444621
                 |
             d90 |
             D1. |  -1.016337   1.441317    -0.71   0.484    -3.915892    1.883218
                 |
             d91 |
             D1. |  -1.887062   1.487032    -1.27   0.211    -4.878584     1.10446
                 |
             d92 |
             D1. |  -2.545118   1.501506    -1.70   0.097    -5.565759    .4755232
                 |
             d93 |
             D1. |  -2.554739    1.55854    -1.64   0.108    -5.690116    .5806388
                 |
             d94 |
             D1. |  -2.595068   1.702459    -1.52   0.134    -6.019975     .829838
                 |
             d95 |
             D1. |  -2.055402   1.773614    -1.16   0.252    -5.623454     1.51265
                 |
             d96 |
             D1. |  -2.365943   1.852271    -1.28   0.208    -6.092231    1.360345
                 |
             d97 |
             D1. |  -2.279428   1.917104    -1.19   0.240    -6.136144    1.577288
                 |
             d98 |
             D1. |  -2.752854   1.983611    -1.39   0.172    -6.743363    1.237656
                 |
             d99 |
             D1. |  -2.841724   2.109661    -1.35   0.184    -7.085814    1.402367
                 |
             d00 |
             D1. |  -3.272902   2.019564    -1.62   0.112     -7.33574     .789937
                 |
             d01 |
             D1. |  -3.081779   2.094007    -1.47   0.148    -7.294378     1.13082
                 |
             d02 |
             D1. |  -2.661003   2.112439    -1.26   0.214    -6.910683    1.588677
                 |
             d03 |
             D1. |  -2.897684   2.102946    -1.38   0.175    -7.128265    1.332898
                 |
             d04 |
             D1. |  -3.116213   2.136746    -1.46   0.151    -7.414792    1.182366
    ------------------------------------------------------------------------------.

