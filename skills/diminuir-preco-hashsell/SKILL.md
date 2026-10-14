---
name: diminuir-preco-hashsell
description: Reduz o lance de uma ordem HashSell autorizada, conferindo preço de entrega, limite de redução e intervalo de espera. Use para baixar preço sem aumentar o lance, alterar a velocidade ou movimentar saldo.
---

# Diminuir lance da HashSell

Leia [as regras comuns](../configurar-bot-hashrate/referencias/regras-operacionais.md), [os cálculos](../configurar-bot-hashrate/referencias/calculos.md) e os limites privados.

1. Navegue para `https://www.hashsell.com/orders` e abra a ordem selecionada. Confira identidade, destino, situação, lance, limite, entrega, referência de corte e preço necessário para o tamanho quando disponível.
2. Calcule o menor alvo que atende à política de entrega com o incremento autorizado. Confira a taxa de consumo e o pagamento RentalHash atual para não interpretar lance mais baixo como garantia de lucro. Se o usuário aceitar entrega parcial ou espera, essa preferência precisa estar explícita.
3. Abra o ajuste e leia o passo de redução, precisão, menor valor aceito e intervalo de espera **atuais**. Não suponha percentuais ou segundos históricos. Se houver espera, feche sem salvar, registre quando poderá consultar de novo e encerre a tentativa.
4. O novo lance é o maior entre o alvo de entrega e o menor lance permitido naquela redução. Ele deve ser **estritamente menor** que o atual e respeitar a variação autorizada. Se ainda não atingir o alvo final, uma redução parcial permitida pode ser feita e informada; não realize várias reduções em sequência nesta execução.
5. Preserve a velocidade e os demais campos. Preencha, releia o valor e confira uma segunda leitura da referência, unidade e resumo. Se o alvo passou a comprometer a entrega exigida ou algum campo divergir, não confirme.
6. Registre a intenção local; salve e confirme dentro da autorização. Nunca aumente o preço nesta skill para “corrigir” uma tentativa. Em bloqueio ou resultado incerto, encerre e confira o histórico antes de repetir.
7. Reabra a ordem e confira lance persistido, limite inalterado e custo unitário reduzido. Registre entrega e amostras disponíveis. Uma redução não garante posição na fila nem entrega constante; informe se o mercado mudou, sem executar uma compensação não autorizada.

Se o lance já for adequado à política ou não puder ser reduzido agora, não salve uma alteração apenas para registrar atividade.
