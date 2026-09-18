# A Brief History of Computers and Programming Languages

## Motivation

But, after all, what is a computer? Do we really need to know the answer before we start programming? Actually, no. However, understanding how a computer works makes us better programmers. Defining what a computer is can be more complicated than it seems. We will see that the idea of a "computer" has changed a lot throughout history, always tied to the need to count. Before diving into the history, let's explore a few different answers to this question.

At the very start of the 2025 classes, in the MAC115 - Introduction to Computing for Exact Sciences and Technology course, we asked the students a simple question: "What is a computer?". We wrote down the answers (see {ref}`sec-questionario`), and despite some funny answers, like "a robot that will control humans one day", we noticed that most students thought of similar things:

1. A computer is a physical object; a machine or device.
2. It is capable of processing information or data.
3. It's like a calculator, just more modern.

These ideas capture some of what a computer does, but they don't fully explain what it is.

## Do Computers Need to Exist?

Professor Douglas Hartree (1897–1958), in his book "Calculating Instruments and Machines", says that computers can be seen from two angles: anatomical (what they are made of) and physiological (how they work). Defining a computer by its parts would not be enough, since computer parts have changed a great deal over the years.

Around the same time, Professor Arthur L. Samuel proposed a functional definition of what a computer is. According to Samuel, a computer is "a device for performing or facilitating the information process, that is, the handling of information in whatever form it may be presented, according to a well-defined procedure." This technical view agrees with what the students think. However, this definition leads us to another question: do computers necessarily need to exist as physical devices or machines, or can we conceive of computation beyond the material aspect?

If we asked someone on the street what they think a computer is, they would most likely say it is a "machine" or a "device". Indeed, this idea comes up often in the answers we collected. However, modern computers were created from an abstract mathematical model called the Turing Machine.

As a child, at age 11, Alan Mathison Turing wrote a letter to his parents, Julius Mathison Turing and Ethel Sara Stoney Turing, describing his idea for how to build a typewriter. Alan Turing did not invent the typewriter, but he used that idea to create the Turing Machine: a mathematical model of computation that manipulates symbols on an infinite tape, following well-defined rules. This model gave rise to modern computers which, according to John A. Robinson, are nothing more than "physical manifestations of one and the same logical abstraction: the universal Turing machine." So, no. Computers do not need to exist.

## Abacuses, Calculating Machines, and Almost-Computers

This is where we start talking about the history of computing. We humans are very creative. We are always building tools to help us. Archaeological evidence suggests that humans have practiced counting for at least 50,000 years. Counting was used by ancient peoples to track social and economic data, such as the number of people in a group, animals, property, or debts. So it's no surprise that humans invented tools to help with this process. Around 2700–2300 BC, humans created the abacus: a tool for doing arithmetic faster.

Over the following centuries, other tools were created to help with this. One example is the slide rule, from the 17th century, which did the same thing as the abacus, but using properties of logarithmic functions. The first computers were like more modern versions of these tools, that is, calculating machines. Charles Babbage's Difference Engine (1820), an idealized but never fully built machine, is an example of a calculating machine designed to compute polynomial functions.

The Englishman Charles Babbage was a mathematician, philosopher, inventor, and mechanical engineer, and is considered by many to be the "father of the computer". This title comes from another machine he designed. The Analytical Engine (1840), unlike any machine of its time, marked the transition from mechanized arithmetic to general-purpose computation. This machine would be programmed using punched cards and would incorporate several features later adopted by modern computers, such as sequential control, branching structures, and repetition mechanisms (loops).

While the Analytical Engine was being designed, the Countess of Lovelace, Augusta Ada Byron King (daughter of the poet Lord Byron), created an algorithm to compute the sequence of Bernoulli numbers, and for that reason is considered the first programmer. Today, Ada's figure is used as an inspiration to increase the presence of women in computing, which was much greater a few decades ago.

Up to that point, every machine that had been built was strictly mechanical. The Z2 was one of the first examples of an electrically operated digital computer, built with electromechanical relays, and was created by civil engineer Konrad Zuse in 1940 in Germany. But it was the invention of the vacuum tube (thermionic valve) that gave us the first real leap into the era of digital computers. Vacuum tubes were fully electric devices that generated electric current from the phenomenon of thermionic emission, and were invented by physicist John Ambrose Fleming in 1904.

