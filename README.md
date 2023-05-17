# Proyecto3
diseño lógico - proyecto de multiplicador secuencial
entity MultiplicadorSecuencial is
  generic (
    N : positive := 8  -- Tamaño de los operandos
  );
  port (
    clk     : in  std_logic;         -- Señal de reloj
    a, b    : in  std_logic_vector(N-1 downto 0); -- Operandos
    resultado : out std_logic_vector(2*N-1 downto 0) -- Resultado de la multiplicación
  );
end MultiplicadorSecuencial;

architecture Behavioral of MultiplicadorSecuencial is
  signal multiplicando, multiplicador, suma_parcial, resultado_parcial : std_logic_vector(N-1 downto 0);
begin
  multiplicando <= a;
  multiplicador <= b;
  
  proceso_reloj: process(clk)
  begin
    if rising_edge(clk) then
      if multiplicador(0) = '1' then -- Si el bit menos significativo del multiplicador es 1, sumar multiplicando al resultado parcial
        suma_parcial <= resultado_parcial + multiplicando;
      else
        suma_parcial <= resultado_parcial; -- Si el bit menos significativo del multiplicador es 0, no se realiza la suma
      end if;
      
      resultado_parcial <= '0' & suma_parcial(N-2 downto 0); -- Desplazar a la izquierda el resultado parcial
      
      if multiplicador(N-1) = '1' then -- Si el bit más significativo del multiplicador es 1, añadir el multiplicando al resultado parcial
        resultado <= suma_parcial & multiplicando;
      else
        resultado <= suma_parcial & (N-1 downto 0 => '0'); -- Si el bit más significativo del multiplicador es 0, llenar con ceros
      end if;
    end if;
  end process proceso_reloj;
end Behavioral;
    end
