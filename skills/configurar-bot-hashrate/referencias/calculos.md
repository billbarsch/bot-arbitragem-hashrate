# Cálculos de preço, margem e cobertura

Use a mesma unidade nas duas plataformas: **USD por PH/s por dia**. `1 PH/s = 1.000 TH/s`; `1 EH/s = 1.000 PH/s`. Preço por EH/s/dia dividido por 1.000 vira preço por PH/s/dia. Preço por TH/s/dia multiplicado por 1.000 vira preço por PH/s/dia.

Leia a cotação e as regras **da conta e do tipo de conexão atuais**. As páginas oficiais descrevem [cobrança pela entrega na HashSell](https://www.hashsell.com/) e [créditos horários e taxa variável na RentalHash](https://rentalhash.com/). As telas atuais prevalecem para valores, mínimos e confirmações. Não copie taxas ou bônus históricos como constantes.

## Variáveis

- `lance`: preço escolhido, em USD por PH/s por dia.
- `taxa_consumo`: proporção da taxa adicional sobre o consumo; converta o percentual lido para decimal.
- `pagamento_efetivo`: receita unitária aplicável; se o bônus não estiver incluído, `pagamento_base × (1 + bonus_aplicavel)`.
- `velocidade`: PH/s efetivamente entregues ou, para projeção conservadora de gasto máximo, limite de PH/s.
- `margem_minima`: margem autorizada sobre o custo, em proporção decimal.

Quando a taxa é adicional sobre o consumo, `custo_unitario = lance × (1 + taxa_consumo)`. Se a tela mostrar outra forma de cobrança ou preço já com taxa, adapte a conta ao resumo e não duplique a taxa.

## Margem e teto

`margem_estimada_percentual = (pagamento_efetivo / custo_unitario − 1) × 100`.

Para margem mínima sobre custo: `teto_economico_lance = pagamento_efetivo / ((1 + taxa_consumo) × (1 + margem_minima))`.

**Limiar de perda**: `lance_de_perda = pagamento_efetivo / (1 + taxa_consumo)`. No `lance_de_perda` a margem é zero — o que a HashSell cobra iguala o que a RentalHash paga. Acima dele cada PH/s entregue é prejuízo; abaixo, lucro. Entre o limiar de perda e o teto econômico a ordem ainda paga, só rende menos que o alvo do usuário. Piso de entrega, `lance_de_perda` e `teto_economico_lance` são três números diferentes e a direção do ajuste precisa citar os três: subir acima do teto econômico é violar a margem mínima, subir acima de `lance_de_perda` é operar no vermelho.

O teto operacional é o menor entre esse teto econômico e o teto de lance autorizado. Taxas fixas de criação e saque reduzem o lucro do ciclo: deduza-as da receita projetada antes de afirmar que a margem líquida atende à política. Sem horizonte/volume conhecido, relate a margem unitária **antes das taxas fixas**, sem chamar de líquida.

Preço alvo de entrega: `referencia_entrega + incremento_autorizado`, arredondado para a precisão aceita. Use o mínimo para o tamanho solicitado quando disponível; caso contrário, a referência atual de corte. Sem corte, avalie a indicação de capacidade e a prévia da ordem; hashprice sozinho não prova o piso de entrega. Se o preço necessário superar o teto, não aumente o lance.

Na **criação** de uma ordem nova, a referência é a que a plataforma está obrigando naquele minuto: `max(hashprice, preco_de_corte)` — onde corte existe — somada do incremento do usuário. Com capacidade sobrando não há corte, então o piso é o hashprice. Como a margem cresce conforme o lance cai e a entrega se ajusta depois, o alvo de criação é o piso mais o incremento, nunca um valor escolhido "por segurança" acima dele.

## Crédito e duração

Para entrega constante e taxa adicional sobre consumo:

`custo_horario_maximo = limite_velocidade × custo_unitario / 24`.

`duracao_estimada_horas = saldo_restante_utilizavel / custo_horario_maximo`.

`credito_necessario = max(0, cobertura_alvo_horas × custo_horario_maximo − saldo_restante_utilizavel)`.

Confira se o saldo mostrado inclui a taxa ou se ela é debitada separadamente, e reconcilie com a prévia da plataforma. Não some novamente a taxa. Arredonde o crédito na precisão monetária aceita, sem exceder o orçamento, a reserva ou o limite por operação. A duração é uma estimativa: disputa, variação de entrega e prazo máximo podem alterar o tempo real. Adicionar saldo não necessariamente renova o prazo máximo contado desde a criação.

## Resultado realizado

`lucro_realizado = creditos_confirmados − consumo_efetivo − taxas_consumo − taxas_criacao − taxas_saque_atribuidas`.

Compare o mesmo período fechado e confira o fuso horário: uma plataforma pode nomear a hora pelo início e outra pelo fim. Aporte/reserva não é despesa; devolução de saldo não é receita. Crédito, saldo disponível e saque são etapas do mesmo dinheiro, não três receitas. Em ordens contínuas, apure por período fechado em vez de esperar o encerramento.

Taxas externas e câmbio precisam de evidência própria. Valores a creditar e saldos futuros são projeções. Não estime receita de uma segunda conta como confirmada só por a primeira ter margem positiva. A estimativa de spread não comprova rendimento garantido nem segurança das plataformas.
