# **About EcoTreat**

EcoTreat Vietnam Trading Co., Ltd is the primary supplier of tires to the majority of Vietnam's taxi cabs, servicing thousands of cabs across Ho Chi Minh City(HCM), Binh Duong Province(BD), Dong Nai Province (DN). Beside local brands like DRC, Casumia; EcoTreat offers a diverse range of tires sourced from renowned global brands, including Michelin, Goodyear, Bridgestone, Continental, Cooper, and more. Established relationships with local taxi collectives have solidified EcoTread's position as the preferred tire supplier in the region.
## Key Skills
- Python programming
- Excel Solver
- Supply Chain Network Desgin Concept
- Mixed-integer linear programming (MILP)
- Problem formulation and modeling
- Scenario analysis]
## Note
- The company name has been modified by the author.
- All data presented was created by the author. 
- Monetary values are expressed in US dollars (USD) for brevity.
- It is highly recommended to open any attached files simultaneously with   README.md file to fully understand the content.

# **EcoTreat’s New Distribution Center Recommendation**
## Overview

Due to the escalating impacts of climate change, and the concurrent rise in income, there's been a notable shift towards public transportation, particularly four-wheel vehicles. Alongside traditional taxis, numerous technology-driven ride-hailing service providers like Grab, Bee, Gojek, and the recent addition of XanhSM, which integrates technology with their own vehicle fleet, have emerged. 

This trend has prompted the establishment of more auto parts companies. Last year , EcoTreat merged with KUMOHA TIRE (VIETNAM) CO., LTD - an auto parts manufacturer. EcoTreat intends to launch its own tire brand made in Vietnam and is considering expanding its market to neighboring provinces in the southern region. However, with their current supply chain network, after 3 m onths operation, the distribution center in Ho Chi Minh City will be unable to handle the increased scale. Therefore, they must establish additional distribution centers, also redesign their supply chain network. 

***Requirement***
A cross-functional team was formed to spearhead the implementation of this project. As a Supply Chain Analyst in this team, my responsibilities include thoroughly **analyzing relevant costs** and **designing a network model** for the company, specifically, I will **assist the Teamleader in deciding where to open the new DC** also **estimate the associated costs.**
## What should we do ?

This problem involves Supply Chain Network Design decisions, which consider an underlying network of nodes (facilities) and arcs (transport flows between facilities). In this case, it is a Transshipment Problem, where the nodes are the plants, Distribution Centers, and retailers, and the arcs represent the flow of goods between them.

If we choose a particular location for DC from among multiple options, it means this location satisfies our objectives better than the others within our constraints. Therefore, the crucial task is to determine our objective and constraints. The optimal location is one that minimizes the total network cost while meeting specific constraints such as fulfilling customer demand, adhering to plant capacity, and maintaining service level requirements,etc.

The relationship between our objective and constraints can be formulated as a mixed-integer linear program and solved using various tools. This time, we'll build the model in both Python and Excel.

We can use either tool to solve the problem, but based on my experience, combining both is most effective. Building the model in Excel is much faster than in Python, and organizing data in Excel provides a comprehensive view of the model, facilitating faster development of the Python version. Additionally, we can solve the problem using the Solver Add-in in Excel. However, when dealing with very large datasets or complex constraints, solving it in Excel can cause lag, delays, or even crashes. This is where Python excels, Python is better suited for handling large datasets and allows the model to be reused multiple times without restarting from scratch.

However, before proceeding, I want to emphasize that the model we built is not perfect. It does not handle dynamic situations, also complex data well in practice, but it provides a solid foundation for decision-making. Some limitations of our model include:
- We limited ourselves to considering variable costs for the arcs (i.e., transport costs per unit).
- We considered only a single commodity.
- The demand was assumed to be deterministic.
- We assumed there were no capacity limits on the arcs.
- Etc.

Now that we understand the situation we are facing and have an good approach to solve it, the next step is to outline the step-by-step implementation process.
## How do we do ?
We have a checklist of tasks outlined below:
- Garther relevant data (or Data Collection)
- Develop Model (Excel - Python)
	- Network Design Baselines 
	- Running Scenarios
