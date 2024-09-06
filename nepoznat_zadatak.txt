--ocigledno da je zadatak radjen kao i kao bihejvioralni opis, gde je
--komponenta radjena kao bihejvioralni opis

Library ieee;
use ieee.std_logic_1164.all;

Entity xor_c is
port(a,b:in std_logic;
     y:out std_logic);
end entity;
architecture behavioral of xor_c is
begin
  y<=a xor b;
end architecture;

Library ieee;
use ieee.std_logic_1164.all;

entity sum_c is
Generic(N:integer);
port(x:in std_logic_vector(N-1 downto 0);
	 o: out std_logic_vector(N-2 downto 0));
end entity;

architecture structural of sum_c is
component xor_c is 
port(a:in std_logic;
	 b:in std_logic;
     y:out std_logic);
end component;
begin
G1:for i in 0 to N-2 generate
     begin
     x_c:xor_c port map(a=>x(i),b=>x(i+1),y=>o(i));
     end generate;	
end architecture;


library IEEE;
use IEEE.std_logic_1164.all;

Entity sum_c_TB is
end entity;


architecture sum_c_TB_arch of sum_c_TB is

signal x_tb : std_logic_vector(3 downto 0);
signal o_tb:std_logic_vector(2 downto 0);
begin
	DUT: Entity work.sum_c(structural)
    generic map(N=>4)
    port map(x=>x_tb,o=>o_tb);
STIMULUS:Process
		    begin
            x_tb<="0000","1000" after 10ps,"1100" after 20ns;
            end process;
end architecture;



