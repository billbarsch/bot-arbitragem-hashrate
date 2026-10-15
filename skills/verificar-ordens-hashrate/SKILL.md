---
name: verificar-ordens-hashrate
description: Consulta ordens, entrega, saldos, custos, taxa de recebimento e margem na HashSell e RentalHash, sem alterar preços, crédito, ordens ou saques. Use para conferir saúde da operação e diagnosticar perda de entrega ou de rentabilidade.
---

# Verificar ordens e margem

Esta skill é **somente leitura**. Sugerir ajuste não autoriza executá-lo. Leia [as regras comuns](../configurar-bot-hashrate/referencias/regras-operacionais.md), [os cálculos](../configurar-bot-hashrate/referencias/calculos.md), [as regras de plataforma observadas](../configurar-bot-hashrate/referencias/regras-da-plataforma.md) e a configuração privada.

1. Navegue para `https://www.hashsell.com/orders`; confira identidade, filtros e paginação. Leia a lista completa das ordens relevantes. Separe ordens gerenciadas, outras ativas, encerradas e as que estão esperando entrega. Não selecione automaticamente todas as ordens para mutação.
2. Abra os detalhes das ordens selecionadas. Registre lance e unidade, limite de velocidade, entrega atual e amostras recentes, posição quando disponível, piso de entrega para o tamanho, corte, referência hashprice, saldo restante, duração e prazo máximo. Uma ordem viva pode estar sem entrega; uma leitura isolada não prova falha persistente.
3. Navegue para `https://www.hashsell.com/wallet`. Leia disponível, travado e extratos necessários. Consumo e taxas são custos; reserva e devolução são movimentações internas. Informe o corte e as limitações da coleta.
4. Na RentalHash, abra a conta correspondente ao destino de cada ordem: `https://rentalhash.com/painel`, `/painel/financeiro` e a área de fidelidade quando aplicável. Leia taxa vigente, bônus elegível para essa conexão, velocidade medida, conexão e últimos créditos fechados. Não atribua os números de uma conta a outra.
5. Calcule a margem usando **o lance da ordem**, a taxa de consumo atual e o pagamento aplicável. Separe margem unitária estimada, taxas fixas e lucro realizado do período fechado. Use o financeiro para corroborar recebimento, sem tratá-lo como preço atual.
6. Releia os dados recentes se houver mudança de hora, preço ou sessão durante a coleta. Dados indisponíveis permanecem indisponíveis. Diferencie disputa de preço, saldo baixo, conexão inválida, prazo máximo e atraso de crédito.
7. Registre a verificação privadamente e mostre um resumo: ordem mascarada/rótulo, lance, referência de entrega, PH/s entregues, cobertura de saldo, margem e eventual ação sugerida. Apresente saques pendentes como pendentes, nunca como dinheiro já recebido.

Se as contas não estiverem acessíveis, abra o login e permita que o usuário conclua. Não aumente preço, não complete crédito, não cancele nem solicite saque para resolver uma consulta.
