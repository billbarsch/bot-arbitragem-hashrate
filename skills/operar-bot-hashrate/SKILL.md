---
name: operar-bot-hashrate
description: Coordena uma rodada do bot HashSell e RentalHash, consultando dados atuais e acionando somente ajustes, recargas, criação e saques autorizados. Use para operar a política configurada ou executar a rotina recorrente; consulta isolada usa a skill de verificação.
---

# Executar uma rodada do bot

Leia [as regras comuns](../configurar-bot-hashrate/referencias/regras-operacionais.md), [os cálculos](../configurar-bot-hashrate/referencias/calculos.md) e [o registro local](../configurar-bot-hashrate/referencias/registro-local.md). As demais skills deste pacote devem estar disponíveis juntas. Se alguma faltar, não improvise uma mutação; informe a dependência.

## Sequência

1. Localize configuração e conversa autorizadas. Em execução recorrente, confira que é a **mesma conversa** cadastrada. Adquira exclusividade e reconcilie intenções anteriores antes de movimentar saldo. Sem configuração, faça o cadastro inicial; sem autorização de mutação, execute somente leitura.
2. Use [verificar-ordens-hashrate](../verificar-ordens-hashrate/SKILL.md). Não opere sobre números antigos. Registre a cotação com horário e confira sua validade antes de cada mutação.
3. Selecione apenas ordens cadastradas e contas corretas. Para cada uma, calcule preço necessário, margem e cobertura. Respeite limites específicos da ordem **e** limites globais compartilhados. Não transfira permissões entre contas.
4. Se dimensionamento estiver habilitado, use [redimensionar-ordem-hashsell](../redimensionar-ordem-hashsell/SKILL.md), respeitando faixas, confirmações consecutivas e rampa. Releia o estado depois; uma mudança combinada de preço e velocidade já conta como ajuste nessa rodada.
5. Compare lance atual com alvo e tolerância autorizados. Use [aumentar-preco-hashsell](../aumentar-preco-hashsell/SKILL.md) ou [diminuir-preco-hashsell](../diminuir-preco-hashsell/SKILL.md) apenas quando a direção estiver justificada e autorizada. No máximo um ajuste de preço por ordem na rodada, incluindo eventual ajuste combinado do passo anterior.
6. Quando o saldo atingir o gatilho, use [adicionar-credito-hashsell](../adicionar-credito-hashsell/SKILL.md). Releia disponível após cada aporte: duas ordens não podem gastar o mesmo saldo. Não complete crédito com margem insuficiente ou resultado de ajuste ainda incerto.
7. Para ordens encerradas, use [criar-ordem-hashsell](../criar-ordem-hashsell/SKILL.md) somente se houver política explícita de recriação com destino, velocidade e orçamento válidos, e nenhuma criação pendente. Não replique uma ordem em cada rodada por o crédito voltar à carteira.
8. Confira se um saque está devido pela política atual, no fuso configurado, confrontando histórico oficial e intenções locais. Use [solicitar-saque-rentalhash](../solicitar-saque-rentalhash/SKILL.md) somente para a conta e a carteira autorizadas. Um pedido aguardando confirmação não é licença para um novo pedido igual.
9. Atualize estado e registro privados. Libere a exclusividade ao encerrar. Informe alterações e pendências de forma curta. Em recorrência, respeite a preferência de notificação; por padrão, alerte só mudanças relevantes, falhas persistentes ou necessidade de ação humana.

## Limites de uma operação contínua

Confira consumo real do dia e gasto máximo projetado até a próxima verificação, somando ordens. Não trate saldo reservado como custo realizado. Se a política diária puder ser ultrapassada por ordens já ativas, avise e use somente a resposta previamente autorizada; não suponha que um agendamento de consulta impede gastos entre rodadas.

Uma falha de conta, destino ou resultado incerto bloqueia mutações daquele escopo. Pode continuar consultas e ações independentes de outras contas apenas se não compartilharem o saldo ou o risco pendente. Em falha de armazenamento/exclusividade, não execute mutações recorrentes. Margem negativa bloqueia aumento de exposição; cancelamento requer autorização específica.
