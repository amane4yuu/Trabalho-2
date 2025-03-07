# Trabalho-2

# alunos:
Maria Eduarda de Sousa Rocha 557456  

Pedro César Duarte Pinto Silva 554618

Antonio Emanuel Abreu da Silva 561528

# Questão 2
 2° Questão

<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Carrossel Simples</title>
    <style>
        .carrossel {
            width: 80%;
            margin: 0 auto;
            position: relative;
            overflow: hidden;
        }
        .carrossel img {
            width: 100%;
            height: auto;
            display: none;
        }
        .carrossel img.ativo {
            display: block;
        }
        .navegacao {
            text-align: center;
            margin-top: 10px;
        }
        .navegacao button {
            padding: 10px 20px;
            margin: 0 5px;
            cursor: pointer;
        }
    </style>
</head>
<body>

<div class="carrossel">
    <img src="imagem1.jpg" alt="Imagem 1" class="ativo">
    <img src="imagem2.jpg" alt="Imagem 2">
    <img src="imagem3.jpg" alt="Imagem 3">
    <img src="imagem4.jpg" alt="Imagem 4">
</div>

<div class="navegacao">
    <button onclick="anterior()">Anterior</button>
    <button onclick="proximo()">Próximo</button>
</div>

<script>
    let imagens = document.querySelectorAll('.carrossel img');
    let index = 0;

    function mostrarImagem(n) {
        for (let i = 0; i < imagens.length; i++) {
            imagens[i].classList.remove('ativo');
        }
        imagens[n].classList.add('ativo');
    }

    function proximo() {
        index = (index + 1) % imagens.length;
        mostrarImagem(index);
    }

    function anterior() {
        index = (index - 1 + imagens.length) % imagens.length;
        mostrarImagem(index);
    }

    setInterval(proximo, 3000); // Troca de imagem a cada 3 segundos
</script>

</body>
</html>


# Questão 3
HTML
<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Avaliação de Imagens com IndexedDB</title>
    <style>
        .container {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 20px;
        }
        .imagem {
            border: 1px solid #ccc;
            padding: 10px;
            border-radius: 5px;
        }
        .rating {
            margin-top: 10px;
        }
        .rating button {
            background-color: #f0f0f0;
            border: 1px solid #ccc;
            padding: 5px 10px;
            cursor: pointer;
        }
        .rating button.active {
            background-color: #4CAF50;
            color: white;
        }
    </style>
</head>
<body>
    <div class="container">        <div class="imagem" data-id="1">
            <img src="imagem1.jpg" alt="Imagem 1" width="200">
            <div class="rating">
                <button data-rating="1">1</button>
                <button data-rating="2">2</button>
                <button data-rating="3">3</button>
                <button data-rating="4">4</button>
                <button data-rating="5">5</button>
            </div>
        </div>
        <div class="imagem" data-id="2">
            <img src="imagem2.jpg" alt="Imagem 2" width="200">
            <div class="rating">
                <button data-rating="1">1</button>
                <button data-rating="2">2</button>
                <button data-rating="3">3</button>
                <button data-rating="4">4</button>
                <button data-rating="5">5</button>
            </div>
        </div>
        <!-- Adicione mais imagens aqui -->
    </div>

    <script>
        // Código JavaScript será inserido aqui
    </script>
</body>
</html>
_____________________________________________________________________________

JS

// Abre ou cria o banco de dados
let db;
const request = indexedDB.open("AvaliacoesDB", 1);

request.onupgradeneeded = function (event) {
db = event.target.result;
    // Cria uma object store (tabela) para armazenar os ratings
    if (!db.objectStoreNames.contains("ratings")) {
        db.createObjectStore("ratings", { keyPath: "id" });
    }
};

request.onsuccess = function (event) {
    db = event.target.result;
    carregarRatings(); // Carrega os ratings ao inicializar a página
};

request.onerror = function (event) {
    console.error("Erro ao abrir o banco de dados:", event.target.error);
};

