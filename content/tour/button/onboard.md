---
title: "Reading button input"
description: Learn how to respond to button presses
weight: 1
---

Having some output using a LED is nice, but what's even better is if the electronics respond to you!

In this example, we'll use one of the supported boards that have a button on them. If you don't have one of these boards, you can just read the code here and try the next step in the tour where you can connect an external button (or just use a wire).

A lot of code will be familiar from the blinking LED in the previous step, but there are some important differences:

```go
button := machine.BUTTONA
button.Configure(machine.PinConfig{Mode: machine.PinInputPulldown})
```

Instead of configuring a LED, we'll configure the pin that's connected to the button labeled 'A' (usually the leftmost button). The mode is `PinInputPullup` or `PinInputPulldown`, the specific mode depends on the board (we'll come back to this later).

```go
if button.Get() {
	println("button input is high")
} else {
	println("button input is low")
}
```

Here we read out the current button state and print whether it was high (`true`) or low (`false`).

You may find that the button pin is normally high but changes to low when pressed, and other boards are the reverse. This depends on how the button is wired, and will be explained later.

## Serial connection

To see the output of the program, you can use a serial monitor. There is a [tutorial for that]({{< ref "serialmonitor.md" >}}). But in short:

  * If you have TinyGo installed, you can open a serial monitor using `tinygo monitor` command. You may need to specify the board, like `tinygo monitor -target=circuitplay-express`.
  * If you have the Arduino IDE installed, you can use the built-in serial monitor. Make sure to select the right port and to set the baud rate to 115200.
  * On Linux and MacOS you can use a command-line tool like picocom (which you probably need to install manually).
  * If you use Windows, Adafruit has a [list of tools you can use](https://learn.adafruit.com/windows-tools-for-the-electrical-engineer/serial-terminal).

## Button wiring



## Pull mode



<script type="module">
import { setupTour } from '/tour.js';
let codePulldown = `
package main

import (
	"machine"
	"time"
)

func main() {
	button := machine.BUTTONA
	button.Configure(machine.PinConfig{Mode: machine.PinInputPulldown})

	for {
		if button.Get() {
			println("button input is high")
		} else {
			println("button input is low")
		}
		time.Sleep(200 * time.Millisecond)
	}
}`;
let codePullup = codePulldown.replace('PinInputPulldown', 'PinInputPullup');
setupTour({
	boards: {
		'circuitplay-bluefruit': { code: codePulldown },
		'circuitplay-express': { code: codePulldown },
		'gopher-badge': { code: codePullup.replace('BUTTONA', 'BUTTON_A // Gopher Badge uses BUTTON_A') },
		'microbit': { code: codePullup },
	},
});
</script>
