# Componentes de Sistemas Digitais

Nesse repositório serão encontrados os módulos, em VHDL, de vários componentes lógicos.

## Atribuição pinos:
```
PIN_U21  0[0]
PIN_V21
PIN_W22
PIN_W21
PIN_Y22
PIN_Y21
PIN_AA22
PIN_AA20 1[0]
PIN_AB20
PIN_AA19
PIN_AA18
PIN_AB18
PIN_AA17
PIN_U22
PIN_Y19 2[0]
PIN_AB17
PIN_AA10
PIN_Y14
PIN_V14
PIN_AB22
PIN_AB21
PIN_Y16 3[0]
PIN_W16
PIN_Y17
PIN_V16
PIN_U17
PIN_V18
PIN_V19
PIN_U20 4[0]
PIN_Y20
PIN_V20
PIN_U16
PIN_U15
PIN_Y15
PIN_P9
PIN_N9 5[0]
PIN_M8
PIN_T14
PIN_P14
PIN_C1
PIN_C2
PIN_W19
```

## Contador gABRIEL
```VHDL
library IEEE;
use IEEE.STD_LOGIC_1164.ALL;
use IEEE.NUMERIC_STD.ALL;  -- Importar o pacote para operações aritméticas

entity part2 is
    Port (
        clk    : in  STD_LOGIC;
        enable : in  STD_LOGIC;
        clear  : in  STD_LOGIC;
        seg1   : out STD_LOGIC_VECTOR(6 downto 0); -- Display 1
        seg2   : out STD_LOGIC_VECTOR(6 downto 0); -- Display 2
        seg3   : out STD_LOGIC_VECTOR(6 downto 0); -- Display 3
        seg4   : out STD_LOGIC_VECTOR(6 downto 0)  -- Display 4
    );
end part2;

architecture Behavioral of part2 is
    -- Declaração do componente 'display'
    component display
        Port (
            bin   : in  STD_LOGIC_VECTOR(3 downto 0);
            seg   : out STD_LOGIC_VECTOR(6 downto 0)
        );
    end component;

    -- Sinais internos
    signal Q       : STD_LOGIC_VECTOR(15 downto 0) := (others => '0');
    signal nibble1 : STD_LOGIC_VECTOR(3 downto 0);
    signal nibble2 : STD_LOGIC_VECTOR(3 downto 0);
    signal nibble3 : STD_LOGIC_VECTOR(3 downto 0);
    signal nibble4 : STD_LOGIC_VECTOR(3 downto 0);

begin
    process(clk, clear)
    begin
        if clear = '1' then
            Q <= (others => '0');
        elsif rising_edge(clk) then
            if enable = '1' then
                Q <= std_logic_vector(unsigned(Q) + 1); -- Converte para unsigned, realiza a adição e converte de volta para std_logic_vector
            end if;
        end if;
    end process;

    -- Divida o valor de 16 bits em 4 nibbles de 4 bits cada
    nibble1 <= Q(3 downto 0);
    nibble2 <= Q(7 downto 4);
    nibble3 <= Q(11 downto 8);
    nibble4 <= Q(15 downto 12);

    -- Instanciar e conectar os decodificadores de display de 7 segmentos
    U1: display port map (bin => nibble1, seg => seg1);
    U2: display port map (bin => nibble2, seg => seg2);
    U3: display port map (bin => nibble3, seg => seg3);
    U4: display port map (bin => nibble4, seg => seg4);

end Behavioral;
```

