<div align="center">
<a href="https://github.com/DigouO" target="_blank"><img align="center" height="120px" src="../assets/logo.png" /></a>
<h1>FASE 5 - Data Universe</h1>
<h2>Capítulo 04: MongoDB na Prática.</h2>
</div>

<div align="center">
<h2>1. MONGODB NA PRÁTICA</h2>
</div>

## 1.1 Terminologia

- comparação entre os termos usados em um banco de dados relacional Oracle e um banco de dados orientado a documentos MongoDB:

<div align="center">

Oracle (RDBMS) | MongoDB (documento)
---------------|-----------------------
Database | Database
Instânca de Banco de Dados | Instância MongoDB
Esquema | Banco de Dados
Tabela, View | Coleção
Tupla | DOcumento (JSON, BSON)
Coluna | Campo ou Field
ROWID / Primary Key | _id
Junção | DBRef (Embedded Document)
Foreign Key | Reference
Partição | Shard
Select | Método find
Insert | Método insert
Updade | Método update
Delete | Método remove

</div>

### 1.1.1 Coleção (collection)
- uma coleção é onde os dados do MongoDB são armazenados. 
- pode ser comparada com as tabelas de um banco de dados relacional, mas sua estrutura é muito menos rígida do que em um RDBMS. 
- é um conjunto de documentos MongoDB.
- uma coleção pertence a um único banco de dados MongoDB e não possui um esquema. 
- cada documento em uma coleção pode ter campos diferentes. 
- por uma questão de praticidade e organização, procuramos criar coleções com documentos semelhantes.
- coleções podem ser criadas no momento em que formos salvar um documento que a referencie, ou por meio do `comando createCollection`.

~~~
db.createCollection(name, options)
~~~

- onde:
  - ***Name***: indica o nome da coleção a ser criada.
  - ***Options***: configurações opcionais para a criação de:
    - Capped collection.
    - Cluestered collection.
    - View.

- o comando a seguir cria uma coleção denominada biblioteca:

~~~
db.createCollection('biblioteca')
~~~

> a execução foi bem-sucedida, porque o comando retornou a chave-valor {“ok”:1}.

- os nomes de coleções devem obedecer aos seguintes critérios:
  - o nome de uma coleção deve começar por uma letra ou sublinhado ("_").
  - pode conter números.
  - não pode utilizar o caractere cifrão ("$").
  - não pode exceder 128 caracteres.

- é possível criar grupos nomeados usando um ponto ("."), e esses grupos são nomeados ***namespace collection***. 
  - podemos, por exemplo, criar uma coleção para os livros de arte e outra para os romances: 
    - livros.romance
    - livros.arte

- o `comando show collections;` pode ser usado para exibir todas as coleções do banco de dados corrente.

~~~
show collections;
~~~

### 1.1.2 Documento (document)
- em um banco MongoDB, um documento é o equivalente a uma linha, ou tupla, em uma tabela de um banco relacional. 
- é representado por um conjunto de pares de chave-valor.
- exemplo:

~~~
{
  Nome: 'Fulano',
  Idade: 30
}
~~~

- o documento é aberto com uma chave e encerrado por outra chave; tudo que está entre essas duas chaves faz parte desse documento. 
- no exemplo há dois pares de chave-valor, e cada par é separado por uma vírgula. 
- entre a chave e o valor existe o símbolo de dois pontos. 
- documentos na mesma coleção não precisam ter a mesma estrutura ou conjunto de campos, e podem conter diferentes tipos de dados.

## 1.2 Tipos de dados

- o MongoDB possui suporte aos principais tipos de dados primitivos utilizados em qualquer banco de dados.
- principais tipos de dados:
  - `String`: 
    - tipo mais usado para armazenar dados. 
    - String no MongoDB deve ser um UTF-8 válido.
  - `Integer`: 
    - este tipo armazena um valor numérico. 
    - Integer pode ser 32 bits ou 64 bits, dependendo do seu servidor.
  - `Boolean`:
    - este tipo armazena um valor booleano (true ou false).
  - `Double`: 
    - tipo que armazena valores de ponto flutuante.
  - `Min/Max keys`: 
    - tipo que compara um valor contra os elementos mais baixos e mais altos de um BSON.
  - `Arrays`: 
    - este tipo armazena arrays, listas ou múltiplos valores dentro de uma chave (key).
  - `Timestamp`: ctimestamp.
    - pode ser útil para a gravação de quando um documento foi modificado ou acrescentado.
  - `Object`:
    - este tipo de dado é para incorporar documentos.
  - `Null`:
    - tipo usado para armazenar um valor nulo (null).
  - `Symbol`:
    - este tipo de dados é usado de forma idêntica ao String, porém geralmente é reservado para linguagens que usam um tipo de símbolo específico.
  - `Date`:
    - tipo de dados utilizado para armazenar a data ou a hora atual no formato de UNIX. 
    - é possível especificar seu próprio date_time por meio da criação do objeto Date e passando o dia, o mês e o ano para ele.
  - `Object ID`:
    - tipo de dados que armazena os identificadores (ID) dos documentos
  - `Binary data`:
    - este tipo de dados armazena um dado binário.
  - `Code`:
    - tipo de dados usado para armazenar código javascript dentro do documento.
  - `Regular expression`:
    - tipo de dados para armazenar expressões regulares.

- dentre os diversos tipos de dados, os mais utilizados são:
  - Numéricos.
  - String (usar '' ou "" para identificá-los).
  - Date.
  - Boolean (true ou false).
  - Array.
  
- o banco de dados MongoDB pode conter uma ou mais coleções; uma coleção pode conter diferentes tipos de documentos; e um documento pode conter um conjunto de chave-valor, arrays e documentos aninhados.
- o nome das ***variáveis shell*** no MongoDB deve iniciar por uma letra minúscula `[a-z]`; as letras seguintes podem ser minúsculas, maiúsculas ou números `[a-zA-Z0-9]`.

## 1.3 Help 

- o MongoDB possui vários comandos que fornecem ajuda ao desenvolvedor:

