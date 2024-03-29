# Lab 07-ffs

### GitHub: 

https://github.com/mvvelja/Digital-electronics-1

### Characteristic equations and completed tables for D, JK, T flip-flops

![1](Images/1.png)

| **D** | **Qn** | **Q(n+1)** | **Comments** |
| :-: | :-: | :-: | :-- |
| 0 | 0 | 0 | No change |
| 0 | 1 | 0 | Change |
| 1 | 1 | 1 | No change |
| 1 | 0 | 1 | Change |

| **J** | **K** | **Qn** | **Q(n+1)** | **Comments** |
| :-: | :-: | :-: | :-: | :-- |
| 0 | 0 | 0 | 0 | No change |
| 0 | 0 | 1 | 1 | No change |
| 0 | 1 | 0 | 0 | Reset |
| 0 | 1 | 1 | 0 | Reset |
| 1 | 0 | 0 | 1 | Set |
| 1 | 0 | 1 | 1 | Set |
| 1 | 1 | 0 | 1 | Toggle |
| 1 | 1 | 1 | 0 | Toggle |

| **T** | **Qn** | **Q(n+1)** | **Comments** |
| :-: | :-: | :-: | :-- |
| 0 | 0 | 0 | No change |
| 0 | 1 | 1 | No change |
| 1 | 0 | 1 | Toggle (invert) |
| 1 | 1 | 0 | Toggle (invert) |

## D latch

### VHDL code listing of the process p_d_latch with syntax highlighting

**VHDL code of p_d_latch.vhd**
```vhdl
p_d_latch : process (d, arst, en)
   begin
       if (arst = '1') then
           q <= '0';
           q_bar <= '1';
       elsif(en = '1') then 
           q <= d;
           q_bar <= not d;    
       end if;
end process p_d_latch;
```
### Listing of VHDL reset and stimulus processes from the testbench tb_d_latch.vhd file with syntax highlighting and asserts

**VHDL code of tb_d_latch.vhd**
```vhdl
-- reset generation process
p_reset_gen: process
begin
      s_arst <= '0';
      wait for 30 ns;
      -- reset activated
      s_arst <= '1';
      wait for 50ns;
      -- reset deactivated
      s_arst <= '0';
      wait for 300ns;
      s_arst <= '1';
      wait;
end process p_reset_gen;
-- data generation process
p_stimulus : process
  begin
      report "Stimulus process started" severity note;
      s_en    <= '0';
      s_d     <= '0';
      wait for 10 ns;
      s_d <= '1';
      wait for 10 ns;
      s_d <= '0';
      wait for 10 ns;
      s_d <= '1';
      wait for 10 ns;
      s_d <= '0';
      wait for 10 ns;
      s_d <= '1';
      wait for 10 ns;
      s_d <= '0';
      s_en <= '1'; wait for 5 ns;
      assert(s_q = '0' and s_q_bar = '1')
      report "expected: s_q 0, q_bar 1" severity error;
      
      s_d <= '1';
      wait for 10 ns;
      s_d <= '0';
      wait for 10 ns;
      s_d <= '0';
      wait for 10 ns;
      s_d <= '0';
      wait for 10 ns;
      s_d <= '1';
      wait for 10 ns;
      s_d <= '1';
      wait for 10 ns;
      s_d <= '1';
      
      s_en <= '0'; wait for 5 ns;
      assert(s_q = '1' and s_q_bar = '0') 
      report "expected: asrt 1" severity error;
      
      wait for 10 ns;
      s_d <= '1';
      wait for 10 ns;
      s_d <= '0';
      wait for 10 ns;
      s_d <= '1';
      wait for 10 ns;
      s_d <= '0';
      wait for 10 ns;
      s_d <= '1';
      wait for 10 ns;
      s_d <= '0';
      wait for 10 ns;
      s_d <= '0';      
      
      s_en <= '1'; wait for 5 ns;
      assert(s_q = '0' and s_q_bar = '1') 
      report "expected: asrt 1" severity error;
   
      report "Stimulus process finished" severity note;
      wait;
  end process p_stimulus;
```
### Screenshot with simulated time waveforms

![2](Images/2.png)

## Flip-flops

### VHDL code listing of the processes p_d_ff_arst, p_d_ff_rst, p_jk_ff_rst, p_t_ff_rst with syntax highlighting

