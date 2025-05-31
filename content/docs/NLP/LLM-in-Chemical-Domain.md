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
**Does LLM have superior extraplation ability for unseen tasks in the chemical domain?**
{{% /callout %}}


## Approach 

{{% steps %}}

### Data Collection


We collect 917 **Epoxy Resin Data points**  with lab experiments measuring [glass transition temperature](https://www.sciencedirect.com/topics/materials-science/glass-transition-temperature) ($T_g$), [tan delta peak](https://www.sciencedirect.com/topics/engineering/tan-delta-peak) ($\delta$), and [cross-link density](https://www.sciencedirect.com/topics/chemistry/crosslink-density) ($v_c$)



### Experimental Setup

To evaluate the extrapolation ability of LLM([*gpt-4-turbo*](https://arxiv.org/abs/2303.08774)), we construct the following four regression tasks.

1. Linear Regression (LR)
2. Ridge Regression (RR)
3. Random Forest (RF)
4. XGBoost (XGB)

Our goal is to predict three properties of the test data given training data from a different chemical domain.

### Test (1) Additional Epoxy Resin $B_i$

Train data: Resin $A$, Curing agent, Catalyst   
Test data: Resin $A$, Curing agent, Catalyst, Resin $B_i$

  - Resin $B_1$: CTBN(Carboyl-Terminated Butadiene Acrylonitrile) modified epoxy resin
  - Resin $B_2$: MBS type core shell rubber (CSR) modified epoxy resin
  - Resin $B_3$: Dimer acid modified epoxy resin



### Test (2) Replaced Epoxy Resin $B_2$ Replacing the Original Resin $A$

Train data: Resin $A$, Curing agent, Catalyst   
Test data: Resin $B_2$, Curing agent, Catalyst

### Evaluate Extrapolation Ability on $v_c$, $\delta$, and $T_g$

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




## Why it matters?

{{% callout warning %}}
Here's some important information...
{{% /callout %}}