---
name: criar-ordem-hashsell
description: Prepara ou cria uma ordem HashSell com destino RentalHash, velocidade, lance e orçamento validados. Use para uma nova ordem ou recriação explicitamente autorizada de uma ordem encerrada.
---

# Criar ordem HashSell

Leia [as regras comuns](../configurar-bot-hashrate/referencias/regras-operacionais.md), [os cálculos](../configurar-bot-hashrate/referencias/calculos.md) e [o registro local](../configurar-bot-hashrate/referencias/registro-local.md). Criar e recriar são permissões distintas.

1. Navegue para `https://www.hashsell.com/orders`. Confira se já existe a ordem pretendida ou uma intenção enviada/incerta de criação. Uma ordem encerrada não autoriza recriação automática, e uma lista vazia por falha de carregamento não prova ausência de ordens.
2. Confira a conta compradora e o destino validado em `https://www.hashsell.com/pools`. Verifique associação exata à conta RentalHash, host, porta e conexão. Se o destino ainda não existir, prepare seu cadastro antes de criar a ordem.
3. Defina tipo de ordem, velocidade, lance e aporte conforme pedido/política. Não escolha automaticamente toda a carteira. Para cobertura em horas, estime custo incluindo as taxas atuais; confira mínimo, taxa de criação, reserva, orçamento e exposição das demais ordens.
4. Leia o pagamento RentalHash aplicável e a disponibilidade HashSell no tamanho pretendido. Confira margem mínima e teto de preço. Prévia de entrega é estimativa, não promessa de retorno.
5. Abra “Nova ordem” na interface. Preencha tipo, pool, unidade, velocidade, lance e valor. Releia o resumo da plataforma: débito total, taxas, saldo utilizável, duração, destino e prévia de entrega. Não assuma que uma ordem fixa esteja disponível ou que todos os tipos tenham as mesmas regras.
6. Faça uma segunda leitura do disponível e das cotações. Registre a intenção; confirme somente dentro do modo autorizado. Se o usuário pediu para concluir pessoalmente, pare com o formulário conferido.
7. Reabra a nova ordem e confira identificador, tipo, destino, lance, limite, aporte e extrato da carteira. Registre entrega após a atualização indicada pela plataforma. Atualize o identificador gerenciado **na configuração privada**, mantendo os limites e o vínculo com a ordem anterior quando for substituição.

Nunca altere o texto público das skills para incluir um identificador real de ordem. Não envie dados ao Git como parte da criação. Resultado incerto exige reconciliação antes de recriar ou repetir.
