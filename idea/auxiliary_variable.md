Notation      |   Description
--------------|------------------------------
$z_p$         | Binary variable, 1 if route $p \in P$ is chosen; 0 otherwise.
$\delta_{pl}$ | Indicator function which is 1 if link $l \in L$ on the route $p \in P$; 0 otherwise
$f_l$         | The fare for the link $l$, where $l \in L$
$\alpha_u$    | Accumulate fare from initial position to node $u$
$\beta_v$     | Accumulate fare from initial position to node $v$
$f(u, v)$     | Auxiliary function which is 1 if node $u$ is the previous node of the node $v$ on the route $r$

$\alpha_u = \sum\limits_{l \in L} \sum\limits_{p \in P_u} z_p \delta_{pl} f_l$

$\beta_v = \sum\limits_{l \in L} \sum\limits_{p \in P_v} z_p \delta_{pl} f_l$

$f(u, v) = \frac{max(\alpha_u - \beta_v, 0)}{\alpha_u - \beta_v} = \frac{\alpha_u - \beta_v + |\alpha_u - \beta_v|}{2}$