<div align="center">

Comando | Função | Exemplo de uso
--------|-------|-------------------
help; | Exibe uma lista de funções. | help;
db.help(); | Fornece uma lista das funções de ajuda para um determinado método do banco de dados. | db.help('insert');
db.&lt;colecao&gt;.help(); | FOrnece uma lista dos métodos aplicáveis a uma coleção. &lt;colecao&gt; indica o nome de uma coleção a ser criada ou previamente existente. | db.biblioteca.help();

</div>

- o `comando show` também ajuda muito, pois permite obter informações sobre quais coleções existem em nosso banco, quais são os bancos de dados existentes no servidor e quais são os usuários do banco de dados corrente. 
- principais usos do comando show:

<div align="center">

Comando | Função
--------|-------------
show dbs; |	Fornece uma lista de todos os bancos de dados do servidor.
show collections;	| Fornece uma lista de todas as coleções do banco de dados corrente.
show users; |	Fornece uma lista de todos os usuários do banco de dados corrente.
show roles; |	Fornece uma lista de todas as roles, predefinidas ou definidas pelo usuário, do banco de dados corrente.
show profile; |	Fornece uma lista das últimas cinco operações que levaram um milissegundo ou mais para serem executadas.
show databases;	| Fornece uma lista de todos os bancos de dados disponíveis.

</div>

- `símbolos e operadores` que usamos para trabalhar com os documentos:

<div align="center">

Operador / Símbolo	| Significado
-----------------|----------------------
() (parênteses)	| Indica o uso de um método.
[] (colchetes)	| Usado em arrays.
{} (chaves)	| Usado em documentos.
. (ponto)	| Separador de métodos.
= (igual) |	Atribui valor para variáveis.
, (vírgula)	| Separa atributos. Não deve ser usada com números.
: (dois pontos)	| Especifica o valor de um atributo.
' (aspas simples)	| Indica que o valor de um atributo é texto.
" (aspas duplas)	| Indica que o valor de um atributo é texto.
; (ponto-e-vírgula)	| Encerra um comando.

</div>

## 1.4 Criando um novo banco de dados

- ao contrário dos bancos de dados relacionais, não existe um comando create database. 
- ***o banco é fisicamente criado no momento em que uma coleção é criada***. 
- estudamos o comando createCollection para criar uma nova coleção, mas ao inserir um documento em uma coleção que não existe, o Mongo DB cria automaticamente esta coleção utilizando a configuração padrão. 
- portanto, para começar a utilizar o banco, não precisamos criar um banco nem uma coleção antes de inserirmos nosso primeiro documento. 
- para mudarmos de um banco para outro, usamos o comando use.

~~~
use db
~~~

- em que:
  - db: indica o nome do banco de dados a ser usado.
  - a variável shell db é alterada para o nome do banco de dados que será usado. 

## 1.5 Um novo banco, collection, ou documentos aninhados?

- a ***regra de ouro do MongoDB*** é: `Dados que são acessados juntos devem ser armazenados juntos`.
- idealmente, devemos contar com um único banco de dados para o mesmo tema. 
- se temos um serviço sendo desenvolvido, esse serviço terá o seu banco de dados; um bom caso de uso para a criação de um novo banco, seria para o desenvolvimento de outro serviço dentro da mesma instância do MongoDB, pois os dados ali presentes não têm nada em comum e são utilizados em produtos distintos.
- essa questão se torna mais complicada quando estamos trabalhando dentro do mesmo serviço. 
  - depois de estudar os conceitos de modelagem de dados relacional, é muito comum tentarmos segregar as “partes” de um mesmo produto em diferentes collections (seguindo os conceitos das tabelas) para garantir as regras de normalização. 
  - dentro do MongoDB, precisamos ter uma visão um pouco mais ampla identificando o que compõe o mesmo produto e que será consultado em conjunto pela nossa aplicação. 
  - exemplos:
    - analogia da conculta de um carro: todas as partes do carro compõem o produto, portanto devem estar persistidas junto com o identificador do carro e estarem acessíveis caso seja do interesse consultar mais detalhes sobre o carro sem a necessidade de enriquecer o nosso documento com dados adicionais de outro documento possivelmente presente em outra collection (nada de joins!).
    - para uma nova collection, um caso de uso interessante seria o de pessoa, pois uma pessoa é a dona deste carro, e, portanto,precisaríamos dos dados da mesma que se encontrariam em uma collection a parte dos dados do veículo em si, existindo apenas uma referência às collections através de um identificador do veículo e um identificador da pessoa.

## 1.6 Organização e escalabilidade

- modelagem de dados refere-se à organização de dados dentro de um banco de dados e aos links entre entidades relacionadas. 
- com um modelo de dados flexível, documentos dentro de uma collection não são obrigados a ter o mesmo conjunto de campos, e os tipos de dados de um campo podem diferir de documento para documento.
- essa divergência de campos e dados pode gerar problemas de organização se não tivermos cuidado.
- portanto, geralmente documentos de uma collection compartilham de estrutura semelhante, mas para garantir a consistência ***é possível criar regras de validação de esquema***, o que possibilita uma confiabilidade maior para o uso de determinadas collections.
- essas regras de validação são chamadas também de `JSON Schema`, e podem ser implementadas na criação de uma Collection da seguinte forma:

~~~
db.createCollection("students", {
   validator: {
      $jsonSchema: {
         bsonType: "object",
         title: "Student Object Validation",
         required: ["address", "major", "name", "year"],
         properties: {
            name: {
               bsonType: "string",
               description: "'name' must be a string."
            },
            year: {
               bsonType: "int",
               minimum: 2017,
               maximum: 3017,
               description: "'year' must be an integer."
            },
            gpa: {
               bsonType: ["double"],
               description: "'gpa' must be a double."
            }
         }
      }
   }
} )
~~~

- o MongoDB oferece o conceito de `operações de escrita atômicas`, na granularidade de documento.
- ou seja, se um documento com outros documentos aninhados for escrito ou atualizado, a operação só será concluída caso todos os documentos aninhados sejam corretamente modificados. 
- um modelo de dados desnormalizado permite operações atômicas, em contraste com um modelo normalizado onde as operações afetam vários documentos.

