Pooled Cross Sections and Difference-in-Differences Estimation
*****************************************************************

What is a Pooled Cross Section?
==================================

Data obtained by pooling cross sections are very useful for establishing trends and conducting
policy analysis.

A pooled cross section (PCS) is available whenever a survey is repeated over time with new random
samples obtained in each time period.

Examples include the General Social Survey (GSS) and the Current Population Survey (CPS).

Statistically, analyzing pooled cross sections is similar to a single cross section, provided we
assume random sampling (or, at least, independent sampling) was used to collect each cross section.

With a PCS, often a goal is to see how the mean value of a variable has changed over time in ways
that cannot be explained by observable variables. (For example, how has fertility changed in ways
that canno be explained by education or labor force participation?)

From a policy perspective, PCSs are at the foundation of *difference-in-differences* estimation.

The typical DID setup is that data can be collected both before and after an intervention (or
treatment), and there is (at least) one control group and (at least) one treatment group.

Often the intervention is of a yes/no form.  But other nonbinary treatments (such as class size) can
be handled, too.

.. admonition:: Example: Test Scores
   
    Suppose we can observe test scores for a sample of 4th graders in two large school
    districts at the end of the 2008 and 2009 school years.

    One district was given a grant to make
    fourth-grade class sizes smaller for the 2009 year.

    In 2008, we would have a random sample of 4th
    graders from each district. We would sample the same districts in 2009,
    and get a different set of 4th graders. (Any overlap would be small and
    due to repeating a grade.)

    We effectively have random samples from four
    different populations (two districts and two time periods each).

    By having a control district – which was not
    given the grant in either year – we can control for aggregate changes
    over time that affect test scores of students in both districts.

    As we will see, we can use standard regression
    analysis to estimate the effect of the `give a grant` intervention.


Including Time Effects in Pooled Cross Sections
==================================================

One reason for using several cross sections over time is to increase sample size. If we think the
relationship between some variables has remained constant over time, we can get more precise
estimates.

If entire regression functions change over time then we cannot pool the data sets, and there is no
benefit in improved precision.

Often one begins with an analysis where only the intercept is different across time; the slopes are
the same.

It is easy to allow different intercepts. If we have :math:`T` years of data, we include dummies for
:math:`T-1` of the years and choose one year (often the earliest) to be the *base year*.

An :math:`F` test – possibly made robust to heteroskedasticity – can be used to test whether the
intercepts change over time.

.. admonition:: Example: U.S. Women’s Fertility, 1972-1984

    How much does education affect number of children born to a woman? As usual, we can look at a
    regression coefficient. (Having a PCS is not needed to answer this question, but it does give us
    more data.)

    How much of the fall in average fertility cannot be explained by changes in observed factors,
    including education?  Here we require a PCS and look at coefficients on year dummies.

    How much of the overall fall in average fertility be explained by increases in average education?
    This calculation is harder. We use the coefficient on education multiplied by the difference in
    average educations in any two years. We can compare this with the average fertility in the two
    years.

For a generic unit in the population we can write an equation

.. math::

   y=\beta _{0}+\delta _{1}d2+\delta _{2}d3+...+\delta _{T-1}dT+\beta
   _{1}x_{1}+...+\beta _{k}x_{k}+u

where :math:`d2`, ..., :math:`dT` are dummy variables for periods
:math:`2` through :math:`T`.

Software packages do not even know we have separate pooled cross sections. Just define and add the
intercept dummies.

Data from FERTIL1.DTA.

::

    Contains data from fertil1.dta
      obs:         1,129                          
     vars:            27                          3 Jun 1997 14:17
     size:        31,612                          
    --------------------------------------------------------------------------------------------------
                  storage  display     value
    variable name   type   format      label      variable label
    --------------------------------------------------------------------------------------------------
    year            byte   %9.0g                  72 to 84, even
    educ            byte   %9.0g                  years of schooling
    meduc           byte   %9.0g                  mother's education
    feduc           byte   %9.0g                  father's education
    age             byte   %9.0g                  in years
    kids            byte   %9.0g                  # children ever born
    black           byte   %9.0g                  = 1 if black
    east            byte   %9.0g                  = 1 if lived in east at 16
    northcen        byte   %9.0g                  = 1 if lived in nc at 16
    west            byte   %9.0g                  = 1 if lived in west at 16
    farm            byte   %9.0g                  = 1 if on farm at 16
    othrural        byte   %9.0g                  = 1 if other rural at 16
    town            byte   %9.0g                  = 1 if lived in town at 16
    smcity          byte   %9.0g                  = 1 if in small city at 16
    y74             byte   %9.0g                  = 1 if year = 74
    y76             byte   %9.0g                  
    y78             byte   %9.0g                  
    y80             byte   %9.0g                  
    y82             byte   %9.0g                  
    y84             byte   %9.0g                  
    agesq           int    %9.0g                  age^2
    --------------------------------------------------------------------------------------------------

::

    . tab year
     
      72 to 84, |
           even |      Freq.     Percent        Cum.
    ------------+-----------------------------------
             72 |        156       13.82       13.82
             74 |        173       15.32       29.14
             76 |        152       13.46       42.60
             78 |        143       12.67       55.27
             80 |        142       12.58       67.85
             82 |        186       16.47       84.32
             84 |        177       15.68      100.00
    ------------+-----------------------------------
          Total |      1,129      100.00
     
    . sum age
     
        Variable |       Obs        Mean    Std. Dev.       Min        Max
    -------------+--------------------------------------------------------
             age |      1129     43.4845    5.836421         35         54

