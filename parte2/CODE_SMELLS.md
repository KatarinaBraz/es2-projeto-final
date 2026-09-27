# Catálogo de Code Smells

## Arquivo analisado
Concentrei a análise em `flaskbb/user/services/factories.py`, usando também arquivos vizinhos de `flaskbb/user/` quando o smell fazia parte do mesmo fluxo de atualização do usuário. Escolhi esse trecho porque reúne criação de formulários, coleta de validadores e construção dos handlers. Na leitura inicial, percebi que o código funcionava, mas havia repetições e responsabilidades misturadas que tornavam o fluxo menos direto.

## 1. Duplicated Code — coleta de validadores
**Localização original:** `flaskbb/user/services/factories.py:34-60`

```python
validators = list(
    chain.from_iterable(
        pluggy.hook.flaskbb_gather_details_update_validators(app=current_app)
    )
)
```
A mesma estrutura aparecia nas factories de detalhes, senha e e-mail, mudando apenas o hook. É duplicação porque a transformação dos grupos em uma lista era repetida três vezes.

## 2. Duplicated Code — execução das validações
**Localização original:** `flaskbb/user/services/update.py:29-30, 48-49 e 65-66`

```python
accumulate_errors(lambda v: v.validate(model, changeset), self.validators)
```
Os handlers de detalhes, senha e e-mail repetiam a mesma etapa antes da atualização. A regra ficava espalhada mesmo representando o mesmo comportamento.

## 3. Primitive Obsession / valor textual repetido
**Localização original:** `flaskbb/user/forms.py`, `ChangeEmailForm`

```python
Email(message=_("Invalid email address."))
```
A mesma mensagem aparecia em diferentes campos de e-mail. Mantê-la diretamente em vários pontos aumenta a chance de inconsistência em uma alteração futura.

## 4. Método com mais de uma responsabilidade
**Localização original:** `flaskbb/user/services/factories.py:67-77`, `settings_form_factory`

```python
form = GeneralSettingsForm()
form.theme.choices = get_available_themes()
form.theme.choices.insert(0, ("", "Default"))
form.language.choices = get_available_languages()
```
Além de criar o formulário, a factory configurava diretamente as opções de tema e idioma e depois preenchia os dados atuais. Mesmo não sendo um método muito longo, havia mais de uma responsabilidade no mesmo fluxo.

## 5. Nomenclatura genérica
**Localização:** `flaskbb/user/services/update.py`, helper de validação

```python
def _validate_changeset(model, changeset, validators):
```
Nesse contexto o objeto é especificamente um usuário. O nome `model` é genérico; `user` comunica melhor o papel do parâmetro e aproxima o código da linguagem do domínio.

## 6. Comentário desatualizado
**Localização original:** `flaskbb/user/forms.py`, `GeneralSettingsForm`

```python
# The choices for those fields will be generated in the user view
# because we cannot access the current_app outside of the context
```
O comentário dizia que as opções seriam geradas na *user view*, mas o fluxo analisado as configura pela factory. Isso poderia levar quem lê o código a procurar a lógica no lugar errado.
