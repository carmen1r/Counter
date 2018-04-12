# Counter
module comparator (input wire [26:0] in0_c,
						 input wire [26:0] in1_c,
						 output reg out0_c
						 );

						 
always @(*)
begin
	if (in0_c >= in1_c)  
		begin
			out0_c <= 1;
		end
	else
		begin
			out0_c <= 0;
		end
end

endmodule

module transcoder ( input wire [3:0] in0_t, 
						  output reg [6:0] out0_t 
						 );

always @(*) 
begin 
	case (in0_t) 
		0: 
		begin 
			out0_t = 7'b0000001; 
	   end

		1: 
		begin 
			out0_t = 7'b1001111; 
		end
		
		2: 
		begin 
			out0_t = 7'b0010010; 
		end
		
		3: 
		begin 
			out0_t = 7'b0000110; 
		end
		
		4: 
		begin 
			out0_t = 7'b1001100; 
		end
		
		5: 
		begin 
			out0_t = 7'b0100100; 
		end
		
		6: 
		begin 
			out0_t = 7'b0100000; 
		end
		
		7: 
		begin 
			out0_t = 7'b0001111; 
		end
		
		8: 
		begin 
			out0_t = 7'b0000000; 
		end
		
		9: 
		begin 
			out0_t = 7'b0000100; 
		end
		
		11: 
		begin 
			out0_t = 7'b0001000; 
		end
		
		12: 
		begin 
			out0_t = 7'b1100000; 
		end
		
		13: 
		begin 
			out0_t = 7'b0110001; 
		end
		
		14: 
		begin 
			out0_t = 7'b1000010; 
		end
		
		15: 
		begin 
			out0_t = 7'b0110000; 
		end
		
		16: 
		begin 
			out0_t = 7'b0111000; 
		end
endcase
end

endmodule

module counter ( input enable,
					  input ud,
					  input clk,
					  input rst,
					  output reg val
					 );
			
always @(posedge clk)
begin
	if (rst == 1)
		begin
			val <= 0;
		end
	else
		begin
			if (enable == 1)
			 begin
				if (ud == 1)
					begin
						val <= val + 1;
					end
				else
					begin
						val <= val - 1;
					end
			 end
		end
end
endmodule
		
module top ( input clk_top,
				 input up2_top,
				 input lim_top,
				 output wire [6:0] out0_top
				);

wire [26:0] c1_out;
wire [26:0] c2_out;
wire comp1_out;
wire comp2_out;

counter c1 ( .enable(1),
				 .ud(0),
				 .clk(clk_top),
				 .val(c1_out)
				);
				
counter c2 ( .enable(c1_out),
				 .ud(up2_top),
				 .clk(clk_top),
				 .val(c2_out)
				);
				
comparator comp1 ( .in0_c(c1_out),
						 .in1_c(10),
						 .out0_c(comp1_out)
						);
						
comparator comp2 ( .in0_c(c2_out),
						 .in1_c(lim_top),
						 .out0_c(comp2_out)
						);
						
transcoder t1 ( .in0_t(c2_out),
					 .out0_t(out0_top)
					);

endmodule
