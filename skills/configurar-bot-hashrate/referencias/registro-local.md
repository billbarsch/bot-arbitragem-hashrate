# Estado privado, registros e prevenção de duplicatas

Todos os arquivos deste procedimento ficam no diretório privado escolhido na configuração. Não crie registros de operações reais no repositório público.

## Estado de execução

Mantenha `estado-execucao.json` com versão, conversa autorizada, última verificação, cotação observada, ordens selecionadas, contadores por período, agendamento existente e operações pendentes. Não guarde senhas, códigos temporários, cookies ou tokens nele.

A cotação da RentalHash é um campo do estado, não uma memória da conversa. Grave-a em `cotacao_rentalhash_vigente` com `lida_em`, `valida_ate` (calculada por `validade_maxima_cotacao_segundos`), o valor efetivo por PH/s/dia, o nível de fidelidade e a `fonte` da leitura, e derive dela `calculos_vigor` com `taxa_consumo`, `lance_de_perda`, `teto_economico` e os alvos de escada por ordem. **Toda rodada que não conseguir renderizar o painel da RentalHash consulta esses dois campos antes de declarar o pagamento indisponível** — painel ilegível não apaga a última leitura dentro da validade, e um recorde de "ilegível" não significa que nunca houve leitura se a própria rodada gravou a cotação. Só depois de vencida a validade o pagamento volta a ser dado indisponível. Ao ler o painel com sucesso, substitua o campo na hora, com horário e fonte; cotação boa que não foi gravada equivale a cotação inexistente para a próxima rodada.

O que fica adiado também é estado, não memória: grave `escada_pendente` com `ordem`, `proximo_lance`, `permitido_a_partir` (hora fechada da janela de espera) e `motivo`, mais `ultima_reducao_em` por ordem. **Mantenha um item por ordem, com a janela contada do salvamento daquela ordem** — a trava de uma não congela as demais, então uma lista em que só uma ordem tem degrau marcado enquanto as outras estão paradas por uma espera que não é delas é um registro errado, não uma decisão conservadora. A rodada seguinte começa executando esse degrau se a janela tiver passado e a entrega continuar; assim a escada não depende de a conversa anterior lembrar dela. Quando o degrau for executado, substitua o campo pelo próximo ou marque a escada encerrada no piso.

O que a rodada já aprendeu também é estado, não memória da conversa: grave `atalhos` — caminho verificado até cada tela, rótulos e formatos já decifrados, travas e passos que o formulário imprime, ordem de coleta que funcionou — sempre com **o que o invalidate** (uma contagem de rodadas, uma data, "a tela mudou de layout", "o valor divergiu"). Um atalho é hipótese barata, nunca fato: encurta a coleta de **leitura**, jamais a guarda, e quando a tela diverge dele a tela ganha, o campo é corrigido na hora e a divergência vira linha `problema` no diário. Número de dinheiro (lance, referência, piso, entrega, saldo, pagamento) não entra em `atalhos`: é lido na fonte toda rodada.

Antes de uma execução recorrente, adquira uma exclusividade local por configuração/conjunto de contas. Use o mecanismo atômico que o ambiente oferecer. Registre proprietário, início e validade. Uma segunda execução consulta o bloqueio e encerra sem ações quando outra está ativa. Não remova bloqueio apenas porque está antigo: confira que a execução anterior terminou e que não há operação pendente. Sem mecanismo confiável de exclusividade, não habilite mutações recorrentes concorrentes.

Antes de confirmar uma operação, persista uma intenção com identificador único, conta, ordem/destino, ação, valor, horário, autorização, limites conferidos e estado `preparada`. Esse identificador permite reconciliar; não presuma que a plataforma possui um campo de prevenção de duplicatas. Depois do envio, registre `enviada`, `confirmada`, `aguardando_confirmacao`, `incerta`, `bloqueada` ou `falhou`, conforme a evidência.

