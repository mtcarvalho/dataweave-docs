# Dataweave Language Guide Notes

- DataWeave is a functional language used in Mule applications to perform data transformations.
- DW enables you to take advantage of the benefits of functional programming: pure functions, immutable variables, function signatures, concise and clear code, call-by-need strategy (lazy evaluation)

## DW Script

header and body, separated by three dashes ( `---`)
- header: contains language directives, including 
	- the output format of the transformation
	- variables and function declarations
	- [importing](#import-function-modules) of prebuilt DW modules with functions available to use in scripts
- body: contains the expression to generate a result (data mapping or transformation)
  example:
```
%dw 2.0   
output application/json   
---   
payload  
```
	
## Data Types

- simple (e.g., String, Boolean, Number)
- composite and complex (e.g., Array, Object and Any)

## Data Selectors

enable to access fields within a data structure so you can retrieve data from payload or variables

- Single-value selector: e.g., `payload.userName` retrieve the single value `userName`, which is a key of `payload`

## Functions


- **every function returns a value** after it finishes processing, and this value can then be used as input for a different function.
- each DW script that you write in **any** component works as an **independent** program, meaning that all function definitions and calls are unique to the script defined in the component.
-  function definition vs. function call
	- in DW, you **define** functions in the header of the script, creating a blueprint for the process that defines the operations that are executed upon the function call.
	- when you **call** a function from a DW expression, you are instructing DW to run all the operations defined in that function at that exact point of the script’s execution.
	- DW supports both **prefix** notation `(function (param1, param2))` and **infix** notation `(param1 function param2)` for function calls. Note that you can use infix notation **only** with functions that accept only two parameters.
	
## Data Operators:

- mathematical: `+, -, /, *`
- relational: `<, >, <=, >=, ==, ~=`
- logical: `not, !, and, or`
- append `(<<, +)`, prepend `(>>)`, and remove `(-)`, which enable you to manage data in an array.
- scope and flow control operators `(do, using, if, else)` that enable you to create a scope where new variables and functions can be declared, or to execute an expression based on a condition.
- update `(update)`, which enables you to update specific fields of a data structure.

## Import Function Modules

- by default, DW scripts import the `dw::Core` function module. 
- to use functions from different modules, import the modules to your script by adding the `import` directive.
	
## Arrays

- to declare an array, use brackets `([ and ])` to enclose a comma-separated list of elements, for example: `[1,2,3]`.
- arrays also support the application of conditional logic to define conditional elements (`[(value1) if condition]`).
  example:
```
%dw 2.0
output application/json
---
[(1) if true, (2) if false, (3) if payload != null]
``` 