- Make comparison
### Garther relevant data
Supply chain network design is a combination of network optimization and facility location models. The first step in building the model is to collect data. Gathering and processing data is the foundation for analyzing and making decisions. Depending on the situation, the type of data needed and the method of collecting it can vary. In this case, we focus on three types of data:
- Transportation Data: Information on transportation costs, times, and capacities for moving goods between different nodes (suppliers, distribution centers, and retailers).
- Facility Data: Details about the locations, capacities, and operating costs facilities (plants, distribution centers, and warehouses).
- Current Product Flow: current Inbound and Outbound flows
- Others.

***INPUT DATA***

After conducting interviews and gathering on-site data, it's revealed that:
- KUMOHA is a manufacturer of auto parts, specializing in car tires. These tires are produced at **two plants** located in the THACO CHU LAI Industrial Park in Quang Nam. Quantity production currently is **divided equally** between two plants.
- Before the M&A, EcoTreat had nine retailers across Ho Chi Minh City, Dong Nai, and Binh Duong. After the M&A, EcoTreat opened six more retailers (three in Tay Ninh and three in Long An).
- With the current EcoTreat supply chain network, products are distributed from the plants to a distribution center in Ho Chi Minh City. From there, they are shipped to **fifteen Retailers (Rs)** before being delivered to customers across Ho Chi Minh City, Dong Nai, Binh Duong, Tay Ninh, and Long An. 
- R1, R2, R3 located in Ho Chi Minh
- R4, R5, R6 located in Binh Duong            
- R7, R8, R9 located in Dong Nai              
- R10, R11, R12 located in Long An            
- R13, R14, R15 located in Tay Ninh  
- Investment capital for a new Distribution Center is limited, allowing for the construction of only one additional DC.
- The CEO has identified **four candidate locations** for a new distribution center, and we are considering **selecting one**. This means we have a total of 5 possible locations for the distribution center, including the current one in Ho Chi Minh City (referred to as DC4). The other 4 locations are potential candidates for the new distribution center.
- The CEO has recommended the following policy: **Plant 2 will supply the Ho Chi Minh City distribution center (DC4) to handle the demand for Ho Chi Minh City, Binh Duong, and Dong Nai. The remaining demand will be managed by the new distribution center, referred to as DC2, which will be supplied by Plant 1.** This means DC2 has been chosen as the new distribution center.
- **Forecasted Demand Per Month** garthered from Demand Planning Team:

| Forecasted Demand  (Month) |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |
| -------------------------- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| R1                         | R2  | R3  | R4  | R5  | R6  | R7  | R8  | R9  | R10 | R11 | R12 | R13 | R14 | R15 |     |
| 620                        | 457 | 397 | 600 | 340 | 535 | 750 | 352 | 520 | 407 | 520 | 690 | 313 | 503 | 496 |     |
- **Distance from Plant to Distribution Center**

| Inbound Distance Maxtrix (Kilometers) |        |        |
| ------------------------------------- | ------ | ------ |
|                                       | Plant1 | Plant2 |
| DC1                                   | 860    | 873    |
| DC2                                   | 830    | 815    |
| DC3                                   | 815    | 790    |
| DC4                                   | 820    | 833    |
| DC5                                   | 727    | 744    |
|                                       |        |        |
- **Distance from Distribution Center to Retailer**

| Outbound Distance Matrix (Kilometers) |     |     |     |     |     |     |     |     |     |     |     |     |     |     |     |
| ------------------------------------- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
|                                       | R1  | R2  | R3  | R4  | R5  | R6  | R7  | R8  | R9  | R10 | R11 | R12 | R13 | R14 | R15 |
| DC1                                   | 89  | 78  | 69  | 95  | 85  | 73  | 88  | 73  | 86  | 90  | 93  | 77  | 55  | 79  | 91  |
| DC2                                   | 90  | 72  | 63  | 61  | 94  | 58  | 89  | 80  | 55  | 66  | 74  | 51  | 51  | 50  | 100 |
| DC3                                   | 81  | 82  | 71  | 98  | 62  | 71  | 85  | 75  | 72  | 62  | 81  | 68  | 50  | 82  | 70  |
| DC4                                   | 73  | 67  | 55  | 75  | 86  | 54  | 62  | 99  | 59  | 75  | 53  | 96  | 68  | 72  | 85  |
| DC5                                   | 89  | 74  | 80  | 97  | 100 | 101 | 87  | 74  | 101 | 97  | 105 | 78  | 88  | 80  | 80  |

