# Plano de Refactoring

Depois de catalogar os smells, escolhi quatro mudanças pequenas. Minha prioridade foi reduzir repetição e separar responsabilidades sem alterar o comportamento já protegido pela suíte de testes.

| Smell | Refatoração | Resultado esperado | Risco antecipado |
|---|---|---|---|
| Duplicação na coleta de validadores | **Extract Method** | Centralizar a conversão dos grupos de validadores para lista. | Alterar a forma ou a ordem da coleta. |
| Duplicação da execução das validações | **Extract Method** | Tornar a etapa de validação explícita e evitar repetição nos handlers. | Interferir na passagem do usuário/changeset ou na propagação de erros. |
| Mensagem de e-mail repetida | **Replace Magic Value with Symbolic Constant** | Manter a mensagem em um único ponto. | Afetar tradução ou texto exibido se a constante fosse criada no contexto errado. |
| Configuração de choices misturada à factory | **Extract Method** | Separar a preparação de tema/idioma da criação do formulário. | Alterar a ordem de inicialização das choices. |

O plano foi propositalmente conservador. Como a Parte 1 já havia acrescentado testes ao módulo `user`, preferi mudanças curtas e verificáveis, executando a suíte completa após cada refatoração.
