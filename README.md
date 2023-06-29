# ml-lrc | ML for Littlewood-Richardson numbers

HornTable5.5.csv contains all 167,594 data points of the form c,lambda,mu,nu.  Lambda, mu, and nu are required to lie in the $5\times 5$ box &mdash; that is, they have at most 5 parts, and the largest part is at most 5.  They are required to satisfy the homogeneity equation $\sum \lambda_i + \sum \mu_i = \sum \nu_i$, as well as the containment conditions $\lambda_i\leq \nu_i$ and $\mu_i\leq \nu_i$.  The labels are c=0 when $c_{\lambda,\mu}^\nu=0$ and c=1 when $c_{\lambda,\mu}^\nu\neq0$.

Horn5.5Balanced.csv contains 100k data points of the form c,lambda,mu,nu.  They are evenly balanced between 50k randomly chosen examples of c=0 and 50k randomly chosen cases of c=1.

Horn5.5train.csv contains 70k data points from the balanced set, still evenly balanced between c=0 and c=1.

Horn5.5test.csv contains the remaining 30k points.

Horn5.train.conf and Horn5.predict.conf are the LightGBM config files.  The full model is too big to host here now, but at approximately 35MB, not actually very big at all.  LightGBM generated it after 5000 iterations, in just about 1s.

Horn5OnesOD.csv and Horn5ZeroesOD.csv together contain the 67,594 points not used in training or testing, separated into c=1 (21,973) and c=0 (45,621).  The confusion matrix for running the model on these points is as follows:

$$\begin{array}{c|c|c|} \text{actual}\backslash\text{predicted} & 0 & 1  \newline 0 & 43699 & 1922  \newline 1 & 89 & 21884  \end{array}$$

So it correctly predicted c=0 with 95.79% accuracy, and correctly predicted c=1 with 99.59% accuracy.

LRTableMaker is a text file with code to ask Maple to produce the tables.  It requires Anders Buch's [lrcalc](https://sites.math.rutgers.edu/~asbuch/lrcalc/).  LRtable-maker.mw is a Maple worksheet demo.

Next steps:
* See if the model notices the inequalities defining the Horn cone.  At a glance, the decision trees seem to give more importance to the larger parts of a partition.  But I don't know how to precisely formulate a statement about this.
* Incorporate confidence information.  That is, the model produces floating-point decimals between 0 and 1, and I simply rounded to the nearest integer to determine its prediction.  Are confusions clustered around 0.5, i.e., around less confident predictions?
* To look for improvement with more data, train a model on partitions in the 6-by-6 box, with 4,812,643 data points.  Or possibly a 5-by-n bounding rectangle.
* Train a model on more distinctly labeled data, say with c=0, c=1, or c=2 (the latter representing LR numbers >=2).  Look for signs that the model recognizes that $c_{\lambda,\mu}^\nu=1$ means $\lambda,\mu,\nu$ is a ray of the Horn cone.
* Possibly re-label $\nu$ by $\nu^\vee$, for the symmetric version $c_{\lambda,\mu,\nu}$.