<div align="center">
<h2>2. OPERAÇÕES CRUD</h2>
</div>

- as quatro operações básicas em banco de dados são:
  - incluir dados (create).
  - consultar dados (read).
  - alterar dados (update).
  - apagar dados (delete).

- o acrônimo das quatro palavras em inglês (create, read, update e delete) forma a palavra CRUD. 
- foi convencionado que o termo CRUD indica essas quatro operações.
- a estrutura básica de um comando no banco de dados MongoDB é:

~~~
database.coleção.função()
~~~

- em que:
  - database: indica o banco de dados. A variável db contém o nome do banco de dados. 
  - coleção: mostra o nome da coleção.
  - função: aponta a função que será aplicada à coleção.

- os comandos usados pelas operações CRUD, normalmente, irão começar por db, indicando que as operações serão realizadas no banco corrente.

## 2.1 Incluindo dados

- adicionamos documentos a uma coleção por meio de uma operação de inclusão. 

> IMPORTANTE: Caso a coleção ainda não tenha sido criada, a operação de inclusão irá criá-la!

- há várias formas de incluir dados em uma coleção. 
- neste primeiro momento, iremos nos concentrar na `função insertOne`.

~~~
db.coleção.insertOne(
  <documento>,
  {
    writeConcern: <documento>
  }
)
~~~

- em que:
  - documento: indica o documento a ser inserido em uma coleção.
  - writeConcern: opcional; há 2 configurações possíveis para o writeConcern, sendo:
    - ***w***: para solicitar o reconhecimento de que a operação de gravação propagou para um número especificado de instâncias.
    - ***j***: para solicitar a confirmação de que a operação de gravação foi gravada no diário em disco.
    - ***wtimeout***: para especificar um limite de tempo para impedir que as operações de gravação sejam bloqueadas indefinidamente.

### `Exemplo de uso`: 
- incluir dados de um funcionário em uma coleção denominada "emp". Os dados do funcionário são:
  - Nome: Sullivan
  - Idade: 21
  - Cargo: Engenheiro de dados
  - Salário: 10.000,00
- ou seja, são quatro pares de chave-valor.
- converter esses dados para uma estrutura de chave-valor mais apropriada:

~~~
{
   Nome: "Sullivan",
	Idade: 21,
	Cargo: 'Engenheiro de dados',
	Salario: 10000.00
}
~~~ 

- detalhes importantes:
  - nesse caso, os campos contêm texto por aspas simples ou aspas duplas, sendo qualquer uma delas aceita, mostrando para o banco de dados que esse campo conterá um texto. 
  - campos numéricos não contêm aspas. 
  - vírgulas são usadas para separar os pares de chaves-valor. 
  - valores decimais são indicados por um ponto. 

- vamos usar a sintaxe para incluir os dados na coleção emp.

~~~
db.emp.insertOne({ Nome: "Sullivan",
                Idade: 21,
                Cargo: 'Engenheiro de dados',
                Salario: 10000.00 
})
~~~

- o resultado da operação retorna o ObjectId do documento inserido,confirmando que o comando foi bem-sucedido e que um documento foi incluído na coleção emp.

### `Outro exemplo`:
- desta vez, os dados são:
  - Nome: Mel
  - Idade:26
  - Cargo: Gerente
  - Salário: 18.830,50
  - Dependentes: Jhonny, Clarinha
  
- agora, acrescentamos a chave "Dependentes", que possui dois valores "Jhonny" e "Clarinha". 
- como esta chave tem dois valores, iremos usar um array para representá-los, lembrando que campos do tipo texto podem ser indicados usando aspas simples ou aspas duplas.

~~~
db.emp.insertOne(
              { Nome: "Mel",
                Idade: 26,
                Cargo: 'Gerente',
                Salario: 18830.50,
                Dependentes: ["Jhonny", 'Clarinha']  
}
)
~~~

### `Um exemplo um pouco mais complexo`:
- desta vez, vamos incluir informações sobre o departamento em que o empregado trabalha. 
- os dados do funcionário são:
  - Nome: Thais
  - Idade: 20
  - Cargo: Analista
  - Salário: 14.530,77
  - Dependentes: Angela, Dora, Hugo
  
- os dados do departamento são:
  - Nome: Pesquisa
  - Local: São Paulo

- vamos tratar os dados do departamento como um documento dentro do documento.

~~~
db.emp.insertOne(
             { Nome: "Thais",
               Idade: 20,
               Cargo: "Analista",
               Salario: 14530.77,
               Dependentes:["Angela", "Dora", "Hugo"],
               Departamento: { Nome: "Pesquisa",
                               Local: "São Paulo"
                              }
              }
)
~~~

- ao contrário dos bancos de dados relacionais, procuramos evitar a junção de duas coleções. 
- sempre que possível, já efetuamos a operação de junção durante a inclusão dos dados. 
- ***podemos ter vários documentos dentro do mesmo documento. Também podemos ter vetores de documentos.***

### `Outro exemplo`: incluindo, no documento do funcionário, as informações sobre as suas promoções dentro da empresa.
- dados do funcionário:
  - Nome: Fátima
  - Idade: 29
  - Cargo: Analista
  - Salário: 12.345,67
  - Dependentes: Gohan
- dados do departamento:
  - Nome: Vendas
  - Local: Campinas
- histórico de promoções:
  - Ano: 2001
  - Cargo: "Estagiário"
  - Valor: 180,00
  - Ano: 2002
  - Cargo: "Desenvolvedor"
  - Valor: 1.700,00

- vamos tratar os dados das promoções como um array de documentos dentro do documento:

~~~
db.emp.insertOne(
             { Nome: "Fátima",
               Idade: 29,
               Cargo: "Analista",
               Salario: 12345.67,
               Dependentes:["Gohan"],
               Departamento: { Nome: "Vendas",
                               Local: "Campinas"
                             },
               Promoções: [{Ano: 2001,
                            Cargo: "Estagiário",
                            Valor: 180.00},
                           {Ano: 2002,
                            Cargo: "Analista",
                            Valor: 1700.00}]
  }
  )
