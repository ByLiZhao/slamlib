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

# Comments
1. Special attention should be paid to the SE-Sync paper. The analysis of the paper reveals some very
fascinating facts about StherLAM problems:
    - The graph optimization problem has an equivalent representation of Rotation Only Optimization.
    - The Rotation Only Optimization problem can be relaxed to a Convex Semidefinite problem if replacing the SO(n) group by the  O(n) group.
    - The Convec semidefinite problem can be solved by "go up" the Remanian staircase, with upper "stair" giving a better approximation of the Convex semidifinite problem.
    - If noise is not very large, global minimum can be found. Otherwise, a suboptimal solution can be found by projecting back to the feasible space.
The beauty of SE-Sync is that has profound geometric ituition. 
Not sure whether the Truncated Newton Method used in the paper would cause some subtle issues.

