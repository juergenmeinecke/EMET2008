Exercises
***************

.. note:: 
   
    Answers to exercises will only be provided during class time. If you cannot make it to class,
    you will need to see me during consultation times and we will work through the exercises
    together. (When you see me during consultation times, I expect you to be prepared. I will never
    merely provide answers to exercises. Instead, I want to see good faith effort on your part in
    which case I will be more than happy to help you work throught the exercises.) 

Week 1
=======

Problem Solving
--------------------

#) Prove that the sample average :math:`\bar{Y}` is an unbiased estimator for the population mean.    

#) Prove that in the linear model :math:`Y_i = \mu + \varepsilon_i` the ordinary least squares
   estimator of :math:`\mu` is equal to the sample average. Mathematically, minimize the sum of
   least squares :math:`\sum_{i=1}^n (Y_i - \hat{\mu})^2` and show that this is obtained by setting
   :math:`\hat{\mu} = \bar{Y}`.

#) Is there a difference between an estimator and an estimate?


Computational
--------------------

We will use this first practice session to become familiar with Stata. 

#)  Work through the "Stata for Researchers" website, you can find the link below.

I will use this exercise to teach you some basic tricks you should know about Stata. For Stata help
and support I can highly recommend the following two sources:


*   The Social Science Computing Cooperative at the University of Wisconsin provides excellent
    support for Stata beginners. Check out their website "Stata for Students":
        
    <http://www.ssc.wisc.edu/sscc/pubs/stata_students1.htm>

    Also, check out their website "Stata for Researchers":
        
    <http://www.ssc.wisc.edu/sscc/pubs/sfr-intro.htm>

    This website should be your first port of call in all things Stata.

*   Furthermore, the UCLA Institute for Digital Research and Education provides fantastic resources
    for people who are interested in learning Stata:

    <http://www.ats.ucla.edu/stat/stata/>

Feel free to use these links throughout the semester to improve your Stata skills!


Week 2
=======

Problem Solving
--------------------

#) Let :math:`Y_i \sim \text{i.i.d.}(\mu, \sigma^2)`. You have learnt in the lecture that 
   :math:`\hat{\mu}_1 := \bar{Y}_n` is an unbiased and consistent estimator for the
   population mean :math:`\mu`. Are the following estimators also unbiased or consistent for
   :math:`\mu`? Discuss!

   a)   :math:`\hat{\mu}_2 := 42 \qquad`       ('the answer to everything' estimator) 

   b)   :math:`\hat{\mu}_3 := \bar{Y}_n + 3/n`   

   c)   :math:`\hat{\mu}_4 := (Y_1 + Y_2 + Y_3 + Y_4 + Y_5)/5`   

#) Excerpt from the website of the Australian Bureau of Statistics::

        "The Adult Literacy and Life Skills Survey (ALLS) was conducted in Australia as part of an
        international study coordinated by Statistics Canada and the Organisation for Economic
        Co-operation and Development (OECD). The ALLS is designed to identify and measure literacy
        which can be linked to the social and economic characteristics of people both across and
        within countries. The ALLS measured the literacy of a sample of people aged 15 to 74 years.
        The ALLS provides information on knowledge and skills in (among others) *Numeracy*, i.e.
        the knowledge and skills required to effectively manage and respond to the mathematical
        demands of diverse situations."

   In a sample of 1,000 randomly selected Australians, the average numeracy score was 312 and the
   sample standard deviation was 41. Construct a 95% confidence interval for the population mean of
   the numeracy score.

#) Exercise 3.3 parts a and b; Exercise 3.4.

Computational
--------------------

#)  Continue working through the "Stata for Researchers" website (as started last week).

#)  Empirical Exercise E4.2 parts a and b.





Week 3
=======

Problem Solving
--------------------

Consider the following linear model for heights:

.. math::
    Y_i = \beta_0 + \beta_1 X_{i1} + u_i,

where :math:`Y_i` is the height of person :math:`i` and :math:`X_{i1}` is a gender dummy variable
that takes on the value 1 if person :math:`i` is male and zero otherwise.

#) In that model, what does :math:`\beta_0` capture? What does
   :math:`\beta_0 + \beta_1` capture?  

#) Define and derive (mathematically) the OLS estimators of :math:`\beta_0` and :math:`\beta_1`.  





Computational
--------------------

#)  Empirical Exercise E3.1 part d.

