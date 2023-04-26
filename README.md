# automacao-de-reports
Script de automação para alguns reports na HKM Indústria e Comércio.<br><br>
Atualmente, a empresa utiliza alguns documentos desenvolvidos em Excel com vínculo ao banco de dados para resumir informações, desenvolver relatórios ou criar ferramentas com interface amigável para o usuário. Esses documentos são facilitadores do controle das informações e tem como principal objetivo auxiliar nas tomadas de decisão estratégicas.<br><br>
Existe a necessidade de manter as informações plenamente sincronizadas com o banco de dados do ERP da empresa. Porém, algumas consultas a este banco são complexas e levam alguns minutos para acontecer. A solução utilizada é manter este script sendo executado diariamente, para que os principais arquivos sejam atualizados nos primeiros minutos do dia.

## Função automation_reporting():
Recebe como argumento um dicionário cujas chaves são os nomes das ferramentas, e os valores são listas com as informações de cada arquivo na seguinte ordem:<br>
caminho na rede [0], nome do relatório [1], caminho do relatório [2], e-mails de destino[3], título do e-mail[4] e corpo do e-mail[5]. Caso algum valor seja '', indica que a informação não é necessária para aquele relatório. Com essas informações a função abre o arquivo, sincroniza-o com o banco de dados da empresa, emite o relatório e o envia por e-mail caso haja necessidade.<br>
A função ainda documenta o tempo de execução para cada arquivo, alimentando o txt 'runtime_reports' e também emite um e-mail de alerta para mim caso algum erro ocorra.

## Função is_first_business_day():
Identifica se o dia vigente é o primeiro dia útil do mês. E necessário para enviar informações financeiras do mês anterior ao invés de tentar enviar o faturamento do mês logo no primeiro dia útil. É utilizada para emitir o report diário de faturamento.

## Parâmetros dos arquivos:
Cada dicionário representa um conjunto de arquivos que serão atualizados em um ou mais dias da semana. Esses dicionários são utilizados na função automation_reporting() de acordo com o dia da semana, determinado pela biblioteca datetime.

## Scripts com padrões que fogem à função automation_reporting():
#### &nbsp;&nbsp;&nbsp;&nbsp;- Report diário de faturamento:
&nbsp;&nbsp;&nbsp;&nbsp;Envia uma tabela, com formatação html, via e-mail contendo a relação das notas fiscais emitidas no mês que são a projeto (faturamento).<br>
&nbsp;&nbsp;&nbsp;&nbsp;Utiliza a função is_first_business_day() para identificar se o dia atual é o primeiro dia útil do mês. Caso seja, o report é enviado com base nas notas do mês anterior, como um resumo do faturamento do mês recém-finalizado.

#### &nbsp;&nbsp;&nbsp;&nbsp;- Alerta de calibração de máquinas de solda:
&nbsp;&nbsp;&nbsp;&nbsp;Através de uma planilha alimentada manualmente, este script relaciona as máquinas de solda que estão com calibração vencida, ou próxima do vencimento.<br>
&nbsp;&nbsp;&nbsp;&nbsp;Caso alguma máquina de solda se encaixe nessas situações, o script dispara um e-mail de alerta. Além disso, caso hajam máquinas vencidas e também outras prestes a vencer, o e-mail passa a conter as informações separadas em duas tabelas.

#### &nbsp;&nbsp;&nbsp;&nbsp;- Alerta de ações de não conformidade atrasadas:
&nbsp;&nbsp;&nbsp;&nbsp;O script sincroniza um relatório com ações de não conformidades cujo prazo esteja vencido. Após a sincronização, o algoritmo extrai o e-mail do responsável pela ação e as informações da não conformidade. A partir de um critério de número de dias de atraso, é emitido um alerta para um conjunto de e-mails.<br>
&nbsp;&nbsp;&nbsp;&nbsp;- De 1 a 7 dias de atraso: alerta emitido para o e-mail do colaborador + equipe de SGQ;<br>
&nbsp;&nbsp;&nbsp;&nbsp;- De 8 a 14 dias de atraso: alerta emitido para o e-mail do colaborador + equipe de SGQ + coordenador do colaborador;<br>
&nbsp;&nbsp;&nbsp;&nbsp;- 15 dias ou mais: alerta emitido para o e-mail do colaborador + equipe de SGQ + coordenador do colaborador + diretor. <br>
&nbsp;&nbsp;&nbsp;&nbsp;Caso o algoritmo não identifique o e-mail do colaborador com ação de não conformidade em atraso, o script indicará o erro e não prosseguirá com o envio de e-mails até que todos estejam cadastrados.

## Report de performance dos scripts:
Cada arquivo, report ou alerta realizado tem o tempo de execução medido através da biblioteca time, e posteriormente inserido no arquivo 'runtime_informations.txt'.<br>
Este bloco final de código lê todas as informações deste arquivo e executa gráficos com a biblioteca matplotlib para cada tipo de automação. Os gráficos contam com informações flutuantes de média e desvio padrão, e também linhas de máximo e mínimo. Para cada gráfico gerado, o mesmo salva na pasta 'pic'.<br>
Ao fim da construção dos gráficos, o algoritmo me envia um e-mail com todos os gráficos em anexo para que eu monitore diariamente se há alguma anomalia de performance.
