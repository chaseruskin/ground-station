# Motion 0x04
Chase Ruskin  
2022/02/14

## Handling Dependencies

There is no debate the most vital role a package manager plays is to automatically and efficiently manage a project's dependencies. Effective code reuse is the key to growing and maintaining a well-organized codebase. 

Dependency solving is a difficult problem. It has been identified as an NP-complete problem [1] for expressive dependencies. However, we take an approach to modify the problem's definition so that we avoid the NP-complete complexity class. The following approach is adapted from orbit's prototype, and go mod's MVS algorithm.

## Version Bounds

To ease difficulty in our dependency solving, and to allow for more flexible builds, orbit supports multiple versions within the same dependency tree.

### References

[1] [Dependency Solving Is Still Hard, but We Are Getting Better at It](https://arxiv.org/pdf/2011.07851.pdf)

[2] [Minimal Version Selection](https://research.swtch.com/vgo-mvs)