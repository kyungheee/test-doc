---
title: Exploring Hallucination Types
weight: 2
math: true
---

## Paper

{{< cards >}}
  {{< card 
        url="https://aiml-k.github.io/publication/2024kcc-bae-lee/" 
        title="Exploring Hallucination Types in Question-Answering Generation and Limitation of Text Evaluation Metrics" 
        subtitle="Authors : Suhyun Bae, Donghun Lee" >}}
{{< /cards >}}


## Main Question

{{% callout note %}}
**If we can't classify all hallucination types by [conventional methods](https://dl.acm.org/doi/10.1145/3571730), why don't we suggest a new hallucination type?**
{{% /callout %}}


## Approach 

{{% steps %}}

### Method

We classified hallucination types into **Five Types** through the following **procedure**.

{{< spoiler text="Click to view the procedure" >}}

<img src="/imgs/nlp/bae-procedure.png" alt="procedure">

{{< /spoiler >}}

- **Type 1 (Incomplete sentence)** : The model’s answer does not have a proper sentence format
- **Type 2 (Irrelevant answer)** : The model’s inference is not consistent with the content of the question and the reference answer
- **Type 3 (Meaning change)** : A critical keyword appears only in the model inference (not in the reference answer)
- **Type 4 (Information omission)** : A critical keyword from the reference answer is missing in the model inference
- **Type 5 (Cause-effect mismatch)** : There is a change in the causal relationship due to Cross during the keyword matching process


### Experiment

1. **Dataset** : `QA_data`   
  It consists of knowledge, question, right_answer, and hallucinated_answer. 
  
2. **Augmentation** : We augmented the dataset by converting short answers into full sentences using the `Vicuna` model

3. **Sample Selection** : From the 1,150 randomly selected samples, we retained the 81 that scored at least 0.7 on any of **BLEU-1, METEOR, or ROUGE-L.**

4. **Evaluation** : Based on the above procedure, we identified the hallucination type and computed the average evaluation scores for each type


{{% /steps %}}


## Result

<p align="center"><strong>Table 2: Average Evaluation Scores Table</strong></p>

<table border="1" style="text-align: center; border-collapse: collapse;">
  <thead>
    <tr>
      <th></th>
      <th>Type 3-short</th>
      <th>Type 3-long</th>
      <th>Type 4</th>
      <th>Type 5</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><b>BLEU-1</b></td>
      <td><strong>0.7421</strong></td>
      <td>0.2921</td>
      <td>0.3990</td>
      <td>0.3030</td>
    </tr>
    <tr>
      <td><b>METEOR</b></td>
      <td>0.0481</td>
      <td><strong>0.7109</strong></td>
      <td><strong>0.5866</strong></td>
      <td><strong>0.7857</strong></td>
    </tr>
    <tr>
      <td><b>ROUGE-L</b></td>
      <td>0.0563</td>
      <td><strong>0.7298</strong></td>
      <td><strong>0.6761</strong></td>
      <td><strong>0.6888</strong></td>
    </tr>
  </tbody>
</table>

<div align="center">
  <img src="/imgs/nlp/bae-score.png" alt="score" width="550">
</div>

<p align="center"><strong>Figure 2: Average Evaluation Scores Graph</strong></p>


- **_Types 1 and 2_** scored below the threshold on all three metrics and were excluded, showing the model can’t distinguish them.

- **BLEU-1** scored high only for **_Type 3-short_** cases, indicating that the model struggles to detect meaning changes in short sentences

- **METEOR and ROUGE-L** scored high on **_Type 3-long, Type 4, and Type 5_** cases, indicating the model reliably catches hallucinations with major semantic shifts 


## Why it matters?

{{% callout warning %}}

Our results show that no single metric reliably detects every hallucination type. This highlights **the need to refine and combine the three metrics or to develop a new one**
{{% /callout %}}