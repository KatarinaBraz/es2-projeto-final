# Refatorações Aplicadas

As quatro refatorações foram feitas em commits separados. Depois de cada mudança, executei a suíte completa. O resultado permaneceu em **252 testes aprovados e 1 ignorado**.

## 1. Coleta de validadores
**Commit:** `3c16ba7`  
**Smell:** Duplicated Code  
**Transformação:** Extract Method

Antes, cada factory repetia `list(chain.from_iterable(...))`. Depois, a conversão passou para:

```python
def _collect_validators(validator_groups):
    return list(chain.from_iterable(validator_groups))
```

As três factories passaram a reutilizar o helper. Foi uma mudança simples, mas eliminou duplicação real sem esconder a regra.

## 2. Validação dos changesets
**Commit:** `89a54f6`  
**Smell:** Duplicated Code  
**Transformação:** Extract Method

Antes:
```python
accumulate_errors(lambda v: v.validate(model, changeset), self.validators)
```

Depois:
```python
def _validate_changeset(model, changeset, validators):
    accumulate_errors(
        lambda validator: validator.validate(model, changeset), validators
    )
```

A etapa comum ficou centralizada, enquanto cada handler continuou responsável por sua alteração específica.

## 3. Mensagem de e-mail reutilizada
**Commit:** `d5d6050`  
**Smell:** valor textual repetido  
**Transformação:** Replace Magic Value with Symbolic Constant

Antes:
```python
Email(message=_("Invalid email address."))
```

Depois:
```python
INVALID_EMAIL_MESSAGE = _("Invalid email address.")
```

Os campos passaram a reutilizar a constante, evitando manter o mesmo texto em vários pontos.

## 4. Configuração das opções do formulário
**Commit:** `0f694c7`  
**Smell:** responsabilidades misturadas  
**Transformação:** Extract Method

Depois da extração:
```python
def _configure_settings_choices(form):
    form.theme.choices = get_available_themes()
    form.theme.choices.insert(0, ("", "Default"))
    form.language.choices = get_available_languages()
```

A factory continuou criando e devolvendo o formulário, mas a preparação das opções ganhou uma função com intenção específica. A suíte permaneceu verde.

## Refatoração complementar — extração em método longo

**Commit:** `12063f8`  
**Smell:** Long Method / responsabilidades misturadas  
**Transformação:** Extract Method

Durante a revisão final, identifiquei que o método `save()` em `flaskbb/user/models.py` também concentrava a atualização dos grupos secundários e a persistência do usuário. A lógica de atualização dos grupos foi extraída para um método específico:

```python
def _update_secondary_groups(self, groups: list[Group]) -> None:
    ...
git status
