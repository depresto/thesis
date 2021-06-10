## 共乘總資費計算

對於任何一筆訂單的共乘資費，目前的計算方式為

$carpool\ cost_c = \sum\limits_{l \in L} \frac{\sum\limits_{p \in P_c} x_p \delta_{pl} f_{cl}}{\sum\limits_{i \in R'} \sum\limits_{p \in P_{cd_i}} y_{p} \delta_{pl} w_{ci} q_i - \sum\limits_{i \in R'} \sum\limits_{p \in P_{co_i}} x_{p} \delta_{pl} w_{ci} q_i}$

步驟如下：

1. 計算該 link 上的共乘資費
$$
\frac{\text{該 link 的資費}}{\text{該 link 上的共乘人數}}
= \frac{f_{cl}}{\sum\limits_{i \in R'} \sum\limits_{p \in P_{cd_i}} y_{p} \delta_{pl} w_{ci} q_i - \sum\limits_{i \in R'} \sum\limits_{p \in P_{co_i}} x_{p} \delta_{pl} w_{ci} q_i}
$$
	而該 link 上的共乘人數，由於需要知道所有訂單中，哪些訂單有被共乘的駕駛所接受，並且要在該 link 上有在車上，因此採用以上做法
2. 加總所有 link 上的共乘資費