- **Inbound Transportation Cost** (from Plant - DC) after transfrorming from `$/shipment` (take an average transportation cost from history data) to `$/unit/km` is $0.06/unit/Km
- **Outbound Transportation Cost** (list prices from Carrier)

| Outbound Tranportation Cost ($/unit/km) |      |      |      |      |      |      |      |      |      |      |      |      |      |      |      |
| --------------------------------------- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
|                                         | R1   | R2   | R3   | R4   | R5   | R6   | R7   | R8   | R9   | R10  | R11  | R12  | R13  | R14  | R15  |
| DC1                                     | 1.90 | 1.90 | 1.00 | 1.20 | 1.50 | 1.20 | 1.10 | 1.10 | 1.30 | 1.80 | 1.70 | 1.60 | 1.60 | 1.20 | 1.50 |
| DC2                                     | 1.00 | 1.40 | 1.20 | 1.80 | 1.90 | 1.00 | 1.60 | 1.60 | 1.30 | 1.70 | 1.50 | 1.20 | 1.00 | 1.80 | 1.20 |
| DC3                                     | 1.70 | 1.40 | 1.60 | 1.50 | 1.40 | 1.80 | 1.70 | 1.90 | 1.00 | 1.90 | 1.10 | 1.60 | 1.80 | 1.50 | 1.90 |
| DC4                                     | 1.80 | 1.40 | 1.40 | 1.70 | 1.80 | 1.30 | 1.20 | 1.50 | 1.20 | 1.90 | 1.10 | 1.20 | 1.60 | 1.60 | 1.10 |
| DC5                                     | 1.70 | 1.50 | 1.80 | 1.70 | 1.60 | 1.30 | 1.70 | 1.50 | 1.30 | 1.40 | 1.90 | 1.30 | 1.10 | 1.10 | 1.30 |

- **Plant variable costs and capacity**

| Plant Costs And Capacities |                |          |
| -------------------------- | -------------- | -------- |
|                            | Variable costs ($/Unit) | Capacity (Units) |
| Plant1                     | 8              | 3000     |
| Plant2                     | 13             | 6000     |

-  **Distribution Center Costs (Variable and Fixed Cost)**

| Distribution Center Costs |                |             |
| ------------------------- | -------------- | ----------- |
|                           | Variable costs ($/Unit) | Fixed costs ($/Month) |
| DC1                       | 8              | 20000       |
| DC2                       | 10             | 17000       |
| DC3                       | 25             | 16000       |
| DC4                       | 30             | 15000       |
| DC5                       | 50             | 11000       |

 - **Average Total Cost per Month (use 3 months historical data) is aproximately $1,400,000**
 - The CEO desire Weighted Average Distance From Retailer to Distribution Center must be less than **65 kilometers** and **80% of all customers need to be within 75 kilometers of DC** 

Using gathered data, we can determine the formula for the total cost of the supply chain network.

**Total Cost = Transportation Cost + Facility Cost**
- Total Cost = Inbound Transportation Cost + Outbound Transportation Cost + DC's Variable Cost + DC's fixed cost + Plant's Variable Cost
### Develop Model
#### Network Design Baselines

We have all the relevant data needed to design the network. Now, we should leverage it effectively.

Firstly, remember that before making any changes, we need to ensure that we have assessed and actually understand our current situation. That's why we are going to set up baselines.

A 'baseline' refers to the initial set of conditions, metrics, and configurations used as a reference point for evaluating the performance of the network.

*Note: I have a separate Excel file that formulate model (Model_Excel.xlsx), **I highly recommend opening the Excel file to understand everything clearly.** I only explain the crucial factor here*

In the case of EcoTreat, we will design three baselines:
1. ***Baseline 1 - Actual Flows***
   - We'll use the current supply chain network flow of EcoTreat, which includes 2 plants, 15 retailers, and only 1 Distribution Center (DC) in Ho Chi Minh City.
   - Build Model for calculate the total cost, then compare this with the average actual cost. This helps us validate that the model's costs reflect the actual costs.

