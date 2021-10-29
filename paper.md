# Constraining data members

## 1. Abstract
```
template<class T> struct value_holder {
    T value requires (not is_void_v<T>);
};
```

## 2. Motivation
We can constrain class template member functions but not data members.  This is unfair:
```
template<int I>
struct S {
    int f() requires (I == 42) { return 5; }; // ok

    int i = 5 requires (I == 42); // not allowed
    inline static int j = 5 requires (I == 42); // not allowed
    template<int = 0> requires (I == 42) inline static int k = 5; // ok

    using T = int requires (I == 42); // not allowed
    template<int = 0> requires (I == 42) using U = int; // ok
};
```

## 3. Proposal
Allow constraints on non-static data members, on non-template static member variables and on non-template member type aliases.

### 3.1. Hiding
A constrained data member or type alias hides names in a base class, etc. even when constrained out; they may be made visible via a using-declaration.

### 3.2. Well-formedness
If a data member or type alias is constrained out, its type-id does not need to name a type in the program, but it must parse.

