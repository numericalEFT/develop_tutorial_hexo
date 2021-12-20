---
title:  "A benchmark on Julia getproperty() function for user defined struct"
date: 2021-12-20 15:45:45
tags: julia
---

# Table of Contents

1.  [A benchmark on Julia getproperty() function for user defined struct](#org9d896eb)
    1.  [Julia getproperty()](#org5236e65)
    2.  [Benchmark](#org4564be8)



<a id="org9d896eb"></a>

# A benchmark on Julia getproperty() function for user defined struct


<a id="org5236e65"></a>

## Julia getproperty()

It is allowed in Julia to override getproperty() function to
provide "properties" for a user defined struct such that
the "properties" acts as if it's a field of the struct but
it's derived from actual fields.

```julia
    struct MyType
        a::Float64
        b::Float64
    end
    
    function Base.getproperty(obj::MyType, sym::Symbol)
        if sym === :mult
            return obj.a * obj.b
        elseif sym === :div
            return obj.a / obj.b
        else # fallback to getfield
            return getfield(obj, sym)
        end
    end
    
    mt = MyType(2.0, 3.0)
    println(mt.a, ",", mt.b, ",", mt.mult, ",",mt.div)
```

```
    2.0,3.0,6.0,0.6666666666666666
```

The properties are not stored in the memory, but derived
every time getproperty() is called, either explicitly
or implicitly via dot operator as in "mt.mult".
The problem is the efficiency of this operation.
If this operation is much less efficient than getfield()
which deals with dot operator to get value of an actual field,
we should add a field to store the value instead of using
getproperty().


<a id="org4564be8"></a>

## Benchmark

We benchmark common operations that could be used in getproperty
to check the efficiency.

```julia
    using BenchmarkTools
    struct MyType
        a::Float64
        b::Float64
    end
    
    function slowfunc(a, b)
        result = 0.0
        for i in 1:100
            result += a
            result += b
            end
        return result
    end
    
    function Base.getproperty(obj::MyType, sym::Symbol)
        if sym === :mult
            return obj.a * obj.b
        elseif sym === :sqrta
            return sqrt(obj.a)
        elseif sym === :repeatedadd # calls dot operator multiple time, could lower the efficiency
            return obj.a + obj.b + obj.a + obj.b + obj.a + obj.b + obj.a + obj.b + obj.a + obj.b
        elseif sym === :mult_add_sqrta
            return obj.mult + obj.sqrta
        elseif sym === :slowfab
            return slowfunc(obj.a, obj.b)
        else # fallback to getfield
            return getfield(obj, sym)
        end
    end
    
    mt = MyType(2.0, 3.0)
    
    println(@benchmark mt.a) # dot operator for a, calls getproperty first and then fallback to getfield
    println(@benchmark getfield(mt, :a) ) # getfield for a directly
    println(@benchmark mt.mult) # a*b
    println(@benchmark mt.sqrta) # sqrt(a) doesn't affect efficiency much
    println(@benchmark mt.repeatedadd) # float operations costs very little
    println(@benchmark mt.mult_add_sqrta) # slower if the calculation is more complex, but not much
    println(@benchmark mt.slowfab) # slow if the calculation is really slow
    println(@benchmark slowfunc(2.0, 3.0)) 
```

```
    Trial(45.045 ns)
    Trial(25.183 ns)
    Trial(17.354 ns)
    Trial(18.239 ns)
    Trial(19.572 ns)
    Trial(20.984 ns)
    Trial(205.156 ns)
    Trial(147.363 ns)
```

The conclusions are the following:

-   For most of simple calculations getproperty() doesn't lower the efficiency.
-   Somehow simple getproperty() runs faster than dot operator for fields.
    Guess the reason could be that dot operator for fields are inlined within getproperty() function,
    but for external dot operator it's triggered via getproperty() -> getfield().
-   For calculations much smaller the efficiency is of course affected.

