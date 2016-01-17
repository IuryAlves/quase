# Quase é um repositório que ensina como usar o módulo difflib do python


difflib é um módulo python usado para comparar sequências.
O uso mais comum desse módulo é para comparar arquivos e
mostrar a diferença entre eles, mas é possível fazer
muito mais do que isso.

**Tabela de conteúdos**

1. [O quão parecidas duas palavras são](#almost_equal_words)
2. [Encontrando palavras parecidas](#finding_close_words)
3. [Como transformar uma palavra em outra](#how_to_transform_a_word_into_another)


#### 1. O quão parecidas duas palavras são <a name="almost_equal_words"></a>

Para sabermos o quão parecidas duas palavras são, podemos usar a classe `SequenceMatcher`
```python
>>> from difflib import SequenceMatcher
>>> palavra1, palavra2 = "perfeito", "prefeito"
>>> sequence_matcher = SequenceMatcher(a=palavra1, b=palavra2)
>>> sequence_matcher.ratio()
0.875
```
    
O método `ratio` retorna o quão parecido duas palavras são. Esse número vai de 0.0 até 1.0

#### 2. Encontrando palavras parecidas <a name="finding_close_words"></a>

Se quisermos encontrar em uma lista de palavras, quais delas são as mais próximas de uma palavra específica usamos:

```python
>>> import difflib
>>> palavra = "lista"
>>> possibilidades = ("lst", "list", "ls", "lit")
>>> difflib.get_close_matches(palavra, possibilidades)
["lista", "lst", "lit"]
```

A lista de items retornados por `get_close_matches` é ordenada da possibilidade mais parecida até a menos parecida.

Por padrão `get_close_matches` retorna uma lista com 3 palavras mais próximas, podemos definir a quantidade de palavras que queremos passando o argumento `n`
```python
>>> import difflib
>>> difflib.get_close_matches(palavra, possibilidades, n=1)
["lista"]
```

Podemos também definir o quão igual uma possibilidade pode ser da palavra, isso é feito usando o argumento `cutoff` que tem como valor default `0.6`.
No exemplo anterior, criamos uma tupla com quatro palavras, sendo que somente três delas foram retornadas por `get_close_matches`. Vamos descobrir por que `ls` não foi retornado.

```python
>>> import difflib
>>> sequence_matcher = difflib.SequenceMatcher(a="lista", b="ls")
>>> sequence_matcher.ratio()
0.5714285714285714
```

O ratio entre `"lista"` e `"ls"` é menor que 0.6. Se quisermos que `get_close_matches` retorne `"ls"` podemos fazer:

```
>>> import difflib
>>> palavra = "lista"
>>> possibilidades = ("lst", "list", "ls", "lit")
>>> difflib.get_close_matches(palavra, possibilidades, n=4, cutoff=0.5)
['list', 'lst', 'lit', 'ls']
```

Observe que temos que indicar também com o argumento `n` que queremos 4 palavras, caso contrário, mesmo passando o `cutoff` a função `get_close_matches` retornaria apenas as 3 possibilidades mais próximas da palavra.

#### 3. Como transformar uma palavra em outra <a name="how_to_transform_a_word_into_another"></a>

Para isso usamos o método `get_opcodes` da classe `SequenceMatcher`. Esse método indica como transformar a palavra `a` na palavra `b`:

```python
>>> palavra1, palavra2 = "acendeu", "ascendeu"
>>> sequence_matcher = SequenceMatcher(a=palavra1, b=palavra2)
>>> sequence_matcher.get_opcodes()
[('equal', 0, 1, 0, 1), ('insert', 1, 1, 1, 2), ('equal', 1, 7, 2, 8)]
```
O método `get_opcodes` retorna uma lista de tuplas, sendo que cada tupla indica uma operação a ser feita. Cada tupla contém cinco elementos: 

1. Operação a ser feita:

    * equal
    * insert
    * delete
    * replace

2. posição inicial da palavra `a`
3. posição final da palavra `a`
4. posição inicial da palavra `b`
5. posição final da palavra `b`

Vamos criar uma função com instruções mais claras sobre como tranformar uma palavra em outra usando o método `get_opcodes`

```python
def transformar_uma_palavra_em_outra(palavra_a, palavra_b):
    sequence_matcher = SequenceMatcher(a=palavra_a, b=palavra_b)
    print ("Palavra a: %s\nPalavra b: %s\n" % (palavra_a, palavra_b))
    for tag, i_a, j_a, i_b, j_b in sequence_matcher.get_opcodes():
        if tag == "replace":
            print ("Troque (%s) em palavra_a[%s:%s] por (%s)" % (palavra_a[i_a:j_a], i_b, j_b, palavra_b[i_b: j_b]))
        elif tag == 'delete':
            print ("Remova (%s) em palavra_a[%s:%s]" % (palavra_a[i_a: j_a], i_a, j_a))
        elif tag == 'insert':
            print ("Insira (%s) em palavra_a[%s:%s]" % (palavra_b[i_b: j_b], i_a, j_a))

```

Assim temos:

```python
>>> como_transformar_uma_palavra_em_outra("consenso", "censo")
Palavra a: consenso
Palavra b: senso

Remova (con) em palavra_a[0:3]
```
