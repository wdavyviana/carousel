# Carousel
Interactive carousel of icons, an element designed to be used in different ways within a website
Claro, aqui está a explicação do código em formato Markdown:

## Explicação do Código JavaScript para um Carousel Infinito

#### 1. Seleção de Elementos e Inicialização
```javascript
const wrapper = document.querySelector(".wrapper");
const carousel = document.querySelector(".carousel");
const firstCardWidth = carousel.querySelector(".card").offsetWidth;
const arrowBtns = document.querySelectorAll(".wrapper i");
const carouselChildrens = [...carousel.children];
let isDragging = false, isAutoPlay = true, startX, startScrollLeft, timeoutId;
```
- **Seleção de Elementos**: Os elementos principais do DOM são selecionados:
  - `.wrapper`: Contêiner que envolve o carousel.
  - `.carousel`: O próprio carousel onde os itens são exibidos.
  - `firstCardWidth`: Largura do primeiro cartão dentro do carousel.
  - `arrowBtns`: Botões de seta usados para navegação no carousel.
  - `carouselChildrens`: Lista dos filhos do carousel, convertida em um array usando spread operator (`...`).

- **Variáveis de Controle**: Variáveis para controlar o estado do arrasto (`isDragging`), a reprodução automática (`isAutoPlay`), e outras variáveis relacionadas ao arrasto (`startX`, `startScrollLeft`, `timeoutId`) são inicializadas.

#### 2. Configuração do Carousel Infinito
```javascript
let cardPerView = Math.round(carousel.offsetWidth / firstCardWidth);

carouselChildrens.slice(-cardPerView).reverse().forEach(card => {
    carousel.insertAdjacentHTML("afterbegin", card.outerHTML);
});

carouselChildrens.slice(0, cardPerView).forEach(card => {
    carousel.insertAdjacentHTML("beforeend", card.outerHTML);
});

carousel.classList.add("no-transition");
carousel.scrollLeft = carousel.offsetWidth;
carousel.classList.remove("no-transition");
```
- **Cálculo de Cartões por Visualização**: `cardPerView` é calculado com base na largura atual do carousel dividida pela largura do primeiro cartão.
- **Inserção de Cartões Duplicados**: Os últimos `cardPerView` cartões são clonados e inseridos no início do carousel (`afterbegin`) para permitir rolagem infinita.
- **Ajuste Inicial de Rolagem**: A classe `no-transition` é temporariamente adicionada ao carousel para ajustar a posição inicial de rolagem e esconder os cartões duplicados no início.

#### 3. Adição de Event Listeners
```javascript
arrowBtns.forEach(btn => {
    btn.addEventListener("click", () => {
        carousel.scrollLeft += btn.id == "left" ? -firstCardWidth : firstCardWidth;
    });
});

const dragStart = (e) => {
    isDragging = true;
    carousel.classList.add("dragging");
    startX = e.pageX;
    startScrollLeft = carousel.scrollLeft;
}

const dragging = (e) => {
    if(!isDragging) return;
    carousel.scrollLeft = startScrollLeft - (e.pageX - startX);
}

const dragStop = () => {
    isDragging = false;
    carousel.classList.remove("dragging");
}

const infiniteScroll = () => {
    if(carousel.scrollLeft === 0) {
        carousel.classList.add("no-transition");
        carousel.scrollLeft = carousel.scrollWidth - (2 * carousel.offsetWidth);
        carousel.classList.remove("no-transition");
    }
    else if(Math.ceil(carousel.scrollLeft) === carousel.scrollWidth - carousel.offsetWidth) {
        carousel.classList.add("no-transition");
        carousel.scrollLeft = carousel.offsetWidth;
        carousel.classList.remove("no-transition");
    }
    clearTimeout(timeoutId);
    if(!wrapper.matches(":hover")) autoPlay();
}

const autoPlay = () => {
    if(window.innerWidth < 800 || !isAutoPlay) return;
    timeoutId = setTimeout(() => carousel.scrollLeft += firstCardWidth, 1000);
}
```
- **Event Listeners para Botões de Setas**: Cada botão de seta (`arrowBtns`) tem um event listener para detectar o clique e ajustar a posição de rolagem do carousel para a esquerda ou direita.
- **Event Listeners para Arrastar**: `mousedown`, `mousemove` e `mouseup` no carousel permitem arrastar manualmente o carousel. `dragStart`, `dragging`, e `dragStop` controlam o comportamento do arrasto.
- **Funções de Controle de Rolagem Infinita**: `infiniteScroll` ajusta a posição de rolagem do carousel para simular um efeito de rolagem infinita quando atinge os extremos. `autoPlay` inicia a reprodução automática do carousel a cada segundo, se a largura da janela for maior que 800 pixels e `isAutoPlay` for verdadeiro.

#### 4. Execução Automática e Event Listeners Finais
```javascript
autoPlay();
carousel.addEventListener("mousedown", dragStart);
carousel.addEventListener("mousemove", dragging);
document.addEventListener("mouseup", dragStop);
carousel.addEventListener("scroll", infiniteScroll);
wrapper.addEventListener("mouseenter", () => clearTimeout(timeoutId));
wrapper.addEventListener("mouseleave", autoPlay);
```
- **Execução Automática Inicial**: `autoPlay()` é chamado inicialmente para iniciar a reprodução automática, se as condições forem atendidas.
- **Event Listeners Finais**: Event listeners adicionados para lidar com eventos de mouse (`mousedown`, `mousemove`, `mouseup`) no carousel e eventos de rolagem. Event listeners adicionados ao `.wrapper` para pausar (`mouseenter`) ou retomar (`mouseleave`) a reprodução automática conforme necessário.

### Resumo
Este código JavaScript implementa um carousel infinito responsivo, permitindo navegação manual através de arrasto e botões de seta, além de suportar reprodução automática. Ele é projetado para oferecer uma experiência de usuário contínua, ajustando dinamicamente a posição de rolagem para simular um carousel que nunca termina.
