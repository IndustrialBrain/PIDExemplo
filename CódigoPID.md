# PIDExemplo

Exemplo de código. Feito em codesys, mas pode ser utilizado em qualquer CLP ou linguagem.
Criado em ST.
__________________________________________________________________
VAR_INPUT
	Hab_PID: BOOL;
	PV: REAL;
	SP: REAL;
	KP: REAL;
	Ki: REAL;
	Kd: REAL;
END_VAR
VAR_OUTPUT
	Out: REAL;
END_VAR
VAR
	erro:REAL;
	erroAnterior:REAL;
	P:REAL;
	I:REAL;
	D:REAL;
	res:REAL;
END_VAR
___________________________________________________________________
//Verifica se o PID está Habilitado. Caso não escreve 0.

IF Hab_PID THEN

  //calcula o erro
  erro:=SP-PV;
  
  //Calcula componente P
  P:=KP*erro;
  
  //Calcula componente I limitando até 20% da saída
  I:=erro+I;
  
  IF I > 20 THEN
   I:=20; 
  ELSIF I < -20 THEN
   I:=-20;
  END_IF
  
  I:=I*Ki;
  
  //Calcula a compoente D
  D:=(erro-erroAnterior)*kd;

  //Resultado
  res:= P+I+D;
  
  IF res >100 THEN
   res:=100;
  ELSIF res < -100 THEN
   res:=-100;
  END_IF
  
  //Transfere para saida o resultado
  
  Out:=Out+res;
  
  IF Out > 100 THEN
   out:=100;
  ELSIF Out < 0 THEN
   Out:=0;
  END_IF

  //Salva erro anterior para utilização no próximo Scan. Componente derivativa.
  erroAnterior:=erro;

ELSE
 Out:=0;
END_IF
