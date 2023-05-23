与门：（全一才一） 
A   B   Y
0   0   0
0   1   0
1   0   0
1   1   1
或门：（有一为一）
A   B   Y
0   0   0
1   0   1
0   1   1
1   1   1
异或门：（相同为0，不同为1）
A   B   Y
0   0   0
0   1   1
1   0   1
1   1   0
与非门：（全1为0）
A   B   Y
0   0   1
0   1   1
1   0   1
1   1   0
或非门：（全0为1）
A   B   Y
0   0   1
0   1   0
1   0   0
1   1   0



32位加法器：
module adder (input wire [31:0] a,
	         input wire [31:0] b,
	       output reg [32:0] y);
always @ (*) begin
    y = a + b;
end
   //assign y = a + b;
endmodule
Testbench：
`timescale 1ns/1ns

module adder_tb ();

reg [31:0] a;
reg [31:0] b;
wire [32:0] y;
adder dut (
	. a(a),
	. b(b),
	. y(y)
);

initial
begin
	a = 32'd 10;
	b = 32'd 20;
	#10_000;
	a = 32'd 30;
	b = 32'd 40;
	#10_000;
	a = 32'd 50;
	b = 32'd 60;
	#10_000;
	$stop;
end

endmodule
反相器：
module inv (input wire [3:0] a,
	   	  output reg [3:0] y);
assign y = ~a;//改成always语句
endmodule  
Testbench:
`timescale 1ns/1ns

module inv_tb;
reg a;
wire y;
 
 
inv dut (
        . a(a),
        . y(y)
        );
 
initial begin
            a=0;
    #10_000 a=1;
    #10_000 a=0;
    #10_000 a=1;
    #10_000 $stop;
end
 
endmodule
逻辑门：
module gates (input wire [3:0] a, b,
	       output reg [3:0] y1, y2, y3, y4, y5);
/*五种不同的2输入逻辑门对4位总线进行操作*/
assign y1 = a & b;      // AND
assign y2 = a | b;      //OR
assign y3 = a ^ b; 	 //XOR
assign y4 =~ (a & b);   //NAND
assign y5 =~ (a | b);    // NOR
endmodule

Testbench:
`timescale 1ns/1ns

module gates_tb;
reg a;
reg b;
wire y1;
wire y2;
wire y3;
wire y4;
wire y5;

 
 
gates dut (
        . a(a),
		. b(b),
		. y1(y1),
. y2(y2), 
. y3(y3), 
. y4(y4),
. y5(y5)
        );
 
initial begin
            a=0;
b=0;
#10_000 
a=0;
b=1;
#10_000 
a=1;
b=0;

#10_000 
a=1;
b=1;
    #10_000 $stop;
end
 
endmodule
二选一多路开关：
module mux2(input wire [7:0] d0，d1,
input wire [1:0] s,
output reg [7:0] y);
assign y = s? d1: d0;
endmodule
//lf s = 1, then y = d1. lf s = 0, then y = d0
Testbench:
`timescale 1ns/1ns
module mux2_tb ();    
reg [1:0] s;    
reg [7:0] d0;
reg [7:0] d1;    
wire [7:0] y;        
mux2 dut (       
. s (s),        
. d0 (d0),        
. d1(d1),        
. y(y)   
 );        
initial 
begin        
// Test case 1: select=0, input_0=5, input_1=10        
s = 2'b 00;        
d0 = 8'd 5;        
d1 = 8'd 10;        
#10_000;               
 // Test case 2: select=1, input_0=5, input_1=10        
s = 2'b 01;       
d0 = 8'd 5;        
d1 = 8'd 10;        
#10_000;                
// Test case 3: select=0, input_0=255, input_1=127        
s = 2'b 00;        
d0 = 8'd 255;       
d1 = 8'd 127;        
#10_000;               
// Test case 4: select=1, input_0=255, input_1=127        
s = 2'b 01;        
d0= 8'd 255;        
d1= 8'd 127;        
#10_000;                
// End simulation        
$stop;    
end    
endmodule

8输入与门：
module and8(input wire [7:0] a, 
output reg y);
always @ (*) begin
y = &a;
// &a 比 assign y = a[7] & a[6] &a[5]& a[4]&a[3] & a[2] &a[1] & a[0];
end
//容易写得多
endmodule