Se ocorrer interrupção entre persistir e confirmar, confira a plataforma antes de decidir. Se o envio tiver resultado incerto, bloqueie repetição da mesma ação até resolver pelo histórico/identificador/comprovante. Não crie outro saque ou aporte para compensar a falta de resposta.

## Registro de operações

Acrescente eventos em `registro-operacoes.jsonl`, sem apagar os anteriores, usando campos em português:

- identificador da execução e da intenção;
- data, hora, fuso e fonte da leitura;
- conta, ordem e ação;
- preço, velocidade, saldo e taxa antes/depois, com unidades;
- **percentual de lucro (margem) da ordem e o pagamento que o produziu**, como campos, não como comentário;
- motivo, autorização e limites aplicados;
- situação observada, identificador do pedido e comprovante quando houver.

### Linha-resumo obrigatória

O arquivo é o **log da operação**, lido por uma pessoa para responder "o que o agente fez e por quê". Toda rodada escreve pelo menos uma linha, inclusive a que não alterou nada. Cada linha usa o mesmo conjunto de chaves — não invente nomes novos a cada execução, porque isso quebra a consulta por campo.

Campos mínimos: `quando` (ISO com fuso), `execucao`, `conta`, `ordem`, `acao`, `resultado`, `resumo` (uma frase, sem jargão), `antes`, `depois`, `motivo`. São os nomes de todas as rodadas, inclusive as recorrentes: uma linha com `em`/`conversa` no lugar de `quando`/`execucao` quebra a consulta por campo, mesmo que o conteúdo esteja certo.

**Toda linha que diz respeito a dinheiro traz também `margem_percentual` e `pagamento_utilizado`** — `verificacao`, `aumentar_lance`, `diminuir_lance`, `recusa_de_ajuste`, `cancelar_ordem` e `adicionar_credito`. A margem é o número que responde "esta ordem está lucrando?"; ela precisa ser **campo consultável**, não prosa dentro de `resumo`, porque é por ele que se compara rodada a rodada e se detecta a margem virar negativa. Anote:

- `margem_percentual`: lucro percentual esperado da ordem com o lance daquela linha (`pagamento_utilizado / (lance × (1 + taxa_consumo)) − 1`, em porcentagem, uma ou duas casas). Em linha de ajuste de lance, é a margem **resultante**, depois do salvamento — a do lance antigo já está na linha anterior.
- `pagamento_utilizado`: o valor efetivo por PH/s/dia que produziu essa margem, com o horário da leitura. Sem esse campo a margem da linha não é verificável depois: duas rodadas podem reportar 9 % a partir de pagamentos diferentes, e só a cotação gravada no estado mostra qual delas estava certa.
- Pagamento ilegível e dentro da validade do estado: use `cotacao_rentalhash_vigente` e registre em `motivo` que a margem veio da cotação anterior e de quando ela foi lida. Margem que não se pode calcular é `margem_percentual: null` com o motivo, nunca um número chutado.

**Carimbo de tempo é o valor local do fuso declarado, nunca um instante UTC rotulado com o sufixo do fuso.** Se o único relógio disponível devolver UTC, converta antes de escrever; anexar `-03:00` a um valor UTC adianta a linha em três horas. O mesmo instante deve aparecer idêntico no log e nos campos do estado (`permitido_a_partir`, `ultima_reducao_em`, `valida_ate`), porque é a partir deles que a próxima rodada decide se a janela de espera abriu — um carimbo adiantado faz a escada esperar três horas a mais, um atrasado faz duas rodadas salvarem na mesma janela.

**Formato aceito é o ISO estrito com dois pontos no fuso: `AAAA-MM-DDTHH:MM:SS-03:00`.** Nada de vírgula de milissegundos herdada do relógio da máquina (`2026-01-01T02:55:03,385-03:00`), nada de `Z` em campo de fuso local, nada de data sem hora. O campo é escrito por uma rodada e parseado por outra: o `Date` que rejeita a vírgula devolve `Invalid Date`, e a leitura inválida faz a rodada tratar a janela como fechada — duas reduções no mesmo minuto, na mesma ordem, contra a trava da plataforma. Se a ferramenta de data só produz milissegundos, descarte-os (`…SS`) antes de gravar, e confira que o valor escrito volta a ser lido como o mesmo instante.

