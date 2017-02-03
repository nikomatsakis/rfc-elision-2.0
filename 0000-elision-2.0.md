- Feature Name: (fill me in with a unique ident, my_awesome_feature)
- Start Date: (fill me in with today's date, YYYY-MM-DD)
- RFC PR: (leave this empty)
- Rust Issue: (leave this empty)

# Summary
[summary]: #summary

A set of related changes that aim to lighten the annotation burden
associated with structs that contain references, as well as tempering
some confusing cases that arise with the current elision rules:

- Inferring the `T: 'a` annotations that are currently required on structs.
- Permit eliding lifetimes in structs if the struct has a single
  lifetime parameter.
- Introducing a "single tick" notation `Foo<'>` that can be used to
  indicate the presence of elided lifetimes in various settings:
  - **Struct declarations:** such as `struct Iter<', T> { vec: &[T], index: usize }`.
  - **Elided lifetime arguments in function signatures:**
      - Rather than writing `fn foo(&self) -> Ref<i32>`, one might write
        `fn foo(&self) -> Ref<', i32>`, which makes it clear that `self` will remain
        borrowed so long as the return value is in used.
      - Similarly, rather than writing `fn foo(&self, r: Ref<i32>)`,
        one might write `fn foo(&self, r: Ref<', i32>)`, which makes
        it clear `Ref` carries lifetime data.
      - In these contexts, the `'` can "stand-in" for any number of lifetime
        parameters.
- Deprecate fully eliding the lifetime parameters on structs (e.g.,
  `Ref<i32>`) in favor of `Ref<', i32>`.
      - FEEDBACK REQUESTED: Everywhere? Or just in fn arguments?

# Motivation
[motivation]: #motivation

Why are we doing this? What use cases does it support? What is the expected outcome?

# Detailed design
[design]: #detailed-design

This is the bulk of the RFC. Explain the design in enough detail for somebody familiar
with the language to understand, and for somebody familiar with the compiler to implement.
This should get into specifics and corner-cases, and include examples of how the feature is used.

# How We Teach This
[how-we-teach-this]: #how-we-teach-this

What names and terminology work best for these concepts and why? 
How is this idea best presented—as a continuation of existing Rust patterns, or as a wholly new one?

Would the acceptance of this proposal change how Rust is taught to new users at any level? 
How should this feature be introduced and taught to existing Rust users?

What additions or changes to the Rust Reference, _The Rust Programming Language_, and/or _Rust by Example_ does it entail?

# Drawbacks
[drawbacks]: #drawbacks

Why should we *not* do this?

# Alternatives
[alternatives]: #alternatives

What other designs have been considered? What is the impact of not doing this?

# Unresolved questions
[unresolved]: #unresolved-questions

What parts of the design are still TBD?
