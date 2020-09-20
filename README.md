# Diamond Reference Implementations

Every diamond implementation implements the following:

1. **diamondCut function** Standard function used to add/replace/remove functions on a diamond.
2. **Loupe functions** Four standard functions used to show what functions and facets are used by a diamond.

There are three diamond reference implementations that have different benefits and disadvantages in terms of code complexity and gas costs.
Here is a breakdown of the differences between the three implementations. The ratings (high, medium, low) are in relation to each other.

| Implementation | diamondCut<br>complexity | diamondCut<br>gas cost | loupe<br>complexity | loupe<br>gas cost |
| -------------- | ------------------------ | ---------------------- | ------------------- | ----------------- |
| diamond-1      | low                      | medium                 | medium              | high              |
| diamond-2      | high                     | low                    | high                | high              |
| diamond-3      | medium                   | high                   | low                 | low               |

I would use diamond-2 for its low gas costs to add/replace/remove functions. It is possible to choose one implementation and then in the future upgrade the diamond to switch to a different implementation.

diamond-1 and diamond-2 are implemented the same way except that diamond-2 is gas optimized. To understand how diamond-2 is implemented look at diamond-1 first.
