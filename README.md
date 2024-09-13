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
## Contadores
> Contador 16 bits:
```VHDL
library ieee;
use ieee.std_logic_1164.all;
use ieee.std_logic_unsigned.all;

entity counter_16bits is
    port(clk, rst: in std_logic;
        q: out std_logic_vector(15 downto 0));
end counter_16bits;

architecture behavior of counter_16bits is

    signal count: std_logic_vector(15 downto 0);  

begin    
    process(clk, rst)
    begin
        if(falling_edge(clk)) then
            if(rst = '1') then
                count <= (others => '0');
            else
                count <= count + 1;
            end if;
        end if;
    end process;
    
    q <= count;
    
end behavior;                              
```
> Contador de 16 bits com display(t1):
```VHDL
library ieee;
use ieee.std_logic_1164.all;
use ieee.numeric_std.all;

entity counter_16bits is
    port(clk, rst: in std_logic;
        d: out std_logic_vector(27 downto 0));
end counter_16bits;

architecture behavior of counter_16bits is

    component display
        port(s0, s1, s2, s3: in std_logic;
            p0, p1, p2, p3, p4, p5, p6: out std_logic);
    end component;

    signal count: std_logic_vector(15 downto 0);  

begin    
    process(clk, rst)
    begin
        if(falling_edge(clk)) then
            if(rst = '1') then
                count <= (others => '0');
            else
                count <= std_logic_vector(unsigned(count) + 1);
            end if;
        end if;
    end process;

    inst1: display
    port map(s0 => count(0),
            s1 => count(1),
            s2 => count(2),
            s3 => count(3),
            p0 => d(0),
            p1 => d(1),
            p2 => d(2),
            p3 => d(3),
            p4 => d(4),
            p5 => d(5),
            p6 => d(6));

    inst2: display
    port map(s0 => count(4),
            s1 => count(5),
            s2 => count(6),
            s3 => count(7),
            p0 => d(7),
            p1 => d(8),
            p2 => d(9),
            p3 => d(10),
            p4 => d(11),
            p5 => d(12),
            p6 => d(13));
    
    inst3: display
    port map(s0 => count(8),
            s1 => count(9),
            s2 => count(10),
            s3 => count(11),
            p0 => d(14),
            p1 => d(15),
            p2 => d(16),
            p3 => d(17),
            p4 => d(18),
            p5 => d(19),
            p6 => d(20));

    inst4: display
    port map(s0 => count(12),
            s1 => count(13),
            s2 => count(14),
            s3 => count(15),
            p0 => d(21),
            p1 => d(22),
            p2 => d(23),
            p3 => d(24),
            p4 => d(25),
            p5 => d(26),
            p6 => d(27));
    
end behavior;
```
> Contador de 16 bits com display(t2):
```VHDL
library ieee;
use ieee.std_logic_1164.all;
use ieee.numeric_std.all;

entity counter_16bits is
    port(clk, rst: in std_logic;
        d: out std_logic_vector(27 downto 0));
end counter_16bits;

architecture behavior of counter_16bits is

    component display
        port(s0, s1, s2, s3: in std_logic;
            p0, p1, p2, p3, p4, p5, p6: out std_logic);
    end component;

    signal counter: integer range 0 to 65535;  
    signal count: std_logic_vector (15 downto 0);

begin    
    process(clk, rst)
    begin
        if(falling_edge(clk)) then
            if(rst = '1') then
                counter <= 0;
            else
                counter <= counter + 1;
            end if;
        end if;
    end process;

    count <= std_logic_vector(to_unsigned(counter, 16));

    inst1: display
    port map(s0 => count(0),
            s1 => count(1),
            s2 => count(2),
            s3 => count(3),
            p0 => d(0),
            p1 => d(1),
            p2 => d(2),
            p3 => d(3),
            p4 => d(4),
            p5 => d(5),
            p6 => d(6));

    inst2: display
    port map(s0 => count(4),
            s1 => count(5),
            s2 => count(6),
            s3 => count(7),
            p0 => d(7),
            p1 => d(8),
            p2 => d(9),
            p3 => d(10),
            p4 => d(11),
            p5 => d(12),
            p6 => d(13));
    
    inst3: display
    port map(s0 => count(8),
            s1 => count(9),
            s2 => count(10),
            s3 => count(11),
            p0 => d(14),
            p1 => d(15),
            p2 => d(16),
            p3 => d(17),
            p4 => d(18),
            p5 => d(19),
            p6 => d(20));

    inst4: display
    port map(s0 => count(12),
            s1 => count(13),
            s2 => count(14),
            s3 => count(15),
            p0 => d(21),
            p1 => d(22),
            p2 => d(23),
            p3 => d(24),
            p4 => d(25),
            p5 => d(26),
            p6 => d(27));
    
    
end behavior;
```
>Contador de 1 segundo:
```VHDL
library ieee;
use ieee.std_logic_1164.all;
use ieee.numeric_std.all;

entity counter_1sec is
    port(clk, rst: in std_logic;
        q: out std_logic_vector(3 downto 0));
end counter_1sec;

architecture behavior of counter_1sec is

    signal counter: std_logic_vector(26 downto 0) := (others => '0');
    signal counter_q: std_logic_vector(3 downto 0) := (others => '0');
    signal clk_1sec: std_logic := '0';
    constant max: unsigned(26 downto 0) := to_unsigned(50000000-1, 27);

begin
    process(clk)
    begin
        if(falling_edge(clk)) then 
            if(unsigned(counter) = max) then
                counter <= (others => '0');
                clk_1sec <= not clk_1sec;
            else
                counter <= std_logic_vector(unsigned(counter)+1);
            end if;
        end if;
    end process;
    
    process(clk_1sec, rst) then
    begin
        if(falling_edge(clk_1sec)) then
            if(rst = '1' or counter_q = '1001') then
                counter_q <= (others => '0');
            else
                counter_q <= std_logic_vector(unsigned(counter_q)+1);
            end if;
        end if;
    end process;
    q <= counter_q;
end behavior;
```
## Rotator
> Rotator de 4 bits (primeira tentativa):
```VHDL
library ieee;
use ieee.std_logic_1164.all;

entity rotator is
    port(clk, rst: in std_logic;
        d: out std_logic_vector(27 downto 0));
end rotator;

architecture rot of rotator is

    component display
        port(s0, s1, s2, s3: in std_logic;
            p0, p1, p2, p3, p4, p5, p6: out std_logic);
    end component;

    signal clk_1sec: std_logic := '0';
    constant ac: integer range 0 to 49999999 := 49999999;
    variable counter1: integer range 0 to 49999999 := 0;
    variable counter2: integer range 0 to 3 := 0;
    signal ini: std_logic_vector(15 downto 0) := "1101111000010000";
    signal n1: std_logic_vector(15 downto 0) := "1110000100001101";
    signal n2: std_logic_vector(15 downto 0) := "0001000011011110";
    signal n3: std_logic_vector(15 downto 0) := "0000110111100001";
    signal q: std_logic_vector(15 downto 0) := "0000000000000000";
    
begin
    process(clk)
    begin
        if(rising_edge(clk))then
            if(counter1 = ac) then
                clk_1sec <= not clk_1sec;
                counter1 <= 0;
            else
                counter1 <= counter1 + 1;
            end if;
        end if;
    end process;

    process(clk_1sec, rst)
    begin
        if(rst = '1') then
            counter2 <= 0;
        elsif (rising_edge(clk_1sec)) then
            if(counter2 = 3) then
                counter2 <= 0;
            else
                counter2 <= counter2 + 1;
            end if;
        end if;
    end process;

    process(counter2)
    begin
        case (counter2) is
            when 0 =>
                q <= ini;
            when 1 =>
                q <= n1;
            when 2 =>
                q <= n2;
            when 3 =>
                q <= n3;
        end case;
    end process;

    inst1: display
    port map(s0 => q(0),
             s1 => q(1),
             s2 => q(2),
             s3 => q(3),
             p0 => d(0),
             p1 => d(1),
             p2 => d(2),
             p3 => d(3),
             p4 => d(4),
             p5 => d(5),
             p6 => d(6));

    inst2: display
    port map(s0 => q(4),
             s1 => q(5),
             s2 => q(6),
             s3 => q(7),
             p0 => d(7),
             p1 => d(8),
             p2 => d(9),
             p3 => d(10),
             p4 => d(11),
             p5 => d(12),
             p6 => d(13));

    inst3: display
    port map(s0 => q(8),
             s1 => q(9),
             s2 => q(10),
             s3 => q(11),
             p0 => d(14),
             p1 => d(15),
             p2 => d(16),
             p3 => d(17),
             p4 => d(18),
             p5 => d(19),
             p6 => d(20));
            
    inst4: display
    port map(s0 => q(12),
             s1 => q(13),
             s2 => q(14),
             s3 => q(15),
             p0 => d(21),
             p1 => d(22),
             p2 => d(23),
             p3 => d(24),
             p4 => d(25),
             p5 => d(26),
             p6 => d(27));
    
end rot;
```
> Rotator de 6 bits(primeira tentativa):
```VHDL
library ieee;
use ieee.std_logic_1164.all;

entity rotator6bits is
    port(rst, clk: in std_logic;
        d: out std_logic_vector(41 downto 0));
end rotator6bits;

architecture rot of rotator6bits is

    component display
        port(s0, s1, s2, s3: in std_logic;
            p0, p1, p2, p3, p4, p5, p6: out std_logic);
    end component;

    constant ac: integer range 0 to 49999999 := 49999999;
    signal counter1: integer range 0 to 49999999 := 0;
    signal counter2: integer range 0 to 5 := 0;
    signal clk_1sec: std_logic := '0';
    signal q: std_logic_vector(23 downto 0) := (others => 0);
    signal d_and: std_logic_vector(41 downto 0) := (others => 0);
    signal ini: std_logic_vector(23 downto 0) := "100010001101111000010000";
    signal n1: std_logic_vector(23 downto 0) := "100011011110000100001000";
    signal n2: std_logic_vector(23 downto 0) := "110111100001000010001000";
    signal n3: std_logic_vector(23 downto 0) := "111000010000100010001101";
    signal n4: std_logic_vector(23 downto 0) := "000100001000100011011110";
    signal n5: std_logic_vector(23 downto 0) := "000010001000110111100001";
    

begin

    process(clk)
    begin
        if(rising_edge(clk))then
            if(counter1 = ac) then
                clk_1sec <= not clk_1sec;
                counter1 <= 0;
            else
                counter1 <= counter1 + 1;
            end if;
        end if;
    end process;

    process(clk_1sec, rst)
    begin
        if(rst = '1') then
            counter2 <= 0;
        elsif(rising_edge(clk_1sec)) then
            if(counter2 = 5) then
                counter2 <= 0;
            else
                counter2 <= counter2 + 1;
            end if;
        end if;
    end process;

    process(counter2)
    begin
        case(counter2) is
            when 0 =>
                q <= ini;
            when 1 =>
                q <= n1;
            when 2 =>
                q <= n2;
            when 3 =>
                q <= n3;
            when 4 =>
                q <= n4;
            when 5 =>
                q <= n5;
        end case;
    end process;

    process(q, d_and)
    begin
        if(q(3 downto 0) = "1000") then
            d(6 downto 0) <= not d_and(6 downto 0);
        else
            d(6 downto 0) <= d_and(6 downto 0);
        end if;

        if(q(7 downto 4) = "1000") then
            d(13 downto 7) <= not d_and(13 downto 7);
        else
            d(13 downto 7) <= d_and(13 downto 7);
        end if;

        if(q(11 downto 8) = "1000") then
            d(20 downto 14) <= not d_and(20 downto 14);
        else
            d(20 downto 14) <= d_and(20 downto 14);
        end if;

        if(q(15 downto 12) = "1000") then
            d(27 downto 21) <= not d_and(27 downto 21);
        else
            d(27 downto 21) <= d_and(27 downto 21);
        end if;

        if(q(19 downto 16) = "1000") then
            d(34 downto 28) <= not d_and(34 downto 28);
        else
            d(34 downto 28) <= d_and(34 downto 28);
        end if;

        if(q(23 downto 20) = "1000") then
            d(41 downto 35) <= not d_and(41 downto 35);
        else
            d(41 downto 35) <= d_and(41 downto 35);
        end if;

    end process;

    inst1: display
    port map(s0 => q(0),
             s1 => q(1),
             s2 => q(2),
             s3 => q(3),
             p0 => d_and(0),
             p1 => d_and(1),
             p2 => d_and(2),
             p3 => d_and(3),
             p4 => d_and(4),
             p5 => d_and(5),
             p6 => d_and(6));

    inst2: display
    port map(s0 => q(4),
             s1 => q(5),
             s2 => q(6),
             s3 => q(7),
             p0 => d_and(7),
             p1 => d_and(8),
             p2 => d_and(9),
             p3 => d_and(10),
             p4 => d_and(11),
             p5 => d_and(12),
             p6 => d_and(13));

    inst3: display
        port map(s0 => q(8),
                s1 => q(9),
                s2 => q(10),
                s3 => q(11),
                p0 => d_and(14),
                p1 => d_and(15),
                p2 => d_and(16),
                p3 => d_and(17),
                p4 => d_and(18),
                p5 => d_and(19),
                p6 => d_and(20));
        
    inst4: display
    port map(s0 => q(12),
             s1 => q(13),
             s2 => q(14),
             s3 => q(15),
             p0 => d_and(21),
             p1 => d_and(22),
             p2 => d_and(23),
             p3 => d_and(24),
             p4 => d_and(25),
             p5 => d_and(26),
             p6 => d_and(27));

    inst5: display
    port map(s0 => q(16),
             s1 => q(17),
             s2 => q(18),
             s3 => q(19),
             p0 => d_and(28),
             p1 => d_and(29),
             p2 => d_and(30),
             p3 => d_and(31),
             p4 => d_and(32),
             p5 => d_and(33),
             p6 => d_and(34));

    inst6: display
        port map(s0 => q(20),
                s1 => q(21),
                s2 => q(22),
                s3 => q(23),
                p0 => d_and(35),
                p1 => d_and(36),
                p2 => d_and(37),
                p3 => d_and(38),
                p4 => d_and(39),
                p5 => d_and(40),
                p6 => d_and(41));

end rot;
```
