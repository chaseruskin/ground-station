Motion 0x01
===========
Chase Ruskin  
2022/02/08  

The hardware development ecosystem is overdue for an upgrade. A big upgrade.

The software industry is rapidly increasing. Software developers are in demand everywhere. Anyone can go online to a code-hosting website such as Github to retrieve a repository and contribute. Open-source has become increasingly popular and relied on in helping move the world forward. It is a great time to be a writing software, but what about hardware, the very thing that allows software to be written in the first place? Tucked in the corner, there is one particular niche set of languages separated from the "software" world; hardware description languages (HDL).

To write in HDL requires a drastically different form of thinking in comparison to writing in programming languages. Programming languages are _compiled_ into a set of instructions that execute sequentially. HDL does not naturally translate to this logic, rather they are more so a circuit diagram translated into text that becomes _synthesized_ into a series of digital logic gates. Even though HDL does not fall under the domain of "software programming languages", they are similar under one key point:

> HDL is code. One intently writes text in a particular style to allow their thoughts to be translated into some form of output.

This single key point brings HDL development under the branch of engineering coined "software engineering".

Software engineering is more than just code development. It also involves testing, updating, installing, managing, improving, assessing, integrating, and delivering. It encapsulates the entire process of taking an idea or design and turning it into a real product. And software engineering is rarely done alone; it takes a team of diversely skilled people to operate under this practice.

A major component to software engineering is code management. Poor code management can have the consequences of introducing fragmentation- where code splits from a single source and becomes difficult to build upon.

Fragmentation is software engineering's worst enemy. Fragmentation:
- violates the DRY principle by encouraging developers to copy files across projects
- slows productivity by requiring time and resources to get an integrated project into a working state
- does not scale well
- burdens the developer in ensuring all necessary code is installed and in proper places
- hinders updating code as it may worsen fragmentation and create more work for developers to get the changes
- prevents fixing bugs in deeply dependent code as the fix will fail to reach all affected areas due to the file, and therefore the bug, existing in more than one place
- blocks fast time-to-market and delivery of the product

As time progresses, developers are wasting their time resolving fragmentation issues and not developing.

Software languages have become increasingly good at handling code management through the creation of tools, typically in their own respective languages, known as _package managers_ to help automate and streamline the development process for its users. To name a few, there is go mod for Go, Cargo for Rust, vcpkg for C/C++, npm for JavaScript, pip for Python, and RubyGems for Ruby. What about HDL? One does not write a "program" in HDL to manage packages and files while keeping the codebase unified. A software language must be used to create such program.

There are lots software languages available today, each with their own features and quirks. While many strive to achieve dominance for a developer's preference, languages are like tools in a toolbox. Some are more suited for particular situations than others.

To select the right language for creating a package manager  within the HDL ecosystem, it is important to identify the desired features. In the context of HDL, a package manager should be:

- robust and well-tested: 
    * results should be predictable, meaning it is verified to work under a range of situations and handle the various errors a user may produce
- flexible: 
    * support the multiple tasks an HDL developer can do in the various ways: linting, synthesizing, executing internal tools, simulating, timing analysis
- fast: 
    * solving dependency resolutions and scanning files should be incredibly fast to handle increasing and large-scale codebases
- secure
    * safe against attempts to exploit the program and the code it manages
- simple
    * easy to pick up and learn, while not overstepping its responsibilities

 It is time for an HDL package manager to change the game for the hardware development ecosystem.