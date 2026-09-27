# Validação Final

Ao terminar as refatorações e melhorias de legibilidade, executei novamente a suíte completa e a cobertura de `flaskbb.user`, o mesmo módulo trabalhado na Parte 1.

## Suíte de testes
```text
252 passed, 1 skipped in 10.51s
```

## Cobertura final
Comando:
```bash
uv run pytest -n 0 --cov=flaskbb.user --cov-branch --cov-report=term-missing
```

Resultado:
```text
TOTAL    567    390    72    8    34%
252 passed, 1 skipped in 13.42s
```

## Comparação com a Parte 1
Ao final da Parte 1:
```text
TOTAL    560    384    72    8    34%
252 passed, 1 skipped
```

Ao final da Parte 2, o total passou para 567 statements e 390 não cobertos, mantendo 72 branches, 8 branches parciais e **34% de cobertura agregada**. Portanto, o percentual apresentado pelo `coverage.py` não regrediu.

O aumento de statements é compatível com os pequenos helpers e a constante introduzidos nas refatorações. O ponto principal foi preservar o comportamento: a suíte continuou com os mesmos **252 testes aprovados e 1 ignorado**.

## Percepção após as refatorações
No começo, minha leitura estava muito concentrada em entender o que cada factory e handler fazia isoladamente. Depois das mudanças, ficou mais fácil enxergar o fluxo como etapas: coletar validadores, validar o changeset, aplicar a alteração e persistir. As refatorações não reescreveram o módulo inteiro, mas reduziram repetições e deixaram algumas intenções mais explícitas. Para mim, essa foi a parte mais útil do exercício: perceber que refatorar também pode ser feito com mudanças pequenas e controladas, desde que os testes deem segurança para confirmar que o comportamento continua o mesmo.
