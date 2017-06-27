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


- Array type syntax [int] makes it hard to find uses of array as they are confused with type parameter Foo[int]

- Not being able to separate Array type into Read and Write traits makes it harder to share them safely.

- One often needs to create a new class to contain the results to be passed back from an actor. The actor then copies (parts of) its working data structure into a new instance of that class. It needs to be a new class because the mode of the working class and the one passed must be different

- Linear is painful to work with. Eventually, we tried to avoid it at all costs.

- I would like to reify a parameter in a function by adding more restrictions to it. For instance,
  I have a method that sends y : T to a function myFun. This function reifies the modes
  adding the constrain that y : T is now y : T + S. This can happen with compatible modes

  class X : T
    def send(y: T): unit
      myFun(y)
    end
  end

  local trait S
  end

  fun myFun(y: T + S): unit
    ...
  end

- It would be great if traits can be without mode and I add the mode as I go by.
  I guess there is some support for it... I believe that read modes need to be
  explictily declared. same for linear ones.

- The following example is a feature or a bug:

  local class C : R
    var x : String
    def foo() : unit
  println("foo")
  end
  end

  read trait R
    require val x : String
  end

  active class A
    val c : C
    def init(c: C): unit
      this.c = c
    end
  end

  active class Main
    var i: int

    def main(args : [String]) : unit
      val c = new C()
      this.i
    end
  end
