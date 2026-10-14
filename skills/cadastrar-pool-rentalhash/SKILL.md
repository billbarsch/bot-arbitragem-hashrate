---
name: cadastrar-pool-rentalhash
description: Obtém a conexão de processamento da conta RentalHash e cadastra ou verifica esse destino na HashSell. Use para preparar o envio de poder computacional entre as plataformas, sem iniciar uma ordem.
---

# Cadastrar destino RentalHash na HashSell

Leia [as regras comuns](../configurar-bot-hashrate/referencias/regras-operacionais.md) e a configuração privada. Cadastro de pool e criação de ordem são ações separadas.

1. Navegue para `https://rentalhash.com/painel` e confirme a conta de recebimento. Localize na interface atual o cadastro ou instruções para receber poder de uma plataforma/marketplace. Não confunda conexão de equipamentos próprios com a conexão adequada ao provedor contratado.
2. Se for preciso criar um nome de conexão ou trabalhador, obtenha o rótulo do usuário e faça somente o cadastro autorizado. Leia host, porta, protocolo, algoritmo, nome de conexão/trabalhador e senha de processamento indicados pela conta. Não invente um trabalhador nem use a senha de login como senha de pool.
3. Registre o destino apenas na configuração privada, associando a conta RentalHash ao identificador correto. Guarde a senha de processamento como segredo protegido. Não reutilize o host de um exemplo histórico sem conferir as instruções atuais.
4. Navegue para `https://www.hashsell.com/pools`. Confira a conta compradora e os destinos existentes antes de cadastrar. Se já houver correspondência exata de conta, host, porta, conexão e algoritmo, verifique-a em vez de duplicar.
5. Preencha o destino com os dados conferidos, releia os campos e salve dentro da autorização. Identifique por dados completos, não por ordem da lista ou nome parecido. Nomes iguais podem apontar para contas RentalHash diferentes.
6. Solicite a verificação de conexão disponível na HashSell e registre o resultado. Se falhar, investigue a mensagem e confira credenciais de processamento; não crie uma ordem para “testar” um destino recusado.
7. Reabra a lista e confirme que o destino persistiu e está validado. Sem ordem entregando, é normal a RentalHash ainda não mostrar velocidade ou crédito: verificação de conexão não comprova mineração nem lucro.

Se faltar login, autenticação ou senha, deixe na tela para o usuário. Relate: conta de destino identificada, pool cadastrado/existente, conexão validada ou pendência e próxima ação necessária. Nenhum saldo é gasto por esta skill.
