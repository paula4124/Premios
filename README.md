<!DOCTYPE html>
<html lang="pt-br">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Painel de Bexigas</title>
<style>
    /* Estilos gerais */
    body {
        font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        background-color: #f0f0f0;
        display: flex;
        justify-content: center;
        align-items: center;
        height: 100vh;
        margin: 0;
    }

    .container {
        width: 80%;
        max-width: 800px; /* Aumentando a largura máxima para melhor ajuste em telas maiores */
        text-align: center;
        padding: 20px;
        background-color: #fff;
        border-radius: 10px;
        box-shadow: 0 0 20px rgba(0, 0, 0, 0.1);
    }

    h1 {
        color: #e74c3c;
        text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.2);
    }

    .balloon {
        display: inline-block;
        position: relative;
        margin: 10px;
        padding: 20px 30px;
        font-size: 24px;
        text-align: center;
        cursor: pointer;
        background-color: #e74c3c;
        color: #fff;
        border: 2px solid #c0392b;
        border-radius: 15px;
        box-shadow: 0 8px 16px rgba(0, 0, 0, 0.2);
        transition: transform 0.3s ease-in-out, box-shadow 0.3s ease-in-out;
    }

    .balloon:before {
        content: "";
        position: absolute;
        bottom: -15px;
        left: 50%;
        width: 2px;
        height: 30px;
        background-color: #c0392b;
        transform: translateX(-50%);
        z-index: -1;
    }

    .balloon:hover {
        transform: scale(1.1);
        box-shadow: 0 12px 20px rgba(0, 0, 0, 0.3);
    }

    .confetti {
        position: absolute;
        width: 10px;
        height: 10px;
        background-color: #f1c40f;
        opacity: 0.9;
        border-radius: 50%;
        animation: confetti 3s ease-out forwards;
    }

    .message {
        position: fixed;
        top: 20px;
        left: 50%;
        transform: translateX(-50%);
        font-size: 20px;
        color: #fff;
        background-color: rgba(0, 0, 0, 0.8);
        padding: 10px 20px;
        border-radius: 5px;
        text-align: center;
        z-index: 999;
        display: none; /* Inicia oculto */
    }

    @keyframes confetti {
        0% { transform: translateY(0) rotateZ(0); }
        100% { transform: translateY(1500px) rotateZ(180deg); } /* Aumentando o valor para mais confetes */
    }

    /* Media queries para responsividade */
    @media (max-width: 768px) {
        .container {
            width: 90%;
        }

        .balloon {
            padding: 15px 25px;
            font-size: 20px;
        }

        .message {
            font-size: 18px;
        }
    }
</style>
</head>
<body>
<div class="container">
    <h1>Painel de Bexigas</h1>
    <div id="panel">
        <!-- Os botões de bexiga serão gerados dinamicamente pelo JavaScript -->
    </div>
</div>

<!-- Div para exibir a mensagem -->
<div class="message" id="message"></div>

<script>
    // Função para gerar um valor aleatório entre R$ 50,00 e R$ 100,00
    function getRandomValue() {
        return Math.random() < 0.5 ? 50 : 100;
    }

    // Função para revelar o valor ao clicar na bexiga
    function revealValue(button) {
        var valor = getRandomValue();
        button.innerHTML = 'R$ ' + valor.toFixed(2); // Formata o valor para duas casas decimais
        button.style.backgroundColor = "#f39c12"; // Cor de fundo para indicar que foi revelado
        button.style.color = "#fff"; // Cor do texto para melhor contraste
        button.style.cursor = "default"; // Remove o cursor de ponteiro após revelar o valor
        button.style.border = "2px solid #e74c3c"; // Borda com a cor principal das bexigas
        button.style.boxShadow = "0 8px 16px rgba(0, 0, 0, 0.2)"; // Sombra para efeito de elevação
        button.style.transition = "none"; // Remove a transição para evitar efeitos indesejados após revelar o valor
        button.onclick = null; // Remove o evento de clique após revelar o valor

        // Criar efeito de confetes
        var confettiCount = 20; // Quantidade de confetes
        for (var i = 0; i < confettiCount; i++) {
            createConfetti(button);
        }

        // Exibir mensagem temporária fora do balão
        var message = document.getElementById('message');
        message.innerHTML = 'Parabéns pela sua venda!';
        message.style.display = 'block';

        // Remover confetes e mensagem após 3 segundos
        setTimeout(function() {
            removeConfetti(button);
            message.innerHTML = ''; // Limpa a mensagem após o tempo determinado
            message.style.display = 'none'; // Oculta novamente a mensagem
        }, 3000);
    }

    // Função para criar um confete
    function createConfetti(button) {
        var confetti = document.createElement('div');
        confetti.className = 'confetti';
        var size = Math.floor(Math.random() * 10) + 5; // Tamanho aleatório entre 5px e 15px
        var positionX = Math.floor(Math.random() * 100); // Posição X aleatória dentro do botão
        var positionY = Math.floor(Math.random() * 100); // Posição Y aleatória dentro do botão
        confetti.style.width = size + 'px';
        confetti.style.height = size + 'px';
        confetti.style.backgroundColor = '#f1c40f';
        confetti.style.opacity = '0.9';
        confetti.style.borderRadius = '50%';
        confetti.style.position = 'absolute';
        confetti.style.left = positionX + '%';
        confetti.style.top = positionY + '%';
        confetti.style.animation = 'confetti 3s ease-out forwards';
        button.appendChild(confetti);
    }

    // Função para remover os confetes após a animação
    function removeConfetti(button) {
        var confettis = button.getElementsByClassName('confetti');
        while (confettis.length > 0) {
            confettis[0].parentNode.removeChild(confettis[0]);
        }
    }

    // Criar os botões dinamicamente com JavaScript
    var panel = document.getElementById('panel');
    for (var i = 1; i <= 20; i++) {
        var button = document.createElement('button');
        button.className = 'balloon';
        button.innerHTML = i.toString(); // Números de 1 a 20 como texto inicial
        button.onclick = function() {
            revealValue(this);
        };
        panel.appendChild(button);
    }
</script>

</body>
</html>
