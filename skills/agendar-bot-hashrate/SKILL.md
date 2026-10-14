---
name: agendar-bot-hashrate
description: Cria, atualiza ou remove recorrência do bot HashSell e RentalHash usando o agendador disponível, sempre retomando a mesma conversa. Use quando o usuário pedir acompanhamento ou operação periódica.
---

# Agendar na mesma conversa

O agente escolhe o mecanismo de agendamento permitido pelo ambiente. Esta skill não exige um serviço ou sistema operacional específico. Leia [as regras comuns](../configurar-bot-hashrate/referencias/regras-operacionais.md), [a configuração privada](../configurar-bot-hashrate/referencias/configuracao-privada.md) e [o registro local](../configurar-bot-hashrate/referencias/registro-local.md).

1. Confira frequência ou horários, fuso e intenção do usuário: acompanhamento somente leitura ou operação dentro de uma política autorizada. Agendar acompanhamento não autoriza ajustes, crédito nem saques.
2. Identifique e registre privadamente a conversa atual. O agendador precisa conseguir **enviar uma nova execução para essa mesma conversa com seu contexto**, sem criar uma conversa por ocorrência. Uma tarefa do sistema que abre uma sessão nova de IA não atende a esse requisito. Um processo auxiliar só é válido se retomar a conversa identificada.
3. Consulte agendamentos existentes no ambiente e no estado privado. Prefira atualizar o correspondente em vez de duplicar. Mantenha configurações não solicitadas e respeite desativação/remoção pedidas pelo usuário.
4. Confira acesso local à configuração protegida, continuidade da conversa e exclusividade entre rodadas. Não coloque usuário, senha, carteira, token ou dados financeiros no texto do agendamento, argumentos ou nome da tarefa. O agendamento guarda referência ao diretório privado e à conversa; proteja essa referência quando o ambiente expuser a configuração publicamente.
5. Configure a recorrência com uma instrução genérica, por exemplo:

   > Retome esta mesma conversa e execute a rotina autorizada do bot de arbitragem de hashrate. Leia configuração e estado privados indicados nesta conversa. Reconcilie ações pendentes, evite execuções simultâneas e use as skills instaladas. Não altere autorizações ou limites. Se faltar acesso ou confirmação, registre a pendência. Notifique apenas mudanças relevantes, falhas ou necessidade de ação humana, salvo preferência diferente do usuário.

6. Para acompanhamento sem mutações, substitua “rotina autorizada” por “verificação somente leitura” e indique [verificar-ordens-hashrate](../verificar-ordens-hashrate/SKILL.md). Para operação, indique [operar-bot-hashrate](../operar-bot-hashrate/SKILL.md).
7. Salve o identificador do agendamento, conversa, fuso e próxima execução no estado privado. Faça uma primeira execução de validação **somente leitura** na mesma conversa; confira retorno e destino antes de habilitar ações financeiras recorrentes. A validação não amplia autorizações.
8. Informe frequência, mesma conversa, ações habilitadas e dependências do ambiente: máquina ligada, sessão, autenticação ou disponibilidade do serviço, quando aplicável. Não prometa funcionamento com a máquina desligada sem suporte confirmado.

Se o ambiente não conseguir retomar a mesma conversa, deixe o agendamento pendente e explique a capacidade ausente. Não crie novas conversas, não migre para outro serviço e não invente execução independente como alternativa. Se não conseguir comprovar o destino da primeira execução, mantenha as mutações recorrentes desabilitadas.
