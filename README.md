<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta name="theme-color" content="#00a86b">
  <title>Só os curiosos vão fazer um Pix 👀</title>

  <style>
    :root {
      --verde: #00a86b;
      --verde-claro: #16d99a;
      --azul: #1565c0;
      --texto: #24312c;
      --texto-suave: #65716c;
      --branco: #ffffff;
      --sombra: 0 24px 70px rgba(0, 55, 37, 0.28);
    }

    * {
      box-sizing: border-box;
    }

    body {
      margin: 0;
      min-height: 100vh;
      padding: 24px;
      display: grid;
      place-items: center;
      overflow-x: hidden;
      font-family: Inter, Arial, Helvetica, sans-serif;
      color: var(--texto);
      background:
        radial-gradient(circle at 15% 20%, rgba(255,255,255,.22), transparent 28%),
        radial-gradient(circle at 85% 80%, rgba(255,255,255,.16), transparent 25%),
        linear-gradient(135deg, #00c853, #00a88c 55%, #00897b);
    }

    .orb {
      position: fixed;
      border-radius: 999px;
      background: rgba(255,255,255,.13);
      filter: blur(1px);
      pointer-events: none;
      animation: flutuar 8s ease-in-out infinite;
    }

    .orb.one {
      width: 180px;
      height: 180px;
      top: -50px;
      left: -45px;
    }

    .orb.two {
      width: 240px;
      height: 240px;
      right: -80px;
      bottom: -90px;
      animation-delay: -3s;
    }

    .card {
      position: relative;
      z-index: 2;
      width: min(100%, 440px);
      padding: 34px;
      text-align: center;
      background: rgba(255,255,255,.96);
      border: 1px solid rgba(255,255,255,.65);
      border-radius: 26px;
      box-shadow: var(--sombra);
      backdrop-filter: blur(14px);
      animation: entrada .75s cubic-bezier(.2,.8,.2,1);
    }

    .emoji {
      width: 72px;
      height: 72px;
      margin: 0 auto 14px;
      display: grid;
      place-items: center;
      font-size: 36px;
      background: #e8fff5;
      border-radius: 22px;
      animation: pulsar 2.4s ease-in-out infinite;
    }

    h1 {
      margin: 0 0 12px;
      color: #008f5a;
      font-size: clamp(26px, 6vw, 34px);
      line-height: 1.12;
    }

    p {
      margin: 0;
      color: var(--texto-suave);
      line-height: 1.65;
    }

    .main-button,
    .copy-button {
      width: 100%;
      margin-top: 24px;
      border: 0;
      border-radius: 999px;
      padding: 17px 24px;
      color: var(--branco);
      font-size: 17px;
      font-weight: 800;
      cursor: pointer;
      box-shadow: 0 12px 28px rgba(0, 168, 107, .28);
      transition: transform .2s ease, box-shadow .2s ease, filter .2s ease;
    }

    .main-button {
      background: linear-gradient(135deg, var(--verde), var(--verde-claro));
    }

    .copy-button {
      margin-top: 12px;
      background: linear-gradient(135deg, #1565c0, #2985e8);
      box-shadow: 0 12px 28px rgba(21, 101, 192, .24);
    }

    .main-button:hover,
    .copy-button:hover {
      transform: translateY(-2px) scale(1.01);
      filter: brightness(1.03);
    }

    .main-button:active,
    .copy-button:active {
      transform: scale(.98);
    }

    button:focus-visible {
      outline: 4px solid rgba(21, 101, 192, .25);
      outline-offset: 3px;
    }

    .pix {
      display: grid;
      grid-template-rows: 0fr;
      opacity: 0;
      transition: grid-template-rows .45s ease, opacity .35s ease, margin-top .45s ease;
    }

    .pix.open {
      grid-template-rows: 1fr;
      opacity: 1;
      margin-top: 24px;
    }

    .pix-inner {
      min-height: 0;
      overflow: hidden;
    }

    .pix-box {
      padding: 20px;
      border-radius: 18px;
      background: #f4fbf8;
      border: 1px solid #dcefe7;
    }

    .pix-label {
      display: block;
      margin-bottom: 10px;
      color: var(--texto);
      font-weight: 800;
    }

    textarea {
      width: 100%;
      min-height: 104px;
      padding: 13px;
      resize: none;
      border: 1px solid #cfe2da;
      border-radius: 13px;
      background: var(--branco);
      color: #314039;
      font: 13px/1.45 ui-monospace, SFMono-Regular, Consolas, monospace;
      outline: none;
    }

    textarea:focus {
      border-color: var(--verde);
      box-shadow: 0 0 0 4px rgba(0,168,107,.12);
    }

    .key {
      margin-top: 17px;
      padding: 12px;
      border-radius: 12px;
      background: var(--branco);
      color: var(--texto-suave);
      overflow-wrap: anywhere;
    }

    .key strong {
      display: block;
      margin-top: 3px;
      color: var(--texto);
    }

    footer {
      margin-top: 22px;
      color: #87918d;
      font-size: 13px;
    }

    .toast {
      position: fixed;
      z-index: 10;
      left: 50%;
      bottom: 24px;
      width: max-content;
      max-width: calc(100% - 32px);
      padding: 13px 18px;
      border-radius: 999px;
      color: white;
      background: #173c30;
      box-shadow: 0 12px 34px rgba(0,0,0,.25);
      transform: translate(-50%, 30px);
      opacity: 0;
      pointer-events: none;
      transition: .3s ease;
    }

    .toast.show {
      transform: translate(-50%, 0);
      opacity: 1;
    }

    .confete {
      position: fixed;
      z-index: 1;
      top: -20px;
      width: 10px;
      height: 16px;
      border-radius: 3px;
      pointer-events: none;
      animation: cair var(--duracao) linear forwards;
    }

    @keyframes entrada {
      from { opacity: 0; transform: translateY(35px) scale(.97); }
      to { opacity: 1; transform: translateY(0) scale(1); }
    }

    @keyframes pulsar {
      0%, 100% { transform: rotate(-3deg) scale(1); }
      50% { transform: rotate(3deg) scale(1.08); }
    }

    @keyframes flutuar {
      0%, 100% { transform: translateY(0) translateX(0); }
      50% { transform: translateY(20px) translateX(12px); }
    }

    @keyframes cair {
      to {
        transform: translate3d(var(--desvio), 115vh, 0) rotate(900deg);
        opacity: .15;
      }
    }

    @media (max-width: 480px) {
      body { padding: 16px; }
      .card { padding: 28px 20px; border-radius: 22px; }
    }

    @media (prefers-reduced-motion: reduce) {
      *, *::before, *::after {
        scroll-behavior: auto !important;
        animation-duration: .01ms !important;
        animation-iteration-count: 1 !important;
        transition-duration: .01ms !important;
      }
    }
  </style>
</head>

<body>
  <div class="orb one"></div>
  <div class="orb two"></div>

  <main class="card">
    <div class="emoji" aria-hidden="true">👀</div>

    <h1>Você é curioso mesmo!</h1>

    <p>
      Já que chegou até aqui, merece uma surpresa.<br>
      Ou melhor… que tal deixar um presentinho? 😂
    </p>

    <button
      class="main-button"
      id="mostrarPix"
      type="button"
      aria-controls="areaPix"
      aria-expanded="false">
      💚 Revelar meu Pix
    </button>

    <section class="pix" id="areaPix" aria-hidden="true">
      <div class="pix-inner">
        <div class="pix-box">
          <label class="pix-label" for="codigoPix">Pix copia e cola</label>

          <textarea
            id="codigoPix"
            readonly
            aria-label="Código Pix copia e cola"></textarea>

          <button class="copy-button" id="copiarPix" type="button">
            📋 Copiar código Pix
          </button>

          <div class="key">
            Chave Pix
            <strong id="chavePix"></strong>
          </div>
        </div>
      </div>
    </section>

    <footer>
      Qualquer valor será recebido com muito carinho ❤️
    </footer>
  </main>

  <div class="toast" id="toast" role="status" aria-live="polite"></div>

  <script>
    /*
      PERSONALIZE SOMENTE ESTES 3 CAMPOS:
    */
    const CONFIG = {
      nome: "SEU NOME",
      chavePix: "sua-chave@pix.com",
      codigoPix: "COLE_AQUI_SEU_PIX_COPIA_E_COLA"
    };

    const botaoMostrar = document.getElementById("mostrarPix");
    const areaPix = document.getElementById("areaPix");
    const codigoPix = document.getElementById("codigoPix");
    const chavePix = document.getElementById("chavePix");
    const botaoCopiar = document.getElementById("copiarPix");
    const toast = document.getElementById("toast");

    codigoPix.value = CONFIG.codigoPix.trim();
    chavePix.textContent = CONFIG.chavePix;
    document.title = `Um Pix para ${CONFIG.nome} 👀`;

    botaoMostrar.addEventListener("click", () => {
      const aberto = areaPix.classList.toggle("open");

      areaPix.setAttribute("aria-hidden", String(!aberto));
      botaoMostrar.setAttribute("aria-expanded", String(aberto));
      botaoMostrar.textContent = aberto
        ? "🙈 Esconder meu Pix"
        : "💚 Faça um pix";

      if (aberto) {
        criarConfetes();
        setTimeout(() => codigoPix.focus(), 350);
      }
    });

    botaoCopiar.addEventListener("click", async () => {
      const texto = codigoPix.value.trim();

      if (!texto || texto.includes("COLE_AQUI")) {
        mostrarToast("47346746805.");
        return;
      }

      try {
        await navigator.clipboard.writeText(texto);
        mostrarToast("Código Pix copiado! Abra o banco e cole 😊");
      } catch {
        codigoPix.select();
        document.execCommand("copy");
        mostrarToast("Código Pix copiado! Abra o banco e cole 😊");
      }
    });

    function mostrarToast(mensagem) {
      toast.textContent = mensagem;
      toast.classList.add("show");

      clearTimeout(window.toastTimer);
      window.toastTimer = setTimeout(() => {
        toast.classList.remove("show");
      }, 3000);
    }

    function criarConfetes() {
      const cores = ["#00c853", "#00bfa5", "#ffd54f", "#ff6b6b", "#4d96ff", "#b967ff"];

      for (let i = 0; i < 45; i++) {
        const confete = document.createElement("span");
        confete.className = "confete";
        confete.style.left = Math.random() * 100 + "vw";
        confete.style.background = cores[Math.floor(Math.random() * cores.length)];
        confete.style.setProperty("--duracao", (Math.random() * 2.5 + 3) + "s");
        confete.style.setProperty("--desvio", (Math.random() * 220 - 110) + "px");
        confete.style.animationDelay = (Math.random() * .6) + "s";

        document.body.appendChild(confete);
        setTimeout(() => confete.remove(), 6500);
      }
    }
  </script>
</body>
</html>
