<div style="text-align: center;"><h1> Axios </h1></div>

# Sumário
- [Introdução Conceitual e Técnica](#introdução-conceitual-e-técnica)
  - [Introdução](#introdução)
  - [Features](#features)
  - [Começando a utilizar o Axios](#começando-a-utilizar-o-axios)
- [Conceitos de Domínio](#conceitos-de-domínio)
  - [URL](#url)
  - [Requisições e respostas HTTP](#requisições-e-respostas-http)
  - [Status de resposta à requisição HTTP](#status-de-resposta-à-requisição-http)
  - [Interceptadores (funções de _middleware_)](#interceptadores-funções-de-middleware)
  - [JSON](#json)
  - [XHR - XMLHttpRequest](#xhr---xmlhttprequest)
  - [Cliente-Servidor](#cliente-servidor)
  - [Isomorfismo](#isomorfismo)
  - [XSRF](#xsrf)
- [Cenários de Uso](#cenários-de-uso)
  - [Cenário: Realizar operações CRUD em um servidor](#cenário-realizar-operações-crud-em-um-servidor)
  - [Cenário: Abortar uma requisição](#cenário-abortar-uma-requisição)
  - [Cenário: Repetição de requisição com interceptadores](#cenário-repetição-de-requisição-com-interceptadores)
  - [Cenário: Implementar um indicador de progresso](#cenário-implementar-um-indicador-de-progresso)
  - [Cenário: Manipular *Headers* de uma requisição](#cenário-manipular-headers-de-uma-requisição)
  - [Cenário: Tratar erros em respostas à requisições](#cenário-tratar-erros-em-respostas-à-requisições)
  - [Cenário: Realizar *upload* de arquivos](#cenário-realizar-upload-de-arquivos)
  - [Cenário: Autenticação de usuário](#cenário-autenticação-de-usuário)
  - [Cenário: Configurar um timeout para uma requisição](#cenário-configurar-um-timeout-para-uma-requisição)
- [Fatos de Execução](#fatos-de-execução)
  - [Referência: axios.create](#referência-axios.create)
  - [Referência: axios.config](#referência-axios.config)
  - [Referência: axios.request](#referência-axios.request)
  - [Referência: axios.head](#referência-axios.head)
  - [Referência: axios.get](#referência-axios.get)
  - [Referência: axios.post](#referência-axios.post)
  - [Referência: axios.put](#referência-axios.put)
  - [Referência: axios.patch](#referência-axios.patch)
  - [Referência: axios.delete](#referência-axios.delete)
  - [Referência: axios.options](#referência-axios.options)
  - [Referência: axios.postForm, axios.putForm e axios.patchForm](#referência-axios.postForm-axios.putForm-e-axios.patchForm)
  - [Referência: axios.interceptors](#referência-axios.interceptors)
  - [Funcionamento interno: Envio de requisições e recebimento de respostas](#funcionamento-interno-envio-de-requisições-e-recebimento-de-respostas)
  - [Funcionamento interno: Da chamada de função a requisição](#funcionamento-interno-da-chamada-de-função-a-requisição)
  - [Funcionamento interno: Interceptadores: uso e funcionamento](#funcionamento-interno-interceptadores:-uso-e-funcionamento)
  - [Funcionamento interno: Conversão dinâmica em JSON](#funcionamento-interno-conversão-dinâmica-em-JSON)
  - [Efeito colateral: Axios e problemas de compatibilidade com navegadores antigos](#efeito-colateral-axios-e-problemas-de-compatibilidade-com-navegadores-antigos)
  - [Efeito colateral: Ineficiência no carregamento de dados relacionados](#efeito-colateral-ineficiência-no-carregamento-de-dados-relacionados)
  - [Efeito colateral: Axios e dependência de outras bibliotecas ou frameworks](#efeito-colateral-axios-e-dependência-de-outras-bibliotecas-ou-frameworks)

-------------------------------------------------------------

# Introdução Conceitual e Técnica

## Introdução

Axios é uma biblioteca cliente HTTP baseado-em-promessas para o node.js e para o navegador. É isomórfico, ou seja, pode rodar no navegador e no node.js com a mesma base de código. No lado do servidor usa o código nativo do node.js - o modulo `http`, enquanto no lado do cliente (navegador) usa `XMLHttpRequests`.

-------------------------------------------------------------

## *Features*

Além de possibilitar a realização de requisições, o Axios traz algumas vantagens, como por exemplo:
* Suporte para navegadores mais antigos;
* Tratamento simples de erros;
* Interceptação de respostas;
* Suporte a _Promises_;
* Conversão automática de dados em JSON;
* Tem suporte a falsificação de solicitações entre sites, conhecido como XRSF.

-------------------------------------------------------------

## Começando a utilizar o Axios

O principal propósito do Axios é permitir a realização de requisições para diferentes APIs. Então nessa sessão, iremos mostrar um simples exemplo de como consumir uma API externa.

### Instalação

Utilizando o npm:
```console
$ npm install axios
```

Utilizando o bower:
```console
$ bower install axios
```

Utilizando o yarn:
```console
$ yarn add axios
```

Utilizando a CDN do jsDelivr:
```javascript
<script src="https://cdn.jsdelivr.net/npm/axios/dist/axios.min.js"></script>
```

Utilizando a CDN do unpkg:
```javascript
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>
```

### Configuração do projeto

Em seguida, crie um projeto **React**.

Para organizar melhor o nosso projeto, vamos criar um arquivo chamado `api.js`, que vai ser responsável por toda a configuração e importação da biblioteca Axios.

Crie uma pasta dentro da pasta raiz do projeto chamada *services* e dentro dela um arquivo chamado `api.js`.

O código do arquivo `api.js` deve seguir o seguinte modelo:

```javascript
import axios from "axios"; // importa a biblioteca

const api = axios.create({ // instancia e configura o objeto api, que será utilizado para as demais operações
  baseURL: "https://api.minha-api.com", // define a URL da API a ser acessada
});

export default api; // exportamos nosso objeto para importação por outros arquivos do projeto
```

Note que a API precisa ser definida dentro desse arquivo, seguindo à risca o modelo demonstrado acima!

### Utilizando o Axios

Com o nosso objeto criado de acesso à API configurado, vamos abrir o arquivo principal `app.js` e importá-lo.

```javascript
import React, { useEffect, useState } from "react"; // importa a biblioteca React e hooks necessários
import api from "./services/api"; // importação do objeto de acesso à api

export default function App() {
  const [user, setUser] = useState(); // define estado inicial para dados do usuário

  return ( // retorna um elemento HTML com as informações do usuário
    <div className="App">
      <p>Usuário: {user?.login}</p>
      <p>Biografia: {user?.bio}</p>
    </div>
  );
}
```

No exemplo de código acima, estamos tentando acessar dois atributos (`login` e `bio`) de um usuário, sendo seus dados armazenados por `user`. Esses dados serão acessados através de uma requisição HTTP que faremos através do Axios. Vamos adicionar a requisição ao trecho de código acima:

```javascript
import React, { useEffect, useState } from "react";
import api from "./services/api";

export default function App() {
  const [user, setUser] = useState();
    
  useEffect(() => { // define um hook a ser executado toda vez que a página é carregada
    api.get("/users/usuario-exemplo") // requisita (GET) os dados do usuário
      .then((response) => setUser(response.data)) // caso a solicitação retorne um valor, usa-o atualizando o valor de user
      .catch((err) => { // caso a solicitação retorne um erro, trata o erro conforme lógica definida
        console.error("ops! ocorreu um erro" + err); // imprime no console a mensagem de erro
      });
  }, []);

  return ( // retorna um elemento HTML do tipo div
    <div className="App">
      <p>Usuário: {user?.login}</p> <!-- elemento de parágrafo com o nome do usuário -->
      <p>Biografia: {user?.bio}</p> <!-- elemento de parágrafo com a biografia do usuário -->
    </div>
  );
}
```

A execução desse código deve renderizar uma página HTML com o nome de usuário (`login`) e a biografia (`bio`) retornados pela requisição.

-------------------------------------------------------------

# Conceitos de Domínio

## URL

- identificado por: Arlindo, Brian

URL é o acrônimo para _Uniform Resource Locator_, ou Localizador Uniforme de Recursos. Trata-se de um endereço único que é atribuído a cada um dos recursos disponíveis em uma rede, como a internet. A URL é utilizada para localizar e identificar um recurso na rede, sendo que cada um deles possui uma URL única.

### Propriedades de uma URL

Uma URL é composta por um *protocolo*, um nome de *domínio*, um *caminho*, um *recurso* e um *hash*. O **protocolo** é o método de comunicação utilizado para acessar o recurso, como o HTTP ou o FTP; o nome de **domínio** é o endereço do servidor onde o recurso está localizado, como o `google.com` ou o `facebook.com`; o **caminho** (ou _path_) é o local onde o recurso está localizado no servidor, como a pasta ou diretório. O **recurso** é o arquivo que está sendo acessado, como um documento HTML ou uma imagem. O **hash** é utilizado para acessar uma parte específica do recurso, como um elemento HTML ou um trecho de código.

URL também é compostas por *parâmetros*, que são utilizados para passar informações para o servidor. Os parâmetros são compostos por uma **chave** e um **valor**, sendo que cada parâmetro é separado por um caractere `&`. Por exemplo, na URL `https://www.google.com/search?q=javascript`, o único parâmetro é `q=javascript`, sendo que a chave é `q` e o valor é `javascript`.

### URL em JavaScript

A API URL do JavaScript é utilizada para manipular URLs. Ela fornece métodos estáticos e construtores para criar e gerenciar objetos URL, além de fornecer acesso a partes específicas de uma URL. No entanto, geralmente as URLs são construídas na mão, utilizando concatenação de *strings*.

#### Protocolo e domínio

O **protocolo** e o **domínio** são obtidos através das propriedades _protocol_ e _hostname_, respectivamente. Por exemplo, na URL `https://www.google.com/search?q=javascript`, a `base URL` (concatenção de _protocol_ + _hostname_) é `https://www.google.com`.

#### Caminho

Também conhecido como _path_, do inglês, representa o caminho do recurso, sendo que ele é composto por uma sequência de diretórios e arquivos separados por uma barra (`/`). O caminho é utilizado para identificar o recurso que está sendo acessado, sendo que ele é utilizado para acessar o arquivo no servidor. Por exemplo, na URL `https://www.google.com/search?q=javascript`, o caminho é `/search`.

#### Parâmetros

Para utilizar parâmetros em uma URL, às vezes os valores precisam ser codificados.

#### Encode/Decode

Para codificar parâmetros é utilizado o método `encodeURI` ou o método `encodeURIComponent`, enquanto que para decodificar os parâmetros é utilizado o método `decodeURI` ou o método `decodeURIComponent`.

### URL no Axios

Na API Axios, o URL pode ser manipulado através da propriedade `baseURL`, que é o endereço base do servidor, e da propriedade `url`, que seria o caminho, além da propriedade `params`; também é possível utilizar a propriedade `url` para passar o endereço completo do recurso, incluindo o *protocolo*, o *domínio* e o *caminho* do recurso.

-------------------------------------------------------------

## Requisições e respostas HTTP

- identificado por: Arlindo, Breno, Brian, Rafael

Como apresentado em seções anteriores, o protocolo HTTP permite a comunicação entre clientes e servidores de uma aplicação; para tal, ações são realizadas por ambas as partes, podendo elas serem classificadas como requisições (*requests*) ou respostas (*responses*) HTTP.

A principal característica que difere uma requisição de uma resposta é a sua direção de destino: enquanto nas requisições há o envio de mensagem do cliente para o servidor, nas respostas temos o oposto, com o servidor enviando uma mensagem ao cliente. Uma requisição busca o acesso a um recurso provido por um servidor, sendo o papel deste validar a solicitação e gerar uma resposta contendo o resultado almejado (ou não) da requisição, que então é enviado ao cliente.

```http
PATCH /objectserver/restapi/alerts/status HTTP/1.1
Accept: application/json
Authorization: Basic dGVzdHVzZXIwMTpuZXRjb29s
Content-Type: application/json
Host: localhost
Connection: keep-alive
Content-Length: 1092

{
	"rowset": {
		"coldesc": [
			{
				"type": "integer",
				"name": "Acknowledged"
			},
			{
				"type": "string",
				"name": "Location"
			},
			{
				"type": "integer",
				"name": "OwnerUID"
			},
			{
				"type": "integer",
				"name": "OwnerGID"
			},
			{
				"type": "utc",
				"name": "LastOccurrence"
			}
		],
		"rows": [
			{
				"Location": "UPDATED",
				"LastOccurrence": 1341412235,
				"Acknowledged": 1,
				"OwnerUID": 65534,
				"OwnerGID": 1
			}
		]
	}
}
```
<p style="text-align: center;">Requisição HTTP. Fonte: IBM</p>

Nas requisições, o cliente utiliza componentes de uma URL, que inclui as informações necessárias para o acesso de determinado recurso. Elas são compostas por três partes distintas que, juntas, determinam o recurso do servidor a ser acessado, sendo elas, a saber:

1. **A linha de requisição (_request line_)**, obrigatória, onde é apresentado o *método de requisição*, seu *destino* (uma URL ou caminho absoluto) e a *versão do protocolo HTTP* a ser utilizada. Os componentes são separados por um espaço em branco (" ");

```http
PATCH /objectserver/restapi/alerts/status HTTP/1.1
```
<p style="text-align: center;">Trecho da linha de requisição. Fonte: IBM</p>

2. **Headers HTTP**, definidos visando fornecer ao destinatário informações sobre a mensagem, seu remetente e a maneira pela qual este deseja se comunicar com o recebedor da requisição; e

```http
Accept: application/json
Authorization: Basic dGVzdHVzZXIwMTpuZXRjb29s
Content-Type: application/json
Host: localhost
Connection: keep-alive
Content-Length: 1092
                                                    // linha vazia
```
<p style="text-align: center;">Headers de uma requisição. Fonte: IBM</p>

3. **O corpo da mensagem (_body_)**, se necessário. Também conhecido como corpo da entidade (_entity body_), ele pode ser considerado conteúdo real da mensagem, ao mesmo tempo que é considerado inapropriado para alguns dos métodos de requisição, como o método `GET`. No trecho destacado abaixo, temos um corpo de mensagem formatado conforme o tipo definido no header `Content-Type`: `application/json`.

```http
{
	"rowset": {
		"coldesc": [
			{
				"type": "integer",
				"name": "Acknowledged"
			},
			{
				"type": "string",
				"name": "Location"
			},
			{
				"type": "integer",
				"name": "OwnerUID"
			},
			{
				"type": "integer",
				"name": "OwnerGID"
			},
			{
				"type": "utc",
				"name": "LastOccurrence"
			}
		],
		"rows": [
			{
				"Location": "UPDATED",
				"LastOccurrence": 1341412235,
				"Acknowledged": 1,
				"OwnerUID": 65534,
				"OwnerGID": 1
			}
		]
	}
}
```
<p style="text-align: center;">Corpo da mensagem de uma requisição. Fonte: IBM</p>

A estrutura das respostas HTTP se assemelha a das requisições, com a seguinte ressalva: substituímos a linha de requisição por uma **linha de status**, que guarda, respectivamente, a *versão do protocolo HTTP* utilizada, o *código status de resposta à requisição* (_response status code_) e a *frase de status* (_reason phrase_). Assim como na linha de requisição, os componentes são separados por um espaço em branco.

```http
HTTP/1.1 200 OK 					// linha de status
Cache-Control: no-cache
Server: libnhttpd
Date: Wed Jul 4 15:32:03 2012
Connection: Keep-Alive:
Content-Type: application/json;charset=UTF-8
Content-Length: 158

{
	"entry":	{
		"affectedRows": 10,
		"uri": "http://localhost/objectserver/restapi/alerts/status"
	}
}
```
<p style="text-align: center;">Resposta HTTP à requisição. Fonte: IBM</p>

### Métodos HTTP

Os métodos HTTP especificam a ação que o cliente deseja executar em um recurso do servidor. Os principais métodos HTTP são:

* **GET**: Solicita um recurso específico do servidor. É usado para recuperar informações.
* **POST**: Envia dados para o servidor para serem processados. É comumente usado para enviar formulários ou dados de envio.
* **PUT**: Envia dados para substituir um recurso existente no servidor. É usado para atualizar completamente um recurso.
* **PATCH**: Envia dados para atualizar parcialmente um recurso existente no servidor.
* **DELETE**: Remove um recurso específico do servidor.

-------------------------------------------------------------

## Status de resposta à requisição HTTP

- identificado por: Breno, Rafael

Respostas HTTP são feitas de servidor para cliente, sendo seu objetivo fornecer ao cliente o recurso solicitado, informar ao cliente que a ação solicitada foi realizada ou informar ao cliente que ocorreu um erro no processamento de sua solicitação.

### Status Code

Os códigos de resposta HTTP são utilizados para indicar o resultado de uma requisição. Eles fornecem informações sobre o status da requisição e orientam o cliente sobre o próximo passo a ser tomado. Alguns códigos de resposta comuns são:

* **1xx**: Informacional - A requisição foi recebida e o processo continua em andamento.
* **2xx**: Sucesso - A requisição foi processada com sucesso.
  * **200 OK**: A requisição foi bem-sucedida e o resultado foi retornado conforme solicitado.
  * **201 Created**: A requisição foi bem-sucedida e um novo recurso foi criado como resultado.
  * **204 No Content**: A requisição foi bem-sucedida, mas não há conteúdo para ser retornado.
* **3xx**: Redirecionamento - O cliente precisa realizar uma ação adicional para completar a requisição.
  * **301 Moved Permanently**: O recurso solicitado foi movido permanentemente para um novo local.
* **4xx**: Erro do cliente - A requisição contém erros ou não pode ser processada pelo servidor.
  * **400 Bad Request**: A requisição contém sintaxe inválida ou não pode ser processada pelo servidor.
  * **401 Unauthorized**: A autenticação é necessária para acessar o recurso.
  * **403 Forbidden**: O servidor entende a requisição, mas se recusa a autorizá-la.
  * **404 Not Found**: O recurso solicitado não foi encontrado no servidor.
* **5xx**: Erro do servidor - O servidor encontrou um erro ao processar a requisição.
  * 500 **Internal Server Error**: O servidor encontrou um erro interno ao processar a requisição.

-------------------------------------------------------------

## Interceptadores (funções de middleware)

- identificado por: Breno, Rafael

Antes que comecemos a falar sobre interceptadores devemos entender o que são os *middlewares*.

Quando empregado na engenharia de *software*, *middleware* pode ser entendido como uma camada intermediária entre dois aplicativos, agindo como um "tradutor oculto" e possibilitando a comunicação e o gerenciamento de dados para aplicativos distribuídos. Essa definição pode ser encontrada no **Dicionário da Computação na Nuvem** da Azure, Microsoft.

Agora, ao buscarmos entender o que seriam as **funções** de *middleware*, nos deparamos com uma aplicação do conceito orientada para a manipulação dos objetos de solicitação e de resposta a uma requisição; na Axios, onde elas são conhecidas como interceptadores (*interceptors*), esse tipo de função nos permite personalizar o comportamento de uma chamada HTTP ou reposta recebida após requisição realizada a outro servidor. Há a possibilidade de adicionarmos uma lógica personalizada à aplicação, como a criação de log ou validação da mensagem recebida ou a adição de *headers* HTTP à mensagem a ser enviada.

```javascript 
axios.interceptors.request.use((config) => { // manipulando o objeto de solicitação antes que seja realizada a requisição
  // ...
  // bloco para implementação da lógica do interceptador

  return config; // retorna o corpo da requisição modificado 
}, (erro) => {
  // ...
  // bloco para tratamento de erro, caso ocorra

  return Promise.reject(error); // rejeição da requisição por ocorrência de erro
});

axios.interceptors.response.use((response) => { // manipulando o objeto de resposta após seu recebimento
  // ...
  // bloco para implementação da lógica do interceptador

  return response;
}, (erro) => {
  // ...
  // bloco para tratamento de erro, caso ocorra

  return Promise.reject(error); // rejeição da requisição por ocorrência de erro
});
```

-------------------------------------------------------------

## JSON

- identificado por: Arlindo, Breno, Rafael

O termo JSON é o acrônimo para *JavaScript Object Notation*, sendo ele uma notação para objetos JavaScript. A notação é sintaticamente idêntica à utilizada para construção de um objeto JavaScript, mas um arquivo JSON em si não é um objeto e sim um formato de armazenamento de texto.

Em um arquivo JSON, assim como em JavaScript, objetos são delimitados por *chaves* e vetores são delimitados por *colchetes*. Os atributos de um objeto são definidos por um par de chave e valor separados por dois pontos, tendo-se tanto o nome da chave quanto o valor que ela armazena envolvidos por aspas **duplas** (tenha atenção a isso!).

```JSON
{
  "Atributo_que_Armazena_vetor_de_objetos": [ // início do vetor
    { // objeto 1
      "chave_1": "valor_1", // primeira propriedade do objeto 1
      "chave_2": "valor_2" // segunda propriedade do objeto 1
    },
    { // objeto 2
      "chave_1": "valor_1", // primeira propriedade do objeto 2
      "chave_2": "valor_2" // segunda propriedade do objeto 2
    },
    { // objeto 3
      "chave_1": "valor_1", // primeira propriedade do objeto 3
      "chave_2": "valor_2" // segunda propriedade do objeto 3
    }
  ] // fim do vetor
}
```

O formato JSON pode ser utilizado no envio de dados por requisições HTTP. Um arquivo JSON pode ser convertido para objetos em diferentes linguagens de programação com a utilização de *parsers*, mas a linguagem JavaScript possui a função própria `JSON.parse()` para converter um texto em formato JSON em um objeto JavaScript. Ao receber um arquivo JSON de uma requisição HTTP, basta se utilizar da função para criar um objeto JavaScript com as informações contidas no JSON:

```javascript
const obj = JSON.parse(response); // transforma a resposta recebida em objeto JavaScript
```

-------------------------------------------------------------

## XHR - XMLHttpRequest

- identificado por: Arlindo, Breno, Brian

O XMLHttpRequest é uma API JavaScript que permite a realização de requisições AJAX, e por consequência para receber requisições HTTP.

O termo AJAX é o acrônimo de *Asynchronous JavaScript & XML*, e é uma prática de programação que se utiliza do HTML, CSS, JavaScript, DOM e tanto os formatos XML e JSON para atualizar partes de uma página HTML com informações recebidas do servidor, ao invés de recarregá-la por inteiro, como ocorria antes do padrão ser criado. Com o padrão AJAX também se torna possível que a página WEB continue funcionando enquanto a parte que deve ser atualizada carrega para ser exibida de forma assíncrona.

Para utilizar a API XMLHttpRequest basta criar um objeto XMLHttpRequest, chamar o método `.open()` para definir uma URL e uma das requisições REST (`GET`, `POST`, `PUT`, `PATCH` ou `DELETE`), e enviar a requisição através do método `.send()`:

```javascript
const XHR =  new XMLHttpRequest(); // instancia um objeto da API XMLHttpRequest
req.open("GET", "http://www.ex.org/ex.txt", true); // define o método e a URL da requisição; um valor booleano, "true", é passado na chamada da função para indicar que a requisição é assíncrona
req.send(); // realiza o envio da requisição ao servidor
```

Após receber a resposta da requisição, armazenada no atributo `response` do objeto XMLHttpRequest, é possível, através da lógica do JavaScript, alterar um componente único da página Web que está sendo exibida.

```html
<!DOCTYPE html>

<html>

<body onload = "loadDoc()">
  <h1>The XMLHttpRequest Object</h1>
  <p id="demo"></p> <!-- elemento HTML que receberá resposta do servidor -->

  <script>
    function loadDoc() {
      const xhttp = new XMLHttpRequest(); // cria uma instância da API XMLHttpRequest

      xhttp.onreadystatechange = function() { // configura função que atualiza o documento HTML com os dados obtidos no servidor após recebimento de resposta à requisição HTTP
        if (this.readyState == 4 && this.status == 200) { // valida se a requisição teve êxito
          document.getElementById("demo").innerHTML = this.response; // atualiza o elemento HTML com o dado recebido do servidor
        }
      };

      xhttp.open("GET", "demo_get.asp", true); // define o método e a URL da requisição, além de que a função é assíncrona
      xhttp.send(); // envia requisição ao servidor
    }

    loadDoc(); // chama função que atualiza o documento HTML
  </script>
</body>

</html>
```

Ao carregar a página, o parágrafo com atributo `id="demo"` irá automaticamente receber um valor que será exibido. Esse é o funcionamento por trás do padrão AJAX, que tanto o Axios quanto o XMLHttpRequest implementam. Recebendo informações formatadas em XML ou JSON por requisições HTTP, é possível atualizar, de forma assíncrona em relação à página WEB como um todo, um único componente da página. Isso ocorre através da lógica do JavaScript, que é utilizada para alterar e controlar os elementos HTML que deverão ser exibidos em tela.

-------------------------------------------------------------

## Cliente-Servidor

- identificado por: Rafael

O modelo Cliente-Servidor define duas pontas de comunicação, onde se tem o lado Servidor, que apenas trata requisições, armazena e fornece dados, e o lado Cliente, que apenas requisita e consome os dados fornecidos pelo lado Servidor.

-------------------------------------------------------------

## Isomorfismo

- identificado por: Brian

Uma aplicação isomórfica pode rodar o mesmo código tanto do lado servidor quanto do lado Cliente. Isso permite que aplicações construídas com frameworks isomórficos possam, após serem carregadas no navegador WEB, realizar requisições direto à base de dados que realiza a persistência da aplicação, sem a necessidade de requisitar dados novamente ao servidor.

-------------------------------------------------------------

## XSRF

- identificado por: Brian

Ataques XSRF são ataques de *Cross-site Request Forgery* (Falsificação de Solicitação entre Sites, em tradução livre), e ocorrem através do envio de requisições HTTP maliciosas por um usuário autenticado e confiável pela aplicação, sem o conhecimento do próprio usuário de as estar enviando. Elas podem ser embutidas em e-mails, ou mesmo atributos de imagem HTML, de forma que apenas ao carregar uma página a requisição seja realizada, mesmo sem o usuário efetivamente clicar no link da requisição.

Ao enviar a requisição por um navegador WEB, o mesmo automaticamente irá carregar os *cookies* do usuário na requisição que o autentica na aplicação, e a requisição seria então atendida pelo lado do servidor.

Formas de evitar esse tipo de ataque são o *Same-Origin-Policy* (SOP), no qual um navegador apenas permite uma página WEB enviar requisições para outra apenas se ambas possuírem o mesmo domínio de origem: mesmo esquema de URI, mesmo número de porta e mesmo nome de *host*, tendo-se um controle de exceção para permitir apenas algumas requisições cruzadas entre domínios (o *Cross-origin Resource Sharing*); e ainda utilizar um *token* de autenticação como o XSRF-TOKEN no header de uma requisição para autenticá-la.

-------------------------------------------------------------

# Cenários de Uso

## Cenário: Realizar operações CRUD em um servidor

- identificado por: Arlindo, Breno, Brian, Leonardo

### Depende de:

- Conceitual
  - [URL](#url)
  - [Requisições e respostas HTTP](#requisições-e-respostas-http)
- Fatos de execução
  - Nenhum

O Axios permite que você faça operações CRUD em um servidor. Para isso, você pode utilizar os métodos `axios.get()`, `axios.post()`, `axios.put()` e `axios.delete()` e passar como parâmetro a URL do servidor e um objeto com os dados que você deseja enviar. 

### 1. Realizar o método GET em um servidor

```javascript
const res1 = await axios.get('/usuarios');
```

No código acima, `axios.get('/usuarios')` faz uma requisição `GET` para a rota `/usuarios`.

- ↪️ [A1. Realizar o método POST em um servidor](#a1-realizar-o-método-post-em-um-servidor)
- ↪️ [A2. Realizar o método PUT em um servidor](#a2-realizar-o-método-put-em-um-servidor)
- ↪️ [A3. Realizar o método DELETE em um servidor](#a3-realizar-o-método-delete-em-um-servidor)

### 2. Obter a resposta das operações

No passo anterior as requisições ficam guardadas em uma variável que aguarda o retorno do método. Após a promise ser cumprida é possivel acessar o corpo de uma response através do atributo `.data`.

Veja o exemplo abaixo:

```javascript
const res1 = await axios.get('/usuarios/1'); // realiza requisição de dados do usuário
const nomeDeUsuario = res.data.username; // atribui a constante nomeDeUsuario o valor de username, propriedade retornada na resposta à requisição
```

No exemplo acima, `axios.get('/usuarios/1')` faz uma requisição `GET` para a rota `/usuarios/1`, e a resposta é armazenada na variável `res1`; em seguida, o nome de usuário é acessado através de `res1.data.username`.

Lembrando que é importante envolver o código em um bloco `try/catch` para tratar erros e evitar que o programa quebre. Veja o cenário [Tratar erros em respostas à requisições](#cenário-tratar-erros-em-respostas-à-requisições) para mais informações.

Outra forma de ser feito sem precisar de uma função assíncrona é utilizando o método `.then()`.

```javascript
axios.get('/usuarios/1') // também pode ser axios.post(), axios.put() ou axios.delete()
  .then((res) => { // recebe e realiza uso da resposta à requisição
    setNomeUsuario(res.data.username); // atualiza o valor do nome do usuário
  }) /*
  .catch((error) => { // você pode usar .catch() ao invés de try/catch para tratar erros
    ...
  });
  */
```

### A1. Realizar o método POST em um servidor

#### A1.1 Definindo os dados que serão enviados

```javascript
const dados = { // define o objeto a ser enviado como corpo da requisição
  nome: 'José',
  idade: 31 
};
```

#### A1.2 Realizando o método POST

```javascript
const res2 = await axios.post('/usuarios', dados); // realiza requisição HTTP do tipo POST e envia dados definidos
```

No exemplo, `axios.post('/usuarios')` faz uma requisição `POST` para a rota `/usuarios` com os dados do usuário a serem criados.

- [↩️ 2. Obter a resposta das operações](#2-obter-a-resposta-das-operações)

### A2. Realizar o método PUT em um servidor

#### A2.1 Definindo os dados que serão enviados

```javascript
const dados = { // define o objeto a ser enviado como corpo da requisição
  idade: 35 // adiciona/atualiza o valor de idade de um registro
};
```

#### A2.2 Realizando o método PUT

```javascript
const res3 = await axios.put('/usuarios/1', dados); // realiza requisição HTTP do tipo PUT e envia dados para atualização de registro
```

No exemplo, `axios.put('/usuarios/1')` faz uma requisição `PUT` para a rota `/usuarios/1` para atualizar os dados de um registro.

- [↩️ 2. Obter a resposta das operações](#2-obter-a-resposta-das-operações)

### A3. Realizar o método DELETE em um servidor

#### A3.1 Realizando o método DELETE

```javascript
const res4 = await axios.delete('/usuarios/1'); // realiza requisição HTTP do tipo DELETE
```

No exemplo, `axios.delete('/usuarios/1')` faz uma requisição `DELETE` para a rota `/usuarios/1` para deletar registro de usuário.

- [↩️ 2. Obter a resposta das operações](#2-obter-a-resposta-das-operações)

-------------------------------------------------------------

## Cenário: Abortar uma requisição

- identificado por: Breno

### Depende de:

- Conceitual
  - [Cliente-Servidor](#cliente-servidor)
  - [URL](#url)
  - [Requisições e respostas HTTP](#requisições-e-respostas-http)
- Fatos de execução
  - Nenhum

### 1. Crie um AbortController

A partir da versão v0.22.0, o Axios passou a oferecer suporte ao `AbortController` para cancelar requisições em forma de API fetch.

```javascript
const controller = new AbortController(); // cria instancia do controllador de cancelamento de requisição
```

### 2. Realize a requisição

Realize sua requisição passando um objeto `signal` que referencia `AbortController.signal`. Dessa forma, associamos `signal` e `controller` com essa requisição, e isso nos permite abortar o processo através do método `AbortController.abort()`.

```javascript
axios.get('/caminho-da-requisicao', { // realiza requisição HTTP do tipo GET
  signal: controller.signal // define instância que monitora requisição
}).then(function(response) {
  // aqui fica a lógica para manipulação da resposta à requisição
});
```

### 3. Aborte a requisição

Chame o `AbortController.abort()` através do nosso `controller` para cancelar a requisição.

```javascript
controller.abort(); // cancela a requisição
```

-------------------------------------------------------------

## Cenário: Repetição de requisição com interceptadores

- identificado por: Arlindo, Breno, Brian

### Depende de:

- Conceitual
  - [Requisições e respostas HTTP](#requisições-e-respostas-http)
  - [Status de resposta à requisição HTTP](#status-de-resposta-à-requisição-http)
  - [Interceptadores (funções de *middleware*)](#interceptadores-funções-de-middleware)
- Fatos de execução
  - Nenhum

### 0. Contexto

Em certas situações, pode ser necessário lidar com erros de rede ou outras condições que exijam a repetição de uma chamada de API. O Axios fornece uma funcionalidade chamada **Interceptors**, que nos permite interceptar e modificar as requisições e respostas antes de serem tratadas pela aplicação.

Vamos ver um exemplo de como utilizar os _Axios Interceptors_ para repetir uma chamada de API automaticamente em caso de erro.

### 1. Importe a biblioteca Axios ao arquivo do projeto

```javascript
import axios from "axios"; // importa a biblioteca no arquivo atual
```

### 2. Defina a lógica do interceptador

```javascript
import axios from "axios";

// define a lógica para interceptação de requisição
axios.interceptors.response.use((response) => {
  // adicione a lógica do interceptador aqui

  return response; // retorna corpo da requisição após sua manipulação
});
```

No código acima, `axios.interceptors.response.use()` é utilizado para adicionarmos a lógica de interceptação de resposta à requisição; perceba que uma função de *callback* é passada como parâmetro, e que essa função tem como parâmetro `response`, que guarda o corpo da resposta. Com interceptadores, podemos manipular os dados recebidos, validar O seu formato, etc.

- ↪️ [A1. Realizar interceptação de requisição à servidor](#a1-realizar-interceptação-de-requisição-à-servidor)

### 3. Defina como os erros serão tratados (opcional) e a repetição de requisição

```javascript
import axios from "axios";

axios.interceptors.response.use((response) => {
  // ...

  return response;
}, (error) => {
  // adicione a lógica de tratamento de erro aqui

  if (error.response && error.response.status === 503) { // exemplo de repetição da chamada em caso de erro 503 (Service Unavailable)
    return axios(error.config);
  }

  return Promise.reject(error); // cancela envio da requisição e retorna erro (promise reject)
});
```

No código acima temos a definição da lógica para tratamento de erros de uma requisição; o block `if`, com as condicionais `error.response` e `error.response.status === 503`, detecta quando há ocorrência de `Service Unavailable`, quando há um problema no servidor da API que impede o êxito da requisição. Caso não seja detectado tal erro, a requisição é rejeitada.

### 4. Realize a requisição

```javascript
import axios from "axios";

axios.interceptors.response.use((response) => {
  // ...

  return response;
}, (error) => {
  // ...

  if (error.response && error.response.status === 503) {
    return axios(error.config);
  }

  return Promise.reject(error);
});

axios.get('https://api.example.com/data') // realiza requisição de dados à API
  .then((response) => {
    console.log('Dados recebidos:', response.data); // imprime dados recebidos caso a requisição seja resolvida
  })
  .catch((error) => {
    console.error('Erro na chamada de API:', error); // imprime mensagem de erro, caso ocorra
  });
```

### A1. Realizar interceptação de requisição à servidor

### A1.1 Defina a lógica do interceptador

```javascript
import axios from "axios";

// define a lógica para interceptação de respostas
axios.interceptors.request.use((config) => {
  // adicione a lógica do interceptador aqui

  return config;
});
```

### A1.2 Defina como os erros serão tratados

```javascript
import axios from "axios";

axios.interceptors.request.use((config) => {
  // ...

  return config;
}, (error) => {
  // adicione a lógica de tratamento de erro aqui

  return Promise.reject(error); // cancela envio da requisição e retorna erro (promise reject)
});
```

- [↩️ 4. Realize a requisição](#4-realize-a-requisição)

No interceptor de requisições, podemos modificar a configuração da requisição antes que ela seja enviada. No interceptor de respostas, podemos modificar a resposta antes que ela seja tratada pela aplicação.

-------------------------------------------------------------

## Cenário: Implementar um indicador de progresso

- identificado por: Brian

### Depende de:

- Conceitual
  - [Cliente-Servidor](#cliente-servidor)
  - [URL](#url)
  - [Requisições e respostas HTTP](#requisições-e-respostas-http)
- Fatos de execução
  - Nenhum

### 0. Contexto

Imagine que você tem um app front end que faz upload de arquivos para um servidor. Você quer acompanhar e mostrar ao usuário o progresso do envio desses arquivos.

Isso é possível e fácil de implementar utilizando o Axios.

Vamos como codar essa funcionalidade.

### 1. Crie um FormData para o arquivo

Crie o seu objeto FormData que será enviado na requisição.

O objeto FormData, fornece uma maneira fácil de construir um conjunto de pares chave/valor representando campos de um elemento form e seus valores.

```javascript
import axios from axios; // importa a biblioteca Axios

const data = new FormData(); // intancia um objeto FormData, que armazena a imagem (arquivo)

data.append("file", file, filename); // adiciona o arquivo
```

### 2. Crie a solicitação

Ao realizar sua solicitação, passe seu arquivo que está contido em `data` e passe também um objeto com a configuração da requisição. Nele contém o método `onUploadProgress` que recebe um `event`. 

Esse `event` contém as propriedades `loaded` e `total`; `loaded` é o quanto já foi carregado e `total`, o tamanho total do arquivo.

Então é feito um cálculo de regra de três para definir a percentagem que já foi carregada no servidor. A variável `progress` armazena esses valores conforme a função `onUploadProgress` é executada automaticamente.

```javascript
axios.post("https://my.server.com/posts", data, { // realiza a requisição para o servidor com método POST e envia o arquivo que está na variável data
  onUploadProgress: (event) => {
    let progress: number = Math.round(
      (event.loaded * 100) / event.total
    );

      console.log(`A imagem ${filename} está ${progress}% carregada...`);
  },
  } // passa um objeto de configuração que possui um método onUploadProgress
);
```

### 3. Trate a resposta

Por fim, podemos chamar o `.then` e o `.catch` para tratar as possiveis conclusões da requisição.

```javascript
axios.post("https://my.server.com/posts", data, {
  onUploadProgress: (event) => {
    let progress: number = Math.round(
      (event.loaded * 100) / event.total
    );

    console.log(`A imagem ${filename} está ${progress}% carregada...`);
  },
}).then((response) => { // caso a requisição possua resposta válida
  // adicione a lógica de manipulação dos dados da resposta

  console.log(
    `A imagem ${filename} já foi enviada para o servidor!`
  );
}).catch((err) => { // caso ocorra um erro
  // adicione a lógica de tratamento

  console.error(
    `Houve um problema ao realizar o upload da imagem ${filename} no servidor AWS:`, err 
  );
});
```

-------------------------------------------------------------

## Cenário: Manipular *headers* de uma requisição

- identificado por: Arlindo

### Depende de:

- Conceitual
  - [URL](#url)
  - [Requisições e respostas HTTP](#requisições-e-respostas-http)
  - [Interceptadores (funções de *middleware*)](#interceptadores-funções-de-middleware)
- Fatos de execução
  - Nenhum

Axios permite que você intercepte requests e responses, o que pode ser útil para anexar um header de autorização em todas as requests. Para isso, você pode utilizar o método `axios.interceptors.request.use()` e passar como parâmetro uma função que recebe como parâmetro a configuração da request e retorna a configuração da request ou um erro.

Veja um exemplo abaixo

### 1. Utilize axios.interceptors

```javascript
axios.interceptors.request.use((config) => {
  // adicione a lógica de manipulação do corpo de requisição, se necessário

  return config;
});
```

No exemplo acima, `config` captura em todas as requisições dessa instância o parametro `config` quando se realiza uma requisição: `axios.get(url[, ***config***])`.

- ↪️ [A1. Utilize axios.create()](#A1-Utilize-axioscreate)

### 2. Faça uma alteração no objeto config.headers

```javascript
axios.interceptors.request.use((config) => {
  // ...

  // caso a configuração da request não possua um header de autorização anexa um header de autorização com o token do usuário
  if (!config.headers['Authorization']) {
    config.headers['Authorization'] = `Bearer ${token}`
  }

  return config;
});
```

Esse exemplo intercepta a configuração de uma requisição e verifica se a configuração possui um header de autorização. Caso não possua, anexa um header de autorização com o token do usuário. O que pode ser útil para garantir que todas as requests possuam um header de autorização.

### 3. Realize uma requisição em uma rota protegida

```javascript
const res = await api.get('/rota-protegida'); // realiza requisição HTTP do tipo GET
```

No exemplo acima, já não é mais necessário passar um *token*, pois o Axios já fará isso automaticamente.

Sem manipular a *header* todas as requisições deveriam ser realizadas passando um objeto `config`:

```javascript
const config = {
  headers: {
    Authorization: `Bearer ${token}` // adiciona valor do header Authorization, passando o seu token de acesso
  }
};

const dados = {
  nome: 'José',
  idade: 31
}; // dados a serem enviados na requisição

const res = await api.post('/rota-protegida', dados, config); // realiza a requisição
```

### A1. Utilize axios.create()

Outra abordagem é inicializando uma instância do axios método `axios.create()` e passar como parâmetro um objeto com as configurações que você deseja personalizar, o que também pode ser usado para anexar um *header* de autorização em todas as requisições.

Veja um exemplo abaixo

#### A1.1 Crie a instânia Axios

```javascript
// configura a instância Axios a ser utilizada nas operações
const api = axios.create({
  headers: {
    'Authorization': `Bearer ${token}` // adiciona o header Authorization, já valorado, à configuração padrão de todas as requisições
  }
});
```

Esse exemplo cria uma instância do Axios com um header de autorização anexado. O que pode ser útil para garantir que todas as requisições possuam um *header* de autorização.

- ↩️ [3. Realize uma requisição em uma rota protegida](#3-Realize-uma-requisição-em-uma-rota-protegida)

-------------------------------------------------------------

## Cenário: Tratar erros em respostas à requisições

- identificado por: Arlindo, Breno, Leonardo

### Depende de:

- Conceitual
  - [URL](#url)
  - [Requisições e respostas HTTP](#requisições-e-respostas-http)
  - [Interceptadores (funções de *middleware*)](#interceptadores-funções-de-middleware)
- Fatos de execução
  - Nenhum

O Axios possui mecanismos para lidar com diferentes tipos de erros retornados pelo servidor.

Quando uma requisição retorna um status de erro, o Axios irá lançar uma exceção. Essa exceção pode ser capturada usando um bloco `try/catch` ou através do método `catch` no final da cadeia de promessas.

Veja abaixo um exemplo de tratamento de erro usando `try/catch`:

### 1. Utilize blocos try/catch

```javascript
try {
  // lógica da operação deve ser adicionada neste bloco

} catch (err) {
  // caso qualquer erro ocorra no bloco anterior, o processo será redirecionado para este, onde deve ser adicionada a lógica de tratamento

}
```

No exemplo acima, caso ocorra um erro, a exceção é capturada no bloco catch e você pode tratar o erro de acordo com suas necessidades, caso contrário tudo o correu bem.

- ↪️ [A1. Utilize o método .then().catch()](#a1-utilize-o-método-thencatch)
- ↪️ [A2. Manipular mensagens de erro em todas as responses](#a2-manipular-mensagens-de-erro-em-todas-as-responses)

### 2. Implemente o código caso a requisição seja bem sucedida

```javascript
try {
  const res = await axios.get('/usuarios/1'); // realiza a requisição

  setNomeUsuario(res.data.username); // atualiza o valor do nome de usuário
} catch (err) {
  // ...

}
```

A requisição `GET` é feita para obter os dados do usuário com `id=1`. Se a requisição for bem-sucedida, o nome de usuário é acessado através de `res.data.username`.

### 3. Trate o erro

```javascript
try {
  const res = await axios.get('/usuarios/1');

  setNomeUsuario(res.data.username);
} catch (err) {
  if (err.status === 500) {
    setError(500, 'Erro no servidor, tente novamente mais tarde'); // imprime uma mensagem de erro no console
  }
} 
```

No exemplo acima, é capturado um erro esperado no servidor e tratado em outra parte do código, como por exemplo setando dinamicamente uma variável `erro` no front-end.

### A1. Utilize o método .then().catch()

Outra abordagem de tratar o erro é usando o método `catch`

#### A1.1 Utilize o método .then().catch()

```javascript
axios.get('/usuarios/1')
  .then((res) => {
    // lógica da operação
  })
  .catch((err) => {
    // tratamento de erro
  });
```

Nesse exemplo, o método `then` é chamado quando a requisição é bem sucedida, caso contrario o `catch` é chamado, onde você pode realizar as operações desejadas, como exibir uma mensagem de erro ou registrar o erro em um sistema de monitoramento.

- ↩️ [2. Implemente o código caso a requisição seja bem sucedida](#2-implemente-o-código-caso-a-requisição-seja-bem-sucedida)

### A2. Manipular mensagens de erro em todas as responses

Axios permite que você intercepte requisições e respostas, o que pode ser útil para manipular mensagens de erro numa resposta. Para isso, você pode utilizar o método `axios.interceptors.response.use()` e passar como parâmetro uma função que recebe como parâmetro a resposta e retorna a resposta alterada ou um erro.

Veja um exemplo abaixo:

#### A2.1. Utilize axios.interceptors

```javascript
axios.interceptors.response.use((response) => {
  // ...

  return response; // retorna a resposta recebida, caso nenhum erro seja encontrado
}, (error) => {
  // realiza o tratamento de erro, caso ocorra
});
```

#### A2.2. Manipule os erros

```javascript
axios.interceptors.response.use((response) => {
  return response;
}, (error) => {
  const { status } = error.response; // armazena o status da requisição
  const { message: defaultMessage } = error.response.data; // armazena a mensagem de erro recebida, se houver
  let message; // define a variável que guardará a mensagem a ser mostrada no console

  switch (status) { // identifica tipo de erro ocorrido e registra mensagem apropriada para impressão no console
    case 400: // status 400
      message = defaultMessage || 'Dados inválidos.';
      break;
    case 401: // status 401
      message = defaultMessage || 'Você não possui permissão para acessar esse recurso.'
      break;
    case 404: // status 404
      message = defaultMessage || 'Recurso não encontrado.';
      break;
    
    default: // status não listado
      message = 'Ocorreu um erro na requisição.'
      break;
  }

  console.log(message); // imprime mensagem de erro no console
});
```

Esse exemplo intercepta a resposta de uma requisição e verifica o status de resposta mal sucedida. Se o status for `400`, `401` ou `404`, uma mensagem de erro é lançada; caso contrário, uma mensagem genérica é lançada. O que pode ser útil para exibir mensagens de erro no front-end de forma mais amigável ao usuário.

- ↪️ [A3. Capturar todos os erros para salvar num arquivo de log ou enviar para um serviço de monitoramento de erros](#a3-capturar-todos-os-erros-para-salvar-num-arquivo-de-log-ou-enviar-para-um-serviço-de-monitoramento-de-erros)

### A3. Capturar todos os erros para salvar num arquivo de log ou enviar para um serviço de monitoramento de erros

Para capturar erros, você pode utilizar o método `axios.interceptors.response.use()` e passar como parâmetro uma função que recebe como parâmetro um objeto com a resposta da requisição.

Veja o exemplo abaixo:

#### A3.1 Envie o erro para um serviço de monitoramento de erros

```javascript
axios.interceptors.response.use((response) => {
  return response;
}, (error) => {
  // adicione uma lógica de tratamento ou envie o objeto para um serviço de monitoramento de erros, como o Sentry
  Sentry.captureException(error);

  // propaga o erro para que seja tratado em outro ponto do código
  throw error;
});
```

O exemplo acima captura qualquer tipo de erro e envia para uma instância do serviço **Sentry.io**.

-------------------------------------------------------------

## Cenário: Realizar _upload_ de arquivos

- identificado por: Arlindo

### Depende de:

- Conceitual
  - [Requisições e respostas HTTP](#requisições-e-respostas-http)
- Fatos de execução
  - Nenhum

Para lidar com arquivos por meio do Axios, é necessário utilizar bibliotecas de formulário, enviando o arquivo pela requisição HTTP dentro de um objeto formulário, e para isso se utiliza da biblioteca `FormData`. Criando uma instância do `FormData`, se adiciona o arquivo no formulário pelo método `.append(nome_do_campo,valor, nome_do_arquivo)`:

### 1. Utilize a biblioteca FormData

```javascript
const FormData = require('form-data'); // importa a biblioteca FormData

const form = new FormData(); // cria uma nova instancia de FormData

form.append('CampoImagem', arquivo, 'imagem.jpg'); // arquivo pode ser um Blob ou Stream, e o terceiro argumento, o nome do arquivo, é essencial
```

No trecho acima, é criada uma instância do `FormData` e um arquivo é adicionado ao formulário através do método `.append`. O arquivo pode ser um Blob ou Stream, e o terceiro argumento é o nome do arquivo.

### 2. Envie o formulário com o Axios

```javascript
const formData = new FormData();
const imagefile = fs.readFile("./imagem/imagem.jpg"); // realiza a leitura do arquivo .jpg

formData.append("fileimage", arquivo, imagem.jpg);

// realiza requisição HTTP do tipo POST
axios.post("http://localhost:xxx/imagem-upload", formData, {
  headers: {
    "Content-Type": `multipart/form-data;boundary=${formData._boundary}`,
  }
}).then((response) => console.log(response));
```

No código anterior é criada uma instância do `FormData` e o arquivo é adicionado ao formulário. Em seguida, é feita uma requisição HTTP do tipo POST utilizando o Axios, onde o formulário é passado como corpo da requisição. É importante definir o header `Content-Type` como `multipart/form-data` e o boundary adequado. A resposta da requisição é então exibida no console.

### 3. Trate o arquivo no servidor

```javascript
const multer = require("multer"); // importa a biblioteca multer

let upload = multer(); // cria uma instância da biblioteca multer para utilização nas operações seguintes

app.post("/imagem-upload", upload.single('fileimage'), (req, res) => {
  console.log(req.body);

  res.status(200).json({ message: "success" });
});
```

Já no trecho acima é utilizado o `multer` no servidor para tratar o arquivo recebido. O campo `fileimage` é definido como a chave que o `multer` irá procurar pelo arquivo de imagem. O corpo da requisição é impresso no console e é enviada uma resposta com status 200 e mensagem de sucesso.

### 4. Upload de arquivo na forma de um Stream

```javascript
const fileStream = fs.createReadStream('./arquivo.zip'); // cria um objeto stream do arquivo a ser enviado, que possui nome 'arquivo.zip'

const form = new FormData(); // importa a biblioteca FormData

form.append('largeFile', fileStream, 'large-file.zip'); // passa o objeto stream direto ao formulário
```

Por fim, é criado um objeto stream a partir do arquivo a ser enviado e é criada uma instância do `FormData`. O objeto stream é passado diretamente para o formulário utilizando o método `.append`.

Dessa forma, é possível realizar o upload.

-------------------------------------------------------------

## Cenário: Autenticação de usuário

- identificado por: Arlindo

### Depende de:

- Conceitual
  - [Requisições e respostas HTTP](#requisições-e-respostas-http)
- Fatos de execução
  - Nenhum

Para realizar a autenticação em um servidor utilizando o framework Axios, existem algumas opções disponíveis.

### 1. Autenticação básica com o parâmetro `auth`

```javascript
const user = await axios.post(BASE_URL+'/users', data, {
  auth: {
    username: USERNAME,
    password: PASSWORD
  }
}); // realiza a requisição HTTP com autenticação básica
```

No exemplo acima, o parâmetro `auth` é passado na requisição para realizar uma autenticação básica (*Basic Auth*). As informações de autenticação (nome de usuário e senha) são automaticamente codificadas em `base64` pelo Axios antes da requisição ser enviada.

### 2. Autenticação manual com token codificado em `base64`

```javascript
const token = `${USERNAME}:${PASSWORD}`; // define o token
const encodedToken = Buffer.from(token).toString('base64'); // codifica o token para base64
const headers = { 'Authorization': 'Basic ' + encodedToken }; // define os headers da requisição

const user = await axios.post(BASE_URL+'/users', data, { headers }); // realiza a requisição HTTP com o token de autenticação manual
```

No exemplo acima, é possível realizar a autenticação manualmente criando o `authToken` (um token de acesso que combina o nome de usuário e senha) e codificando-o em `base64`. Em seguida, o token codificado é passado como parte dos headers da requisição, especificamente no campo `Authorization` com o valor `'Basic ' + encodedToken`.
### 3. Autenticação com Bearer Token
A autenticação com um Bearer token só pode ser realizada no Axios por meio da inserção do token de autenticação nos `headers` de uma requisição:

```javascript
const headers =  { headers: { Authorization: `Bearer ${token}`} };

axios.get('https://api.exemplo.com/endpoint',headers)
  .then(response => {
    // Manipula os dados da resposta
    console.log(response.data);
  })
  .catch(error => {
    // Lida com erros de solicitação
    console.error(error);
  });
```
### 4. Configurar um `authToken` nas configurações globais do Axios

```javascript
axios.defaults.headers.common['Authorization'] = AUTH_TOKEN;
```

No trecho anterior é possível configurar um `authToken` nas configurações globais do Axios, definindo-o como valor do header `'Authorization'`. Dessa forma, o `authToken` será incluído automaticamente em todas as requisições feitas pelo Axios.

Essas são algumas opções para realizar a autenticação de usuário utilizando o Axios. A escolha da abordagem depende das necessidades específicas do sistema e das preferências de implementação.

-------------------------------------------------------------

## Cenário: Configurar um `timeout` para uma requisição

- identificado por: Brian

### Depende de:

- Conceitual
  - [Requisições e respostas HTTP](#requisições-e-respostas-http)
- Fatos de execução
  - Nenhum

O parâmetro `timeout` define um tempo limite para uma requisição ficar sem resposta antes de abortá-la.

### 1. Definindo um tempo limite padrão

```javascript
const api = axios.create(); // cria uma instancia pelo "config defaults" do framework Axios; na criação o timeout é 0 por padrão

api.defaults.timeout = 2500; // sobrescreve o timeout padrão de forma que as requisições usando a instância esperarão 2.5 segundos antes de serem abortadas
```

No trecho de código anterior, é criada uma instância do Axios utilizando o método `create()`. Por padrão, o timeout é definido como 0. É possível sobrescrever o timeout padrão para que as requisições que utilizem essa instância esperem 2.5 segundos antes de serem abortadas.

### 2. Sobrescrevendo o timeout em uma requisição específica

```javascript
api.get('/longRequest', {
  timeout: 5000
}); // sobrescreve o timeout padrão para esta requisição específica
```

No exemplo acima, o timeout padrão é sobrescrito para uma requisição específica. Nesse caso, a requisição do tipo GET para a rota '/longRequest' terá um timeout de 5 segundos.

### 3. Utilizando `timeout()` em conjunto com `abortSignal`

```javascript
api.get('/foo/bar', {
  signal: AbortSignal.timeout(5000)
}).then(function(response) {
  // adiciona a lógica de manipulação da resposta recebida à requisição
});
```

Por último, o método `timeout()` é utilizado em conjunto com o `abortSignal`. Ao configurar `signal` como `AbortSignal.timeout(5000)`, a requisição GET para '/foo/bar' será abortada após 5 segundos de espera. É possível adicionar a lógica de manipulação da resposta dentro do bloco `then` para lidar com a resposta recebida.

Dessa forma, é possível configurar um tempo limite para uma requisição utilizando o Axios.

-------------------------------------------------------------

# Fatos de execução

## Referência

### axios.create

#### Depende de:
- Conceitual
  - Nenhum
- Cenários de uso
  - Nenhum

`Axios.create` é um recurso útil dentro do Axios usado para criar uma nova instância com uma configuração personalizada. Com `axios.create`, podemos gerar um cliente para qualquer API e reutilizar a configuração para qualquer chamada usando o mesmo cliente.

```javascript
const Cliente = axios.create({
  baseURL: 'https://seu.Dominio.com/',
  timeout: 1000,
  headers: {
    'Accept': 'application/json',
    'Authorization': 'token <seu-token-aqui> // opcional
  }
});
```

-------------------------------------------------------------

### axios.config

#### Depende de:
- Conceitual
  - [URL](#url)
  - [Requisições e respostas HTTP](#requisições-e-respostas-http)
- Cenários de uso
  - Nenhum

Estas são as opções de configuração disponíveis para fazer solicitações. Apenas a `url` é necessária. As solicitações serão padronizadas para `GET` se o `method` não for especificado.

URL e Method:

```javascript
{
  // `url` é a URL do servidor que será usada para a requisição
  url: '/user',

  // `method` é o método de requisição a ser usado ao fazer a requisição
  method: 'get', // default
}
```
Outras configurações que também podem ser especificadas são:

BaseURL:

```javascript
  // `baseURL` será anexado a `url` a menos que `url` seja absoluto.
  // Pode ser conveniente definir `baseURL` para uma instância de axios para passar URLs relativos
  // para métodos dessa instância.
  baseURL: 'https://some-domain.com/api'
```

TransformRequest:

```javascript
  // `transformRequest` permite alterações nos dados da solicitação antes de serem enviados ao servidor
  // Isso é aplicável apenas para os métodos de solicitação 'PUT', 'POST', 'PATCH' e 'DELETE'
  // A última função do array deve retornar uma string ou uma instância de Buffer, ArrayBuffer,
  // FormData ou Stream
  // Você pode modificar o objeto headers.
  transformRequest: [function (data, headers) {
    // Faça o que quiser para transformar os dados

    return data;
  }]
```

TransformResponse:

```javascript
  // `transformResponse` permite que alterações nos dados de resposta sejam feitas antes
  // é passado para then/catch
  transformResponse: [function (data) {
    // Faça o que quiser para transformar os dados

    return data;
  }]
```

Headers:

```javascript
  // `headers` são cabeçalhos personalizados a serem enviados
  headers: {'X-Requested-With': 'XMLHttpRequest'}
```

Params:

```javascript
  // `params` são os parâmetros de URL a serem enviados com a requisição
  // Deve ser um objeto simples ou um objeto URLSearchParams
  // NOTA: parâmetros nulos ou indefinidos não são renderizados na URL.
  params: {
    ID: 12345
  }
```

ParamsSerializer:

```javascript
  // `paramsSerializer` é uma função opcional encarregada de serializar os parâmetros `params` 
  // (por exemplo, https://www.npmjs.com/package/qs, http://api.jquery.com/jquery.param/)
  paramsSerializer: function (params) {
    return Qs.stringify(params, {arrayFormat: 'brackets'})
  }
```

Data:

```javascript
  // `data` são os dados a serem enviados como o corpo da solicitação
  // Aplicável apenas para métodos de solicitação 'PUT', 'POST', 'DELETE' e 'PATCH'
  // Quando nenhum `transformRequest` é definido, deve ser de um dos seguintes tipos:
  // - string, objeto simples, ArrayBuffer, ArrayBufferView, URLSearchParams
  // - Somente navegador: FormData, File, Blob
  // - Somente Node: Stream, Buffer
  data: {
    firstName: 'Fred'
  },
  
  // alternativa de sintaxe para enviar dados para o corpo
  // postagem do método
  // apenas o valor é enviado, a chave não
  data: 'Country=Brasil&City=Belo Horizonte'
```

Timeout:

```javascript
  // `timeout` especifica o número de milissegundos antes do tempo limite da solicitação.
  // Se a requisição demorar mais que `timeout`, a requisição será abortada.
  timeout: 1000 // padrão é `0` (sem timeout)
```

WithCredentials:

```javascript
  // `withCredentials` indica se há ou não solicitações de controle de acesso entre sites
  // deve ser feito usando credenciais
  withCredentials: false // padrão
```

Adapter:

```javascript
  // `adapter` permite manipulação customizada de requisições o que torna o teste mais fácil.
  // Retorna uma promessa e fornece uma resposta válida 
  adapter: function (config) {
    /* ... */
  }
```

Auth:

```javascript
  // `auth` indica que a autenticação básica HTTP deve ser usada e fornece credenciais.
  // Isso definirá um cabeçalho `Authorization`, sobrescrevendo qualquer existente
  // Cabeçalhos customizados `Authorization` que você definiu usando `headers`.
  // Observe que apenas a autenticação HTTP Basic é configurável por meio desse parâmetro.
  // Para tokens de portador e outros, use cabeçalhos personalizados `Authorization`.
  auth: {
    username: 'jorge',
    password: 'amongus'
  }
```

ResponseType:

```javascript
  // `responseType` indica o tipo de dado com o qual o servidor responderá
  // as opções são: 'arraybuffer', 'document', 'json', 'text', 'stream'
  // somente navegador: 'blob'
  responseType: 'json' // padrão
```

ResponseEncoding:

```javascript
  // `responseEncoding` indica a codificação a ser usada para decodificar respostas (somente Node.js)
  // Nota: Ignorado para `responseType` de 'stream' ou solicitações do lado do cliente
  responseEncoding: 'utf8' // padrão
```

xsrfCookieName:

```javascript
  // `xsrfCookieName` é o nome do cookie a ser usado como um valor para o token xsrf
  xsrfCookieName: 'XSRF-TOKEN' // padrão
```

xsrfHeaderName:

```javascript
  // `xsrfHeaderName` é o nome do cabeçalho http que carrega o valor do token xsrf
  xsrfHeaderName: 'X-XSRF-TOKEN' // padrão
```

OnUpLoadProgress:

```javascript
  // `onUploadProgress` permite a manipulação de eventos de progresso para uploads
  // somente navegador
  onUploadProgress: function (progressEvent) {
    // Faça o que quiser com o evento de progresso nativo
  }
```

OnDownloadProgress:

```javascript
  // `onDownloadProgress` permite a manipulação de eventos de progresso para downloads
  // somente navegador
  onDownloadProgress: function (progressEvent) {
    // Faça o que quiser com o evento de progresso nativo
  }
```

MaxContentLength:

```javascript
  // `maxContentLength` define o tamanho máximo do conteúdo da resposta http em bytes permitido em node.js
  maxContentLength: 2000
```

MaxBodyLength:

```javascript
  // `maxBodyLength` (opção apenas de nó) define o tamanho máximo do conteúdo da solicitação http em bytes permitidos
  maxBodyLength: 2000
```

ValidateStatus:

```javascript
  // `validateStatus` define se deve resolver ou rejeitar a promessa para um determinado
  // Código de status da resposta HTTP. Se `validateStatus` retornar `true` (ou for definido como `null`
  // ou `undefined`), a promessa será resolvida; caso contrário, a promessa será rejeitado.
  validateStatus: function (status) {
    return status >= 200 && status < 300; // padrão
  }
```

MaxRedirects:

```javascript
  // `maxRedirects` define o número máximo de redirecionamentos a seguir em node.js.
  // Se definido como 0, nenhum redirecionamento será seguido.
  maxRedirects: 5 // padrão
```

SocketPath:

```javascript
  // `socketPath` define um Socket UNIX a ser usado em node.js.
  // por exemplo. '/var/run/docker.sock' para enviar solicitações ao daemon docker.
  // Somente `socketPath` ou `proxy` podem ser especificados.
  // Se ambos forem especificados, `socketPath` é usado.
  socketPath: null // padrão
```

HttpAgent e HttpsAgent:

```javascript
  // `httpAgent` e `httpsAgent` definem um agente personalizado a ser usado ao realizar http
  // e solicitações https, respectivamente, em node.js. Isso permite que opções sejam adicionadas como
  // `keepAlive` que não são ativados por padrão.
  httpAgent: new http.Agent({ keepAlive: true }),
  httpsAgent: new https.Agent({ keepAlive: true })
```

Proxy:

```javascript
  // `proxy` define o nome do host, porta e protocolo do servidor proxy.
  // Você também pode definir seu proxy usando o `http_proxy` convencional e
  // variáveis de ambiente `https_proxy`. Se você estiver usando variáveis de ambiente
  // para sua configuração de proxy, você também pode definir um ambiente `no_proxy`
  // variável como uma lista separada por vírgulas de domínios que não devem ser proxy.
  // Use `false` para desabilitar proxies, ignorando variáveis de ambiente.
  // `auth` indica que HTTP Basic auth deve ser usado para conectar ao proxy, e
  // fornece credenciais.
  // Isso definirá um cabeçalho `Proxy-Authorization`, sobrescrevendo qualquer
  // Cabeçalhos customizados `Proxy-Authorization` que você definiu usando `headers`.
  // Se o servidor proxy usar HTTPS, você deverá definir o protocolo como `https`.
  proxy: {
    protocol: 'https',
    host: '127.0.0.1',
    port: 9000,
    auth: {
      username: 'mikeymike',
      password: 'rapunz3l'
    }
  }
```

CancelToken:

```javascript
  // `cancelToken` especifica um token de cancelamento que pode ser usado para cancelar a solicitação
  cancelToken: new CancelToken(function (cancel) {
  })
```

Decompress:

```javascript
  // `decompress` indica se o corpo da resposta deve ou não ser descompactado
  // automaticamente. Se definido como `true` também removerá o cabeçalho 'codificação de conteúdo'
  // dos objetos de respostas de todas as respostas descompactadas
  // - Somente Node (o XHR não pode desligar a descompressão)
  decompress: true // padrão
```

-------------------------------------------------------------

### axios.request

#### Depende de:
- Conceitual
  - [URL](#url)
  - [Requisições e respostas HTTP](#requisições-e-respostas-http)
- Cenários de uso
  - Nenhum

O método `axios.request` é parte da biblioteca JavaScript Axios, utilizada para fazer requisições HTTP. Ele permite realizar uma requisição HTTP personalizada, com controle total sobre os parâmetros da requisição, como URL, método HTTP, cabeçalhos e corpo da requisição.

A sintaxe básica do método é a seguinte:

```javascript
axios.request(config)
```

O parâmetro `config` é um objeto que contém as opções de configuração da requisição. Algumas das opções disponíveis são:

- `url` (string): a URL para a qual a requisição será enviada.
- `method` (string): o método HTTP a ser utilizado, como GET, POST, PUT, DELETE, etc.
- `headers` (object): um objeto contendo os cabeçalhos da requisição.
- `params` (object): um objeto contendo os parâmetros da URL da requisição.
- `data` (object): os dados a serem enviados no corpo da requisição.

Segue um exemplo de como utilizar o método `axios.request` para fazer uma requisição POST:

```javascript
axios.request({
  url: 'https://api.exemplo.com/endpoint',
  method: 'post',
  headers: {
    'Content-Type': 'application/json',
    'Authorization': 'Bearer token_de_autorizacao'
  },
  data: {
    nome: 'Exemplo',
    idade: 25
  }
})
  .then(response => {
    console.log(response.data);
  })
  .catch(error => {
    console.error(error);
  });
```

Neste exemplo, estamos enviando uma requisição POST para a URL 'https://api.exemplo.com/endpoint'. Definimos os cabeçalhos da requisição, incluindo o tipo de conteúdo como JSON e um token de autorização. Também passamos um objeto com os dados a serem enviados no corpo da requisição.

O método `axios.request` retorna uma Promessa (Promise), permitindo que utilizemos os métodos `.then()` e `.catch()` para lidar com a resposta da requisição.

É importante lembrar que a biblioteca Axios deve ser importada corretamente no projeto antes de utilizar o método.

-------------------------------------------------------------

### axios.head

#### Depende de:
- Conceitual
  - [URL](#url)
  - [Requisições e respostas HTTP](#requisições-e-respostas-http)
- Cenários de uso
  - Nenhum

O método `axios.head` é mais uma funcionalidade oferecida pela biblioteca Axios, amplamente utilizada para realizar requisições HTTP em aplicações JavaScript. Ele permite enviar uma requisição HTTP do tipo HEAD para um determinado endpoint, obtendo apenas os cabeçalhos da resposta, sem retornar o corpo da resposta.

A sintaxe básica do método é a seguinte:

```javascript
axios.head(url, config)
```

O primeiro parâmetro `url` é uma string que representa a URL para a qual a requisição HEAD será enviada. O segundo parâmetro opcional `config` é um objeto que contém opções de configuração adicionais para a requisição, semelhante ao que vimos no exemplo anterior do método `axios.request`.

Ao utilizar o método `axios.head`, a biblioteca Axios enviará uma requisição HTTP do tipo HEAD para a URL especificada. Esse tipo de requisição é útil quando estamos interessados apenas nos cabeçalhos da resposta, como informações de status, cabeçalhos personalizados ou informações de autenticação.

Aqui está um exemplo de como utilizar o método `axios.head`:

```javascript
axios.head('https://api.exemplo.com/endpoint', {
  headers: {
    'Authorization': 'Bearer token_de_autorizacao'
  }
})
  .then(response => {
    console.log(response.headers);
  })
  .catch(error => {
    console.error(error);
  });
```

Nesse exemplo, estamos enviando uma requisição HEAD para a URL 'https://api.exemplo.com/endpoint'. Também definimos um cabeçalho de autorização usando um token de autenticação.

Ao receber a resposta, utilizamos o método `.then()` para acessar o objeto `response` que contém informações sobre a resposta da requisição. No caso do método `axios.head`, estamos interessados nos cabeçalhos da resposta, que podem ser acessados através da propriedade `response.headers`.

Assim como outros métodos Axios, o método `axios.head` retorna uma Promessa, permitindo que lidemos com a resposta da requisição utilizando `.then()` para o caso de sucesso e `.catch()` para o caso de erro.

É importante ressaltar que a biblioteca Axios deve ser corretamente importada no projeto antes de utilizar o método `axios.head`, assim como nos demais métodos da biblioteca.

-------------------------------------------------------------

### axios.get

#### Depende de:
- Conceitual
  - [Cliente-Servidor](#cliente-servidor)
  - [URL](#url)
  - [Requisição e respostas HTTP](#requisições-e-respostas-http)
  - [Status de resposta à requisição HTTP](#status-de-resposta-à-requisição-http)
- Cenários de uso
  - [Realizar operações CRUD em um servidor](#cenário-realizar-operações-crud-em-um-servidor)

```
axios.get(url[, config])
```

Envia uma requisição GET para um servidor.

```javascript=
const axios = require('axios');

axios.get('https://api.example.com/data')
  .then(response => {
    console.log(response.data);
  })
  .catch(error => {
    console.error(error);
  });
```

-------------------------------------------------------------

### axios.post

#### Depende de:
- Conceitual
  - [Cliente-Servidor](#cliente-servidor)
  - [URL](#url)
  - [Requisição e respostas HTTP](#requisições-e-respostas-http)
  - [Status de resposta à requisição HTTP](#status-de-resposta-à-requisição-http)
- Cenários de uso
  - [Realizar upload de arquivos](#cenário-realizar-upload-de-arquivos)
  - [Realizar operações CRUD em um servidor](#cenário-realizar-operações-crud-em-um-servidor)

```
axios.post(url[, data[, config]])
```

Envia uma requisição POST para um servidor.

```javascript=
const axios = require('axios');

const data = {
  name: 'Brian Pravato',
  email: 'brianpravato@edu.unirio.br'
};

axios.post('https://api.example.com/postData', data)
  .then(response => {
    console.log(response.data);
  })
  .catch(error => {
    console.error(error);
  });
```

-------------------------------------------------------------

### axios.put

#### Depende de:
- Conceitual
  - [Cliente-Servidor](#cliente-servidor)
  - [URL](#url)
  - [Requisição e respostas HTTP](#requisições-e-respostas-http)
  - [Status de resposta à requisição HTTP](#status-de-resposta-à-requisição-http)
- Cenários de uso
  - [Realizar upload de arquivos](#cenário-realizar-upload-de-arquivos)
  - [Realizar operações CRUD em um servidor](#cenário-realizar-operações-crud-em-um-servidor)

```
axios.put(url[, data[, config]])
```

Envia uma requisição PUT para um servidor.

```javascript=
const axios = require('axios');

const data = {
  name: 'Mateus Solano',
  email: 'mateus.solano@edu.unirio.br',
};

axios.put('https://api.example.com/updateData', data)
  .then(response => {
    console.log(response.data);
  })
  .catch(error => {
    console.error(error);
  });
```

-------------------------------------------------------------

### axios.patch

#### Depende de:
- Conceitual
  - [Cliente-Servidor](#cliente-servidor)
  - [URL](#url)
- cenários de uso
  - [Realizar operações CRUD em um servidor](#cenário-realizar-operações-crud-em-um-servidor)

```
axios.patch(url[, data[, config]])
```

Envia uma requisição PATCH para um servidor.

```javascript=
const axios = require('axios');

const data = {
  email: 'msolano@edu.unirio.br',
};

axios.patch('https://api.example.com/updateData', data)
  .then(response => {
    console.log(response.data);
  })
  .catch(error => {
    console.error(error);
  });
```

-------------------------------------------------------------

### axios.delete

#### Depende de:
- Conceitual
  - [Cliente-Servidor](#cliente-servidor)
  - [URL](#url)
- Cenários de uso
  - Nenhum

```
axios.delete(url[, config])
```

Envia uma requisição DELETE para um servidor.

```javascript=
const axios = require('axios');

axios.delete('https://api.example.com/deleteData/123')
  .then(response => {
    console.log(response.data);
  })
  .catch(error => {
    console.error(error);
  });
```

-------------------------------------------------------------

### axios.options

#### Depende de:
- Conceitual
  - [Cliente-Servidor](#cliente-servidor)
  - [URL](#url)
- Cenários de uso
  - Nenhum

```
axios.options(url[, config])
```

Envia uma requisição OPTIONS para um servidor.

```javascript=
const axios = require('axios');

axios.options('https://api.example.com')
  .then(response => {
    console.log(response.data);
  })
  .catch(error => {
    console.error(error);
  });
```

-------------------------------------------------------------

### axios.postForm, axios.putForm e axios.patchForm

#### Depende de:
- Conceitual
  - [URL](#url)
  - [Requisições e respostas HTTP](#requisições-e-respostas-http)
- Cenários de uso
  - Nenhum

Esses são métodos alternativos ao `axios.post`, `axios.put` e `axios.patch` (respectivamente) que serializam automaticamente um objeto javascript com campos e arquivos em um FormData.

Veja um exemplo de uso abaixo:

```js
const dados = {
  nome: 'José',
  bebidasFavoritas: [
    { nome: 'Ice', preco: 9 },
    { nome: 'Monster', preco: 8 }
  ],
  avatar: objetoFile
}
axios.postForm('https://end.po/int', dados);
```

Sem esse método o mesmo resultado pode ser atingido, porém com mais trabalho, serializando manualmente com a classe FormData:

```js
const formData = new FormData();
formData.append('nome', 'José');
formData.append('bebidasFavoritas[0][nome]', 'Ice');
formData.append('bebidasFavoritas[0][preco]', 9);
formData.append('bebidasFavoritas[1][nome]', 'Monster');
formData.append('bebidasFavoritas[1][preco]', 8);
formData.append('avatar', objetoFile);

axios.post('https://end.po/int', formData, {
  headers: {
    'Content-Type': 'multipart/form-data'
  }
});
```

-------------------------------------------------------------

### axios.interceptors

#### Depende de:
- Conceitual
  - [Cliente-Servidor](#cliente-servidor)
  - [URL](#url)
  - [Requisição e respostas HTTP](#requisições-e-respostas-http)
  - [Status de resposta à requisição HTTP](#status-de-resposta-à-requisição-http)
- Cenários de uso
  - [Repetição de requisição com interceptadores](#cenário-repetição-de-requisição-com-interceptadores)

A biblioteca Axios fornece a funcionalidade `axios.interceptors` para interceptar e manipular as requisições e respostas HTTP antes de serem enviadas ou recebidas. Ela permite adicionar interceptadores tanto para as requisições (axios.interceptors.request) quanto para as respostas (axios.interceptors.response).

#### axios.interceptors.request

O método `axios.interceptors.request` permite adicionar interceptadores para as requisições HTTP antes de serem enviadas. Isso possibilita a modificação dos cabeçalhos, a adição de tokens de autenticação, a manipulação dos dados da requisição e outras operações prévias ao envio da requisição.

Aqui está um exemplo de como utilizar o `axios.interceptors.request`:

```javascript
axios.interceptors.request.use(function (config) {
  // Executa alguma lógica antes do envio da requisição
  config.headers['Authorization'] = 'Bearer token_de_autorizacao';
  return config;
}, function (error) {
  // Lida com erros de requisição
  return Promise.reject(error);
});
```

Neste exemplo, estamos adicionando um interceptador para as requisições usando o método `axios.interceptors.request.use`. O primeiro argumento da função é um callback que será executado antes do envio da requisição. Nele, podemos realizar qualquer lógica desejada, como adicionar um cabeçalho de autorização à configuração da requisição.

Se a lógica prévia ao envio da requisição for concluída com sucesso, devemos retornar a configuração atualizada utilizando a declaração `return config;`. Caso ocorra algum erro durante a manipulação, podemos retornar uma Promessa rejeitada com `return Promise.reject(error);`.

#### axios.interceptors.response

O método `axios.interceptors.response` permite adicionar interceptadores para as respostas HTTP recebidas após o envio das requisições. Isso permite a manipulação dos dados de resposta, o tratamento de erros específicos e outras operações pós-recebimento da resposta.

Aqui está um exemplo de como utilizar o `axios.interceptors.response`:

```javascript
axios.interceptors.response.use(function (response) {
  // Executa alguma lógica com a resposta recebida
  console.log(response.data);
  return response;
}, function (error) {
  // Lida com erros de resposta
  return Promise.reject(error);
});
```

Neste exemplo, estamos adicionando um interceptador para as respostas usando o método `axios.interceptors.response.use`. O primeiro argumento da função é um callback que será executado após a recepção da resposta. Nele, podemos realizar qualquer lógica desejada, como exibir os dados da resposta utilizando `console.log(response.data)`.

Se a lógica pós-recebimento da resposta for concluída com sucesso, devemos retornar a resposta original utilizando `return response;`. Caso ocorra algum erro durante a manipulação, podemos retornar uma Promessa rejeitada com `return Promise.reject(error);`.

É importante destacar que, ao adicionar interceptadores, devemos levar em consideração a ordem em que eles serão executados. Os interceptadores são chamados na ordem em que foram adicionados, e cada interceptador pode modificar os dados da requisição ou resposta antes que eles sejam passados para o próximo interceptador ou para o código que realizou a chamada à biblioteca Axios

-------------------------------------------------------------

### axios.defaults

#### Depende de:
- Conceitual
  - [Cliente-Servidor](#cliente-servidor)
  - [URL](#url)
- Cenários de uso
  - Nenhum

Você pode especificar padrões de configuração que serão aplicados a cada solicitação.

Esses padrões podem ser aplicados globalmente, ou seja, a todas instâncias do Axios. Ou podem ser aplicadas individualmente, em apenas uma instância.

Padrões globais do Axios:
```javascript
axios.defaults.baseURL = 'https://api.exemplo.com';
axios.defaults.headers.common['Autorizacao'] = AUTH_TOKEN;
axios.defaults.headers.post['Content-Type'] = 'application/x-www-form-urlencoded';
```

Padrões de uma instância customizada:

```javascript
// Configurando os padrões de uma instância customizada em sua criação
const instance = axios.create({
  baseURL: 'https://api.example.com'
});

// Alterando os padrões de uma instância após sua criação
instance.defaults.headers.common['Authorization'] = AUTH_TOKEN;
```

Ordem de precedência de configuração<br>

A configuração será mesclada seguindo uma ordem de precedência. A ordem é: <br>
1- Os padrões da biblioteca encontrados em lib/defaults/index.js <br>
2- A propriedade padrão da instância <br>
3- E finalmente, o argumento de configuração para a solicitação. 

O último terá precedência sobre o primeiro. Aqui está um exemplo:

```javascript
// Cria uma instância usando os padrões de configuração fornecidos pela biblioteca
// Neste ponto, o valor de configuração de tempo limite é `0` como é o padrão para a biblioteca
const instance = axios.create();


// Substitui o tempo limite padrão da biblioteca
// Agora todas as requisições que usam esta instância irão esperar 2,5 segundos antes de atingir o tempo limite
instance.defaults.timeout = 2500;

// Substitui o tempo limite para esta solicitação, pois é conhecido por demorar muito
instance.get('/longRequest', {
  timeout: 5000
});
```

-------------------------------------------------------------

## Funcionamento interno

### Envio de requisições e recebimento de respostas

- identificado por: Arlindo, Breno, Brian, Rafael

#### Depende de:
- Conceitual
  - [URL](#url)
  - [Requisição e respostas HTTP](#requisições-e-respostas-http)
  - [Status de resposta à requisição HTTP](#status-de-resposta-à-requisição-http)
- Cenários de uso
  - [Realizar operações CRUD em um servidor](#cenário-realizar-operações-crud-em-um-servidor)

Envio de Requisições e Recebimento de Respostas no Axios: Controle Avançado do Fluxo HTTP

O Axios é uma poderosa biblioteca que oferece controle avançado sobre o envio de requisições HTTP e o recebimento de respostas. Por meio da implementação cuidadosa de suas funcionalidades internas, o Axios proporciona flexibilidade e personalização em todo o processo de comunicação com servidores. Neste artigo, vamos explorar a estrutura e o funcionamento interno do Axios para entender como ele gerencia o envio de requisições e o recebimento de respostas.

#### Envio de Requisições

O envio de requisições HTTP no Axios é realizado por meio da função `dispatchRequest`, que desempenha um papel central nesse processo. Vamos analisar um trecho de código dessa função para entender sua implementação interna:

```javascript
export default function dispatchRequest(config) {
  throwIfCancellationRequested(config);

  config.headers = AxiosHeaders.from(config.headers);

  // Transforma contidos na requisição
  config.data = transformData.call(
    config,
    config.transformRequest
  );

  if (['post', 'put', 'patch'].indexOf(config.method) !== -1) {
    config.headers.setContentType('application/x-www-form-urlencoded', false);
  }

  const adapter = adapters.getAdapter(config.adapter || defaults.adapter);

  return adapter(config).then(function onAdapterResolution(response) {
    throwIfCancellationRequested(config);

    // Transforma dados da resposta à requisição
    response.data = transformData.call(
      config,
      config.transformResponse,
      response
    );

    response.headers = AxiosHeaders.from(response.headers);

    return response;
  }, function onAdapterRejection(reason) {
    if (!isCancel(reason)) {
      throwIfCancellationRequested(config);

      // Transforma dados da resposta à requisição
      if (reason && reason.response) {
        reason.response.data = transformData.call(
          config,
          config.transformResponse,
          reason.response
        );
        reason.response.headers = AxiosHeaders.from(reason.response.headers);
      }
    }

    return Promise.reject(reason);
  });
}
```

Nesse trecho de código, várias etapas são executadas para enviar uma requisição:

1. Verificação de cancelamento: A função `throwIfCancellationRequested` é chamada para verificar se a requisição foi cancelada. Isso permite interromper o processamento da requisição caso o cancelamento tenha sido solicitado.

2. Manipulação de cabeçalhos: Os cabeçalhos da requisição são processados usando a classe `AxiosHeaders`. Isso permite a personalização e a manipulação dos cabeçalhos antes do envio da requisição.

3. Transformação dos dados da requisição: A função `transformData` é chamada para aplicar transformações personalizadas nos dados da requisição. Isso permite que os desenvolvedores modifiquem e ajustem os dados antes de serem enviados ao servidor.

4. Configuração do tipo de conteúdo: Caso o método da requisição seja "post", "put" ou "patch", o tipo de conteúdo é definido como "application/x-www-form-urlencoded". Essa configuração específica é importante para garantir a correta interpretação dos dados pelo servidor.

5. Obtenção do adaptador: O adaptador apropriado é obtido com base na configuração da requisição. O adaptador é responsável por enviar a requisição para o servidor e retornar uma promessa que será resolvida com a resposta do servidor.

6. Manipulação da resposta: Após receber a resposta do servidor, a função `onAdapterResolution` é chamada. Nessa função, ocorre a verificação de cancelamento novamente, para garantir que a resposta seja processada apenas se a requisição não tiver sido cancelada. Em seguida, a função `transformData` é usada para transformar os dados da resposta, permitindo que sejam manipulados antes de serem retornados ao usuário.

7. Rejeição de promessas: Caso ocorra uma rejeição na chamada ao adaptador, a função `onAdapterRejection` é chamada. Nessa função, é verificado se o motivo da rejeição é um cancelamento. Se não for, a função `throwIfCancellationRequested` é chamada novamente para garantir que a requisição não tenha sido cancelada. Além disso, a função `transformData` é usada para transformar os dados da resposta, caso existam, antes de rejeitar a promessa com o motivo fornecido.

Essas etapas demonstram como o Axios implementa o envio de requisições HTTP. Através da verificação de cancelamento, manipulação de cabeçalhos, transformação dos dados da requisição e utilização de adaptadores, o Axios oferece uma estrutura robusta e personalizável para enviar requisições HTTP aos servidores.

#### Recebimento de Respostas

O recebimento de respostas no Axios também é tratado dentro da função `dispatchRequest`. Vamos destacar o trecho de código relevante para entender a implementação do recebimento de respostas:

```javascript
return adapter(config).then(function onAdapterResolution(response) {
  throwIfCancellationRequested(config);

  // Transforma dados da resposta à requisição
  response.data = transformData.call(
    config,
    config.transformResponse,
    response
  );

  response.headers = AxiosHeaders.from(response.headers);

  return response;
}, function onAdapterRejection(reason) {
  if (!isCancel(reason)) {
    throwIfCancellationRequested(config);

    // Transforma dados da resposta à requisição
    if (reason && reason.response) {
      reason.response.data = transformData.call(
        config,
        config.transformResponse,
        reason.response
      );
      reason.response.headers = AxiosHeaders.from(reason.response.headers);
    }
  }

  return Promise.reject(reason);
});
```

Nesse trecho de código, ocorrem as seguintes etapas:

1. Chamada ao adaptador: O adaptador é chamado para enviar a requisição ao servidor e obter a resposta correspondente.

2. Tratamento da resposta bem-sucedida: Se a promessa for resolvida com sucesso, a função `onAdapterResolution` é chamada. Nessa função, novamente é verificada a possibilidade de cancelamento da requisição. Em seguida, ocorre a transformação dos dados da resposta usando a função `transformData`. Isso permite que os dados sejam manipulados antes de serem retornados ao usuário. Além disso, os cabeçalhos da resposta são processados usando a classe `AxiosHeaders`, permitindo a personalização e manipulação dos cabeçalhos antes de serem retornados.

3. Tratamento de rejeição: Caso ocorra uma rejeição na chamada ao adaptador, a função `onAdapterRejection` é chamada. Nessa função, verifica-se se o motivo da rejeição não é um cancelamento. Se não for, a função `throwIfCancellationRequested` é chamada novamente para garantir que a requisição não tenha sido cancelada. Além disso, ocorre a transformação dos dados da resposta, caso existam, usando a função `transformData`. Também são processados os cabeçalhos da resposta com a classe `AxiosHeaders`. Por fim, a promessa é rejeitada com o motivo fornecido.

Essas etapas ilustram como o Axios implementa o recebimento de respostas. Ao usar adaptadores para enviar as requisições e manipular as respostas, o Axios oferece uma estrutura robusta e personalizável para receber respostas HTTP dos servidores.

Em suma, o Axios proporciona controle avançado do fluxo de envio de requisições e recebimento de respostas por meio da função `dispatchRequest`. Ao utilizar a verificação de cancelamento, manipulação de cabeçalhos, transformação de dados e adaptadores, o Axios oferece uma solução flexível e poderosa para lidar com requisições HTTP de forma personalizada e eficiente.

-------------------------------------------------------------

### Da chamada de função a requisição: como o Axios utiliza a API XMLHttpRequest

- identificado por: Rafael

#### Depende de:
- Conceitual
  - [Cliente-Servidor](#cliente-servidor)
  - [URL](#url)
  - [Requisição e respostas HTTP](#requisições-e-respostas-http)
  - [Status de resposta à requisição HTTP](#status-de-resposta-à-requisição-http)
  - [XHR - XMLHttpRequest](#xhr---xmlhttprequest)
- Cenários de uso
  - Nenhum

O Axios é uma biblioteca JavaScript amplamente utilizada para fazer solicitações HTTP de forma fácil e eficiente. Uma das formas como o Axios lida com as solicitações é por meio da API XMLHttpRequest, que é uma interface nativa dos navegadores para enviar e receber dados por meio de requisições HTTP assíncronas.

Ao usar o Axios, a biblioteca cria uma instância da classe `Axios` e utiliza a API XMLHttpRequest internamente para enviar solicitações HTTP. Vamos examinar parte do código-fonte relevante no arquivo `lib/core/Axios.js`:

```javascript
function Axios(instanceConfig) {
  // ...
  this.request = function request(config) {
    // ...
    // Cria uma instância de XMLHttpRequest
    var adapter = config.adapter || defaults.adapter;
    var options = adapter(config);
    // ...

    // Envia a solicitação usando a API XMLHttpRequest
    return new Promise(function dispatchXhrRequest(resolve, reject) {
      var requestData = config.data;
      var requestHeaders = config.headers;

      // ...

      var xhr = new XMLHttpRequest();

      // Configura os manipuladores de eventos
      // para lidar com a resposta da solicitação
      xhr.open(config.method.toUpperCase(), buildURL(config.url, config.params, config.paramsSerializer), true);

      xhr.onreadystatechange = function handleLoad() {
        if (!xhr || xhr.readyState !== 4) {
          return;
        }

        // ...
        // Lógica para tratar a resposta
        // ...

        resolve(response);
      };

      xhr.onerror = function handleError() {
        reject(createError('Network Error', config, null, xhr));
      };

      xhr.ontimeout = function handleTimeout() {
        reject(createError('timeout of ' + config.timeout + 'ms exceeded', config, 'ECONNABORTED', xhr));
      };

      // ...

      // Envia a solicitação
      xhr.send(requestData);
    });
  };
  // ...
}
```

No código acima, a função `request` é definida dentro da classe `Axios` e é responsável por enviar uma solicitação HTTP. Dentro dessa função, uma instância da classe `XMLHttpRequest` é criada e utilizada para realizar a solicitação real.

Primeiro, o Axios obtém o adaptador de solicitação apropriado, que é responsável por retornar as opções específicas do adaptador para a API XMLHttpRequest. Em seguida, as opções de solicitação são obtidas chamando o adaptador com a configuração da solicitação.

Depois, é criada uma nova instância da classe `XMLHttpRequest` e configurada usando o método `open()`. O método `open()` define o método HTTP, a URL e a natureza assíncrona da solicitação. Em seguida, os manipuladores de eventos são configurados para lidar com a resposta da solicitação.

O manipulador `onreadystatechange` é responsável por verificar o estado da solicitação e executar a lógica apropriada quando a solicitação é concluída. No exemplo acima, ele chama a função `handleLoad()` quando o estado da solicitação é `4` (indicando que a resposta está completa). Dentro dessa função, a resposta é processada e a promessa é resolvida com a resposta.

Outros manipuladores de eventos, como `onerror` e `ontimeout`, são configurados para lidar com erros de rede e tempos limite. Esses manipuladores re

jeitam a promessa com um erro apropriado, fornecendo informações detalhadas sobre o erro que ocorreu durante a solicitação.

Finalmente, a solicitação é enviada chamando o método `send()` da instância da API XMLHttpRequest, passando os dados da solicitação.

Essa implementação mostra como o Axios utiliza a API XMLHttpRequest internamente para enviar e receber solicitações HTTP. A biblioteca cria uma instância do objeto XMLHttpRequest, configura os manipuladores de eventos apropriados e envia a solicitação por meio do método `send()`. Essa integração transparente com a API nativa dos navegadores permite que o Axios forneça uma funcionalidade abrangente e confiável para a realização de solicitações HTTP.

Em resumo, o Axios utiliza a API XMLHttpRequest para enviar solicitações HTTP de forma assíncrona. A biblioteca configura e utiliza os métodos e manipuladores de eventos da API para fornecer um mecanismo robusto para envio e recebimento de dados por meio de requisições HTTP. Essa abordagem permite que os desenvolvedores utilizem o Axios de maneira intuitiva e eficiente, aproveitando a capacidade da API XMLHttpRequest para realizar solicitações HTTP.

-------------------------------------------------------------

### Interceptadores: uso e funcionamento

- identificador por: Rafael

#### Depende de:
- Conceitual
  - [Cliente-Servidor](#cliente-servidor)
  - [URL](#url)
  - [Requisição e respostas HTTP](#requisições-e-respostas-http)
  - [Status de resposta à requisição HTTP](#status-de-resposta-à-requisição-http)
  - [Interceptadores (funções de middleware)](#interceptadores-funções-de-middleware)
- Cenários de uso
  - [Repetição de requisição com interceptadores](#cenário-repetição-de-requisição-com-interceptadores)

Os Axios Interceptors são uma funcionalidade fundamental implementada na biblioteca Axios, permitindo o controle flexível e personalizado das solicitações HTTP. Por meio dos interceptadores, é possível adicionar lógica personalizada antes de uma solicitação ser enviada ou após uma resposta ser recebida, abrindo caminho para uma série de possibilidades de personalização e tratamento de requisições.

#### Interceptador de Solicitação (Request Interceptor)

O Axios implementa o interceptador de solicitação dentro da função `dispatchRequest`, que é responsável pelo processamento de uma solicitação HTTP. Dentro dessa função, um array chamado `chain` é definido para armazenar uma lista de funções que serão executadas sequencialmente como parte do fluxo do interceptador de solicitação.

```javascript
function dispatchRequest(config) {
  // ...

  // Cria uma cadeia de promessas para executar os interceptadores
  var chain = [dispatchRequest, undefined];

  // Adiciona os interceptadores de solicitação à cadeia
  this.interceptors.request.forEach(function unshiftRequestInterceptors(interceptor) {
    chain.unshift(interceptor.fulfilled, interceptor.rejected);
  });

  // ...

  // Executa a cadeia de promessas
  var promise = Promise.resolve(config);
  while (chain.length) {
    promise = promise.then(chain.shift(), chain.shift());
  }

  // ...

  return promise;
}
```

No trecho de código acima, os interceptadores de solicitação são adicionados ao array `chain` por meio da função `unshift`, que insere os interceptadores no início do array. Essa lógica garante que os interceptadores de solicitação sejam executados na ordem correta, antes da execução da solicitação real.

#### Interceptador de Resposta (Response Interceptor)

A implementação do interceptador de resposta no Axios ocorre na mesma função `dispatchRequest`. O array `chain` é novamente utilizado para armazenar as funções de resposta que serão executadas sequencialmente.

```javascript
function dispatchRequest(config) {
  // ...

  // Cria uma cadeia de promessas para executar os interceptadores
  var chain = [dispatchRequest, undefined];

  // ...

  // Adiciona os interceptadores de resposta à cadeia
  this.interceptors.response.forEach(function pushResponseInterceptors(interceptor) {
    chain.push(interceptor.fulfilled, interceptor.rejected);
  });

  // ...

  // Executa a cadeia de promessas
  var promise = Promise.resolve(config);
  while (chain.length) {
    promise = promise.then(chain.shift(), chain.shift());
  }

  // ...

  return promise;
}
```

No exemplo acima, os interceptadores de resposta são adicionados ao array `chain` por meio da função `push`, que insere os interceptadores ao final do array. Dessa forma, é garantido que os interceptadores de resposta sejam executados sequencialmente após a recepção da resposta do servidor.

Essa abordagem permite uma configuração flexível dos interceptadores de solicitação e resposta no Axios. Ao adicionar interceptadores à cadeia, os desenvolvedores podem personalizar o comportamento do Axios antes e depois das solicitações HTTP, possibilitando a realização de ações como a modificação de dados, manipulação de erros, tratamento de cabeçalhos e muito mais.

Em resumo, os interceptadores de solicitação e resposta do Axios são implementados por meio da criação de uma cadeia de promessas, onde as funções dos interceptadores são adicionadas na ordem correta. Essa abordagem garante o controle preciso do fluxo das solicitações HTTP, permitindo a personalização e manipulação das requisições de acordo com as necessidades do desenvolvedor.

-------------------------------------------------------------

### Conversão dinâmica em JSON

- identificado por: Breno e Brian

#### Depende de:
- Conceitual
  - [Cliente-Servidor](#cliente-servidor)
  - [URL](#url)
  - [Requisição e respostas HTTP](#requisições-e-respostas-http)
  - [JSON](#json)
- Cenários de uso
  - [Manipular Headers de uma requisição](#cenário-manipular-headers-de-uma-requisição)

A conversão de dados JavaScript para JSON é uma funcionalidade fundamental implementada no Axios, permitindo que os dados sejam formatados corretamente antes de serem enviados em uma requisição HTTP. Essa conversão é realizada pela função `transformData`, presente no arquivo `transformData.js`. Vamos analisar o código-fonte relevante:

```javascript
import utils from './../utils.js';
import defaults from '../defaults/index.js';
import AxiosHeaders from '../core/AxiosHeaders.js';

/**
 * Transforma os dados de uma requisição ou resposta
 *
 * @param {Array|Function} fns Uma função única ou uma matriz de funções
 * @param {?Object} response O objeto de resposta
 *
 * @returns {*} Os dados transformados resultantes
 */
export default function transformData(fns, response) {
  const config = this || defaults;
  const context = response || config;
  const headers = AxiosHeaders.from(context.headers);
  let data = context.data;

  utils.forEach(fns, function transform(fn) {
    data = fn.call(config, data, headers.normalize(), response ? response.status : undefined);
  });

  headers.normalize();

  return data;
}
```

No trecho de código acima, a função `transformData` recebe dois parâmetros: `fns` e `response`. O parâmetro `fns` pode ser uma função única ou uma matriz de funções. Essas funções serão aplicadas aos dados a serem enviados em uma requisição HTTP.

Dentro da função, uma série de operações são realizadas. Primeiro, as variáveis `config` e `context` são definidas com base nos valores recebidos. Em seguida, a variável `headers` é inicializada como uma instância da classe `AxiosHeaders`, que é responsável pelo tratamento e normalização dos cabeçalhos HTTP.

A variável `data` é definida com o valor dos dados a serem transformados, que pode ser o objeto de dados da configuração ou o objeto de resposta, dependendo do contexto. Em seguida, é feito um loop pelas funções fornecidas em `fns` usando a função `forEach` do utilitário `utils`. A cada iteração, uma função de transformação é chamada, passando os dados, os cabeçalhos normalizados e, opcionalmente, o status da resposta. Essas funções de transformação podem ser personalizadas pelo usuário para realizar qualquer operação desejada nos dados.

Após o loop, os cabeçalhos são normalizados novamente chamando o método `normalize()` da variável `headers`. Por fim, os dados transformados são retornados pela função.

A implementação da conversão de JavaScript para JSON no Axios fornece uma flexibilidade significativa ao permitir que os desenvolvedores personalizem e manipulem os dados antes de enviá-los em uma requisição HTTP. Com essa funcionalidade, é possível formatar os dados adequadamente e ajustá-los de acordo com os requisitos específicos da API que está sendo consumida.

Em resumo, a função `transformData` do Axios é responsável pela conversão de dados JavaScript para JSON antes de uma requisição ser enviada. Ela permite que o desenvolvedor aplique transformações personalizadas nos dados, garantindo que estejam formatados corretamente antes do envio. Essa funcionalidade é essencial para garantir a interoperabilidade correta entre o Axios e as APIs com as quais ele interage.

-------------------------------------------------------------

## Efeitos colaterais

###  Efeito colateral: Axios e problemas de compatibilidade com navegadores antigos

- identificado por: Brian, Rafael

#### Depende de:
- Conceitual
  - Nenhum
- Cenários de uso
  - Nenhum

O uso da biblioteca Axios pode apresentar um efeito colateral relacionado à compatibilidade com navegadores antigos e desatualizados. Esses *browsers* podem não suportar totalmente os recursos modernos utilizados pelo Axios, o que pode resultar em comportamentos inesperados ou falhas na execução das requisições.

Para mitigar esse problema e garantir uma melhor compatibilidade com *browsers* antigos, é possível adotar algumas estratégias. Uma delas é o uso de um **polyfill**, código JavaScript que fornece implementações de recursos ou funcionalidades que estão faltando em browsers mais antigos, permitindo que esses browsers suportem recursos modernos.

Nesse contexto, um polyfill para o Axios pode adicionar suporte para os recursos utilizados pela biblioteca em browsers mais antigos, preenchendo as lacunas de funcionalidade e garantindo o comportamento esperado das requisições.

Para utilizar um polyfill com o Axios, é necessário importar o polyfill antes de utilizar a biblioteca. Existem diferentes polyfills disponíveis que se destinam a preencher as lacunas de compatibilidade do navegador para recursos específicos do Axios, como *Promises* e a *API Fetch*.

Aqui está um exemplo de como utilizar um polyfill para a funcionalidade de Promises, que é amplamente utilizada pelo Axios:

```javascript
// Importa o polyfill para Promises
import 'es6-promise/auto';

// Importa o Axios normalmente
import axios from 'axios';

// Utiliza o Axios para fazer a requisição
axios.get('https://api.exemplo.com/data')
  .then(response => {
    console.log(response.data);
  })
  .catch(error => {
    console.error(error);
  });
```

Neste exemplo, primeiro importamos o polyfill para Promises utilizando a biblioteca `es6-promise/auto`. Em seguida, importamos o Axios normalmente e utilizamos o Axios para fazer a requisição `GET`.

O polyfill para Promises garante que navegadores mais antigos que não possuem suporte nativo para Promises possam utilizar essa funcionalidade. Ele adiciona as implementações necessárias para que o código do Axios funcione corretamente nesses browsers, permitindo o uso consistente das Promises.

É importante lembrar que, ao utilizar um polyfill, é necessário considerar o tamanho do arquivo adicionado e o desempenho em browsers mais modernos, que já possuem suporte nativo para os recursos em questão. Portanto, é recomendado avaliar cuidadosamente a necessidade do polyfill em cada contexto e considerar o suporte de browsers específicos ao tomar a decisão de utilizá-lo.

Além disso, é importante manter o polyfill atualizado e acompanhar as atualizações do Axios para garantir a compatibilidade contínua com browsers antigos e o melhor desempenho possível.

-------------------------------------------------------------

### Efeito colateral: Ineficiência no carregamento de dados relacionados

- identificado por: Rafael

#### Depende de:
- Conceitual
  - [Cliente-Servidor](#cliente-servidor)
  - [URL](#url)
  - [Requisições e respostas HTTP](#requisições-e-respostas-http)
- Cenários de uso
  - [Realizar operações CRUD em um servidor](#cenário-realizar-operações-crud-em-um-servidor)
  - [Manipular Headers de uma requisição](#cenário-manipular-headers-de-uma-requisição)

O uso incorreto da biblioteca Axios pode resultar em ineficiências no carregamento de dados relacionados, especialmente quando o conceito de **_lazy loading_** é utilizado para buscar objetos relacionados. Esse efeito colateral ocorre devido ao aumento do número de consultas realizadas.

Quando utilizamos o Axios com *lazy loading*, cada objeto relacionado é buscado individualmente, resultando em uma consulta separada para cada objeto. Essa abordagem pode se tornar problemática quando lidamos com grandes conjuntos de dados, pois o número de consultas aumenta proporcionalmente à quantidade de objetos relacionados.

Considere um exemplo de um sistema de blog com posts e autores, onde cada post possui um relacionamento de chave estrangeira com o autor correspondente. Se utilizarmos o Axios com *lazy loading* para buscar os autores de cada post, será emitida uma consulta separada para cada post, resultando em N+1 consultas (1 consulta para buscar a lista de posts e N consultas para buscar os autores, sendo N o número de posts).

Para mitigar o problema de ineficiência no carregamento de dados relacionados, existem diferentes abordagens que podem ser adotadas, dependendo do contexto da aplicação.

#### Opção 1: Uso do parâmetro `include`

Uma opção oferecida pela biblioteca Axios é utilizar o parâmetro `include` para passar os parâmetros de busca dos objetos relacionados em uma única requisição. Dessa forma, é possível reduzir significativamente o número de consultas realizadas, melhorando a eficiência do carregamento dos dados relacionados.

Aqui está um exemplo de como utilizar o Axios com o parâmetro `include`:

```javascript
axios.get('https://api.exemplo.com/posts', {
  params: {
    include: 'author' // Inclui os dados do autor na mesma requisição
  }
})
  .then(response => {
    console.log(response.data);
  })
  .catch(error => {
    console.error(error);
  });
```

Nesse exemplo, fazemos uma requisição GET para a URL 'https://api.exemplo.com/posts' e utilizamos o parâmetro `include` para incluir os dados do autor na mesma requisição. Com isso, podemos buscar os posts e seus autores em uma única consulta, evitando o efeito colateral de ineficiência no carregamento de dados relacionados.

#### Opção 2: Endpoint personalizado no servidor

Outra abordagem para resolver o problema é fazer uma modificação na estrutura da API para suportar uma rota específica que retorne os posts juntamente com os dados dos autores. Essa abordagem pode ser chamada de "endpoint personalizado" ou "endpoint de busca combinada".

A ideia é criar uma nova rota na API, como '/posts-with-authors', que retorne os dados dos posts e seus autores em uma única resposta. Dessa forma, em vez de realizar várias consultas separadas para buscar os autores de cada post, podemos obter todas as informações necessárias em uma única requisição.

No servidor da API, é necessário implementar a lógica para buscar os dados dos posts e realizar uma operação de junção (JOIN) para trazer os dados dos autores relacionados. Os dados formatados são então retornados como resposta à requisição HTTP.

**É importante ressaltar que essa e a solução anterior requerem alterações na implementação do lado do servidor**. Aqui está um exemplo simplificado de como a implementação poderia ser feita:

```javascript
// Servidor da API
app.get('/posts-with-authors', (req, res) => {
  // Consulta ao banco de dados para buscar os dados dos posts com os autores
  const postsWithAuthors = db.posts.findAll({
    include: {
      model: db.authors
    }
  });

  // Formatação dos dados combinados em um objeto JSON
  const formattedData = formatData(postsWithAuthors);

  // Resposta da requisição com os dados formatados
  res.json(formattedData);
});
```

No lado do cliente, podemos utilizar o Axios para fazer uma única requisição para buscar os dados dos posts e dos autores:

```javascript
axios.get('https://api.exemplo.com/posts-with-authors')
  .then(response => {
    console.log(response.data);
  })
  .catch(error => {
    console.error(error);
  });
```

Ao adotar essas abordagens, podemos obter os dados relacionados de forma mais eficiente, reduzindo o número de requisições e melhorando o desempenho geral da aplicação. É importante considerar a viabilidade e a complexidade dessas soluções, levando em conta as necessidades e os recursos disponíveis na aplicação.

É fundamental avaliar a melhor estratégia de carregamento de dados relacionados, considerando o volume de dados e a necessidade de otimização. Em alguns casos, outras técnicas de otimização, como a paginação ou o carregamento assíncrono, podem ser mais adequadas. A escolha da melhor abordagem depende do contexto específico da aplicação e dos requisitos do sistema.

-------------------------------------------------------------

### Efeito colateral: Axios e dependência de outras bibliotecas ou frameworks

- identificado por: Rafael

#### Depende de:
- Conceitual
  - Nenhum
- cenários de uso
  - Nenhum

O uso da biblioteca Axios pode exigir a dependência de outras bibliotecas ou *frameworks*. Isso pode aumentar a complexidade do projeto e introduzir uma dependência adicional, o que impacta na manutenção e compatibilidade futura do sistema.

Para mitigar esse problema, é importante avaliar cuidadosamente as dependências do projeto. Antes de adicionar uma nova biblioteca ou *framework*, é recomendado analisar sua real necessidade e considerar alternativas mais simples ou nativas.

Também é fundamental pesquisar a estabilidade, suporte e atualizações das bibliotecas ou *frameworks* escolhidos. Optar por soluções bem estabelecidas, com uma comunidade ativa e documentação abrangente, pode minimizar problemas de dependência.

Ao utilizar o Axios, é importante acompanhar as atualizações da biblioteca e estar atento a possíveis mudanças nas dependências necessárias. Manter as versões atualizadas e realizar testes de compatibilidade evita problemas decorrentes de atualizações incompatíveis entre as dependências utilizadas.

Em resumo, é essencial analisar as implicações das dependências ao usar o Axios e garantir um gerenciamento adequado delas. Planejar com antecedência, escolher soluções estáveis e manter-se atualizado são boas práticas para minimizar os efeitos colaterais relacionados à dependência de bibliotecas ou *frameworks* no contexto do uso do Axios.