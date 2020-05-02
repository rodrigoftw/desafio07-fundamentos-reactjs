<img alt="GoStack" src="https://storage.googleapis.com/golden-wind/bootcamp-gostack/header-desafios.png" />

<h3 align="center">
  Desafio 07: GoFinances Web
</h3>

## :rocket: Sobre o desafio

Nesse desafio, deve-se continuar desenvolvendo a aplicação de gestão de transações, a GoFinances. Agora deve-se praticar o que foi aprendido até agora no React.js junto com TypeScript, utilizando rotas e envio de arquivos por formulário.

Essa será uma aplicação que irá se conectar ao backend do [Desafio 06](https://github.com/rodrigoftw/desafio06-database-upload), exibir as transações criadas e permitir a importação de um arquivo CSV para gerar novos registros no banco de dados.

Deve-se executar o comando `yarn` no terminal para instalar todas as dependências.

### Preparando o backend

Antes de tudo, para que o frontend se conecte corretamente ao backend, deve-se ir até a pasta do `backend` e executar o comando `yarn add cors @types/cors`.

Depois disso deve-se ir até o `app.ts` ainda no backend, importar o `cors` e adicionar `app.use(cors())` antes da linha que contém `app.use(routes)`;

Além disso, deve-se se certificar que as informações da categoria, estão sendo retornadas junto com a transação do backend no formato como o seguinte:

```json
{
  "id": "c0512e43-6589-4dc8-bd0c-2a3f71b347aa",
  "title": "Loan",
  "type": "income",
  "value": "1500.00",
  "category_id": "d93eccc7-64bb-4268-b825-9200103f3a8b",
  "created_at": "2020-04-20T00:00:49.620Z",
  "updated_at": "2020-04-20T00:00:49.620Z",
  "category": {
    "id": "d93eccc7-64bb-4268-b825-9200103f3a8b",
    "title": "Others",
    "created_at": "2020-04-20T00:00:49.594Z",
    "updated_at": "2020-04-20T00:00:49.594Z"
  }
}
```

Para isso, deve-se utilizar a funcionalidade de eager loading do TypeORM, adicionando o seguinte na model de transactions:

```js
@ManyToOne(() => Category, category => category.transaction, { eager: true })
@JoinColumn({ name: 'category_id' })
category: Category;
```

Deve-se também fazer o mesmo na model de Category, mas referenciando a tabela de Transaction.

```js
@OneToMany(() => Transaction, transaction => transaction.category)
transaction: Transaction;
```

### Layout da aplicação

Essa aplicação possui um layout que você pode seguir para conseguir visualizar o seu funcionamento.

O layout pode ser acessado através da página do Figma, no [seguinte link](https://www.figma.com/file/EgOhyj1Inz14dhWGVhRlhr/GoFinances?node-id=1%3A863).

Você precisará uma conta (gratuita) no Figma pra inspecionar o layout e obter detalhes de cores, tamanhos, etc.

### Funcionalidades da aplicação

Deve-se verificar os arquivos da pasta `src` e completar onde não possui código, com o código para atingir os objetivos de cada rota.

- **`Listar as transações da API`**: A página `Dashboard` deve ser capaz de exibir uma listagem através de uma tabela, com o campo `title`, `value`, `type` e `category` de todas as transações que estão cadastradas na API.

- **`Exibir o balance da API`**: A página `Dashboard` deve exibir o balance que é retornado do backend, contendo o total geral, junto ao total de entradas e saídas.

- **`Importar arquivos CSV`**: Na página `Import`, deve-se permitir o envio de um arquivo no formato `csv` para o backend, que irá fazer a importação das transações para o banco de dados. O arquivo csv deve seguir o seguinte [modelo](https://github.com/Rocketseat/bootcamp-gostack-desafios/blob/master/desafio-database-upload/assets/file.csv).

### Específicação dos testes

Em cada teste, há uma breve descrição no que a aplicação deve cumprir para que o teste passe.

Para esse desafio, existem os seguintes testes:

- **`should be able to list the total balance inside the cards`**: Para que esse teste passe, a aplicação deve permitir que sejam exibidos na Dashboard, cards contendo o total de `income`, `outcome` e o total da subtração de `income - outcome` que são retornados pelo balance do backend.

* **`should be able to list the transactions`**: Para que esse teste passe, a aplicação deve permitir que sejam listadas dentro de uma tabela, todas as transações que são retornadas do backend.

**Dica**: Para a exibição dos valores na listagem de transações, as transações com tipo `income` devem exibir os valores no formado `R$ 5.500,00`. Transações do tipo `outcome` devem exibir os valores no formato `- R$ 5.500,00`.

- **`should be able to navigate to the import page`**: Para que esse teste passe, deve-se permitir a troca de página através do Header, pelo botão que contém o nome `Importar`.

- **`should be able to upload a file`**: Para que esse teste passe, deve-se permitir que um arquivo seja enviado através do componente de drag-n-drop na página de `import`, e que seja possível exibir o nome do arquivo enviado para o input.
