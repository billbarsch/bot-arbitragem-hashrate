---
name: configurar-bot-hashrate
description: Configura acessos privados, carteiras, destinos, limites e autorizações para operar HashSell e RentalHash. Use na primeira utilização, quando faltar configuração ou quando o usuário pedir mudança de conta, carteira ou política do bot.
---

# Configurar o bot de hashrate

Prepare o uso das skills sem iniciar ordens, modificar preços ou movimentar saldo. A instalação não equivale a autorização financeira.

## Configuração inicial

1. Descubra se existe configuração privada indicada pelo usuário ou pelo ambiente. Leia [referencias/configuracao-privada.md](referencias/configuracao-privada.md) antes de criar ou alterar o arquivo. Não procure credenciais em outras pastas, contas ou repositórios.
2. Pergunte qual é o usuário da HashSell e da RentalHash e como deseja fornecer a senha de **cada conta**, separadamente. Ofereça entrada segura local ou login manual. Não peça senha, código de autenticação, chave privada ou frase de recuperação na conversa. Se o ambiente não permitir entrada segura e armazenamento protegido, registre login manual; não crie um arquivo de senhas em texto puro.
3. Pergunte a carteira de saque, moeda, rede, plataforma de destino e eventual identificador adicional exigido pelo destinatário. O usuário pode deixar qualquer informação pendente. Só valide a carteira quando moeda, rede e titular/destinatário estiverem claros.
4. Pergunte quais ordens e contas podem ser gerenciadas, ações permitidas, orçamento por ordem, reserva de saldo, teto de lance, margem mínima, velocidade, cobertura de crédito (gatilho em horas e quanto repor), política de perda — se e quando cancelar ordem que roda no prejuízo — e política de saque. Não herde limites de outro usuário nem atribua um valor financeiro padrão.
5. Pergunte fuso horário, frequência, preferência de notificações e a conversa que será retomada, caso queira recorrência. Não agende durante esta configuração; registre as preferências para a skill de agendamento.
6. Crie a configuração privada a partir do [modelo vazio](referencias/configuracao.exemplo.json), sem alterar o modelo público. Crie também o estado e o registro locais conforme [referencias/registro-local.md](referencias/registro-local.md). Registre campos pendentes como `null` ou listas vazias, nunca como zero.
7. Confira que o diretório está fora do repositório, que os arquivos têm acesso restrito e que nenhum segredo entrou em saída, captura ou histórico versionado. Se não conseguir verificar essa proteção, deixe armazenamento de segredos pendente.
8. Mostre um resumo sem senhas e com endereços mascarados: contas identificadas, ações autorizadas, limites, pendências e localização privada. Comece em somente leitura até existir autorização suficiente para uma ação concreta.

## Uso pelas demais skills

Leia [referencias/regras-operacionais.md](referencias/regras-operacionais.md) em toda operação. Leia [referencias/calculos.md](referencias/calculos.md) para estimar margem, custo ou cobertura. Peça somente os campos ausentes que sejam necessários à ação atual; não bloqueie uma consulta pública por falta de carteira de saque.

Login manual, sessão já autenticada e segredo protegido são alternativas válidas. Não suponha que as duas plataformas usam o mesmo usuário ou senha. Nunca solicite frase de recuperação de carteira.