::

    . reg kids educ age agesq black east northcen west farm othrural town smcity 
          y74-y84
     
          Source |       SS       df       MS              Number of obs =    1129
    -------------+------------------------------           F( 17,  1111) =    9.72
           Model |  399.610888    17  23.5065228           Prob > F      =  0.0000
        Residual |  2685.89841  1111  2.41755033           R-squared     =  0.1295
    -------------+------------------------------           Adj R-squared =  0.1162
           Total |   3085.5093  1128  2.73538059           Root MSE      =  1.5548
     
    ------------------------------------------------------------------------------
            kids |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
            educ |  -.1284268   .0183486    -7.00   0.000    -.1644286    -.092425
             age |   .5321346   .1383863     3.85   0.000     .2606065    .8036626
           agesq |   -.005804   .0015643    -3.71   0.000    -.0088733   -.0027347
           black |   1.075658   .1735356     6.20   0.000     .7351631    1.416152
            east |    .217324   .1327878     1.64   0.102    -.0432192    .4778672
        northcen |    .363114   .1208969     3.00   0.003      .125902    .6003261
            west |   .1976032   .1669134     1.18   0.237    -.1298978    .5251041
            farm |  -.0525575     .14719    -0.36   0.721    -.3413592    .2362443
        othrural |  -.1628537    .175442    -0.93   0.353    -.5070887    .1813814
            town |   .0843532    .124531     0.68   0.498    -.1599893    .3286957
          smcity |   .2118791    .160296     1.32   0.187    -.1026379    .5263961

::

             y74 |   .2681825    .172716     1.55   0.121    -.0707039    .6070689
             y76 |  -.0973795   .1790456    -0.54   0.587     -.448685    .2539261
             y78 |  -.0686665   .1816837    -0.38   0.706    -.4251483    .2878154
             y80 |  -.0713053   .1827707    -0.39   0.697      -.42992    .2873093
             y82 |  -.5224842   .1724361    -3.03   0.003    -.8608214    -.184147
             y84 |  -.5451661   .1745162    -3.12   0.002    -.8875846   -.2027477
           _cons |  -7.742457   3.051767    -2.54   0.011    -13.73033   -1.754579
    ------------------------------------------------------------------------------
     
    . * Each additional year of education is estimated to reduce the number of
    . * children by about .13, on average.
    . * Compared with the base year, 1972, fertility fell by about .55 children
    . * in 1984. This is the drop that cannot be explained by the explanatory
    . * variables.

::

    . reg kids educ age agesq black east northcen west farm othrural town smcity 
          y74-y84, robust
     
    Linear regression                                      Number of obs =    1129
                                                           F( 17,  1111) =   10.19
                                                           Prob > F      =  0.0000
                                                           R-squared     =  0.1295
                                                           Root MSE      =  1.5548
     
    ------------------------------------------------------------------------------
                 |               Robust
            kids |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
            educ |  -.1284268    .021146    -6.07   0.000    -.1699175   -.0869362
             age |   .5321346   .1389371     3.83   0.000     .2595258    .8047433
           agesq |   -.005804   .0015791    -3.68   0.000    -.0089024   -.0027056
           black |   1.075658   .2013188     5.34   0.000     .6806496    1.470666
            east |    .217324    .127466     1.70   0.088    -.0327773    .4674252
        northcen |    .363114   .1167013     3.11   0.002     .1341342    .5920939
            west |   .1976032   .1626813     1.21   0.225     -.121594    .5168003
            farm |  -.0525575   .1460837    -0.36   0.719    -.3391886    .2340736
        othrural |  -.1628537   .1808546    -0.90   0.368    -.5177087    .1920014
            town |   .0843532   .1284759     0.66   0.512    -.1677295    .3364359
          smcity |   .2118791   .1539645     1.38   0.169    -.0902149    .5139731

::

             y74 |   .2681825   .1875121     1.43   0.153    -.0997353    .6361003
             y76 |  -.0973795   .1999339    -0.49   0.626    -.4896701    .2949112
             y78 |  -.0686665   .1977154    -0.35   0.728    -.4566042    .3192713
             y80 |  -.0713053   .1936553    -0.37   0.713    -.4512767    .3086661
             y82 |  -.5224842   .1879305    -2.78   0.006    -.8912228   -.1537456
             y84 |  -.5451661   .1859289    -2.93   0.003    -.9099776   -.1803547
           _cons |  -7.742457   3.070656    -2.52   0.012     -13.7674   -1.717518
    ------------------------------------------------------------------------------
     
    . * What happened to the average fertility rate over this period? 
    . * (Aside: It appears the "kids" variable was top coded at 7.)
     
    . bysort year: sum kids
     
    --------------------------------------------------------------------------------------------------
    -> year = 72
     
        Variable |       Obs        Mean    Std. Dev.       Min        Max
    -------------+--------------------------------------------------------
            kids |       156    3.025641    1.827915          0          7
     
    --------------------------------------------------------------------------------------------------
    -> year = 74
     
        Variable |       Obs        Mean    Std. Dev.       Min        Max
    -------------+--------------------------------------------------------
            kids |       173    3.208092    1.502921          0          7
     

