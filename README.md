

Overview:

Pareto modelled the wealth of individuals in a stable economy using the distribution $y \sim x^{-v}$.
	  
The paper [Distributions of money in model markets of the economy](https://arxiv.org/abs/cond-mat/0205221) introduced a simple economic model (subsequently named the *yard-sale* model) which reproduced this distribution.
  
[This online article](*https://pudding.cool/2022/12/yard-sale/*) got me started, and
[this pdf](*https://b9370b48-a70c-49e9-88aa-0f98cc0a5df4.filesusr.com/ugd/cb3c27_bf9072f3634e43a0ac01921af71370bc.pdf*)
has more details.

In the original *yard sale* model, economic agents interact subject to the following rules:
1. A pair of agents is randomly selected to engage in a transaction. 
2. An agent with wealth $w^0$ enters a transaction with another agent with wealth $w^1$. 
3. A certain amount of wealth $\Delta w$ will change hands. The value $\Delta w$ is a constant $\alpha$ times the smaller of $w^0$ and $w^1$, and the direction of wealth transfer is determined by the flip of a fair coin.

4. the [Dyalog APL](https://www.dyalog.com/) simulation below, the algorithm is sped up by pairing off the entire population and then applying the algorithm above on each pair. This allows the computation to be parallelised.

```
simulate_round←{
⎕io ← 0
⍺ ← 0.5
wealth ← ⍵
size←⍴wealth
npairs←size÷2
pairs←(2,npairs)⍴size?size
stakes←⍺×⌊⌿wealth[pairs]
outcomes←¯1 1[?npairs⍴2]
changes←1 ¯1∘.×outcomes×stakes
wealth[pairs]←wealth[pairs]+changes
wealth
}
```

A subsequent paper [Oligarchy as a Phase Transition: The effect of wealth-attained advantage in a Fokker-Planck description of asset exchange](https://arxiv.org/abs/1511.00770) introduced refinements to the model and provided a detailed mathematical analysis of their behaviour.
  
The first refinement introduced redistribution of wealth. A tax proportional to each agent's wealth is levied, and the proceeds are distributed evenly to all the agents.
  
APL code to implement that is shown below.
  ```
  tax ← { ⍺ ← 0.1 ⋄ revenue ← ⍺ × ⍵ ⋄ wealth ← ⍵ - revenue ⋄ ⌈ wealth + revenue ÷ ⍴⍵ }
  ```

