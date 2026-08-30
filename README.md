<!DOCTYPE html>
<html lang="pt-BR">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <title>Simulador de Juros Compostos</title>

    <style>

        * {
            box-sizing: border-box;
        }

        body {
            margin: 0;
            font-family: Arial, sans-serif;
            background: #f3f6fa;
            color: #222;
        }

        /* =========================
           CABEÇALHO
        ========================= */

        header {
            background: linear-gradient(135deg, #111827, #2563eb);
            color: white;
            text-align: center;
            padding: 40px 20px;
        }

        header h1 {
            margin: 0;
            font-size: 30px;
        }

        header p {
            margin-top: 10px;
            font-size: 16px;
            opacity: 0.9;
        }

        /* =========================
           CONTAINER
        ========================= */

        .container {
            max-width: 900px;
            margin: 30px auto;
            padding: 0 20px;
        }

        /* =========================
           CARD
        ========================= */

        .card {
            background: white;
            padding: 25px;
            border-radius: 18px;
            margin-bottom: 25px;
            box-shadow: 0 8px 25px rgba(0,0,0,0.08);
        }

        .card h2 {
            margin-top: 0;
            color: #2563eb;
        }

        /* =========================
           CAMPOS
        ========================= */

        label {
            display: block;
            margin-top: 15px;
            margin-bottom: 6px;
            font-weight: bold;
        }

        input {
            width: 100%;
            padding: 13px;
            border: 1px solid #ccc;
            border-radius: 10px;
            font-size: 16px;
        }

        /* =========================
           BOTÃO
        ========================= */

        button {
            width: 100%;
            margin-top: 25px;
            padding: 15px;
            border: none;
            border-radius: 10px;
            background: linear-gradient(135deg, #2563eb, #1d4ed8);
            color: white;
            font-size: 17px;
            font-weight: bold;
            cursor: pointer;
        }

        button:hover {
            opacity: 0.9;
        }

        /* =========================
           RESULTADOS
        ========================= */

        .resultados {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 15px;
        }

        .resultado {
            background: #f8fafc;
            padding: 20px;
            border-radius: 12px;
            text-align: center;
        }

        .resultado span {
            display: block;
            color: #666;
            font-size: 14px;
            margin-bottom: 8px;
        }

        .resultado strong {
            font-size: 20px;
            color: #2563eb;
        }

        /* =========================
           GRÁFICO
        ========================= */

        canvas {
            width: 100%;
            max-height: 350px;
        }

        /* =========================
           RODAPÉ
        ========================= */

        footer {
            text-align: center;
            padding: 25px;
            color: #777;
            font-size: 14px;
        }

        /* =========================
           CELULAR
        ========================= */

        @media (max-width: 650px) {

            header h1 {
                font-size: 25px;
            }

            .resultados {
                grid-template-columns: 1fr;
            }

        }

    </style>

</head>


<body>


    <!-- =========================
         CABEÇALHO
    ========================= -->

    <header>

        <h1>💰 Simulador de Juros Compostos</h1>

        <p>
            Descubra quanto seu dinheiro pode crescer ao longo do tempo.
        </p>

    </header>


    <div class="container">


        <!-- =========================
             SIMULADOR
        ========================= -->

        <div class="card">

            <h2>📊 Faça sua simulação</h2>


            <label>
                Valor inicial (R$)
            </label>

            <input
                type="number"
                id="inicial"
                value="5000"
                min="0"
            >


            <label>
                Aporte mensal (R$)
            </label>

            <input
                type="number"
                id="aporte"
                value="5000"
                min="0"
            >


            <label>
                Taxa de juros ao ano (%)
            </label>

            <input
                type="number"
                id="taxa"
                value="12"
                step="0.01"
                min="0"
            >


            <label>
                Tempo (anos)
            </label>

            <input
                type="number"
                id="anos"
                value="10"
                min="1"
            >


            <button onclick="calcular()">
                🚀 Calcular investimento
            </button>

        </div>


        <!-- =========================
             RESULTADOS
        ========================= -->

        <div class="card">

            <h2>💎 Resultado</h2>

            <div class="resultados">

                <div class="resultado">

                    <span>
                        Total investido
                    </span>

                    <strong id="totalInvestido">
                        R$ 0,00
                    </strong>

                </div>


                <div class="resultado">

                    <span>
                        Juros ganhos
                    </span>

                    <strong id="juros">
                        R$ 0,00
                    </strong>

                </div>


                <div class="resultado">

                    <span>
                        Patrimônio final
                    </span>

                    <strong id="final">
                        R$ 0,00
                    </strong>

                </div>

            </div>

        </div>


        <!-- =========================
             GRÁFICO
        ========================= -->

        <div class="card">

            <h2>📈 Evolução do patrimônio</h2>

            <canvas id="grafico"></canvas>

        </div>


    </div>


    <!-- =========================
         RODAPÉ
    ========================= -->

    <footer>

        Simulador de Juros Compostos © 2026

    </footer>


    <script>

        let graficoAtual = null;


        function dinheiro(valor) {

            return valor.toLocaleString(
                "pt-BR",
                {
                    style: "currency",
                    currency: "BRL"
                }
            );

        }


        function calcular() {

            const inicial =
                Number(document.getElementById("inicial").value);

            const aporte =
                Number(document.getElementById("aporte").value);

            const taxaAnual =
                Number(document.getElementById("taxa").value);

            const anos =
                Number(document.getElementById("anos").value);


            if (
                inicial < 0 ||
                aporte < 0 ||
                taxaAnual < 0 ||
                anos <= 0
            ) {

                alert("Digite valores válidos.");

                return;

            }


            /*
                Converte a taxa anual
                para uma taxa mensal equivalente.
            */

            const taxaMensal =
                Math.pow(
                    1 + taxaAnual / 100,
                    1 / 12
                ) - 1;


            const meses = anos * 12;


            let patrimonio = inicial;

            let totalInvestido = inicial;

            let valores = [patrimonio];

            let labels = ["Início"];


            for (
                let mes = 1;
                mes <= meses;
                mes++
            ) {

                patrimonio =
                    patrimonio * (1 + taxaMensal);

                patrimonio += aporte;

                totalInvestido += aporte;

                if (
                    mes % 12 === 0 ||
                    mes === meses
                ) {

                    valores.push(patrimonio);

                    labels.push(
                        "Ano " + Math.ceil(mes / 12)
                    );

                }

            }


            const juros =
                patrimonio - totalInvestido;


            document.getElementById(
                "totalInvestido"
            ).innerText = dinheiro(totalInvestido);


            document.getElementById(
                "juros"
            ).innerText = dinheiro(juros);


            document.getElementById(
                "final"
            ).innerText = dinheiro(patrimonio);


            criarGrafico(
                labels,
                valores
            );

        }


        function criarGrafico(
            labels,
            valores
        ) {

            const canvas =
                document.getElementById("grafico");

            const ctx =
                canvas.getContext("2d");


            /*
                Limpa o gráfico anterior.
            */

            ctx.clearRect(
                0,
                0,
                canvas.width,
                canvas.height
            );


            const largura =
                canvas.width =
                canvas.clientWidth * 2;

            const altura =
                canvas.height =
                350 * 2;


            ctx.scale(2, 2);


            const w =
                largura / 2;

            const h =
                altura / 2;


            const margem = 45;


            const maior =
                Math.max(...valores);


            /* Linha do gráfico */

            ctx.beginPath();


            valores.forEach(
                (valor, index) => {

                    const x =
                        margem +
                        (
                            index /
                            (valores.length - 1)
                        ) *
                        (w - margem * 1.5);


                    const y =
                        h -
                        margem -
                        (
                            valor /
                            maior
                        ) *
                        (h - margem * 1.5);


                    if (index === 0) {

                        ctx.moveTo(x, y);

                    } else {

                        ctx.lineTo(x, y);

                    }

                }
            );


            ctx.strokeStyle = "#2563eb";

            ctx.lineWidth = 3;

            ctx.stroke();


            /* Pontos */

            valores.forEach(
                (valor, index) => {

                    const x =
                        margem +
                        (
                            index /
                            (valores.length - 1)
                        ) *
                        (w - margem * 1.5);


                    const y =
                        h -
                        margem -
                        (
                            valor /
                            maior
                        ) *
                        (h - margem * 1.5);


                    ctx.beginPath();

                    ctx.arc(
                        x,
                        y,
                        4,
                        0,
                        Math.PI * 2
                    );

                    ctx.fillStyle = "#2563eb";

                    ctx.fill();

                }
            );

        }


        /*
            Faz uma simulação automaticamente
            quando o site abre.
        */

        calcular();

    </script>


</body>

</html>