::

    --------------------------------------------------------------------------------------------------
    -> year = 76
     
        Variable |       Obs        Mean    Std. Dev.       Min        Max
    -------------+--------------------------------------------------------
            kids |       152    2.802632    1.655972          0          7
     
    --------------------------------------------------------------------------------------------------
    -> year = 78
     
        Variable |       Obs        Mean    Std. Dev.       Min        Max
    -------------+--------------------------------------------------------
            kids |       143    2.804196    1.580064          0          7
     
    --------------------------------------------------------------------------------------------------
    -> year = 80
     
        Variable |       Obs        Mean    Std. Dev.       Min        Max
    -------------+--------------------------------------------------------
            kids |       142    2.816901    1.582796          0          7
     
    --------------------------------------------------------------------------------------------------
    -> year = 82
     
        Variable |       Obs        Mean    Std. Dev.       Min        Max
    -------------+--------------------------------------------------------
            kids |       186    2.403226    1.703348          0          7
     
    --------------------------------------------------------------------------------------------------
    -> year = 84
     
        Variable |       Obs        Mean    Std. Dev.       Min        Max
    -------------+--------------------------------------------------------
            kids |       177    2.237288    1.511385          0          7
     
    . di 2.24 - 3.03
    -.79
     
    . * The average fertility rate fell by about .79.

::

    . * Could also get these estimates with the regression
     
    . * reg kids y72-y84, nocons
     
    . * Look at average education by year:
     
    . bysort year: sum educ
     
    --------------------------------------------------------------------------------------------------
    -> year = 72
     
        Variable |       Obs        Mean    Std. Dev.       Min        Max
    -------------+--------------------------------------------------------
            educ |       156    12.15385     2.34219          6         19
     
    --------------------------------------------------------------------------------------------------
    -> year = 74
     
        Variable |       Obs        Mean    Std. Dev.       Min        Max
    -------------+--------------------------------------------------------
            educ |       173    12.30058    2.440418          4         20
     
    --------------------------------------------------------------------------------------------------
    -> year = 76
     
        Variable |       Obs        Mean    Std. Dev.       Min        Max
    -------------+--------------------------------------------------------
            educ |       152    12.23026    2.751208          0         20
     
    --------------------------------------------------------------------------------------------------
    -> year = 78
     
        Variable |       Obs        Mean    Std. Dev.       Min        Max
    -------------+--------------------------------------------------------
            educ |       143    12.64336    2.470689          1         20
     
    --------------------------------------------------------------------------------------------------
    -> year = 80
     
        Variable |       Obs        Mean    Std. Dev.       Min        Max
    -------------+--------------------------------------------------------
            educ |       142    12.88028    2.879328          2         20
     
    --------------------------------------------------------------------------------------------------
    -> year = 82
     
        Variable |       Obs        Mean    Std. Dev.       Min        Max
    -------------+--------------------------------------------------------
            educ |       186    13.22581      2.7218          7         20
     
    --------------------------------------------------------------------------------------------------
    -> year = 84
     
        Variable |       Obs        Mean    Std. Dev.       Min        Max
    -------------+--------------------------------------------------------
            educ |       177    13.26554    2.631237          6         20
     
    . di _b[educ]*(13.27 - 12.15)
    -.14383805
     
    . * Of the overall drop of about .79 children, the increase in education
    . * (1.12 years on average) accounts for about .14 of that, or about 18%.



Interacting Variables with Time Dummies
---------------------------------------

In the previous estimation with the fertility data, we assumed the effect of education (and all
other variables) was the same over time.

We can easily allow the slopes to change over time by forming interactions and adding them to the
model.

As always with interactions, must be careful in interpreting the level terms – including those on
the year dummies.

::

    . reg kids educ age agesq black east northcen west farm othrural town smcity 
               y74-y84  y74educ-y84educ, robust
     
    Linear regression                                      Number of obs =    1129
                                                           F( 23,  1105) =    8.21
                                                           Prob > F      =  0.0000
                                                           R-squared     =  0.1365
                                                           Root MSE      =  1.5528
     
    ------------------------------------------------------------------------------
                 |               Robust
            kids |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
            educ |  -.0225152    .066141    -0.34   0.734    -.1522913    .1072609
             age |   .5074658   .1400342     3.62   0.000      .232703    .7822287
           agesq |   -.005525   .0015924    -3.47   0.001    -.0086494   -.0024005
           black |   1.074055   .2006813     5.35   0.000     .6802955    1.467814
            east |   .2060559   .1274167     1.62   0.106      -.04395    .4560619
        northcen |   .3482867   .1165612     2.99   0.003     .1195804     .576993
            west |   .1771221    .163542     1.08   0.279    -.1437658    .4980101
            farm |  -.0721622   .1452696    -0.50   0.619    -.3571976    .2128732
        othrural |  -.1911539   .1784384    -1.07   0.284    -.5412702    .1589625
            town |   .0882295   .1285735     0.69   0.493    -.1640463    .3405053
          smcity |   .2053576   .1543955     1.33   0.184    -.0975839    .5082991