// Função para salvar o rating no IndexedDB
function salvarRating(id, rating) {
    const transaction = db.transaction("ratings", "readwrite");
    const store = transaction.objectStore("ratings");
    const request = store.put({ id, rating });

    request.onsuccess = function () {
        console.log(`Rating salvo para a imagem ${id}: ${rating}`);
    };

    request.onerror = function (event) {
        console.error("Erro ao salvar o rating:", event.target.error);
    };
}

// Função para carregar os ratings ao inicializar a página
function carregarRatings() {
    const transaction = db.transaction("ratings", "readonly");
    const store = transaction.objectStore("ratings");
    const request = store.getAll();
    request.onsuccess = function (event) {
        const ratings = event.target.result;
        ratings.forEach((item) => {
            const imagem = document.querySelector(`.imagem[data-id="${item.id}"]`);
            if (imagem) {
                const buttons = imagem.querySelectorAll(".rating button");
                buttons.forEach((button) => {
                    if (parseInt(button.getAttribute("data-rating")) === item.rating) {
                        button.classList.add("active");
                    }
                });
            }
        });
    };

    request.onerror = function (event) {
        console.error("Erro ao carregar os ratings:", event.target.error);
    };
}

// Adiciona eventos de clique aos botões de rating
document.querySelectorAll(".rating button").forEach((button) => {
    button.addEventListener("click", function () {
        const imagem = this.closest(".imagem");
        const id = imagem.getAttribute("data-id");
        const rating = parseInt(this.getAttribute("data-rating"));

        // Remove a classe "active" de todos os botões da imagem
        imagem.querySelectorAll(".rating button").forEach((btn) => {
            btn.classList.remove("active");
        });

        // Adiciona a classe "active" ao botão clicado
        this.classList.add("active");
        // Salva o rating no IndexedDB
        salvarRating(id, rating);
    });
});


# Questao 4
HTML
<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Tooltip com Detalhes da Imagem</title>
    <style>
        .container {
            display: flex;
            flex-wrap: wrap;
            gap: 20px;
            padding: 20px;
        }
        .thumbnail {
            position: relative;
            cursor: pointer;
        }
        .thumbnail img {
            width: 150px;
            height: 150px;
            object-fit: cover;
            border-radius: 5px;
        }
        .tooltip {
            position: absolute;
            bottom: 100%;
            left: 50%;
            transform: translateX(-50%);
            background-color: rgba(0, 0, 0, 0.8);
            color: white;
            padding: 5px 10px;
            border-radius: 5px;
            font-size: 14px;
            white-space: nowrap;
            opacity: 0;
visibility: hidden;
            transition: opacity 0.3s ease, visibility 0.3s ease;
        }
        .thumbnail:hover .tooltip {
            opacity: 1;
            visibility: visible;
        }
    </style>
</head>
<body>
    <div class="container">
        <!-- Thumbnails serão inseridas dinamicamente via JavaScript -->
    </div>

    <script>
        // Código JavaScript será inserido aqui
    </script>
</body>
</html>

JS
// IDs das imagens no Lorem Picsum (pode ser alterado conforme necessário)
const imageIds = [0, 1, 10, 100, 1000];

// Função para buscar detalhes da imagem
async function fetchImageDetails(id) {
    const response = await fetch(`https://picsum.photos/id/${id}/info`);
    const data = await response.json();
    return data;
}

// Função para criar uma thumbnail com tooltip
function createThumbnail(image) {
    const thumbnail = document.createElement("div");
    thumbnail.className = "thumbnail";

    const img = document.createElement("img");
    img.src = `https://picsum.photos/id/${image.id}/150/150`;
    img.alt = `Imagem ${image.id}`;

    const tooltip = document.createElement("div");
    tooltip.className = "tooltip";
    tooltip.textContent = `Autor: ${image.author}, Dimensões: ${image.width}x${image.height}`;

    thumbnail.appendChild(img);
    thumbnail.appendChild(tooltip);

    return thumbnail;
}

