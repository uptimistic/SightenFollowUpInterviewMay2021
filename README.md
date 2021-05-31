

# SightenFollowUpInterviewMay2021

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

## Solution to Problem 1 :  Reading CSV file input string list object 


Parsing CSV input string list object
```python
def inputfile_extract(filename):
    with open(filename,'r',encoding='utf-8') as fileObject:
        collected_dataList=[eachLineFromFile.strip('\n') for eachLineFromFile in fileObject ]
        return tuple(collected_dataList)
print('input_1=',inputfile_extract("SightenQuestion1A-input1.csv"))
print('input_2=',inputfile_extract("SightenQuestion1A-input2.csv"))
```