Testbench:
`timescale 1ns/1ns

module and8_tb ();
  reg [7:0] a;
  wire y;
  
  and8 dut (
    . a(a),
    . y(y)
  );
  
  initial 
begin
    // 激活输入值
    a = 8'b11111111;
#5_000;
a = 8'b11110000;
    #5_000;
a = 8'b00001111;
#5_000;
a = 8'b01010101;
#5_000;
$stop;
  end
endmodule

四选一多路开关（采用嵌套）：
module mux4(input wire [3:0]  d0，d1，d2，d3,
input wire [1:0] s,
output reg [3:0] y);
always @ (*) begin
y = s [1]? (s [0]? d3: d2)
:(s [0]? d1: d0);
end
endmodule
Testbench:
`timescale 1ns/1ns

module mux4_tb ();
  reg [3:0] d0, d1, d2, d3;
  reg [1:0] s;
  wire [3:0] y;
  
  mux4 dut (
    . d0(d0),
    . d1(d1),
    . d2(d2),
    . d3(d3),
    . s(s),
    . y(y)
  );
  
  initial begin
    // 设置输入值
    d0 = 4'b0000;
    d1 = 4'b0011;
    d2 = 4'b1100;
    d3 = 4'b1111;
    // 设置选择信号
    s = 2'b00;
#5_000;
    d0 = 4'b0000;
    d1 = 4'b0011;
    d2 = 4'b1100;
    d3 = 4'b1111;
    s = 2'b01;
    #5_000;
    d0 = 4'b0000;
    d1 = 4'b0011;
    d2 = 4'b1100;
    d3 = 4'b1111;
    s = 2'b10;
    #5_000;
    d0 = 4'b0000;
    d1 = 4'b0011;
    d2 = 4'b1100;
    d3 = 4'b1111;
    s = 2'b11;
#5_000;
 $stop;
  end
endmodule

全加器：
module fulladder(input  wire a,b,cin,
output reg s,cout);
reg p,g;
always @ (*) begin

p = a ^ b;
g = a & b;
s = p ^ cin;
cout = g | (p & cin);
end

endmodule
Testbench:
`timescale 1ns/1ns

module fulladder_tb();
  
  reg a, b, cin;
  wire s, cout;
  
  // Instantiate the DUT (Device Under Test)
  fulladder dut (
    . a(a),
    . b(b),
    . cin(cin),
    . s(s),
    . cout(cout)
  );
  initial begin
    // 测试用例 1
    a = 0; b = 0; cin = 0;
    #10_000;
    // 测试用例 2
    a = 0; b = 0; cin = 1;
    #10_000;
    // 测试用例 3
    a = 0; b = 1; cin = 0;
    #10_000;
    // 测试用例 4
    a = 0; b = 1; cin = 1;
    #10_000;
    // 测试用例 5
    a = 1; b = 0; cin = 0;
    #10_000;
    // 测试用例 6
    a = 1; b = 0; cin = 1;
    #10_000;
    // 测试用例 7
    a = 1; b = 1; cin = 0;
    #10_000;
    // 测试用例 8
    a = 1; b = 1; cin = 1;
    #10_000;
    $stop;
  end
endmodule

三态缓冲器：
module tristate (input logic [3:0] a,
input logic en,
output tri [3:0] y);
assign y= en? a: 4'bz;
endmodule
Testbench:
`timescale 1ns/1ns

module tristate_tb();

  // Declare signals
  logic [3:0] a;
  logic en;
  wire [3:0] y;

  tristate dut (
    . a(a),
    . en(en),
    . y(y)
  );


  // Stimulus generation
  initial begin

    // Test case 1
    a = 4'b0000;
    en = 1;
    #10_000;
    // Test case 2
    a = 4'b1111;
    en = 1;
    #10_000;
    // Test case 3
    a = 4'b1010;
    en = 0;
#10_000;
    // Test case 4
    a = 4'b0101;
    en = 0;
    #10_000;
    $stop;
  end
endmodule

