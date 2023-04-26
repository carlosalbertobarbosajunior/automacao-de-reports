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
###   - Report diário de faturamento:
  Envia uma tabela, com formatação html, via e-mail contendo a relação das notas fiscais emitidas no mês que são a projeto (faturamento).<br>
  Utiliza a função is_first_business_day() para identificar se o dia atual é o primeiro dia útil do mês. Caso seja, o report é enviado com base nas notas do mês anterior, como um resumo do faturamento do mês recém-finalizado.
