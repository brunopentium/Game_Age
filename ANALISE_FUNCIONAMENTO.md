# Análise de funcionamento — Guerra dos Reinos

## Problemas encontrados

1. **Construções podem ficar travadas para sempre (sem aldeão construtor).**
   - No clique de construção, o prédio é criado mesmo quando não há aldeões selecionados para construir.
   - Resultado: o edifício fica em `buildProgress = 0`, ocupando o terreno e sem concluir.
   - Trecho-chave: criação com `completed = false` e ausência de validação de `builders.length` antes de criar.

2. **A IA paga custo errado ao treinar unidade no quartel.**
   - A lógica da IA verifica/paga sempre o custo de `espadachim`, mas pode enfileirar `lanceiro` aleatoriamente.
   - Resultado: inconsistência econômica (paga um tipo e treina outro), afetando balanceamento e progressão.

3. **Recursos esgotados não liberam o tile no mapa.**
   - Quando `r.amount` chega a zero, aldeões param de coletar corretamente, mas o tile continua marcado como recurso/bloqueado.
   - Resultado: áreas ficam permanentemente inutilizáveis para construção/movimentação estratégica, mesmo após esgotamento.

4. **Trecho de lógica de “primeiro ataque da IA” é efetivamente morto.**
   - `G.ai.lastAttack` é incrementado antes da verificação `G.ai.lastAttack === 0`, então esse ramo nunca é usado na prática.
   - Resultado: comportamento real de tempo de ataque não segue a intenção declarada no código e dificulta manutenção.

## Prioridade sugerida de correção

1. Corrigir criação de construção sem construtor (bloqueio de jogabilidade).
2. Corrigir pagamento/enfileiramento da IA no quartel (integridade de regras do jogo).
3. Definir política para tile de recurso esgotado (liberar ou manter bloqueado explicitamente) e implementar de forma consistente.
4. Simplificar/corrigir cálculo de janela do primeiro ataque da IA.
