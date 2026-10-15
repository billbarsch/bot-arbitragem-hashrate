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
5. Grave no agendamento **uma única instrução: executar as instruções da skill de verificação**. O texto salvo é a porta de entrada, não a rotina:

   > Execute as instruções da skill de verificação.

   Acrescente a essa frase só o que o ambiente precisa para localizar os arquivos — o caminho da skill e o do diretório privado. Formato completo aceito: "Execute as instruções da skill de verificação em `caminho/da/skill`; configuração e estado privados estão em `diretório privado`." Nada além disso. A skill chamada é [verificar-ordens-hashrate](../verificar-ordens-hashrate/SKILL.md); é ela que decide, a partir das autorizações privadas, se a rodada também ajusta lance ou apenas registra e avisa.

6. **Não reescreva a rotina dentro do agendamento.** Passos, fórmulas, limites, guardas e regras de plataforma copiados no texto da tarefa criam uma segunda fonte de verdade, que diverge da skill na primeira edição e sobrevive silenciosamente a ela. Quem quiser mudar o comportamento muda a skill; o agendamento continua valendo sem retoque. Se a recorrência também deve ajustar lance, isso vem das `autorizacoes` da configuração privada, lidas pela skill — não do texto do agendamento. Não escreva "somente leitura" nem "pode ajustar tudo" no prompt: o escopo se resolve pelas autorizações, e indicar a skill de verificação já define o que é feito.
7. Confira o texto salvo depois de criar ou atualizar: ele deve continuar sendo a instrução de executar a skill de verificação. Ao migrar de um agendamento que duplica a rotina, atualize o existente em vez de criar outro e registre a substituição no estado privado.
8. Salve o identificador do agendamento, conversa, fuso e próxima execução no estado privado. Faça uma primeira execução de validação na mesma conversa e confira retorno e destino antes de deixar a recorrência rodando sozinha; se as autorizações de mutação estiverem desligadas, a primeira rodada é só leitura de qualquer forma. A validação não amplia autorizações.
9. Informe frequência, mesma conversa, ações habilitadas e dependências do ambiente: máquina ligada, sessão, autenticação ou disponibilidade do serviço, quando aplicável. Não prometa funcionamento com a máquina desligada sem suporte confirmado.

Se o ambiente não conseguir retomar a mesma conversa, deixe o agendamento pendente e explique a capacidade ausente. Não crie novas conversas, não migre para outro serviço e não invente execução independente como alternativa. Se não conseguir comprovar o destino da primeira execução, mantenha as mutações recorrentes desabilitadas.