| Inbound Flow | Plant1 | Plant2 | Open Or Not ? |
| ------------ | ------ | ------ | ------------- |
| DC1          | 0      | 0      | 0             |
| DC2          | 0      | 0      | 0             |
| DC3          | 0      | 0      | 0             |
| DC4          | 3750   | 3750   | 1             |
| DC5          | 0      | 0      | 0             |

| Outbound Flow | R1  | R2  | R3  | R4  | R5  | R6  | R7  | R8  | R9  | R10 | R11 | R12 | R13 | R14 | R15 |
| ------------- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| DC1           | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   |
| DC2           | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   |
| DC3           | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   |
| DC4           | 620 | 457 | 397 | 600 | 340 | 535 | 750 | 352 | 520 | 407 | 520 | 690 | 313 | 503 | 496 |
| DC5           | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   |
 
| Total Cost          | $1,463,333 |
| ------------------- | ---------- |
| P_var_cost          | $78,750    |
| Inbound_trans_cost  | $371,925   |
| DC_fixed_cost       | $15,000    |
| DC_var_cost         | $225,000   |
| Outbound_trans_cost | $772,658   |

| Percent of regional demand to be within 75 kilometers of the DC |     | 75%   |
| --------------------------------------------------------------- | --- | ----- |
| Average weighted distance from DC-R                             |     | 71.58 |


The total cost of Baseline 1 is approximately $1,463,333, which is very close to the average historical total cost of $1,400,000. While this may not reflect 100% precision, a residual under 10% is generally acceptable.

**=> Model reflected the actual cost => ACCEPT MODEL**

However, the current flow does not sastify the CEO's level of service requirement: 
- Percent of Retailer demand to be within 75 kilometers of the DC = 75% (require >=80%)
- Average weighted distance from DC-R = 71.58 (require <=65)

2. ***Baseline 2 - Adhere to CEO Recommendation***
   - We'll force the flows to follow the CEO's policy, which dictates that Plant 2 will supply the Ho Chi Minh City distribution center (DC4) to handle the demand for Ho Chi Minh City, Binh Duong, and Dong Nai. The remaining demand will be managed by the new distribution center, referred to as DC2, which will be supplied by Plant 1.

| Inbound Flow | Plant1 | Plant2 | Open Or Not ? |
| ------------ | ------ | ------ | ------------- |
| DC1          | 0      | 0      | 0             |
| DC2          | 2929   | 0      | 1             |
| DC3          | 0      | 0      | 0             |
| DC4          | 0      | 4571   | 1             |
| DC5          | 0      | 0      | 0             |

| Outbound Flow | R1  | R2  | R3  | R4  | R5  | R6  | R7  | R8  | R9  | R10 | R11 | R12 | R13 | R14 | R15 |
| ------------- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| DC1           | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   |
| DC2           | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 407 | 520 | 690 | 313 | 503 | 496 |
| DC3           | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   |
| DC4           | 620 | 457 | 397 | 600 | 340 | 535 | 750 | 352 | 520 | 0   | 0   | 0   | 0   | 0   | 0   |
| DC5           | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   |

| Total Cost          | $1,388,445 |
| ------------------- | ---------- |
| P_var_cost          | $82,855    |
| Inbound_trans_cost  | $374,323   |
| DC_fixed_cost       | $32,000    |
| DC_var_cost         | $166,420   |
| Outbound_trans_cost | $732,847   |

| Percent of regional demand to be within 75 kilometers of the DC |     | 84%    |
| --------------------------------------------------------------- | --- | ------ |
| Average weighted distance from DC-R                             |     | 67.216 |

Adhering to the CEO's recommendation results in a total cost improve of approximately 5%, from $1,463,333 to $1,388,445. The percentage of Retailer demand within 75 kilometers of the Distribution Center increased from 75% to 84%, meeting the requirement. However, the average weighted distance from the Distribution Center to the retailer remains below expectations.

We can see, this policy is better than current one, however, it's not the an optimized option. Maybe we have a better one, so now we will develop Baseline 3 to find the optimal DC assignment

3. ***Baseline 3 - Optimal DC Assignment***
   - We'll solve the model to get an optimized outcome, keeping all relevant data the same except not forcing any specific flow.
