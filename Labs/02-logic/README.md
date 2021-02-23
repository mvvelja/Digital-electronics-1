# Lab 02-logic

## GitHub: 

https://github.com/mvvelja/Digital-electronics-1

## 2-bit comparator truth table

| **Dec. equivalent** | **B[1:0]** | **A[1:0]** | **B is greater than A** | **B equals A** | **B is less than A** |
| :-: | :-: | :-: | :-: | :-: | :-: |
| 0 | 0 0 | 0 0 | 0 | 1 | 0 |
| 1 | 0 0 | 0 1 | 0 | 0 | 1 |
| 2 | 0 0 | 1 0 | 0 | 0 | 1 |
| 3 | 0 0 | 1 1 | 0 | 0 | 1 |
| 4 | 0 1 | 0 0 | 1 | 0 | 0 |
| 5 | 0 1 | 0 1 | 0 | 1 | 0 |
| 6 | 0 1 | 1 0 | 0 | 0 | 1 |
| 7 | 0 1 | 1 1 | 0 | 0 | 1 |
| 8 | 1 0 | 0 0 | 1 | 0 | 0 |
| 9 | 1 0 | 0 1 | 1 | 0 | 0 |
| 10 | 1 0 | 1 0 | 0 | 1 | 0 |
| 11 | 1 0 | 1 1 | 0 | 0 | 1 |
| 12 | 1 1 | 0 0 | 1 | 0 | 0 |
| 13 | 1 1 | 0 1 | 1 | 0 | 0 |
| 14 | 1 1 | 1 0 | 1 | 0 | 0 |
| 15 | 1 1 | 1 1 | 0 | 1 | 0 |

## 2-bit comparator

### Karnaugh maps for all functions

The K-map for the "equals" function is as follows:

![1](Images/1.png)

K-map for the SoP form of the "greater than" function:

![2](Images/2.png)

K-map for the PoS form of the "less than" function:

![3](Images/3.png)

Equations of simplified SoP form of the "greater than" function and simplified PoS form of the "less than" function:

![4](Images/4.png)

### EDA Playground link: 
https://www.edaplayground.com/x/HemP

## 4-bit comparator

### Listing of VHDL architecture from design file:

**VHDL code**
```vhdl
architecture Behavioral of comparator_2bit is
begin

    B_greater_A_o   <= '1' when (b_i > a_i) else '0';
    B_equals_A_o    <= '1' when (b_i = a_i) else '0';
    B_less_A_o      <= '1' when (b_i < a_i) else '0';
    
end architecture Behavioral;
```
### Listing of VHDL architecture from testbench file:

**VHDL code**
```vhdl
p_stimulus : process
begin
    -- Report a note at the begining of stimulus process
    report "Stimulus process started" severity note;
    -- First test values
    s_b <= "0000"; s_a <= "0000"; wait for 100 ns;
    -- Expected output
    assert ((s_B_greater_A = '0') and (s_B_equals_A = '1') and (s_B_less_A = '0'))
    -- If false, then report an error
    report "Test failed for input combination: 0000, 0000" severity error;
    
	s_b <= "0000"; s_a <= "0001"; wait for 100 ns;
	assert ((s_B_greater_A = '0') and (s_B_equals_A = '0') and (s_B_less_A = '1'))
	report "Test failed for input combination: 0000, 0001" severity error;
	s_b <= "0000"; s_a <= "0010"; wait for 100 ns;
	assert ((s_B_greater_A = '0') and (s_B_equals_A = '0') and (s_B_less_A = '1'))
	report "Test failed for input combination: 0000, 0010" severity error;
	s_b <= "0000"; s_a <= "0011"; wait for 100 ns;
	assert ((s_B_greater_A = '0') and (s_B_equals_A = '0') and (s_B_less_A = '1'))
	report "Test failed for input combination: 0000, 0011" severity error;
	s_b <= "0001"; s_a <= "0000"; wait for 100 ns;
	assert ((s_B_greater_A = '1') and (s_B_equals_A = '0') and (s_B_less_A = '0'))
	report "Test failed for input combination: 0001, 0000" severity error;
	s_b <= "0001"; s_a <= "0001"; wait for 100 ns;
	assert ((s_B_greater_A = '0') and (s_B_equals_A = '1') and (s_B_less_A = '0'))
	report "Test failed for input combination: 0001, 0001" severity error;
	s_b <= "0001"; s_a <= "0010"; wait for 100 ns;
	assert ((s_B_greater_A = '0') and (s_B_equals_A = '0') and (s_B_less_A = '1'))
	report "Test failed for input combination: 0001, 0010" severity error;
	s_b <= "0001"; s_a <= "0011"; wait for 100 ns;
	assert ((s_B_greater_A = '0') and (s_B_equals_A = '0') and (s_B_less_A = '1'))
	report "Test failed for input combination: 0001, 0011" severity error;
	s_b <= "0010"; s_a <= "0000"; wait for 100 ns;
	assert ((s_B_greater_A = '1') and (s_B_equals_A = '0') and (s_B_less_A = '0'))
	report "Test failed for input combination: 0010, 0000" severity error;
	s_b <= "0010"; s_a <= "0001"; wait for 100 ns;
	assert ((s_B_greater_A = '1') and (s_B_equals_A = '0') and (s_B_less_A = '0'))
	report "Test failed for input combination: 0010, 0001" severity error;
	s_b <= "0010"; s_a <= "0010"; wait for 100 ns;
	assert ((s_B_greater_A = '0') and (s_B_equals_A = '1') and (s_B_less_A = '0'))
	report "Test failed for input combination: 0010, 0010" severity error;
	s_b <= "0010"; s_a <= "0011"; wait for 100 ns;
	assert ((s_B_greater_A = '0') and (s_B_equals_A = '0') and (s_B_less_A = '1'))
	report "Test failed for input combination: 0010, 0011" severity error;
	s_b <= "0011"; s_a <= "0000"; wait for 100 ns;
	assert ((s_B_greater_A = '1') and (s_B_equals_A = '0') and (s_B_less_A = '0'))
	report "Test failed for input combination: 0011, 0000" severity error;
	s_b <= "0011"; s_a <= "0001"; wait for 100 ns;
	assert ((s_B_greater_A = '1') and (s_B_equals_A = '0') and (s_B_less_A = '0'))
	report "Test failed for input combination: 0011, 0001" severity error;
	s_b <= "0011"; s_a <= "0010"; wait for 100 ns;
	assert ((s_B_greater_A = '1') and (s_B_equals_A = '0') and (s_B_less_A = '0'))
	report "Test failed for input combination: 0011, 0010" severity error;
	s_b <= "0011"; s_a <= "0011"; wait for 100 ns;
	assert ((s_B_greater_A = '0') and (s_B_equals_A = '1') and (s_B_less_A = '0'))
	report "Test failed for input combination: 0011, 0011" severity error;
	s_b <= "0100"; s_a <= "0000"; wait for 100 ns;
	assert ((s_B_greater_A = '1') and (s_B_equals_A = '0') and (s_B_less_A = '1')) --error
	report "Test failed for input combination: 0100, 0000" severity error;
    -- Report a note at the end of stimulus process
    report "Stimulus process finished" severity note;
    wait;
end process p_stimulus;
```

### Simulator with one reported error

![5](Images/5.PNG)

### EDA Playground link: 
https://www.edaplayground.com/x/fnhD
