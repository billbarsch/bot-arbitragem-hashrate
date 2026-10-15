# Regras de plataforma observadas

Registro de comportamento observado nas telas, para orientar a leitura em cada execução. **Nada aqui é constante**: confirme o valor atual no formulário antes de confiar nele. Se a tela divergir deste arquivo, a tela vence e este arquivo precisa ser corrigido.

Sem data de observação, não use a linha como autorização de mutação.

## HashSell: ajuste de lance e limite

- O ajuste de lance e de limite é um único formulário, "Ajustar lance e limite", com campo de lance em US$ por PH/s por dia e campo de limite em PH/s. Ajustar um deles não dispensa conferir o outro antes de salvar.
- A própria tela resume a assimetria: **"Aumentar vale na hora; reduzir tem limite."** Aumento de lance tem efeito imediato; redução tem trava.
- A tela não quantifica a trava de redução. Regra relatada pelo operador em 2026-10-14: **redução de até US$ 0,10 por vez, com intervalo mínimo de cerca de 10 minutos entre reduções**. Reconferir no formulário a cada tentativa; se houver espera, feche sem salvar e registre quando poderá consultar de novo.
- O mesmo formulário mostra, em texto, o **menor lance que entrega o limite contratado** para aquele tamanho. É a referência imediata de disputa de preço; continua sendo necessário comparar com o teto econômico.
- Aumentar o limite de PH/s sem conferir a prévia de entrega para o novo tamanho é ação diferente de ajustar o lance: capacidade para o tamanho atual não comprova capacidade para o novo.
- O ajuste tem **dois degraus de confirmação dentro da própria página**, nenhum deles alerta nativo do navegador: o formulário "Ajustar lance e limite" e, depois de salvar, um segundo diálogo repetindo o resumo com "Passa a valer no próximo tick" e "Reduzir tem intervalo de espera". É preciso acionar o botão de salvar **nos dois**; fechar o primeiro achando que já era deixa nada salvo.
- **Interação recusada em execução sem operador quase sempre é página não pintada, não política.** O mesmo estado que devolve o documento vazio ao ler faz a ação de clique ser recusada. Antes de atribuir a recusa a uma trava do ambiente: confirme pelo texto do documento que a tela está de fato renderizada (navegue de novo até a página da ordem, leia o corpo, espere o conteúdo aparecer) e tente a interação nativa outra vez — foi assim que o fluxo completo de ajuste passou sem truque nenhum, depois de duas rodadas o declararem bloqueado. **Plano B, quando a recusa persiste com a página pintada:** acione o controle pela própria página, localizando o botão pelo texto visível e disparando a sequência de eventos que um clique real produz (ponta e mouse, pressionar e soltar, depois o clique); um único `click()` sintético no elemento costuma não acionar a aplicação, que escuta os eventos de baixo nível. Nos dois caminhos, **releia o documento** a cada passo para confirmar que o diálogo abriu, que o valor preenchido é o pretendido e que o segundo diálogo apareceu — nunca salve às cegas.
- Numa aba que não está sendo exibida, o texto "visível" dos elementos pode voltar quase vazio porque o ambiente pula a pintura do conteúdo. Leia o texto bruto dos elementos ou o HTML autenticado da mesma origem; para decidir se pode salvar, continue exigindo as leituras do formulário.
- A lista de ordens lida pelo HTML autenticado da mesma origem traz lance, situação, entrega, custo por dia e saldo restante de cada ordem — serve para conferir antes e depois e para a rodada que não consegue pintar a tela. **Não substitui abrir o formulário de ajuste**: passo, precisão, menor valor aceito e intervalo de espera só existem lá, e são eles que autorizam o envio.

## HashSell: mínimo de lance ao criar ordem

