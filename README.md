# ml-lrc | ML for Littlewood-Richardson numbers

HornTable5.5.csv contains all 167,594 data points of the form c,lambda,mu,nu.  Lambda, mu, and nu are required to lie in the $5\times 5$ box &mdash; that is, they have at most 5 parts, and the largest part is at most 5.  They are also required to satisfy the homogeneity equation $\sum \lambda_i + \sum \mu_i = \sum \nu_i$.  The labels are c=0 when $c_{\lambda,\mu}^\nu=0$ and c=1 when $c_{\lambda,\mu}^\nu\neq0$.

Horn5.5Balanced.csv contains 100k data points of the form c,lambda,mu,nu.  They are evenly balanced between 50k randomly chosen examples of c=0 and 50k randomly chosen cases of c=1.

Horn5.5train.csv contains 70k data points from the balanced set, still evenly balanced between c=0 and c=1.

Horn5.5tes.csv contains the remaining 30k points.
