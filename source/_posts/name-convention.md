---
title: Julia Name Convention
date: 2021-12-25 17:23:47
tags: julia
---
# Name Conventions 

1. *Modules* and *type* names use **capitalization** and **camel** case: `module SparseArrays` , `struct UnitRange`.    

2. *Functions* are **lowercase** ( `maximum`, `convert` ) and, when readable, with multiple words squashed together (`isequal`, `haskey`).
    - When necessary, use **underscores** as word separators. 
    - Underscores are also used to indicate a combination of concepts (`remotecall_fetch` as a more efficient implementation of 
    ``fetch(remotecall(...)))`` or as modifiers.
    - If a function name requires multiple words, consider whether it might represent more than one concept and might be better split into pieces.  

3. Functions that mutate at least one of their arguments end in `!`.   

4. Conciseness is valued, but **avoid abbreviation** ( `indexin` rather than `indxin` ) as it becomes difficult to remember whether and how particular words are abbreviated.

# Write functions with argument ordering similar to Julia Base

As a general rule, the Base library uses the following order of arguments to functions, as applicable:

1. Function argument. Putting a function argument first permits the use of `do` blocks for passing multiline anonymous functions.

2. I/O stream. Specifying the IO object first permits passing the function to functions such as `sprint`, e.g. ``sprint(show, x)``.

3. Input being mutated. For example, in ``fill!(x, v)``, `x` is the object being mutated and it appears before the value to be inserted into `x`.

4. Type. Passing a type typically means that the output will have the given type. In ``parse(Int, "1")``, the type comes before the string to parse. There are many such examples where the type appears first, but it's useful to note that in ``read(io, String)``, the IO argument appears before the type, which is in keeping with the order outlined here.

5. Input not being mutated. In fill!(x, v), v is not being mutated and it comes after x.

6. Key. For associative collections, this is the key of the key-value pair(s). For other indexed collections, this is the index.

7. Value. For associative collections, this is the value of the key-value pair(s). In cases like ``fill!(x, v)``, this is `v`.

8. Everything else. Any other arguments.

9. Varargs. This refers to arguments that can be listed indefinitely at the end of a function call. For example, in ``Matrix{T}(undef, dims)``, the dimensions can be given as a Tuple, e.g. ``Matrix{T}(undef, (1,2))``, or as Varargs, e.g. ``Matrix{T}(undef, 1, 2)``.

10. Keyword arguments. In Julia keyword arguments have to come last anyway in function definitions; they're listed here for the sake of completeness.

The vast majority of functions will not take every kind of argument listed above; the numbers merely denote the precedence that should be used for any applicable arguments to a function.

There are of course a few exceptions. For example, in `convert`, the type should always come first. In `setindex!`, the value comes before the indices so that the indices can be provided as varargs.

When designing APIs, adhering to this general order as much as possible is likely to give users of your functions a more consistent experience.