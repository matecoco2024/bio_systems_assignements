# Assignment 4: VirtualLeaf - Pathogen Infection Model

**Course**: KEN3170 - Biological Systems Modeling  
**Topic**: Plant-Pathogen Interaction Simulation  
**Platform**: VirtualLeaf

## Model Overview

This assignment focuses on modeling **pathogen infection dynamics** in plant tissues using VirtualLeaf. The simulation involves:

- **Pathogen (Red)**: Infectious agent that spreads through plant tissue
- **Plant Tissue (Green)**: Host cells that can be infected
- **Chemical Secretion**: Pathogen releases cell wall weakening agents
- **Cell Wall Dynamics**: Chemical reduces cell wall stability, facilitating infection spread

---

## Assignment Tasks

### 1. Initial Simulation & Observation
**Objective**: Run the baseline model and document the infection dynamics

**Instructions**:
- [ ] Open the VirtualLeaf pathogen infection model
- [ ] Run the simulation for **4 hours** (as-is, no modifications)
- [ ] Document observations with screenshots at:
    - [ ] **t = 0h** (Initial time point)
        
        <img src="image-1.png" alt="hour0" style="width: 400px;">
        
    - [ ] **t = 1h** 
        
        <img src="image-2.png" alt="hour1" style="width: 400px;">
        
    - [ ] **t = 2h**
        
        <img src="image-3.png" alt="hour2" style="width: 400px;">
        
    - [ ] **t = 3h**
        
        <img src="image-4.png" alt="hour3" style="width: 400px;">
        
    - [ ] **t = 4h** (Final time point)
        
        <img src="image-5.png" alt="hour4" style="width: 400px;">

**Guiding Questions**:
- What is happening during the simulation?
the auxin of the cells next to the pathogen is lowering, and the pathogen is incresing in size, meaning that that thing is reducing cell wall stiffness and expanding into compromised areas
- How does the pathogen spread?
it increases by size, almost adopting a circle shape, by secreting chemicals that weaken neighbouring cell walls 
- What changes do you observe in the plant tissue?
it goes brown instead of green, meaninig that auxin levels lower, and the wall structure gets altered since it's stiffness decreases
- How does the infection pattern evolve over time?
the pathogen just increases in size by sucking the auxin of the neighbouring cells and altering the wall structure, the infection then spreads towards more adjacent cells and the infection just grows.
---

### 2. Code Analysis: CellHouseKeeping Section
**Objective**: Understand the computational implementation of cellular processes

**Instructions**:
- [ ] Examine the VirtualLeaf code and focus on the **CellHouseKeeping** section

```cpp
void Assignment::CellHouseKeeping(CellBase *c) {
    // add cell behavioral rules here
    if(c->CellType()==2){
        c->EnlargeTargetArea(2);
    }

    // initial cell length setup
    double base_element_length = 25;
    c->LoopWallElements([base_element_length](auto wallElementInfo){
        if(std::isnan(wallElementInfo->getWallElement()->getBaseLength())){
        wallElementInfo->getWallElement()->setBaseLength(base_element_length);
        }
    });


    //cell wall weakening happens here
    double patho_chem_level = c->Chemical(0) / (0.5);
    if (patho_chem_level > 1.2) {
        patho_chem_level = 1.2;
    }
    double stiffness_inf = 2.5;
    if(patho_chem_level>0.1 && c->CellType()!=2){
        c->SetCellVeto(false);
        stiffness_inf = 2.5 - (patho_chem_level);
    c->LoopWallElements([stiffness_inf](auto wallElementInfo){
        wallElementInfo->getWallElement()->setStiffness(stiffness_inf);
    });
    }
    else{
        c->LoopWallElements([stiffness_inf](auto wallElementInfo){
        wallElementInfo->getWallElement()->setStiffness(stiffness_inf);
        });
        c->SetCellVeto(true);
    }
}
```


- [ ] Write a detailed description of what this code section does

1) settign a default cell length (25) for wall elements that haven't been initialized

2) first we calculate pathogen chemical level, patho_chem_level = c->Chemical(0) / (0.5) and we cap that level at 1.2

3) when the pathogen chemical levels surpasses a level (0.1), the wall stifness of the neightbouring cells decrease (2.5 - chemical_level), making them less stiff and altering their structure AND removes cell division restrictions (SetCellVeto(false))

4) for the unnafected cells, they remain at default stiffness level (2.5 )

---

### 3. Network Analysis & Completion
**Objective**: Compare code implementation with conceptual network diagram

**Instructions**:
- [ ] Examine the **cell-to-cell transport** code section
- [ ] Examine the **cell dynamics** code section  
- [ ] Compare the equations in the code to the provided network sketch
- [ ] Identify missing information in the sketch
- [ ] Complete the network diagram

**Deliverables**:
- [ ] Updated network sketch (complete diagram)
- [ ] Documentation of missing elements identified
- [ ] Comparison between code equations and network representation

**Reference Image**: 
![Network Sketch](image.png)

---

### 4. Parameter Sensitivity Analysis
**Objective**: Investigate the effect of diffusion coefficient on infection dynamics

**Instructions**:
- [ ] **Experiment A**: Decrease pathogen chemical diffusion coefficient by **factor of 10**
  - Run simulation for 4h
  - Document with screenshots (t = 0, 1, 2, 3, 4h)
  - Record observations
  
- [ ] **Experiment B**: Increase pathogen chemical diffusion coefficient by **factor of 10** 
  - Run simulation for 4h  
  - Document with screenshots (t = 0, 1, 2, 3, 4h)
  - Record observations

**Analysis**:
- [ ] Compare results to baseline simulation
- [ ] Describe changes in infection patterns
- [ ] Explain biological significance of diffusion coefficient changes

---

### 5. Plant Defense Mechanism Design
**Objective**: Design a computational model for plant immune response

**Scenario**: 
> The plant has evolved a defense mechanism that:
> - Detects the pathogen's chemical signal
> - Triggers strengthening of cell walls in uninfected plant cells
> - Provides resistance against pathogen invasion

**Instructions**:
- [ ] Design the implementation strategy (conceptual level)
- [ ] Write pseudocode for different model sections
- [ ] **Note**: No actual C++ code required - pseudocode is sufficient

**Deliverables**:
```markdown
## Plant Defense Mechanism Implementation

### Detection System
[Pseudocode for chemical detection]

### Signal Transduction  
[Pseudocode for defense signal propagation]

### Cell Wall Strengthening
[Pseudocode for wall reinforcement mechanism]

### Integration with Existing Model
[How to incorporate into current VirtualLeaf framework]
```