// Função principal para carregar as imagens
async function loadImages() {
    const container = document.querySelector(".container");

    for (const id of imageIds) {
        const imageDetails = await fetchImageDetails(id);
        const thumbnail = createThumbnail(imageDetails);
        container.appendChild(thumbnail);
    }
}

// Carrega as imagens ao inicializar a página
loadImage



# Questao 5
questão 5


A função assíncrona updateImageDetails recebe dois parâmetros: o id, que identifica a imagem, e imageElement, que é o elemento HTML da imagem.  

Primeiro, ela chama fetchImageInfo(id) usando await para buscar informações. Isso faz com que o código espere a resposta antes de seguir.  

Se os dados forem retornados, a função addTooltip(imageElement, info) adiciona um *tooltip* ao elemento, mostrando detalhes quando o usuário passa o mouse sobre a imagem.  

Se der algum erro, tipo falha na rede ou um id inválido, o catch captura e exibe a mensagem "Erro ao atualizar detalhes da imagem:" no console.  

O uso de async/await deixa o código mais fácil de entender, já que o fluxo fica mais direto. Além disso, o try...catch ajuda a evitar que erros atrapalhem o funcionamento do programa.



# Questão 6
<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Imagens com Animação</title>
  <style>
    /* Estilos para o grid */
    .grid {
      display: flex;
      justify-content: center; /* Centraliza a imagem */
      align-items: center; /* Alinha verticalmente */
      height: 100vh; /* Ocupa toda a altura da tela */
      overflow: hidden; /* Esconde imagens que saem da área */
    }

    /* Animação de fade-in */
    .fade-in {
      opacity: 0;
      animation: fadeIn 1s ease-in-out forwards;
    }

    /* Animação de fade-out */
    .fade-out {
      opacity: 1;
      animation: fadeOut 1s ease-in-out forwards;
    }

    /* Keyframes para animação de fade-in */
    @keyframes fadeIn {
      from {
        opacity: 0;
      }
      to {
        opacity: 1;
      }
    }

    /* Keyframes para animação de fade-out */
    @keyframes fadeOut {
      from {
        opacity: 1;
      }
      to {
        opacity: 0;
      }
    }

    /* Estilos para a imagem */
    .grid img {
      width: 100%;
      height: auto;
      max-width: 600px; /* Define um tamanho máximo para as imagens */
      display: block;
    }
  </style>
</head>
<body>
  <div id="grid" class="grid"></div>

  <script>
    const images = [
      "imagens/imagem1.jpg",  // Caminho relativo da imagem
      "imagens/imagem2.png",  // Caminho relativo da imagem
      "imagens/imagem3.png"  // Caminho relativo da imagem
    ];

    let currentImageIndex = 0;
    const grid = document.querySelector("#grid");

    // Função para alternar entre as imagens com animação
    function switchImage() {
      const img = document.createElement("img");
      img.src = images[currentImageIndex];
      img.alt = `Imagem ${currentImageIndex + 1}`;

      // Remover a imagem anterior (se houver)
      const currentImage = grid.querySelector("img");
      if (currentImage) {
        currentImage.classList.add("fade-out"); // Adiciona animação de fade-out
        // Espera o tempo da animação de fade-out para remover a imagem
        setTimeout(() => {
          currentImage.remove(); // Remove a imagem antiga
        }, 1000); // Tempo da animação de fade-out
      }

      // Adiciona a nova imagem com animação de fade-in
      grid.appendChild(img);
      img.classList.add("fade-in");

      // Atualiza o índice para a próxima imagem
      currentImageIndex = (currentImageIndex + 1) % images.length;
    }

    // Alterar as imagens a cada 3 segundos
    setInterval(switchImage, 3000);

    // Carregar a primeira imagem ao iniciar
    window.onload = switchImage;
  </script>
</body>
</html>



