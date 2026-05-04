## Mutation Testing
(i) A mutation is an altered section of code altered to produce a bug for testing purposes
(ii) A mutant is the variant of the core program that has been generated with a mutation
(iii) A survivor is a mutant that was not caught by the testing process
(iv) Effectiveness is the percentage of mutants that were killed during testing
(v) Efficiency is a measure of the resources and time required to achieve a specific level of effectiveness

**Negate Conditionals:** Searches your code for conditional statements (if, while, for) and flips their logical operators to their exact opposites
- `==` becomes `!=`
- `<` becomes `>=`
- `>` becomes `<=`
- `<=` becomes `>`
- `true` becomes `false`

**Replace Return Value with Default Value:** Instead of returning what your code actually computed, it returns meaningless values.
- Numbers usually return `0`.
- Booleans usually return `false`.
- Strings usually return `""` (empty string).
- Objects usually return `null` or `undefined`

**Replace relational operator with boundary equivalent:** 
- `<` is replaced with `<=`
- `>` is replaced with `>=`
- `<=` is replaced with `<`
- `>=` is replaced with `>`