#)  Empirical Exercise E4.4. 

    The following Stata do-file is a solution to this exercise. Please feel free to use this as the
    starting point for all your future do-files. Just copy and paste it into your Stata do-file
    editor and save it as a new do-file. Since it contains the answer to Empirical Exercise 4.4, I
    gave it the name "E4_4.do", but you can choose whatever name you want.

    What's important here is that you need to customize the code below in one place:

    *   Work directory: This is the location on your computer where your access and store all your
        files. This includes the textbook's data files (the files with a dta-suffix), your
        self-written Stata do-files as well as the log-files that are created by your do-files. You
        choose the work directory; it is likely different on different computers. For example, on my
        office desktop computer I created a work directory called ``/Users/juergen/EMET2008/Stata``. 

    For the code below to work, you need to keep ALL files in the same work directory! Again, this
    includes your textbook's data files as well as the Stata do-files and log-files that you
    create.::
        
        // ====================================================
        // PREAMBLE
        // ====================================================
        clear all		// clear memory
        capture log close	// close any open log files
        set more off		// don't pause when screen fills

        
        // set work directory (put your own path here!):
        cd /path/to/location/on/your/computer/where/Stata/files/go

        log using E4_4.log, replace	    // open new log-file 

        // ====================================================
        // Work on your data set
        // ====================================================

        use "Growth.dta"   // loading data set (needs to be in work directory)
        summarize
        scatter growth tradeshare

        regress growth tradeshare
        margins, at(tradeshare==1)
        margins, at(tradeshare==0.5)

        regress growth tradeshare if country_name!="Malta"
        margins, at(tradeshare==1)
        margins, at(tradeshare==0.5)

        log close	// close log-file



Week 4
=======

Problem Solving
--------------------

#)  Derive the bias from omitted variables.

#)  In a recent applied econometrics research project, I have been interested in the causal effect of
    academic fraud on labor market outcomes. The broad research question is: Do people who commit
    academic fraud (at university) benefit significantly from it? Sounds like a straightforward
    research question, but answering it is quite challenging econometrically. 

    Let's say the model looks like
  
    .. math::
   
       Y_i = \beta_0 + \beta_1 Fraud_i + \beta_2 Male_i + \beta_3 Educ_i + \beta_4 Age_i + u_i,

    where :math:`Y_i` are weekly earnings (full time), :math:`Fraud_i` is a dummy variable that is
    equal to one if a person reported that s/he committed academic fraud during university and zero
    otherwise. (All other rhs variables are self-explanatory.)

    If I run this regression and obtain the estimate :math:`\hat{\beta}_1` for :math:`\beta_1`, can I
    interpret this as the causal effect of academic fraud on earnings? Discuss!


Computational
--------------------

#)  Empirical Exercise E6.3. 

    Solution::
 
        // ====================================================
        // PREAMBLE
        // ====================================================
        clear all	        // clear memory
        capture log close	// close any open log files
        set more off		// don't pause when screen fills

        // set work directory (put your own path here!):
        cd /path/to/location/on/your/computer/where/Stata/files/go
        log using E6_3.log, replace		// open new log-file 

        // ====================================================
        // Work on your data set
        // ====================================================

        use "./Stock_data/Growth.dta"

        drop if (country_name=="Malta")
        summarize
        reg growth tradeshare yearsschool rev_coups assasinations rgdp60

        margins, atmeans
        margins, at((mean) _all  tradeshare=.771)


        // checking for heteroskedasticity
        // 1) pedestrian way
        predict uhat, res
        generate uhatsq = uhat^2
        regress uhatsq tradeshare yearsschool rev_coups assasinations rgdp60

        // null: homoskedasticity
        // check F-stat

        // 2) lazy way
        reg growth tradeshare yearsschool rev_coups assasinations rgdp60
        estat hettest, rhs fstat

        log close	// close log-file



#)  In EMET2007 you (hopefully!) have learned how to test for homoskedasticity versus
    heteroskedasticity. How would you do this with Stata? (Use the `Growth` data set from the
    previous exercise to illustrate the test.) If you indeed find that the data is heteroskedastic,
    how would you correct for it with Stata?




Week 5
=========

Problem Solving
------------------

Consider the simple linear model :math:`Y_i = \beta_0 + \beta_1 X_i + u_i`. 

#) Mathematically define the OLS estimator and prove that it is inconsistent under endogeneity.

#) Mathematically define the TSLS estimator and prove that it is consistent under endogeneity.

#) Which of the two estimators is consistent under exogeneity?   

