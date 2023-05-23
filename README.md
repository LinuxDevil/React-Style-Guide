
# Code Principles

### Hi All ❤️

There are a few of these principles that will be universally accepted, but not all of them must be completely adhered to. These are merely guidelines,  
but ones that have been formalized over a long period of time.

One more thing: even if you have years of experience with them, knowing them won't make you a better software developer right away.  
Like wet clay being molded into its final form, every line of code begins as a first draft.  
When we review it with our colleagues/friends, we finally chisel out the flaws. Don't be hard on yourself if your early drafts need work. Instead, abuse the code!

#### Let's code our wisdom like a melodic poetry ❤️


### To keep in mind:

-   Use type annotations to explicitly declare the types of variables, function arguments, and return values. This makes it clear to other developers (and to the TypeScript compiler) what the expected types are, and can help prevent type errors.
    
-   Use interfaces to define the shape of complex types, such as objects with multiple properties or arrays with specific elements. This can make your code more self-documenting and easier to understand.
    
-   Use classes and encapsulation to organize your code into logical units and protect the internal state of your objects from being modified by external code.
    
-   Follow the SOLID principles of object-oriented design to create code that is easy to understand, maintain, and extend.

#### [DRY (Don't Repeat Yourself)](./princibles/DRY.md)


#### [ KISS (Keep it simple, stupid)](/princibles/KISS.md)


#### [YAGNI (You ain't gonna need it)](./princibles/YAGNI.md)

----

## Variables
### [Use meaningful variable names](/variables/good_naming.md)
### [Use pronounceable variable names](/variables/speakable_names.md)
### [Use the same vocabulary for the same type of variable](/variables/vocab_for_type.md)
### [Use searchable names](/variables/searchable_names.md)
### [Use explanatory variables](/variables/explanitory_vars.md)
### [Avoid Mental Mapping](/variables/no_mental_mapping.md)
### [Use default arguments instead of short circuiting or conditionals](/variables/default_args_no_short_circuit.md)
### [Use enum to document the intent](/variables/enums.md)

----

## Functions

### [Function callers and callees should be close](/functions/calees_should_be_close.md)
### [Function arguments (2 or fewer ideally)](./functions/arguments.md)
### [Functions should do one thing](./functions/one_job.md)
### [Function names should say what they do](/functions/meaningful_names.md)
### [Functions should only be one level of abstraction](/functions/one_abstraction_lvl.md)
### [Remove duplicate code](/functions/no_dublication.md)
### [Set default objects with Object.assign or destructuring](/functions/default_objects.md)
### [Don't use flags as function parameters](/functions/flags_as_parameters.md)
### [Avoid Side Effects (part 1)](/functions/no_side_effects_usage_1.md)
### [Avoid Side Effects (part 2)](/functions/no_side_effect_usage_2.md)
### [Don't write to global functions](/functions/no_globals.md)
### [Favor functional programming over imperative programming](/functions/functional_over_imerative.md)
### [Encapsulate conditionals](/functions/encapsulate_conditions.md)
### [Avoid negative conditionals](/functions/avoid_negative_conditions.md)
### [Avoid conditionals](/functions/avoid_conditionals.md)
### [Avoid type checking](/functions/avoid_type_checking.md)
### [Don't over-optimize](/functions/don't_over_optimize.md)
### [Remove dead code](/functions/dead_code.md)
### [Use iterators and generators](/functions/iterators_and_generators.md)

----

## Objects and Data Structures

### [Use getters and setters](/objects/getters_and_setters.md)
### [Make objects have private/protected members](/objects/access_modifiers.md)
### [Prefer immutability](/objects/close_for_modification.md)
### [type vs. interface](/objects/being_loosly_coupled.md)

---

## Classes

### [Classes should be small](/classes/classes_should_be_thin_and_do_one_thing.md)
### [High cohesion and low coupling](/classes/cohesion.md)
### [Prefer composition over inheritance](/classes/composition_over_inheritance.md)
### [Use method chaining](/classes/method_chaining.md)

----

## SOLID

### [Single Responsibility Principle (SRP)](/SOLID/SRP.md)
### [Open/Closed Principle (OCP)](/SOLID/OCP.md)
### [Liskov Substitution Principle (LSP)](/SOLID/LSP.md)
### [Interface Segregation Principle (ISP)](/SOLID/ISP.md)
### [Dependency Inversion Principle (DIP)](/SOLID/DIP.md)

----

## Testing

Testing is more important than shipping. If you have no tests or an inadequate amount, then every time you ship code you won't be sure that you didn't break anything.  
Deciding on what constitutes an adequate amount is up to your team, but having 100% coverage (all statements and branches)  
is how you achieve very high confidence and developer peace of mind. This means that in addition to having a great testing framework, you also need to use a good [coverage tool](https://github.com/gotwarlost/istanbul).

There's no excuse to not write tests. There are [plenty of good JS test frameworks](http://jstherightway.org/#testing-tools) with typings support for TypeScript, so find one that your team prefers. When you find one that works for your team, then aim to always write tests for every new feature/module you introduce. If your preferred method is Test Driven Development (TDD), that is great, but the main point is to just make sure you are reaching your coverage goals before launching any feature, or refactoring an existing one.

### The three laws of TDD

1.  You are not allowed to write any production code unless it is to make a failing unit test pass.
    
2.  You are not allowed to write any more of a unit test than is sufficient to fail, and; compilation failures are failures.
    
3.  You are not allowed to write any more production code than is sufficient to pass the one failing unit test.
    

### [F.I.R.S.T. rules](/testing/first_rule.md)
### [Single concept per test](/testing/single_concept_per_Test.md)
### [The name of the test should reveal its intention](/testing/naming.md)

---
## Concurrency

### [Prefer promises vs callbacks](./Concurrency/promise_over_callbacks.md)
### [Async/Await are even cleaner than Promises](/Concurrency/async_await.md)

----

## Error Handling

Thrown errors are a good thing! They mean the runtime has successfully identified when something in your program has gone wrong and it's letting you know by stopping function

execution on the current stack, killing the process (in Node), and notifying you in the console with a stack trace.

### [Always use Error for throwing or rejecting](/errors/thorw_err.md)
### [Don't ignore caught errors](/errors/be_kind_to_caught_errors.md)
### [Don't ignore rejected promises](/errors/don't_ignore_rejection.md)
### [Migrating from TSLint to ESLint](/errors/from_tslint_to_eslint.md)

---
## Code Organization

### [Use consistent capitalization](/general/consistant_capitalization.md)

### [Organize imports](/general/import_organization.md)
### [Use typescript aliases](/general/typescript_organization.md)

-----

## Comments

The use of a comments is an indication of failure to express without them. Code should be the only source of truth.

> Don’t comment bad code—rewrite it.
> 
> — _Brian W. Kernighan and P. J. Plaugher_

### [Prefer self explanatory code instead of comments](/comments/clear_code_instead_of_comments.md)
### [Don't leave commented out code in your codebase](/comments/abandoned_comments_are_sad.md)
### [Don't have journal comments](/comments/journal_in_a_diary_not_in_code.md)
### [Avoid positional markers](/comments/we're_not_playing_plant_flags.md)
### [TODO comments](/comments/TODO.md)
