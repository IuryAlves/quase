# Quase é um repositório que ensina como usar o módulo difflib do python


difflib é um módulo python usado para comparar sequências.
O uso mais comum desse módulo é para comparar arquivos e
mostrar a diferença entre eles, mas é possível fazer
muito mais do que isso.**

**Tabela de conteúdos**

1. [Encontrando palavras parecidas](#finding_close_words)

#### Encontrando palavras parecidas <a name="finding_close_words"></a>

Se quisermos encontrar em uma lista de palavras, quais delas são as mais próximas de uma palavra específica usamos:

    >>> import difflib
    >>> palavra = "lista"
    >>> possibilidades = ("lst", "list", "ls", "lit")
    >>> difflib.get_close_matches(palavra, possibilidades)
    >>> ["lista", "lst", "lit"]

Por padrão `get_close_matches` retorna uma lista com 3 palavras mais próximas, podemos definir a quantidade de palavras que queremos passando o argumento `n`
    
    >>> import difflib
    >>> difflib.get_close_matches(palavra, possibilidades, n=1)
    >>> ["lista"]