**VHDL code of p_d_ff_arst.vhd**
```vhdl
p_d_ff_arst : process (clk, arst)                    
   begin                                             
       if (arst = '1') then                          
           q <= '0';                                 
           q_bar <= '1';                             
       elsif rising_edge(clk) then                        
           q <= d;                                   
           q_bar <= not d;                           
       end if;                                       
    end process p_d_ff_arst;
```

**VHDL code of p_d_ff_rst.vhd**
```vhdl
p_d_ff_rst : process (clk)                    
    begin
        if rising_edge(clk) then
            if (rst = '1')then
                q <= '0';
                q_bar <= '1';
            else
                q <= d;
                q_bar <= not d;
            end if; 
        end if;                                           
end process p_d_ff_rst;
```

**VHDL code of p_jk_ff_rst.vhd**
```vhdl
p_jk_ff_rst : process (clk)
begin
    if rising_edge(clk) then
        if (rst = '1') then
            s_q <= '0';
        else
            if (j = '0' and k = '0') then
                s_q <= s_q;
            elsif (j = '0' and k = '1') then
                s_q <= '0';
            elsif (j = '1' and k = '0') then
                s_q <= '1';
            elsif (j = '1' and k = '1') then
                s_q <= not s_q;
            end if;
        end if;
    end if;
end process p_jk_ff_rst;
```

**VHDL code of p_t_ff_rst.vhd**
```vhdl
architecture Behavioral of p_t_ff_rst is
    signal  s_q     :   STD_LOGIC;
    signal  s_q_bar :   STD_LOGIC;
begin
    p_t_ff_rst : process(clk)
    begin
        if rising_edge(clk) then
            if (rst = '1') then
                s_q     <=  '0';
                s_q_bar <=  '1'; 
             else
                if (t = '0') then
                    s_q     <=  s_q;
                    s_q_bar <=  s_q_bar;
                else
                    s_q     <=  not s_q;
                    s_q_bar <=  not s_q_bar;
                end if;
             end if;
        end if;
    end process p_t_ff_rst;
    q       <=  s_q;
    q_bar   <=  s_q_bar;
end Behavioral;
```

### Listing of VHDL clock, reset and stimulus processes from the testbench files with syntax highlighting and asserts

