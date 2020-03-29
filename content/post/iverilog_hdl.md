---
date: 2020-03-29
title: open source verilog emulation tool
weight: 10
---

# Introduction

FPGA is an interesting topic. Products with different market proposition are
on the market. Xilinx's product ranks in high performance while Lattice is on
lower cost. Verilog is a HDL (Hardware Description Language). This document
describe step to use [iverilog](https://github.com/steveicarus/iverilog.git)
on my Mac book.

# Build and Install

The steps is written in details on https://github.com/steveicarus/iverilog.git.
Please note that it needs the new version of Bison as mentioned "It has been
reported that bison 2.3 on MacOS generates broken code, but bison 3.0.4 works."

I was just using brew to install the latest bison, please note that at end of
bison installation, following is suggest to use the brew installed version
instead of osx default one.

```
echo 'export PATH="/usr/local/opt/bison/bin:$PATH"' >> ~/.zshrc 
```

The above command might be different for `bash`, as it is for zsh.

# An example

The following example is copied from the examples directory:
examples/xram16x1.v and some var name are replaced for easy understanding.


```verilog

// defines a 16x1 ram module
module ram16x1 (out, data, index, assign_flag, wclk);
     output out;
     input data;
     input [3:0] index;
     input assign_flag;
     input wclk;

     reg mem[15:0];

     assign out = mem[index];
     always @(posedge wclk) if (assign_flag) mem[index] = data;

 endmodule /* ram16x1 */

// the toplevel emulation module
module main;
    wire out;
    reg data;
    reg [3:0] index;
    reg assign_flag, wclk;

    ram16x1 r1 (out, data, index, assign_flag, wclk);

    initial begin
    $dumpfile("show_vcd.vcd");
    $dumpvars(1, main.r1);
    wclk = 0;
    assign_flag = 1;
    for (index = 0 ; index < 4'hf ;  index = index + 1) begin
        data = index[0];
        #1 wclk = 1; //the data is written to ram on positve edge
        #1 wclk = 0;
        $display("r1[%x] == %b", index, out);
    end

    for (index = 0 ; index < 4'hf ;  index = index + 1)
        #1 if (out !== index[0]) begin
            $display("FAILED -- mem[%h] !== %b", index, index[0]);
            $finish;
        end

        $display("PASSED");
    end
endmodule /* main */

```

The commands started with `$` are for emulation only. $dumpfile and $dumpvars
are for dump all data to e.g. `show_vcd.vcd`.

# compile

```
iverylog -oxram16x1 xram16x1.v
```

A copy could be fetched from [here](https://github.com/pandysong/iverilog_xram16x1.git)

Where a Makefile is added, so a simple `make` could suffice.

# run

```
./xram16x1
```

The output:

```
VCD info: dumpfile show_vcd.vcd opened for output.
r1[0] == 0
r1[1] == 1
r1[2] == 0
r1[3] == 1
r1[4] == 0
r1[5] == 1
r1[6] == 0
r1[7] == 1
r1[8] == 0
r1[9] == 1
r1[a] == 0
r1[b] == 1
r1[c] == 0
r1[d] == 1
r1[e] == 0
PASSED
```

The above printing is corresponding to the code:

```
$display("r1[%x] == %b", index, out);
```

# open the data

To run `xram16x1`, it also generates "show_vcd.vcd" file.

using gtkwave command to show:

```
gtkwave show_vcd.vcd
```

The tool could shows all the logics during emulation:

![gtkwave](/img/gtkwave_xram16x1.png)

Some explanation on the above figure:

- index[3:0] is the index to access the ram
- data is the 1 bit data get out of ram
- if `assign_flag` is set, during positive edge of wclk, the data is saved to
  ram
- the reading of ram, does not need an wclk, so in the second part to read the
  memory, wclk is not altered.

BTW, I am not sure what "rect square" means in the figure.
