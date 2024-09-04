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
> Full Adder sem Half Adders:
```VHDL
library ieee;
use ieee.std_logic_1164.all;

entity full_adder is
    port(A, B, Cin: IN std_logic;
        S, Cout: OUT std_logic);
end full_adder;

architecture full of full_adder is

signal w1,w2,w3: std_logic;

begin
    w1 <= A XOR B;
    S <= w1 XOR Cin;
    w2 <= w1 AND Cin;
    w3 <= A AND B;
    Cout <= w2 OR w3;
end full;
```
> Full Adder com Half Adders:
```VHDL
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
