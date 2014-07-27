Illustration of Central Limit Theorem
========================================

Bla bla bla

.. plot:: 

    import numpy as np
    import pylab as P
    import random
    from scipy.stats import expon

    P.plt.subplots_adjust(hspace=0.4)

    # histogram of population
    P.subplot(321)
    N = 10000    # size of population
    mu = sigma = 1
    bins = np.linspace(0, 4, 50)
    y = expon.pdf(bins)   # standard normal density
    P.title(r'Population density: exponential distribution with $\lambda=1$')
    P.plot(bins, y, 'k--', linewidth=3)
    Y = np.random.exponential(scale=mu, size=(N, 1))

    # histogram of sample average, n = 500
    P.subplot(322)
    n = 1     # sample size
    R = 10000    # number of repetitions
    Ybar = []
    for i in range(1, R):
        Ybar += [np.sqrt(n) * (np.mean(random.sample(Y, n)) - mu)]
    bins = np.linspace(-4, 4, 50)
    P.ylim(0, 0.5)
    P.hist(Ybar, bins, normed=True)
    P.title(r'Histogram of $\sqrt{1} (\bar{Y}_{1} - 1)$')
    y = P.normpdf(bins, 0, 1)   # standard normal density
    P.plot(bins, y, 'k--', linewidth=3)

    # histogram of sample average, n = 250
    P.subplot(323)
    n = 5     # sample size
    Ybar = []
    for i in range(1, R):
        Ybar += [np.sqrt(n) * (np.mean(random.sample(Y, n)) - mu)]
    P.ylim(0, 0.5)
    P.hist(Ybar, bins, normed=True)
    P.title(r'Histogram of $\sqrt{5} (\bar{Y}_{5} - 1)$')
    y = P.normpdf(bins, 0, 1)   # standard normal density
    P.plot(bins, y, 'k--', linewidth=3)


    # histogram of sample average, n = 50
    P.subplot(324)
    n = 10      # sample size
    Ybar = []
    for i in range(1, R):
        Ybar += [np.sqrt(n) * (np.mean(random.sample(Y, n)) - mu)]
    P.ylim(0, 0.5)
    P.hist(Ybar, bins, normed=True)
    P.title(r'Histogram of $\sqrt{10} (\bar{Y}_{10} - 1)$')
    y = P.normpdf(bins, 0, 1)   # standard normal density
    P.plot(bins, y, 'k--', linewidth=3)

    # histogram of sample average, n = 5
    P.subplot(325)
    n = 30       # sample size
    Ybar = []
    for i in range(1, R):
        Ybar += [np.sqrt(n) * (np.mean(random.sample(Y, n)) - mu)]
    P.ylim(0, 0.5)
    P.hist(Ybar, bins, normed=True)
    P.title(r'Histogram of $\sqrt{30} (\bar{Y}_{30} - 1)$')
    y = P.normpdf(bins, 0, 1)   # standard normal density
    P.plot(bins, y, 'k--', linewidth=3)

    # histogram of sample average, n = 5
    P.subplot(326)
    n = 100       # sample size
    Ybar = []
    for i in range(1, R):
        Ybar += [np.sqrt(n) * (np.mean(random.sample(Y, n)) - mu)]
    P.ylim(0, 0.5)
    P.hist(Ybar, bins, normed=True)
    P.title(r'Histogram of $\sqrt{100} (\bar{Y}_{100} - 1)$')
    y = P.normpdf(bins, 0, 1)   # standard normal density
    P.plot(bins, y, 'k--', linewidth=3)


    P.show()
