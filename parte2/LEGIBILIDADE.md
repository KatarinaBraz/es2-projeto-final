# Melhorias de Legibilidade

Além das quatro refatorações do plano, fiz três melhorias de legibilidade em categorias diferentes. Cada alteração ficou em um commit próprio e foi validada pelos testes.

## 1. Nomenclatura
**Categoria:** Nomenclatura  
**Commit:** `2fe49fa` — `refactor(user): rename model parameter to user`

Antes:
```python
def _validate_changeset(model, changeset, validators):
```

Depois:
```python
def _validate_changeset(user, changeset, validators):
```

Como o helper trabalha especificamente com o usuário atualizado, `user` comunica melhor o papel do argumento do que o nome genérico `model`.


## 2. Estilo de código
**Categoria:** Estilo de código  
**Commit:** `34441e6` — `refactor(user): improve settings update formatting`

Antes:
```python
return SettingsUpdate(language=self.language.data, theme=self.theme.data)
```

Depois:
```python
return SettingsUpdate(
    language=self.language.data,
    theme=self.theme.data,
)
```

A quebra da expressão deixa os campos visíveis separadamente e facilita a leitura, sem alterar a lógica.

## 3. Remoção de comentário desatualizado
**Categoria:** Eliminação de comentário desatualizado  
**Commit:** `d4cde02` — `refactor(user): remove outdated settings comment`

Antes:
```python
# The choices for those fields will be generated in the user view
# because we cannot access the current_app outside of the context
language = SelectField(_("Language"))
```

Depois:
```python
language = SelectField(_("Language"))
```

O comentário apontava para a *user view*, mas a configuração ocorre na factory. Removê-lo evita direcionar quem lê o código ao lugar errado.
