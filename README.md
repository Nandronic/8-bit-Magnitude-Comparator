# 8-bit Magnitude Comparator (Verilog)

A compact, synthesizable Verilog design that compares two 8-bit numbers and asserts one of three outputs to indicate **A > B**, **A < B**, or **A == B**. Includes a simple testbench, guidance for simulation (CLI + EDA Playground), and a summary of UVM verification and OpenROAD physical design results.

---

## ✨ Features
- Purely **combinational** RTL (no clock needed)
- Simple, readable relational-operator-based implementation
- Clean, self-explanatory outputs: `A_greater`, `A_less`, `A_equal`
- Reference **testbench** with example vectors and `$display` logs
- Optional **UVM**-style environment sketch for block-level verification
- Example **OpenROAD** flow notes (ASAP7) for layout/GDS generation

---

## 📦 Module Interface
```verilog
module magnitude_comparator_8bit (
    input  [7:0] A,
    input  [7:0] B,
    output       A_greater,
    output       A_less,
    output       A_equal
);
```

### I/O Description
- `A[7:0]`, `B[7:0]` — Unsigned 8-bit inputs to compare
- `A_greater` — High when `A > B`
- `A_less` — High when `A < B`
- `A_equal` — High when `A == B`

> Only one of the three outputs should be high for valid input pairs.

---

## 🧠 Design Logic (RTL)
```verilog
module magnitude_comparator_8bit (
    input  [7:0] A, B,
    output       A_greater,
    output       A_less,
    output       A_equal
);
    assign A_greater = (A > B)  ? 1'b1 : 1'b0;
    assign A_less    = (A < B)  ? 1'b1 : 1'b0;
    assign A_equal   = (A == B) ? 1'b1 : 1'b0;
endmodule
```

---

## 🧪 Testbench
```verilog
`timescale 1ns/1ps
module tb_magnitude_comparator_8bit;
  reg  [7:0] A, B;
  wire       A_greater, A_less, A_equal;

  magnitude_comparator_8bit dut (
    .A(A), .B(B),
    .A_greater(A_greater), .A_less(A_less), .A_equal(A_equal)
  );

  initial begin
    A = 8'h00; B = 8'h00; #10;

    // A > B
    A = 8'h0F; B = 8'h03; #10;
    $display("A=%b B=%b | G=%b L=%b E=%b", A,B,A_greater,A_less,A_equal);

    // A < B
    A = 8'h03; B = 8'h0F; #10;
    $display("A=%b B=%b | G=%b L=%b E=%b", A,B,A_greater,A_less,A_equal);

    // A == B
    A = 8'hAA; B = 8'hAA; #10;
    $display("A=%b B=%b | G=%b L=%b E=%b", A,B,A_greater,A_less,A_equal);

    $finish;
  end
endmodule
```

---

## ▶️ Simulation (Icarus Verilog)
```bash
iverilog -o comp8.vvp magnitude_comparator_8bit.v tb_magnitude_comparator_8bit.v
vvp comp8.vvp
```

---

## 👤 Author
**Nandan U**  
ECE, VVIET Mysuru

---

## License
This project is provided for educational use. You may copy, modify, and distribute with attribution.
