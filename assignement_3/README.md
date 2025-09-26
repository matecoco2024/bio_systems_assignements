# Multi Scale Modelling Of Biological Systems 
# Assignment 3 Boolean Networks

This project expands on the Boolean Networks practical by conducting an independent analysis of 
cancer-causing mutations using Boolean network modeling. Using the completed BooleanModeling.ipynb notebook
as a foundation, four distinct mutated versions of a cell regulatory network was created by modifying 
Boolean rules to simulate specific genetic alterations. 

These mutations included:

Mutation A — p53 knockout (loss of tumor suppressor activity),

Mutation B — MYC amplification (oncogene overexpression), 

Mutation C — MDM2 overexpression (disruption of the p53 pathway), and 

Mutation D — CDK2 knockout. 

For each mutated network, along with the normal network, scenario analyses were conducted based on the 
original practical, and attractor analysis was performed to identify stable states. This quantified
the impact of each mutation by determining the percentage of states that lead to cancer-like conditions, thereby 
providing insights into the regulatory dynamics of cancer progression.


Based on the practical, and attractor analysis;

## 1. Which mutation is most dangerous and why?
In this boolean model Mutations D is the most dangerous whereas  A, B, and C are the least dangerous.

All three of the mutations A, B, and C result in 256 / 256 initial states converging into cancer-like attractors.
This means they consistently drive the system into uncontrolled proliferation with Growth=1 and Death=0.
Although this is clearly dangerous from an oncogenic perspective, the cells remain viable and dividing.

In contrast, Mutation D (CDK2 knockout) produces no cancer-like attractors at all. Instead, it drives nearly half of the system (120/256 states, 46.9%) into apoptosis, while the rest end up in quiescent or unresolved non-proliferative states. This indicates a large-scale loss of cells, which at the tissue or organismal level can be highly damaging, since excessive apoptosis or growth arrest can compromise organ function more directly than the persistence of abnormal, but still proliferating, cells.

## 2. Explain the role of feedback loops (e.g., MYC → MDM2 → p53)

The core feedback loop in the wild-type network is MYC → MDM2 ⊣ p53 → p21 ⊣ MYC, which overall forms a positive
feedback loop. To be more specific, MYC activates MDM2 (MDM2 = MYC), MDM2 inhibits p53 (p53 = DNA_damage ∧ ¬MDM2), 
p53 activates p21 (p21 = p53), and p21 inhibits MYC (MYC = ¬p53 ∧ ¬p21).

Multiplying the signs of these interactions results in a positive loop, producing bistability with two 
self-reinforcing states: a 'pro-growth' state (MYC=1, MDM2=1, p53=0, p21=0, CDK2=1, Growth=1) and a 'protective' state 
(DNA damage high, p53=1 → p21=1 → MYC=0 → MDM2=0 → p53 remains on). Within this loop, p53 functions as a 
damage-sensitive gate (p53 = DNA_damage ∧ ¬MDM2), such that with damage and low MDM2, p53 activates p21, 
inhibits MYC/CDK2, and promotes Death (p53 ∧ DNA_damage ∧ ¬Growth), whereas high MDM2 (often driven by MYC) 
suppresses p53, allowing continued growth despite damage. Downstream, MYC and CDK2 jointly promote Growth 
(Growth = CDK2 ∧ MYC ∧ ¬p53), and Growth in turn inhibits Death, making the pro-growth attractor self-protecting.

Each mutation alters these loops; Mutation A (p53 knockout) removes the protective branch, allowing MYC and 
CDK2 unchecked activation and collapsing the system into Growth=1. Mutation B (MYC amplification) enforces MDM2=1, 
suppressing p53 even under damage, keeping p21=0 and CDK2=1, thereby locking Growth=1. Mutation C (MDM2 overexpression)
directly blocks p53, sustaining MYC and promoting Growth. And Mutation D (CDK2 knockout) disrupts the growth execution 
arm, so even if MYC is high and p53 low, Growth=0, allowing Death if p53 is activated under damage.

## 3. What are the limitations of this Boolean network model? Discuss 3 specific limitations.
While boolean networks are advantageous, as it is with many models there were limitations. 

1. Binary on/off logic simplifies complex biological processes by forcing all states into 0 or 1 -which 
obscures important details such as graded MYC expression, threshold-dependent p53 activation, or pulsatile p53 
dynamics observed after DNA damage. This means biologically distinct conditions (eg. “2 x MYC” vs “10 x MYC”) 
are treated identically, potentially making different mutations (A, B, C) appear equally severe. 

2. the Boolean update scheme ignores real kinetic differences between processes such as transcription and 
phosphorylation, as well as feedback delays and asynchronous reactions. In Boolean form, the size of basins of 
attraction and even which attractor the system reaches can depend heavily on whether updates are synchronous or 
asynchronous, their order, and the chosen time horizon. This means results such as “100% cancer-like” may reflect modeling
assumptions rather than biology.

3. The sparse topology of the network and hard-wired mutation rules oversimplify biology by omitting numerous regulators
(eg. growth factors), environmental signals, and cell-type context. Mutations are treated as absolute “on” or “off” 
states rather than partial or context-dependent changes, limiting the model’s ability to explain cases where MYC 
overexpression does not induce proliferation or where p53 loss alone is insufficient without cooperating lesions.