The optimal Distribution Center (DC) assignment aims to minimize the total network cost. This objective is achieved through an mixed integer linear programming (MILP) model.
   - The total cost function, or **Objective** in the MILP model seeks to identify the set of decision variables (flows) that result in the lowest cost. However, there are several constraints that the company must adhere to, such as ensuring that retailers meet their specified demand and that plants do not exceed their production capacities, etc. These constraints are translated into mathematical formulations and integrated into the model. Consequently, the model determines the optimal flow that minimizes the total cost while satisfying all the specified constraints.

**OBJECTIVE :** Minimize the Total Cost = Inbound Transportation Cost + Outbound Transportation Cost + DC's Variable Cost + DC's fixed cost + Plant's Variable Cost

**CONSTRAINTs:** 
- Demand Constraint: The total product supplied to each retailer must meet its forecasted monthly demand.
- Supply Constraints: The total product produced by each plant must not exceed its production capacity.
- Flow Balance Constraints: The total inflow of products into each distribution center (DC) must equal the total outflow from that DC.
- Linking Constraints: The binary variable representing the selection status of each DC must reflect the actual selection status (if the DC is selected, the "Open or Not" variable should be 1, and vice versa).
- Number of DCs Open Constraint: Currently, one DC is open, and the requirement is to open one more, resulting in a total of two open DCs.
- Regional Demand Coverage: 80% of the regional demand must be within a 75-kilometer radius of the selected DCs.
- Average Weighted Distance Constraint: The average weighted distance from the selected DCs to the retailer must be less than or equal to 65 kilometers.
- Forced DC Selection: DC4 must be selected and remain open because it is an existing DC.
- Inbound and Outbound Flow Variables: The variables representing inbound and outbound product flows should be integer values and greater than or equal to zero.
- Binary Variables for DC Selection ("open or not?"): The variables representing whether a distribution center is open or not should be binary (0 or 1).

***After Solving...*** 

**OUTCOME:**

| Inbound Flow | Plant1 | Plant2 | Open Or Not ? |
| ------------ | ------ | ------ | ------------- |
| DC1          | 0      | 0      | 0             |
| DC2          | 586    | 4500   | 1             |
| DC3          | 0      | 0      | 0             |
| DC4          | 2414   | 0      | 1             |
| DC5          | 0      | 0      | 0             |
   
| Outbound Flow | R1  | R2  | R3  | R4  | R5  | R6  | R7  | R8  | R9  | R10 | R11 | R12 | R13 | R14 | R15 |
| ------------- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| DC1           | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   |
| DC2           | 312 | 457 | 397 | 600 | 0   | 535 | 0   | 352 | 520 | 407 | 0   | 690 | 313 | 503 | 0   |
| DC3           | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   |
| DC4           | 308 | 0   | 0   | 0   | 340 | 0   | 750 | 0   | 0   | 0   | 520 | 0   | 0   | 0   | 496 |
| DC5           | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   |

| Total Cost          | $1,263,808 |
| ------------------- | ---------- |
| P_var_cost          | $82,500    |
| Inbound_trans_cost  | $368,002   |
| DC_fixed_cost       | $32,000    |
| DC_var_cost         | $123,280   |
| Outbound_trans_cost | $658,026   |

| Percent of Retailer demand to be within 75 kilometers of the DC |     | 80%  |
| --------------------------------------------------------------- | --- | ---- |
| Average weighted distance from DC-R                             |     | 64.2 |

Amazingly, after solving by tool, DC2 was selected for the new Distribution Center, aligning perfectly with the CEO's recommendation, baseline 2. However, there have been changes in the flow of products, detailed in the attached Excel file.

These adjustments have led to a reduction in total costs by approximately 8%, lowering expenses from $1,374,033 to $1,263,808 compared with baseline 2. Additionally, the average weighted distance has decreased from 67.216 to 64.2 kilometers compared with Baseline 2, meeting the required standard.

This outcome represents the optimal redesign for the new supply chain network, especially if the company decides to open an additional distribution center, DC4, in HCM City, while considering other constraints.

=> **From now the Baseline 3 will be used to compare against future design.**

