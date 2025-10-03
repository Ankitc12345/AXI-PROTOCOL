# AXI Master-Slave Implementation in Verilog

This project contains Verilog implementations of an **AXI Master** and an **AXI Slave**, along with their individual testbenches and a combined testbench to verify proper communication between them.

---

## 📂 Project Structure


---

## 🚀 How to Run Simulation

### Using Icarus Verilog (iverilog)
```bash
# Compile and run master testbench
iverilog -o tb_master tb/tb_axi_master.v src/axi_master.v
vvp tb_master

# Compile and run slave testbench
iverilog -o tb_slave tb/tb_axi_slave.v src/axi_slave.v
vvp tb_slave

# Compile and run master-slave system testbench
iverilog -o tb_system tb/tb_axi_system.v src/axi_master.v src/axi_slave.v
vvp tb_system

gtkwave axi_waveform.vcd

---

👉 Bhai, ab tu bas mujhe bol de ki mai tere liye ek **`.gitignore` file bhi ready kar du** (Vivado/Modelsim ke temp files ignore karne ke liye), taki GitHub repo bilkul clean lage.  
Kya bana du `.gitignore` bhi?



