<!-- This is the markdown template for the final project of the Building AI course, 
created by Reaktor Innovations and University of Helsinki. 
Copy the template, paste it to your GitHub README and edit! -->

# OPTIMIZATION PROBLEM
### How to buy healthy food at a convenience store with low cost and high nutritional benefits.

<p align="center">
  <img width="600" height="450" src="https://user-images.githubusercontent.com/63940535/131987938-2b4fc99f-7714-4370-8abf-d90bb2d296e9.png">
</p>




## I.Introduction


Now, people are more interested in health care. Whether eating or exercising But with work or time, it is difficult to prepare food. Causing to have to find food in a convenience store In which convenience stores have more healthy food, but how do we know which food is the cheapest and most suitable for us.
   
In the optimization subject, we choose which products are suited for the needs of the user and the lowest price using Linear programing to find answers and use Python Computer-language to resolve this issue.




## II.Methodology 
### A.	Collected Data

The data are taken from watching food reviews in convenience stores such as [Pantip](https://pantip.com/) [MThai](https://mthai.com/) or ***social media***, then choose popular health food products, then take photos and fill them into `Excel` or `Csv` files.
<p align="center">
   <img width="617" alt="Screen Shot 2564-09-03 at 17 31 31" src="https://user-images.githubusercontent.com/63940535/131991706-6ecf8866-3bfa-428a-94b7-7e658ade6929.png">
</p>

The data will include:
* Item names
* Item brand
* Taste
* Serving size
* Serving per container
* Total energy (Calories)
* Total Protein
* Total Carbohydrate
* Total Fat
* Price

We've collected more than **60** item. All products come from a web-based product or social media review as referred above. We will upload files on the `GitHub` so that it can be edited and used at any time.

### B.	Input
   - 1. We will use optimization tools to make python Computer-languages using Google collaborator. We will pull data from “GitHub” by **url** first.

```
import pandas as pd 
url = 'https://raw.githubusercontent.com/frukkurt/food/master/Food%20copy.csv'
df = pd.read_csv(url)
df
```
<img width="1073" alt="Screen Shot 2564-09-03 at 17 56 05" src="https://user-images.githubusercontent.com/63940535/131994754-c68b38d5-8082-4efb-aee1-fdbf2c3a8f24.png">

   - 2. Check that the products we choose, Are there any products whose nutritional data does not match the calories by adding features such as Calories_Calculator ***(Eq 1)***, Price_per_Serving ***(Eq 2)***, Different_Calories.And delete data calories different more than 10 calories. Because sometimes the nutrients calculated may be not match calorie that is written on the product.

<p align="center">
  <img width="800" alt="Screen Shot 2564-09-04 at 10 09 49" src="https://user-images.githubusercontent.com/63940535/132080554-505e619d-6988-4bff-a9ae-2f63fc9520ce.png">
</p>

```
cal=(df["Protein"]*4)+(df["Carbohydrate"]*4)+(df["Fat"]*9)
PPS=df["Price"]/df["Serving per Contain"]
Diff=abs(cal-df["Calories"])
df1=df.assign(Calories_Calculator = cal,Price_per_Serving = PPS,Different_Calories = Diff)
difin=df1.index[df1['Different_Calories'] >= 10].tolist()
df2=df1.drop(df1.index[difin])
df2=df2.reset_index()
df2=df2.drop(columns=["index"])
df2
```
<img width="1587" alt="Screen Shot 2564-09-04 at 10 17 23" src="https://user-images.githubusercontent.com/63940535/132080697-f417cbd3-dbdd-4f6f-ab2e-eb9216ee356f.png">

   - 3. The input is then received as the user's information, filling in the name, age, gender, weight, height, number of days in the exercise, number of meals in exercise to bring up **BMR** (Basal Metabolic Rate) and **TDEE** (Total Daily Energy Expenditure). **BMR** is calculated separately for men and women ***(Eq 3.1,3.2)*** and **TDEE** calculated using activity from user.

<p align="center">
   <img width="640" alt="Screen Shot 2564-09-04 at 10 18 51" src="https://user-images.githubusercontent.com/63940535/132080739-1a47f8f7-0a11-4a8e-81ce-1330caa95411.png">
</p>

#
**TDEE**:
- Sitting and working not exercising = **BMR** x 1.2
- Low exercise or sport around 1-2 days per week = **BMR** x 1.375
- Moderate exercise or sport around 3-5 days per week = **BMR** x 1.55
- Hard exercise or sports around 6-7 days per week = **BMR** x 1.725
- Hard exercise or sports twice day every day = **BMR** x 1.9
#


   - 4. When we are aware of **BMR** and **TDEE** values, we can find out how much the nutrient needs each user receives. We do the default setting of macro nutrient is *protein* 30%, *carbohydrate* 40%, *Fat* 30% from **TDEE**. Then it's calculated how much protein, carbohydrates and fat in one meal. Because we don’t know about goal from user ,default is maintain calories.

<img width="1000" alt="Screen Shot 2564-09-04 at 11 06 10" src="https://user-images.githubusercontent.com/63940535/132081851-bb8c631f-4e20-4458-8b9d-9d63fe859528.png">

## III.Optimization
   - 1. We define **Decision variables**, **Objective functions** and **Constraints first**. 

**Decision variable:** When ***i***  represents the index of food.


<p align="center">
   <img width="260" alt="Screen Shot 2564-09-04 at 11 09 38" src="https://user-images.githubusercontent.com/63940535/132081930-8fe91c30-c415-4c1b-8c93-10b4695ba623.png">
</p>



**Objective function:** When ***Ei*** is price and ***i*** represents the index of food.


<p align="center">
   <img width="132" alt="Screen Shot 2564-09-04 at 11 10 31" src="https://user-images.githubusercontent.com/63940535/132081955-3fd276ad-a661-4839-a77f-eda312773a25.png">
</p>


**Constraints:** When ***Ai*** is calorie intake (Kcal), ***Pi*** is protein intake(gram), ***Ci*** is carbohydrate intake(gram) and ***Fi*** is fat intake(gram).



<p align="center">
   <img width="325" alt="Screen Shot 2564-09-04 at 11 13 33" src="https://user-images.githubusercontent.com/63940535/132082047-3c49cc37-779c-4d6a-bca5-d7211c8ad495.png">
</p>


- 2. We use [scipy.optimize](https://docs.scipy.org/doc/scipy/reference/generated/scipy.optimize.linprog.html#scipy.optimize.linprog)  using function `linprog`. This is a library that uses **simplex algorithm**. But in linprog has a problem is output from linprog is **float number**, we cannot set output to integer number.
```
from scipy.optimize import linprog
import numpy as np
z=np.array(df2["Price_per_Serving"])
C =np.array([(1*df2["Calories"]),(-1*df2["Protein"]),(-1*df2["Carbohydrate"]),(-1*df2["Fat"])])
b =np.array([round(1*TDEE/meal),round(-1*P_g),round(-1*C_g),round(-1*F_g)])

res = linprog(z, A_ub = C, b_ub = b, bounds = (0,None), method='revised simplex')
res
```

<p align="center">
   <img width="1000" alt="Screen Shot 2564-09-04 at 11 27 29" src="https://user-images.githubusercontent.com/63940535/132082359-edf9b596-a714-4175-8432-8e4f2e394d3e.png">
</p>

- 3. Causing us to switch to `Pulp`. Because We can define output as **integer** or **float**. `Pulp` is an open source linear programming package for python. `PulP` can be installed using `pip`. `Pulp` is similar to using **excel**, such as defining mark of constraint or output type.

```
from pulp import * 

# set the dictionary for each feature
prob = LpProblem('Buy',LpMinimize) # Objective function

inv_items = list(df2['Name']+" brand:"+df2['Brand']+" taste:"+df2["Taste"]) # Variable name

Price_per_Serving = dict(zip(inv_items,df2["Price_per_Serving"])) # Function

Calories = dict(zip(inv_items,df2["Calories_Calculator"])) # Function 

Protein = dict(zip(inv_items, df2['Protein'])) # Function

Carbohydrate = dict(zip(inv_items, df2['Carbohydrate'])) # Function

Fat = dict(zip(inv_items, df2['Fat'])) # Function

Brand = dict(zip(inv_items, df2['Brand'])) # Function
# next, we are defining our decision variables as investments and are adding a few parameters to it
inv_vars = LpVariable.dicts('Pick:', inv_items, lowBound=0, cat='Integer')

# set the decision variables
# all add in the problem setting
prob += lpSum([inv_vars[i] * Price_per_Serving[i] for i in inv_items])

# Constraint
prob += lpSum([inv_vars[i]*Calories[i] for i in inv_items]) >= cal, 'Total_Calories'
prob += lpSum([inv_vars[i]*Protein[i] for i in inv_items]) == round(P_g1), 'Total_Protein'
prob += lpSum([inv_vars[i]*Carbohydrate[i] for i in inv_items]) == round(C_g1), 'Total_Carbohydrate'
prob += lpSum([inv_vars[i]*Fat[i] for i in inv_items]) == round(F_g1), 'Total_Fat'

#prob += lpSum([inv_vars[i]*Fat[i] for i in inv_items]) <= round(P_g1+4), 'Not over_Protein'
#prob += lpSum([inv_vars[i]*Carbohydrate[i] for i in inv_items]) <= round(C_g1+4), 'Not over_Carbohydrate'
#prob += lpSum([inv_vars[i]*Fat[i] for i in inv_items]) <= round(F_g1+2), 'Not over_Fat'

prob += lpSum([inv_vars[i]for i in inv_items]) >= 0, 'Item'

prob.solve()

```
- 4. We use [scipy.optimize](https://docs.scipy.org/doc/scipy/reference/generated/scipy.optimize.linprog.html#scipy.optimize.linprog)  using function `linprog`. This is a library that uses **simplex algorithm**. But in linprog has a problem is output from linprog is **float number**, we cannot set output to integer number.

| Status      | Description |
| ----------- | ----------- |
| Not Solved  | Status prior to solving the problem.       |
| Optimal     | An optimal solution has been found.        |
| Infeasible  | There are no feasible solutions. ***(e.g. if you set the constraints x <= 1 and x >=2)***       |
| Unbounded   | The constraints are not bounded, maximising the solution will tend towards infinity. ***(e.g. if the only constraint was x >= 3)***       |
| Undefined   | The optimal solution may exist but may not have been found.        |

- 5. The output of this optimization will result in **3 parts** which are information of the product, the number of products to buy and the amount of nutrients and Price to be paid.

<p align="center">
   <img width="1000" alt="Screen Shot 2564-09-04 at 11 43 28" src="https://user-images.githubusercontent.com/63940535/132082677-2c258f77-b72a-4fd5-9f3c-b6695bd3a5fc.png">
</p>

- 6. We will have a price based on optimization and the price actually paid. ***(e.g. Whole wheat in one piece has 5 serving per contain, but optimization eat 2 serving when we calculated price, we use price per serving. Causing us to have the actual price paid and the price calculated from the Pulp.)***



## IV.Result & Comparison
By optimizing using `linprog` and `Pulp`, `linprog` is unable to give an integer. `Pulp` provides an integer. We can compare the results with `Excel` using *add-in* solver to compare. Find out if there are matching results or not.
In terms `Excel` and `linprog` result ,there is no similarity because `linprog` cannot use equal symbols. `Pulp` results exactly as excel.

#
`Excel` and `linprog`
<p align="center">
   <img height="375" width="475" alt="Screen Shot 2564-09-04 at 12 07 22" src="https://user-images.githubusercontent.com/63940535/132083220-ac5df833-ba90-405c-a91a-6c8d848362ac.png">
   <img height="375" width="475" alt="Screen Shot 2564-09-04 at 12 08 52" src="https://user-images.githubusercontent.com/63940535/132083251-905b377b-58cb-4f07-a40c-ab64e4f2275e.png">
</p>

`Excel` and `Pulp`
<p align="center">
   <img height="375" width="475" alt="Screen Shot 2564-09-04 at 12 14 03" src="https://user-images.githubusercontent.com/63940535/132083372-502f7d6d-958b-4f9a-9e1f-18b482e354fa.png">
   <img height="375" width="475" alt="Screen Shot 2564-09-04 at 12 15 08" src="https://user-images.githubusercontent.com/63940535/132083389-380ad15e-8a44-47e5-82cf-8c6f5c38b7eb.png">
</p>

#



## V.Conclusion
- By choosing optimization tools such as `linprog` and `Pulp`, `Pulp` is easier to apply and more versatile than `linprog`. The problem is to buy healthy food at the lowest price and get the desired nutrients according to the characteristics user.
- By comparing to other tools such as `Excel`, which are very popular and easy to use, `Pulp` can provide similar results than `linprog`.
- From the use of `Colaboratory`, it can be expanded such as If the data is retrieved directly from [7-11](https://www.7eleven.co.th/), then the data should be improved or Interface to get the user, but the `Colaboratory` still have few tools to use May have to wait a while to have tools that are more suitable.


## VI.References
[1] Sci.Py.org, open-source software for mathematics, science, and engineering [Online] Available: https://docs.scipy.org/doc/scipy-0.15.1/reference/generated/scipy.optimize.linprog.html#r120

[2] pypi.org, Pulp is an LP modeler written in python. Pulp can generate MPS or LP files and call GLPK, COIN CLP/CBC, CPLEX, and GUROBI to solve linear problems.[online]Available: https://pypi.org/project/PuLP/

[3] FATNEVER.com, online calculator tools for BMR and TDEE, [Online]Available: https://www.fatnever.com/bmr/

[4]CookingLight.com, Cooking Light is part of the All recipes Food Group.[online] Available: https://www.cookinglight.com/eating-smart/macro-diet-counting-macros-weight-loss-better-nutrition
[5] GitHub food repository, GitHub for this project [Online]Available:
https://github.com/frukkurt/food