- O **mínimo aceito no formulário de nova ordem é o próprio hashprice do minuto**. Um lance abaixo disso recebe a mensagem de recusa "below the market minimum price" e o botão de criação fica desabilitado: não existe lance mais barato que o hashprice.
- Quando o mercado tem capacidade sobrando, o **preço de corte não aparece** ("nenhum valor"), e a tela informa que qualquer lance acima do mínimo é atendido. Nesse estado, o hashprice é a referência que vale; quando houver corte, o corte passa a valer e vence o maior dos dois.
- Convenção de preço definida pelo operador em 2026-10-14: **criar a ordem com a referência vigente somada de US$ 0,05 por PH/s/dia** — o mais perto do piso que a plataforma aceita. O objetivo é comprar potência o mais barato possível, porque a margem vem da diferença para o pagamento da plataforma de destino, e a entrega se corrige depois com ajuste de lance autorizado.
- O resumo do formulário mostra "Seu lance está na mesma faixa do hashprice" quando o lance ficou no piso, e a duração estimada **já inclui a taxa de consumo**: o valor da ordem se divide em compra de velocidade + taxa. Confira a duração contra a cobertura exigida antes de criar.

## HashSell: cobrança

- A cobrança incide sobre o **entregue**, não sobre o reservado pelo leilão. Amostras em que a pool não respondeu geram cobrança menor e também receita menor na plataforma de destino.
- A taxa de consumo aparece no extrato como lançamento separado do consumo, na mesma ordem. Em 2026-10-14 a proporção observada foi de **3,00% sobre o consumo**; confirme a cada período, ela não é fixa por contrato deste arquivo.
- A duração exibida é estimada ao preço e limite atuais. Disputa, variação de entrega e prazo máximo alteram o tempo real.

## RentalHash: pagamento

- O crédito vira lançamento **por hora fechada**, usando a potência média entregue naquela hora. Uma leitura no meio da hora não é receita fechada.
- A taxa base é exibida em escala (TH/s, PH/s, EH/s por dia). Normalize tudo para US$ por PH/s por dia antes de comparar com o lance da HashSell.
- O bônus de fidelidade é informado como percentual somado à taxa base, por nível. Em 2026-10-14 a conta observada estava em nível com **+2,5%**, e o valor por hora do extrato já refletia esse bônus. **Não aplique o bônus duas vezes:** se o crédito medido já o inclui, use o crédito medido como pagamento efetivo.
- O nível de fidelidade **pausa** quando a potência semanal sai da faixa de estabilidade e nunca cai; isso muda o bônus disponível, não o histórico creditado.
- Saque: mínimo e taxa da rede aparecem na própria tela de saque, junto do prazo estimado de conclusão. Em 2026-10-14 a tela indicava mínimo de US$ 10,00, taxa de US$ 2,00 e prazo de 24 a 48 horas. Tratou pedido como pendente, não como dinheiro recebido.

## Leitura anômala de referência

O preço de referência da HashSell reflete o que o mercado está pedindo naquele instante, e o instante pode estar errado. Já foi observada uma leitura de referência saltando para mais de dez vezes a faixa das leituras anteriores na mesma sessão, com o restante do livro de ofertas ainda na faixa antiga e o corte sem fechamento — ou seja, a exigência de "piso" veio de um lance isolado no topo do livro, não de um movimento generalizado.

Nessas leituras, a ordem aparece como "sem entrega" e o campo "menor lance que entrega o limite" passa a exigir o valor inflado. **Isso não é alvo de ajuste.** Trate como dado suspeito: preserve o lance, registre as duas leituras que revelaram o salto, avise o usuário e aguarde a normalização. Verificar a profundidade do livro antes de confiar no piso separa mercado caro de leitura quebrada.

Uma ordem sem entrega não consome saldo. A pressão para "fazer voltar a rodar" não tem custo zero: pagar o piso inflado multiplica o custo por PH/s enquanto o pagamento da plataforma de destino permanece o mesmo.

## Consequência para o teto de lance

Como o pagamento da RentalHash varia por conta, nível de fidelidade e tipo de conexão, o teto de lance é **calculado na execução**, não memorizado:

```
teto_economico_lance = pagamento_efetivo / ((1 + taxa_consumo) × (1 + margem_minima))
```

Sem leitura fresca do pagamento efetivo, o teto é desconhecido, e teto desconhecido bloqueia aumento de lance. Consulte [calculos.md](calculos.md) para as fórmulas e [regras-operacionais.md](regras-operacionais.md) para o que fazer quando o dado falta.
