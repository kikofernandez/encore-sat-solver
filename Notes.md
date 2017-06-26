- programming with linear references can be a pain because one is forced to make fields and variables holding them to be Maybe type
- This would compile

    fun unjust[linear t](x : Maybe[Link[t]]) : Link[t]
      match x with
        case Just(ice) => ice
        case Nothing => abort("Cannot unjust Nothing.")
      end
    end

  But this wouldn't    
  
    fun unjust[linear t](x : Maybe[t]) : t
      match x with
        case Just(ice) => ice
        case Nothing => abort("Cannot unjust Nothing.")
      end
    end
    
- Transitioning from non-Kappa to Kappa by just using the compiler errors and trying to fix them without understanding the program was an exercise in futility. Basically, I just pushed the error from one place to another

- The approach Kiko and Dave took was too look at the data passed between actors and to see what was done with it before and after it was passed and to look at whether it was aliased (and where) and to see whether the methods used were limited (was there shared mutable state, was there linearity)

- Inspection of smaller classes (and their usage) often lead to identifying whether they were immutable, local, or subordinate.
