# Stored Procedure `sp_migracao`

A `sp_migracao` é uma Stored Procedure utilizada para auxiliar na migração de dados novos para o banco de dados. Ela trará o primeiro ID do valor especificado na tabela, caso não exista, ele irá inserir e trazer a informação, assim auxiliando na inserção de valores novos em chaves estrangeiras.

## Parâmetros

- `valor`: valor que se deseja buscar na tabela especificada.
- `campo`: campo da tabela onde se deseja buscar o valor especificado.
- `campo_complemento`: campo complementar ao `campo` utilizado para a inserção na tabela em caso de não encontrar o valor.
- `valor_complemento`: valor complementar ao `campo_complemento` utilizado para a inserção na tabela em caso de não encontrar o valor.
- `tabela_nome`: nome da tabela onde se deseja buscar o valor especificado.

## Funcionamento

A Stored Procedure utiliza a cláusula `REGEXP_REPLACE` para substituir os caracteres `+` por espaços em branco, além de `UPPER` e `TRIM` para normalizar os valores na busca e inserção.

Em seguida, utiliza a cláusula `PREPARE` para preparar a query SQL concatenando os valores passados como parâmetros.

Executa a query com `EXECUTE` e recupera o `id` mínimo encontrado na tabela com a cláusula `SELECT`.

Caso o `id` não seja encontrado, a Stored Procedure prepara e executa uma query SQL de inserção na tabela, novamente utilizando a cláusula `PREPARE`.

Por fim, a Stored Procedure recupera o `id` mínimo encontrado na tabela após a inserção e o retorna como resultado da Stored Procedure.

## Exemplo de uso

```mysql
CALL sp_migracao('John Doe', 'nome', ', sobrenome', 'Doe', 'usuarios');
```

O exemplo acima busca pelo valor `'John Doe'` no campo `'nome'` da tabela `'usuarios'`. Caso não encontre, a Stored Procedure insere o registro com o valor complementar `', sobrenome'` igual a `'Doe'`. Retorna o `id` mínimo encontrado ou inserido na tabela `'usuarios'`.