## Display
```VHDL
library IEEE;
use IEEE.std_logic_1164.all;

entity display is
    port (
        bin : in std_logic_vector(3 downto 0);
        seg     : out std_logic_vector(6 downto 0) -- Segments a to g
    );
end entity display;

architecture Behavioral of display is
begin
    process(bin)
    begin
        case bin is
    when "0000" => seg <= "1000000"; -- 0
    when "0001" => seg <= "1111001"; -- 1
    when "0010" => seg <= "0100100"; -- 2
    when "0011" => seg <= "0110000"; -- 3
    when "0100" => seg <= "0011001"; -- 4
    when "0101" => seg <= "0010010"; -- 5
    when "0110" => seg <= "0000010"; -- 6
    when "0111" => seg <= "1111000"; -- 7
    when "1000" => seg <= "0000000"; -- 8
    when "1001" => seg <= "0010000"; -- 9
    when "1010" => seg <= "0001000"; -- A
    when "1011" => seg <= "0000011"; -- b
    when "1100" => seg <= "1000110"; -- C
    when "1101" => seg <= "0100001"; -- d
    when "1110" => seg <= "0000110"; -- E
    when "1111" => seg <= "0001110"; -- F
    when others => seg <= "1111111"; -- Default case
end case;
    end process;
end architecture Behavioral;

```
```VHDL
library ieee;
use ieee.std_logic_1164.all;
use ieee.numeric_std.all;

entity counter_16bits is
    port(clk, rst: in std_logic;
        d: out std_logic_vector(4 downto 0));
end counter_16bits;

architecture behavior of counter_16bits is


    signal counter: std_logic_vector (15 downto 0) := "0000000000000000";  
    signal count: std_logic_vector (15 downto 0) := "0000000000000000";

begin    
    process(clk, rst)
    begin
        if(rst = '1') then
				counter <= "0000000000000000";
		  elsif(rising_edge(clk)) then
                counter <= std_logic_vector(unsigned(counter) + 1);
        else
                counter <= counter;
        end if;
    end process;

    d(4 downto 0) <= counter(4 downto 0);
    
    
end behavior;
```

