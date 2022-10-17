# Rust update from 1.63 to 1.64 makes the following code not compile

This repo has been created to create a minimal example to report a bug
in retrocompatibility in the rust compiler. The following code builds and works
in 1.63 but do not build in 1.64 due to a lifetime problem I do not understand.


When updating to 1.64, error is:

```
error: lifetime may not live long enough
  --> src/main.rs:15:9
   |
11 |   impl<'s> VecNumber<'s> {
   |        -- lifetime `'s` defined here
12 |       pub fn vec_number_iterable_per_item_in_auxiliary_object(
13 |           &self,
   |           - let's call the lifetime of this reference `'1`
14 |       ) -> impl Iterator<Item = (&'s usize, impl Iterator<Item = &Number<'s>> + Clone)> {
15 | /         self.auxiliary_object.iter().map(move |n| {
16 | |             let iter_number = self.vec_number.iter();
17 | |             (n, iter_number)
18 | |         })
   | |__________^ associated function was supposed to return data with lifetime `'s` but it is returning data with lifetime `'1`
```

An issue on this topic has been opened here: https://github.com/rust-lang/rust/issues/103141
