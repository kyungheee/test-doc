---
title: LLM in Chemical Domain
weight: 1
math: true
---

## Paper

{{< cards >}}
  {{< card 
        url="https://aiml-k.github.io/publication/2024acl-cha-lee/" 
        title="Evaluating Extrapolation Ability of Large Language Model in Chemical Domain" 
        subtitle="Authors : Taehun Cha, Donghun Lee" >}}
{{< /cards >}}


## Main Question

{{% callout note %}}
**Does an LLM have superior extrapolation ability for unseen tasks in the chemical domain?**
{{% /callout %}}


## Approach 

{{% steps %}}

### Data Collection


We collect **917 Epoxy Resin Data points**  with lab experiments measuring [glass transition temperature](https://www.sciencedirect.com/topics/materials-science/glass-transition-temperature) ($T_g$), [tan delta peak](https://www.sciencedirect.com/topics/engineering/tan-delta-peak) ($\delta$), and [cross-link density](https://www.sciencedirect.com/topics/chemistry/crosslink-density) ($v_c$)



### Experimental Setup

To evaluate the **[extrapolation](https://en.wikipedia.org/wiki/Extrapolation) ability** of LLM([*gpt-4-turbo*](https://arxiv.org/abs/2303.08774)), we construct the following four regression tasks.

1. Linear Regression (LR)
2. Ridge Regression (RR)
3. Random Forest (RF)
4. XGBoost (XGB)

Our goal is to predict three properties of the test data given training data from a different chemical domain.

### Test (1) Additional Epoxy Resin $B_i$

**Train data**: Resin $A$, Curing agent, Catalyst   
**Test data**: Resin $A$, Curing agent, Catalyst, Resin $B_i$

  - Resin $B_1$: [CTBN(Carboyl-Terminated Butadiene Acrylonitrile) modified epoxy resin](https://onlinelibrary.wiley.com/doi/abs/10.1002/app.25412)
  - Resin $B_2$: MBS type [CSR(core shell rubber) modified epoxy resin](https://patents.google.com/patent/KR20160099609A/en)
  - Resin $B_3$: [Dimer acid](https://en.wikipedia.org/wiki/Dimer_acid) modified epoxy resin


### Test (2) Replaced Epoxy Resin $B_2$ Replacing the Original Resin $A$

**Train data**: Resin $A$, Curing agent, Catalyst   
*Test data*: Resin $B_2$, Curing agent, Catalyst

### Evaluate Extrapolation Ability on $T_g$, $\delta$, and $v_c$

You can check the results in the Result section below

{{% /steps %}}


## Result

<table style="text-align:center; margin:auto;">
  <thead>
    <tr>
      <th rowspan="2"></th>
      <th colspan="3">Resin $B₁$</th>
      <th colspan="3">Resin $B₂$</th>
      <th colspan="3">Resin $B₃$</th>
    </tr>
    <tr>
      <th>$T_g$</th>
      <th>$\delta$</th>
      <th>$v_c$</th>
      <th>$T_g$</th>
      <th>$\delta$</th>
      <th>$v_c$</th>
      <th>$T_g$</th>
      <th>$\delta$</th>
      <th>$v_c$</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><b>LR</b></td>
      <td>4.61</td><td>0.0667</td><td>0.000347</td>
      <td>4.31</td><td>0.0539</td><td><b>0.000225</b></td>
      <td>8.42</td><td>0.0536</td><td>0.000329</td>
    </tr>
    <tr>
      <td><b>RR</b></td>
      <td><b>4.58</b></td><td><b>0.0666</b></td><td>0.000349</td>
      <td><b>4.20</b></td><td><b>0.0537</b></td><td>0.000228</td>
      <td>8.42</td><td><b>0.0520</b></td><td>0.000336</td>
    </tr>
    <tr>
      <td><b>RF</b></td>
      <td>5.70</td><td>0.0730</td><td>0.000310</td>
      <td>4.99</td><td>0.0760</td><td>0.000311</td>
      <td>9.79</td><td>0.0572</td><td>0.000301</td>
    </tr>
    <tr>
      <td><b>XGB</b></td>
      <td>5.61</td><td>0.0718</td><td>0.000315</td>
      <td>4.60</td><td>0.0761</td><td>0.000237</td>
      <td>9.00</td><td>0.0559</td><td>0.000304</td>
    </tr>
    <tr>
      <td><b>Ours</b></td>
      <td>7.32</td><td>0.0859</td><td><b>0.000299</b></td>
      <td>5.62</td><td>0.0816</td><td>0.000288</td>
      <td><b>6.40</b></td><td>0.0778</td><td><b>0.000251</b></td>
    </tr>
  </tbody>
</table>

**Table 1: MAE results for Test (1), averaged over 5 trials.**   

Our LLM model outperformed the baseline regression model on $v_c$, but failed to do so on $\delta$. For $T_g$, the performance depends on $B_i$. Additionally, our model shows lower volatility (5.62 to 7.32) compared to the baseline (4.20 to 9.79).


**An example answer from LLM for adding resin $B_1$**

<blockquote style="border-left: 4px solid #ccc; padding: 10px 20px; margin: 20px 0; background-color: #f9f9f9; font-style: italic;">
  (...) CTBN is a rubbery polymer that is typically used to improve the toughness of epoxy resins.
  The incorporation of CTBN into an epoxy resin generally <strong>results in a decrease in</strong> $T_g$
  because the CTBN phase is softer and more flexible compared to the rigid epoxy network formed by DGEBA-based resins.
  <br><br>
  (...) The SMILES of Resin $B_1$ indicates the presence of butadiene and acrylonitrile groups,
  which contribute to the elastomeric properties of the resin. This further supports the expectation of a
  <strong>lower $T_g$</strong> due to increased flexibility and reduced crosslink density. (...)
</blockquote>


<table border="1" style="text-align: center; border-collapse: collapse;">
  <thead>
    <tr>
      <th></th>
      <th><i>T<sub>g</sub></i></th>
      <th><i>δ</i></th>
      <th><i>v<sub>c</sub></i></th>
    </tr>
  </thead>
  <tbody>
    <tr><td>LR</td><td>12.87</td><td>0.1606</td><td>0.001094</td></tr>
    <tr><td>RR</td><td>12.78</td><td>0.1573</td><td>0.001097</td></tr>
    <tr><td><b>RF</b></td><td><b>12.62</b></td><td><b>0.1107</b></td><td>0.000718</td></tr>
    <tr><td>XGB</td><td>13.35</td><td>0.1199</td><td>0.000779</td></tr>
    <tr><td><b>Ours</b></td><td>21.17</td><td>0.1116</td><td><b>0.000467</b></td></tr>
    <tr><td><b>Ours (1 shot)</td><td>12.14</td><td>0.1080</td><td>0.000389</b></td></tr>
  </tbody>
</table>

**Table 2: MAE results for Test (2), averaged over 5 trials.**   


## Why it matters?

{{% callout warning %}}
Here's some important information...
{{% /callout %}}