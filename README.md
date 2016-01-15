# Quase é um repositório que ensina como usar o módulo difflib do python


difflib é um módulo python usado para comparar sequências.
O uso mais comum desse módulo é para comparar arquivos e
mostrar a diferença entre eles, mas é possível fazer
muito mais do que isso.

**Tabela de conteúdos**

1. [O quão parecidas duas palavras são](#almost_equal_words)
2. [Encontrando palavras parecidas](#finding_close_words)


#### 1. O quão parecidas duas palavras são <a name="almost_equal_words"></a>

Para sabermos o quão parecidas duas palavras são, podemos usar a classe `SequenceMatcher`
```python
>>> from difflib import SequenceMatcher
>>> palavra1, palavra2 = "perfeito", "prefeito"
>>> sequence_matcher = SequenceMatcher(a=palavra1, b=palavra2)
>>> sequence_matcher.ratio()
>>> 0.875
```
    
O método `ratio` retorna o quão parecido duas palavras são. Esse número vai de 0.0 até 1.0

#### 2. Encontrando palavras parecidas <a name="finding_close_words"></a>

Se quisermos encontrar em uma lista de palavras, quais delas são as mais próximas de uma palavra específica usamos:

```python
>>> import difflib
>>> palavra = "lista"
>>> possibilidades = ("lst", "list", "ls", "lit")
>>> difflib.get_close_matches(palavra, possibilidades)
>>> ["lista", "lst", "lit"]
```

A lista de items retornados por `get_close_matches` é ordenada da possibilidade mais parecida até a menos parecida.

Por padrão `get_close_matches` retorna uma lista com 3 palavras mais próximas, podemos definir a quantidade de palavras que queremos passando o argumento `n`
```python
>>> import difflib
>>> difflib.get_close_matches(palavra, possibilidades, n=1)
>>> ["lista"]
```

