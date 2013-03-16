## Network programming in Haskell

Some people believe that
functional programming languages are slow or impractical.
However, to the best of our knowledge, 
Haskell provides a nearly ideal approach for network programming.
This is because the Glasgow Haskell Compiler (GHC), 
the flagship compiler for Haskell, provides
lightweight and robust user threads (sometimes called green threads).
In this section, we will briefly explain the history of
server side network programming.

### Native threads

Traditional servers use a technique called thread programming.
In this architecture, each connection is handled
by a single process or native thread- sometimes called an OS thread.
