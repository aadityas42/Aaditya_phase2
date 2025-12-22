# Chennai Express

## Solution

- In this, we have 3 tracks and a lot of switches, which we can change with the goal of making the trains collide on track 1.
- Only one switch can be green at a time to prevent collisions. However, the switches handle delays differently:

    Normal Switches: Delay ->Check -> Switch to Green.

    Switch -1 (Vulnerable): Check -> Delay -> Switch to Green.

- By triggering a normal switch and then immediately triggering Switch -1, Switch -1 performs its "Check" while the first switch is still in its "Delay" phase. Both checks pass, and both switches eventually turn green at the same time.
- We do as follows:

  Move Train B: Use Switch -3 to put it on the path to Track 1.

  Move Train A: Use Switch 6 to put it on Track 3.

  Divert Train B: Use Switch 2 to move it to Track 2 temporarily. This clears Track 1 so the safety checks pass.

- Send two packets to the server as fast as possible (or in a single stream):

    Activate Switch -3 (Normal behavior).

    Activate Switch -1 (Bugged behavior).

- Because of the flaw in Switch -1, the system fails to realize that Switch -3 is already about to turn green. Both tracks open, the trains enter Track 1 simultaneously, and the speed difference causes them to collide.

## Flag
```
nite{T#r0ugh_g0e5_Th0m4s!!}
```
