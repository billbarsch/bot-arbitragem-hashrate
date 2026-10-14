---
name: adicionar-credito-hashsell
description: Adiciona crédito de saldo disponível da HashSell a uma ordem autorizada, por valor definido ou cobertura de horas. Use para prolongar uma ordem existente; não faz depósito externo, Pix nem transferência de outra plataforma.
---

# Adicionar crédito a uma ordem

Leia [as regras comuns](../configurar-bot-hashrate/referencias/regras-operacionais.md), [os cálculos](../configurar-bot-hashrate/referencias/calculos.md) e [o registro local](../configurar-bot-hashrate/referencias/registro-local.md).

1. Navegue para `https://www.hashsell.com/orders` e abra a ordem autorizada. Confira identidade, pool, situação, preço, velocidade, saldo restante, cobertura e prazo máximo. Não complete uma ordem encerrada.
2. Navegue para `https://www.hashsell.com/wallet` ou leia o disponível no diálogo. Confira a margem atual da conta RentalHash destinatária. Recarga automática exige saldo abaixo do gatilho autorizado e margem suficiente.
3. Use o valor pedido ou calcule **cobertura alvo menos saldo utilizável atual**, conforme a política. Não adicione novamente o orçamento total de cobertura. Confira mínimo atual, máximo por operação/dia, orçamento da ordem e reserva mínima da carteira. Se não couber, não reduza o valor nem esvazie a reserva silenciosamente; informe a limitação ou aplique uma regra parcial previamente autorizada.
4. Abra “Adicionar saldo” ou o controle equivalente. Limpe o valor padrão, preencha o valor aprovado e releia campo e resumo. Valor padrão do formulário não é o valor autorizado.
5. Antes da confirmação, releia disponível e saldo da ordem. Se um segundo diálogo omitir o valor, confira o formulário subjacente por um recurso confiável; se não puder relê-lo, volte ao formulário. Não confirme sem conferir o montante.
6. Registre a intenção local, envie dentro da autorização e confira o extrato, aporte acumulado e os saldos depois. O crédito transferido deve corresponder ao pedido. Quando não há consumo simultâneo, disponível cai e travado sobe igualmente; com ordem rodando, reconcilie também o consumo entre as duas leituras, em vez de exigir totais ao vivo idênticos.
7. Registre antes/depois e eventual nova duração. Se ocorrer erro após envio, consulte o histórico antes de tentar novamente. Uma intenção não reconciliada bloqueia nova recarga automática.

Adicionar saldo prolonga a cobertura financeira, mas não necessariamente o prazo máximo da ordem. Não crie outra ordem nem movimente carteira externa para compensar falta de saldo.