The Z3, the successor to the Z2, was also developed by Zuse and is considered the first fully automatic, programmable digital computer in the world. Konrad Zuse was also responsible for designing Plankalkül, the first high-level programming language, that is, one closer to human language and farther from machine language. Although this language was never implemented at the time, it introduced fundamental programming concepts such as data types and control structures.

Machines like the Z3, Colossus, and the ENIAC were built by hand, using circuits made of relays or vacuum tubes, and often used punched cards or punched paper tape for input and as their main storage medium. Most machines from this period were not able to store and modify programs (with the exception of the Z3). On the ENIAC, a "program" was defined by the configuration of its connection cables and switches, a feature that distinguishes these machines from modern computers. Programming at that time was carried out mostly by women, though this predominance gradually declined over the following years.

## The Birth of Modern Computing

Up to this point, every machine (Babbage's, Zuse's, Colossus) followed the same design proposed by Babbage: machines were built to perform calculations, and then coded instructions, stored separately in some other form, were organized to make them run. The great paradigm shift happened in 1945 with the ideas of Alan Turing and John von Neumann. Both realized that programs should be stored in the same way as data. The Manchester Baby was the world's first electronic stored-program computer and ran its first program on June 21, 1948. The von Neumann architecture marks the beginning of the era of modern computers. Since then, advances have been made to make them faster, smaller, and easier to use, but their core remains the same: the inherent universality of the stored-program computer.

In the same decade, Kathleen Booth, working in the same place as von Neumann, developed the first assembly language, a programming language very close to machine language. However, programming in assembly demanded considerable intellectual effort. The languages that came afterward were attempts to abstract machine language into a high-level language, closer to natural, human language. Although several languages were created in the following years, it was only in 1954 that the first widely adopted language appeared: FORTRAN, developed at IBM by a team led by John Backus. Today, more than 70 years later, FORTRAN is still used to rank the TOP500 list of the world's fastest supercomputers.

Starting in 1955, transistor technology revolutionized computing by replacing vacuum tubes in computer design. Transistors had significant advantages: they were smaller and used less power, and consequently generated less heat than their predecessors. The milestone of this transition was the TRADIC Phase One, completed in 1954, considered the first fully transistorized computer.

Technological progress continued with the invention of the integrated circuit in 1958 by Jack Kilby, who was later recognized with the Nobel Prize in the 2000s for this contribution. An integrated circuit consists of a set of electronic circuits made up of several components (transistors, resistors, and capacitors) and their interconnections. These components are carefully etched onto a small, flat piece known as a "chip", made from a semiconductor material, initially germanium and today silicon.

The period from the late 1960s to the late 1970s was marked by the emergence of many programming languages, as well as the consolidation of the main paradigms we know today. Simula, created by Norwegian computer scientists Ole-Johan Dahl and Kristen Nygaard, stood out as the first language specifically designed to support object-oriented programming. At the same time, Dennis Ritchie and Ken Thompson were developing the C language at Bell Labs between 1969 and 1973, aimed at systems programming. The development of C was closely tied to the UNIX operating system, since C was created precisely to make it easier to port UNIX across different hardware platforms. In that same context of innovation, the ML language, conceived by British scientist Robin Milner, emerged as a pioneer among statically typed functional programming languages.

In terms of hardware, the microprocessor enabled the last great leap in the history of computing. Its development was only possible thanks to MOS integrated circuits (CMOS), which allowed the progressive miniaturization of transistors. Today, we are already able to produce transistors on the order of 50 nanometers, which is remarkable when we consider that the radius of a silicon atom is approximately 0.13 nanometers.

On the other hand, the evolution of programming languages was especially notable in the 1990s, driven by the rapid growth of the Internet. During this period, programmer productivity became an important concern, which led to the emergence of several Rapid Application Development (RAD) languages. These languages came together with integrated development environments (IDEs) and garbage collection features. As a result of this trend, languages such as Python, Lua, R, Ruby, Java, JavaScript, and PHP emerged.

The growing popularity of new programming languages was driven by mobile applications. Cell phones and mobile devices became increasingly present in people's daily lives, significantly increasing the demand for apps. Some examples of these languages include Dart, Kotlin, TypeScript, and Swift.

In 2012, the language that will be studied in this course was born: Julia. Aiming to be both fast and productive, Julia combines the performance of older languages with the productivity of more recent ones. It was developed by Jeff Bezanson, Stefan Karpinski, Viral B. Shah, and Alan Edelman. The site <https://julialang.org/benchmarks/> presents a comparative benchmark of different languages solving several algorithms. There, we can see that Julia performs on par with the C language.
