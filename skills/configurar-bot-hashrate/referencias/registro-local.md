# Estado privado, registros e prevenção de duplicatas

Todos os arquivos deste procedimento ficam no diretório privado escolhido na configuração. Não crie registros de operações reais no repositório público.

## Estado de execução

Mantenha `estado-execucao.json` com versão, conversa autorizada, última verificação, cotação observada, ordens selecionadas, contadores por período, agendamento existente e operações pendentes. Não guarde senhas, códigos temporários, cookies ou tokens nele.

Antes de uma execução recorrente, adquira uma exclusividade local por configuração/conjunto de contas. Use o mecanismo atômico que o ambiente oferecer. Registre proprietário, início e validade. Uma segunda execução consulta o bloqueio e encerra sem ações quando outra está ativa. Não remova bloqueio apenas porque está antigo: confira que a execução anterior terminou e que não há operação pendente. Sem mecanismo confiável de exclusividade, não habilite mutações recorrentes concorrentes.

Antes de confirmar uma operação, persista uma intenção com identificador único, conta, ordem/destino, ação, valor, horário, autorização, limites conferidos e estado `preparada`. Esse identificador permite reconciliar; não presuma que a plataforma possui um campo de prevenção de duplicatas. Depois do envio, registre `enviada`, `confirmada`, `aguardando_confirmacao`, `incerta`, `bloqueada` ou `falhou`, conforme a evidência.

Se ocorrer interrupção entre persistir e confirmar, confira a plataforma antes de decidir. Se o envio tiver resultado incerto, bloqueie repetição da mesma ação até resolver pelo histórico/identificador/comprovante. Não crie outro saque ou aporte para compensar a falta de resposta.

## Registro de operações

Acrescente eventos em `registro-operacoes.jsonl`, sem apagar os anteriores, usando campos em português:

- identificador da execução e da intenção;
- data, hora, fuso e fonte da leitura;
- conta, ordem e ação;
- preço, velocidade, saldo e taxa antes/depois, com unidades;
- motivo, autorização e limites aplicados;
- situação observada, identificador do pedido e comprovante quando houver.

Endereços completos necessários à reconciliação podem ficar na configuração privada; use referências de carteira no registro. Nunca registre segredo, código de autenticação ou link de confirmação. Exiba o registro mais recente primeiro ao consultar, preservando o arquivo histórico. Grave estado por substituição atômica e preserve dados existentes; não sobrescreva uma configuração inteira para atualizar apenas um campo.

## Saques e limites acumulados

Confira tanto o histórico oficial quanto as intenções locais ainda aguardando confirmação. Conte pedidos enviados ou incertos para reservar os limites até esclarecer seu resultado. Use a mesma intenção ao atualizar de pendente para concluído. Reconciliar mudança de situação não cria nova receita.

Compare recargas e saques com seus limites por operação e por dia no fuso configurado, incluindo ações manuais visíveis na plataforma. Se não houver cobertura suficiente do histórico para conferir um limite acumulado, mantenha a ação pendente.
