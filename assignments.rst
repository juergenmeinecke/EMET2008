Assignments 
#############

Assignment 1
****************

.. admonition:: Instructions

    Answer all questions!

    This assignment is due at 2.00pm on Thursday, 28 August. It is worth 10% of your final mark for
    this course. Hand in your work by putting it in the EMET2008/6008 assignment box (HW Arndt
    Building 25a, opposite of room 1002). Absolutely no extensions will be given, late assignments
    will receive zero credit. If you have a university approved excuse for not handing in this
    assignment, then your marks for your final exam will be weighted up by 10% to compensate for the
    missed work.  
   
    While I would prefer it if you could provide *typed* answers, you may also hand in written
    answers as long as they are legible and easy to follow. (I will not only mark the correctness of
    your answers but also the clarity of the exposition and the transparency with which you
    communicate your results.) The work that you hand in should consist of answers to the questions,
    together with an appendix which contains both the printout of a complete Stata do-file and a
    log-file (prodced by the do-file) that covers the entire assignment.
    
    Answers should be in sentence form (i.e. single word or single number answers without
    explanation will be considered incomplete), but clarity of presentation is important, so try to
    make your comments/discussion brief and to the point. Annotated output does not constitute a
    sufficient answer to any question, but you should highlight those parts of your output that you
    explicitly use in your answers.

    If you have any questions regarding to these instructions, do not hesitate to ask me (either
    during class meetings or consultation times or e-mail.)

Exercise 1 
============= 

Frankel and Rose (for short: FR), in their 2005 paper ''*Is Trade Good or Bad for the Environment?
Sorting Out the Causality.*'' which is published in *The Review of Economics and Statistics* (volume
87(1), pages 85-91) empirically address the question::

    Is globalization good or bad for the environment?

In particular, they examine whether countries which are more open to international trade incur more
(or less) environmental damage as result, controlling for international variations in real growth
rates and in political institutions. FR quantify environmental damage on seven dimensions:
:math:`\text{SO}_2` air concentrations, :math:`\text{NO}_2` air concentrations, particulate matter
air concentrations, :math:`\text{CO}_2` air concentrations, deforestation, energy resources
depletion, and rural clean water access.  Their analytic focus is primarily on the
:math:`\text{SO}_2`, :math:`\text{NO}_2`, and particulate matter air pollution impacts, however. 

Overall, FR find that greater trade openness, quantified as 

.. math::
   \frac{\text{Exports} + \text{Imports}}{\text{GDP}}, 
   
is actually associated with better environmental outcomes. This result might seem surprising, in
view of the::

    ''...race-to-the-bottom hypothesis, which says that open countries in general adopt looser
    standards of environmental regulation, out of fear of a loss in international competitiveness.
    Alternatively, poor open countries may act as pollution havens, adopting lax environmental
    standards to attract multinational corporations and export pollution-intensive goods.  
    
    Less widely recognized is the possibility of an effect in the opposite direction, which we call
    the gains-from-trade hypothesis. If trade raises income, it allows countries to attain more of
    what they want, which includes environmental as well as more conventional output. Openness could
    have a positive effect on environmental quality (even for a given level of GDP per capita) for a
    number of reasons. First, trade can spur managerial and technological innovation, which can have
    positive effects on both the economy and the environment. Second, multinational corporations
    tend to bring clean state-of-the-art production techniques from high-standard source countries
    of origin to host countries. Third is the international ratcheting up of environmental standards
    through heightened public awareness. Whereas some environmental gains may tend to occur with any
    increase in income, whether taking place in an open economy or not, others may be more likely
    when associated with international trade and investment. Whether the race-to-the-bottom effect
    in practice dominates the gains-from-trade effect is an empirical question.''
    (modified quotation from FR 2005)


In this exercise you will replicate FR's empirical results for the :math:`\text{SO}_2` measure of air
pollution and diagnostically check their regression model, so as to assess the credibility of their
statistical inference results.

Two features of this exercise should be noted at the outset. First, it is worth noting that the FR
model is estimated using only 41 sample observations. This is an unusually small sample size for a
piece of research in applied econometrics that is published in such a high-quality journal. As we
have learned in class, sample estimators will have an approximate normal distribution (justified by
the central limit theorem) -- the larger the sample size, the better the approximation. For the
purpose of this assignment, we will not worry further about the small sample size here. We keep it
in the back of our minds but are otherwise happy to use and apply our standard econometric toolkit.

Second, we conduct our analysis here under the assumption that all key explanatory variables are
*exogenous*. In particular, real per capita income and trade openness are considered to be
determined outside of the model. (We will deviate from that assumption in the next exercise.)

Use the Stata file *Frankel_Rose.dta* (available for download on Wattle). This file contains
data on 41 countries, collecting (among others) the following variables:

