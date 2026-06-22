---
name: writing-or-reviewing-csharp-code
description: This should be used when C# code is generated or reviewed.
---

## Main Instructions

In addition to your knowledge of C#, follow the given rules to generate/review C# code:
- **Use** `var` to declare a local variable, unless it has a primitive numeric type, or there is a comment to explain why.
- **Use** `readonly` for constants, whose name should be all capital.
- **Use** `as` for type conversion and use `is` for type check.
  + Cast expression **can be used only if** a user-defined implicit conversion operator is required (must be given in a comment).
  + If you are converting an object to a primitive type, **use** nullable type.
- **Use** interpolated string first, instead of `string.Format`.
  + **Never use** string concatenation for SQL.
  + **You may use** `StringBuilder` for more complex logic that cannot fit in 2~3 lines.
- **Never** add an overloading version for `string` type if there is a version for `FormattableString`.
- **Use** `nameof` if there is any string for symbols.
- **Never** throw an error in delegates.
  + A delegate's type **should never** has a return type that is not `void`, unless it is used by `GetInvocationList` (and there should be comments to explain it)
- **Use** null condition operator (`?.`) instead of checking if the callee is null when possible.
- **You should avoid** unnecessary boxing/unboxing.
  + **Use** `ToString` to convert a variable with primitive type before passing it to interpolated string.
  + **Use** a generic collection, instead of those that take `System.Object` (e.g., .Net 1.x collections)
- **Do not use** `new` for a non-private member.
- **You should** always give a default value to a member when possible.
  + **Do not** do it if it is initialized by constructors.
- **Use** laziness (see Auxiliary Instructions - Laziness) for static members if the initialization is expensive.
- **You should avoid** using helper functions to reduce duplications in constructors.
- **You should** reduce object allocations.
  + **Use** a member, instead of local variable, if the same object can be allocated more than once.
  + **Use** laziness (see Auxiliary Instructions - Laziness) if the initialization is expensive.

At the end, execute the following script if there exists `.editorconfig` file:
```bash
dotnet format
```

## Auxiliary Instructions
### Laziness
By expensive, we mean operations that take more than 10 MB memory
and/or 0.1 seconds.

Use `Lazy<T>` first when possible.
Create a lazy getter when:
- `Lazy<T>` cannot be used for some reasons, or
- Initialization cannot fit in less than 5 lines, or
- The value may be assigned to different values more than once
