# Bloom Filter

Bloom filters are approximate data-structures hat says with 100% certainity that an element does not belong to a set

- False positivity rate increases with the increase in the size of the filter
- It was created by a researcher called bloom, so it’s named after him
- Filter = Bit Array

## Practical application of Bloom Filter

- We do not have to re-implement bloom filter
- There are libraries in every single language
- Redis has it as ne of it’s core features (Now-a-days people mostly go with this)
- Use it whenever
  - You need to insert but not remove the data
  - You need a NO with 100% certainity
  - Having false-positivity is okay

**Eg:** Medium recommendation, feed generation, web crawlers, linkedin feed

## Eg: An 8-bit array

![Bloom filter demonstration](https://bharath-lakshman-kumar.s3.ap-south-1.amazonaws.com/Bloom%20Filter/bloom-filter.png)

Let’s say that we store each of these (apple, ball, cat) in an Array (Filter)

- Respective item will be stored in the array based on the hash function
- So we are only utilizing 1B (One bit) in the array (It’s very space efficient)

What about finding if an elephant is stored in the array?

- Let’s get the hash → `Elephant → f(elephant) % 8 = 5`
- So at the fifth index in the array, we don’t find any value, so there is no elephant in the list

<aside>
💡 Thus, when bloom filter says there is no item present → We can be sure of that

But, when bloom filter says the item is present → We still need to check

</aside>