#### Running Scenarios
Baseline 3 represents an optimal solution within its specific constraints. Next, we will create expanded scenarios to observe how the model responds to varying conditions. However, it's important to recognize that running numerous scenarios isn't always the best approach. Identifying the relevant scenarios and, most importantly, understanding how to interpret the results is crucial.

1. ***Scenario 1: There are no capital investment restrictions***

Baseline 3 is an optimal solution, but it is based on the assumption that we only have enough investment capital to open one additional DC. This restricts the model, forcing the total number of open DCs to be two, including the mandatory selection of the existing DC4.

However, if there are no capital investment restrictions, allowing us to open any number of DCs to minimize total costs, we need to explore the following:
- How will the model react in this situation? Will there be any changes in the product flow?
- What are the potential savings?
- How will the level of service be affected?"

To explore a scenario with no capital investment restrictions, we will adjust the model's constraints as follows:
- Change the minimum number of DCs to 0 and the maximum to 5.
- Eliminate the constraint that forces DC4 to be open.

**OUTCOME**

| Inbound Flow | Plant1 | Plant2 | Open Or Not ? |
| ------------ | ------ | ------ | ------------- |
| DC1          | 1089   | 0      | 1             |
| DC2          | 145    | 4500   | 1             |
| DC3          | 0      | 0      | 0             |
| DC4          | 1766   | 0      | 1             |
| DC5          | 0      | 0      | 0             |
 
| Outbound Flow | R1  | R2  | R3  | R4  | R5  | R6  | R7  | R8  | R9  | R10 | R11 | R12 | R13 | R14 | R15 |
| ------------- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| DC1           | 0   | 0   | 397 | 0   | 340 | 0   | 0   | 352 | 0   | 0   | 0   | 0   | 0   | 0   | 0   |
| DC2           | 620 | 457 | 0   | 600 | 0   | 535 | 0   | 0   | 520 | 407 | 0   | 690 | 313 | 503 | 0   |
| DC3           | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   |
| DC4           | 0   | 0   | 0   | 0   | 0   | 0   | 750 | 0   | 0   | 0   | 520 | 0   | 0   | 0   | 496 |
| DC5           | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   |

| Total Cost          | $1,229,575 |
| ------------------- | ---------- |
| P_var_cost          | $82,500    |
| Inbound_trans_cost  | $370,351   |
| DC_fixed_cost       | $52,000    |
| DC_var_cost         | $108,142   |
| Outbound_trans_cost | $616,583   |

| Percent of regional demand to be within 75 kilometers of the DC |     | 81%   |
| --------------------------------------------------------------- | --- | ----- |
| Average weighted distance from DC-R                             |     | 64.84 |

Compared to Baseline 3, we achieved approximately 3% total cost savings, reducing expenses from $1,263,808 to $1,229,575. Increasing the number of DCs (upto 3 DCs open) raises the DC fixed cost. However, with more DCs, the distance from DC to retailer decreases, leading to a reduction in outbound transportation costs.

The level of service does not change significantly. This means that investing in one additional DC results in a 3% total cost savings per month compared to Baseline 3 (which has 2 DCs). This information will be useful for the CEO to evaluate whether the 3% total cost savings per month justifies the investment in one additional DC.

2. ***Scenario 2: With a 50% increase in demand, Ecotreat is considering expanding the plant's capacity.***

The company is considering which plant to prefer if future demand from all retailers increases by 50%, requiring an expansion of the plant's capacity. Which plant should they choose? 
There are some changes in model:
- Forecasted Demand raise up 1.5 times

| Forecasted Demand  (Month) |     |     |     |     |     |      |     |     |     |     |      |     |     |     |
| -------------------------- | --- | --- | --- | --- | --- | ---- | --- | --- | --- | --- | ---- | --- | --- | --- |
| R1                         | R2  | R3  | R4  | R5  | R6  | R7   | R8  | R9  | R10 | R11 | R12  | R13 | R14 | R15 |
| 930                        | 686 | 596 | 900 | 510 | 803 | 1125 | 528 | 780 | 611 | 780 | 1035 | 470 | 755 | 744 |

- Set capacity of 2 plant = 9,999,999 (infinity)

**OUTCOME**

