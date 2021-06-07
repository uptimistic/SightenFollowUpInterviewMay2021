
# SightenFollowUpInterviewMay2021

*****


## Problem 1 - Parsing CSV input string list object 

> *Problem Statement*: 
> Given the following inputs and outputs, please write a function parse_csv such that parse_csv(input_1) returns output_1 and parse_csv(input_2) returns output_2.

* Inputs
  * Input1
  
  ```python
     input_1 = (
    'state,solar,storage,energy_efficiency\n'
    'CA,active,inactive,active\n'
    'NV,inactive,active,active'
     )
  ```


  * Input2
  
  ```python
  input_2 = (
    'product_type,is_active\n'
    'loan,true\n'
    'lease,true\n'
    'ppa,false\n'
    'cash,false\n'
    )
  ```


* Outputs
  * Output1
  
  ```python
     output_1 = [
    {
        'state': 'CA',
        'solar': 'active',
        'storage': 'inactive',
        'energy_efficiency': 'active',
    },
    {
        'state': 'NV',
        'solar': 'inactive',
        'storage': 'active',
        'energy_efficiency': 'active',
    }
   ]

  ```


  * Output2
  
  ```python
  output_2 = [
    {
        'product_type': 'loan',
        'is_active': True
    },
    {
        'product_type': 'lease',
        'is_active': True
    },
    {
        'product_type': 'ppa',
        'is_active': False
    },
    {
        'product_type': 'cash',
        'is_active': False
    }
   ]
  ```

## Solution to Problem 1 :  File Read/Write, Funtions, Data Structures /Algorithmic thinking

### Step 0 (Test Driven Development approach) - reverse-engineered inputs for a universally working parsing Python function on a CSV file
Parsing CSV file to generate the supplied input string Python tuple collections object for each row
The following code below, when supplied with a `.csv` file(or any text based file extension) will read each line or row  as a iterable list item

```python

import pprint
def inputfile_extract(inputFile):
    with open(inputFile,'r', encoding='utf-8') as fileObject:
        collected_datalist=[eachLineFromFile for eachLineFromFile in fileObject ]
        # collected_datalist=[eachLineFromFile.strip('\n') for eachLineFromFile in fileObject ] # .strip('\n') can be omitted if end of line charatcer is desired
        #return collected_datalist
        print("input = ")
        pprint.pprint(tuple(collected_datalist),sort_dicts=False)# pretty print to retain order
inputfile_extract('input1.csv') # test input file is saved in repository
inputfile_extract('input1.csv') # test input file is saved in repository

```
### Step 1 - Pasring input string Python tuple collections object for individual strings in each row 

* Removing the end of line characters `/n` at the end of each row
  * Option 1 : using String split(<delimeter>) method and specifying the delimeter argument as  `/n`  OR
  * Option 2 : using String splitlines() method without specifying  `/n`  argument- this method was used in the initial parsing code



```python
import pprint # Python pretty print module to render desired display of output
def parse_csv(input):
    parsedDictList=[]# initial collection list to contain mapped dictionaries
    allRows = input.splitlines()# this is the same is .splitline('\n') method
    eachRowTuples = [tuple(eachRow.split(',')) for eachRow in allRows]
    headerRow=eachRowTuples[0]# first index of list of lists
    # index=1 refers to the second row(first row beneath the header row)
    #index spans between 1 and one less than the length of allRows
    for index in range(1,len(eachRowTuples)):
        parsedDictList.append(dict(zip(headerRow,eachRowTuples[index])))
    print("output = ")    
    pprint.pprint(parsedDictList,sort_dicts=False)# pretty print to retain order
    #print("output={} ".format(parsedDictList)) #  non-pretty formated string print

 ### Given Input for Problem 1
input_1 = (
    'state,solar,storage,energy_efficiency\n'
    'CA,active,inactive,active\n'
    'NV,inactive,active,active'
)

input_2 = (
    'product_type,is_active\n'
    'loan,true\n'
    'lease,true\n'
    'ppa,false\n'
    'cash,false\n'
)

parse_csv(input_1)
parse_csv(input_2)
```
*****


## Solution to Problem 2 & 3 :  
 > *Problem Statement : Based on the given Django ORM Models, write Django ORM and corresponding SQL queries :* 

 > * Retrieve all Quotes
 > * Retrieve all Quotes where the install_cost equals 1000
 > * Retrieve all Quotes where the Project name equals "Project 1"
 > * Retrieve all Quotes where the GenerationProject capacity is greater than 15
 > * Retrieve all Projects that have a Quote with an install_cost greater than 15,000
 > * [BONUS] Retrieve all Projects that have at least one Quote
 > * [BONUS] Retrieve all Projects whose Quotes have an average install_cost across its Quotes greater than 15,000