~~~

- ou seja, temos um array de documentos em promoções!

### 2.1.1 Inclusão de documentos por meio de variáveis

- o uso de variáveis permite que incluamos vários documentos em uma única operação.
- imagine que você queira incluir os seguintes dados em uma coleção denominada grade:

<div align="center">

| Nome                              | Autor     | Professor | Novo  | Cód  | Mês | ISBN     | Valor  |
|-----------------------------------|-----------|-----------|-------|------|-----|----------|--------|
| Estruturas de dados               | Rossetti  |           | FALSE |      |     | 12345678 | 171,13 |
| Engenharia de software            |           | Rita      |       | FES  | 2   |          |        |
| Inglês instrumental               | Ana       |           | FALSE |      |     | 45678901 | 122,99 |
| Interfaces com usuário            |           | André     |       | IU   | 2   |          |        |
| Programação orientada a objeto    | Gatti     |           | TRUE  |      |     | 23456789 | 161,16 |
| Modelagem orientada a objeto      | Rodrigo   |           |       | MO01 | 2   |          |        |
| Banco de dados                    | Angélica  |           | TRUE  |      |     | 34567890 | 133,29 |
| Gestão empresarial                | Silva     | Silva     | TRUE  | GE   | 2   | 56789012 | 156,55 |

</div>

- é possível criar uma variável usando a instrução var, que irá receber esses documentos.

~~~
var dados = [ 
{Nome: 'Estruturas de dados', 
 Autor: 'Rossetti', 
 Novo: false, 
 ISBN: 12345678, 
 Valor: 171.13},
 
{Nome: 'Engenharia de software', 
 Professor: 'Rita', 
 Cod: 'FES', Mes: 2}, 
{Nome: 'Inglês instrumental', 
 Autor: 'Ana', 
 Novo: false, 
 ISBN: 45678901, 
 Valor: 122.99},
 
{Nome: 'Interfaces com usuário',
 Professor: 'André', 
 Cod: 'IU', 
 Mes: 2},
 
{Nome: 'Programação orientada a objeto', 
 Autor: 'Gatti', 
 Novo: true, 
 ISBN: 23456789, 
 Valor: 161.16},

{Nome: 'Modelagem orientada a objeto', 
 Professor: 'Rodrigo', 
 Cod: 'MOO1', 
 Mes: 2}, 
{Nome: 'Banco de dados', 
 Autor: 'Angélica', 
 Novo: true, 
 ISBN: 34567890, 
 Valor: 133.29},
 
{Nome: 'Gestão empresarial', 
 Autor: 'Silva', 
 Professor: 'Silva', 
 Novo: true, 
 Cod: 'GE', 
 Mes: 2, 
 ISBN: 56789012, Valor: 156.55}
 ]
~~~

- esta variável pode ser usada com o `método insertMany()` para incluir os documentos na coleção grade:

~~~
db.grade.insertMany(dados);
~~~

## 2.2 Consultando dados

- os documentos que foram incluídos nas coleções podem ser consultados usando o `método find()`. 
- este método retorna um array com os objetos da coleção, mesmo que se trate de apenas um objeto.

~~~
db.Coleção.find(Consulta, Projeção, Opções)
~~~

- em que:
  - ***Coleção***: indica o nome da coleção.
  - ***Consulta***: 
    - aponta quais serão os filtros aplicados na pesquisa. 
    - para retornar todos os documentos de uma coleção, basta omitir este parâmetro ou informar um documento vazio ({}). 
    - este parâmetro é opcional.
  - ***Projeção***: 
    - exibe quais campos específicos do documento serão retornados pela pesquisa. 
    - se omitido, todos os campos do documento serão retornados. 
    - este parâmetro é opcional.
  - ***Opções***:
    - especifica opções adicionais para a query. 
    - estas opções modificam o comportamento da query e como os resultados são retornados.E
    - este parâmetro é opcional

- a consulta básica aos dados de uma coleção MongoDB é bem simples. 
- exemplo de uso:

~~~
db.grade.find();
~~~

- o método find() retornou uma lista com todos os documentos da coleção.

- podemos usar o `método findOne()` para retornar o primeiro objeto encontrado na coleção que atenda aos critérios de busca da consulta.

~~~
db.coleção.findOne(consulta, projeção, opções)
~~~

- em que:
  - ***Coleção***: indica o nome da coleção.
  - ***Consulta***:
    - mostra quais serão os filtros aplicados na pesquisa. 
    - para retornar todos os documentos de uma coleção, basta omitir este parâmetro ou informar um documento vazio ({}). 
    - este parâmetro é opcional.
  - ***Projeção***: 
    - aponta quais campos específicos do documento serão retornados pela pesquisa. 
    - se omitido, todos os campos do documento serão retornados. 
    - este parâmetro é opcional.
  - ***Opções***: 
    - especifica opções adicionais para a query. 
    - estas opções modificam o comportamento da query e como os resultados são retornados.
    - este parâmetro é opcional.

- consulta básica usando o findOne():

~~~
db.grade.findOne();
~~~

- nesse caso, apenas os dados do primeiro objeto da coleção foram exibidos.

### 2.2.1 Atributo _id

- o atributo _id tem o comportamento similar a uma chave primária em bancos de dados relacional. 
- ***cada documento possui seu próprio _id e o seu número é único***. 
- um índice único (unique index) é criado para o atributo _id quando a coleção é criada.
- ***sempre é o primeiro campo de um documento***, e ***se não for informado durante a inclusão de dados, um tipo de dados ObjectId do BSON com 12 bytes será atribuído a ele***. 
- se o valor do _id for informado e ele não for o primeiro campo, o servidor desloca o atributo para a primeira posição.

- normalmente, a `estrutura do _id` possui a seguinte composição:
  - quatro primeiros bytes indicam o timestamp da criação do objeto.
  - 3 bytes identificam a máquina.
  - 2 bytes mostram o identificador (id) do processo.
  - 3 bytes contador, iniciado com um valor aleatório.

