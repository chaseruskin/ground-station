# Motion 0x04
Chase Ruskin  
2022/02/14

## Handling Dependencies

There is no debate the most vital role a package manager plays is to automatically and efficiently manage a project's dependencies. Effective code reuse is the key to growing and maintaining a well-organized codebase. 

Dependency solving is a difficult problem. It has been identified as an NP-complete problem [1] for expressive dependencies. However, we take an approach to modify the problem's definition so that we avoid the NP-complete complexity class. The following approach is adapted from orbit's prototype, and go mod's MVS algorithm.

## Multiple version support

To ease difficulty in our dependency solving, and to allow for more flexible builds, orbit supports multiple versions within the same dependency tree.

For example, an IP named `gates` can be used as different versions in the same project.

Consider the project `gates` with the following structure:

    /gates
        /nor_gate.vhd
        /IP.cfg

The `IP.cfg` is a mandatory manifest file for orbit to collect information about the given IP package. More discussion about `IP.cfg` is for a different time.

This would be a directory structure within the cache for `gates` if v1.0.0, v1.2.0, and v1.2.1 were installed:

    /gates 
        /v1.0.0
            /nor_gate.vhd
            /IP.cfg  
        /v1.2.0
            /nor_gate.vhd
            /IP.cfg
        /v1.2.1  
            /nor_gate.vhd
            /IP.cfg

Within each folder is the state of the code for the ip at its respective vcs version tags.

## How does this work in new projects?

A main concern you may have is, how would writing code differentiate between using
v1.0.0, v1.2.0, v1.2.1, when A has an entity say `nor_gate`. Trying to reference `nor_gate` under each context will run into errors regarding renaming/redefining the entity.

The solution is that for each installation in the cache, orbit actually performs a one-time analysis through the HDL files and alters the code with a _primary design unit modification_ (PDUM). Since primary design units are technically the API for your code,
modifying the identifiers will give unique units for each version.

So we have access to calling `nor_gate_v1_0_0`, `nor_gate_v1_2_0`, and `nor_gate_v1_2_1`. This is the tighest bound we can place on a unit since we explicitly specify the 3 version numbers, and we will see shortly that this is not always necessary.

## Version bounds

Say for our design we wanted to loosen the version bound for using `nor_gate`. Following semantic versioning guidelines, we want the latest backwards-compatible version under v1, so anything under the v1 tag will suffice. When installing packages, orbit also maintains who is the latest v1 version and performs PDUM for the highest v1 installation. 

So our cache now looks like this:

    /gates 
        /v1.0.0
            /nor_gate.vhd
            /IP.cfg  
        /v1.2.0
            /nor_gate.vhd
            /IP.cfg
        /v1.2.1  
            /nor_gate.vhd
            /IP.cfg
        /v1                 * files copied from v1.2.1 code state
            /nor_gate.vhd
            /IP.cfg

We have access to `nor_gate_v1`, which in our case references the state of code from release v1.2.1.

This same logic extends to using a version bound that only includes the first 2 version numbers, such as v1.0 and v1.2.

## Rationale behind version bounds

Version bounds increase the flexibility of your current project to easily accept
future changes to defined dependencies. When you are ready for a project to use a
newer compatible version within given version bounds, you will be able to open the project under development and run a single command to update your cache and the project's dependency file to now point to the newer versions.

## Tradeoffs

The main tradeoff between allowing this flexibility is that the "locked" version it selected may not always be selected. So it is important to think carefully about what version bounds you intend to allow.

Here is an example scenario:

- X uses A with bound v1 from v1.2.1
- Y uses A with bound v1 from v1.4.0
- A's latest version is v1.8.2

You now have project Z that must incorporate X and Y. However, what version should be pointed to for using the v1 bound? Enter _Minimum Version Selection_ (MVS) [2].

## A quick glance at MVS

Our MVS will build the entire graph, collecting what versions are referenced for each version bound. So for A under project Z's build, we can only choose between v1.2.1 and v1.4.0 for determining A's bound for v1; v1.8.2 is excluded. 

MVS will then select v1.4.0 to use as A's bound for v1, because it is the minimum allowed version from all of A's choices (v1.2.1, v1.4.0, v1.8.2) which is also the maximum of all the constraints. To get a better grasp of MVS, I encourage you to read https://research.swtch.com/vgo-mv.

### References

[1] [Dependency Solving Is Still Hard, but We Are Getting Better at It](https://arxiv.org/pdf/2011.07851.pdf)

[2] [Minimal Version Selection](https://research.swtch.com/vgo-mvs)