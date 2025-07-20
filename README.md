# 💫 Hi 👋, I'm Priya Yadav
**A passionate engineer **

Email Me 👉 ✉️ **priyayadavsj1111@gmail.com** For Collaboration/Project or Anything Else. 

Stay updated with the latest project

<!-- Snake Game Repo View -->

<div align="center">
  <img src="https://profile-readme-generator.com/assets/snake.svg" alt="Snake animation" />
</div>  

Tech Stack:
![HTML5](https://img.shields.io/badge/html5-%23E34F26.svg?style=for-the-badge&logo=html5&logoColor=white) ![Python](https://img.shields.io/badge/python-3670A0?style=for-the-badge&logo=python&logoColor=ffdd54)  ![AWS](https://img.shields.io/badge/AWS-%23FF9900.svg?style=for-the-badge&logo=amazon-aws&logoColor=white) ! ![MySQL](https://img.shields.io/badge/mysql-%2300000f.svg?style=for-the-badge&logo=mysql&logoColor=white) ![Postgres](https://img.shields.io/badge/postgres-%23316192.svg?style=for-the-badge&logo=postgresql&logoColor=white) ![Adobe](https://img.shields.io/badge/adobe-%23FF0000.svg?style=for-the-badge&logo=adobe&logoColor=white) ![Adobe Photoshop](https://img.shields.io/badge/adobe%20photoshop-%2331A8FF.svg?style=for-the-badge&logo=adobe%20photoshop&logoColor=white) ![C](https://img.shields.io/badge/c-%2300599C.svg?style=for-the-badge&logo=c&logoColor=white)
![C++](https://img.shields.io/badge/c++-%2300599C.svg?style=for-the-badge&logo=c%2B%2B&logoColor=white)
![Python](https://img.shields.io/badge/python-%233670A0.svg?style=for-the-badge&logo=python&logoColor=ffdd54)
![PHP](https://img.shields.io/badge/php-%23777BB4.svg?style=for-the-badge&logo=php&logoColor=white)


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
