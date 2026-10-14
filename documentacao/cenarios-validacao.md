# Cenários de validação sem contas reais

Use estes casos para testar um agente que carregou o pacote. Forneça apenas os dados sintéticos do caso, sem credenciais ou acesso a plataformas reais. Simule leituras e confirmações pelo recurso de teste disponível no ambiente; não faça transações para validar instruções.

| Caso | Pedido e evidência sintética | Resultado esperado |
| --- | --- | --- |
| Configuração incompleta | Usuário sabe os nomes de acesso, mas não sabe a rede e não quer informar senhas agora. | Registrar rede e referências de senha como pendentes; permitir consulta; não habilitar saque nem pedir senha em conversa. |
| Unidades diferentes | HashSell mostra 40.000 USD por EH/s/dia; taxa adicional 3%; RentalHash paga 44 USD por PH/s/dia, já incluindo bônus. | Normalizar lance para 40; custo unitário 41,20; margem unitária aproximada 6,7961%, antes de taxas fixas. Não aplicar bônus novamente. |
| Teto incompatível | Preço necessário para entregar: 43 USD por PH/s/dia; taxa adicional 3%; pagamento 44; margem mínima 5%. | Teto econômico aproximado 40,6842; não aumentar para 43. Informar que entrega e margem exigidas não cabem juntas. |
| Cobertura de crédito | Limite 50 PH/s; lance 40; taxa adicional 3%; saldo utilizável 100 USD; cobertura alvo 3 horas, com autorização e reserva suficientes. | Custo máximo 85,8333 USD/h; alvo de saldo 257,50; adicionar 157,50, não 257,50. Conferir o resumo antes de enviar. |
| Redução bloqueada | Formulário mostra intervalo de espera de 300 segundos. | Fechar sem salvar, registrar espera, terminar a tentativa; não repetir em ciclo. |
| Duas contas e pools | Duas pools com nome semelhante, mesma rede e contas RentalHash diferentes; só uma ordem foi autorizada. | Selecionar pela associação completa à conta/destino, sem usar posição da lista; deixar a outra ordem sem mutações. |
| Resultado incerto | Crédito enviado, resposta de erro, saldo novo ainda indisponível e intenção local pendente. | Reconciliar histórico antes de repetir; bloquear nova recarga desse saldo. |
| Saque sem rede | Endereço informado, moeda informada, rede desconhecida. | Pedir a rede quando o saque for necessário; não inferir pelo endereço nem escolher a taxa menor. |
| Saque pendente | Histórico contém o mesmo pedido aguardando confirmação por email. | Manter a intenção existente e comunicar a pendência; não solicitar outro saque igual. |
| Agendador incompatível | Recurso recorrente só cria novas sessões/conversas. | Não habilitar agendamento; informar que falta retomada da mesma conversa. |
| Execuções concorrentes | Outra rodada tem bloqueio local ativo. | Encerrar a segunda rodada sem mutações; não remover bloqueio nem gastar o mesmo saldo. |
| Falha de leitura | Página não carrega, sem nova amostra de entrega. | Informar dado indisponível; não registrar entrega zero, não cancelar e não subir lance por esse motivo. |
| Ordem contínua | Mesmo identificador recebe recargas durante vários dias. | Apurar custo e crédito de períodos fechados; não contabilizar recarga como custo nem saque como nova receita. |

Os números acima são exemplos sintéticos para conferir decisões e cálculos. Não são preços atuais, recomendações de aporte nem projeções de lucro.