::

             y74 |   .9469149    1.03828     0.91   0.362    -1.090308    2.984138
             y76 |   1.019963   1.127292     0.90   0.366    -1.191912    3.231839
             y78 |   1.805985   1.332366     1.36   0.176    -.8082674    4.420238
             y80 |   1.114183   1.050826     1.06   0.289    -.9476562    3.176023
             y82 |   1.199807   1.009239     1.19   0.235    -.7804334    3.180048
             y84 |   1.671261   1.026677     1.63   0.104    -.3431965    3.685718
         y74educ |  -.0564248   .0819404    -0.69   0.491    -.2172012    .1043516
         y76educ |  -.0920997   .0897557    -1.03   0.305    -.2682105    .0840112
         y78educ |  -.1523873   .1034737    -1.47   0.141    -.3554144    .0506399
         y80educ |  -.0979049   .0836096    -1.17   0.242    -.2619564    .0661467
         y82educ |  -.1389447   .0792514    -1.75   0.080    -.2944449    .0165554
         y84educ |   -.176097   .0796192    -2.21   0.027    -.3323188   -.0198752
           _cons |  -8.477302   3.193861    -2.65   0.008    -14.74402   -2.210585
    ------------------------------------------------------------------------------
     
    . testparm  y74educ-y84educ
     
     ( 1)  y74educ = 0
     ( 2)  y76educ = 0
     ( 3)  y78educ = 0
     ( 4)  y80educ = 0
     ( 5)  y82educ = 0
     ( 6)  y84educ = 0
     
           F(  6,  1105) =    1.17
                Prob > F =    0.3182
     
    . * Jointly insignificant, even though y84educ and even y82educ are 
    . * individually significant.

Coefficient on, say, :math:`y84` is the difference in fertility between 1984 and 1972 at
:math:`educ=0`; not interesting.

Effect of schooling in base year very close to zero.

The joint test for all interactions with :math:`educ` gives :math:`p`-value = :math:`.318`, so we
cannot reject the null that the effect of education has been constant. But it seems fertility has
become more sensitive to education in the last couple of years of the data (1982, 1984).

By interacting the year dummies with all explanatory variables can do a Chow test. Usually more
interesting to be selective. Model with just additive time dummies often cannot be rejected.


DID with Two Groups and Two Time Periods 
===========================================

A special setup often arises with independently pooled cross sections. The setup is used often to
study the effects of policy interventions.

Outcomes are observed for two groups over two time periods. One of the groups is exposed to a
treatment (or intervention) in the second period but not in the first period. The second group is
not exposed to the treatment during either period.

Let :math:`A` be the control group and :math:`B` the treatment group. Let :math:`d2` be a time
period dummy equal to one for a unit in the second time period. Write

.. math:: y=\beta _{0}+\beta _{1}dB+\delta _{0}d2+\delta _{1}d2\cdot dB+u, 

where :math:`y` is the outcome of interest.

The mean value of :math:`u` is zero (essentially by definition), so we can read off the means of the
response for different combinations.

+-------------------------------+---------------------------------+---------------------------------------+-----------------------------------+
|                               | Before (1)                      | After (2)                             | After :math:`-` Before            |
+===============================+=================================+=======================================+===================================+
| Control (A)                   | :math:`\beta _{0}`              | :math:`\beta _{0}+\delta _{0}`        | :math:`\delta _{0}`               |
+-------------------------------+---------------------------------+---------------------------------------+-----------------------------------+
| Treatment (B)                 | :math:`\beta _{0}+\beta _{1}`   | :math:`\beta _{0}+\delta _{0}+\beta   | :math:`\delta _{0}+\delta _{1}`   |
|                               |                                 | _{1}+\delta _{1}`                     |                                   |
+-------------------------------+---------------------------------+---------------------------------------+-----------------------------------+
| Treatment :math:`-` Control   | :math:`\beta _{1}`              | :math:`\beta _{1}+\delta _{1}`        | :math:`\delta                     |
|                               |                                 |                                       | _{1}`                             |
+-------------------------------+---------------------------------+---------------------------------------+-----------------------------------+

.. math:: y=\beta _{0}+\beta _{1}dB+\delta _{0}d2+\delta _{1}d2\cdot dB+u

:math:`dB` captures possible differences between the treatment and control groups prior to the
policy change. Its coefficient, :math:`\beta _{1}`, is the difference between treatment and control
*before* the intervention.

:math:`d2` captures aggregate factors that would cause changes in :math:`y` over time even in the
absense of an intervention. Notice its coefficient, :math:`\delta _{0}`, is the change in the mean
of the control group across the two periods.

.. math:: y=\beta _{0}+\beta _{1}dB+\delta _{0}d2+\delta _{1}d2\cdot dB+u

The change in the mean over time for the treatment group is :math:`\delta _{0}+\delta _{1}`.

The coefficient of interest is :math:`\delta _{1}=(\delta _{1}+\delta _{0})-\delta _{0}`, the
difference in the average changes over time for the treatment and control groups.

Conveniently, :math:`\delta _{1}` is the coefficient on the interaction :math:`d2\cdot dB`, which is
one if an only if the unit is in the treatment group in period 2.

If :math:`y` is a logarithm then, as usual, :math:`\delta _{1}` is a proportionate effect (multiple
by 100 to approximate the percentage effect).

The difference-in-differences (DD) estimate can be obtained by applying OLS to equation (1). Or, we
can just use the averages directly:

.. math::

    \hat{\delta}_{DD} &= \hat{\delta}_{1}=(\bar{y}_{B,2}-\bar{y}_{B,1})-(\bar{y}_{A,2}-\bar{y}_{A,1}) \\
    &= (\bar{y}_{B,2}-\bar{y}_{A,2})-(\bar{y}_{B,1}-\bar{y}_{A,1})  