================    =============================================================================
Variable            Description 
================    =============================================================================
``sulfdm``          mean 1990 :math:`\text{SO}_2` (sulfur dioxide) concentration 
                    (in micrograms per cubic meter).
``inc``             logarithm of real per capita GDP 
                    (from the Penn World Tables 5.6; in 1990 dollars, PPP adjusted)
``incsqr``          squared value of the logarithm of real per capita GDP.
``pwtopen``         :math:`100 \cdot` (Imports + Exports)/GDP from the Penn World Tables 5.6.
``polity``          index of democratic (+10) versus autocratic (-10) institutions.
``lareapc``         logarithm of land area per capita.
``oecd``            dummy variable which equals 1 if country is an OECD member country
``country``         country name
================    =============================================================================

a)  Estimate a basic regression model with ``sulfdm`` as the outcome variable using the regressors
    ``inc``, ``incsqr``, ``pwtopen``, ``polity``, ``lareapc``. Interpret your coefficient estimates.
    
    What does this say about the impact of trade openness on :math:`\text{SO}_2` concentrations --
    i.e., on the relative importance of the ''race-to-the- bottom'' versus ''gains-from-trade''
    hypotheses alluded to earlier?

#)  Test the model for heteroskedasticity. Do you conclude that the model is heteroskedastic? (If
    so, proceed with the heteroskedasticity-corrected version of the model in everything that
    follows.)

    In the linear regression model, when you correct for heteroskedasticity, how do your coefficient
    estimates change (vis-a-vis the model in which you do not correct for homoskedasticity)? What
    about the standard errors?
    
#)  Explain (in words, not maths) what the :math:`R^2` of a regression measures. How does the
    *adjusted* :math:`R^2`, denoted :math:`\bar{R}^2`, differ from this?
    
    Using the adjusted :math:`R^2` statistic, what is the fraction of the sample variation in sulfur
    dioxide concentration which is *explained* by these five explanatory variables? By how much does
    this fraction decrease once the openness variable is dropped from the model? 
    
    (Note: To have Stata report the value of adjusted :math:`R^2`, use the command *ereturn list*
    after the regress command: adjusted :math:`R^2` will be listed as ``e(r2_a)``.)

#)  Produce the scatter plot of ``sulfdm`` against the crucial independent variable ``pwtopen``. Can
    you spot two outliers? Which countries do they correspond to? Are they the driving force behind
    your estimation results?
    
#)  Estimate a re-specified model, using both the logs of ``sulfdm`` and ``pwtopen``. (Recall from
    your study of the log-log model in EMET2007 that the coefficient on ``logpwtopen`` can be
    interpreted as the elasticity of ``sulfdm`` with respect to ``pwtopen``.) How do your conclusions about
    the openness effect change? 
    
#)  Check whether the key coefficient in the model is different for OECD countries.

(*Note: This exercise is from the book ''Fundamentals of Applied Econometrics'' by Richard Ashley.*)

Exercise 2
===============

In Exercise 1 you used OLS to study the relationship between trade openness and sulfur dioxide
levels (as a proxy for environmental outcomes). That analysis was done under the assumption that all
explanatory variables are exogenous. The actual contribution of the paper by FR is to look deeper
and examine the *causal* relationship between trade openness and environmental quality while both
controlling for income and appropriately dealing with the likely endogeneity of both income and
trade openness. To that end they used several instrumental variables to deal with these two
explanatory variables. You will replicate some of these results in the current exercise.

Use the Stata file *Frankel_Rose.dta*. This file contains data on 41 countries, collecting (in
addition to the variables mentioned in the previous exercise) the following instrumental
variables:

======================  ============    ============================================================
Instrument              IV for          Description
======================  ============    ============================================================
``trade_potential``     ``pwtopen``     Trade potential of a country. This variable combines
                                        information on a country's geographical location (number of
                                        neighbor countries, access to sea, landlock status),
                                        population size, land area and language to construct a
                                        measure of potential trade. For example, all else equal, a
                                        country with access to the sea will have a higher trade
                                        potential than a country that is landlocked. This IV is
                                        notably correlated with the endogenous regressor ``pwtopen``
                                        while plausibly uncorrelated with environmental outcomes.

``inc_exog``            ``inc``         Exogenous income of a country. While per capita income
                                        ``inc`` is likely endogenous, it contains some exogenous
                                        components. FR combine information on a country's lagged
                                        income as well as school attainment to construct the
                                        exogenous component of income. For example, all else equal,
                                        a country with higher average school attainment will have
                                        higher per capita income than a country with lower average
                                        school attainment. This IV is notably correlated with the
                                        endogenous regressor ``inc`` while plausibly uncorrelated
                                        with environmental outcomes.

``inc_exogsqr``         ``incsqr``      Since their model specification also includes the square of
                                        the logarithm of real per capita GDP (``incsqr``), FR also
                                        define ``inc_exogsq`` as the square of ``inc_exog`` and use
                                        this as an instrument for ``incsqr``.
