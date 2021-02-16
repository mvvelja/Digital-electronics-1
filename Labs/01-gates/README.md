# Lab 01-gates

## GitHub: https://github.com/mvvelja/Digital-electronics-1


## Verification of De Morgan's laws of function f(c,b,a)

![1](Images/1.png)

**VHDL code**
```vhdl
architecture dataflow of gates is
begin
    f_o  <= ((not b_i) and a_i) or ((not c_i) and (not b_i));
    fnand_o <= not (not (not b_i and a_i) and not(not b_i and not c_i));
    fnor_o <= (not (b_i or not a_i)) or (not (c_i or b_i));

end architecture dataflow;
```

### Waveforms

![Signal1](Images/signal1.png)

### Table

| **c** | **b** |**a** | **f(c,b,a)** |
| :-: | :-: | :-: | :-: |
| 0 | 0 | 0 | 1 |
| 0 | 0 | 1 | 1 |
| 0 | 1 | 0 | 0 |
| 0 | 1 | 1 | 0 |
| 1 | 0 | 0 | 0 |
| 1 | 0 | 1 | 1 |
| 1 | 1 | 0 | 0 |
| 1 | 1 | 1 | 0 |

### EDA Playground link: https://www.edaplayground.com/x/GYxg


## Verification of Distributive laws

![2](Images/2.png)

**VHDL code**
```vhdl
architecture dataflow of gates is
begin
    f1_o <= (a_i and b_i)or(a_i and c_i);
	f2_o <= a_i and (b_i or c_i);
	f3_o <= (a_i or b_i) and (a_i or c_i);
	f4_o <= a_i or (b_i and c_i);

end architecture dataflow;
```

### Waveforms

![Signal2](Images/signal2.png)

### EDA Playground link: https://www.edaplayground.com/x/N_pz
