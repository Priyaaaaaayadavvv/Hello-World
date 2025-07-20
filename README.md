# 💫 Hi 👋, I'm Priya Yadav
**A passionate engineer **

Email Me 👉 ✉️ **priyayadavsj1111@gmail.com** For Collaboration/Project or Anything Else. 

Stay updated with the latest project

<!-- Snake Game Repo View -->

<div align="center">
  <img src="https://profile-readme-generator.com/assets/snake.svg" alt="Snake animation" />
</div>  

#vhdl
#Implementation of an encoder for error detection using reed-solomon codes

library IEEE;
use IEEE.STD_LOGIC_1164.ALL;
use IEEE.NUMERIC_STD.ALL;

entity tb_rs_error_detect is
end tb_rs_error_detect;

architecture testbench of tb_rs_error_detect is

    -- Component Declaration
    component rs_error_detect
        Port (
            clk             : in  std_logic;
            rst             : in  std_logic;
            codeword        : in  std_logic_vector(6 downto 0);
            error_detected  : out std_logic
        );
    end component;

    -- Signals
    signal clk_tb            : std_logic := '0';
    signal rst_tb            : std_logic := '1';
    signal codeword_tb       : std_logic_vector(6 downto 0) := (others => '0');
    signal error_detected_tb : std_logic;

    -- Clock Generation
    constant clk_period : time := 10 ns;

begin

    -- Instantiate Unit Under Test (UUT)
    uut: rs_error_detect
        Port map (
            clk => clk_tb,
            rst => rst_tb,
            codeword => codeword_tb,
            error_detected => error_detected_tb
        );

    -- Clock process
    clk_process : process
    begin
        while true loop
            clk_tb <= '0';
            wait for clk_period / 2;
            clk_tb <= '1';
            wait for clk_period / 2;
        end loop;
    end process;

    -- Stimulus process
    stim_proc: process
    begin
        -- Hold reset for a few cycles
        wait for 20 ns;
        rst_tb <= '0';

        -- Test 1: Valid Codeword (no error)
        codeword_tb <= "1011001";
        wait for 20 ns;
        assert (error_detected_tb = '0')
            report "Test 1 Failed: False error detected." severity error;

        -- Test 2: Introduce Error (bit flip)
        codeword_tb <= "1111001";
        wait for 20 ns;
        assert (error_detected_tb = '1')
            report "Test 2 Failed: Error not detected." severity error;

        -- Test 3: Multiple Errors
        codeword_tb <= "0000000";
        wait for 20 ns;
        assert (error_detected_tb = '1')
            report "Test 3 Failed: Error not detected for multiple errors." severity error;

        -- Test 4: Another valid codeword
        codeword_tb <= "1100110";
        wait for 20 ns;
        assert (error_detected_tb = '0')
            report "Test 4 Failed: False error detected." severity error;

        wait for 20 ns;
        report "All tests completed." severity note;
        wait;
    end process;

end testbench;
