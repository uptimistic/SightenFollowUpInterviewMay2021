
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
#### 3-5. Retrieve all Projects that have a Quote with an install_cost greater than 15,000- Django
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
 

## Solution to Problem 4 :  Data Analysis, Regression/Statistics, Algorithmic thinking
 
  > *Problem Statement : Write the function get_commission_amount that returns the commission amount based on a given commission_model and sale :*

 
 Whiteboard Mindmap Solution to Problem before Coding 
 <p align="center">
  <img src="https://github.com/uptimistic/SightenFollowUpInterviewMay2021/blob/main/Whiteboard.MindMap.Problem4.jpg">
</p>
 
 Concept Complexity Tree Representation before coding 
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
 
 model 1
y= mx+c
y = Commission(amount per $ sold) 
x= Tier (# of units)
m= model slope =(125-25)/(5000-1000)=0.025
c= y -intercept =25 
Commission(cost )= 25+0.025*Tier (unit cost)
 
model 2
y= mx+c
y = Commission(amount per $ sold) 
x= Tier (# of units)
m= model slope =(45-30)/(15-0)=1
c= y -intercept =30 
Commission(amount per $ sold)= 30+Tier (# of units)
 Delta Tier = 5 units 
 Delta Commision = $5

 *****

 

 ## Solution to Problem 5 :  Generic Software Engineering/Computer Science Interest
 > *Problem Statement : Briefly describe a technology that excites you. It can be broad (e.g. a programming language, framework, or toolkit), narrow (e.g. a specific piece of functionality), something you’ve worked with or something that just interests you. What is it, and what do you find exciting about it? :*

 In the past 4-5 years, my exposure to the work of engineering navigated my tasks along the lines of software development and engineering. In spanning across these varied projects, my settled excitement resulted in a resting place of a hybrid combination hardware-software interface implemented through what is popularly known as the NAND2TETRIS. It is a powerful merger of how the software sits on a hardware platform and the concemtual layers of abstracted complexities that begins with fundamental CS logic gates reasoning though key hardware components which then fades effortlessly into the software that sits on the hardware platform.
 
The high return on time invested for the qualitative exposure to to a high lvel software coding and testing technologies is enormous and not necessarily for  prepping for hardware computer engineering. 
 
 I am of a trong belief that with an integrated hybrind knwleged of software and hardware, software development nuances are captured with core solidified concepts that hleps in ctahing up and getting up to speed in terms of the constantly changing frameworks and technology stack evolutions as the core stays the same.
 
 It is againts this interest and background that gravitates me towards full stack developemt with a combo of front end and back-end development. My Journey along NAND2TETRIS has exposed me to computer programming stucture and interpretations(specifically Python and JavaScript), databases(Mostly SQL- based) and efficeincy prioritization through judicious awareness and use of data structures and algorithms.
 *****
