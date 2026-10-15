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

### Linha-resumo obrigatória

O arquivo é o **log da operação**, lido por uma pessoa para responder "o que o agente fez e por quê". Toda rodada escreve pelo menos uma linha, inclusive a que não alterou nada. Cada linha usa o mesmo conjunto de chaves — não invente nomes novos a cada execução, porque isso quebra a consulta por campo.

Campos mínimos: `quando` (ISO com fuso), `execucao`, `conta`, `ordem`, `acao`, `resultado`, `resumo` (uma frase, sem jargão), `antes`, `depois`, `motivo`.

Vocabulário de `acao` para o log ficar pesquisável:

| `acao` | Quando usar |
| --- | --- |
| `verificacao` | Rodada que só leu e registrou; `resultado: nenhuma_acao`. |
| `aumentar_lance` / `diminuir_lance` | Lance movido, com valor antes e depois na mesma unidade. |
| `recusa_de_ajuste` | A direção estava certa, mas teto econômico, margem mínima, passo, espera ou a guarda de anomalia impediram a mudança. |
| `criar_ordem` / `recriar_ordem` / `cancelar_ordem` | Ordem criada, recriada ou cancelada — e também a decisão de **não** cancelar, registrada como `recusa_de_ajuste` com `motivo` explicando que o cancelamento foi considerado para evitar perda. |
| `adicionar_credito` / `solicitar_saque` | Movimentação autorizada e executada. |
| `bloqueio` | Exclusividade ocupada por outra execução, ou pagamento/cotação ilegível: a rodada encerra sem agir. |
| `configuracao` | Mudança fora das ordens: autorização concedida ou revogada, limite definido, agendamento criado ou reescrito, pool cadastrada. Registre antes e depois do que mudou. |
| `problema` | Login expirado, entrega perdida, saldo sem cobertura, divergência de extrato, campo fora do esperado, resultado incerto a reconciliar. |

Exemplo de duas linhas seguidas:

```
{"quando":"2026-01-01T10:11:00-03:00","execucao":"R-0001","conta":"hashsell-principal","ordem":"HS-XXXXXX","acao":"verificacao","resultado":"nenhuma_acao","antes":"36,4707","depois":"36,4707","resumo":"Li as duas ordens, a carteira e a RentalHash; o lance já estava coerente, não mexi.","motivo":"Lance acima do piso e margem 9,1% acima do mínimo de 1%."}
{"quando":"2026-01-01T10:22:00-03:00","execucao":"R-0002","conta":"hashsell-principal","ordem":"HS-XXXXXX","acao":"aumentar_lance","resultado":"confirmada","antes":"36,4707","depois":"36,6200","resumo":"Ordem perdeu entrega; subi o lance ao piso informado pela plataforma, dentro do teto de 39,41.","motivo":"Piso de entrega 36,62; pagamento RentalHash 41,00 lido agora; margem resultante 5,1%."}
```

`resumo` descreve o fato e o número, não a intenção: "subi o lance de 36,4707 para 36,62 porque a ordem ficou sem entrega" serve; "ajustei conforme a política" não serve. Registros com `resultado: incerta` ou `falhou` exigem linha extra quando forem reconciliados, apontando o identificador da intenção original.

Endereços completos necessários à reconciliação podem ficar na configuração privada; use referências de carteira no registro. Nunca registre segredo, código de autenticação ou link de confirmação. Exiba o registro mais recente primeiro ao consultar, preservando o arquivo histórico. Grave estado por substituição atômica e preserve dados existentes; não sobrescreva uma configuração inteira para atualizar apenas um campo.

## Saques e limites acumulados

Confira tanto o histórico oficial quanto as intenções locais ainda aguardando confirmação. Conte pedidos enviados ou incertos para reservar os limites até esclarecer seu resultado. Use a mesma intenção ao atualizar de pendente para concluído. Reconciliar mudança de situação não cria nova receita.

Compare recargas e saques com seus limites por operação e por dia no fuso configurado, incluindo ações manuais visíveis na plataforma. Se não houver cobertura suficiente do histórico para conferir um limite acumulado, mantenha a ação pendente.
