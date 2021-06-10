## 系統假設

對於本系統，有以下假設：

1. 系統計算時間不計，意即駕駛位置在系統計算後會維持不變
2. 共乘費用會均勻分攤給車上的乘客，如在一段 link 上有 $n$ 個乘客在車上，費用為 $f_l$，則每位乘客在該段 link 的費用為 $\frac{f_l}{n}$
3. 系統中的乘客在滿足訂單的條件下，均會允許系統分派的共乘
4. 系統中的駕駛，會接受所有系統分派的訂單
5. 乘車時間為所有經過 links 的加總
6. 在任何決策週期中，駕駛的數量充足，一定能接受所有該決策週期分派的訂單

接下來，將介紹系統中的給定參數以及決策變數

## 給定參數

- $C$：系統中駕駛的集合
- $R$：系統中所有訂單的集合
- $R'$：系統中當下決策週期未被分派的訂單的集合
- $O$：系統中所有訂單的起始點的集合
- $D$：系統中所有訂單的到達點的集合
- $N$：系統路網中所有 nodes 的集合，包含駕駛當下位置、所有訂單的起點與終點，$N \in C \cup O \cup D$
- $L$：系統路網中所有 links 的集合，路網中任意兩 nodes 即為一 link
- $o_i$：第 $i$ 筆訂單的起始點， $i \in R$, $o_i \in O$
- $d_i$：第 $i$ 筆訂單的到達點， $i \in R$, $d_i \in D$
- $q_i$：第 $i$ 筆訂單的乘客人數， $i \in R$
- $q^{male}_i$：第 $i$ 筆訂單的男性乘客人數， $i \in R$
- $q^{female}_i$：第 $i$ 筆訂單的女性乘客人數， $i \in R$
- $e_i$：第 $i$ 筆訂單相較於專程接送，所要求的最大共乘旅程時間增加比例限制， $i \in R$
- $g_i$：第 $i$ 筆訂單相較於專程接送，所要求的最小共乘車資折扣比例限制， $i \in R$
- $m_i$：第 $i$ 筆訂單的共乘旅程時間限制， $i \in R$
- $n^{male}_i$：第 $i$ 筆訂單的男性人數限制，若該車限制只能有女性，則 $n^{male}_i = 0$， $i \in R$
- $n^{female}_i$：第 $i$ 筆訂單的女性人數限制，若該車限制只能有男性，則 $n^{female}_i = 0$， $i \in R$
- $h_i$：第 $i$ 筆訂單所要求的最大共乘人數限制， $i \in R$
- $t_i$：在當下決策週期中，距離第 $i$ 筆訂單開始等待乘車時間的所剩時間， $i \in R$
- $u_i$：在當下決策週期中，距離第 $i$ 筆訂單結束等待乘車時間的所剩時間， $i \in R$
- $T$：系統中等待乘車時間的額外緩衝時間
- $Q_c$：駕駛 $c$ 車上的最大允許乘車人數， $c \in C$
- $L_{O}$：系統中所有訂單的起始點由 Node splitting 技術所產生的 artificial links 的集合
- $L_{D}$：系統中所有訂單的到達點由 Node splitting 技術所產生的 artificial links 的集合
- $P_{co_i}$：由駕駛 $c$ 目前位置到第 $i$ 筆訂單的起始位置 $o_i$ 所有可能 paths 的集合，$c \in C, i \in R, o_i \in O$
- $P_{cd_i}$：由駕駛 $c$ 目前位置到第 $i$ 筆訂單的到達位置 $d_i$ 所有可能 paths 的集合，$c \in C, i \in R, d_i \in D$
- $P_c$：由駕駛 $c$ 目前位置至路網中任一 node (包含駕駛目前位置) 所連結的虛擬節點，該虛擬節點不屬於目前路網，且與任一路網中的 node 所形成的 link 長度為 0，$c \in C$
- $t_{cl}$：駕駛 $c$ 在 link $l$ 上的行駛所需花費的時間，$c \in C, l \in L$
- $t_{ci}$：駕駛 $c$ 專程接送第 $i$ 筆訂單從起始節點至到達節點所需花費的時間，$c \in C, i \in R$
- $f_{cl}$：駕駛 $c$ 在 link $l$ 上的行駛所要花費的車資，$c \in C, l \in L$
- $f_{ci}$：駕駛 $c$ 專程接送第 $i$ 筆訂單從起始節點至到達節點所需花費的車資，$c \in C, i \in R$
- $\delta_{pl}$：為一 Indicator function， 若 link $l$ 在路徑 $p$ 上為 1，否則為 0，$l \in L, p \in P_c \cup P_{co_i} \cup P_{cd_i}$
- $\alpha$：上個決策週期駕駛接單數的 fairness index，若系統中駕駛數量有增減，則該 fairness index 重設為 0
- $r_c$：駕駛 $c$ 自 fairness index 重設以來所累積的總車資收入 $c \in C$