# Questão 7
<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Imagens com Filtro por Autor</title>
  <style>
    /* Fundo da página preto */
    body {
      background-color: black;
      color: white;
      font-family: Arial, sans-serif;
      margin: 0;
      padding: 0;
    }

    /* Lista de autores no canto superior esquerdo */
    .author-list {
      position: fixed;
      top: 20px;
      left: 20px;
      background-color: rgba(0, 0, 0, 0.7);
      color: white;
      padding: 10px;
      border-radius: 5px;
      font-size: 16px;
    }

    /* Estilo do grid */
    .grid {
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
      flex-wrap: wrap;
      gap: 20px;
    }

    .grid img {
      width: 100%;
      height: auto;
      max-width: 300px;
      display: block;
    }

    .fade-in {
      opacity: 0;
      animation: fadeIn 1s ease-in-out forwards;
    }

    @keyframes fadeIn {
      from { opacity: 0; }
      to { opacity: 1; }
    }

    /* Campo de pesquisa */
    .search-box {
      display: block;
      margin: 20px auto;
      padding: 10px;
      font-size: 16px;
      width: 250px;
      text-align: center;
      background-color: #333;
      color: white;
      border: 1px solid #444;
      border-radius: 5px;
    }
  </style>
</head>
<body>

  <!-- Lista de autores no canto superior esquerdo -->
  <div class="author-list">
    <strong>Autores Disponíveis:</strong>
    <ul>
      <li>Clemilson</li>
      <li>Robson</li>
      <li>Jorge</li>
      <li>Leon.x.Genesis</li>
      <li>2bi</li>
    </ul>
  </div>

  <!-- Campo de pesquisa para filtrar imagens por autor -->
  <input type="text" id="authorSearch" class="search-box" placeholder="Digite o nome do autor para filtrar" />

  <!-- Grid onde as imagens serão exibidas -->
  <div id="grid" class="grid"></div>

    <script>
    const images = [
      { src: "imagens/imagem1.jpg", author: "Clemilson" },
      { src: "imagens/imagem2.png", author: "Robson" },
      { src: "imagens/imagem3.png", author: "Jorge" },
      { src: "imagens/imagem4.jpg", author: "Leon.x.Genesis" },
      { src: "imagens/imagem5.png", author: "2bi" }
    ];

    const grid = document.querySelector("#grid");
    const searchInput = document.querySelector("#authorSearch");

    // Função para adicionar imagem ao grid com o autor
    function addImage(image) {
      const div = document.createElement("div");
      const img = document.createElement("img");
      img.src = image.src;
      img.alt = `Imagem de ${image.author}`;
      img.classList.add("fade-in");
      img.setAttribute("data-author", image.author); // Atributo data-author para filtrar

      div.appendChild(img);
      grid.appendChild(div);
    }

    // Função para filtrar as imagens com base no autor
    function filterImages() {
      const query = searchInput.value.toLowerCase().trim();
      const allImages = grid.querySelectorAll("img");

      allImages.forEach(img => {
        const author = img.getAttribute("data-author").toLowerCase();
        if (author.includes(query)) {
          img.parentElement.style.display = "block"; // Exibe a imagem se o autor combinar
        } else {
          img.parentElement.style.display = "none"; // Esconde a imagem se não combinar
        }
      });
    }

    // Adiciona todas as imagens inicialmente
    images.forEach(image => addImage(image));

    // Filtra as imagens sempre que o usuário digitar algo no campo de pesquisa
    searchInput.addEventListener("input", filterImages);
  </script>

</body>
</html>



