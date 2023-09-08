# How to approach a system design

The following are two common approaches to design a system

## Spiral

Decide your core and build the system around it

Core is use-case specific: Database, Communication

## Incremental Building

1. Start with a day ero architecture
2. See how each component would behave
   1. Under Load
   2. At Scale
3. Identify the bottleneck
4. Re-Architect
5. Repeat

## Points to remember

1. Understand the core property and the access pattern
2. Affinity towards a tech is second (I will use dynamodb, I will use redis: without even thinking of a use-case of it)
3. Build an intuition towards building systems