- ordenar os documentos pelo _id é o equivalente a ordená-los pela data de criação. 
- podemos usar o `método getTimestamp()` para verificar a data de criação de um objeto. 
- exemplo de uso do método getTimestamp():

~~~
ObjectId("667b2c8bc3ae93d5a5c721b1").getTimestamp();
~~~

### 2.2.2 Pesquisa por igualdade
- uma consulta vazia ({}) retorna todos os documentos de uma coleção. 
  - não especificar uma consulta é o equivalente a uma consulta vazia. 
  - isso quer dizer que o comando db.emp.find() é equivalente a db.emp.find({}).
- listar todos os dados de uma coleção é útil, mas as vezes precisamos fazer consultas por igualdade.
- para isso, basta usar o `método find() com uma chave-valor`. 
- exemplo:

~~~
db.emp.find({Idade: 20});
~~~

- neste exemplo, serão listados todos os documentos cujo atributo Idade seja igual a 20. 
- ***o método find() pode encadear vários métodos para melhorar a busca de documentos***. 
  - por exemplo, podemos encadear o `método pretty` para formatar a saída de nossa consulta e deixá-la mais legível. 

- o exemplo a seguir ilustra a ***saída formatada***.

~~~
db.emp.find({Idade: 19}).pretty;
~~~

### 2.2.3 Classificando a saída dos dados
- vários outros métodos podem ser concatenados ao método find(), o `método sort()`, por exemplo, ***classifica a saída em ordem crescente ou decrescente***. 
  - o `valor 1` no parâmetro indica que os dados serão classificados em ordem crescente.
  - o `valor -1` no parâmetro indica que os dados serão classificados em ordem decrescente.

- exemplos de utilização:

~~~
db.emp.find().sort({Nome:1});
db.emp.find().sort({Idade:-1});
db.emp.find().sort({Cargo:1, Idade:-1});
~~~

- no exemplo, o primeiro comando retorna todos os documentos da coleção emp classificados pelo nome, em ordem crescente; 
- o segundo comando retorna todos os documentos da coleção classificados pela idade, em ordem decrescente; 
- e o último comando retorna todos os documentos classificados pelo cargo, em ordem crescente, e pela idade, em ordem decrescente.

### 2.2.4 Operadores de comparação
- operadores de comparação retornam um valor booleano. 
- comparam as duas expressões informadas nos argumentos e retornam o valor true ou false.

- `$lt` (abreviação de less than): 
  - verifica se um valor é menor que outro. 
  - retorna true se o primeiro valor for menor que o segundo.

~~~
db.emp.find({Idade:{$lt: 22}});
~~~

- `$lte` (abreviação de less than or equal):
  - analisa se um valor é menor ou igual a outro. 
  - retorna true se o primeiro valor for menor ou igual ao segundo.

~~~
db.emp.find({Idade:{$lte: 22}});
~~~

- `$gt` (abreviação de greater than):
  - avalia se um valor é maior que outro. 
  - retorna true se o primeiro valor for maior que o segundo.

~~~
db.emp.find({Idade:{$gt: 22}});
~~~

- `$gte` (abreviação de greater than or equal):
  - apura se um valor é maior ou igual a outro. 
  - retorna true se o primeiro valor for maior ou igual ao segundo.

~~~
db.emp.find({Idade:{$gte: 22}});
~~~

- o quadro abaixo resume esses operadores e os exemplifica:

<div align="center">

| Operador | Significado          | Exemplo de Uso                     |
|----------|----------------------|------------------------------------|
| $lt      | Menor que            | db.emp.find({Idade:{$lt: 23}});    |
| $lte     | Menor que ou igual a | db.emp.find({Idade:{$lte: 23}});   |
| $gt      | Maior que            | db.emp.find({Idade:{$gt: 23}});    |
| $gte     | Maior que ou igual a | db.emp.find({Idade:{$gte: 23}});   |


</div>

- o quadro abaixo compara o uso de comandos SQL dos bancos de dados relacionais e o MongoDB:

<div align="center">

| Exemplo em SQL                      | Exemplo em MongoDB                    |
|-------------------------------------|---------------------------------------|
| SELECT * FROM emp                   | db.emp.find()                         |
| SELECT * FROM emp WHERE idade = 33  | db.emp.find({idade: 33})              |
| SELECT * FROM emp WHERE idade > 33  | db.emp.find({idade: {$gt: 33}})       |
| SELECT * FROM emp WHERE idade >= 33 | db.emp.find({idade: {$gte: 33}})      |
| SELECT * FROM emp WHERE idade < 33  | db.emp.find({idade: {$lt: 33}})       |
| SELECT * FROM emp WHERE idade <= 33 | db.emp.find({idade: {$lte: 33}})      |

</div>

### 2.2.5 Operadores lógicos

- `$or`: retorna documentos pesquisados quando pelo menos uma das cláusulas de comparação for verdadeira.

~~~
db.emp.find({$or:[{Idade:{$gt:21}},{Salario:{$lt:12500}}]});
~~~

- `$and`: retorna documentos pesquisados quando todas as cláusulas de comparação forem verdadeiras.

~~~
db.emp.find({$and:[{Idade:{$gt:21}},{Salario:{$lt:12500}}]});
ou
db.emp.find({Idade: {$gt: 21},Salario:{$lt: 12500}});
~~~

- `$nor`: retorna documentos pesquisados que não satisfaçam as cláusulas de comparação. 

~~~
db.emp.find({$nor:[{Idade:{$gt:22}},{Salario:{$gt:12500}}]});
~~~

- `$ne`: retorna documentos pesquisados cujos valores sejam diferentes do valor informado. 

~~~
db.emp.find({Idade:{$ne:22}});
~~~

- `$not`: efetua uma operação NOT lógica. 

~~~
db.emp.find({Idade:{$not:{$gt:21}}});
~~~

<div align="center">