####  Solution to Django-SQL Problem : Databases

 
 #### 2-1. Retrieve all Quotes -Django
 
```python
quotes = Quote.objects.all()
for quote in quotes:
    print(quote.project)
    print(quote.related_name)
    print(quote.install_cost)
``` 
 
 #### 3-1. Retrieve all Quotes -SQL
 
```sql
SELECT *
FROM Quote;
```

 #### 2-2. Retrieve all Quotes where the install_cost equals 1000 -Django
 
```python
Quote.objects.filter(install_cost=1000)
```

#### 3-2. Retrieve all Quotes where the install_cost equals 1000 -SQL
```sql
 SELECT *
FROM Quote
WHERE install_cost = 1000;
```

 
#### 2-3. Retrieve all Quotes where the Project name equals "Project 1 " - Django
```python
projects = Project.objects.prefetch_related('name').filter(name="Project 1")
quotes = projects.name.all()
```
 
 
#### 3-3. Retrieve all Quotes where the Project name equals "Project 1 " - SQL

```sql
SELECT *
FROM Quote
LEFT JOIN Project
ON Quote.project_id = Project.name
WHERE Project.name= "Project 1";
```



#### 2-4. Retrieve all Quotes where the GenerationProject capacity is greater than 15- Django
```python

generationproject = GenerationProject.objects.prefetch_related('capacity').filter(capacity__gt=15)
quotes = generationproject.capacity.all()
```

#### 3-4. Retrieve all Quotes where the GenerationProject capacity is greater than 15 -SQL 
 
```sql
ELECT *
FROM Quote
LEFT JOIN GenerationProject
ON Quote.generationproject_id = GenerationProject.capacity
WHERE GenerationProject.capcity > 15;
```

 #### 2-5. Retrieve all Projects that have a Quote with an install_cost greater than 15,000- Django 

```pythonquote = Quote.objects.prefetch_related('install_cost').get(install_cost__gt=15000)
projects = quote.install_cost.all()

```
#### 3-5. Retrieve all Projects that have a Quote with an install_cost greater than 15,000- SQL
```sql
 ELECT *
FROM Project
LEFT JOIN Quote
ON Project.quote_id = Quote.install_cost
WHERE Quote.install_cost > 15,000;
```

 
 
 
Project tabular schema concept representation
| name | generation_project | related_name |
| :---:       |     :---:      |          :---: |
| name_1  |generation_project_1    |related_name_1    |
| name_2     | generation_project_2       | related_name_2    |
 | name_n     | generation_project_n       | related_name_n    |

 
 
  GenerationProject tabular schema concept representation
| capacity | input_mode | 
| :---:       |     :---:      |   
| capacity_1   | input_mode_1     | 
| capacity_2     | input_mode_2      | 
| capacity_n     | input_mode_n      | 

 
 
 
 
  Quote tabular schema concept representation
| project | install_cost |  
| :---:       |     :---:      |  
| project_1 |    install_cost_1    
| project_2 |     install_cost_2|       
 | project_n |    install_cost_n|       

 
 ### Given Input for Problems 2 &3 
 

 ```python
 # apps/project/models.py
from django.db import models

class Project(models.Model):
   name = models.CharField(max_length=64)
   generation_project = models.OneToOneField(
       'project.GenerationProject',
       related_name='project',
       null=True,
       blank=True,
   )

class GenerationProject(models.Model):
   capacity = models.FloatField(default=0)
   input_mode = models.CharField(
       max_length=10,
       choices=(
           ('RSD', 'Remote System Design'),
           ('OFFSET', 'Usage Offset'),
       ),
       default='RSD',
   )


# apps/quote/models.py
from django.db import models

class Quote(models.Model):
   project = models.ForeignKey(
       'project.Project',
       related_name='quotes',
   )
   install_cost = models.FloatField()
 ```
 
 *****
 

## Solution to Problem 4 :  Algorithmic Thinking, Object Oriented Programming 
 
  > *Problem Statement : Write the function get_commission_amount that returns the commission amount based on a given commission_model and sale :*

 
 Whiteboard Mindmap Solution to Problem before Coding 
 <p align="center">
  <img src="https://github.com/uptimistic/SightenFollowUpInterviewMay2021/blob/main/Whiteboard.MindMap.Problem4.jpg">
</p>
 
