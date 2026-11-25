# media-notas
<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Média de Notas</title>
    <link rel="stylesheet" href="styles.css">
    <style>
        #loginConfirmar { background-color: #2563eb; }
        #loginCancelar { background-color: #64748b; }
        #loginLimpar { background-color: #0f766e; }
        button:disabled { opacity: 0.5; cursor: not-allowed; }
        #tentativas { text-align: center; font-size: 13px; color: #64748b; }
    </style>
    <script src="script.js" defer></script>
</head>
<body>

    <!-- TELA DE LOGIN -->
    <div class="container" id="telaLogin">
        <div class="cabecalho">
            <img class="logo" src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcT5xpPpFMuY3wUb6_ArL00gFh7eHOT4Rl32Uwe4KgPzCA&s=10" alt="logo">
            <h1>Área de Login</h1>
            <p class="subtitulo">Informe seus dados para acessar</p>
        </div>

        <form id="formLogin">
            <div class="campo">
                <label for="login">Login:</label>
                <input type="text" id="login" placeholder="Digite seu login">
            </div>
            <div class="campo">
                <label for="senha">Senha:</label>
                <input type="password" id="senha" placeholder="Digite sua senha">
            </div>
            <div class="botoes">
                <button type="submit" id="loginConfirmar">CONFIRMAR</button>
                <button type="button" id="loginCancelar">CANCELAR</button>
                <button type="button" id="loginLimpar">LIMPAR</button>
            </div>
        </form>

        <p id="msgLogin" class="erro"></p>
        <p id="tentativas">Tentativas disponíveis: 3</p>
        <footer>Dica: abc57</footer>
    </div>

    <!-- PÁGINA DA MÉDIA (aparece depois do login) -->
    <div class="container" id="telaMedia" hidden>

        <div class="cabecalho">
            <img class="logo" src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcT5xpPpFMuY3wUb6_ArL00gFh7eHOT4Rl32Uwe4KgPzCA&s=10" alt="logo">
            <h1>Média de Notas</h1>
            <p class="subtitulo">Informe o aluno e as notas (0 a 10)</p>
        </div>

        <form id="formNotas">
            <div class="campo">
                <label for="nome">Nome do aluno:</label>
                <input type="text" id="nome" placeholder="Digite o nome do aluno">
            </div>

            <div class="linha-notas">
                <div class="campo">
                    <label for="nota1">Nota 1:</label>
                    <input type="number" id="nota1" step="0.1" placeholder="0 a 10">
                </div>
                <div class="campo">
                    <label for="nota2">Nota 2:</label>
                    <input type="number" id="nota2" step="0.1" placeholder="0 a 10">
                </div>
                <div class="campo">
                    <label for="nota3">Nota 3:</label>
                    <input type="number" id="nota3" step="0.1" placeholder="0 a 10">
                </div>
            </div>

            <div class="botoes">
                <button type="button" id="btnMedia">MÉDIA</button>
                <button type="button" id="btnSituacao">SITUAÇÃO</button>
                <button type="button" id="btnLimpar">LIMPAR</button>
            </div>
        </form>

        <p id="erro" class="erro"></p>

        <div class="resultado">
            <p><strong>Nome do aluno:</strong> <span id="resNome"></span></p>
            <p><strong>Média obtida:</strong> <span id="resMedia"></span></p>
            <p><strong>Situação:</strong> <span id="resSituacao"></span></p>
        </div>

        <a class="link" href="pagina2.html">Ir para a página de data e hora</a>
        <a class="link" href="#" id="sair">Sair (voltar ao login)</a>

        <p class="data-hora" id="dataHoraIndex"></p>

        <footer>São Paulo maior do Brasil</footer>
    </div>

    <script>
        var telaLogin = document.getElementById("telaLogin");
        var telaMedia = document.getElementById("telaMedia");
        var formLogin = document.getElementById("formLogin");
        var campoLogin = document.getElementById("login");
        var campoSenha = document.getElementById("senha");
        var msgLogin = document.getElementById("msgLogin");
        var textoTentativas = document.getElementById("tentativas");
        var btnLoginConfirmar = document.getElementById("loginConfirmar");
        var tentativas = 3;

        function entrar() {
            telaLogin.hidden = true;
            telaMedia.hidden = false;
            try { sessionStorage.setItem("logado", "sim"); } catch (e) {}
        }

        // Se já fez login (ex.: voltou da página de data e hora), ***** o login
        try {
            if (sessionStorage.getItem("logado") === "sim") { entrar(); }
        } catch (e) {}

        formLogin.addEventListener("submit", function (evento) {
            evento.preventDefault();

            if (tentativas <= 0) {
                msgLogin.className = "erro";
                msgLogin.textContent = "Acesso bloqueado!";
                return;
            }

            var login = campoLogin.value.trim();
            var senha = campoSenha.value.trim();

            if (login === "" || senha === "") {
                msgLogin.className = "erro";
                msgLogin.textContent = "Preencha o login e a senha.";
                return;
            }

            if (login.toLowerCase() === "abc" && senha.toLowerCase() === "abc57") {
                msgLogin.className = "aprovado";
                msgLogin.textContent = "Acesso válido! Entrando...";
                btnLoginConfirmar.disabled = true;
                setTimeout(entrar, 800);
            } else {
                tentativas--;
                msgLogin.className = "erro";
                if (tentativas > 0) {
                    msgLogin.textContent = "Acesso inválido! Tente novamente.";
                } else {
                    msgLogin.textContent = "Acesso bloqueado! Você esgotou as 3 tentativas.";
                    btnLoginConfirmar.disabled = true;
                }
                textoTentativas.textContent = "Tentativas disponíveis: " + tentativas;
            }
        });

        document.getElementById("loginLimpar").addEventListener("click", function () {
            campoLogin.value = "";
            campoSenha.value = "";
            msgLogin.textContent = "";
            campoLogin.focus();
        });

        document.getElementById("loginCancelar").addEventListener("click", function () {
            formLogin.reset();
            msgLogin.textContent = "";
            campoLogin.focus();
        });

        // Botão SAIR: volta para a tela de login
        document.getElementById("sair").addEventListener("click", function (e) {
            e.preventDefault();
            try { sessionStorage.removeItem("logado"); } catch (err) {}
            telaMedia.hidden = true;
            telaLogin.hidden = false;
            formLogin.reset();
            msgLogin.textContent = "";
            btnLoginConfirmar.disabled = (tentativas <= 0);
            campoLogin.focus();
        });
    </script>
</body>
</html>
