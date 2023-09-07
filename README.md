# ml-lrc | ML for Littlewood-Richardson numbers

### 2023.6.29

HornTable5.5.csv contains all 167,594 data points of the form c,lambda,mu,nu.  Lambda, mu, and nu are required to lie in the $5\times 5$ box &mdash; that is, they have at most 5 parts, and the largest part is at most 5.  They are required to satisfy the homogeneity equation $\sum \lambda_i + \sum \mu_i = \sum \nu_i$, as well as the containment conditions $\lambda_i\leq \nu_i$ and $\mu_i\leq \nu_i$.  The labels are c=0 when $c_{\lambda,\mu}^\nu=0$ and c=1 when $c_{\lambda,\mu}^\nu\neq0$.

Horn5.5Balanced.csv contains 100k data points of the form c,lambda,mu,nu.  They are evenly balanced between 50k randomly chosen examples of c=0 and 50k randomly chosen cases of c=1.

Horn5.5train.csv contains 70k data points from the balanced set, still evenly balanced between c=0 and c=1.

Horn5.5test.csv contains the remaining 30k points.

Horn5.train.conf and Horn5.predict.conf are the LightGBM config files.  The full model is too big to host here now, but at approximately 35MB, not actually very big at all.  LightGBM generated it after 5000 iterations, in just about 1s.

Horn5OnesOD.csv and Horn5ZeroesOD.csv together contain the 67,594 points not used in training or testing, separated into c=1 (21,973) and c=0 (45,621).  The confusion matrix for running the model on these points is as follows:

$$\begin{array}{c|c|c|} \text{actual}\backslash\text{predicted} & 0 & 1  \newline \hline 0 & 43699 & 1922  \newline \hline 1 & 89 & 21884  \end{array}$$

So it correctly predicted c=0 with 95.79% accuracy, and correctly predicted c=1 with 99.59% accuracy.

LRTableMaker is a text file with code to ask Maple to produce the tables.  It requires Anders Buch's [lrcalc](https://sites.math.rutgers.edu/~asbuch/lrcalc/).  LRtable-maker.mw is a Maple worksheet demo.

Next steps:
* See if the model notices the inequalities defining the Horn cone.  At a glance, the decision trees seem to give more importance to the larger parts of a partition.  But I don't know how to precisely formulate a statement about this.
* Incorporate confidence information.  That is, the model produces floating-point decimals between 0 and 1, and I simply rounded to the nearest integer to determine its prediction.  Are confusions clustered around 0.5, i.e., around less confident predictions?
* To look for improvement with more data, train a model on partitions in the 6-by-6 box, with 4,812,643 data points.  Or possibly a 5-by-n bounding rectangle.
* Train a model on more distinctly labeled data, say with c=0, c=1, or c=2 (the latter representing LR numbers >=2).  Look for signs that the model recognizes that $c_{\lambda,\mu}^\nu=1$ means $\lambda,\mu,\nu$ is a ray of the Horn cone.
* Possibly re-label $\nu$ by $\nu^\vee$, for the symmetric version $c_{\lambda,\mu,\nu}$.
* Do the analogous analysis for Schubert structures constants $c_{u,v}^w$.  Here $u,v,w$ are permutations, and we seek the coefficient for multiplication in the Schubert polynomial basis.  In contrast to the LR numbers, a concise combinatorial rule for $c_{u,v}^w$ remains elusive.

### 2023.8.16

Update: I trained another LightGBM model on the LR numbers for partitions in a 6-by-6 box.  The training data this time consisted of 1M data points, balanced as before between 500k randomly sampled 1's and 500k randomly sampled 0's.  The confusion matrix for the remaining 3,812,641 points is as follows:

$$\begin{array}{c|c|c|} \text{actual}\backslash\text{predicted} & 0 & 1  \newline \hline 0 & 2,257,325 & 227,603  \newline \hline 1 & 12,247 & 1,315,467  \end{array}$$

So it predicts c=0 with 90.84% accuracy, and c=1 with 99.08% accuracy.

The accuracy declined somewhat from n=5, which is disappointing.  Some notes: I used the same number of iterations (5000), which took about 230s this time.  Possibly increasing the number of iterations would help.  Less optimistically, the percentage of training data used was smaller, just over 1/5 of the total for n=6 rather than about 1/2 as for n=5; presumably increasing the relative size of the training could help.

### 2023.9.7

Using 25000 iterations increased accuracy significantly for the 6-by-6 case.  Same data as before, the confusion matrix is now

$$\begin{array}{c|c|c|} \text{actual}\backslash\text{predicted} & 0 & 1  \newline \hline 0 & 2,423,479 & 61,449  \newline \hline 1 & 2,008 & 1,325,706  \end{array}$$

So it predicts c=0 with 97.53% accuracy, and c=1 with 99.85% accuracy.