Vocabulário de `acao` para o log ficar pesquisável:

| `acao` | Quando usar |
| --- | --- |
| `verificacao` | Rodada que só leu e registrou; `resultado: nenhuma_acao`. |
| `aumentar_lance` / `diminuir_lance` | Lance movido, com valor antes e depois na mesma unidade. |
| `recusa_de_ajuste` | A direção estava certa, mas teto econômico, margem mínima, passo, espera ou a guarda de anomalia impediram a mudança. |
| `criar_ordem` / `recriar_ordem` / `cancelar_ordem` | Ordem criada, recriada ou cancelada — e também a decisão de **não** cancelar, registrada como `recusa_de_ajuste` com `motivo` explicando que o cancelamento foi considerado para evitar perda. |
| `adicionar_credito` / `solicitar_saque` | Movimentação autorizada e executada. |
| `bloqueio` | Exclusividade ocupada por outra execução; ou cotação vencida e painel ilegível, o que encerra a rodada na subida — a descida em escada continua permitida. |
| `configuracao` | Mudança fora das ordens: autorização concedida ou revogada, limite definido, agendamento criado ou reescrito, pool cadastrada. Registre antes e depois do que mudou. |
| `problema` | Login expirado, entrega perdida, saldo sem cobertura, divergência de extrato, campo fora do esperado, resultado incerto a reconciliar. |

Exemplo de duas linhas seguidas:

```
{"quando":"2026-01-01T10:11:00-03:00","execucao":"R-0001","conta":"hashsell-principal","ordem":"HS-XXXXXX","acao":"verificacao","resultado":"nenhuma_acao","antes":"40,2500","depois":"40,2500","margem_percentual":8.5,"pagamento_utilizado":"45,00 lida às 10:08","resumo":"Li as duas ordens, a carteira e a RentalHash; o lance já estava coerente, não mexi.","motivo":"Lance acima do piso e margem 8,5% acima do mínimo de 1%."}
{"quando":"2026-01-01T10:22:00-03:00","execucao":"R-0002","conta":"hashsell-principal","ordem":"HS-XXXXXX","acao":"aumentar_lance","resultado":"confirmada","antes":"40,2500","depois":"40,8000","margem_percentual":7.1,"pagamento_utilizado":"45,00 lida às 10:20","resumo":"Ordem perdeu entrega; subi o lance ao piso informado pela plataforma, dentro do teto de 43,25.","motivo":"Piso de entrega 40,80; pagamento RentalHash 45,00 lido agora; margem resultante 7,1%."}
```

`resumo` descreve o fato e o número, não a intenção: "subi o lance de 40,25 para 40,80 porque a ordem ficou sem entrega" serve; "ajustei conforme a política" não serve. Registros com `resultado: incerta` ou `falhou` exigem linha extra quando forem reconciliados, apontando o identificador da intenção original.

Endereços completos necessários à reconciliação podem ficar na configuração privada; use referências de carteira no registro. Nunca registre segredo, código de autenticação ou link de confirmação. Exiba o registro mais recente primeiro ao consultar, preservando o arquivo histórico. Grave estado por substituição atômica e preserve dados existentes; não sobrescreva uma configuração inteira para atualizar apenas um campo.

## Saques e limites acumulados

Confira tanto o histórico oficial quanto as intenções locais ainda aguardando confirmação. Conte pedidos enviados ou incertos para reservar os limites até esclarecer seu resultado. Use a mesma intenção ao atualizar de pendente para concluído. Reconciliar mudança de situação não cria nova receita.

Compare recargas e saques com seus limites por operação e por dia no fuso configurado, incluindo ações manuais visíveis na plataforma. Se não houver cobertura suficiente do histórico para conferir um limite acumulado, mantenha a ação pendente.
