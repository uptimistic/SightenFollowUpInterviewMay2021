

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

## Solution to Problem 1 :  

### Step 0 (Test Driven Development approach) - reverse-engineered inputs for a universally working parsing Python function on a CSV file
Parsing CSV file to generate the supplied input string Python tuple collections object for each row
The following code below, when supplied with a `.csv` file(or any text based file extension) will read each line or row  as a iterable list item

```python
def inputfile_extract(filename):
    with open(filename,'r',encoding='utf-8') as fileObject:
        collected_dataList=[eachLineFromFile.strip('\n') for eachLineFromFile in fileObject ] // the .strip('\n') removes the end of line character after each row 
        # collected_dataList=[eachLineFromFile for eachLineFromFile in fileObject ] // this line of code reproduces given input lists with end of line \n
        return tuple(collected_dataList)
print('input_1=',inputfile_extract("SightenQuestion1A-input1.csv")) # SightenQuestion1A-input1.csv can be downloaded from repository
print('input_2=',inputfile_extract("SightenQuestion1A-input2.csv")) # SightenQuestion1A-input2.csv can be downloaded from repository
```
### Step 1 - Pasring input string Python tuple collections object for individual strings in each row 

* Removing the end of line characters `/n` at the end of each row
  * Option 1 : using String split(<delimeter>) method and specifying the delimeter argument as  `/n` 
  * Option 2 : using String splitlines() method without specifying  `/n`  argument