| Operador | Significado | Exemplo de Uso                   |
|---|---|---|
| $or      | Ou lógico   | db.emp.find({$or:[{Idade:{$lt:21}}, {Salario:{$gt:12500}}]}); |
| $and     | E lógico    | db.emp.find({Idade: {$lt: 21}, Salario: {$gt: 12500}});     |
| $nor     | NOR lógico  | db.emp.find({$nor:[{Idade:{$lt:21}}, {Salario:{$gt:12500}}]}); |
| $ne      | Diferente    | db.emp.find({Idade: {$ne:21}});                        |

</div>

- comparação de comandos SQL com MongoDB:

<div align="center">

| Exemplo em SQL                               | Exemplo em MongoDB                                     |
|---|---|
| SELECT * FROM clientes WHERE idade > 30       | db.clientes.find({ idade: { $gt: 30 } })              |
| SELECT nome, sobrenome FROM funcionários     | db.funcionarios.find({}, { nome: 1, sobrenome: 1 }) |
| SELECT COUNT(*) FROM pedidos                 | db.pedidos.countDocuments({})                         |
| SELECT AVG(valor) FROM vendas                | db.vendas.aggregate([{ $avg: "$valor" }])           |
| SELECT * FROM produtos WHERE categoria = 'Eletrônicos' | db.produtos.find({ categoria: 'Eletrônicos' })       |
| SELECT * FROM clientes ORDER BY nome ASC      | db.clientes.find().sort({ nome: 1 })                |

</div>

### 2.2.6 Operadores para tratamento de arrays

- `$in`: retorna documentos nos quais o valor pesquisado esteja na lista de valores de um array.

~~~
db.emp.find({Dependentes:{$in:["Jhonny", "Clarinha"]}});
~~~

- `$nin`: retorna documentos nos quais o valor pesquisado não esteja na lista de valores de um array ou o campo pesquisado não exista no documento. 

~~~
db.emp.find({Dependentes:{$nin:["Rosa", "Rita"]}});
~~~

- `$all`: retorna documentos nos quais todos os valores pesquisados estejam na lista de valores de um array.

~~~
db.emp.find({Dependentes:{$all:["Gohan"]}});
~~~

- `$exists`: não específico para o uso em arrays, mas pode ser utilizado para complementar os operadores $in e $nin. Testa se um campo existe em um documento ou não. No exemplo, usamos o método count(), que conta o número de elementos retornados.

~~~
db.emp.find({Dependentes:{$exists: true}}).count()
ou
db.emp.find({Dependentes:{$exists: false}}).count()
~~~

<div align="center">

| Operador | Exemplo de Uso                                        |
|---|---|
| $in      | db.emp.find({Dependentes: {$in:["Dora", "Pietra"]}}); |
| $nin     | db.emp.find({Dependentes: {$nin:["Dora", "Pietra"]}}); |
| $all     | db.emp.find({Dependentes: {$all:["Dora", "Hugo"]}});   |
| $exists  | db.emp.find({Dependentes: {$exists:true, $nin:["Dora", "Pietra"]}}); |

</div>

### 2.2.7 Projeção
- o uso do opcional de projeção permite indicar quais campos serão retornados pelo método find().

~~~
db.emp.find({Idade:20},{_id:0, Idade:0, Cargo:0});
~~~

- no exemplo, serão apontados todos os campos dos funcionários com idade igual a 20 anos, exceto os campos _id, Idade e Cargo.
- o quadro abaixo compara o uso de comandos SQL dos bancos de dados relacional e o MongoDB.

<div align="center">

| Exemplo em SQL | Exemplo em MongoDB |
|--------------------------|----------------|
| SELECT * FROM emp | db.emp.find() |
| SELECT Nome, Salario FROM emp WHERE Idade = 22 | db.emp.find({Idade:22},{_id:0, Idade:0, Cargo:0})      |
| SELECT Nome, Salario FROM emp WHERE Idade = 22 | db.emp.find({Idade:22},{Nome:1, Salario:1})     |

</div>

### 2.2.8 Expressões regulares
- em MongoDB, expressões regulares permitem pesquisar por padrões em campos texto. 
- há duas formas para a sintaxe de expressões regulares.

~~~
{<campo>: { $regex: /padrão/, $options: '<opções>' } }
{<campo>: { $regex: 'padrão', $options: '<opções>' } }
{<campo>: { $regex: /padrão/<opções> } }
Ou
{ <campo>: /padrão/<opções> }
~~~

- em que:
  - `Campo`: indica o campo onde será pesquisado o padrão.
  - `Padrão`: mostra o padrão a ser pesquisado.
  - `Opções`: aponta as opções a serem usadas com a expressão regular. Algumas das opções são:
    - ***i***: indica case insensitive, isto é, o texto pode estar tanto em letras maiúsculas quanto em minúsculas.
    - ***m***: usado em padrões que incluem âncoras, isto é, ^ para o início de um texto e $ para o final do texto.
    - ***x***: utilizado para ignorar todos os espaços em branco no padrão.

- alguns exemplos:

~~~
db.emp.find({Nome:{$regex:"^F"}});
// Pesquisa por todos os nomes iniciados pela letra F maiúscula.
~~~

~~~
db.emp.find({Nome:{$regex:"^f", $options: 'i'}});
// Pesquisa pelos nomes iniciados pela letra F, 
//independente se maiúscula ou minúscula.
~~~

~~~
db.emp.find({Nome:{$regex:"i"}});
// Pesquisa por todos os nomes que possuem a letra i, 
// minúscula, em qualquer parte do nome.
~~~

~~~
db.emp.find({Nome:{$regex:"i", $options: 'i'}});
// Pesquisa por todos os nomes que possuem a letra i, 
// independente se for maiúscula ou minúscula,
// em qualquer parte do nome.
~~~

~~~
db.emp.find({Nome:{$regex:"n$"}});
// Pesquisa por todos os nomes terminados pela letra n minúscula.
~~~

<div align="center">

Exemplo em SQL | Exemplo em MongoDB
----------------|------------------
SELECT * FROM emp WHERE nomeLIKE '%h%' | db.emp.fund({Nome: {regex:"h"}});

