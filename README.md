# Verilog Gadget for Sublime Text

[![Package Control](https://packagecontrol.herokuapp.com/downloads/Verilog%20Gadget.svg?style=round-square)](https://packagecontrol.io/packages/Verilog%20Gadget)

Use **Command Palette (ctrl+shift+p)** or **Right-Click (context menu)** to run.
The context menu only can be seen for `.v, .vh, .sv, .svh` file.
(file extensions can be added or changed in settings)  
**NOTE** : `View Log` command was deprecated.
Use **[Log Highlight](https://packagecontrol.io/packages/Log%20Highlight)** instead, I separated this function with adding customizable settings.

* **Verilog Gadget: Instantiate Module**
	- It parses module ports in currently open file
	- It generates module's instance text
	- It copies generated text to clipboard
	- You can paste the text on where you want
	- Support Verilog-1995, Verilog-2001 style ports and parameters
```Verilog
	e.g)
		module test #(parameter WIDTH = 3)(input [WIDTH - 1:0] a, output [WIDTH - 1:0] b);
		endmodule

		--> generate

		test #(.WIDTH(WIDTH)) inst_test (.a(a), .b(b));
```

![Image](https://raw.githubusercontent.com/poucotm/Links/master/image/vg_inst.gif)

* **Verilog Gadget: Generate Testbench**
	- It parses module ports in currently open file
	- It generates a simple testbench with module's instance and signals
	- Testbench will be generated as a systemverilog file
	- Support Verilog-1995, Verilog-2001 style ports and parameters
```Verilog
	e.g)
		module test #(parameter WIDTH = 3)(input [WIDTH - 1:0] a, output [WIDTH - 1:0] b);
		endmodule

		--> generate

		`timescale 1ns/1ps

		module tb_test (); /* this is automatically generated */
			logic rstb;
			logic clk;
			...
			parameter WIDTH = 3;
			...
			logic [WIDTH - 1:0] a;
			logic [WIDTH - 1:0] b;

			test #(.WIDTH(WIDTH)) inst_test (.a(a), .b(b));

			initial begin
			...
		endmodule
```

![Image](https://raw.githubusercontent.com/poucotm/Links/master/image/vg_testb.gif)

* **Verilog Gadget: Insert Template**
	- You can insert your own template text (larger than a snippet) from the file specified in settings
	- Multiple templates are possible
	- Run `Insert Template` and choose the type listed in the quick panel
```json
	e.g)
		In settings :

		"templates": [
			[ "Default", "Packages/Verilog Gadget/template/verilog_template_default.v" ],
			[ "FSM", "D:/template/verilog_fsm_template.v" ]
		]
```

* **Verilog Gadget: Insert Header**
	- You can insert your own header-description as your format from the file specified in settings
	- {DATE} will be replaced with current date
	- {YEAR} will be replaced with this year
	- {TIME} will be replaced with current time
	- {FILE} will be replaced with current file name
	- {TABS} will be replaced with current tab size
	- {SUBLIME_VERSION} will be replaced with current sublime text version
	- {ENCODING} will be replaced with current encoding (utf-8,...)
```Verilog
	e.g)
		In settings :

		"header": "D:/template/verilog_header.v"

		//
		// -----------------------------------------------------------------------------
		// Copyright (c) 2014-{YEAR} All rights reserved
		// -----------------------------------------------------------------------------
		// Author : yongchan jeon (Kris) poucotm@gmail.com
		// File   : {FILE}
		// Create : {DATE} {TIME}
		// Editor : sublime text{SUBLIME_VERSION}, tab size ({TABS})
		// -----------------------------------------------------------------------------

		--> after insertion

		//
		// -----------------------------------------------------------------------------
		// Copyright (c) 2014-2016 All rights reserved
		// -----------------------------------------------------------------------------
		// Author : yongchan jeon (Kris) poucotm@gmail.com
		// File   : abc.v
		// Create : 2016-06-03 22:34:43
		// Editor : sublime text3, tab size (3)
		// -----------------------------------------------------------------------------
```

![Image](https://raw.githubusercontent.com/poucotm/Links/master/image/vg_head.gif)

* **Verilog Gadget: Repeat Code with Numbers**
	- Select codes to be repeated, it may include Python's format symbol like {...}
	- Run `Repeat Code with Numbers` command (default key map : `ctrl+f12`)
	- Type a range in the input panel as the following : [from]~[to],[↓step],[→step]
	  ``(e.g. 0~10 or 0~10,2 or 10~0,-1 or 0~5,1,1 ...)``
	- [↓step] means row step, default is 1, [→step] means column step, default is 0
	- The codes will be repeated with incremental or decremental numbers
	- Python's format symbol supports variable formats : binary, hex, leading zeros, ...
	- To use '{' as is, you should type twice as '{{'
	- Refer to Python's format symbol here, [https://www.python.org/dev/peps/pep-3101/](https://www.python.org/dev/peps/pep-3101/)
	- For **sublime text 2 (python 2.x)**, you should insert index behind of ':' in curly brackets like `foo {0:5b} bar {1:3d}`
```Verilog
	e.g)
		case (abc)
			5'b{:05b} : def <= {:3d};

		--> select `5'b{:05b} : def <= {:3d};`, run the command and type the range 0~10,1,3

		case (abc)
			5'b{:05b} : def <= {:3d};
			5'b00000 : def <=   3;
			5'b00001 : def <=   4;
			5'b00010 : def <=   5;
			5'b00011 : def <=   6;
			5'b00100 : def <=   7;
			5'b00101 : def <=   8;
			5'b00110 : def <=   9;
			5'b00111 : def <=  10;
			5'b01000 : def <=  11;
			5'b01001 : def <=  12;
			5'b01010 : def <=  13;
		...

		abc[{0:2d}] = {0:2d} + {1:2d} + {2:2d} + {2:2d};

		--> index-used case, select and run the command and type the range 0~8,1,2

		abc[ 0] =  0 +  2 +  4 +  4;
		abc[ 1] =  1 +  3 +  5 +  5;
		abc[ 2] =  2 +  4 +  6 +  6;
		abc[ 3] =  3 +  5 +  7 +  7;
		abc[ 4] =  4 +  6 +  8 +  8;
		abc[ 5] =  5 +  7 +  9 +  9;
		abc[ 6] =  6 +  8 + 10 + 10;
		abc[ 7] =  7 +  9 + 11 + 11;
		abc[ 8] =  8 + 10 + 12 + 12;
```

![Image](https://raw.githubusercontent.com/poucotm/Links/master/image/vg_rep.gif)

## issues

When you have an issue, tell me through [https://github.com/poucotm/Verilog-Gadget/issues](https://github.com/poucotm/Verilog-Gadget/issues), or send me an e-mail poucotm@gmail.com, yongchan.jeon@samsung.com