| Inbound Flow | Plant1 | Plant2 | Open Or Not ? |
| ------------ | ------ | ------ | ------------- |
| DC1          | 0      | 0      | 0             |
| DC2          | 7633   | 0      | 1             |
| DC3          | 0      | 0      | 0             |
| DC4          | 3620   | 0      | 1             |
| DC5          | 0      | 0      | 0             |

| Outbound Flow | R1  | R2  | R3  | R4  | R5  | R6  | R7   | R8  | R9  | R10 | R11 | R12  | R13 | R14 | R15 |
| ------------- | --- | --- | --- | --- | --- | --- | ---- | --- | --- | --- | --- | ---- | --- | --- | --- |
| DC1           | 0   | 0   | 0   | 0   | 0   | 0   | 0    | 0   | 0   | 0   | 0   | 0    | 0   | 0   | 0   |
| DC2           | 468 | 686 | 596 | 900 | 1   | 803 | 0    | 528 | 780 | 611 | 0   | 1035 | 470 | 755 | 0   |
| DC3           | 0   | 0   | 0   | 0   | 0   | 0   | 0    | 0   | 0   | 0   | 0   | 0    | 0   | 0   | 0   |
| DC4           | 462 | 0   | 0   | 0   | 509 | 0   | 1125 | 0   | 0   | 0   | 780 | 0    | 0   | 0   | 744 |
| DC5           | 0   | 0   | 0   | 0   | 0   | 0   | 0    | 0   | 0   | 0   | 0   | 0    | 0   | 0   | 0   |

| Total Cost          | $1,852,489 |
| ------------------- | ---------- |
| P_var_cost          | $90,024    |
| Inbound_trans_cost  | $558,227   |
| DC_fixed_cost       | $32,000    |
| DC_var_cost         | $184,930   |
| Outbound_trans_cost | $987,307   |

Upon examination, we can observe that the total forecasted demand in Scenario 2 exceeded the combined total capacity of the two plants as per the Baseline 3 scenario. Interestingly, when we set unlimited capacity for both plants, Plant 1 supplied all products for the network. This indicates that Plant 1 is more cost-effective compared to Plant 2. Therefore, if we are considering expanding the capacity of one of the two plants to meet the increased demand, the optimal choice would be to expand Plant 1 to bridge the larger capacity deficit.

3. ***Scenario 3: DC2 Closure***

For some reason, DC2 is eliminated from the list of potential locations for a new distribution center, and the CEO asks me to find an alternative, how should I proceed?

We will modify the constraints to effectively eliminate DC2 by setting its fixed cost to 9,999,999 (essentially infinity). This will make the cost of opening DC2 prohibitively high, forcing the model to prioritize finding an alternative location that minimizes overall costs.

I tried running the model in Excel, but it seems to get stuck when solving Scenario 3. It takes a long time to generate outcomes. This is a good opportunity to leverage Python, which is better suited for handling very large datasets or complex constraints.

I have built an Model in attached Python file, now I will transform the outcome into a table format: 

*NOTE: I highly recommend opening the attached Python file (Scenario3_Python.ipynb) for a full understanding.*

**OUTCOME**

| Inbound Flow | Plant1 | Plant2 | Open Or Not ? |
| ------------ | ------ | ------ | ------------- |
| DC1          | 0      | 0      | 0             |
| DC2          | 0      | 0      | 0             |
| DC3          | 0      | 2269   | 1             |
| DC4          | 3000   | 2231   | 1             |
| DC5          | 0      | 0      | 0             |

 
| Outbound Flow | R1  | R2  | R3  | R4  | R5  | R6  | R7  | R8  | R9  | R10 | R11 | R12 | R13 | R14 | R15 |
| ------------- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| DC1           | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   |
| DC2           | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   |
| DC3           | 0   | 0   | 0   | 0   | 340 | 0   | 0   | 352 | 0   | 407 | 0   | 690 | 313 | 0   | 167 |
| DC4           | 620 | 457 | 397 | 600 | 0   | 535 | 750 | 0   | 520 | 0   | 520 | 0   | 0   | 503 | 329 |
| DC5           | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   |

After obtaining the outcome from Python, we can plug it back into the Excel model to scrutinize the data in more detail. This allows for further analysis and refinement within the familiar Excel environment.

