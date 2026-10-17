Requisitos Funcionais (RF)
RF01 O sistema deve permitir cadastrar funcionários, registrando seu identificador e nome.

RF02 O sistema deve permitir consultar a lista de funcionários cadastrados.

RF03 O sistema deve permitir registrar o horário de entrada dos funcionários.

RF04 O sistema deve permitir registrar a saída e o retorno do intervalo dos funcionários.

RF05 O sistema deve permitir registrar o horário de saída ao final da jornada de trabalho.

RF06 O sistema deve calcular o total de horas trabalhadas com base nos registros de ponto.

RF07 O sistema deve gerar um resumo da frequência dos funcionários, identificando atrasos e horas extras.



Requisitos Não Funcionais (RNF)
RNF01 O sistema deve ser desenvolvido em linguagem C e executado no terminal.

RNF02 O sistema deve apresentar menus e mensagens em português claro.

RNF03 O sistema deve validar as opções informadas pelo usuário, evitando entradas inválidas.

RNF04 O código deve ser organizado em funções para facilitar a manutenção e a compreensão.

RNF05 O sistema deve apresentar mensagens de erro compreensíveis quando ocorrer uma operação inválida.

RNF06 O sistema deve apresentar datas e horários em um formato padronizado.

RNF07 O sistema deve manter consistência nos cálculos de horas trabalhadas, atrasos e horas extras.



Regras de Negócio (RN)
RN01 Cada funcionário deve possuir um identificador único no sistema.

RN02 Somente funcionários cadastrados podem ter registros de ponto associados a eles.

RN03 Os registros da jornada devem seguir uma sequência lógica: entrada, saída para intervalo, retorno do intervalo e saída final, quando aplicável.

RN04 O horário de saída para intervalo deve ser anterior ao horário de retorno.

RN05 O cálculo das horas trabalhadas deve descontar os intervalos registrados.

RN06 O cálculo de atrasos deve considerar o horário previsto para o início da jornada.

RN07 O cálculo de horas extras deve considerar o tempo trabalhado além da jornada prevista.
