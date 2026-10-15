---
name: redimensionar-ordem-hashsell
description: Ajusta a velocidade máxima de uma ordem HashSell usando margem atual, orçamento, disponibilidade e faixas autorizadas. Use para subir ou reduzir PH/s; não define uma estratégia financeira por conta própria.
---

# Redimensionar velocidade

Leia [as regras comuns](../configurar-bot-hashrate/referencias/regras-operacionais.md), [os cálculos](../configurar-bot-hashrate/referencias/calculos.md), [as regras de plataforma observadas](../configurar-bot-hashrate/referencias/regras-da-plataforma.md) e a política de dimensionamento privada. Sem faixas autorizadas, apenas simule alternativas; não invente uma tabela de velocidade.

1. Navegue para a ordem selecionada em `https://www.hashsell.com/orders`. Confira conta, destino, velocidade atual, lance, entrega, saldo e prazo máximo.
2. Calcule a margem atual com o pagamento e bônus aplicáveis. Use as faixas locais que ligam margem a velocidade. Respeite confirmação por verificações consecutivas, zona sem ajuste, rampa por execução e teto de PH/s definidos pelo usuário.
3. Estime o consumo diário na velocidade proposta, somando a exposição das outras ordens autorizadas. Confira orçamento, reserva, cobertura de saldo e limites diários. Aumentar velocidade acelera gasto; não aumenta o percentual de margem por si só.
4. Confira a prévia de entrega e preço necessário no novo tamanho. Capacidade para a velocidade antiga não comprova capacidade para a nova. Se for preciso mudar também o lance, obtenha autorização para ambos e confira o teto econômico novamente.
5. No ajuste de lance e limite, preencha a velocidade alvo e preserve o lance quando ele não fizer parte da ação autorizada. Releia os dois valores, unidades, custo, destino e resumo antes de confirmar.
6. Registre a intenção; execute dentro dos limites e verifique os valores persistidos, entrega e cobertura. Se houver passo máximo, espera ou restrição de redução, registre e encerre a tentativa, sem contornar a regra.

Sem margem suficiente, não aumente velocidade. Cancelamento ou pausa exige sua própria autorização; uma falha de carregamento não autoriza interromper a ordem. Registre também quando o alvo depender de condição ainda não confirmada.