## 決策變數

- $x_{p}$：為一個二元變數, 若 $p$ 路徑有被駕駛 $c$ 選擇時為 1，否則為 0，$p \in P_{co_i}, c \in C, i \in R, o_i \in O$ 
- $y_{p}$：為一個二元變數, 若 $p$ 路徑有被駕駛 $c$ 選擇時為 1，否則為 0，$p \in P_{cd_i}, c \in C, i \in R, d_i \in D$
- $z_{p}$：為一個二元變數, 若 $p$ 路徑有被駕駛 $c$ 選擇時為 1，否則為 0，$p \in P_{c}, c \in C$
- $w_{ci}$：為一個二元變數, 若駕駛 $c$ 接受第 $i$ 筆訂單時為 1，否則為 0，$c \in C, i \in R$
- $s_{cl}$：為一個二元變數, 若駕駛 $c$ 在共乘路徑上有經過 $l$ link時為 1，否則為 0，$c \in C, l \in L \cup L_O \cup L_D$
- $\beta$：當前決策週期駕駛接單數的 fairness index

## 目標函數

本系統的目標是最小化所有訂單中，因共乘所造成的最大車程增加量，意即相比於專程載運，共乘所額外增加的繞路成本

$$
\begin{align}
&
\underset{c \in C}{\operatorname{min}}
\underset{i \in R'}{\operatorname{max}}
\frac{
	w_{ci} t_{ci}
}
{
	w_{ci} t_{ci} -
	carpool\ cost_{ci}
}
\tag{IP1} \\
&
\text{where}\ 
carpool\ cost_{ci}
= \sum\limits_{p \in P_{cd_i}} \sum\limits_{l \in L} y_{p} \delta_{pl} t_{cl} -
\sum\limits_{p \in P_{co_i}} \sum\limits_{l \in L} x_{p} \delta_{pl} t_{cl}
\end{align}
$$

## 限制式

- 限制式 $\eqref{waiting_time_start}$ 確保接送開始時間符合訂單要求
$$
\begin{align}
& \sum\limits_{p \in P_{co_i}} \sum\limits_{l \in L} x_{p} \delta_{pl} t_{cl} 
\geq t_i - T
&&
\forall c \in C, i \in R'
\label{waiting_time_start} \tag{3.1}\\
\end{align}
$$
- 限制式 $\eqref{waiting_time_end}$ 確保接送截止時間符合訂單要求
$$
\begin{align}
& \sum\limits_{p \in P_{co_i}} \sum\limits_{l \in L} x_{p} \delta_{pl} t_{cl} 
\leq u_i + T
&&
\forall c \in C, i \in R' 
\label{waiting_time_end}\tag{3.2}\\
\end{align}
$$
- 限制式 $\eqref{path_limit}$ 確保系統中任何駕駛會選擇一條路線（到訂單起點與終點，以及共乘路線），共乘路線是由駕駛當下位置至一虛擬終點，因此不管是否有在該決策週期接到任何單，都需要有一條共乘路線
$$
\begin{align}
  & \sum\limits_{p \in P_{c}} z_{p} = 1                                  && \forall c \in C \label{path_limit}\tag{3.3} \\
\end{align}
$$
- 限制式 $\eqref{path_limit_origin}, \eqref{path_limit_destination}$ 確保駕駛接到訂單後，會規劃出到該訂單起訖點的路線
$$
\begin{align}
& \sum\limits_{p \in P_{co_i}} x_{p} = w_{ci}                               && \forall c \in C, i \in R' \label{path_limit_origin}\tag{3.4} \\
  & \sum\limits_{p \in P_{cd_i}} y_{p} = w_{ci}                               && \forall c \in C, i \in R' \label{path_limit_destination}\tag{3.5} \\
\end{align}
$$
- 限制式 $\eqref{riding_request_limit}$ 確保任何一筆訂單最多分配給一位駕駛
$$
\begin{align}
& \sum\limits_{c \in C} w_{ci} \leq 1 && \forall i \in R' \label{riding_request_limit}\tag{3.6}
\end{align}
$$
- 限制式 $\eqref{pick_up_drop_off_limit}$ 確保任何一筆訂單會先接後送（以花費時間來判斷，若到起點所需的時間少於到終點的時間，則為先接後送）
$$
\begin{align}
& \sum\limits_{p \in P_{co_i}} \sum\limits_{l \in L} x_{p} \delta_{pl} t_{cl} \leq \sum\limits_{p \in P_{cd_i}} \sum\limits_{l \in L} y_{p} \delta_{pl} t_{cl} && \forall c \in C, i \in R' \label{pick_up_drop_off_limit}\tag{3.7} \\
\end{align}
$$
- 限制式 $\eqref{artificial_link_limit}$ 確保有接到訂單駕駛一定會經過該訂單起訖點的 artificial links
$$
\begin{align}
& s_{cl} \geq w_{ci}             && \forall l \in L_O \cup L_D, i \in R', c \in C \label{artificial_link_limit}\tag{3.8} \\
\end{align}
$$
- 限制式 $\eqref{overlap_limit_route}, \eqref{overlap_limit_origin}, \eqref{overlap_limit_destination}$ 確保司機從當前位置到司機接受訂單的起迄點所形成的路徑與共乘路徑重疊
$$
\begin{align}
& \sum\limits_{p \in P_{c}} z_{p} \delta_{pl} \geq s_{cl} && \forall l \in L \cup L_O \cup L_D, c \in C \label{overlap_limit_route}\tag{3.9} \\
& \sum\limits_{p \in P_{co_i}} x_{p} \delta_{pl} \leq s_{cl} && \forall l \in L \cup L_O \cup L_D, i \in R', c \in C \label{overlap_limit_origin}\tag{3.10} \\
& \sum\limits_{p \in P_{cd_i}} y_{p} \delta_{pl} \leq s_{cl} && \forall l \in L \cup L_O \cup L_D, i \in R', c \in C \label{overlap_limit_destination}\tag{3.11} \\
\end{align}
$$
- 限制式 $\eqref{capacity_limit}$ 確保共乘中乘客人數不會超出車子載客量（在任何共乘經過的 link 中，車上的人數都不能超過限制）
$$
\begin{align}
& \sum\limits_{i \in R'} \sum\limits_{p \in P_{cd_i}} y_{p} \delta_{pl} q_i - \sum\limits_{i \in R'} \sum\limits_{p \in P_{co_i}} x_{p} \delta_{pl} q_i \leq Q_c && \forall l \in L, c \in C \label{capacity_limit}\tag{3.12} \\
\end{align}
$$
- 限制式 $\eqref{capacity_male_limit}, \eqref{capacity_female_limit}$ 確保共乘中乘客的性別人數不會超出訂單的要求
$$
\begin{align}
& \sum\limits_{i \in R'} \sum\limits_{p \in P_{cd_i}} y_{p} \delta_{pl} q^{male}_i - 
\sum\limits_{i \in R'} \sum\limits_{p \in P_{co_i}} x_{p} \delta_{pl} q^{male}_i 
\leq n^{male}_{i} 
&&
\forall l \in L, i \in R'
\label{capacity_male_limit}\tag{3.13} \\
& \sum\limits_{i \in R'} \sum\limits_{p \in P_{cd_i}} y_{p} \delta_{pl} q^{female}_i -
\sum\limits_{i \in R'} \sum\limits_{p \in P_{co_i}} x_{p} \delta_{pl} q^{female}_i 
\leq n^{female}_{i}
&&
\forall l \in L, i \in R'
\label{capacity_female_limit}\tag{3.14} \\
\end{align}
$$
- 限制式 $\eqref{carpooling_time_limit}$ 確保共乘後的旅程時間不會超過訂單的要求
$$
\begin{align}
  & \sum\limits_{p \in P_{cd_i}} \sum\limits_{l \in L} y_{p} \delta_{pl} t_{cl} - \sum\limits_{p \in P_{co_i}} \sum\limits_{l \in L} x_{p} \delta_{pl} t_{cl} \leq m_i && \forall c \in C, i \in R' \label{carpooling_time_limit}\tag{3.15} \\
\end{align}
$$
- 限制式 $\eqref{detour_ratio_limit}$ 確保共乘後的繞路時程比例不會超過訂單的要求
$$
\begin{align}
& \frac{
	\sum\limits_{p \in P_{cd_i}} \sum\limits_{l \in L} y_{p} \delta_{pl} t_{cl} - 
	\sum\limits_{p \in P_{co_i}} \sum\limits_{l \in L} x_{p} \delta_{pl} t_{cl}
}
{
	t_{ci}
}
\leq e_i 
&&
\forall c \in C, i \in R'
\label{detour_ratio_limit}\tag{3.16} \\
\end{align}
$$
- 限制式 $\eqref{discount_ratio_limit}$ 確保共乘後的節費比例符合訂單要求 ($\epsilon$ 為一極小常數)
$$
\begin{align}
& \frac{
	\sum\limits_{p \in P_{cd_i}} \sum\limits_{l \in L} y_{p} \delta_{pl} f_{cl} - 
	\sum\limits_{p \in P_{co_i}} \sum\limits_{l \in L} x_{p} \delta_{pl} f_{cl}
} 
{
	f_{ci} 
	(
		\sum\limits_{i \in R'} \sum\limits_{p \in P_{cd_i}} y_{p} \delta_{pl} q_i - 
		\sum\limits_{i \in R'} \sum\limits_{p \in P_{co_i}} x_{p} \delta_{pl} q_i +
		\epsilon
	)
} 
\geq g_i
&& 
\forall c \in C, i \in R'
\label{discount_ratio_limit}\tag{3.17} \\
\end{align}
$$
- 限制式 $\eqref{custom_capacity_limit}$ 確保共乘中乘客人數不會超出訂單要求的共乘人數（在任何共乘經過的 link 中，車上的人數都不能超過限制）
$$
\begin{align}
  & \sum\limits_{i \in R'} \sum\limits_{p \in P_{cd_i}} y_{p} \delta_{pl} q_i - \sum\limits_{i \in R'} \sum\limits_{p \in P_{co_i}} x_{p} \delta_{pl} q_i \leq h_i && \forall l \in L, c \in C \label{custom_capacity_limit}\tag{3.17} \\
\end{align}
$$ 
- 限制式 $\eqref{driver_order_quantity_fairness}$ 為駕駛接單量的 fairness index 的計算方式
$$
\begin{align}
& \frac{(\sum\limits_{c \in C} \sum\limits_{i \in R'} w_{ci})^2}{|C| \sum\limits_{c \in C} (\sum\limits_{i \in R'} w_{ci})^2} = \beta         \label{driver_order_quantity_fairness}\tag{3.18} \\
\end{align}
$$
- 限制式 $\eqref{fairness_order_comparison}$ 確保本決策週期 fairness index 不會少於上個決策週期
$$
\begin{align}
& \beta \geq \alpha                                       \label{fairness_order_comparison}\tag{3.19} \\
\end{align}
$$
- 限制式 $\eqref{bound_x}, \eqref{bound_y}, \eqref{bound_z}, \eqref{bound_w}, \eqref{bound_s}$ 為決策變數 $x_p$, $y_p$, $z_p$, $w_{ci}$, $s_{cl}$ 的上下界
$$
\begin{align}
& x_p \in \{0, 1\} && \forall p \in P_{co_i}, c \in C, i \in R' \label{bound_x}\tag{3.20} \\
& y_p \in \{0, 1\} && \forall p \in P_{cd_i}, c \in C, i \in R' \label{bound_y}\tag{3.21} \\
& z_p \in \{0, 1\} && \forall p \in P_c, c \in C \label{bound_z}\tag{3.22} \\
& w_{ci} \in \{0, 1\} && \forall c \in C, i \in R' \label{bound_w}\tag{3.23} \\
& s_{cl} \in \{0, 1\} && \forall l \in L \cup L_O \cup L_D, c \in C \label{bound_s}\tag{3.24} \\
\end{align}
$$
- 限制式 $\eqref{bound_fairness_index_beta}$ 為決策變數 $\beta$ 的上下界
$$
\begin{align}
& \frac{1}{|C|} \leq \beta \leq 1                      \label{bound_fairness_index_beta}\tag{3.25} \\
\end{align}
$$