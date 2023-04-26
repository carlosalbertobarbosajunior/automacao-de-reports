# automacao-de-reports
Script de automação para alguns reports de necessidade ao menos semanal na HKM Indústria e Comércio.

## Função automation_reporting():
Recebe como argumento um dicionário cujas chaves são os nomes das ferramentas, e os valores são listas com as informações de cada arquivo na seguinte ordem:
caminho na rede [0], nome do relatório [1], caminho do relatório [2], e-mails de destino[3], título do e-mail[4] e corpo do e-mail[5]. Caso algum valor seja '', indica que a informação não é necessária para aquele relatório. Com essas informações a função abre o arquivo, sincroniza-o com o banco de dados da empresa, emite o relatório e o envia por e-mail caso haja necessidade.
A função ainda documenta o tempo de execução para cada arquivo, alimentando o txt 'runtime_reports' e também emite um e-mail de alerta para mim caso algum erro ocorra.

## Função is_first_business_day():
Identifica se o dia vigente é o primeiro dia útil do mês. E necessário para enviar informações financeiras do mês anterior ao invés de tentar enviar o faturamento do mês logo no primeiro dia útil. É utilizada para emitir o report diário de faturamento.