#) Research question: Do girls who attend girls' schools do better in math than girls who attend
   coed schools? I give you a data set that includes the following variables:

   *    *score*: score in a standardized math test
   *    *girlshs*: dummy variable which is equal to 1 if a person attended girls' school or zero
        otherwise
   *    *fecud*: father's education
   *    *meduc*: mother's education
   *    *hhinc*: household income

   a)   You run an OLS estimation of *score* on *girlshs* and all the other variables. Will your OLS
        estimate of the coefficient on *girlshs* capture the causal effect of girls' school on math
        score? If not, why not? 

   b)   What would be a good instrumental variable for *girlshs*?

   Note: this exercise is based on Wooldridge, *Introductory Econometrics, A Modern Approach*, 5th
   edition, chapter 15.


Computational
---------------

#) Empirical Exercise E12.2 (Stock and Watson book)


Week 6
========

Problem Solving
-------------------

Cool things can be done with randomized control trials. Here I expose you to the work of two recent
economics papers published in a top field journal.

#)  We will read and discuss the paper on the effects of home computer use on academic achievement
    of school children (written by Fairlie and Robinson). 

    Here the paper for download (with my annotations): :download:`Fairlie Robinson 2013 <./_static/AEJ_FR2013.pdf>`

#)  We will read and discuss the paper on the effects of dropping schools by helicopter on rural
    villages in Afghanistan (written by Burde and Linden).
    
    Here the paper for download (with my annotations): :download:`Burde Linden 2013 <./_static/AEJ_BL2013.pdf>`


Computational
--------------

#)  Empirical Exercise E13.1 (Stock and Watson book)


    
Week 7
========

Midterm exam


Week 8
=======

Problem Solving
-------------------

We will review the midterm exam. In particular: Q1, Q2 and Q5. (The other two questions are easy to
answer if you have read the papers.)

Computational
--------------

#)  Empirical Exercise E11.1 (Stock and Watson book)

    Solution to part (f) ::

        twoway function y = _b[_cons] + _b[age] * x + _b[agesq]* x^2 + _b[colgrad], range(18 65)

    
Week 9
=======

Problem Solving
-------------------

Maximum likelihood estimation of probit and logit coefficients.

#)  Define the maximum likelihood estimator.

#)  Derive the maximum likelihood estimator.

#)  Discuss statistical inference for the probit and logit coefficients.

#)  Discuss consistency of the probit and logit estimators.

Note: In contrast to the linear probability model (which is a linear model that can be estimated
straightforwardly by OLS) the probit and logit models are non-linear (remember that S-shaped curve
from the lecture?). Non-linear models are considerably more difficult to estimate. In this problem
solving session I will try to explain to you the principle idea and math of maximum likelihood
estimation of probit and logit models. In the end, the estimation will need to be done by computers.
Luckily, Stata offers a nice set of commands to help out.


Computational
--------------

#)  Empirical Exercise E11.2 (Stock and Watson book)

    Solution::
 
        // ====================================================
        // PREAMBLE
        // ====================================================
        clear all	        // clear memory
        capture log close	// close any open log files
        set more off		// don't pause when screen fills

        // set work directory (put your own path here!):
        cd /path/to/location/on/your/computer/where/Stata/files/go
        log using E11_2.log, replace		// open new log-file 

        // ====================================================
        // Work on your data set
        // ====================================================

        use "./Stock_data/Smoking.dta"

        summarize

        ******* a ***********
        generate agesq = age^2
        probit smoker smkban female age agesq hsdrop hsgrad colsome colgrad black hispanic, robust

        ******* b ***********
        * just read off from probit output in part a
        
        ******* c ***********
        test hsdrop hsgrad colsome colgrad

        ******* d ***********
        margins, at(smkban=(0 1) female=0 age=20 agesq=400 hsdrop=1 hsgrad=0 colsome=0 colgrad=0 black=0 hispanic=0)
        margins, dydx(smkban) at(female=0 age=20 agesq=400 hsdrop=1 hsgrad=0 colsome=0 colgrad=0 black=0 hispanic=0)

        ******* e ***********
        margins, at(smkban=(0 1) female=1 age=40 agesq=1600 hsdrop=0 hsgrad=0 colsome=0 colgrad=1 black=1 hispanic=0)
        margins, dydx(smkban) at(female=1 age=40 agesq=1600 hsdrop=0 hsgrad=0 colsome=0 colgrad=1 black=1 hispanic=0)

        ******* f ***********
        regress smoker smkban female age agesq hsdrop hsgrad colsome colgrad black hispanic, robust
        margins, at(smkban=(0 1) female=0 age=20 agesq=400 hsdrop=1 hsgrad=0 colsome=0 colgrad=0 black=0 hispanic=0)
        margins, dydx(smkban) at(female=0 age=20 agesq=400 hsdrop=1 hsgrad=0 colsome=0 colgrad=0 black=0 hispanic=0)
        margins, at(smkban=(0 1) female=1 age=40 agesq=1600 hsdrop=0 hsgrad=0 colsome=0 colgrad=1 black=1 hispanic=0)
        margins, dydx(smkban) at(female=1 age=40 agesq=1600 hsdrop=0 hsgrad=0 colsome=0 colgrad=1 black=1 hispanic=0)

        log close	// close log-file

    
