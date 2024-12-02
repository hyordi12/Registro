
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Registro de Usuario</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f5f5f5;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
        }
        .container {
            background-color: #ffffff;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
            width: 300px;
        }
        .form-group {
            margin-bottom: 15px;
        }
        label {
            display: block;
            margin-bottom: 5px;
            font-weight: bold;
        }
        input[type="text"],
        input[type="email"],
        input[type="password"],
        input[type="date"],
        input[type="number"],
        select {
            width: 100%;
            padding: 8px;
            border: 1px solid #ddd;
            border-radius: 4px;
        }
        button {
            width: 100%;
            padding: 10px;
            background-color: #4CAF50;
            color: white;
            border: none;
            border-radius: 4px;
            cursor: pointer;
        }
        button:hover {
            background-color: #45a049;
        }
        .extra-fields {
            display: none;
        }
        .mensaje-cumple {
            color: #FF5722;
            font-weight: bold;
            text-align: center;
            margin-top: 10px;
        }
    </style>
    <script>
        function mostrarCamposAdicionales() {
            const tipoUsuario = document.getElementById("tipoUsuario").value;
            const camposTurista = document.getElementById("camposTurista");
            const camposGuia = document.getElementById("camposGuia");

            if (tipoUsuario === "turista") {
                camposTurista.style.display = "block";
                camposGuia.style.display = "none";
            } else if (tipoUsuario === "guia") {
                camposTurista.style.display = "none";
                camposGuia.style.display = "block";
            } else {
                camposTurista.style.display = "none";
                camposGuia.style.display = "none";
            }
        }

        function registrarUsuario(event) {
            event.preventDefault();

            const nombre = document.getElementById("nombre").value;
            const fechaNacimiento = new Date(document.getElementById("fechaNacimiento").value);
            const email = document.getElementById("email").value;
            const password = document.getElementById("password").value;
            const tipoUsuario = document.getElementById("tipoUsuario").value;

            let datosAdicionales = {};
            if (tipoUsuario === "turista") {
                datosAdicionales.nacionalidad = document.getElementById("nacionalidad").value;
            } else if (tipoUsuario === "guia") {
                datosAdicionales.experiencia = document.getElementById("experiencia").value;
                datosAdicionales.idiomas = document.getElementById("idiomas").value;
            }

            const usuario = {
                nombre,
                fechaNacimiento: fechaNacimiento.toISOString(),
                email,
                password,
                tipoUsuario,
                ...datosAdicionales
            };

            // Guardar usuario en localStorage
            let usuarios = JSON.parse(localStorage.getItem("usuarios")) || [];
            usuarios.push(usuario);
            localStorage.setItem("usuarios", JSON.stringify(usuarios));

            // Mostrar mensaje de cumpleaños si coincide con la fecha actual
            const hoy = new Date();
            if (fechaNacimiento.getDate() === hoy.getDate() && fechaNacimiento.getMonth() === hoy.getMonth()) {
                document.getElementById("mensajeCumple").innerText = `¡Feliz cumpleaños, ${nombre}!`;
            } else {
                document.getElementById("mensajeCumple").innerText = "";
            }

            // Redirigir según el tipo de usuario
            if (tipoUsuario === "turista") {
                window.location.href = "https://hyordi12.github.io/Turista/#1-%C2%BFqu%C3%A9-es-las-coloradas";
            } else if (tipoUsuario === "guia") {
                window.location.href = "https://hyordi12.github.io/Guia/";
            }
        }
    </script>
</head>
<body>

<div class="container">
    <h2>Registro de Usuario</h2>
    <form onsubmit="registrarUsuario(event)">
        <div class="form-group">
            <label for="nombre">Nombre:</label>
            <input type="text" id="nombre" required>
        </div>

        <div class="form-group">
            <label for="fechaNacimiento">Fecha de Nacimiento:</label>
            <input type="date" id="fechaNacimiento" required>
        </div>

        <div class="form-group">
            <label for="email">Correo Electrónico:</label>
            <input type="email" id="email" required>
        </div>

        <div class="form-group">
            <label for="password">Contraseña:</label>
            <input type="password" id="password" required>
        </div>

        <div class="form-group">
            <label for="tipoUsuario">Tipo de Usuario:</label>
            <select id="tipoUsuario" onchange="mostrarCamposAdicionales()" required>
                <option value="">Seleccione una opción</option>
                <option value="turista">Turista</option>
                <option value="guia">Guía</option>
            </select>
        </div>

        <!-- Campos adicionales para Turista -->
        <div id="camposTurista" class="extra-fields">
            <div class="form-group">
                <label for="nacionalidad">Nacionalidad:</label>
                <input type="text" id="nacionalidad">
            </div>
        </div>

        <!-- Campos adicionales para Guía -->
        <div id="camposGuia" class="extra-fields">
            <div class="form-group">
                <label for="experiencia">Años de Experiencia:</label>
                <input type="number" id="experiencia" min="0">
            </div>
            <div class="form-group">
                <label for="idiomas">Idiomas:</label>
                <input type="text" id="idiomas" placeholder="Ej: Español, Inglés">
            </div>
        </div>

        <button type="submit">Registrarse</button>
    </form>
    <div id="mensajeCumple" class="mensaje-cumple"></div>
</div>

</body>
</html>