## Display de 7 segmentos
```VHDL
LIBRARY ieee;
USE ieee.std_logic_1164.all;

LIBRARY work;

ENTITY p004_2 IS
	PORT
	(
    	S0 :  IN  STD_LOGIC;
    	S1 :  IN  STD_LOGIC;
    	S2 :  IN  STD_LOGIC;
    	S3 :  IN  STD_LOGIC;
    	p0 :  OUT  STD_LOGIC;
    	p1 :  OUT  STD_LOGIC;
    	p2 :  OUT  STD_LOGIC;
    	p3 :  OUT  STD_LOGIC;
    	p4 :  OUT  STD_LOGIC;
    	p5 :  OUT  STD_LOGIC;
    	p6 :  OUT  STD_LOGIC
	);
END p004_2;

ARCHITECTURE bdf_type OF p004_2 IS

SIGNAL	SYNTHESIZED_WIRE_123 :  STD_LOGIC;
SIGNAL	SYNTHESIZED_WIRE_124 :  STD_LOGIC;
SIGNAL	SYNTHESIZED_WIRE_125 :  STD_LOGIC;
SIGNAL	SYNTHESIZED_WIRE_126 :  STD_LOGIC;
SIGNAL	SYNTHESIZED_WIRE_32 :  STD_LOGIC;
SIGNAL	SYNTHESIZED_WIRE_33 :  STD_LOGIC;
SIGNAL	SYNTHESIZED_WIRE_34 :  STD_LOGIC;
SIGNAL	SYNTHESIZED_WIRE_127 :  STD_LOGIC;
SIGNAL	SYNTHESIZED_WIRE_128 :  STD_LOGIC;
SIGNAL	SYNTHESIZED_WIRE_129 :  STD_LOGIC;
SIGNAL	SYNTHESIZED_WIRE_130 :  STD_LOGIC;
SIGNAL	SYNTHESIZED_WIRE_131 :  STD_LOGIC;
SIGNAL	SYNTHESIZED_WIRE_132 :  STD_LOGIC;
SIGNAL	SYNTHESIZED_WIRE_133 :  STD_LOGIC;
SIGNAL	SYNTHESIZED_WIRE_134 :  STD_LOGIC;
SIGNAL	SYNTHESIZED_WIRE_135 :  STD_LOGIC;
SIGNAL	SYNTHESIZED_WIRE_136 :  STD_LOGIC;
SIGNAL	SYNTHESIZED_WIRE_137 :  STD_LOGIC;
SIGNAL	SYNTHESIZED_WIRE_138 :  STD_LOGIC;
SIGNAL	SYNTHESIZED_WIRE_139 :  STD_LOGIC;
SIGNAL	SYNTHESIZED_WIRE_140 :  STD_LOGIC;
SIGNAL	SYNTHESIZED_WIRE_141 :  STD_LOGIC;
SIGNAL	SYNTHESIZED_WIRE_142 :  STD_LOGIC;
SIGNAL	SYNTHESIZED_WIRE_119 :  STD_LOGIC;
SIGNAL	SYNTHESIZED_WIRE_120 :  STD_LOGIC;
SIGNAL	SYNTHESIZED_WIRE_121 :  STD_LOGIC;
SIGNAL	SYNTHESIZED_WIRE_122 :  STD_LOGIC;


BEGIN



SYNTHESIZED_WIRE_129 <= SYNTHESIZED_WIRE_123 AND SYNTHESIZED_WIRE_124 AND SYNTHESIZED_WIRE_125 AND SYNTHESIZED_WIRE_126;


SYNTHESIZED_WIRE_139 <= S0 AND SYNTHESIZED_WIRE_124 AND SYNTHESIZED_WIRE_125 AND SYNTHESIZED_WIRE_126;


SYNTHESIZED_WIRE_135 <= SYNTHESIZED_WIRE_123 AND S1 AND SYNTHESIZED_WIRE_125 AND S3;


SYNTHESIZED_WIRE_142 <= S0 AND S1 AND SYNTHESIZED_WIRE_125 AND S3;


SYNTHESIZED_WIRE_137 <= SYNTHESIZED_WIRE_123 AND SYNTHESIZED_WIRE_124 AND S2 AND S3;


SYNTHESIZED_WIRE_141 <= S0 AND SYNTHESIZED_WIRE_124 AND S2 AND S3;


SYNTHESIZED_WIRE_136 <= SYNTHESIZED_WIRE_123 AND S1 AND S2 AND S3;


SYNTHESIZED_WIRE_138 <= S0 AND S1 AND S2 AND S3;


SYNTHESIZED_WIRE_128 <= SYNTHESIZED_WIRE_123 AND S1 AND SYNTHESIZED_WIRE_125 AND SYNTHESIZED_WIRE_126;


SYNTHESIZED_WIRE_127 <= S0 AND S1 AND SYNTHESIZED_WIRE_125 AND SYNTHESIZED_WIRE_126;


SYNTHESIZED_WIRE_140 <= SYNTHESIZED_WIRE_123 AND SYNTHESIZED_WIRE_124 AND S2 AND SYNTHESIZED_WIRE_126;


SYNTHESIZED_WIRE_132 <= S0 AND SYNTHESIZED_WIRE_124 AND S2 AND SYNTHESIZED_WIRE_126;


SYNTHESIZED_WIRE_131 <= SYNTHESIZED_WIRE_123 AND S1 AND S2 AND SYNTHESIZED_WIRE_126;


SYNTHESIZED_WIRE_130 <= S0 AND S1 AND S2 AND SYNTHESIZED_WIRE_126;


SYNTHESIZED_WIRE_134 <= SYNTHESIZED_WIRE_123 AND SYNTHESIZED_WIRE_124 AND SYNTHESIZED_WIRE_125 AND S3;


SYNTHESIZED_WIRE_133 <= S0 AND SYNTHESIZED_WIRE_124 AND SYNTHESIZED_WIRE_125 AND S3;


p0 <= NOT(SYNTHESIZED_WIRE_32);



p1 <= NOT(SYNTHESIZED_WIRE_33);



p6 <= NOT(SYNTHESIZED_WIRE_34);



SYNTHESIZED_WIRE_123 <= NOT(S0);



SYNTHESIZED_WIRE_32 <= SYNTHESIZED_WIRE_127 OR SYNTHESIZED_WIRE_128 OR SYNTHESIZED_WIRE_129 OR SYNTHESIZED_WIRE_130 OR SYNTHESIZED_WIRE_131 OR SYNTHESIZED_WIRE_132 OR SYNTHESIZED_WIRE_133 OR SYNTHESIZED_WIRE_134 OR SYNTHESIZED_WIRE_135 OR SYNTHESIZED_WIRE_136 OR SYNTHESIZED_WIRE_137 OR SYNTHESIZED_WIRE_138;


SYNTHESIZED_WIRE_33 <= SYNTHESIZED_WIRE_128 OR SYNTHESIZED_WIRE_139 OR SYNTHESIZED_WIRE_129 OR SYNTHESIZED_WIRE_130 OR SYNTHESIZED_WIRE_140 OR SYNTHESIZED_WIRE_127 OR SYNTHESIZED_WIRE_133 OR SYNTHESIZED_WIRE_134 OR SYNTHESIZED_WIRE_135 OR SYNTHESIZED_WIRE_141 OR SYNTHESIZED_WIRE_141 OR SYNTHESIZED_WIRE_141;


SYNTHESIZED_WIRE_119 <= SYNTHESIZED_WIRE_127 OR SYNTHESIZED_WIRE_139 OR SYNTHESIZED_WIRE_129 OR SYNTHESIZED_WIRE_131 OR SYNTHESIZED_WIRE_132 OR SYNTHESIZED_WIRE_140 OR SYNTHESIZED_WIRE_134 OR SYNTHESIZED_WIRE_130 OR SYNTHESIZED_WIRE_133 OR SYNTHESIZED_WIRE_142 OR SYNTHESIZED_WIRE_135 OR SYNTHESIZED_WIRE_141;


SYNTHESIZED_WIRE_120 <= SYNTHESIZED_WIRE_127 OR SYNTHESIZED_WIRE_128 OR SYNTHESIZED_WIRE_129 OR SYNTHESIZED_WIRE_134 OR SYNTHESIZED_WIRE_131 OR SYNTHESIZED_WIRE_132 OR SYNTHESIZED_WIRE_142 OR SYNTHESIZED_WIRE_133 OR SYNTHESIZED_WIRE_137 OR SYNTHESIZED_WIRE_136 OR SYNTHESIZED_WIRE_141 OR SYNTHESIZED_WIRE_136;


SYNTHESIZED_WIRE_121 <= SYNTHESIZED_WIRE_131 OR SYNTHESIZED_WIRE_128 OR SYNTHESIZED_WIRE_129 OR SYNTHESIZED_WIRE_142 OR SYNTHESIZED_WIRE_135 OR SYNTHESIZED_WIRE_134 OR SYNTHESIZED_WIRE_141 OR SYNTHESIZED_WIRE_137 OR SYNTHESIZED_WIRE_136 OR SYNTHESIZED_WIRE_138 OR SYNTHESIZED_WIRE_138 OR SYNTHESIZED_WIRE_138;


SYNTHESIZED_WIRE_122 <= SYNTHESIZED_WIRE_132 OR SYNTHESIZED_WIRE_140 OR SYNTHESIZED_WIRE_129 OR SYNTHESIZED_WIRE_133 OR SYNTHESIZED_WIRE_134 OR SYNTHESIZED_WIRE_131 OR SYNTHESIZED_WIRE_142 OR SYNTHESIZED_WIRE_135 OR SYNTHESIZED_WIRE_137 OR SYNTHESIZED_WIRE_138 OR SYNTHESIZED_WIRE_136 OR SYNTHESIZED_WIRE_138;


SYNTHESIZED_WIRE_34 <= SYNTHESIZED_WIRE_140 OR SYNTHESIZED_WIRE_127 OR SYNTHESIZED_WIRE_128 OR SYNTHESIZED_WIRE_134 OR SYNTHESIZED_WIRE_131 OR SYNTHESIZED_WIRE_132 OR SYNTHESIZED_WIRE_135 OR SYNTHESIZED_WIRE_133 OR SYNTHESIZED_WIRE_142 OR SYNTHESIZED_WIRE_136 OR SYNTHESIZED_WIRE_141 OR SYNTHESIZED_WIRE_138;


SYNTHESIZED_WIRE_124 <= NOT(S1);



SYNTHESIZED_WIRE_125 <= NOT(S2);



SYNTHESIZED_WIRE_126 <= NOT(S3);



p2 <= NOT(SYNTHESIZED_WIRE_119);



p3 <= NOT(SYNTHESIZED_WIRE_120);



p4 <= NOT(SYNTHESIZED_WIRE_121);



p5 <= NOT(SYNTHESIZED_WIRE_122);



END bdf_type;
```

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
use ieee.numeric_std.all;