Week 10
=======

Problem Solving
-------------------

We will briefly revisit last week's problem solving session to summarize ML estimation of probit and
logit models.

Computational
--------------

#)  Revisit Empirical Exercise E11.2 (Stock and Watson book)

#)  Empirical Exercise E10.1 (Stock and Watson book)

    (a)     Regress ``lnvio`` on ``shall`` separately for the years 1977 and 1999. What is the
            causal effect?

    (#)     Run a pooled regression across all years.

    (#)     Can you think of an unobserved variable that varies by state but not across time? How
            about one that varies across time but not by state?

    (#)     Reshape your data from long format to wide format. Use the reshaped data to create
            differenced variables (between 1999 and 1977) for ``lnvio`` and ``shall``.

    (#)     Run a regression of the differences. What is the causal effect? How does it compare to
            part (a)? Why should the estimate be different theoretically?
            
            For the rest of this exercise, reshape your data back into long format. (Simply reload
            the original data set.)

    (#)     Run an :math:`(n-1)`-binary regressors estimation of ``lnvio`` on ``shall``. 

    (#)     Run a fixed effects estimation of ``lnvio`` on ``shall``. Do it in two different ways:
           
            (i)     Hard way: demean the variables yourself and regress demeaned variables on each
                    other.

            (ii)    Lazy way: use Stata's inbuilt fixed effect estimation command.

            How do the results differ to part (f)? 

    (#)     Add the explanatory variables ``incarc_rate``, ``density``, ``avginc``, ``pop``,
            ``pb1064``, ``pw1064``, and ``pm1029`` to the estimation.

    (#)     Now also control for time fixed effects. Do it in three different (yet equivalent) ways:

            (i) Entity demeaning with :math:`(T-1)`-binary time indicators

            (#) Time demeaning with :math:`(n-1)`-binary entity indicators

            (#) :math:`(T-1)`-binary time indicators and :math:`(n-1)`-binary entity indicators


    (#)     Redo the main estimation using the logarithms of ``rob`` and ``mur`` instead of ``vio``
            as outcome variables. How do your findings change?


Week 11
=======

Problem Solving
-------------------

Define and derive the fixed effect estimator.


Computational
--------------

Continue working on Empirical Exercise E10.1 (Stock and Watson book), see previous week.


Week 12
=======

Problem Solving
-------------------

No problem solving this week.


Computational
--------------

Empirical Exercise E15.1 (Stock and Watson book).

(Note: you need to import the Excel spreadsheet that holds the data and save the imported data as a
dta-file before you start working.)

    Solution::
 
        // ====================================================
        // PREAMBLE
        // ====================================================
        clear all	        // clear memory
        capture log close	// close any open log files
        set more off		// don't pause when screen fills

        // set work directory (put your own path here!):
        cd /path/to/location/on/your/computer/where/Stata/files/go
        log using E15_1.log, replace		// open new log-file 

        // ====================================================
        // Work on your data set
        // ====================================================

        use "./Stock_data/USMacro_Monthly.dta"

        summarize

        *********** part a
        generate time = m(1947m1) + _n-1
        format t %tm
        tsset time

        generate L_IP = L1.IP
        generate ip_growth = 100 * log(IP/L_IP)
        summarize if tin(1952m1, 2009m12)

        *********** part b
        tsline Oil


        *********** part c and d
        generate D_Oil = D.Oil

        * non-cumulative effects
        newey ip_growth L(0/18).Oil if tin(1947m1, 2009m12), lag(7)
        testparm L(0/18).Oil

        * cumulative effects (see eq. 15.7 in Stock and Watson, 3rd ed)
        newey ip_growth L(0/17).D_Oil L(18/18).Oil if tin(1947m1, 2009m12), lag(7)


        ************ part e
        * do these plots in Excel, Stata is to awkward for plotting this stuff

        log close	// close log-file


