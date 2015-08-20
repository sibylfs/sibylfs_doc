# System overview

The system structure is as follows:

![](systemstructure.svg)


  - The test scripts are a combination of automatically generated and
    hand-written scripts
    
  - These are fed into a test executor, which executes the tests on
    the host system; the result is a set of traces which record the
    behaviour of the tests as observed at the libc interface
    
  - The traces are then checked by SibylFS to produce a set of checked
    traces in various forms (ASCII, HTML)
    

