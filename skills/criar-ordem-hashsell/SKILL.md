---
name: criar-ordem-hashsell
description: Prepara ou cria uma ordem HashSell com destino RentalHash, velocidade, lance no piso do mercado e orçamento validados. Use para uma nova ordem ou recriação explicitamente autorizada de uma ordem encerrada.
---

# Criar ordem HashSell

Leia [as regras comuns](../configurar-bot-hashrate/referencias/regras-operacionais.md), [os cálculos](../configurar-bot-hashrate/referencias/calculos.md), [as regras observadas da plataforma](../configurar-bot-hashrate/referencias/regras-da-plataforma.md) e [o registro local](../configurar-bot-hashrate/referencias/registro-local.md). Criar e recriar são permissões distintas.

1. Navegue para `https://www.hashsell.com/orders`. Confira se já existe a ordem pretendida ou uma intenção enviada/incerta de criação. Uma ordem encerrada não autoriza recriação automática, e uma lista vazia por falha de carregamento não prova ausência de ordens.
2. Confira a conta compradora e o destino validado em `https://www.hashsell.com/pools`. Verifique associação exata à conta RentalHash, host, porta e conexão. Se o destino ainda não existir, prepare seu cadastro antes de criar a ordem.
3. Escolha o lance a partir da **referência em vigor no minuto da criação**: o preço de corte, quando o mercado fechou e ele está exibido; o hashprice, quando a tela informa que há capacidade sobrando e não há corte. Se os dois existirem, use o maior — é o que vale primeiro. Some o incremento do usuário (por padrão US$ 0,05 por PH/s/dia, o valor da configuração privada) e arredonde para a precisão aceita pelo formulário.
4. Trate `referência + incremento` como o **menor lance utilizável**, não como alvo de disputa. A plataforma recusa lance abaixo do hashprice, e sem corte qualquer lance acima do mínimo é atendido. Abaixe o incremento só com decisão explícita do usuário; nunca suba por cima do teto econômico (`pagamento_efetivo / ((1 + taxa_consumo) × (1 + margem_minima))`) nem para acompanhar referência anômala. Reconfirme corte e hashprice imediatamente antes de enviar: a referência muda a cada minuto.
5. Defina tipo de ordem e o aporte conforme o pedido. **Não drene a carteira por iniciativa própria**; use todo o saldo disponível apenas quando o usuário pedir o saldo inteiro, e nesse caso registre o valor lido como orçamento máximo desta ordem. Com aporte escolhido e duração alvo exigida, limite a velocidade por `velocidade_maxima = saldo × 24 / (custo_unitario × cobertura_alvo_horas)` — para 24 horas, `saldo / custo_unitario` — e confirme a duração na prévia da plataforma, que já inclui a taxa de consumo. Confira mínimo de lance, mínimo de velocidade, mínimo de aporte, taxa de criação, reserva e a exposição das demais ordens vivas.
6. Leia o pagamento RentalHash aplicável e a disponibilidade HashSell no tamanho pretendido. Confira margem mínima e teto de preço. Prévia de entrega é estimativa, não promessa de retorno.
7. Abra “Nova ordem” na interface. Preencha tipo, pool, unidade, velocidade, lance e valor. Releia o resumo da plataforma: débito total, taxas, saldo utilizável, duração, destino e prévia de entrega. Não assuma que uma ordem fixa esteja disponível ou que todos os tipos tenham as mesmas regras.
8. Faça uma segunda leitura do disponível e das cotações. Registre a intenção; confirme somente dentro do modo autorizado. Se o usuário pediu para concluir pessoalmente, pare com o formulário conferido.
9. Reabra a nova ordem e confira identificador, tipo, destino, lance, limite, aporte e extrato da carteira. Registre entrega após a atualização indicada pela plataforma. Atualize o identificador gerenciado **na configuração privada**, mantendo os limites e o vínculo com a ordem anterior quando for substituição.

Nunca altere o texto público das skills para incluir um identificador real de ordem. Não envie dados ao Git como parte da criação. Resultado incerto exige reconciliação antes de recriar ou repetir.
