# Diamond Reference Implementations

A diamond implementation implements [EIP-2535 Diamond Standard](https://eips.ethereum.org/EIPS/eip-2535). These are examples of diamond implementations. 

Diamonds enable you to build efficient, powerful, flexible, modular contract systems.

Every diamond implementation implements the following:

1. **diamondCut function** Standard function used to add/replace/remove functions on a diamond.
2. **Loupe functions** Four standard functions used to show what functions and facets a diamond has.

There are three diamond reference implementations that have different benefits and disadvantages in terms of code complexity and gas costs.
Here is a breakdown of the differences between the three implementations. The ratings (high, medium, low) are in relation to each other.

| Implementation | diamondCut<br>complexity | diamondCut<br>gas cost | loupe<br>complexity | loupe<br>gas cost |
| -------------- | ------------------------ | ---------------------- | ------------------- | ----------------- |
| diamond-1      | low                      | medium                 | medium              | high              |
| diamond-2      | high                     | low                    | high                | high              |
| diamond-3      | medium                   | high                   | low                 | low               |

It is possible to choose one implementation and then in the future upgrade the diamond to switch to a different implementation.

All three implementations pass the same tests.

diamond-1 and diamond-2 use less gas to add/replace/remove functions.

diamond-3 uses less gas to call the diamond loupe functions.

diamond-1 and diamond-2 are implemented the same way except that diamond-2 is gas optimized. To understand how diamond-2 is implemented look at diamond-1 first.

### Diamond Repositories

Links to diamond reference implementation repositories:

- [diamond-1](https://github.com/mudgen/diamond-1)
- [diamond-2](https://github.com/mudgen/diamond-2)
- [diamond-3](https://github.com/mudgen/diamond-3)

#### How diamond-1 is implemented

1. Has an `bytes4[] selectors` array that stores the function selectors of a diamond.
2. Has a `mapping(bytes4 => FacetAddressAndSelectorPosition) facetAddressAndSelectorPosition` mapping that maps each function selector to its facet address and its position in the `selectors` array.

It's `facets`, `facetFunctionSelectors`, `facetAddresses` loupe functions should not be called in on-chain transactions because their gas cost is too high. These functions should be called by off-chain software.

The `facetAddress` loupe function has a low fixed gas cost in all implementations and can be called in on-chain transactions.

#### How diamond-2 is implemented

diamond-2 is implemented the same way as diamond-1 except that the `selectors` array is implemented as a mapping of 32-byte storage slots and uses various gas-optimizations to reduce storage reads and writes.

This implementation is gas-optimized for adding/replacing/removing functions on a diamond.

It's `facets`, `facetFunctionSelectors`, `facetAddresses` loupe functions should not be called in on-chain transactions because their gas cost is too high. These functions should be called by off-chain software.

The `facetAddress` loupe function has a low gas cost in all implementations and can be called in on-chain transactions.

#### How diamond-3 is implemented

1. Has an `address[] facetAddresses` array that stores the facet addresses of a diamond.

1. Has a `mapping(address => FacetFunctionSelectors) facetFunctionSelectors` mapping that maps each facet address to its array of function selectors and its position in the `facetAddresses` array.

1. Has a `mapping(bytes4 => FacetAddressAndPosition) selectorToFacetAndPosition` mapping that maps each function selector to its facet address and its position in the `facetFunctionSelectors[facetAddress].functionSelectors` array.

The standard loupe functions `facets`, `facetFunctionSelectors`, `facetAddresses` **CAN** be called in on-chain transactions. Note that if a diamond has a great many functions and/or facets these functions may still cause an out-of-gas error.

The `facetAddress` loupe function has a low fixed gas cost in all implementations and can be called in on-chain transactions.
