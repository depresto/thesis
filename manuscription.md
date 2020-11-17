## 共乘問題

1. 可利用前述之model
2. Rider為car pool中之共享車輛(M to M)
3. 餐廳為共乘旅客之出發地點
4. 客戶所在地（送貨點）為乘客之目的地
5. 食物運送箱之客量為共乘車輛可搭載之人數
6. 對乘客之QoS指標可含
    1. Pick up time
    2. Travel time
    3. Arrival time
    4. 共乘人數
    5. 分攤費用
    6. 共乘路徑與最短路徑之差值等

經多給定點之最短路徑問題(S to M)
E.g. 機車接送學生 or 某員工開個pick up同仁一同上班，若為後者可進一步考慮公平性問題

1. 由A出發，目的地為Z
2. 途中需經$U_1, U_2, …, U_n$ (無需依序)之給定點
    
    $(U_i, U_i') \in L’$

3. Modeled as $G(N, L)$, 將$U_1 ~ U_n$利用node splitting技術產出artificial link集合$L’$
4. 可將n必經點排序，再找出依序之最短路徑，但複雜度為$n!$，過高 
5.  
    $min \sum\limits_{p \in P_z} \sum\limits_{l \in L \cup L'} x_p s_{pl} a_l \hspace{90pt} (LP)$ ($P_z$: 所有由A至Z路徑所成之集合)

    $\text{where cost of link } l; a_l=0 \text{ } \forall l \in L'$

    $s.t. \sum\limits_{p \in P_z} x_p  = 1 \hspace{124pt} (1)$

    $\hspace{15pt} x_p = 0 \text{ or } 1 \hspace{80pt} \forall p \in P_z \hspace{10pt} (2)$
    
    (to be relaxed)$\sum\limits_{p \in P_z} x_p s_{pl} = 1 \hspace{30pt} \forall l \in L' \hspace{10pt} (3)$

- 第(3)式保證最短路徑會經過 $U_1,U_2,...U_n$
- 經relaxed後，對應之arc weight (LR multipliers) 可為± or 0, 若為負則LR中做短路徑問題即不可用Dijkstra，而須改用 e.g. Bellman-Ford with max hop constraint (避免負迴圈)

將前述經特定多點之最短路徑問題修改為每一$U_i$有一需經過之對應$u_i$，同時所標路徑必須滿足先經$U_i$後經$u_i$

1. 有趣之問題為合乎所有$U_i$，均須前於$u_i$條件之permutations共有多少種？
2. math formulation: ($u_i$亦經node splitting並增加artificial links $\in L''$)
   
   {令$U_i$之集合為$V$, $u_i$之集合為$U$, $i \in \{1,2,...,n\}$}

   $min \sum\limits_{p \in P_z} \sum\limits_{l \in L \cup L' \cup L''} x_p s_{pl} a_l$

   $s.t. \sum\limits_{p \in P_z} x_p = 1 \hspace{30pt} \text{for } U_i \& u_i, i \in \{1,2,...,n\} \hspace{10pt} (1)$

   $\hspace{20pt} x_p = 0 \text{ or } 1 \hspace{30pt} \forall p \in P_z \hspace{80pt} (2)$

   $\hspace{20pt} \sum\limits_{p \in P_z} x_p s_{pl} = y_l \hspace{15pt} \forall l \in L \cup L' \cup L'' \hspace{44pt} (3)$ 
      
    link $l$ 若在最短路徑上，則 $y_l = 1$，否則為 0

    $\hspace{20pt} y_l = 1 \hspace{50pt} \forall l \in L' \cup L"" \hspace{55pt} (4)$

    $\hspace{20pt} \sum\limits_{p \in P_w} z_p s_{pl} \leq y_l \hspace{15pt} \forall w \in V \cup U = W \hspace{38pt} (5)$

    (保證計算到 $U_i$ 及 $u_i$ 所用之路徑 $"z_p"$ 重疊於A至Z之路徑 $"x_p"$)

    $\hspace{20pt} \sum\limits_{p \in P_w} z_p = 1 \hspace{30pt} \forall w \in W \hspace{78pt} (6)$

    $\hspace{20pt} z_p = 0 \text{ or } 1 \hspace{145pt} (7)$

    $\hspace{20pt} \sum\limits_{l \in L \cup L'} \sum\limits_{p \in Q_{U_i}} z_p s_{pl} a_l \leq \sum\limits_{l \in L \cup L'} \sum\limits_{p \in Q_{u_i}} z_p s_{pl} a_l \hspace{10pt} \forall i \in \{1,2,,...,n\} (8)$

    (保證$U_i$先$u_i$後)

    ($Q_{u_i}$為A至$U_i$所有路徑之集合
    
    $Q_{u_i},...,u_i,..., i \in \{1,2,3,...,n\} = R$)

於前頁之 formulation 中可加入各客戶 $u \in \{1,2,...,n\} = R$ 所要求之QoS，如此則需加入admission control機制