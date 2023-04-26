Jogo da Cobrinha
Este projeto é uma implementação simples do clássico jogo da cobrinha usando HTML, CSS e JavaScript para o curso do IBRF com a Gama Academy.

Requisitos
Navegador web moderno com suporte a HTML5 e JavaScript.
Como usar
Clone ou baixe este repositório para o seu computador.
Abra o arquivo index.html no seu navegador.
Funcionalidades
A cobra cresce ao comer a comida (representada por um quadrado vermelho) no tabuleiro.
A cobra morre e o jogo reinicia se a cabeça da cobra colidir com seu corpo ou sair do tabuleiro.
Controles simples usando as setas do teclado para movimentar a cobra.
Estrutura do projeto
O projeto consiste nos seguintes arquivos:

index.html: Contém a estrutura básica do HTML e inclui o arquivo de estilo CSS e o arquivo de script JavaScript.
style.css: Define os estilos para o jogo, como o plano de fundo da página e a borda do canvas.
script.js: Contém a lógica do jogo, incluindo a renderização da cobra e da comida, detecção de colisões e manipulação de eventos de teclado.
Código explicado
HTML
A estrutura do HTML é simples, consistindo em um elemento canvas onde o jogo será renderizado.

<!DOCTYPE html>
<html lang="en">
<head>
    ...
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <canvas id="game-board" width="400" height="400"></canvas>
    <script src="script.js"></script>
</body>
</html>
CSS
O arquivo style.css contém estilos para centralizar o canvas na tela e definir a borda do canvas.

body {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    background-color: #222;
    margin: 0;
}

canvas {
    border: 1px solid #fff;
}
JavaScript
O arquivo script.js contém a lógica do jogo. Algumas das principais funções incluem:

main: Função principal que é chamada a cada frame do jogo, manipulando a movimentação da cobra, a verificação de colisões e a renderização do jogo.
drawSnake e drawFood: Funções para desenhar a cobra e a comida no canvas.
moveSnake: Função que atualiza a posição da cobra com base em sua direção atual.
checkSnakeCollision e checkFoodCollision: Funções que verificam se a cobra colidiu consigo mesma ou com a comida.
growSnake: Função que aumenta o tamanho da cobra ao comer a comida.
generateFood: Função que gera uma nova posição para a comida no tabuleiro.
resetGame: Função que reinicia o jogo quando a cobra morre.
const canvas = document.getElementById('game-board');
const ctx = canvas.getContext('2d');

const tileSize = 20;
const numRows = canvas.height/tileSize;
const numCols = canvas.width/tileSize;

let snake = [{ x:10, y:10 }];
let food = { x:15, y:15 };
let dx = 0;
let dy = 0;

document.addEventListener('keydown', (event) => {
    if(event.key === 'ArrowUp' && dy === 0){
        dx = 0;
        dy = -1;
    }
    if(event.key === 'ArrowDown' && dy === 0){
        dx = 0;
        dy = 1;
    }
    if(event.key === 'ArrowLeft' && dx === 0){
        dx = -1;
        dy = 0;
    }
    if(event.key === 'ArrowRight' && dx === 0){
        dx = 1;
        dy = 0;
    }
})

function main(){
    setTimeout(() => {
        moveSnake()
        checkSnakeCollision()
        checkFoodCollision()
        clearCanvas()
        drawSnake()
        drawFood()
        main()
    }, 100)
}

function drawSnake(){
    ctx.fillStyle = 'green'
    snake.forEach((segment) => {
        ctx.fillRect(segment.x*tileSize, segment.y*tileSize, tileSize, tileSize);
        ctx.strokeStyle = 'black';
        ctx.strokeRect(segment.x*tileSize, segment.y*tileSize, tileSize, tileSize);
    })
}

function drawFood(){
    ctx.fillStyle = 'red';
    ctx.fillRect(food.x * tileSize, food.y * tileSize, tileSize, tileSize)
}

function moveSnake(){
    const headSnake = { x: snake[0].x + dx, y: snake[0].y + dy };
    snake.unshift(headSnake)
    snake.pop()
}

function checkFoodCollision(){
    const positionX = snake[0].x === food.x;
    const positionY = snake[0].y === food.y;
    if(positionX && positionY){
        generateFood();
        growSnake();
    }
}

function growSnake(){
    const tailSnake = { ...snake[snake.length -1]}
    snake.push(tailSnake)
}

function generateFood(){
    food.x = Math.floor(Math.random() * numCols)
    food.y = Math.floor(Math.random() * numRows)
}

function checkSnakeCollision(){
    if( 
        snake[0].x < 0 || 
        snake[0].x >= numCols || 
        snake[0].y < 0 || 
        snake[0].y >= numRows
    ){
        resetGame()
    }
    for(let i = 1; i < snake.length; i++){
        const positionX = snake[i].x === snake[0].x;
        const positionY = snake[i].y === snake[0].y;
        if(positionX && positionY){
            resetGame()
        }
    }
}

function resetGame() {
    snake = [{ x:10, y:10 }];
    food = { x:15, y:15 };
    dx = 0;
    dy = 0;
}

function clearCanvas(){
    ctx.fillStyle = 'black';
    ctx.fillRect(0, 0, canvas.width, canvas.height);
}

main()


Créditos
Hugo Moura,
Claudio Rapôso,
Instituto BRF Gama Academy.