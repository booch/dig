# Dig

This repo hosts a few experimental updates to Ruby's `dig` method.

Ruby already has a `dig` method on Hash and Array classes, but it has a couple limitations:

1. It works very much like `fetch`, but doesn't accept a default value like `fetch` does.
2. There's no easy way to use it to update a deeply nested structure.
3. If you dig into something that's not a Hash or Array, you'll get an exception.
4. There's no documentation telling you that you can use it for Hashes nested inside Arrays, and vice-versa.


## Usage

~~~ ruby
x = [1, {a: {b: 2}}]

# These are existing behaviors.
x.dig(1, :a, :b)  # => 2
x.dig(1, :a, :c)  # => nil
x.dig(1, :a, :b, :c)  # => TypeError: Integer does not have #dig method

# These are behaviors that we're adding.
x.dig(1, :a, :c){ 3 }  # => 3
x.dig(1, :a, :c){ |index_or_key| "Can't find '#{index_or_key}'" }  # => "Can't find 'c'"
x.dig(1, :a, :b, :c)  # => nil (NOTE: This changes existing behavior, so might not be worth doing.)
x.dig(1, :a, :b, :c){ nil }  # => nil
x.dig!(1, :a){ |a| a[:b] = 4 }  # => [1, {a: {b: 4}}]
x.dig!(1, :a){ |a| a.delete(:b) }  # => [1, {a: {}}]
~~~

## Notes

* `dig!` is roughly equivalent to Elixir's `Kernel.update_in`
* For something similar, but more extensive, take a look at scopes in [xf](https://github.com/baweaver/xf)