| Total Cost          | $1,427,480 |
| ------------------- | ---------- |
| P_var_cost          | $82,500    |
| Inbound_trans_cost  | $366,656   |
| DC_fixed_cost       | $31,000    |
| DC_var_cost         | $213,655   |
| Outbound_trans_cost | $733,669   |

Although we still end up with two DCs opening, the total cost is approximately 13% higher. This increase mainly stems from DC variable costs and outbound transportation costs (this cause by distance aslo outbound transportation cost from DC3-Retailer higher than DC2-Retailer). Despite this, DC3 emerges as the optimal alternative selection for a new distribution center.

Besides, an interesting observation is that the percentage of demand within 75 kilometers of the DC improves significantly, increasing by more than 15% from 80% to 95.6%. This makes DC3 a viable selection if the company wants to upscale the level of service.
### Make comparison

|                              | BL1: Actual    | BL2: Adhere Policy  | BL3: Optimal   | S1: No Investment Limits | S2: Expand for Demand  <br>(no limit capacity) | S3: DC2 Closure |
| ---------------------------- | -------------- | ------------------- | -------------- | ------------------------ | ---------------------------------------------- | --------------- |
| **Total Cost**               | **$1,463,333** | **$1,388,445** <br> | **$1,263,808** | **$1,229,575**           | **$1,852,489**                                 | **$1,427,480**  |
| Plant Variable Cost          | $78,750        | $82,855 <br>        | $82,500        | $82,500                  | $90,024                                        | $82,500         |
| Inbound Transportation Cost  | $371,925       | $374,323 <br>       | $368,002       | $370,351                 | $558,227                                       | $366,656        |
| DC Fixed Cost                | $15,000        | $32,000             | $32,000        | $52,000                  | $32,000                                        | $31,000         |
| DC Variable Cost             | $225,000       | $166,420            | $123,280       | $108,142                 | $184,930                                       | $213,655        |
| Outbound Transportation Cost | $772,658       | $732,847            | $658,026       | $616,583                 | $987,307                                       | $733,669        |
|                              |                |                     |                |                          |                                                |                 |
| # DC Open                    | 2              | 2                   | 2              | 3                        | 2                                              | 2               |
| LOS - Avg Distance           | 71.58          | 67.22               | 64.20          | 64.84                    | 64.20                                          | 65.00           |
| LOS - PctIn75Kilometers      | 75.0%          | 84.0%               | 80.0%          | 80.6%                    | 80.0%                                          | 95.6%           |
| Total Demand                 | 7500           | 7500                | 7500           | 7500                     | 11253                                          | 7500            |
| $/demand                     | $195.11        | 185.13              | 168.5          | 163.9                    | 164.6                                          | 190.3           |

We have summarized six cases, including three baseline scenarios and three expanded scenarios.

Comparing the $/demand across scenarios, Scenario 1 yields the lowest value. However, it requires significant effort to evaluate due to the company's capital investment limitations. We need to consider if these savings justify the opportunity cost.

Scenario 3, on the other hand, has a relatively high $/demand at around $190, but it ensures that approximately 95% of retailer demand is within 75km of a DC. In certain cases, this high level of service can be a competitive advantage, helping to dominate the market.

Based on the gathered data and requirements, the most suitable choice appears to be an Optimal Baseline (Baseline 3). By following this optimal policy, EcoTreat can **save approximately 14% in total supply chain network costs**, equating to **$199,525 per month** compared to the current flow (Baseline 1). Additionally, this scenario maintains the required level of service, **reduces the average distance to retailer by approximately 11%**, and **improves the percentage of retailer demand within 75km of a DC by 5%.**
## Summary

EcoTreat, an auto parts company, is expanding its supply chain network to serve the growing demand for its products. The analysis involved developing baseline scenarios and optimizing the network through mixed-integer linear programming. The optimal solution recommends opening a new distribution center, which is expected to save around 14% in total supply chain costs compared to the current network while satisfying the given Level of Service requirements.

Remember that this analytical method is not a "holy grail." These network models are solved using MILP programs, so they have some limitations, as mentioned above. The model cannot reflect all factors or the dynamic nature of real-world situations. However, it remains a valuable prescriptive tool that helps describe how the network responds to different scenarios and supports decision-making.