# Questao 8
<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Imagem do Cache</title>
</head>
<body>
    <h1>Exemplo de Imagem com Cache</h1>
    <img id="imagem" src="" alt="Imagem não carregada" style="width: 300px;">
    
    <script>
        function verificarImagemNoCache(url) {
            // Verifica se a imagem já está no localStorage
            const imagemCache = localStorage.getItem(url);

            if (imagemCache) {
                // Se a imagem está no cache, coloca a imagem no elemento <img>
                document.getElementById('imagem').src = imagemCache;
                console.log('Imagem carregada do cache.');
            } else {
                // Se a imagem não está no cache, realiza a requisição
                fetch(url)
                    .then(response => response.blob()) // Converte a resposta para um blob
                    .then(blob => {
                        const imagemURL = URL.createObjectURL(blob); // Cria uma URL para a imagem
                        
                        // Armazena a URL da imagem no localStorage
                        localStorage.setItem(url, imagemURL);

                        // Exibe a imagem no elemento <img>
                        document.getElementById('imagem').src = imagemURL;
                        console.log('Imagem carregada da rede e salva no cache.');
                    })
                    .catch(error => console.error('Erro ao carregar a imagem:', error));
            }
        }

        // Exemplo de URL de imagem para testar
        const urlImagem = 'https://via.placeholder.com/300';
        
        // Chama a função para verificar se a imagem está no cache
        verificarImagemNoCache(urlImagem);
    </script>
</body>
</html>



# Questão 9
<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Lightbox com Detalhes da Imagem</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            background-color: #f7f7f7;
        }
        
        .image-container {
            display: flex;
            justify-content: center;
            gap: 20px;
            margin-top: 50px;
        }

        .image-container img {
            width: 200px;
            cursor: pointer;
            border-radius: 8px;
            transition: transform 0.3s ease;
        }

        .image-container img:hover {
            transform: scale(1.1);
        }

        /* Lightbox - Estilo do modal */
        .lightbox {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.8);
            justify-content: center;
            align-items: center;
            z-index: 1000;
        }

        .lightbox img {
            max-width: 90%;
            max-height: 90%;
            border-radius: 8px;
        }

        /* Modal para exibir detalhes */
        .modal {
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background-color: #fff;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
            display: none;
            width: 80%;
            max-width: 400px;
            text-align: left;
            z-index: 2000;
        }

        .modal h2 {
            margin-top: 0;
        }

        .modal p {
            margin: 5px 0;
        }

        .close {
            position: absolute;
            top: 10px;
            right: 10px;
            font-size: 20px;
            cursor: pointer;
        }

        .modal-overlay {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.5);
            z-index: 999;
        }
    </style>
</head>
<body>
    <h1>Click na Imagem para Ver Detalhes</h1>
    
    <div class="image-container">
        <img src="https://via.placeholder.com/400x300" alt="Imagem 1" id="img-1" onclick="abrirLightbox('https://via.placeholder.com/1200x900', 1)">
        <img src="https://via.placeholder.com/400x300/FF6347" alt="Imagem 2" id="img-2" onclick="abrirLightbox('https://via.placeholder.com/1200x900/FF6347', 2)">
    </div>

    <!-- Lightbox -->
    <div class="lightbox" id="lightbox">
        <img id="lightbox-img" src="" alt="Imagem Ampliada">
    </div>

    <!-- Modal de Detalhes -->
    <div class="modal-overlay" id="modal-overlay"></div>
    <div class="modal" id="modal">
        <span class="close" id="close-modal">×</span>
        <h2>Detalhes da Imagem</h2>
        <p><strong>Autor:</strong> <span id="autor"></span></p>
        <p><strong>Dimensões:</strong> <span id="dimensao"></span></p>
        <p><strong>URL:</strong> <a id="url-imagem" href="" target="_blank">Ver imagem</a></p>
    </div>

    <script>
        // Função para abrir a lightbox e fazer chamada à API
        function abrirLightbox(imagemURL, imagemId) {
            // Exibe a lightbox com a imagem
            const lightbox = document.getElementById('lightbox');
            const lightboxImg = document.getElementById('lightbox-img');
            lightbox.style.display = 'flex';
            lightboxImg.src = imagemURL;

            // Fazer a chamada à API para obter detalhes da imagem (simulado)
            buscarDetalhesImagem(imagemId);
        }

        // Função para buscar detalhes da imagem
        function buscarDetalhesImagem(imagemId) {
            // Exemplo de "detalhes" simulados
            const autor = "John Doe";
            const largura = 1200;
            const altura = 900;
            const urlImagem = `https://via.placeholder.com/1200x900`;

            // Exibe as informações no modal
            const modal = document.getElementById('modal');
            const autorElem = document.getElementById('autor');
            const dimensaoElem = document.getElementById('dimensao');
            const urlElem = document.getElementById('url-imagem');

            autorElem.textContent = autor;
            dimensaoElem.textContent = `${largura} x ${altura}`;
            urlElem.href = urlImagem;

            // Exibe o modal
            modal.style.display = 'block';
            document.getElementById('modal-overlay').style.display = 'block';
        }

        // Função para fechar o modal
        document.getElementById('close-modal').addEventListener('click', () => {
            document.getElementById('modal').style.display = 'none';
            document.getElementById('modal-overlay').style.display = 'none';
        });

        // Função para fechar a lightbox
        document.getElementById('lightbox').addEventListener('click', () => {
            document.getElementById('lightbox').style.display = 'none';
        });
    </script>
