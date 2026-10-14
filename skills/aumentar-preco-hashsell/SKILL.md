---
name: aumentar-preco-hashsell
description: Aumenta o lance de uma ordem HashSell autorizada para buscar entrega dentro do teto de preço e da margem definidos pelo usuário. Use para subir preço; não altera velocidade nem adiciona saldo.
---

# Aumentar lance da HashSell

Leia [as regras comuns](../configurar-bot-hashrate/referencias/regras-operacionais.md), [os cálculos](../configurar-bot-hashrate/referencias/calculos.md) e os limites privados da ordem. Sem autorização suficiente, prepare o formulário e deixe a confirmação para o usuário.

1. Navegue para `https://www.hashsell.com/orders` e abra a ordem selecionada. Confira conta, destino, situação, lance, limite e entrega. Diagnostique se a disputa de preço é a causa; aumento não corrige credenciais inválidas, ordem encerrada ou saldo inexistente.
2. Leia uma referência atual de entrega para **essa velocidade**, taxa de consumo e pagamento RentalHash da conta destinatária. Calcule o alvo com o incremento autorizado e a precisão do formulário. Não adote incremento, bônus ou taxa fixos de outro usuário.
3. O alvo deve ser **estritamente maior** que o lance atual, menor ou igual ao teto autorizado e ao teto econômico, e dentro da variação por execução. Confira a margem mínima incluindo taxas fixas do horizonte quando conhecido. Se os limites não permitirem entrega, relate a incompatibilidade sem aumentar.
4. Abra o ajuste de lance. Preserve a velocidade e os demais campos. Preencha o valor alvo usando os recursos disponíveis e releia o campo e o resumo. Confira unidade, separadores, preço anterior/novo, custo projetado, margem e velocidade inalterada.
5. Faça uma leitura nova dos dados relevantes. Se o preço necessário ou a taxa mudou, recalcule antes de enviar. Persistindo os limites, registre a intenção local, salve e confirme apenas no modo autorizado.
6. Reabra a ordem. Confira lance persistido, limite inalterado, custo coerente e entrega após a atualização indicada pela plataforma. A entrega pode variar por disponibilidade; não repita o aumento só porque uma amostra demorou.
7. Registre o antes/depois e o resultado real. Faça no máximo um ajuste desta direção por ordem na execução. Em espera, erro ou resultado incerto, encerre a tentativa e reconcilie antes de qualquer repetição.

Pagar acima do necessário reduz a margem. Nunca interprete “manter rodando” como autorização para exceder teto, orçamento ou margem mínima.
