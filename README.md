# Bot de arbitragem de hashrate

Skills em português para um agente de IA acompanhar e operar a compra de poder computacional na HashSell e sua entrega à RentalHash. Este repositório contém instruções, não um programa que executa sozinho. O agente escolhe os recursos de navegação, armazenamento e agendamento disponíveis no ambiente.

## Organização

| Skill | Finalidade |
| --- | --- |
| `configurar-bot-hashrate` | Perguntar acessos e preferências, criar configuração privada e registrar pendências. |
| `cadastrar-pool-rentalhash` | Obter a conexão da conta RentalHash e cadastrar/verificar o destino na HashSell. |
| `verificar-ordens-hashrate` | Conferir ordens, entrega, saldos, pagamentos, taxas e margem, sem movimentar dinheiro. |
| `aumentar-preco-hashsell` | Aumentar o lance dentro do teto de custo e da margem autorizados. |
| `diminuir-preco-hashsell` | Reduzir o lance respeitando entrega, passo permitido e intervalo de espera. |
| `redimensionar-ordem-hashsell` | Ajustar a velocidade máxima usando faixas e limites definidos pelo usuário. |
| `adicionar-credito-hashsell` | Transferir saldo já disponível para uma ordem existente. |
| `criar-ordem-hashsell` | Preparar ou criar uma ordem com destino, velocidade, lance e orçamento conferidos. |
| `solicitar-saque-rentalhash` | Preparar ou solicitar saque para uma carteira previamente validada. |
| `operar-bot-hashrate` | Coordenar as ações autorizadas, evitando concorrência e operações duplicadas. |
| `agendar-bot-hashrate` | Configurar recorrência sempre na mesma conversa, com o agendador disponível. |

As referências compartilhadas ficam dentro de `skills/configurar-bot-hashrate/referencias/`. Instale ou disponibilize **todas as pastas de skills juntas**, preservando seus nomes e essa estrutura. Se o ambiente não tiver instalação de skills, peça ao agente que leia a skill desejada e as referências indicadas. A sintaxe de invocação depende do ambiente; alguns aceitam `$nome-da-skill`.

## Começar

1. Disponibilize as skills ao agente e peça: **“Configure o bot para minha conta na HashSell e na RentalHash.”**
2. Informe usuários, conta de recebimento, moeda, rede e carteira de saque. Senhas são solicitadas por entrada segura local ou preenchidas diretamente no login; não cole senhas na conversa.
3. O agente cria `configuracao-privada.json` em um diretório privado **fora do repositório**, com acesso restrito. O [modelo público](skills/configurar-bot-hashrate/referencias/configuracao.exemplo.json) contém apenas campos vazios. Senhas ficam em armazenamento seguro; o arquivo guarda suas referências. Sem armazenamento seguro, o login permanece manual.
4. Defina as ordens que o agente pode gerenciar, orçamento, reserva, margem mínima, teto de lance, velocidade e autorizações de cada ação. O que você não souber fica pendente até ser necessário.
5. Peça uma verificação e valide o destino de processamento antes de operar. Depois autorize ações específicas ou uma política recorrente com limites.
6. Para recorrência, peça o agendamento. Todas as execuções retomam **a mesma conversa**. Se o agendador não permitir isso, a automação fica pendente; o agente não cria conversas novas como alternativa.

## Exemplos de pedidos

- “Verifique minhas ordens e diga a margem atual, sem alterar nada.”
- “Diminua o preço da ordem selecionada, dentro dos limites configurados.”
- “Complete o crédito dessa ordem até a cobertura autorizada.”
- “Prepare o saque para minha carteira cadastrada e deixe a confirmação para mim.”
- “Execute periodicamente as ações que autorizei nesta mesma conversa.”

## Privacidade e execução

Contas, senhas, chaves, cookies, carteiras, identificadores de ordens, históricos, capturas e registros financeiros ficam somente no diretório privado do usuário. O `.gitignore` é uma proteção adicional; não substitui guardar esses arquivos fora do repositório. Não há dados de uma conta real nos modelos.

Operações automáticas exigem autorização definida pelo usuário e capacidade permitida pelo ambiente. Algumas confirmações, autenticações e saques precisam de participação humana. As skills não contornam essas exigências nem prometem lucro ou prazo de recebimento.

As regras atuais devem ser conferidas nas [telas da HashSell](https://www.hashsell.com/) e da [RentalHash](https://rentalhash.com/). Taxas, mínimos, bônus e preços não são constantes neste pacote.

Para colaborar, leia [AGENTS.md](AGENTS.md). As condições de uso do conteúdo estão em [LICENCA](LICENCA).

Os [cenários de validação](documentacao/cenarios-validacao.md) permitem revisar o comportamento com dados sintéticos, sem acessar contas reais. A verificação estrutural das skills e dos modelos não substitui um teste supervisionado no ambiente em que serão usadas.