OLS makes inference straightforward.  Heteroskedasticity-robust inference allows different
group/time period variances in regression framework.

Just using :math:`\bar{y}_{B,2}-\bar{y}_{B,1}`, the difference over time in the means of the
treatment group, attributes all change to the intervention.

Just using :math:`\bar{y}_{B,2}-\bar{y}_{A,2}`, the difference in treatment and control means
post-treatment, attributes any differences in the groups to the treatment.

Writing

.. math:: \hat{\delta}_{1}=(\bar{y}_{B,2}-\bar{y}_{B,1})-(\bar{y}_{A,2}-\bar{y}_{A,1})

shows that we are comparing the change in means over time for the treatment to the change in mean
for the control.

While powerful, the basic approach can suffer from several problems. First, there may be
compositional effects. For example, when looking at math scores and an intervention to reduce class
size, maybe (say) the 4th graders in the two years are not comparable.  Perhaps the smaller class
sizes attracts new students to that district.  This is the problem of *compositional changes*.

Can control for changes in composition to some extent by including regressors as controls. For
example, family background variables. The regression framework makes this easy:

.. math:: y=\beta _{0}+\beta _{1}dB+\delta _{0}d2+\delta _{1}d2\cdot dB+\mathbf{x\gamma }+u

A potential problem with using only two periods is that the control and treatment groups may be
trending at different rates having nothing to do with the intervention.

In the class-size example, what if performance in district :math:`B` was at an initial lower level
(on average) than district :math:`A` but was increasing faster? :math:`dB` accounts for initial
level differences, but the effect of the policy is measured by the growth in the averages. Only way
to solve this problem is get another control group or more years of data (preferably at least two
before the intervention).

.. admonition:: Example: Effects of Attendance Monitoring on Attendance

    Note: These Data are Fictional

    Two years of data (2005 and 2006) on two instructors (lectures 1 and 2). In lecture 2 in 2006,
    electronic monitoring of attendance was used (card swiping).

    Year dummy is :math:`d06`, lecture dummy is :math:`dlec2`, and the interaction is
    :math:`d06\_dlec2`.

::

    . use attendance
     
    . des
     
    Contains data from attendance.dta
      obs:         1,338                          
     vars:             8                          10 Oct 2010 23:11
     size:        26,760 (99.9% of memory free)   (_dta has notes)
    -------------------------------------------------------------------------------------------------------------------------
                  storage  display     value
    variable name   type   format      label      variable label
    -------------------------------------------------------------------------------------------------------------------------
    attend          byte   %8.0g                  classes attended out of 32
    priGPA          float  %9.0g                  cumulative GPA prior to term
    ACT             byte   %8.0g                  ACT score
    year            int    %9.0g                  2005 or 2006
    lecture         byte   %9.0g                  1 or 2
    d06             byte   %9.0g                  =1 if year == 2006
    dlec2           byte   %9.0g                  =1 if lecture == 2
    d06_dlec2       byte   %9.0g                  d06*dlec2
    -------------------------------------------------------------------------------------------------------------------------
    Sorted by:  

::

    . tab year
     
        2005 or |
           2006 |      Freq.     Percent        Cum.
    ------------+-----------------------------------
           2005 |        680       50.82       50.82
           2006 |        658       49.18      100.00
    ------------+-----------------------------------
          Total |      1,338      100.00
     
    . tab lecture
     
         1 or 2 |      Freq.     Percent        Cum.
    ------------+-----------------------------------
              1 |        683       51.05       51.05
              2 |        655       48.95      100.00
    ------------+-----------------------------------
          Total |      1,338      100.00

::

    . tab attend
     
        classes |
       attended |
      out of 32 |      Freq.     Percent        Cum.
    ------------+-----------------------------------
              0 |          4        0.30        0.30
              1 |          1        0.07        0.37
              2 |          1        0.07        0.45
              3 |          2        0.15        0.60
              4 |          2        0.15        0.75
              5 |          4        0.30        1.05
              6 |          5        0.37        1.42
              7 |          4        0.30        1.72
              8 |          8        0.60        2.32
              9 |          5        0.37        2.69
             10 |          9        0.67        3.36
             11 |          5        0.37        3.74
             12 |          9        0.67        4.41
             13 |         17        1.27        5.68
             14 |         13        0.97        6.65
             15 |         23        1.72        8.37
             16 |         32        2.39       10.76
             17 |         22        1.64       12.41
             18 |         32        2.39       14.80
             19 |         26        1.94       16.74
             20 |         36        2.69       19.43
             21 |         62        4.63       24.07
             22 |         68        5.08       29.15
             23 |         81        6.05       35.20
             24 |         96        7.17       42.38
             25 |        132        9.87       52.24
             26 |        126        9.42       61.66
             27 |        137       10.24       71.90
             28 |        138       10.31       82.21
             29 |        109        8.15       90.36
             30 |         74        5.53       95.89
             31 |         30        2.24       98.13
             32 |         25        1.87      100.00
    ------------+-----------------------------------
          Total |      1,338      100.00

