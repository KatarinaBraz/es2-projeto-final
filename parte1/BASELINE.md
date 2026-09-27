# Baseline --- Parte 1

## Ambiente

Para a realização da Parte 1, utilizei o fork didático do FlaskBB
disponibilizado para a disciplina.

-   Fork: https://github.com/KatarinaBraz/flaskbb
-   Commit de referência inicial: `22989b7`
-   Ambiente local: macOS
-   Gerenciador utilizado: `uv`
-   Python utilizado pelo projeto: 3.12.14

Durante a configuração do ambiente, foi necessário ajustar alguns pontos
locais para conseguir executar a suíte corretamente, principalmente a
criação do diretório `instance`, a execução sem paralelismo (`-n 0`) e a
compilação das traduções. Depois desses ajustes, a baseline ficou verde.

## Baseline de testes

Resultado obtido antes da inclusão dos meus novos testes:

``` text
232 passed, 1 skipped
```

Isso confirmou que eu estava partindo de uma versão funcional do projeto
antes de fazer qualquer alteração.

## Cobertura inicial

Também gerei o relatório de cobertura dos três módulos indicados no
enunciado. No módulo `user`, que depois escolhi como alvo, os valores
iniciais por arquivo foram:

  Arquivo                           Statements   Miss   Cobertura
  ------------------------------- ------------ ------ -----------
  `user/__init__.py`                         4      4          0%
  `user/forms.py`                           45     36         20%
  `user/models.py`                         246    193         22%
  `user/plugins.py`                         26     23         12%
  `user/services/__init__.py`                0      0        100%
  `user/services/factories.py`              33     33          0%
  `user/services/update.py`                 42     27         36%
  `user/services/validators.py`             40     21         48%
  `user/views.py`                          124     51         59%

Somando os arquivos do módulo `user`, a baseline tinha 560 statements e
388 não executados, ou seja, aproximadamente 31% das linhas/statements
estavam cobertas.

Para `forum` e `management`, o relatório inicial também mostrou vários
pontos com cobertura baixa, principalmente em formulários e views. Como
a atividade pedia a escolha de apenas um módulo, usei esses dados
somente como referência para decidir onde concentrar o trabalho.

> Observação: a primeira coleta da baseline foi executada sem a opção
> `--cov-branch`, portanto não registrei um percentual inicial de
> branches. Preferi manter o dado como realmente foi coletado em vez de
> estimar um valor que não foi medido.