寄存器：
module flop( input logic clk，
input logic [3:0] d,
output logic [3:0] q);
always_ff @ (posedge clk)
q<= d; 
endmodule
Testbench:
`timescale 1ns/1ns

module flop_tb();

  // Declare signals
  reg clk;
  reg [3:0] d;
  wire [3:0] q;

  // Instantiate the DUT (Device Under Test)
  flop dut (
    . clk(clk),
    . d(d),
    . q(q)
  );

  // Clock generation
  always #5 clk = ~clk; // 时钟周期为10个时间单位

  initial begin
    // Test case 1
    d = 4'b0000;
    #10_000;
    // Test case 2
    d = 4'b1111;
    #10_000;
    // Test case 3
    d = 4'b1010;
    #10_000;
    // Test case 4
    d = 4'b0101;
    #10_000;
    $stop;
  end
endmodule

同步器：
module sync (input wire clk,
input wire d,
output reg q);
logic n1;
always_ff @ (posedge clk)
begin
n1 <= d;
q <= n1;
end
endmodule
Testbench:
`timescale 1ns/1ns

module sync_tb();

  // Declare signals
  reg clk;
  reg d;
  wire q;

  // Instantiate the DUT (Device Under Test)
  sync dut (
    .clk(clk),
    .d(d),
    .q(q)
  );

  // Clock generation
  always #5 clk = ~clk; // 时钟周期为10个时间单位

  initial begin
    // Test case 1
    d = 1'b0;
    #10_000;
    // Test case 2
    d = 1'b1;
    #10_000;
    // Test case 3
    d = 1'b0;
    #10_000;
    // Test case 4
    d = 1'b1;
    #10_000;

    $stop;
  end

  // Clock driver
  always #5 clk = ~clk;

endmodule

D锁存器：
module latch (input logic clk,
input logic [3:0] d,
output logic [3:0] q);
always_latch
if (clk) q <= d;
endmodule
Testbench:
`timescale 1ns/1ns
module latch_tb();

  // Declare signals
  reg clk;
  reg [3:0] d;
  wire [3:0] q;

  // Instantiate the DUT (Device Under Test)
  latch dut (
    . clk(clk),
    . d(d),
    . q(q)
  );

  // Stimulus generation
  initial begin
    // Test case 1
    d = 4'b0000;
    clk = 0;
    #10_000;
    // Test case 2
    d = 4'b1111;
    clk = 0;
    #10_000;
    // Test case 3
    d = 4'b1010;
    clk = 1;
    #10_000;
    // Test case 4
    d = 4'b0101;
    clk = 1;
    #10_000;
    $stop;
  end

endmodule


计数器（行为级风格）：
module counter (input logic clk,
input logic reset,
output logic [3;0] q);
always_ff @ (posedge clk)
if (reset) q <= 4'b0;
else q<= q+1;
endmodule
Testbench:
`timescale 1ns/1ns
module counter_tb();

  // Declare signals
  reg clk;
  reg reset;
  wire [3:0] q;

  // Instantiate the DUT (Device Under Test)
  counter dut (
    . clk(clk),
    . reset(reset),
    . q(q)
  );

  // Clock generation
  always #5 clk = ~clk; // 时钟周期为10个时间单位

  // Stimulus generation
  initial begin
    // Test case 1
    reset = 1'b1;
    #10_000;
    // Test case 2
    reset = 1'b0;
    #10_000;
    // Test case 3
    #10_000;
    // Test case 4
    reset = 1'b1;
    #10_000;
    // Test case 5
    reset = 1'b0;
    #200;
    $stop;
  end

  // Clock driver
  always #5 clk = ~clk;

endmodule



3:8译码器：
module decoder3_8(input logic [2:0] a,
output logic [7:0] y);
always_comb
case(a)
3’b000: y=8'b00000001;
3'b001: y=8'b00000010;
3'b010: y=8'b00000100;
3'b011: y=8'b00001000;
3'b100: y=8'b00010000;
3'b101: y=8'b00100000;
3'b110: y=8'b01000000;
3'b111: y=8’b10000000:
endcase
endmodule
Testbench:
`timescale 1ns/1ns

module decoder3_8_tb ();

  // Declare signals
  reg [2:0] a;
  wire [7:0] y;

  // Instantiate the DUT (Device Under Test)
  decoder3_8 dut (
    . a(a),
    . y(y)
  );

  // Stimulus generation
  initial begin
    // Test case 1
    a = 3'b000;
    #10_000;
    // Test case 2
    a = 3'b001;
    #10_000;
    // Test case 3
    a = 3'b010;
    #10_000;
    // Test case 4
    a = 3'b011;
    #10_000;
    // Test case 5
    a = 3'b100;
    #10_000;
    // Test case 6
    a = 3'b101;
    #10_000;
    // Test case 7
    a = 3'b110;
    #10_000;
    // Test case 8
    a = 3'b111;
    #10_000;
    $stop;
  end

endmodule




