proyecto/ # app-clima-js
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Aplicación del Clima</title>

    <link rel="stylesheet" href="css/styles.css">
</head>
<body>

<div class="contenedor">

    <h2>🌤 Aplicación del Clima</h2>

    <input
        type="text"
        id="ciudad"
        placeholder="Escribe una ciudad">

    <button onclick="consultarClima()">
        Consultar Clima
    </button>

    <div id="resultado"></div>

</div>

<script src="js/app.js"></script>

</body>
</html>

css/
*{
    margin:0;
    padding:0;
    box-sizing:border-box;
    font-family:Arial,sans-serif;
}

body{
    display:flex;
    justify-content:center;
    align-items:center;
    height:100vh;
    background:linear-gradient(135deg,#4facfe,#00f2fe);
    transition:background 1s ease;
}

.contenedor{
    width:370px;
    padding:30px;
    border-radius:20px;
    text-align:center;
    background:rgba(255,255,255,.25);
    backdrop-filter:blur(15px);
    border:1px solid rgba(255,255,255,.3);
    box-shadow:0 8px 30px rgba(0,0,0,.30);
    animation:aparecer .8s;
}

@keyframes aparecer{
    from{
        opacity:0;
        transform:translateY(25px);
    }
    to{
        opacity:1;
        transform:translateY(0);
    }
}

h2{
    color:white;
    margin-bottom:20px;
    font-size:34px;
}

input{
    width:100%;
    padding:12px;
    font-size:18px;
    border:none;
    border-radius:10px;
    outline:none;
    margin-bottom:15px;
}

button{
    width:100%;
    padding:12px;
    font-size:18px;
    border:none;
    border-radius:10px;
    background:#1976D2;
    color:white;
    cursor:pointer;
    transition:.3s;
}

button:hover{
    background:#0D47A1;
    transform:scale(1.03);
}

#resultado{
    margin-top:20px;
    color:white;
    animation:aparecer .8s;
}

#resultado h3{
    font-size:28px;
    margin-bottom:10px;
}

#resultado p{
    margin:8px;
    font-size:18px;
}

img{
    width:120px;
}

js/
const API_KEY = "031da8e2ad2f2a8066bca860910f0410";

async function consultarClima(){

    const ciudad = document.getElementById("ciudad").value.trim();

    if(ciudad === ""){
        document.getElementById("resultado").innerHTML =
        "<p>Ingrese una ciudad.</p>";
        return;
    }

    const url = `https://api.openweathermap.org/data/2.5/weather?q=${encodeURIComponent(ciudad)},SV&appid=${API_KEY}&units=metric&lang=es`;

    try{

        const respuesta = await fetch(url);
        const datos = await respuesta.json();

        if(datos.cod != 200){
            document.getElementById("resultado").innerHTML =
            "<p>❌ Ciudad no encontrada.</p>";
            return;
        }

        let clima = datos.weather[0].main;

        switch(clima){

            case "Clear":
                document.body.style.background =
                "linear-gradient(135deg,#FFD54F,#FF9800)";
                break;

            case "Clouds":
                document.body.style.background =
                "linear-gradient(135deg,#90A4AE,#607D8B)";
                break;

            case "Rain":
            case "Drizzle":
                document.body.style.background =
                "linear-gradient(135deg,#1565C0,#0D47A1)";
                break;

            case "Thunderstorm":
                document.body.style.background =
                "linear-gradient(135deg,#512DA8,#311B92)";
                break;

            case "Snow":
                document.body.style.background =
                "linear-gradient(135deg,#ECEFF1,#B0BEC5)";
                break;

            default:
                document.body.style.background =
                "linear-gradient(135deg,#4facfe,#00f2fe)";
        }

        document.getElementById("resultado").innerHTML = `

            <h3>📍 ${datos.name}, ${datos.sys.country}</h3>

            <img src="https://openweathermap.org/img/wn/${datos.weather[0].icon}@4x.png">

            <p><b>${datos.weather[0].description}</b></p>

            <p>🌡 <b>Temperatura:</b> ${datos.main.temp} °C</p>

            <p>🥵 <b>Sensación:</b> ${datos.main.feels_like} °C</p>

            <p>💧 <b>Humedad:</b> ${datos.main.humidity}%</p>

            <p>💨 <b>Viento:</b> ${datos.wind.speed} m/s</p>

        `;

    }catch(error){

        document.getElementById("resultado").innerHTML =
        "<p>⚠ Error al conectar con la API.</p>";

    }

}
