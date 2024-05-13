# VLSI-LAB-EXP-4
SIMULATION AND IMPLEMENTATION OF SEQUENTIAL LOGIC CIRCUITS

AIM: 
 To simulate and synthesis SR, JK, T, D - FLIPFLOP, COUNTER DESIGN using vivado 2023.1.

APPARATUS REQUIRED:
vivado 2023.1.

**LOGIC DIAGRAM**

SR FLIPFLOP

![image](https://github.com/navaneethans/VLSI-LAB-EXP-4/assets/6987778/77fb7f38-5649-4778-a987-8468df9ea3c3)


JK FLIPFLOP

![image](https://github.com/navaneethans/VLSI-LAB-EXP-4/assets/6987778/1510e030-4ddc-42b1-88ce-d00f6f0dc7e6)

T FLIPFLOP

![image](https://github.com/navaneethans/VLSI-LAB-EXP-4/assets/6987778/7a020379-efb1-4104-85ee-439d660baa08)


D FLIPFLOP

![image](https://github.com/navaneethans/VLSI-LAB-EXP-4/assets/6987778/dda843c5-f0a0-4b51-93a2-eaa4b7fa8aa0)


COUNTER

![image](https://github.com/navaneethans/VLSI-LAB-EXP-4/assets/6987778/a1fc5f68-aafb-49a1-93d2-779529f525fa)


  
PROCEDURE:
1. Open Vivado: Launch Xilinx Vivado software on your computer.

2. Create a New Project: Click on "Create Project" from the welcome page or navigate through "File" > "Project" > "New".

3. Project Settings: Follow the prompts to set up your project. Specify the project name, location, and select RTL project type.

4. Add Design Files: Add your Verilog design files to the project. You can do this by right-clicking on "Design Sources" in the Sources window, then selecting "Add Sources". Choose your Verilog files from the file browser.

5. Specify Simulation Settings: Go to "Simulation" > "Simulation Settings". Choose your simulation language (Verilog in this case) and simulation tool (Vivado Simulator).

6. Run Simulation: Go to "Flow" > "Run Simulation" > "Run Behavioral Simulation". This will launch the Vivado Simulator and compile your design for simulation.

7. Set Simulation Time: In the Vivado Simulator window, set the simulation time if it's not set automatically. This determines how long the simulation will run.

8. Run Simulation: Start the simulation by clicking on the "Run" button in the simulation window.

9. View Results: After the simulation completes, you can view waveforms, debug signals, and analyze the behavior of your design.
VERILOG CODE
Dflipflop
~~~
module dff(d,clk,rst,q);
input d,clk,rst;
output reg q;
always @(posedge clk)
begin
if (rst==1)
q=1'b0;
else
q=d;
end
endmodule
~~~
OUTPUT

![image](https://github.com/Abitha-2004/VLSI-LAB-EXP-4/assets/161303006/46819139-6bbb-4391-949a-f01f77f4e97d)

![image](https://github.com/Abitha-2004/VLSI-LAB-EXP-4/assets/161303006/1e5cb39c-8964-4e6b-9a7c-157c2d41b043)
JKflipflop
~~~
module JK_flipflop (q, q_bar, j,k, clk, reset);  
  input j,k,clk, reset;
  output reg q;
  output q_bar;
  // always@(posedge clk or negedge reset) // for asynchronous reset
  always@(posedge clk) begin // for synchronous reset
    if(!reset)
           q <= 0;
    else 
  begin
      case({j,k})
        2'b00: q <= q;    // No change
        2'b01: q <= 1'b0; // reset
        2'b10: q <= 1'b1; // set
        2'b11: q <= ~q; // Toggle
      endcase
    end
  end
  assign q_bar = ~q;
endmodule
~~~
OUTPUT

![image](https://github.com/Abitha-2004/VLSI-LAB-EXP-4/assets/161303006/b5608b13-bbda-4afd-a37c-0a03020e9281)

![image](https://github.com/Abitha-2004/VLSI-LAB-EXP-4/assets/161303006/eabe32f4-9fab-4cd0-ada4-41323ba413a8)
Mod10Counter
~~~
module counter(
input clk,rst,enable,
output reg [3:0]counter_output
);
always@ (posedge clk)
begin 
if( rst | counter_output==4'b1001)
counter_output <= 4'b0000;
else if(enable)
counter_output <= counter_output + 1;
else
counter_output <= 0;
end
endmodule
~~~
OUTPUT

![image](https://github.com/Abitha-2004/VLSI-LAB-EXP-4/assets/161303006/e207e575-459a-4ed2-8517-b9c56d8dca18)

![image](https://github.com/Abitha-2004/VLSI-LAB-EXP-4/assets/161303006/fd964968-644a-4707-93ab-13373ec912bc)
Ripplecarrycounter
~~~
module D_FF(q, d, clk, reset);
output q;
input d, clk, reset;
reg q;
always @(posedge reset or negedge clk)
if (reset)
q = 1'b0;
else
q = d;
endmodule
module T_FF(q, clk, reset);
output q;
input clk, reset;
wire d;
D_FF dff0(q, d, clk, reset);
not n1(d, q); 
endmodule
module ripple_carry_counter(q, clk, reset);
output [3:0] q;
input clk, reset;
T_FF tffo(q[0], clk, reset);
T_FF tff1(q[1], q[0], reset);
T_FF tff2(q[2], q[1], reset);
T_FF tff3(q[3], q[2], reset);
endmodule
~~~
OUTPUT

![image](https://github.com/Abitha-2004/VLSI-LAB-EXP-4/assets/161303006/d9432b4e-3608-4d08-bab3-78fcfb957de9)

![image](https://github.com/Abitha-2004/VLSI-LAB-EXP-4/assets/161303006/b633f27c-12e7-4b03-aec4-a09577b318e7)
SRflipflop
~~~
module SR_flipflop (q, q_bar, s,r, clk, reset);
  input s,r,clk, reset;
  output reg q;
  output q_bar;
  always@(posedge clk) begin 
    if(!reset)�        q <= 0;
    else 
  begin
      case({s,r})
        2'b00: q <= q;    
        2'b01: q <= 1'b0; 
        2'b10: q <= 1'b1; 
        2'b11: q <= 1'bx; 
      endcase
    end
  end
  assign q_bar = ~q;
endmodule
~~~
output

![image](https://github.com/Abitha-2004/VLSI-LAB-EXP-4/assets/161303006/bc98c309-2fd4-4435-9d9e-638f491f2e7c)

![image](https://github.com/Abitha-2004/VLSI-LAB-EXP-4/assets/161303006/a558d65b-2f00-480f-b939-b3dde7b72183)
T flipflop
~~~
module tff (t,clk, rstn,q);  
 input t,clk, rstn;
 output reg q;
  always @ (posedge clk) begin  
    if (!rstn)  
      q <= 0;  
    else  
        if (t)  
            q <= ~q;  
        else  
            q <= q;  
  end  
endmodule
~~~
OUTPUT

![image](https://github.com/Abitha-2004/VLSI-LAB-EXP-4/assets/161303006/7dce4d97-68e0-4764-b217-4999c06a418d)

![image](https://github.com/Abitha-2004/VLSI-LAB-EXP-4/assets/161303006/597f7bf3-50ac-4783-b3e3-f07daa9402ea)
Updowncounter
~~~
module updown_counter(clk,rst,updown,out);
input clk,rst,updown;
output reg [3:0]out;
always@(posedge clk)
begin
if (rst==1)
out=4'b0000;
else if(updown==1)
out=out+1;
else
out=out-1;
end
endmodule
~~~
output

![image](https://github.com/Abitha-2004/VLSI-LAB-EXP-4/assets/161303006/58b35e77-1d9f-41d6-b4dc-9f0b9fb9e6ff)

![image](https://github.com/Abitha-2004/VLSI-LAB-EXP-4/assets/161303006/25556d01-72bb-4ce9-8d7e-10d3e60ee078)

RESULT
Simulate And Synthesis SR, JK, T, D - FLIPFLOP, COUNTER DESIGN is Successfully Verified using Vivado Software.




