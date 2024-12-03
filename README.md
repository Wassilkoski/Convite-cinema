<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Convite para o cinema</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            margin: 0;
            padding: 0;
            height: 100vh;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            background-color: #f0f8ff;
            overflow: hidden;
        }
        h1 {
            color: #333;
        }
        p {
            font-size: 18px;
            margin-bottom: 30px;
        }
        .buttons {
            position: relative;
            width: 100%;
            max-width: 300px;
            display: flex;
            justify-content: space-between;
        }
        button {
            padding: 10px 20px;
            font-size: 16px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            transition: all 0.3s ease;
        }
        #sim {
            background-color: #4caf50;
            color: white;
        }
        #nao {
            background-color: #f44336;
            color: white;
            position: absolute;
        }
        canvas {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
            z-index: 9999;
        }
    </style>
</head>
<body>
    <h1>Oi, Laura! 🎥</h1>
    <p>Gostaria de te convidar para ir ao cinema comigo. Aceita?</p>
    <div class="buttons">
        <button id="sim" onclick="aceitou()">Sim</button>
        <button id="nao" onmouseover="mover()">Não</button>
    </div>
    <canvas id="fireworks"></canvas>
    <script>
        // Função para mover o botão "Não"
        function mover() {
            const nao = document.getElementById("nao");
            const container = document.body;

            // Obtém as dimensões da tela
            const larguraMax = container.offsetWidth - nao.offsetWidth;
            const alturaMax = container.offsetHeight - nao.offsetHeight;

            // Gera posições aleatórias
            const novaPosX = Math.random() * larguraMax;
            const novaPosY = Math.random() * alturaMax;

            // Aplica as novas posições
            nao.style.left = `${novaPosX}px`;
            nao.style.top = `${novaPosY}px`;
        }

        // Função para mostrar fogos de artifício
        function aceitou() {
            // Mensagem de aceitação
            alert("Que ótimo, Laura! Vou marcar os detalhes e te aviso! 😊");

            // Inicia fogos de artifício
            startFireworks();
        }

        // Lógica para fogos de artifício
        const canvas = document.getElementById("fireworks");
        const ctx = canvas.getContext("2d");
        canvas.width = window.innerWidth;
        canvas.height = window.innerHeight;

        let particles = [];

        function Particle(x, y, color) {
            this.x = x;
            this.y = y;
            this.color = color;
            this.size = Math.random() * 5 + 2;
            this.velocityX = Math.random() * 4 - 2;
            this.velocityY = Math.random() * 4 - 2;
            this.alpha = 1;
        }

        Particle.prototype.update = function () {
            this.x += this.velocityX;
            this.y += this.velocityY;
            this.alpha -= 0.01;
        };

        Particle.prototype.draw = function () {
            ctx.globalAlpha = this.alpha;
            ctx.beginPath();
            ctx.arc(this.x, this.y, this.size, 0, Math.PI * 2);
            ctx.fillStyle = this.color;
            ctx.fill();
        };

        function createFirework(x, y) {
            const colors = ["#FF1461", "#18FF92", "#5A87FF", "#FBF38C"];
            for (let i = 0; i < 50; i++) {
                const color = colors[Math.floor(Math.random() * colors.length)];
                particles.push(new Particle(x, y, color));
            }
        }

        function animateFireworks() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            particles = particles.filter(p => p.alpha > 0);
            particles.forEach(p => {
                p.update();
                p.draw();
            });
            requestAnimationFrame(animateFireworks);
        }

        function startFireworks() {
            animateFireworks();
            setInterval(() => {
                const x = Math.random() * canvas.width;
                const y = Math.random() * canvas.height;
                createFirework(x, y);
            }, 300);
        }
    </script>
</body>
</html>
