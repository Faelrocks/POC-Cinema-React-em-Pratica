# POC-Cinema (React em Prática)

<img src="https://campinas.com.br/wp-content/uploads/2023/09/pipoca-no-cinema-com-almofada-no-chao-scaled-e1695841582534.jpg" width="600px" >

# POC-Cinema (React em Prática)

Nome: Rafael Queiroz Moraes     RA: 10441847
Nome: Fabrício de Almeida Silva RA: 10437724
Nome: Joao Vitor Macea Vinhando RA:10437139

 <!--ts-->
 
 * [Overview do projeto](#Overview)
 * [Ajustes Iniciais NextJS](#Ajustes_Iniciais)
 * [Criando Componentes - BuyButton](#BuyButton)
 * [Criando Componentes - Movie](#Componente_Movie)
   * [Layout Básico](#Layout_Movie)
   * [Inserindo informações através de um json (require)](#Require)
   * [Inserindo a tela e a leganda (useState/useEffect)](#UseState)
 * [Criando Componentes - Seats](#Assentos)
   * [Layout Básico](#Layout_Assentos)
   * [Inserindo assentos (Map)](#Map)
 * [Deixando a Página Responsiva](#Estilos)
  
 <!--te-->
#Overview

<img src="https://secure-res.craft.do/v2/35zTBcczcaXEcbgADPkKgoFxBZ8GUC6ptGooB4zwhvadzGvQgH4q4Lfbyoj3YFe76zNCrL965LWprH1FnU2oKFmCtT6b8VX9EjNpJLNBVbdmgMsn2pJ2gwfhKBjVUye1h3Gwoy8qSKw6EYNYPjoDKrdkByNA81UHkj9dkk52nNVkGAvPLjCMWxMVBWgfBoZLrvYCCBiGVkBbQGmvPxQ7853iJaJ2x7z4ea5zhJKNUAXYd5WKM" width="600px" >


## Ajustes_Iniciais
 
A estrutura básica de um projeto Next.js geralmente se parece com isso:
~~~arduino
   my-next-app/
   ├── app/
   │   ├── page.js
   │   ├── layout.js
   │   ├── globals.css
   │   └── ... (outras pastas e arquivos)
   ├── public/
   │   ├── images/
   │   └── favicon.ico
   ├── styles/
   │   └── globals.css
   ├── package.json
   ├── next.config.js
   └── README.md
~~~
### Limpando o Page JS e o global CSS/<br>
O Nex.JS vem com um layout default quando o iniciamos, algo parecido com a imagem abaixo (a depender da versão):
[imagem]

Entretanto, nosso layout será bem diferente, então podemos deletar tudo que está dentro da cláusula "Return" no arquivo "page.js",e vamos aplicar a cláusula "use client" no inicio do script:

```javascript
"use client"

export default function Home() {
  return ( 

  );
}

```

Também vamos deletar todo o código dentro do arquivo "global.css" para que aplicar a nossa estilização em fases posteriores do projeto

[imagem] 


## BuyButton

Vamos criar o nosso primeiro componente, o botão de compra de ingressos. 
Para isso, o primeiro passo é criar <b>duas pastas dentro do diretório "src/app"</b>,  uma chamada "components" e outra chamada "BuyButton", resultando no caminho "src/app/components/BuyButton". 
Depois, criamos os arquivos "index.js" e "buybutton.module.css" 

Ao abrir o arquivo "index.js" inputamos a seguinte estrutura inicial:

```javascript
"use client" //definicão entre cliente e servidor js
import styles from './buybutton.module.css' // importa o aquivo css

const BuyButton = () => { 

    return (

    )
} //arrown function de retorno - vai retornar a estrutura html

export default BuyButton //exporta este componente para que possa ser utilizado em outros componentes

```
Observação: Esta estrutura básica com a importaçao do css, Arrow Function de retorno e expot do objeto será usada para a criação de todos os demais componentes deste projeto.

Em seguida, criamos a estrutura html que o componente deve retornar para gerar o botão, com as devidas classes para estilização no css:

```javascript
"use client"
import styles from './buybutton.module.css'

const BuyButton = ({preco}) => { // a arrow function recebe a variável preço -  que seá calculada em outro componente

    return (
    <section className={styles.button}> //section inicial
        <button className={styles.buybutton}> // criação do botão
            <div>
                Comprar<br/> //texto dentro do botão
            </div>
            <div className={styles.preco}>
                R${preco} // preço total da compra - que será calculado em outro componente
            </div>
        </button>
    </section>
    )
}

export default BuyButton
```
Desta forma, já temos um botão, podemos importar este componente na página principal (arquivo "page.js") 

```javascript
"use client"
import BuyButton from "./components/BuyButton"; // importação do componente "Buy Buttons"

export default function Home() {

  return ( 
    <div>
      <BuyButton /> //tag gerada com o export do componente - desta forma, todo o código que fizemos no arquivo "index.js" é transposto para este arquivo.
    </div>
  );
}

```

Agora, já temos um botão em nossa página

[imagem]

Com isso, podemos formatá-lo através do arquivo "buybutton.module.css" conforme abaixo:

```css
.button {
    display: flex;
    justify-content: center;
}

.buybutton {
    background-color: #CD4A3E;
    color: #F0F0F0;
    padding: 12px;
    margin-top: 24px;
    font-size: 32px;
    border-radius: 12px;
    width: 354px ;
    border: none;
    cursor: pointer;
    
}

.preco {
    font-size: 16px;
    margin-top: 8
    4px;
    font-weight: 200;
}

```

Assim, temos como resultafo final:

<img src="imagens_readme/botao-final.png" width="600px" >


## Componente_Movie

## Layout_Movie

Agora, necessitamos um componente para importar as inforomações do json e criar o "corpo" da aplicação

Criamos uma basta chamada "Movie" dentro do diretório "components", em seguida criamos os arquivos "index.js" e "movie.module.css".

Assim como no componente anterior, importamos o css e criamos a estrutura padrão no arquivo "index.js"

Na construção do index.js demosntrada abaixo, temos os seguintes passos:

 1. Criamos uma constante chamada "Movie" que recebe as informaçoes referentes ao filme como título, horário, sinópise, lancamento e direcao (informações contidas no arquivo json)
 2. Retornamos o html com as devidas seções, separadas em topo da página e base da página.  
 3. A base da página foi separada entre "sala" para representar a sala de cinema, e "texto", onde estarão as informacoes sobre o filme (nosso foco nesse bolo)
 4. Os parágrafos "assentos", "tela" e "legenda" serão substituídos posteriormente, apenas represnetam o posicionamento destes elementos
 5. Utilizamos as tags "styles" para a formatação com o css
 6. Utilizamos as demais tags para obter as informações que necessitamos do arquivo json direto da oágina principal

```javascript

"use client"
import styles from './movie.module.css'

const Movie = ({ titulo, horario, sinopise, lancamento, direcao }) => {

    return (
        <section className={styles.movie}>
            <div className={styles.top}>
                <h1 className={styles.titulo}><b>{titulo}</b><br /></h1>
                <h2 className={styles.horario}>{horario}</h2>
            </div>
            <section className={styles.bottom}>
                <div className={styles.room}>
                    <div className={styles.seat}>
                        <p>assentos</p>
                    </div>
                    <div className={styles.tela}>
                        <p>Tela</p>
                        <p>Legenda</p>
                    </div>
                </div>
                <div className={styles.texto}>
                    <p className={styles.sin}><b>Sinopise do filme</b><br /> {sinopise} </p>
                    <p className={styles.data}><b>Data de lancamento</b><br /> {lancamento} </p>
                    <p className={styles.dir}><b>Direcao</b><br /> {direcao} </p>
                </div>
            </section>
        </section>

    )
}

export default Movie

```

## Require

Para obtermos as informações sobre o filme, fizemos as seguintes alterações no arquivo "page.js":

 1. Importamos o componente "movies" no inicio da página
 2. Criamos uma constante chamda "movies" que recebe as informações do arquivo json através da função "require"
 3. Dento da função "Home()", inserimos a nova tag html "<Movie />" e associamos cada valor esperado com o json

```javascript

"use client"
import BuyButton from "./components/BuyButton";
import Movie from "./components/Movie";

const movies = require("./components/filme.json");

export default function Home() {

  return ( 
    <div>
      <section>
        <Movie titulo={movies.titulo} horario={movies.horario} sinopise={movies.sinopse} lancamento={movies.dataLancamento} direcao={movies.direcao} />
      </section>
      <BuyButton />
    </div>
  );
}

```
Exemplificando: para extrair a sinópise do filme, utilizamos o argumento "movies.sinopise", a variável "movies" contem em vetor todas as infomações do arquivo json, quando utilizamos o ".sinopise" conseguimos acessar a informação dentro do arquivo (como se fosse um ínfice).

Repare que o arquivo possui o formato "chave":"valor" de todas as informações que necessitamos:

```java

{
  "titulo": "A Forja",
  "sinopse": "Um ano depois de encerrar o ensino médio, o jovem Isaías Wright não tem planos para o futuro e é desafiado por sua mãe solo e um empresário de sucesso a começar a traçar um rumo melhor para sua vida. Ele passa a ser discipulado pelo seu novo mentor, conta com orações de sua mãe e de uma guerreira de orações, Dona Clara, e começa a descobrir o propósito de Deus para sua vida.",
  "dataLancamento": "26 de setembro de 2024 (Brasil)",
  "direcao": "Alex Kendrick",
  "horario": "16:40",
}

```

Por fim, formatamos com o seginte CSS:

```CSS

.movie {
    font-family: sans-serif;
    color: white;
    display: flex;
    flex-direction: column;
}

.top {
    display: flex;
    align-items: center;
    flex-direction: column;
    justify-content: center;
    font-size: 40px;
    font-weight: 700;
    text-align: center;
}

.horario {
    font-size: 32px;
    font-weight: 200;
    margin-top: -48px;
}

.bottom {
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: space-around;
}

.room {
    display: block;
    flex-direction: column;
    align-items: center;
    justify-content: space-around;
    padding: 24px;

}

.seat {
    display: flex;
    flex-direction: row;
    justify-content: center;
    align-items: center;
    flex-wrap: wrap;
    width: 45%;
    margin: 0 27%;
}

.tela{
    display: flex;
    flex-direction: column;
    padding: 24px;
    align-items: center;
}


.texto {
    display: flex;
    flex-direction: column;
    justify-content: flex-start;
    font-size: 18px;
    font-weight: lighter;
    align-items: baseline;
    margin-right: 10%;
    
}

.sin {
    padding-bottom: 10%;
}

.data {
    padding-bottom: 10%;
}

.dir {
    padding-bottom: 10%;
}

```

Assim, temos o seguinte resultado:

<img src="imagens_readme/movie-component.png" width="600px" >


## UseState

Como nossa aplicação deve ser dinâmica com o esquema de cor do usuário, utilizaremos o "useState" e o "useEffect" para conseguir representar esta dinâmica

Sobre o useState e useEffect:

O "useState" é um hook do React que permite adicionar estado às funções componentes, ele é para gerenciar valores dinâmicos que podem mudar ao longo do ciclo de vida de um componente.
Em outras palavras, se um componente muda de estado na aplicação (precisa ser renderizado novamente), devemos utilizar o "useState".

Já o "useEffect" é um hook do React que permite lidar com efeitos colaterais em componentes funcionais. Ele é usado para executar código em momentos específicos do ciclo de vida do componente, como após a renderização ou quando determinadas dependências mudam. Em outras palavras, se um componente tem um gatilho de alteração (sem necessariamente precisar ser renderizado novamente), devemos utilizar o "useEffect".


Realizamos as seguintes alterações no aquivo "index.js" (do componente Movie):

 1. Importamos o "useState" e "useEffect" do react
 2. Inicializamos o useState logo no inicio da constate "Movie" com uma string vazia para ambos os casos (tela e subtítulo)
 3. Utilizamos a função "window.matchMedia" para identificar o esquema de cores que o usuário está utilizando
 4. Realizamos um "if statement" para selecionar as imagens que devem ser utilizadas em cada ocasião (tema escuro/tema claro)
 5. Utilizamos o "useEffect" para atualizar automaticamente as imagens caso o usuário altere a cor enquanto utiliza a aplicação (sem necessidade de recarregar a página)
 6. Realizamos isso através do método "addEventListener" que observa estas alterações do usuário
 7. Com isso, geramos "gatilhos" de alteração, onde o componente é alterado e renderizado novamente conforme a utilização o usuário
 8. Substituímos o texto pelas tags no html (screenImage, subtitleImage)


```javascript

"use client"
import { useState, useEffect } from 'react'
import styles from './movie.module.css'
import Seats from '../Seats'

const movies = require("../filme.json")

const Movie = ({ titulo, horario, sinopise, lancamento, direcao }) => {
    const [screenImage, setScreenImage] = useState("")
    const [subtitleImage, setSubtitleImage] = useState("")

    const updateScreenImage = (screen, subtitle) => {

        const corTema = window.matchMedia('(prefers-color-scheme: dark)')

        if (corTema.matches === true) {
            screen = "/images/escuro/tela.png"
            subtitle = "/images/escuro/legendas.png"

        } else {
            screen = "/images/claro/tela.png"
            subtitle = "/images/claro/legendas.png"
        }

    }

    useEffect(() => {
        const corTema = window.matchMedia('(prefers-color-scheme: dark)')

        // Define a imagem inicialmente
        updateScreenImage()

        // Adiciona um listener para monitorar alterações no esquema de cores
        const handleThemeChange = () => updateScreenImage()
        corTema.addEventListener("change", handleThemeChange)

        // Remove o listener ao desmontar o componente
        return () => {
            corTema.removeEventListener("change", handleThemeChange)
        }
    })

    return (
        <section className={styles.movie}>
            <div className={styles.top}>
                <h1 className={styles.titulo}><b>{titulo}</b><br /></h1>
                <h2 className={styles.horario}>{horario}</h2>
            </div>
            <section className={styles.bottom}>
                <div className={styles.room}>
                    <div className={styles.seat}>
                        <p>assentos</p>
                    </div>
                    <div className={styles.tela}>
                        <p>Tela</p>
                        <img src={screenImage} />
                        <img src={subtitleImage} />
                    </div>
                </div>
                <div className={styles.texto}>
                    <p className={styles.sin}><b>Sinopise do filme</b><br /> {sinopise} </p>
                    <p className={styles.data}><b>Data de lancamento</b><br /> {lancamento} </p>
                    <p className={styles.dir}><b>Direcao</b><br /> {direcao} </p>
                </div>
            </section>
        </section>

    )
}

export default Movie

```

Com isso temos o seguinte resultado

Tema escuro:

<img src="imagens_readme/movie-component-dark.png" width="600px" >

Tema claro:

<img src="imagens_readme/movie-component-light.png" width="600px" >


## Assentos

## Layout_Assentos

Agora, necessitamos um componente para importar as inforomações do json e criar o "corpo" da aplicação

Criamos uma basta chamada "Seats" dentro do diretório "components", em seguida criamos os arquivos "index.js" e "seats.module.css".

Assim como no componente anterior, importamos o css e criamos a estrutura padrão no arquivo "index.js"

Na construção do index.js demosntrada abaixo, temos os seguintes passos:

 1. Importamos o "useClient" e o "useState"
 2. Criamos uma constante chamada "Seats" que recebe o número do assento e o status dele (se está vazio ou não)
 3. Definimos a imagem padrão inicial
 4. Definimos que a variável de compra do assento é falsa
 5. Utilizamos o mesmo conceito do tópico anterior para alterar as imagens do assento quando o usuário troca o esquema de cores, bem como definir a imagem diferente para os assentos ocupados
 6. A constante "vacantSeat" é resposnsável por alterar o estado do assento (se está livre = true, se está ocupado = false)
 7. A constante selectSeat é responsável por alterar a figura e deixar o assento na cor vermelha quando realizamos a seleção, bem como impedir a selecão de assentos préviamente indisponíveis através do json
 8. Dentro do "Return", utiulizamos a função "onLoadedData" para importar as infomações do json e definir os assentos livres e ocupados
 9. Utilizamos a função "onClick" para que a função "selectSeat" execute ao clique do usuário

```javascript

"use client"
import { useState, useEffect } from 'react'
import style from "./seat.module.css"

const Seats = ({ number, vacant }) => {
    const [seatVacant, setSeatStatus] = useState(vacant)
    const [seatImage, setSeatImage] = useState("/images/escuro/assentolivre.png")
    const [seatBuy, setSeatBuy] = useState(false)



    const updateThemeImage = (seat) => {

        const corTema = window.matchMedia('(prefers-color-scheme: dark)')

        if (corTema.matches === true) {
            seat = "/images/escuro/assentolivre.png"

            if (seatVacant === false) {
                seat = "/images/escuro/assentoocupado.png"
            }
            } else {
                seat = "/images/claro/assentolivre.png";
                if (seatVacant === false) {
                    seat = "/images/claro/assentoocupado.png"
                }
            }

        setSeatImage(seat)
    }

    useEffect(() => {
        const corTema = window.matchMedia('(prefers-color-scheme: dark)')

        // Define a imagem inicialmente
        updateThemeImage()

        // Adiciona um listener para monitorar alterações no esquema de cores
        const handleThemeChange = () => updateThemeImage()
        corTema.addEventListener("change", handleThemeChange)

        // Remove o listener ao desmontar o componente
        return () => {
            corTema.removeEventListener("change", handleThemeChange)
        }
    }, [seatVacant]) // Atualiza também se o estado `seatVacant` mudar

    const vacantSeat = () => {
        setSeatStatus(seatVacant)
    }

    const selectSeat = (seat) => {

        if (seatVacant == true && seatBuy == false) {
            seat = "/images/assentoselecionado.png"
            setSeatImage(seat)
            setSeatBuy(true)
        } else if (seatVacant == true && seatBuy == true) {
            updateThemeImage(seat)
            setSeatBuy(false)
        }

    }



    return (
        <section className={style.seat}>
            <img src={seatImage} onLoadedData={vacantSeat} onClick={selectSeat} />
        </section>
    )
}

export default Seats

```

Formatamos também o CSS para que os assentos mantenham o padrão do figma

```CSS

.seat {
    display: flex;
    flex-direction: row;
    justify-content: center;
    align-items: center;
    padding: 8px;
}

```




## Map

Para que os assentos sejam renderizados na tela, temos que alterar o componente "Movies"

Precisamos transpor o vetor do json para a nossa aplicação, faremos isso através da função map.

Sobre a função map:

Portanto, no componente Movie, dentro do html, inserimos a função map, que nos retorna o número e se o assento está disponível ou não através de um vetor.
Desta forma, nossa aplicação renderiza todas os assentos do Cinema

```javascript
                    <div className={styles.seat}>
                        {movies.assentos.map(vacant => <Seats
                            number={vacant.numero}
                            vacant={vacant.disponivel}
                        />
                        )}
                    </div>
```

Como resultado final, temos os layouts:

Tema escuro:

<img src="imagens_readme/seat-component-dark.png" width="600px" >

Tema claro:

<img src="imagens_readme/seat-component-light.png" width="600px" >

## Estilos

Por último, vamos formatas os arquivos CSS para que nosso projeto seja responsivo para SmartPhones (atraves de media queries), e para que as fontes e textos fiquem adequados

global.css

```CSS

:root {
    color-scheme: normal;
}

@media (prefers-color-scheme: dark) {

body {
    background-color: #1A1A24;
    color: #F0F0F0;
    margin: 12px;
}
}

@media (prefers-color-scheme: light) {
    
body {
    background-color: #F0F0F0;
    color: #1A1A24;
    margin: 12px;
}

}

```

movioes.module.css 

```CSS


.movie {
    font-family: sans-serif;
    color: white;
    display: flex;
    flex-direction: column;
}

.top {
    display: flex;
    align-items: center;
    flex-direction: column;
    justify-content: center;
    font-size: 40px;
    font-weight: 700;
    text-align: center;
}

.horario {
    font-size: 32px;
    font-weight: 200;
    margin-top: -48px;
}

.bottom {
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: space-around;
}

.room {
    display: block;
    flex-direction: column;
    align-items: center;
    justify-content: space-around;
    padding: 24px;

}

.seat {
    display: flex;
    flex-direction: row;
    justify-content: center;
    align-items: center;
    flex-wrap: wrap;
    width: 45%;
    margin: 0 27%;
}

.tela{
    display: flex;
    flex-direction: column;
    padding: 24px;
    align-items: center;
}


.texto {
    display: flex;
    flex-direction: column;
    justify-content: flex-start;
    font-size: 18px;
    font-weight: lighter;
    align-items: baseline;
    margin-right: 10%;
    
}

.sin {
    padding-bottom: 10%;
}

.data {
    padding-bottom: 10%;
}

.dir {
    padding-bottom: 10%;
}

@media (prefers-color-scheme: light) {
    
    .movie {
        color: black;
    }
}

@media (max-width: 768px) {
    .bottom {
        display: flex;
        flex-direction: column;
    }
    .room {
        display: block;
        flex-direction: column;
        align-items: center;
        justify-content: space-evenly;
        padding: 0px;
    
    }
    
    .seat {
        display: flex;
        flex-direction: row;
        justify-content: center;
        align-items: center;
        flex-wrap: wrap;
        width: 400px;
        margin: 0;
        
    }
    
    .texto {
        display: none;
    }
}

```

Resultado Final:

Tema escuro:

<img src="imagens_readme/smartphone-dark.png" width="600px" >

Tema claro:

<img src="imagens_readme/smartphone-light.png" width="600px" >

