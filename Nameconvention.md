# Julia name conventions 
(1) *Modules* and type names use **capitalization** and **camel** case: `module SparseArrays` , `struct UnitRange`.    
(2) *Functions* are **lowercase** ( `maximum`, `convert` ) and, when readable, with multiple words squashed together (`isequal`, `haskey`). When necessary, use **underscores** as word separators. Underscores are also used to indicate a combination of concepts (`remotecall_fetch` as a more efficient implementation of `remotecall(...)` ) or as modifiers.fetch.  
If a function name requires multiple words, consider whether it might represent more than one concept and might be better split into pieces.  
(3) functions mutating at least one of their arguments end in `!`.   
(4) conciseness is valued, but **avoid abbreviation** ( `indexin` rather than `indxin` ) as it becomes difficult to remember whether and how particular words are abbreviated.