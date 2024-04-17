library ieee ;
    use ieee.std_logic_1164.all;
    use ieee.numeric_std.all;

entity xorKolo is
  port (
    a, b : in std_logic;
    y : out std_logic);
end xorKolo ;

architecture a_xorKolo of xorKolo is
begin
    y <= a xor b;
end architecture;

library ieee ;
    use ieee.std_logic_1164.all;
    use ieee.numeric_std.all;

entity orKolo is
  port (
    a, b : in std_logic;
    y : out std_logic);
end orKolo;

architecture a_orKolo of orKolo is
begin
    y <= a or b;
end architecture ;

library ieee ;
    use ieee.std_logic_1164.all;
    use ieee.numeric_std.all;

entity jednoKolo
is
    generic (n : integer := 4);
    port (
    	a, b : in std_logic_vector (n-1 downto 0);
    	c : in std_logic;
    	y : out std_logic_vector (n-1 downto 0));
end jednoKolo ;

architecture a_kolo of jednoKolo is
    signal medjuSignal : std_logic_vector(n-1 downto 0);
begin
    generisi: for i in 0 to n-1 generate
    begin
    nultaIteracijaKola : if i = 0 generate
    	begin 
        	nultaIteracijaXor : entity work.xorKolo(a_xorKolo)
       			port map (c, a(0), medjuSignal(0));
    		nultaIteracijaOr : entity work.orKolo(a_orKolo)
        		port map (medjuSignal(0), b(0), y(0));
        end generate nultaIteracijaKola;
    ntaIteracijaKola : if i /= 0 generate
    begin
        ntaIteracijaXor : entity work.xorKolo(a_xorKolo)
            port map (y(i-1), a(i), medjuSignal(i));
        ntaIteracijaOr : entity work.orKolo(a_orKolo)
            port map (medjuSignal(i), b(i), y(i));
    end generate ntaIteracijaKola;        
    end generate generisi;
end architecture;



--TESTBENCH



library IEEE;
use IEEE.std_logic_1164.all;

entity tb is
    generic (n : integer := 4);
end tb ;

architecture a_tb of tb is
    signal a, b, y : std_logic_vector (n-1 downto 0);
    signal c : std_logic;
begin
    dut : entity work.jednoKolo(a_kolo)
        port map (a, b, c, y);
    stimuli : process
    begin
    	wait for 50ns;
        c <= '1';
        a <= "0011";
        b <= "0110";
        wait for 50 ns;
        c <= '0';
        a <= "1100";
        b <= "1001";
        wait for 50ns;
    end process;
end architecture;