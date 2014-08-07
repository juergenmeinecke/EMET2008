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



Week 4
=======

Problem Solving
--------------------

Coming soon



Computational
--------------------

#)  Empirical Exercise E6.3. 

#)  In EMET2007 you (hopefully!) have learned how to test for homoskedasticity versus
    heteroskedasticity. How would you do this with Stata? (Use the `Growth` data set from the
    previous exercise to illustrate the test.) If you indeed find that the data is heteroskedastic,
    how would you correct for it with Stata?

#)  Coming soon
