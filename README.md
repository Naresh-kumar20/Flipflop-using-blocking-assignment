# EXPERIMENT 3A: Simulation of All Flip-Flops using Blocking Assignment

## AIM
To design and simulate basic flip-flops (SR, D, JK, and T) using **blocking assignment** in Verilog HDL, and verify their functionality through simulation in Vivado 2023.1.

## APPARATUS REQUIRED
- Vivado 2023.1
- Computer with HDL Simulator

## DESCRIPTION
Flip-flops are the basic memory elements in sequential circuits.  
In this experiment, different types of flip-flops (SR, D, JK, T) are modelled using **behavioural modelling** with **blocking assignment (`=`)** inside the `always` block.  
Blocking assignments execute sequentially in the given order, which makes it easier to describe simple synchronous circuits.

## PROCEDURE
1. Open **Vivado 2023.1**.  
2. Create a **New RTL Project** (e.g., `FlipFlop_Simulation`).  
3. Add Verilog source files for each flip-flop (SR, D, JK, T).  
4. Add a testbench file to verify all flip-flops.  
5. Run **Behavioral Simulation**.  
6. Observe waveforms of inputs and outputs for each flip-flop.  
7. Verify that outputs match the truth table.  
8. Save results and capture simulation screenshots.

---

## VERILOG CODE

### SR Flip-Flop (Blocking)

```
module sr_ff (
    input wire S, R, clk,
    output reg Q
);
    always @(posedge clk) begin
        if (rst)
            q = 0;
        else if (s == 1 && r == 0)
            q = 1;
        else if (s == 0 && r == 1)
            q = 0;
        else if (s == 1 && r == 1)
            q = 1'bx; 
        else
            q = q;   
    end
endmodule
```

### SR Flip-Flop Test bench 

```
module sr_ff_tb;
    reg clk_t, rst_t, s_t, r_t;
    wire q_t;
    sr_ff dut(.clk(clk_t), .rst(rst_t), .s(s_t), .r(r_t), .q(q_t));
    always #5 clk_t = ~clk_t;
    initial begin
        clk_t = 0;
        rst_t = 1;
        s_t = 0;
        r_t = 0;
        #10 rst_t = 0;
        #10 s_t = 1; r_t = 0;
        #10 s_t = 0; r_t = 1;
        #10 s_t = 0; r_t = 0;
        #20 $finish;
    end
endmodule
```

#### SIMULATION OUTPUT

<img width="1280" height="720" alt="image" src="https://github.com/user-attachments/assets/44e22871-b140-4f86-adeb-06be1a32eb77" />

### JK Flip-Flop (Blocking)

```
module jk_ff (
    input wire J, K, clk,
    output reg Q
);
    always @(posedge clk) begin
if (rst) begin
            q = 0;
        end
        else begin
            case ({j, k})
                2'b00: q = q;      
                2'b01: q = 0;      
                2'b10: q = 1;      
                2'b11: q = ~q;     
                default: q = q;    
            endcase
        end
    end
endmodule
```

### JK Flip-Flop Test bench 

```
module jkflipflop_tb;
    reg j, k, clk, rst;
    wire q;
    jkflipflop uut(j, k, clk, rst, q);
    always #5 clk = ~clk;
    initial begin
        clk = 0;
        j = 0;
        k = 0;
        rst = 1;
        $monitor("Time=%0d, clk=%b, rst=%b, j=%b, k=%b, q=%b", $time, clk, rst, j, k, q);
        #10 rst = 0;
        #10 j = 1; k = 0;
        #10 j = 0; k = 0; 
        #10 j = 0; k = 1; 
        #10 j = 1; k = 1; 
        #10 j = 1; k = 0; 
        #20 $finish;
    end
endmodule
```

#### SIMULATION OUTPUT

<img width="1280" height="720" alt="image" src="https://github.com/user-attachments/assets/e042b121-5ec7-4057-b61f-d365e5b6643f" />

### D Flip-Flop (Blocking)

```
module d_ff (
    input wire d,clk,
    output reg Q
);
    always @(posedge clk) begin
begin
if(rst==1)
q=0;
else
q=d;
end 
endmodule
```

### D Flip-Flop Test bench 

```
module dff_tb;
    reg clk_t, rst_t, d_t;
    wire q_t;
    dff dut (.clk(clk_t), .rst(rst_t), .d(d_t), .q(q_t));
    always #5 clk_t = ~clk_t;
    initial begin
        clk_t = 0;
        rst_t = 1;
        d_t = 0;
        #10 rst_t = 0;
        #10 d_t = 1;
        #10 d_t = 0;
        #10 d_t = 1;
        #20 $finish;
    end
endmodule
```

#### SIMULATION OUTPUT

<img width="1280" height="720" alt="image" src="https://github.com/user-attachments/assets/5aebe736-4644-437b-bc31-00f96fdf557a" />

### T Flip-Flop (Blocking)

```
module d_ff (
    input wire d,clk,
    output reg Q
);
    always @(posedge clk) begin
begin 
if (rst==1)
q=0;
else if(t)
q = ~q;
else
q=t;
end
endmodule
```

### T Flip-Flop Test bench 

```
module tff_tb;
reg clk_t,rst_t,t_t;
wire q_t;
tff dut(.clk(clk_t),.rst(rst_t),.t(t_t),.q(q_t));
initial 
begin
clk_t=0;
rst_t=1;
t_t=0;
#20
rst_t=1;
t_t=1;
#20
t_t=1;
end 
always
#10
clk_t=~clk_t;
endmodule
```

#### SIMULATION OUTPUT

<img width="1280" height="720" alt="image" src="https://github.com/user-attachments/assets/976cf7d7-97f0-4ec3-b31f-9798db78e7fd" />


### RESULT

All flip-flops (SR, D, JK, T) were successfully simulated using blocking statements in Verilog HDL.
The outputs matched the expected truth table values, demonstrating correct sequential behavior.
