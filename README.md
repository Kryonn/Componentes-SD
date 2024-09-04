# Componentes de Sistemas Digitais

Nesse repositório serão encontrados os módulos, em VHDL, de vários componentes lógicos.

## Half Adder
```VHDL
LIBRARY ieee;
USE ieee.std_logic_1164.all;

Entity half_adder is
    port(A, B: IN std_logic;
    S, Cout: OUT std_logic);
end half_adder;

architecture half of half_adder is
begin
      S <= A XOR B;
      Cout <= A AND B;
end half;
```

## Full Adder
> obs: o Full adder foi implementado usando Half adders, então é necessário ter o código do Half adder.
```VHDL
VHDL

LIBRARY ieee;
USE ieee.std_logic_1164.all;

Entity full_adder is
    port(A, B, Cin: IN std_logic;
        S, Cout: OUT std_logic);
end full_adder;

Architecture full of full_adder is
    component half_adder
        port(A, B: IN std_logic;
            S, Cout: OUT std_logic);
    end component;
    
signal w1: std_logic;
signal w2: std_logic;
signal w3: std_logic;
    
begin
    ha1: half_adder
    port map(A => A,
            B => B,
            S => w1,
            Cout => w2);
            
    ha2: half_adder
    port map(A => Cin,
            B => w1,
            S => S,
            Cout => w3);
            
    Cout <= w2 OR w3;
end full;
```
