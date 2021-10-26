# slamlib
A collect of libraries for SLAM, for my own reference

1. [g2o](https://github.com/RainerKuemmerle/g2o)
2. [GTSAM](https://github.com/borglab/gtsam)
3. [Ceres](https://github.com/ceres-solver/ceres-solver)
4. [SE-Sync](https://github.com/david-m-rosen/SE-Sync)

# papers
1. [A paper that compares the four](https://lamor.fer.hr/images/50036607/2021-ajuric-comparison-mipro.pdf)
[The SE-Sync paper](https://journals.sagepub.com/doi/full/10.1177/0278364918784361).
An oversimplied interpretation of main conclusions of this paper is that GTSAM and SE-Sync is generally at least not worse than alternatives. 
Considering that SE-Sync is not as mature as GTSAM, STSAM could be used as a starting point if one needs to experiment with SLAM.
2. [Factor graphs and gtsam a hands-on introduction](https://www.cc.gatech.edu/~dellaert/FrankDellaert/Frank_Dellaert/Entries/2013/5/10_Factor_Graphs_Tutorial_files/gtsam.pdf)
GTSAM incorporates the work of [iSAM2](https://www.cs.cmu.edu/~kaess/pub/Kaess12ijrr.pdf), which is designed to solve graph optimization problem while the graph is growing, 'i' in "iSAM" stands for "incremental". The resulted nonlinear least square problem is solved using generic solvers with sparse block matrix computation.
4. [The g2o paper](http://europa.informatik.uni-freiburg.de/files/kuemmerle11icra.pdf) g2o is like GTSAM,it also model the problem domain with a graph model. Its distincitve feature
is that it uses the square-add operator ⊞. The square-add operator is used where a generic optimization solvers uses "+" to generate a one-step update of the estimated value.
5. ["Least Squares Optimization: From Theory to Pratice" by Giorgio Grisetti, et al.](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=&cad=rja&uact=8&ved=2ahUKEwink56VzefzAhVayYsBHf-NAq0QFnoECBUQAQ&url=https%3A%2F%2Fres.mdpi.com%2Fd_attachment%2Frobotics%2Frobotics-09-00051%2Farticle_deploy%2Frobotics-09-00051-v2.pdf&usg=AOvVaw2NDJ5w4OH4mi8rOI1Wql9F),
an interesting paper that tries to design a module-based composable and configurable unified interface to the optimization problem.
# Comments
1. Special attention should be paid to the SE-Sync paper. The analysis of the paper reveals some very
fascinating facts about StherLAM problems:
    - The graph optimization problem has an equivalent representation of Rotation Only Optimization.
    - The Rotation Only Optimization problem can be relaxed to a Convex Semidefinite problem if replacing the SO(n) group by the  O(n) group.
    - The Convec semidefinite problem can be solved by "go up" the Remanian staircase, with upper "stair" giving a better approximation of the Convex semidifinite problem.
    - If noise is not very large, global minimum can be found. Otherwise, a suboptimal solution can be found by projecting back to the feasible space.
The beauty of SE-Sync is that has profound geometric ituition. 
Not sure whether the Truncated Newton Method used in the paper would cause some subtle issues.
It is also not clear the "Riemannian optimization" by [Boumal](https://hal.archives-ouvertes.fr/hal-01213337/document) has some unkown issues.
2. Among the four solvers, Ceres is more of a general-purpose solver. It can solve nonlinear least square problems with box constraints, or general
unconstrained nonlinear optimization problem. Ceres is very good at Automatic Differentiation. In fact, g2o uses Ceres's AD code. 
3. GTSAM has its own "Expression" based Automatic Differentiation framework. The authors of GTSAM state that it is faster than that of Ceres.

# Outliners and large-residual problems
Optimization-based SLAM algorithms generally behave well when the resdual of the objective function is small. 
A few outliners can "poison" a small-residual optimization problem into a large-residual problem.
There are two related problems:
1. Before actually solving a SLAM problem, it is hard to tell beforehand whether it is a small-residual problem or not. 
But the effectiveness of many algorithms rely on the fact that residuals are small. 
2. Even if we are lucky and get the global minimum of the optimization problem, for example, through a brutal-force approach,
The result is usually not what we want. Because a few outliners can scew the resulted map. What we really need is actually a method
to filter out those outliners.
3. [Robust estimators](https://en.wikipedia.org/wiki/Robust_statistics) can partially solve the outliner problem, for example,
instead of solving the original nonlinear least square problem, we can solve a so-called Iteratively Reweighted Least Square problem.
The idea behind it is to use an objective function that is less-sensitive to outliners while converging to the same optimizer under
some conditions. Though it is possible that the algorithm is trapped into newly introduced local minimum because of reweighting.
4. Outliner detection method such as [RANSAC](https://en.wikipedia.org/wiki/Random_sample_consensus) is too expensive to be practical.

