## motivation

When a producer function has a hard enough job that it requires maintaining state between values produced, most programming languages offer no pleasant and efficient solution beyond adding a callback function to the producer’s argument list, to be called with each value produced.

> keywords:  `producer`  `responsibility`  `callback` `state manage`

> Instead of using callback function we expose the value of the function expecting to produce, so we can use it in outside

可以降低==耦合(coupling)==
************