entity counter_16bits is
    port(clk, rst: in std_logic;
        q: out std_logic_vector(15 downto 0));
end counter_16bits;

architecture behavior of counter_16bits is

    signal count: std_logic_vector(15 downto 0);  

begin    
    process(clk, rst)
    begin
	if(rst = '1') then
		count <= (others => '0');
	elsif(rising_edge(clk)) then
		count <= std_logic_vector(unsigned(count)+1);
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
> Contador de 16 bits com display(t2)(TESTAR):
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

    signal counter: std_logic_vector (15 downto 0);  
    signal count: std_logic_vector (15 downto 0);

begin    
    process(clk, rst)
    begin
        if(rst = '1')then
		counter <= (others => '0');
	elsif(rising_edge(clk)) then
		counter <= std_logic_vector(unsigned(counter) + 1);
	end if;
    end process;

    count <= counter;

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
>Contador de 16 bits(t3):
```VHDL
library ieee;
use ieee.std_logic_1164.all;
use ieee.numeric_std.all;

entity counter_16bits is
    port(clk, rst, enable: in std_logic;
        d1: out std_logic_vector(6 downto 0);
		  d2: out std_logic_vector(6 downto 0);
		  d3: out std_logic_vector(6 downto 0);
		  d4: out std_logic_vector(6 downto 0));
end counter_16bits;

architecture behavior of counter_16bits is

    component display
        port(bin: in std_logic_vector(3 downto 0);
            seg: out std_logic_vector(6 downto 0));
    end component;

    signal counter: std_logic_vector (15 downto 0) := (others => '0');
	 signal nibble1: std_logic_vector (3 downto 0);
	 signal nibble2: std_logic_vector (3 downto 0);
	 signal nibble3: std_logic_vector (3 downto 0);
	 signal nibble4: std_logic_vector (3 downto 0);

begin    
    process(clk, rst)
    begin
    if(rst = '1')then
			counter <= (others => '0');
	 elsif(rising_edge(clk)) then
			if enable = '1' then
				counter <= std_logic_vector(unsigned(counter) + 1);
		end if;
    end if;
	 end process;
	 
	 nibble1 <= counter(3 downto 0);
	 nibble2 <= counter(7 downto 4);
	 nibble3 <= counter(11 downto 8);
	 nibble4 <= counter(15 downto 12);

    inst1: display
    port map(bin => nibble1,
				seg => d1);
				
	inst2: display
    port map(bin => nibble2,
				seg => d2);
	
	inst3: display
    port map(bin => nibble3,
				seg => d3);
			
	inst4: display
    port map(bin => nibble4,
				seg => d4);
			
			
	

    

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
    constant max: unsigned(26 downto 0) := to_unsigned(25000000-1, 27);

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
    
    process(clk_1sec, rst)
    begin
        if(rst = '1') then
            counter_q <= (others => '0');
        elsif(rising_edge(clk_1sec)) then
            if(counter_q = "1001") then
                counter_q <= (others => '0');
            else
                counter_q <= std_logic_vector(unsigned(counter_q) + 1);
            end if;
        end if;
    end process;
    q <= counter_q;
end behavior;
```
> contador de 1 segundo(com display):
```VHDL
library ieee;
use ieee.std_logic_1164.all;
use ieee.numeric_std.all;

entity counter_1sec_d is
    port(clk, rst: in std_logic;
        q: out std_logic_vector(6 downto 0));
end counter_1sec_d;

architecture behavior of counter_1sec_d is

    component display
    port(s0, s1, s2, s3: in std_logic;
        p0, p1, p2, p3, p4, p5, p6: out std_logic);
    end component;

    signal counter: std_logic_vector(26 downto 0) := (others => '0');
    signal counter_q: std_logic_vector(3 downto 0) := (others => '0');
    signal clk_1sec: std_logic := '0';
    constant max: unsigned(26 downto 0) := to_unsigned(25000000-1, 27);

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
    
    process(clk_1sec, rst)
    begin
        if(rst = '1') then
            counter_q <= (others => '0');
        elsif(rising_edge(clk_1sec)) then
            if(counter_q = "1001") then
                counter_q <= (others => '0');
            else
                counter_q <= std_logic_vector(unsigned(counter_q) + 1);
            end if;
        end if;
    end process;
    
    inst: display
    port map(s0 => counter_q(0),
             s1 => counter_q(1),
             s2 => counter_q(2),
             s3 => counter_q(3),
             p0 => q(0),
             p1 => q(1),
             p2 => q(2),
             p3 => q(3),
             p4 => q(4),
             p5 => q(5),
             p6 => q(6));

end behavior;
```
## Rotator
> Rotator de 4 bits:
```VHDL
library ieee;
use ieee.std_logic_1164.all;
use ieee.numeric_std.all;

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
    constant ac: integer range 0 to 24999999 := 24999999;
    signal counter1: std_logic_vector(26 downto 0) := (others => '0');
    signal counter2: std_logic_vector(1 downto 0):= (others => '0');
    signal ini: std_logic_vector(15 downto 0) := "1000110111100000";
    signal n1: std_logic_vector(15 downto 0) := "1101111000001000";
    signal n2: std_logic_vector(15 downto 0) := "1110000010001101";
    signal n3: std_logic_vector(15 downto 0) := "0000100011011110";
    signal q: std_logic_vector(15 downto 0) := "0000000000000000";
	 signal nd: std_logic_vector(27 downto 0) := (others => '0');
    
begin
    process(clk)
    begin
        if(rising_edge(clk))then
            if(unsigned(counter1) = ac) then
                clk_1sec <= not clk_1sec;
                counter1 <= (others => '0');
            else
                counter1 <= std_logic_vector(unsigned(counter1) + 1);
            end if;
        end if;
    end process;

    process(clk_1sec, rst)
    begin
        if(rst = '1') then
            counter2 <= (others => '0');
        elsif (rising_edge(clk_1sec)) then
            if(counter2 = "11") then
                counter2 <= (others => '0');
            else
                counter2 <= std_logic_vector(unsigned(counter2) + 1);
            end if;
        end if;
    end process;

    process(counter2)
    begin
        case (counter2) is
            when "00" =>
                q <= ini;
            when "01" =>
                q <= n1;
            when "10" =>
                q <= n2;
            when "11" =>
                q <= n3;
        end case;
    end process;
	 
	 process(q)
	 begin
		if(q(3 downto 0) = "1000") then
			d(6 downto 0) <= not nd(6 downto 0);
		else
			d(6 downto 0) <= nd(6 downto 0);
		end if;
		
		if(q(7 downto 4) = "1000") then
			d(13 downto 7) <= not nd(13 downto 7);
		else
			d(13 downto 7) <= nd(13 downto 7);
		end if;
		
		if(q(11 downto 8) = "1000") then
			d(20 downto 14) <= not nd(20 downto 14);
		else
			d(20 downto 14) <= nd(20 downto 14);
		end if;
		
		if(q(15 downto 12) = "1000") then
			d(27 downto 21) <= not nd(27 downto 21);
		else
			d(27 downto 21) <= nd(27 downto 21);
		end if;
		
	end process;

    inst1: display
    port map(s0 => q(0),
             s1 => q(1),
             s2 => q(2),
             s3 => q(3),
             p0 => nd(0),
             p1 => nd(1),
             p2 => nd(2),
             p3 => nd(3),
             p4 => nd(4),
             p5 => nd(5),
             p6 => nd(6));

    inst2: display
    port map(s0 => q(4),
             s1 => q(5),
             s2 => q(6),
             s3 => q(7),
             p0 => nd(7),
             p1 => nd(8),
             p2 => nd(9),
             p3 => nd(10),
             p4 => nd(11),
             p5 => nd(12),
             p6 => nd(13));

    inst3: display
    port map(s0 => q(8),
             s1 => q(9),
             s2 => q(10),
             s3 => q(11),
             p0 => nd(14),
             p1 => nd(15),
             p2 => nd(16),
             p3 => nd(17),
             p4 => nd(18),
             p5 => nd(19),
             p6 => nd(20));
            
    inst4: display
    port map(s0 => q(12),
             s1 => q(13),
             s2 => q(14),
             s3 => q(15),
             p0 => nd(21),
             p1 => nd(22),
             p2 => nd(23),
             p3 => nd(24),
             p4 => nd(25),
             p5 => nd(26),
             p6 => nd(27));
    
end rot;
```
> Rotator de 6 bits:
```VHDL
library ieee;
use ieee.std_logic_1164.all;
use ieee.numeric_std.all;

entity rotator6bits is
    port(rst, clk: in std_logic;
        d: out std_logic_vector(41 downto 0));
end rotator6bits;

architecture rot of rotator6bits is

    component display
        port(s0, s1, s2, s3: in std_logic;
            p0, p1, p2, p3, p4, p5, p6: out std_logic);
    end component;

    constant ac: integer range 0 to 24999999 := 24999999;
    signal counter1: std_logic_vector (26 downto 0) := (others => '0');
    signal counter2: std_logic_vector (2 downto 0) := (others => '0');
    signal clk_1sec: std_logic := '0';
    signal q: std_logic_vector(23 downto 0) := (others => '0');
    signal d_and: std_logic_vector(41 downto 0) := (others => '0');
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
            if(unsigned(counter1) = ac) then
                clk_1sec <= not clk_1sec;
                counter1 <= (others => '0');
            else
                counter1 <= std_logic_vector(unsigned(counter1) + 1);
            end if;
        end if;
    end process;

    process(clk_1sec, rst)
    begin
        if(rst = '1') then
            counter2 <= (others => '0');
        elsif(rising_edge(clk_1sec)) then
            if(counter2 = "101") then
                counter2 <= (others => '0');
            else
                counter2 <= std_logic_vector(unsigned(counter2) + 1);
            end if;
        end if;
    end process;

    process(counter2)
    begin
        case(counter2) is
            when "000" =>
                q <= ini;
            when "001" =>
                q <= n1;
            when "010" =>
                q <= n2;
            when "011" =>
                q <= n3;
            when "100" =>
                q <= n4;
            when "101" =>
                q <= n5;
	    when others =>
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