</body>
</html>



# Questão 10
<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Avaliações - Resetar</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            background-color: #f7f7f7;
        }
        
        .container {
            margin-top: 50px;
        }

        .avaliacao {
            margin: 10px;
            padding: 20px;
            background-color: #fff;
            border-radius: 8px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
            display: inline-block;
            text-align: left;
            width: 250px;
        }

        .avaliacao h3 {
            margin: 0;
        }

        .avaliacao p {
            font-size: 14px;
            color: #555;
        }

        .reset-btn {
            margin-top: 30px;
            padding: 10px 20px;
            background-color: #007bff;
            color: white;
            border: none;
            border-radius: 8px;
            cursor: pointer;
        }

        .reset-btn:hover {
            background-color: #0056b3;
        }
    </style>
</head>
<body>
    <h1>Minhas Avaliações</h1>
    <div class="container" id="avaliacoes-container">
        <!-- As avaliações serão exibidas aqui -->
    </div>

    <button class="reset-btn" id="reset-btn">Resetar Avaliações</button>

    <script>
        // Função para simular carregamento de avaliações
        function carregarAvaliacoes() {
            const container = document.getElementById('avaliacoes-container');
            container.innerHTML = ''; // Limpa a área de avaliações antes de carregar

            const avaliacoes = JSON.parse(localStorage.getItem('avaliacoes'));

            if (avaliacoes) {
                avaliacoes.forEach(avaliacao => {
                    const div = document.createElement('div');
                    div.classList.add('avaliacao');
                    div.innerHTML = `
                        <h3>${avaliacao.titulo}</h3>
                        <p>${avaliacao.texto}</p>
                    `;
                    container.appendChild(div);
                });
            } else {
                container.innerHTML = '<p>Não há avaliações registradas.</p>';
            }
        }

        // Função para resetar as avaliações (limpar localStorage)
        function resetarAvaliacoes() {
            localStorage.removeItem('avaliacoes'); // Remove as avaliações do localStorage
            carregarAvaliacoes(); // Atualiza a interface
        }

        // Evento do botão "Resetar Avaliações"
        document.getElementById('reset-btn').addEventListener('click', resetarAvaliacoes);

        // Simula a adição de algumas avaliações (para testes)
        if (!localStorage.getItem('avaliacoes')) {
            const avaliacoes = [
                { titulo: 'Imagem 1', texto: 'Avaliação muito boa, gostei da imagem!' },
                { titulo: 'Imagem 2', texto: 'Imagem de qualidade excelente, muito nítida.' },
            ];
            localStorage.setItem('avaliacoes', JSON.stringify(avaliacoes));
        }

        // Carregar as avaliações ao iniciar a página
        carregarAvaliacoes();
    </script>
</body>
</html>

