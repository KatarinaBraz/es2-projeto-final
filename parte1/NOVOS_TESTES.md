# Novos Testes --- Parte 1

Os testes novos foram adicionados ao diretório `tests/unit/user/` do meu
fork do FlaskBB.

Commits principais:

-   `4b5edbc` --- `test: add user service validator tests`
-   `b84cfd5` --- `test: add user service factory tests`

Depois das alterações, a suíte completa terminou com:

``` text
252 passed, 1 skipped
```

## Testes de validators

Arquivo: `tests/unit/user/test_additional_validators.py`

  ----------------------------------------------------------------------------------------------------------------
                         Linha Caso                                                          Tipo
  ---------------------------- ------------------------------------------------------------- ---------------------
                            35 `test_old_email_must_match_parametrized`                      Feliz + erro/borda

                            49 `test_emails_must_be_different_accepts_new_email`             Feliz

                            56 `test_emails_must_be_different_rejects_same_email`            Erro

                            64 `test_passwords_must_be_different_accepts_new_password`       Feliz

                            74 `test_passwords_must_be_different_rejects_current_password`   Erro

                            83 `test_old_password_must_match_accepts_correct_password`       Feliz

                            93 `test_old_password_must_match_rejects_wrong_password`         Erro

                           103 `test_avatar_validator_ignores_empty_avatar`                  Borda

                           114 `test_avatar_validator_calls_check_image_for_valid_url`       Feliz

                           127 `test_avatar_validator_rejects_invalid_image`                 Erro

                           138 `test_avatar_validator_handles_request_error`                 Erro

                           149 `test_cant_share_email_accepts_unique_email`                  Feliz

                           161 `test_cant_share_email_rejects_registered_email`              Erro
  ----------------------------------------------------------------------------------------------------------------

### Teste parametrizado

O teste `test_old_email_must_match_parametrized` utiliza
`pytest.mark.parametrize` com quatro combinações. Duas representam
e-mails válidos e duas representam situações inválidas, incluindo
diferença simples entre os valores e diferença de caixa
(`USER@example.com` x `user@example.com`).

Como o teste é parametrizado, essas quatro combinações são executadas
separadamente pelo pytest.

### Uso de dublês/mocks

Usei mocks principalmente para evitar que testes unitários simples
dependessem de serviços ou estados externos. Um exemplo é
`test_avatar_validator_calls_check_image_for_valid_url`, no qual
`check_image` é substituído por um mock. Além de verificar o resultado
da validação, o teste confirma a interação com
`assert_called_once_with(avatar_url)`, garantindo que a dependência foi
chamada uma única vez com a URL esperada.

Também usei mocks para `check_password` e para a consulta usada em
`CantShareEmailValidator`, mantendo os testes isolados.

## Testes de factories

Arquivo: `tests/unit/user/test_additional_factories.py`

  ----------------------------------------------------------------------------------------------------------
                         Linha Caso                                                    Tipo
  ---------------------------- ------------------------------------------------------- ---------------------
                             5 `test_settings_update_handler_uses_db_and_pluggy`       Feliz

                            12 `test_change_password_form_factory_uses_current_user`   Feliz/interação

                            22 `test_change_email_form_factory_uses_current_user`      Feliz/interação

                            32 `test_change_details_form_factory_uses_current_user`    Feliz/interação
  ----------------------------------------------------------------------------------------------------------

Nos três testes de criação de formulários, usei `patch` para substituir
o formulário real. O objetivo foi testar somente a responsabilidade da
factory e verificar se ela instancia o formulário correto com
`current_user`. As chamadas são conferidas com
`assert_called_once_with`, portanto o teste verifica explicitamente a
interação com o dublê.

## Resultado

O primeiro conjunto acrescentou 16 execuções de teste (o teste
parametrizado gera quatro execuções). O segundo conjunto acrescentou
mais 4. A suíte passou de 232 testes aprovados na baseline para 252
testes aprovados, mantendo apenas o mesmo teste previamente ignorado
(`1 skipped`) e sem desabilitar ou modificar testes existentes.
