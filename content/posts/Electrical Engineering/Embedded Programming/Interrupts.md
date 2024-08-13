---
title: "Interrupts"
date: 2024-08-13T11:35:39.977Z
---

# Interrupts

(docs)[https://www.learningaboutelectronics.com/Articles/Interrupts-embedded-C-for-micrcontrollers.php]
Interrupts are hardware executions used in computer systems to stop the current execution code, execute some code in the interrupt, and then return back to the main execution code.

Because they stop the execution code completely, the `Interrupt Service Routine (ISR)` handlers has to be quick so that it doesn't block the execution too much, and that there are not too many Interrupts being called.

Systems can be written completely with interrupts.

```C++
interrupt [EXT_INT0] void external_into0 (void)
{
//whatever is in this function will execute. Called automaticcally on external interrupt 0
}
```

An interrupt modifier is given an `interrupt vector` (the address of the hardware interrupt).

## Interrupts in Arduinos

(docs)[https://deepbluembedded.com/arduino-interrupts-tutorial-examples/]
