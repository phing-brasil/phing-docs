# Disclaimer

Phing Brasil is a project to translate the documentation of Phing in Portuguese from Brazil. We aren't related to [Phing official](https://github.com/phingofficial/phing) project. To any issue please report to the official project, here our  goal is just translate the documentation.

# Phing-docs

Phing-docs é um projeto criado para a tradução da documentação oficial do projeto Phing. A documentação original do projeto Phing pode ser encontrada em https://www.phing.info/trac/wiki/Users/Documentation

## Tradução

Você pode visualizar a documentação traduzida para o Português em **http://phing-brasil.github.io/phing-docs/**

# Phing

Phing (PHing Is Not GNU make) é um sistema de automação de tarefas baseado no Apache Ant, mas escrito em PHP. Você pode fazer qualquer coisa com ele que você faria com um sistema de automação tradicional, como o [GNU make](https://www.gnu.org/software/make/).

Se você encontrar-se escrevendo scripts personalizados para lidar com implantação ou testes de seus aplicativos,  sugerimos que olhe o Phing. Phing possui várias tarefas prontas para serem utilizadas, e um modelo Orientado a Objetos para que seja possível estender ou adicionar suas próprias tarefas.

Para maiores informações acesso o site oficial [Phing.info](https://www.phing.info/)

# Como contribuir ?

Phing-docs utiliza o Docpad para gerar as páginas para isso você irá precisar seguir os seguintes passos

1. Instalar o node.js (https://nodejs.org/en/download/)
2. Instalar o NPM     (https://docs.npmjs.com/getting-started/installing-node)
3. instalar o Docpad  (http://docpad.org/docs/install)

Após ter instalado todos os itens necessários estamos prontos para seguir em frente, a primeira coisa que devemos fazer é clonar o repositório

```
git clone https://github.com/seu-usuario/phing-docs.git phing-docs
```

Após isso devemos entrar na pasta em que o repositório foi clonado

```
cd phing-docs/
```

E executar o seguinte comando para instalar as dependências do projeto phing-docs

```
npm install
```

Para ter certeza de que tudo ocorreu bem vamos executar o docpad

```
docpad run
```

Após o docpad gerar os arquivos ele irá automaticamente criar um servidor local na porta 9778, para ter certeza que tudo está correto você deverá ver uma paǵina como essa http://phing-brasil.github.io/phing-docs/ ao acessar no seu navegar a URL **http://localhost:9778**

```
firefox localhost:9778
```