```vhdl
-- clock
p_clk_gen : process
begin
    while now < 750 ns loop         -- 75 periods of 100MHz clock
        s_clk <= '0';
        wait for c_CLK_100MHZ_PERIOD / 2;
        s_clk <= '1';
        wait for c_CLK_100MHZ_PERIOD / 2;
    end loop;
    wait;                           
end process p_clk_gen;

-- reset
p_reset_gen : process 
begin
    s_arst <= '0';
    wait for 30 ns;
    s_arst <= '1';
    wait for 15 ns;
    s_arst <= '0';                
    wait;
end process p_reset_gen;

-- stimulus
p_stimulus : process
begin
    report "Stimulus process started" severity note;
    
    s_d <= '1';
    wait for 10ns;
    assert (s_q = '0' and s_q_bar = '1')
    report "Error" severity note;
    
    s_d <= '0';
    wait for 10ns;
    assert (s_q = '0' and s_q_bar = '1')
    report "Error" severity note;
    
    s_d <= '1';
    wait for 10ns;
    assert (s_q = '1' and s_q_bar = '0')
    report "Error" severity note;
   
    s_d <= '0';
    wait for 10ns;
    assert (s_q = '0' and s_q_bar = '1')
    report "Error" severity note;
    
    wait for 20ns;
    s_d <= '1';
    wait for 10ns;
    assert (s_q = '0' and s_q_bar = '1')
    report "Error" severity note;

    report "Stimulus process finished" severity note;
    wait;
end process p_stimulus;
 -- clock
p_clk_gen : process
begin
    while now < 750 ns loop         
        s_clk <= '0';
        wait for c_CLK_100MHZ_PERIOD / 2;
        s_clk <= '1';
        wait for c_CLK_100MHZ_PERIOD / 2;
    end loop;
    wait;                           
end process p_clk_gen;

-- reset
p_reset_gen : process 
begin
    s_rst <= '0';
    wait for 30 ns;
    s_rst <= '1';
    wait for 15 ns;
    s_rst <= '0';                
    wait;
end process p_reset_gen;

-- stimulus
p_stimulus : process
begin
    report "Stimulus process started" severity note;
    
    s_d <= '1';
    wait for 10ns;
    assert (s_q = '1' and s_q_bar = '0')
    report "Error" severity note;
    
    s_d <= '0';
    wait for 10ns;
    assert (s_q = '0' and s_q_bar = '1')
    report "Error" severity note;
    
    s_d <= '1';
    wait for 10ns;
    assert (s_q = '0' and s_q_bar = '1')
    report "Error" severity note;
   
    s_d <= '0';
    wait for 10ns;
    assert (s_q = '0' and s_q_bar = '1')
    report "Error" severity note;
    
    wait for 20ns;
    s_d <= '1';
    wait for 10ns;
    assert (s_q = '1' and s_q_bar = '0')
    report "Error" severity note;

    report "Stimulus process finished" severity note;   
    wait;
end process p_stimulus;
-- clock
p_clk_gen : process
begin
    while now < 750 ns loop         
        s_clk <= '0';
        wait for c_CLK_100MHZ_PERIOD / 2;
        s_clk <= '1';
        wait for c_CLK_100MHZ_PERIOD / 2;
    end loop;
    wait;                           
end process p_clk_gen;

-- reset
p_reset_gen : process 
begin
    s_rst <= '0';
    wait for 30 ns;
    s_rst <= '1';
    wait for 15 ns;
    s_rst <= '0';                
    wait;
end process p_reset_gen;

-- stimulus
p_stimulus : process
begin
    report "Stimulus process started" severity note;
    s_j <= '1';
    s_k <= '0';
    wait for 10ns;
    assert (s_q = '1' and s_q_bar = '0')
    report "Error" severity note;
    
    s_j <= '0';
    s_k <= '1';
    wait for 10ns;
    assert (s_q = '0' and s_q_bar = '1')
    report "Error" severity note;
    
    s_j <= '1';
    s_k <= '0';
    wait for 10ns;
    assert (s_q = '1' and s_q_bar = '0')
    report "Error" severity note;
   
    s_j <= '0';
    s_k <= '1';
    wait for 10ns;
    assert (s_q = '0' and s_q_bar = '1')
    report "Error" severity note;  
    
    s_j <= '1';
    s_k <= '1';
    wait for 10ns;
    assert (s_q = '1' and s_q_bar = '0')
    report "Error" severity note;
    
    s_j <= '1';
    s_k <= '1';
    wait for 10ns;
    assert (s_q = '0' and s_q_bar = '1')
    report "Error" severity note;
    
    s_j <= '1';
    s_k <= '1';
    wait for 10ns;
    assert (s_q = '1' and s_q_bar = '0')
    report "Error" severity note;
    
    s_j <= '0';
    s_k <= '0';
    wait for 10ns;
    assert (s_q = '1' and s_q_bar = '0')
    report "Error" severity note;

    report "Stimulus process finished" severity note;   
    wait;
end process p_stimulus;
-- clock
p_clk_gen : process
begin
    while now < 750 ns loop         
        s_clk <= '0';
        wait for c_CLK_100MHZ_PERIOD / 2;
        s_clk <= '1';
        wait for c_CLK_100MHZ_PERIOD / 2;
    end loop;
    wait;                           
end process p_clk_gen;

-- reset
p_reset_gen : process 
begin
    s_rst <= '0';
    wait for 30 ns;
    s_rst <= '1';
    wait for 15 ns;
    s_rst <= '0';                
    wait;
end process p_reset_gen;

-- stimulus
p_stimulus : process
begin
    report "Stimulus process started" severity note;
    
    wait for 20ns;
    s_t <= '1';
    wait for 10ns;
    assert (s_q = '1' and s_q_bar = '0')
    report "Error" severity note;
    
    s_t <= '0';
    wait for 10ns;
    assert (s_q = '1' and s_q_bar = '0')
    report "Error" severity note;
    
    s_t <= '1';
    wait for 10ns;
    assert (s_q = '0' and s_q_bar = '1')
    report "Error" severity note;
    
    s_t <= '0';
    wait for 10ns;
    assert (s_q = '0' and s_q_bar = '1')
    report "Error" severity note;
    
    s_t <= '1';
    wait for 10ns;
    assert (s_q = '1' and s_q_bar = '0')
    report "Error" severity note;
    
    s_t <= '0';
    wait for 10ns;
    assert (s_q = '1' and s_q_bar = '0')
    report "Error" severity note;

    report "Stimulus process finished" severity note;      
    wait;
end process p_stimulus;
```

### Screenshot with simulated time waveforms

#### p_d_ff_arst
![3](Images/3.png)

#### p_d_ff_rst
![4](Images/4.png)

#### p_jk_ff_rst
![5](Images/5.png)

#### p_t_ff_rst
![6](Images/6.png)

## Shift register

![7](Images/7.png)