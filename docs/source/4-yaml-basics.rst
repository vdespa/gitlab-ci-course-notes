***********
YAML Basics
***********


Overview
--------

- we will discuss the most relevant YAML features that are necessary when using Gitlab CI 

Understanding YAML
------------------

- in this lecture we go over the most important characteristics of the language
- if we look at yaml in a very simple way, we can say that YAML stores key - value pairs

.. code-block:: yaml

    name: John

- so this is a variable name with John as a string value
- we can define numbers, booleans, lists, objects

- comments: # (number sign)

- YAML is very similar to another format called JSON (JavaScript Object Notation)
- try in on your own with this YAML to JSON convertor: https://codebeautify.org/yaml-to-json-xml-csv
- take the quiz after this lecture to check you understanding of YAML

person: 
    name: John
    age: 23
    food:
    - pizza
    - donuts
    - coke
    friend:
        name: Joe
        age: 30
        food: null
    
stuff: [car, my car,bike,stereo]



Using anchors
-------------

- define an anchor with &name
- use anchor with *name (asterix)
- anchor a value
- anchor a key-value pairs

