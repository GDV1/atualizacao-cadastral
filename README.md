# Atualização Cadastral - Estudo de Caso com NgRx

Este projeto foi gerado com [Angular CLI](https://github.com/angular/angular-cli) na versão 15.2.11.

O projeto consiste em um estudo de caso de uma aplicação responsável pela atualização de cadastro de clientes. O objetivo dessa implementação é validar conceitos sobre a biblioteca de gerenciamento de estado NgRx.


## Servidor de desenvolvimento 

Execute o comando `ng serve` para subir o servidor de desenvolvimento local. Navegue até `http://localhost:4200/`. A aplicação irá recarregar automaticamente sempre que um arquivo for alterado.

## Estrutura de código

Execute `ng generate component component-name` para gerar um novo componente. Você também pode usar `ng generate directive|pipe|service|class|guard|interface|enum|module`.

## Build

Execute `ng build` para buildar o projeto. Os arquivos do build serão salvos na pasta `dist/`.

## Rodando testes unitários

Execute `ng test` para rodar os testes unitários do projeto usando o test runner [Karma](https://karma-runner.github.io).

## Rodando testes end-to-end

Rode o comando `ng e2e` para executar os testes end-to-end através da plataforma de sua preferência. Para usar este comando, primeiro você precisa adicionar um pacote que implemente recursos de testes end-to-end.

## Mais ajuda

Para obter mais ajuda sobre o Angular CLI use o comando `ng help` ou navegue até a página [Angular CLI Overview and Command Reference](https://angular.io/cli).


# Organização dos diretórios

Temos diversas formas de montar uma SPA em Angular e para os nossos projetos vamos procurar seguir um padrão em sua estrutura. Basicamente, todo nosso fonte esta dentro do diretório /src. É a partir dele que o build acontece. Abaixo uma recomendação de como devemos estuturar nossos arquivos utilizando a biblioteca de Gerenciamento de Estado NgRx.

```
...
|-- /mock                           # Arquivos usados para executar o projeto localmente com mock, simulando requisições à API/BFF
|-- /src                            # Raiz do projeto
    |-- /app                        # Fontes relacionados a aplicação
        |-- /constants              # Arquivos com Enums e Contantes usados no projeto
        |-- /core                   # Interceptors, services e guardas de rotas e componentes padrão para o projeto (pode ser revisto e readequado)
            |-- /error              # Módulos e Interceptors relacionados aos fluxos de erro
            |-- /services           # Serviços responsáveis por lógica de negócio comuns a toda a aplicação (comunicação com API e regras internas de componentes)
            |-- /guards             # Guardas de rotas responsáveis pela de autenticação
            |-- /interceptors       # HTTP Interceptors
            |-- /interfaces         # Interfaces do core
            |-- /constants          # Constantes do core referente a código, como por exemplo, tipo de header a serem concatenado pelas requisições. Títulos, textos, placeholders e demais redações devem ser colocados no arquivo de traducao `/src/assets/i18n/<lang>.json`
        
        |-- /modules                # Módulos que compõem a aplicação, como home, cadastro, login, autenticação etc. Funciona como um componente container de módulo para cada tela.
            |-- /books              # Pasta raiz do modulo - contendo seu module e routing.module
                |-- /actions        # Actions do estado a serem disparadas pelas pages do módulo 
                |-- /components     # Sub-componentes a serem instanciados nas pages do módulo e só devem se comunicar com eles através de @Input e @Output
                |-- /constants      # Constantes do módulo referente a código, como por exemplo, o nome de classes e ids a serem usadas pelos scripts. Títulos, textos, placeholders e demais redações devem ser colocados no arquivo de traducao `/src/assets/i18n/<lang>.json`
                |-- /pages          # Componentes que controlam a lógica da tela, se comunicam com o estado da aplicacao através do disparo de Actions ou do consumo de Selectors e que, por boas práticas, podem ter seu código separado em sub-componentes na pasta `/components` no mesmo nivel da `/pages`
                |-- /directives     # Diretivas utilizadas exclusivamente pelo módulo - diretivas compartilhadas por mais de um módulo devem estar na pasta `shared/directives`
                |-- /effects        # Effects que podem ser disparados pelas Actions do módulo 
                |-- /interfaces     # Interfaces do módulo
                |-- /mocks          # Mocks do módulo - geralmente utilizados por testes unitários
                |-- /pipes          # Pipes utilizados exclusivamente pelo módulo - pipes compartilhados por mais de um módulo devem estar na pasta `shared/pipes`
                |-- /reducers       # Reducers a serem disparados pelas Actions do módulo e Selectors para serem consumidos pelos componentes da pasta pages 
                |-- /services       # Service que irá fazer os disparos das requisicoes HTTP do módulo
                |-- /validators     # Validators para serem usados pelos formulários do módulo - validators que serao compartilhados entre os modulos devem estar na pasta `shared/validators`
        |-- /reducers               # Raiz do estado a ser gerenciado pelos módulos
        |-- /shared                 # Artefatos de uso comum. Não se deve criar modulos dentro da pasta shared, pois as telas (mesmo as que serão destino de diversos módulos) devem estar dentro de pastas containêres nos módulos na pasta `/modules`
            |-- /actions            # Actions para o estado `shared` 
            |-- /components         # Componentes que podem ser instanciados/disparados por diversos módulos, como por exemplo, o componente de loading.
            |-- /constants          # Constantes compartilhadas para todos os módulos referente a código. Titulos, textos, placeholders e demais redações devem ser colocados no arquivo de tradução `/src/assets/i18n/<lang>.json`
            |-- /directives         # Diretivas compartilhadas com todos os módulos
            |-- /interfaces         # Interfaces compartilhadas com todos os módulos
            |-- /mocks              # Mocks compartilhados com todos os módulos
            |-- /pipes              # Pipes compartilhados com todos os modulos
            |-- /reducers           # Reducers para serem disparados pelas Actions do `shared` e Selectors para serem consumidos por todos os módulos 
            |-- /services           # Servicos compartilhados com todos os módulos, como por exemplo, um servico de utilidades/ferramentas de script genéricos
            |-- /validators         # Validators pra formulários compartilhados com todos os módulos
    |-- /assets                     # Estáticos específicos do projeto.
        |-- /config                 # Diretório do estático de configuração
        |-- /i18n                   # Diretório com os json contendo títulos, textos, placeholders e demais redações
...
```

# Instalação NgRx

A seguir você encontra os comandos necessários para instalação da biblioteca de Gerenciamento de estado NgRx. Para termos todas as funcionalidades disponíveis, precisamos, primeiramente, adicionar o pacote NgRx Store com o seguinte comando:


`ng add @ngrx/store@latest --no-minimal`


Em seguida, instalamos outros quatro pacotes que nos auxiliarão no desenvolvimento da nossa aplicação com NgRx. Estamos falando dos pacotes NgRx Effects, NgRx Entity, NgRx Store Devtools e NgRx Schematics. Podemos adicionar todos de uma única vez ou em comandos separados. 


`npm install @ngrx/effects@latest --save`

`npm install @ngrx/entity@latest --save`

`npm install @ngrx/store-devtools@latest --save`

`npm install @ngrx/schematics@latest --save-dev`


Após instalar todas as dependências necessárias, gere os módulos com os arquivos de roteamento de acordo com a estrutura recomendada acima usando os comandos da CLI do Angular. 


`ng generate module modules/books --routing true`

`ng generate component modules/books/containers/books --flat true --module modules/books/books.module.ts --style scss`


Gerando services: 


`ng generate service modules/books/services/books`


Gerando actions: 


`ng generate action books --group --path src/app/modules/books`


Gerando reducers: 


`ng generate reducer books --group --path src/app/modules/books --module books.module.ts --reducers ../../reducers/index.ts`


Gerando selectors: 


`ng generate selector books --path src/app/modules/books/reducers`


Gerando effects:


`ng generate effect books --path src/app/modules/books --group --module books.module.ts`


Após finalizar a criação da estrutura do projeto com os módulos necessários, configure o `app.module` com o `EffectsModule` adicionando a seguinte linha dentro do array de imports:


`EffectsModule.forRoot([])`