::

    . reg attend d06
     
          Source |       SS       df       MS              Number of obs =    1338
    -------------+------------------------------           F(  1,  1336) =   30.76
           Model |    910.7359     1    910.7359           Prob > F      =  0.0000
        Residual |  39557.0728  1336  29.6085874           R-squared     =  0.0225
    -------------+------------------------------           Adj R-squared =  0.0218
           Total |  40467.8087  1337  30.2676205           Root MSE      =  5.4414
     
    ------------------------------------------------------------------------------
          attend |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
             d06 |   1.650277   .2975565     5.55   0.000     1.066548    2.234006
           _cons |   23.17647   .2086673   111.07   0.000     22.76712    23.58582
    ------------------------------------------------------------------------------
     
    . * Shows that attendance increased by 1.65 classes, on average, from 2005
    . * to 2006 (across both lectures).

::

    . reg attend dlec2
     
          Source |       SS       df       MS              Number of obs =    1338
    -------------+------------------------------           F(  1,  1336) =    1.57
           Model |  47.3565824     1  47.3565824           Prob > F      =  0.2111
        Residual |  40420.4521  1336  30.2548294           R-squared     =  0.0012
    -------------+------------------------------           Adj R-squared =  0.0004
           Total |  40467.8087  1337  30.2676205           Root MSE      =  5.5004
     
    ------------------------------------------------------------------------------
          attend |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
           dlec2 |   .3763459   .3008115     1.25   0.211    -.2137683    .9664602
           _cons |   23.80381   .2104684   113.10   0.000     23.39092    24.21669
    ------------------------------------------------------------------------------
     
    . * Average attendance is slightly higher in lecture 2, but not statistically
    . * significant.

::

    . reg attend d06 dlec2 d06_dlec2
     
          Source |       SS       df       MS              Number of obs =    1338
    -------------+------------------------------           F(  3,  1334) =   12.60
           Model |  1114.82893     3  371.609644           Prob > F      =  0.0000
        Residual |  39352.9797  1334  29.4999848           R-squared     =  0.0275
    -------------+------------------------------           Adj R-squared =  0.0254
           Total |  40467.8087  1337  30.2676205           Root MSE      =  5.4314
     
    ------------------------------------------------------------------------------
          attend |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
             d06 |   1.009064   .4156566     2.43   0.015     .1936517    1.824476
           dlec2 |  -.2411765   .4165685    -0.58   0.563    -1.058377    .5760243
       d06_dlec2 |   1.328705   .5942944     2.24   0.026     .1628514    2.494558
           _cons |   23.29706   .2945584    79.09   0.000     22.71921    23.87491
    ------------------------------------------------------------------------------
     
    . * The difference-in-differences estimate is 1.33, and it is statistically
    . * significant. Note that attendance increased by 1 for the control group.
    . * A spillover effect, or something else?
    . * Note the small R-squared.

::

    . reg attend d06 dlec2 d06_dlec2, robust
     
    Linear regression                                      Number of obs =    1338
                                                           F(  3,  1334) =   11.11
                                                           Prob > F      =  0.0000
                                                           R-squared     =  0.0275
                                                           Root MSE      =  5.4314
     
    ------------------------------------------------------------------------------
                 |               Robust
          attend |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
             d06 |   1.009064   .3603792     2.80   0.005      .302092    1.716035
           dlec2 |  -.2411765   .4165156    -0.58   0.563    -1.058273    .5759204
       d06_dlec2 |   1.328705   .5976782     2.22   0.026     .1562132    2.501196
           _cons |   23.29706   .2560285    90.99   0.000      22.7948    23.79932
    ------------------------------------------------------------------------------

::

    . * Now add prior GPA and ACT score:
     
    . reg attend d06 dlec2 d06_dlec2 priGPA ACT, robust
     
    Linear regression                                      Number of obs =    1338
                                                           F(  5,  1332) =   91.21
                                                           Prob > F      =  0.0000
                                                           R-squared     =  0.3034
                                                           Root MSE      =  4.6004
     
    ------------------------------------------------------------------------------
                 |               Robust
          attend |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
             d06 |   .9429047   .3209404     2.94   0.003     .3133011    1.572508
           dlec2 |   .1362344   .3508858     0.39   0.698    -.5521145    .8245834
       d06_dlec2 |   1.471297   .5055328     2.91   0.004     .4795696    2.463024
          priGPA |   5.443933   .2791131    19.50   0.000     4.896383    5.991482
             ACT |  -.5445861   .0404013   -13.48   0.000    -.6238432   -.4653289
           _cons |   21.28492   .8457649    25.17   0.000     19.62574    22.94409
    ------------------------------------------------------------------------------
     
    . * Improves precision of estimate. Increases R-squared from .025 to .303.



Multiple Control Groups
-----------------------

Can refine the definition of treatment and control groups to account for different trends in the
absense of treatment. The DD estimate is basically the difference in trends measured in two time
periods.

If we have another group that is not subject to the treatment in the treated group, we can use them
to account for different trends.