======================  ============    ============================================================

a)  Re-estimate the basic model from Exercise 1) part a) using instrumental variables estimation
    instead. Use all three instruments and make your estimation robust to heteroskedasticity.
    
    What does this say about the impact of trade openness on :math:`\text{SO}_2` concentrations --
    i.e., on the relative importance of the ''race-to-the- bottom'' versus ''gains-from-trade''
    hypotheses alluded to earlier?

#)  Examining the first-stage regressions, do all three first-stage models have reasonably high
    adjusted :math:`R^2` values? Do you need to be concerned about *weak instruments*? (Use the
    'Rule of Thumb' explained in the textbook, section 12.3.)

#)  Using the insights gained from Exercise 1, re-estimate the model, replacing ``sulfdm`` and
    ``pwtopen`` by their logarithms, ``logsulfdm`` and ``logpwtopen``. How do your conclusions about
    the openness effect change?

#)  Test whether the OLS and 2SLS coefficient estimates are significantly different. (Hint: use the
    Hausman test; in Stata type ``help: hausman`` to learn how to use it.  Provide a brief
    explanation of what the Hausman test does.)

#)  In conclusion to Exercises 1 and 2, what is your answer to the question *Is globalization good
    or bad for the environment*? What are the strengths and weaknesses of the econometric analysis
    conducted here? Do you see any possible extensions that could help improve your research?

(*Note: This exercise is from the book ''Fundamentals of Applied Econometrics'' by Richard Ashley.*)

Exercise 3
=============

In the research paper ''Does Size Matter in Australia'', published in *The Economic Record* (Vol.
86, No. 272, March 2010, pp.71-83), Michael Kortt and Andrew Leigh address the research question::

    Do taller and slimmer workers earn more?

To that effect, they consider the following linear model:

.. math::
   W_i = \beta_0 + \beta_1 \text{Height}_i + \beta_2 \text{BMI}_i + \beta_3 X_{i3} + \cdots +
   \beta_k X_{ik} + u_i.

