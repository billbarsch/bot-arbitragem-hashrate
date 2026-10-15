# Regras de plataforma observadas

Registro de comportamento observado nas telas, para orientar a leitura em cada execução. **Nada aqui é constante**: confirme o valor atual no formulário antes de confiar nele. Se a tela divergir deste arquivo, a tela vence e este arquivo precisa ser corrigido.

Sem data de observação, não use a linha como autorização de mutação.

## HashSell: ajuste de lance e limite

- O ajuste de lance e de limite é um único formulário, "Ajustar lance e limite", com campo de lance em US$ por PH/s por dia e campo de limite em PH/s. Ajustar um deles não dispensa conferir o outro antes de salvar.
- A própria tela resume a assimetria: **"Aumentar vale na hora; reduzir tem limite."** Aumento de lance tem efeito imediato; redução tem trava.
- A tela não quantifica a trava de redução. Regra relatada pelo operador em 2026-10-14: **redução de até US$ 0,10 por vez, com intervalo mínimo de cerca de 10 minutos entre reduções**. Reconferir no formulário a cada tentativa; se houver espera, feche sem salvar e registre quando poderá consultar de novo.
- O mesmo formulário mostra, em texto, o **menor lance que entrega o limite contratado** para aquele tamanho. É a referência imediata de disputa de preço; continua sendo necessário comparar com o teto econômico.
- Aumentar o limite de PH/s sem conferir a prévia de entrega para o novo tamanho é ação diferente de ajustar o lance: capacidade para o tamanho atual não comprova capacidade para o novo.

## HashSell: cobrança

- A cobrança incide sobre o **entregue**, não sobre o reservado pelo leilão. Amostras em que a pool não respondeu geram cobrança menor e também receita menor na plataforma de destino.
- A taxa de consumo aparece no extrato como lançamento separado do consumo, na mesma ordem. Em 2026-10-14 a proporção observada foi de **3,00% sobre o consumo**; confirme a cada período, ela não é fixa por contrato deste arquivo.
- A duração exibida é estimada ao preço e limite atuais. Disputa, variação de entrega e prazo máximo alteram o tempo real.

## RentalHash: pagamento

- O crédito vira lançamento **por hora fechada**, usando a potência média entregue naquela hora. Uma leitura no meio da hora não é receita fechada.
- A taxa base é exibida em escala (TH/s, PH/s, EH/s por dia). Normalize tudo para US$ por PH/s por dia antes de comparar com o lance da HashSell.
- O bônus de fidelidade é informado como percentual somado à taxa base, por nível. Em 2026-10-14 a conta observada estava em nível com **+2,5%**, e o valor por hora do extrato já refletia esse bônus. **Não aplique o bônus duas vezes:** se o crédito medido já o inclui, use o crédito medido como pagamento efetivo.
- O nível de fidelidade **pausa** quando a potência semanal sai da faixa de estabilidade e nunca cai; isso muda o bônus disponível, não o histórico creditado.
- Saque: mínimo e taxa da rede aparecem na própria tela de saque, junto do prazo estimado de conclusão. Em 2026-10-14 a tela indicava mínimo de US$ 10,00, taxa de US$ 2,00 e prazo de 24 a 48 horas. Tratou pedido como pendente, não como dinheiro recebido.

## Consequência para o teto de lance

Como o pagamento da RentalHash varia por conta, nível de fidelidade e tipo de conexão, o teto de lance é **calculado na execução**, não memorizado:

```
teto_economico_lance = pagamento_efetivo / ((1 + taxa_consumo) × (1 + margem_minima))
```

Sem leitura fresca do pagamento efetivo, o teto é desconhecido, e teto desconhecido bloqueia aumento de lance. Consulte [calculos.md](calculos.md) para as fórmulas e [regras-operacionais.md](regras-operacionais.md) para o que fazer quando o dado falta.