Mindmap Concept Complexity Tree Representation before coding 
 ```bash
 ├── CommisionAmmount
│   ├── CommisionModel
│   │   ├── CommissionModel1
│   │   │   ├── Base
│   │   │   │   ├── Model1
│   │   │   │   │   ├── Base
│   │   │   │   │   ├── CommisionTier1
│   │   │   │   │   ├── Increment(Delta)
│   │   │   │   │   └── TotalSaleofUnitsSold(Cost)
│   │   │   │   └── Model2
│   │   │   │       ├── Base
│   │   │   │       ├── CommisionTier2
│   │   │   │       ├── CommissionAmtPerUnit
│   │   │   │       └── Increment(Delta)
│   │   │   ├── CommisionTier1
│   │   │   │   ├── Base
│   │   │   │   ├── Increment(Delta)
│   │   │   │   ├── Max
│   │   │   │   └── UnitCost
│   │   │   ├── Increment(delta)
│   │   │   └── TotalSalePriceofUnitsSold(Cost)
│   │   └── CommissionModel2
│   │       ├── Base
│   │       │   ├── Model1
│   │       │   │   ├── Base
│   │       │   │   ├── CommisionTier1
│   │       │   │   ├── Increment(Delta)
│   │       │   │   └── TotalSaleofUnitsSold(Cost)
│   │       │   └── Model2
│   │       │       ├── Base
│   │       │       ├── CommisionTier2
│   │       │       ├── CommissionAmtperUnit
│   │       │       └── Increment(Delta)
│   │       ├── CommisionAmtPerUnit
│   │       ├── CommisionTier2
│   │       │   ├── Base
│   │       │   ├── Increment(Delta)
│   │       │   ├── Max
│   │       │   └── UnitSold
│   │       └── Increment(delta)
│   └── Sale
│       ├── Total Sale of Units Sold(Cost)
│       └── Units Sold
 ```
 
 ```
Model 1
y= mx+c
y = Commission(amount per $ sold) 
x= Tier (total cost of units sold)
m= model slope =(125-25)/(5000-1000)=0.025
c= y -intercept =25 
Commission(cost)= 25+0.025*Tier(total cost of units sold)
 
Model 2
y= mx+c
y = Commission(amount per $ sold) 
x= Tier (# of units sold)
m= model slope =(45-30)/(15-0)=1
c= y -intercept =30 
Commission(amount per $ sold)= 30+Tier (# of units)
Delta Tier = 5 units 
Delta Commision = $5
```
 
 ### Cooded Solution to Problem 4 
 ```python
#*******************************************************************************
# CODE FOR PROBLEM 4- OPTIMIZED CLASS OBJECT 
#*******************************************************************************



class get_commission_amount:

    # units = total units sold
    # cost = total sales price or total sales cost
    def __init__(self, units, cost):
        self.units = units    # instance variable unique to each instance
        self.cost = cost    # instance variable unique to each instance


    """
      The only thing to change in Commision class are Tier and Commision parameters

      Model1:
                model1_tier = (1000, 1000, 5000)
                #(base,increment,max_value)=(1000, 1000, 5000)
                model1_commision = (25, 25)
                #(base,increment)=(25, 25)


      Model2:
                model2_tier = (0, 5, 15)
                #(base,increment,max_value)=(0, 5, 15)
                model2_commision = (30, 5)
                #(base,increment)=(30, 5)

    """

    #Model 1
                   # +---------+-------------+
                   # |  Tier   |  Commission |
                   # +---------+-------------+
                   # |  $1000  |    $25      |
                   # |  $2000  |    $50      |
                   # |  $3000  |    $75      |
                   # |  $4000  |    $100     |
                   # |  $5000  |    $125     |
                   # +---------+-------------+



    # Model1 class variable shared by all instances of Commision class
    model1_tier = (1000, 1000, 5000)

    #(base,increment,max_value)=(1000, 1000, 5000)
    model1_commision = (25, 25)
    #(base,increment)=(25, 25)


    #Model 2
                    # +----------+-------------+
                    # |  Tier    |  Commission |
                    # +----------+-------------+
                    # | 0 Units  |   $30/unit  |
                    # | 5 Units  |   $35/unit  |
                    # | 10 Units |   $40/unit  |
                    # | 15 Units |   $45/unit  |
                    # +----------+-------------+

    # Model2 class variable shared by all instances of Commision class
    model2_tier = (0, 5, 15)
    #(base,increment,max_value)=(0, 5, 15)
    model2_commision = (30, 5)
    #(base,increment)=(30, 5)



    def Model1_Tier_Commision(self):

        # (base,increment,max_value)=(1000, 1000, 5000)
        m1t1base,m1t1delta,m1t1max=get_commission_amount.model1_tier
         # unpack  model1 tier into base, increment, and max respectively

        #(base,increment)=(25, 25)
        m1c1base,m1c1delta=get_commission_amount.model1_commision
         # unpack  model1 commision into base, and increment respectively

        if self.cost < m1t1base:
            CommisionM1 = 0
        elif m1t1base <= self.cost < (m1t1base + m1t1delta):
            CommisionM1 = m1c1base
        elif (m1t1base + m1t1delta) <= self.cost < (m1t1base + (2*m1t1delta)):
            CommisionM1 = m1c1base + m1c1delta
        elif (m1t1base + (2*m1t1delta)) <= self.cost < (m1t1base + (3*m1t1delta)):
            CommisionM1 = (m1c1base + (2*m1c1delta))
        elif (m1t1base + (3*m1t1delta)) <= self.cost < (m1t1base + (4*m1t1delta)):
            CommisionM1 = (m1c1base + (3*m1c1delta))
        elif self.cost >= m1t1max:
            CommisionM1 = (m1c1base + (4*m1c1delta))
        return CommisionM1 # Model 1 Commision



    def Model2_Tier_Commision(self):
        # (base,increment,max_value)=(0, 5, 15)
        m2t2base,m2t2delta,m2t2max=get_commission_amount.model2_tier
         # unpack  model2 tier into base, increment, and max respectively

        #(base,increment)=(30, 5)
        m2c2base,m2c2delta=get_commission_amount.model2_commision

         # unpack  model2 commision into base, and increment respectively

        if m2t2base <= self.units < (m2t2base + m2t2delta):
            CommisionM2 = m2c2base*self.units
        elif (m2t2base + m2t2delta) <= self.units < (m2t2base + (2*m2t2delta)):
            CommisionM2 = (m2c2base + m2c2delta)*self.units
        elif (m2t2base + (2*m2t2delta)) <= self.units < (m2t2base + (3*m2t2delta)):
            CommisionM2 = (m2c2base + (2*m2c2delta))*self.units
        elif  self.units >= m2t2max:
            CommisionM2 = (m2c2base + (3*m2c2delta))*self.units
        return CommisionM2  # Model 2 Commision

    def __str__(self): # string dunder method to display summary of results
        return """
         {} units sold for a total of ${}.
         Model 1 commision : ${}
         Model 2 commision: ${}
         """.format(self.units,self.cost,self.Model1_Tier_Commision(),self.Model2_Tier_Commision())


#*******************************************************************************
# DRIVER CODE -PROBLEM 4 , INSTANCIATING  get_commission_amount object
#*******************************************************************************

print(get_commission_amount(5,2500))
print(get_commission_amount(1,750))
print(get_commission_amount(16,7500))

 
#*******************************************************************************
# OUTPUT OF CODE FOR PROBLEM 4
#*******************************************************************************

 """
 OUTPUT
         5 units sold for a total of $2500.
         Model 1 commision : $50
         Model 2 commision: $175


         1 units sold for a total of $750.
         Model 1 commision : $0
         Model 2 commision: $30


         16 units sold for a total of $7500.
         Model 1 commision : $125
         Model 2 commision: $720




 """
 ```
 
 
 *****

 

 ## Solution to Problem 5 :  Generic Software Engineering/Computer Science Interest
 > *Problem Statement : Briefly describe a technology that excites you. It can be broad (e.g. a programming language, framework, or toolkit), narrow (e.g. a specific piece of functionality), something you’ve worked with or something that just interests you. What is it, and what do you find exciting about it? :*

 In the past 4-5 years, my exposure to the work of engineering navigated my tasks along the lines of software development and engineering. In spanning across these varied projects, my settled excitement resulted in a resting place of a hybrid combination of hardware-software interface implemented through what is popularly known as the NAND2TETRIS computer science project. It served my interest well as  merger of how the software sits on a hardware platform as well as the conceptual layers of abstracted complexities that begins with fundamental CS logic gates reasoning though key hardware components which then fades effortlessly into the software that sits on the hardware platform without trading off algorithmic thinking and datat structures.
 
The high return on time invested for the qualitative exposure to  a high-lvel software coding and testing technologies is enormous with universal applicability and not necessarily biased towards  prepping for  computer hardware engineering. 
 
 I am of a strong belief that with an integrated hybrind knwlege of software and hardware, software development nuances are captured with core solidified concepts that hleps in ctahing up and getting up to speed in terms of the constantly changing frameworks and technology stack evolutions as the core stays the same.
 
 It is against this background that gravitates me towards full stack developemt with a combo of front end and back-end development. My Journey along NAND2TETRIS has exposed me to computer programming stucture and interpretations(specifically implemented in Python and JavaScript), databases(Mostly SQL- based) and efficeincy prioritization through judicious awareness and use of data structures and algorithms providing a solid understanding of "computer architecture under the hood" by shedding light on infromed decision making reources during coding in the context of time and space complexities. .
 *****
