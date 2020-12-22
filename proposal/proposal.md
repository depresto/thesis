# 基於總體成本與駕駛與乘客間公平性之最佳化共乘演算法

An Optimization Based Carpool Algorithm in Consideration of the Total Cost and Fairness among the Driver and All Passengers

## Abstract

共享經濟是隨著網路以及行動裝置普及下逐漸興起的熱潮，傳統的計程車與共乘服務在共享經濟的風潮下產生出新的商業模式，不同於以往使用者必須到目的地才能知道價錢，或是只能針對特定路線如通勤、通學等才容易有共乘機會；透過線上叫車服務，使用者除了可以事先知道價錢，還可以透過共乘配對服務，自動配對有相近路線的乘客，共同分攤車資，提供使用者更便宜經濟的選擇。

目前的線上共乘配對服務，為共乘平台針對目前行徑中或是配對中的乘客透過運算，找尋最適合的路線與乘客。自動配對的共乘服務中，往往需要多繞路以同時滿足不同乘客間的載運需求，如何讓使用者之間繞路多寡符合公平性，便是重要的挑戰。本研究考慮共享經濟中共乘服務的多乘客路線規劃問題，將乘客可接受的抵達時間、繞多少路的可接受程度，車輛人數限制，以及載客的優先順序納入考量，以最小最大化各乘客所多繞的路徑為目標式，透過拉格朗日鬆弛法以取得最佳解。

關鍵字：共乘、共享經濟、車輛路徑問題、動態車輛路線、拉格朗日鬆弛法、最佳化演算法

## Introduction

### 研究背景

近年來，隨著網際網路的普及以及幾乎人手一支的行動裝置帶動之下，資訊分享無遠弗屆，開始有人將閒置資源放到網路上與人們共享與交換，然而與陌生人共享意味者風險，有人嗅到了趨勢，便設計了可以透過評比知道使用者好壞的共享平台，大大降低了網路共享行為的風險[1]，此後這樣的共享平台逐漸推廣到生活的各個層面，蔚為風潮，從食物外送、住宿、乘車都可以見到他的身影，而人們稱呼這樣的新模式為「共享經濟」。

如何定義共享經濟目前並沒有統一的說法，不過一般來說，共享經濟中的活動主要分為四大類型：商品的再循環、增加耐久財的使用率、服務交換與經營性資產的共享[2]。而這些活動基本上圍繞著「閒置資產」的重新分配，不論是透過資源的交換，或從中賺取報酬，只要透過共享經濟平台的媒合，低廉的交易成本使得閒置資產擁有者將資產發揮更大的價值，而需求者能以較低廉的成本運用該資產，如此一來便能增進整體社會資產的使用率，甚至可以「以租代買」，改變以往必須擁有資產才能使用的思維。

共乘是一個行之有年的運輸解決方案，在學術中，也有如Dial-a-Ride Problem等文獻，在探討如何在已確定的固定乘客情境如通勤等的路徑規劃問題。在共享經濟的推波助瀾下，再加上行動裝置與GPS定位的普及，使得車輛在行徑中動態配對乘客成為可能，隨之而來的，便有如Dynamic Ridesharing Problem的相對研究議題產生，探討如何在行徑間。

### 研究動機

### 研究目的

## Literature review

## Problem formulation

The object of this research is to minimize the maximum percentage of cost saving when a passenger chooses to carpool, with consideration of the number of drivers, car capacity and routing limit constraints.

Take Figure 3-1 as an example scenario; Passenger 1 and Passenger 2 are requesting a trip. When Passenger 1 chooses to take a ride by himself/herself, called "exclusive" riding shown as Figure 3-2, it would cost $5. We can describe the scenario as a shortest path problem from $S$ to $D_1$, and $P_1$ is a must-pass node. We use Steiner tree to solve this kind of shortest path problem with must-pass nodes. Passenger 2's "exclusive" riding would also cost $5 in Figure 3-3. In this case, it would be a shortest path problem from $S$ to $D_2$ with $P_2$ as a must-pass node. When the passengers choose to take a carpool, which is called "sharing" riding, would have two best routings for the trip. We can describe the scenario as a shortest problem from $S$ to $D_1$ with $P_1$, $P_2$ and $D_2$ as must-pass nodes or $S$ to $D_2$. In Figure 3-4 is a case, which would cost $8 for all the passengers, in Figure 3-5 would cost the same and both of the cost they could share is $5 ( $\overline{P_2D_1}$ and $\overline{P_2D_2}$ respectively); however, in Figure 3-4 would cost $1 + \frac{1}{2} \times 5 = \$3.5$ for Passenger 1 and $\frac{1}{2} \times 5 + 1 = \$3.5$ for Passenger 2, in Figure 3-5 would cost $1 + \frac{1}{2} \times 5 + 1 = \$4.5$ for Passenger 1 and $\frac{1}{2} \times 5 = \$2.5$ for Passenger 2.

### Mathematical Model

#### Given parameters

$R$: Set of passengers $\{1, 2, 3, …, n\}$ in the system

$r_i$: Cost for passenger $i$ if the trip has no ride-sharing, $i \in R$

$V_i$: Set of nodes to pick up passenger $i$, $i \in R$

$\delta_{pl}$: Indicator function that subpath on path p

#### Decision variables

$x_p$: Binary variable that indicates where route $p$ is chosen, $p \in P_z$. 1 if $p$ is chosen; 0 otherwise.

$y_l$: Binary variable that indicates whether link $l$ is on the shortest path. 1 if $l$ is on the shortest path; 0 otherwise.