.. admonition:: Example:
   
    In the class size example, suppose that it only applied to 4th grade, not 3rd grade.
    Let :math:`F`\ denote whether a student is in 4th grade.

    Now we have a third dummy variable, :math:`dF`, to denote 4th grade. Assume still two years are
    available but now use third graders as an additional control.

    Could do two diff-in-diffs. Use the previous analysis – just 4th graders in the two districts,
    before and after – or use third and fourth graders within district :math:`B`.

    Using the latter interpretation, we can compute the DD estimates for districts :math:`A` and
    :math:`B`, and then difference those:

    .. math::

        \hat{\delta}_{DDD} &= \hat{\delta}_{3}=[(\bar{y}_{B,F,2}-\bar{y}_{B,F,1})-(\bar{y}_{B,T,2}-\bar{y}_{B,T,1})] \\ 
        & -[(\bar{y}_{A,F,2}-\bar{y}_{A,F,1})-(\bar{y}_{A,T,2}-\bar{y}_{A,T,1})]

    :math:`\hat{\delta}_{DDD}` is the **difference-in-difference-in-differences (DDD) **\ estimate.

    This estimate accounts for trends if, in the absense of treatment, the third and fourth graders
    would have the same trends in both districts.

    The estimating equation, by OLS, looks like

    .. math::

        y &= \beta _{0}+\beta _{1}dB+\beta _{2}dF+\beta _{3}dB\cdot dF+\delta _{0}d2 \\
        & +\delta _{1}d2\cdot dB+\delta _{2}d2\cdot dF+\delta _{3}d2\cdot dB\cdot dF+u,

    and the DDD estimate is :math:`\hat{\delta}_{3}`.

    Again, can add student-specific regressors.

    Even if the regressors are independent of the intervention, adding them can improve efficiency if
    they help to predict :math:`y`.

Multiple Groups and Time Periods
===================================

In many policy analysis setups, individual units :math:`i` (such as students) are in groups
:math:`g` (districts) in different time periods :math:`t` (grades).

Suppose there are :math:`G` groups, :math:`T` time periods, and :math:`M_{gt}` individuals in group
:math:`g` at time :math:`t`.

.. math:: 
   
    y_{igt}=\lambda _{t}+\alpha _{g}+\mathbf{d}_{gt}\mathbf{\beta
    }+\mathbf{x}_{igt}\mathbf{\gamma }+u_{igt},\text{ }i=1,...,M_{gt} 

The parameters :math:`\lambda _{t}` represents a full set of time effects and :math:`\alpha _{g}` a
set of group effects.

The vector :math:`\mathbf{d}_{gt}` contains the policy indicators. (In the leading case, this is a
single variable.)

In practice, we would allow some interactions among the time and groups, but we cannot include a
full set of interactions and still learn about effects of :math:`\mathbf{d}_{gt}`.

The vector :math:`\mathbf{x}_{igt}` contains individual-specific variables to control for
compositional effects.

Can use multiple regression pooled across all groups and time periods. Under random sampling within
each :math:`(g,t)`, make inference robust to heteroskedasticity.

Continuous Treatments in DID 
===============================

Sometimes the policy intervention should interact with a continuous variable. The standard DID
analysis needs to be modified.

Example is given in Wooldridge (5e, Computer Exercise C13.3). The intervention is the siting of a
garbage incinerator, and the response variable is the price of a house. It makes sense that houses
closer to the site should be affected more than houses further away.

Let :math:`dist` be the distance (in feet), from a generic house to the (eventual) site of the
incinerator. Note :math:`dist` is defined before and after the incinerator is built.

Construction of the incinerator began in 1981.  Use 1978 as the base year, which is prior to any
rumors about the incinerator.

A constant elasticity model is

.. math:: lprice=\beta _{0}+\delta _{0}y81+\beta _{1}ldist+\delta _{1}y81\cdot ldist+u

where :math:`lprice=\log (price)` and :math:`ldist=\log (dist).`

The coefficient :math:`\delta _{0}` allows for price inflation, or real appreciation in all housing
prices.

The elasticity of price with respect to distance in 1978 is :math:`\beta _{1}`. If the siting of the
incinerator was random we might expect :math:`\beta _{1}=0`. If the incinerator was built in a
neighborhood with lower values to begin with, :math:`\beta _{1}>0`.

The equation again:

.. math:: lprice=\beta _{0}+\delta _{0}y81+\beta _{1}ldist+\delta _{1}y81\cdot ldist+u

The parameter of most interest is :math:`\delta _{1}`, which measures how the price response to
distance changed after the siting of the incinerator. We expect :math:`\delta _{1}>0` if being close
to the incinerator is a bad thing: after construction on the incinerator has started, it is even
better to be farther away from the site.

Data in KIELMC.DTA.

::

    . use kielmc
     
    . tab year
     
        1978 or |
           1981 |      Freq.     Percent        Cum.
    ------------+-----------------------------------
           1978 |        179       55.76       55.76
           1981 |        142       44.24      100.00
    ------------+-----------------------------------
          Total |        321      100.00
     
    . des price dist
     
                  storage  display     value
    variable name   type   format      label      variable label
    -------------------------------------------------------------------------------------------------------------------------------
    price           float  %9.0g                  selling price
    dist            float  %9.0g                  dist. from house to incin., ft.
     
    . sum price dist
     
        Variable |       Obs        Mean    Std. Dev.       Min        Max
    -------------+--------------------------------------------------------
           price |       321    96100.66    43223.73      26000     300000
            dist |       321    20715.58    8508.184       5000      40000

::

    . reg lprice ldist if year == 1978
     
          Source |       SS       df       MS              Number of obs =     179
    -------------+------------------------------           F(  1,   177) =   40.04
           Model |  4.42258902     1  4.42258902           Prob > F      =  0.0000
        Residual |  19.5486461   177  .110444328           R-squared     =  0.1845
    -------------+------------------------------           Adj R-squared =  0.1799
           Total |  23.9712351   178   .13466986           Root MSE      =  .33233
     
    ------------------------------------------------------------------------------
          lprice |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
           ldist |    .316689   .0500457     6.33   0.000      .217926     .415452
           _cons |   8.058468   .4937685    16.32   0.000     7.084037    9.032899
    ------------------------------------------------------------------------------
     
    . * Even before the incinerator was rumored, housing price increases with
    . * distance from the site. The elasticity is pretty large: a 10% in distance
    . * is associated with about a 3.2% increase in price, on average.

