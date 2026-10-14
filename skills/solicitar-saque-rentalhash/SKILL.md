---
name: solicitar-saque-rentalhash
description: Prepara ou solicita saque RentalHash para carteira validada, conferindo conta, saldo, moeda, rede, taxa, limites e pedidos existentes. Use para retirar saldo ou executar uma política de saques previamente autorizada.
---

# Solicitar saque RentalHash

Leia [as regras comuns](../configurar-bot-hashrate/referencias/regras-operacionais.md), [a configuração privada](../configurar-bot-hashrate/referencias/configuracao-privada.md) e [o registro local](../configurar-bot-hashrate/referencias/registro-local.md).

1. Navegue para `https://rentalhash.com/painel/financeiro`; confirme a conta. Confira saldo **disponível**, mínimos e taxas atuais. A creditar não é saldo sacável. Consulte pedidos aguardando confirmação, pendentes e recentes para prevenir duplicação e conferir limites acumulados.
2. Leia a carteira autorizada exclusivamente da configuração privada. Valide moeda, rede, destinatário, endereço completo e eventual identificador adicional contra as instruções de depósito do destino. Formato de endereço não prova compatibilidade de rede. Se a rede estiver desconhecida, deixe o saque pendente e pergunte; não escolha a mais barata por conta própria.
3. Defina o valor: montante pedido ou regra privada de valor fixo/todo disponível. “Sacar tudo” só quando autorizado. Confira gatilho, reserva, máximo por operação/dia, quantidade, intervalo e horários, no fuso configurado. Uma aprovação para saque único não habilita saques recorrentes.
4. Preencha valor, moeda, rede e endereço com a fonte privada confiável. Releia o endereço completo no formulário e compare caractere a caractere. Confira o identificador adicional se exigido; não o omita por não aparecer em um exemplo.
5. Confira uma segunda vez conta, valor bruto, taxa, líquido, moeda e rede. Se o destinatário for HashSell, confirme que a moeda/rede é aceita no depósito da conta correta e que o líquido atende ao mínimo do destino. Um pedido pode sair da RentalHash e ainda não estar creditado na HashSell.
6. Registre a intenção. Execute a confirmação final somente dentro da autorização e das regras do ambiente. Se a plataforma exigir autenticação, email ou código, deixe o usuário concluir quando necessário. Não acesse email nem registre links/códigos por implicação de um pedido de saque.
7. Registre o estado observado: preparado, aguardando confirmação, solicitado, pendente, concluído ou incerto. Confira depois no histórico oficial. Conclusão na plataforma e recebimento no destino são verificações distintas; use comprovante de transação para reconciliar quando disponível.

Se o resultado for incerto, não peça outro saque nem reenvie confirmação automaticamente. O mesmo pedido muda de situação; não o conte duas vezes. Reenviar email, cancelar pedido ou trocar carteira exige escopo próprio. Relate bruto, taxa, líquido e pendência com endereço mascarado, sem promessa de prazo.