</div>

## 2.3 Alterando dados

- o `método updateOne()` permite alterar um ou mais documentos em uma coleção. 
- é possível alterar um ou mais campos de um documento ou, até mesmo, o documento inteiro. 
- o método updateOne() altera apenas um documento por vez. 
- sua sintaxe é:

~~~
db.coleção.updateOne(consulta, atualização, opções)
~~~

- em que:
  - `Coleção`: indica o nome da coleção.
  - `Consulta`: aponta quais serão os critérios para pesquisar o documento que será alterado.
  - `Atualização`: mostra a modificação que será feita. Aqui precisamos adicionar o ***comando $set*** antes das alterações que serão realizadas. 
  - `Opções`: apresenta quais são as opções para o comando. Entre as opções, estão:
    - ***upsert***: opcional. Se definido como true, cria um novo documento caso nenhum documento atenda aos critérios de busca. O padrão é false.
    - ***multi***: opcional. Se definido como true, atualiza todos os documentos que atendam aos critérios de busca. O padrão é false.

- o método updateOne() pode alterar um documento inteiro. 
- exemplo:

~~~
db.paises.find();
db.paises.insertOne({_id:504,Nome:"Mauritânia",Capital: "Nouakchott"});
db.paises.find();
db.paises.updateOne({_id:504},{$set:{Pais:"Honduras", Capital:"Tegucigalpa"}});
~~~

- neste caso, o documento inteiro foi substituído. 
- o método updateOne() tem ***operadores*** para tratar os campos individualmente:
  - `$inc`: incrementa o valor de um campo pelo valor informado no operador.

~~~
db.emp.insert({Nome: "Ana", idade: 22});
db.emp.find({Nome: "Ana"});
db.emp.updateOne({Nome: "Ana"}, {$inc: { idade: 5}});
db.emp.find({Nome: "Ana"});
~~~

  - `$set`: modifica o conteúdo de um campo específico. Caso o campo não exista, um novo campo será acrescido ao documento.

~~~
db.emp.updateOne({Nome: "Ana"}, {$set: { idade: 22}});
db.emp.find({Nome: "Ana"});
db.emp.updateOne({Nome:"Ana"}, {$set: {salario: 15000}});
db.emp.find({Nome: "Ana"});
~~~

  - `$rename`: renomeia um campo.

~~~
db.emp.find({Nome: "Ana"});
db.emp.updateOne({Nome:"Ana"},{$rename:{"salario":"Remuneracao"}});
db.emp.find({Nome: "Ana"});
~~~

  - `$unset`: remove um campo específico de um documento.

~~~
db.emp.find({Nome: "Ana"});
db.emp.updateOne({Nome:"Ana"},{$unset:{remuneracao:""}});
db.emp.find({Nome: "Ana"});
~~~

<div align="center">

| Exemplo em SQL | Exemplo em MongoDB |
|--------------------------|-------------------------------------|
| UPDATE emp SET Idade = Idade + 2 WHERE Nome = 'Ana' | db.emp.update({Nome: "Ana"}, {$inc: {Idade: 2}}, {multi: true}) |
| UPDATE emp SET Idade = 33 WHERE Nome = 'Ana' | db.emp.update({Nome: "Ana"}, {$set: {Idade: 33}}, {multi: true}) |

</div>

## 2.4 Eliminando dados

- o `método deleteMany()` elimina todos os documentos que atendam às condições de pesquisa especificadas.
- sintaxe:

~~~
db.coleção.deleteMany(consulta);
~~~

- em que:
  - ***Coleção***: mostra o nome da coleção.
  - ***Consulta***: indica quais serão os critérios para pesquisar o documento que será removido. 
  
> Se todos os documentos forem eliminados, mesmo assim a coleção continuará existindo. Caso queira eliminar a coleção, use o `método drop();`!

~~~
db.apaga.insertOne({cod:7369,nome:"Maria",cargo:"DBA"});
db.apaga.insertOne ({cod:7499,nome:"Rosa",cargo:"DBA"});
db.apaga.insertOne({cod:7521,nome:"Ana",cargo:"DBA"});
db.apaga.insertOne({cod:7566,nome:"Rita",cargo:"DBA"});
db.apaga.find();
db.apaga.deleteMany({cod:{$gt:7521}});
db.apaga.find();
db.apaga.deleteMany({cargo:"DBA"});
db.apaga.find();
show collections;
db.apaga.drop();
show collections;
~~~

<div align="center">
<h2>3. CONSULTAS COM AGREGAÇÃO</h2>
</div>

- para fins analíticos, o MongoDB conta com um componente chamado `Aggregation Framework`. 
- esse componente é responsável por consultas onde teremos transformações ou agregações dos dados, permitindo que cálculos sejam processados dentro do próprio ambiente do MongoDB. 
- até o momento aprendemos como consultar os documentos utilizando filtros, agora poderemos retornar resultados que possibilitem alguma tomada de decisão sem termos que ler individualmente cada um dos documentos retornados em nossas consultas.

## 3.1 Introdução ao Aggregation Framework

- no MongoDB, há duas formas básicas de consultar dados: 
  - através do comando find(), 
  - e através do ***framework de agregação*** pelo `comando aggregate()`.
- enquanto o comando find possibilita transformações simples como contagens e ordenação de documentos, o comando aggregate possibilita um novo leque de opções que são geralmente utilizadas para finalidades analíticas e construção de relatórios. 
- ***o Aggregation Framework é definido por uma sequência de operações (Stages), que chamaremos de “Pipeline” seguindo o padrão da documentação do MongoDB***.

## 3.2 Stages

- há cerca de 40 estágios de agregação disponíveis, que nos permitem realizar uma série de transformações para diferentes finalidades. 
- algumas características:
  - um pipeline será iniciado com documentos.
  - para cada estágio, os documentos entram, são trabalhados, e um ou mais documentos saem.
  - alguns estágios podem aparecer mais de uma vez no mesmo pipeline.