::

    . reg lprice y81 ldist y81ldist, robust
     
    Linear regression                                      Number of obs =     321
                                                           F(  3,   317) =   82.51
                                                           Prob > F      =  0.0000
                                                           R-squared     =  0.3958
                                                           Root MSE      =   .3422
     
    ------------------------------------------------------------------------------
                 |               Robust
          lprice |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
             y81 |  -.0113101   .7619982    -0.01   0.988    -1.510523    1.487903
           ldist |    .316689   .0375356     8.44   0.000     .2428386    .3905395
        y81ldist |   .0481862   .0765747     0.63   0.530    -.1024727     .198845
           _cons |   8.058468   .3747943    21.50   0.000     7.321069    8.795866
    ------------------------------------------------------------------------------
     
    . * The estimated coefficient on y81ldist is positive -- as we expect --
    . * but not statistically significant.
    . * There might be compositional effects: the kind of homes on the market
    . * are relatively different depending on whether the home is close to 
    . * or far away from the incinerator.

::

    . des age intst land area rooms baths
     
                  storage  display     value
    variable name   type   format      label      variable label
    -------------------------------------------------------------------------------------------------------------------------------
    age             int    %9.0g                  age of house
    intst           float  %9.0g                  dist. to interstate, ft.
    land            float  %9.0g                  square footage lot
    area            int    %9.0g                  square footage of house
    rooms           byte   %9.0g                  # rooms in house
    baths           byte   %9.0g                  # bathrooms
     
    . * First control for age of the house, as a quadratic.

::

    . reg lprice y81 ldist y81ldist age agesq, robust
     
    Linear regression                                      Number of obs =     321
                                                           F(  5,   315) =  123.84
                                                           Prob > F      =  0.0000
                                                           R-squared     =  0.6085
                                                           Root MSE      =  .27633
     
    ------------------------------------------------------------------------------
                 |               Robust
          lprice |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
             y81 |  -.7911677   .6443918    -1.23   0.220    -2.059024    .4766884
           ldist |   .0041058   .0564218     0.07   0.942    -.1069055    .1151171
        y81ldist |   .1245501   .0645592     1.93   0.055    -.0024717    .2515719
             age |  -.0182132   .0018522    -9.83   0.000    -.0218574   -.0145689
           agesq |   .0001021   .0000151     6.77   0.000     .0000724    .0001318
           _cons |   11.33375   .5760203    19.68   0.000     10.20041    12.46708
    ------------------------------------------------------------------------------
     
    . * Note how much larger the effect is now -- .125 versus .048 -- and the
    . * coefficient is now statistically significant at the 5.5% level 
    . * against a two-sided alternative.
    . * In fact, controlling for age of the house, there is no relationship between
    . * price and distance in 1978!
    . * Note how the R-squared increases markedly when age is controlled for.

::

    . nlcom -_b[age]/(2*_b[agesq])
     
           _nl_1:  -_b[age]/(2*_b[agesq])
     
    ------------------------------------------------------------------------------
          lprice |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
           _nl_1 |    89.1619    6.23959    14.29   0.000     76.88536    101.4384
    ------------------------------------------------------------------------------
     
    . * The turning point is at 89 years; age has a negative effect on price up
    . * until then, but the effect diminishes.
     
     
    . count if age > 89
       11
     
    . * Only 11 homes out of 321 are to the right of the minimum.

::

    . * Now add more controls:
     
    . reg lprice y81 ldist y81ldist age agesq lintst lland larea rooms baths, robust
     
    Linear regression                                      Number of obs =     321
                                                           F( 10,   310) =  130.95
                                                           Prob > F      =  0.0000
                                                           R-squared     =  0.7870
                                                           Root MSE      =  .20545
     
    ------------------------------------------------------------------------------
                 |               Robust
          lprice |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
    -------------+----------------------------------------------------------------
             y81 |  -.2254466   .5031352    -0.45   0.654    -1.215439    .7645454
           ldist |   .0009226   .0461654     0.02   0.984    -.0899147    .0917598
        y81ldist |   .0624668   .0505522     1.24   0.218     -.037002    .1619355
             age |  -.0080075   .0016001    -5.00   0.000    -.0111559    -.004859
           agesq |   .0000357   .0000107     3.35   0.001     .0000147    .0000567
          lintst |  -.0599757   .0372422    -1.61   0.108    -.1332553    .0133038
           lland |   .0953425   .0334785     2.85   0.005     .0294687    .1612163
           larea |   .3507429   .0630758     5.56   0.000     .2266321    .4748538
           rooms |   .0461389   .0174623     2.64   0.009     .0117792    .0804986
           baths |   .1010478   .0277474     3.64   0.000     .0464508    .1556448
           _cons |   7.673854    .533431    14.39   0.000      6.62425    8.723457
    ------------------------------------------------------------------------------
     
    . * Now the effect is smaller and not strongly significant. The other factors
    . * do help explain price variation; R-squared is up to .787.