(This equation is my version of equation (1) on page 73 of their paper.) Here, :math:`W_i` is the
log hourly wage of person :math:`i`, :math:`\text{Height}_i` represents a person's height and
:math:`\text{BMI}_i` stands for a person's body mass index. The remaining regressors, :math:`X_{i3},
\ldots, X_{ik}` capture a person's demographic characteristics, including gender, age (linear and
quadratic) and education.

Obtain a copy of the paper (available online for ANU students and faculty) and answer the following
questions.

a)  Kortt and Leigh begin the analysis by estimating all coefficients by OLS. Summarize their OLS
    results regarding the two main coefficients of interest, :math:`\beta_1` and :math:`\beta_2`
    (for height and BMI). 

#)  Would you interpret these estimates as *causal*? What are the main endogeneity problems in this
    regression?

#)  Explain how Kortt and Leigh attempt to address the endogeneity problem using instrumental
    variables. How do their findings change?

#)  What is the main conclusion of the paper? Do taller and slimmer workers in Australia earn more?
    What is the evidence from other countries?





Assignment 2
****************

.. admonition:: Instructions

    Answer all questions!

    This assignment is due at 2.00pm on Wednesday, 29 October. It is worth 10% of your final mark for
    this course. Hand in your work by putting it in the EMET2008/6008 assignment box (HW Arndt
    Building 25a, opposite of room 1002). Absolutely no extensions will be given, late assignments
    will receive zero credit. If you have a university approved excuse for not handing in this
    assignment, then your marks for your final exam will be weighted up by 10% to compensate for the
    missed work.  
   
    While I would prefer it if you could provide *typed* answers, you may also hand in written
    answers as long as they are legible and easy to follow. (I will not only mark the correctness of
    your answers but also the clarity of the exposition and the transparency with which you
    communicate your results.) The work that you hand in should consist of answers to the questions,
    together with an appendix which contains both the printout of a complete Stata do-file and a
    log-file (prodced by the do-file) that covers the entire assignment.
    
    Answers should be in sentence form (i.e. single word or single number answers without
    explanation will be considered incomplete), but clarity of presentation is important, so try to
    make your comments/discussion brief and to the point. Annotated output does not constitute a
    sufficient answer to any question, but you should highlight those parts of your output that you
    explicitly use in your answers.

    If you have any questions regarding to these instructions, do not hesitate to ask me (either
    during class meetings or consultation times or e-mail.)

Exercise 1 
============= 

The data set ``PNTSPRD`` (available on Wattle) contains information from the Las Vegas sport betting market. The
overarching research question is whether the favorite team is more likely to win the game.

Consider the linear probability model

.. math::
    \Pr(favwin=1 | spread) = \beta_0 + \beta_1 spread,

where ``spread`` is a proxy for the favorite team. A high point spread means that a team is the
favorite. Here a quick primer on point spread betting from Wikipedia::

    The general purpose of spread betting is to create an active market for both sides of a binary
    wager []. If the wager is simply "Will the favorite win?", more bets are likely to be made for
    the favorite, possibly to such an extent that there would be very few betters willing to take
    the underdog.  
    
    The point spread is essentially a handicap towards the underdog. The wager becomes "Will the
    favorite win by more than the point spread?" The point spread can be moved to any level to
    create an equal number of participants on each side of the wager. This allows a bookmaker to act
    as a market maker by accepting wagers on both sides of the spread. The bookmaker charges a
    commission, or vigorish, and acts as the counterparty for each participant. As long as the total
    amount wagered on each side is roughly equal, the bookmaker is unconcerned with the actual
    outcome; profits instead come from the commissions.

    (excerpt taken on October 7, 2014)


(a)     Explain why, if the spread incorporates all relevant information, we expect
        :math:`\beta_0=0.5`?

(b)     Estimate the linear probability model. Test the hypothesis :math:`\beta_0=0.5` against a
        two-sided alternative. (Make all estimations robust to heteroskedasticity throughout this
        entire exercise.)

(c)     Is spread statistically significant? What is the estimated probability that the favored team
        wins when :math:`spread=10`?

(d)     Now estimate the model by probit. Interpret and test the hypothesis that the intercept is
        equal to 0.5?

(e)     Use the probit model to estimate the probability that the favored team wins when
        :math:`spread=10`. Compare this with the linear probability model.

(f)     Add the variables ``favhome``, ``fav25``, and ``und25`` to the probit model and test joint
        significance of these variables.

(g)     Redo parts (d), (e), and (f) using the logit model.

(h)     Which sport is this exercise about?

(*Note: This exercise is from the book ''Introductory Econometrics: A Modern Approach'' by Jeffrey Wooldridge*)

Exercise 2
==============

Krueger and Maleckova, in their paper ''Education, Poverty and Terrorism: Is There a Causal
Connection?'', published in the *Journal of Economic Perspectives*
(2003), attempt to estimate the causal effect of education and poverty on terrorism.

(a)     What is the main research question of the paper?

(b)     What econometric method do they use to estimate causal effects?

(c)     What is the main outcome variable?

(d)     What are the main explanatory variables?

(e)     What other explanatory variables do they include?

(f)     What is their main finding?

(g)     What problems/shortcomings do you see in their research? 

   

Exercise 3
=============

The data set ``Airfare`` (available on Wattle) contains information on airfares, passenger volume,
flight distance and market concentration for 1,149 flight routes (connections) for the years 1997
through 2000. The overarching research question is whether increased competition reduces air fares.
(Do connections with less market concentration have cheaper prices?) The data set contains the
following variables:

================    =============================================================================
Variable            Description 
================    =============================================================================
``year``            year: 1997, 1998, 1999, 2000
``id``              route identifier

                    (the subject of analysis are flight routes)
``dist``            distance of flight route (in miles)
``passen``          average number of passengers per day
``fare``            average one way airfare, \$
``bmktshr``         market share, biggest carrier

                    (proxy variable for market concentration)
================    =============================================================================

The main explanatory variable is ``bmktshr``. A higher value of ``bmktshr`` implies higher market
concentration on that route and therefore less competition.

Consider the following linear model:

.. math::
    \log(fare)_{it} = \eta_t + \beta_1 bmktshr_{it} + \beta_2 \log(dist)_{it} + \beta_3
    [ \log(dist)_{it}]^2 + \alpha_i + u_{it},

where :math:`\eta_t` means that we allow for different year intercepts.

(a)     Estimate the above linear model separately for all four years. If :math:`\Delta bmktshr =
        0.1`, what is the estimated percentage increase in ``fare``? (Make all estimations robust to
        heteroskedasticity throughout this entire exercise.)

(b)     Run pooled OLS across all years, i.e. treat the data as if it were one big regression and
        control for years by including year dummies. What is your estimate of :math:`\beta_1`? Is it
        significant?

(c)     For what value of ``dist`` does the relationship between ``log(fare)`` and ``dist`` become
        positive? 

(d)     Estimate the linear model using fixed effects. What is the fixed effect estimate of
        :math:`\beta_1`?

(e)     Add the logarithm of ``passen`` to the model. How do your estimates change?  In summary,
        does higher concentration (i.e, higher ``bmktshr``) on a route increase air fares? What is
        your best estimate? 

(f)     Name two characteristics of a route (other than distance) that are captured by
        :math:`\alpha_i` and that are correlated with ``bmktshr``.

(*Note: This exercise is from the book ''Introductory Econometrics: A Modern Approach'' by Jeffrey Wooldridge*)