- a sintaxe de um pipeline pode se tornar volumosa, pois cada estágio é adicionado como um componente dentro de uma lista, que será o principal parâmetro do comando aggregate.

~~~
db.coleção.aggregate(pipeline, opções);
~~~

- em que:
  - ***Coleção***: indica o nome da coleção.
  - ***Pipeline***: lista com todos os estágios a serem executados. Podemos definir o pipeline como uma variável externa para facilitar a definição de cada estágio.
  - ***Opções***: apresenta quais são as opções para o comando. Parâmetro opcional.

- podemos focar em 3 estágios fundamentais do aggregation framework: `$match`, `$project` e `$group`. 
- se os 3 forem utilizados, uma variável contendo a sequência de comandos seguiria um padrão semelhante ao exemplo:

~~~
pipeline = [
        { $match : { … } },
        { $project : { … } },
        { $group : { … } }
       ]
~~~

### 3.2.1 Match
- o comando $match funciona como um filtro, onde reduzimos os documentos que utilizaremos no nosso pipeline de acordo com os critérios definidos. 
- a sintaxe se assemelha bastante ao find, onde passamos uma consulta para realizar nosso filtro.

~~~
db.emp.aggregate([{$match: {Salario: {$gte: 12000}}}]);
~~~

### 3.2.2 Project
- funciona como um SELECT se fizermos uma analogia ao SQL, onde reduzimos a quantidade de campos a serem retornados dos nossos documentos.
- o campo _id precisa ser explicitamente removido caso não seja necessário, enquanto os demais podem ser apenas ignorados no comando.
- para um exemplo onde precisamos trazer apenas Nome e Salário dos documentos na collection emp:

~~~
db.emp.aggregate([{$project: {_id: 0, Nome: 1, Salario: 1}}]);
~~~

### 3.2.3 Group
- utilizado para operações com funções de agregação.
- as funções mais comuns são: `$count`, `$max`, `$min`, `$avg` e `$sum`.
- de forma semelhante ao SQL, definimos quais os campos de agregação e quais as funções que aplicaremos a eles. 
- em um cenário onde precisaríamos calcular a média dos salários da collection emp:

~~~
db.emp.aggregate([{$group: {_id:null, SalarioMedio: {$avg: '$Salario'}}}]);
~~~

- no qual:
  - _id: é um campo obrigatório, sendo um ou mais campos que trarão a granularidade da agregação. No caso queremos a média total dos documentos, por isso o valor Null.
  - a função $avg recebe o campo Salário no formato ‘$Salario’.

## 3.3 O pipeline completo

- para encadearmos todos os estágios anteriores vamos organizar o código em uma variável e executar da seguinte forma:

~~~
pipeline = [
  {$match: {Salario: {$gte: 12000}}},
  {$project: {_id: 0, Nome: 1, Salario: 1}},
  {$group: {_id:null, SalarioMedio: {$avg: '$Salario'}}}
];

db.emp.aggregate(pipeline);
~~~

## 3.4 Performance em consultas

- boas práticas na hora de utilizarmos esse banco. 
- a modelagem de dados é um tópico extremamente importante para a escalabilidade de um projeto utilizando MongoDB, principalmente visando a velocidade das consultas.
- algumas técnicas e boas práticas podem ser utilizadas em paralelo para garantir performance:
  - `Indexação`:
    - para melhorar o desempenho das queries que seu aplicativo executa com frequência, crie índices nos campos comumente consultados.
    - à medida o aplicativo cresce, monitore o uso do índice da implantação para garantir que os índices ainda estejam dando suporte a consultas relevantes.
    - ao mesmo tempo, não mantenha índices que não são mais utilizados pois eles depreciam a performance do banco para serem mantidos. 
    - utilize a visualização de cobertura de índices do Atlas para auxiliar na definição e gerenciamento dos índices da sua instancia de MongoDB.
    - índices são simples de serem criados, a gestão deles que é o real desafio, garantindo que ainda estão eficientes e úteis no decorrer do tempo. 
    - para criar um novo índice, podemos utilizar o comando ***createIndex***:

~~~
db.emp.createIndex({Nome: -1});
~~~

> Para visualizar todos os índices existentes em uma coleção, podemos executar o `comando getIndexes`:

~~~
db.emp.getIndexes();
~~~

> Perceba que criamos apenas um índice, mas o comando retornou dois. Isso se deve ao campo _id, que por si só já é um índice criado automaticamente em todas as coleções e, por padrão, deve ser mantido para garantir a performance correta para consultas com este campo. Como em nossa coleção emp sabemos que grande parte das consultas utilizam o nome do funcionário, adicionar o índice no campo Nome ajuda na performance conforme o volume de dados na coleção aumentar.

- `$lookup`:
  - MongoDB não oferece joins, mas o Lookup visa atender algumas demandas onde é necessário enriquecer um documento com mais dados do que os disponíveis. 
  - não deve ser utilizado como uma funcionalidade padrão em nossas consultas pois reduz drasticamente a performance do banco. 
  - é um recurso que deve ser utilizado como suporte para operações não planejadas, mas não parte do modelo de dados.
  
- `Regex`:
  - evite o uso de expressões regulares que não diferenciam maiúsculas de minúsculas pois estas configurações podem ser feitas em nível de indexação e são muito mais rápidas.

---

## FAST TEST

### 1. Qual comando é utilizado para obter informações sobre quais coleções existem em nosso banco, quais são os bancos de dados existentes no servidor e quais são os usuários do banco de dados corrente?
> show.

### 2. Em relação aos tipos de dados suportados pelo MongoDB, assinale a alternativa correta:
> Code: tipo de dados utilizado para armazenar código JavaScript dentro do documento.


### 3. Assinale a alternativa que completa corretamente a seguinte frase: "Em um banco MongoDB, um _____________ é o equivalente a uma tupla em uma tabela de um banco relacional. O _____________ é representado por um conjunto de pares de chave-valor".
> documento / documento.

--- 

[Voltar ao início!](https://github.com/DigouO/Smart_Cities_FIAP_2024)