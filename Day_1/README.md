# Day 1: Introduction to Verilog RTL Design & Synthesis

### *SKY130RTL D1SK1 L1 Introduction to iverilog design and test bench*
### Simulator
- A simulator is a software tool that checks your digital circuit’s functionality by applying test inputs and viewing outputs. This helps you verify your design before hardware implementation.
- Simulator is tool used for simulate your design.
- In Day 1, we are using open source simulator iverilog.
- Stimulator is looking for the change in the values of input.
- Output of the stimulator is the vcd file.

<img width="1823" height="862" alt="Screenshot 2025-09-23 112627" src="https://github.com/user-attachments/assets/abd58bb5-e55c-4159-96de-5f0f69059aa9" />


### Design 
- Design is the set of verilog codes which has the intended functionality to meet the specific requirements.

### Test Bench 
- It is setup to apply stimulus to the design to check its functionality.
- Test Bench doesn't have a Primary input or outputs.
- Note, only the design has its Primary input or outputs.

<img width="1637" height="773" alt="Screenshot 2025-09-23 112012" src="https://github.com/user-attachments/assets/2760af3b-d030-48c1-ba2b-aa7d6d72f9e7" />


### GTKWave
- It is used for viewing the waveform.

### SKY130RTL D1SK2 L1 Lab1 introduction to lab
For cloning the git https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git

```bash
sudo -i
sudo apt-get install git
cd /home
# for the vbox users
cd vboxuser 
cd vsd
cd VLSI
git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git
cd sky130RTLDesignAndSynthesisWorkshop
cd my_lib
cd verilog_model/
cd ..
ls
cd ..
cd verilog_files/
ls
cd ..
ls
```

<img width="986" height="849" alt="Screenshot from 2025-09-23 12-13-47" src="https://github.com/user-attachments/assets/e4ffdbff-11d6-4207-ba73-60b710b17b42" />

After  cloning the git

```bash
ls
iverilog good_mux.v tb_good_mux.v
ls
# To dump the vcd file
. /a.out
```
If you want to change the desgin
```bash
gvim tb_good_mux.v -o good_mux.v
```


