# Plano de Testes --- Módulo `flaskbb/user`

## Módulo escolhido

Escolhi trabalhar com o módulo `flaskbb/user`. Entre os três módulos
sugeridos, ele me pareceu um bom ponto para começar porque reúne regras
que são fáceis de entender do ponto de vista do usuário, como troca de
e-mail, alteração de senha, avatar e atualização de dados. Ao mesmo
tempo, existem validações importantes que não deveriam depender de uma
execução completa da aplicação para serem verificadas.

Na baseline, o módulo tinha aproximadamente 31% de cobertura de
linhas/statements (172 de 560 statements executados). Alguns arquivos
chamaram atenção, principalmente `services/factories.py`, que estava com
0%, e `services/validators.py`, com 48%. Por isso decidi concentrar os
novos testes nessas duas áreas.

## Meta

Minha meta foi aumentar a confiança sobre as validações do módulo `user`
e começar a retirar `services/factories.py` do estado de cobertura zero.
Como referência quantitativa, defini como objetivo chegar a pelo menos
32% de cobertura de linhas/statements no módulo, além de exercitar
explicitamente caminhos válidos, erros esperados e interações com
dependências usando mocks.

## Cenários planejados

1.  **Caminho feliz:** aceitar o e-mail antigo quando ele corresponde ao
    e-mail atual.
2.  **Erro/borda:** rejeitar o e-mail antigo quando ele não corresponde
    ao e-mail atual.
3.  **Caminho feliz:** aceitar um novo e-mail diferente do atual.
4.  **Erro:** rejeitar a tentativa de manter o mesmo e-mail.
5.  **Caminho feliz:** aceitar uma nova senha quando ela é diferente da
    senha atual.
6.  **Erro:** rejeitar uma nova senha igual à senha atual.
7.  **Caminho feliz:** aceitar a senha antiga correta.
8.  **Erro:** rejeitar a senha antiga incorreta.
9.  **Borda:** ignorar a validação de avatar quando a URL estiver vazia.
10. **Caminho feliz:** validar uma URL de avatar válida chamando
    `check_image`.
11. **Erro:** tratar uma imagem inválida.
12. **Erro:** tratar falha de requisição durante a validação do avatar.
13. **Caminho feliz:** aceitar um e-mail que ainda não esteja
    cadastrado.
14. **Erro:** rejeitar um e-mail já registrado.
15. **Caminho feliz:** verificar se as factories de formulários utilizam
    o usuário atual corretamente.

Além dos casos individuais, planejei usar parametrização para testar
combinações válidas e inválidas da conferência do e-mail antigo e mocks
para isolar chamadas como `check_image`, consulta de usuários e criação
de formulários.
