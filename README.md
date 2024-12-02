
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Registro e Inicio de Sesión</title>
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
            margin-bottom: 10px;
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
        .switch-container {
            text-align: center;
            margin-top: 15px;
            font-size: 14px;
        }
        .switch-container button {
            background-color: #008CBA;
        }
        .switch-container button:hover {
            background-color: #007BB5;
        }
    </style>
    <script>
        let isRegisterMode = true;

        function toggleMode() {
            isRegisterMode = !isRegisterMode;
            document.getElementById("registerForm").style.display = isRegisterMode ? "block" : "none";
            document.getElementById("loginForm").style.display = isRegisterMode ? "none" : "block";
        }

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

            alert("Registro exitoso.");
            toggleMode();
        }

        function iniciarSesion(event) {
            event.preventDefault();

            const email = document.getElementById("loginEmail").value;
            const password = document.getElementById("loginPassword").value;

            const usuarios = JSON.parse(localStorage.getItem("usuarios")) || [];
            const usuario = usuarios.find(u => u.email === email && u.password === password);

            if (usuario) {
                alert(`Bienvenido, ${usuario.nombre}!`);
                if (usuario.tipoUsuario === "turista") {
                    window.location.href = "https://hyordi12.github.io/Turista/#1-%C2%BFqu%C3%A9-es-las-coloradas";
                } else if (usuario.tipoUsuario === "guia") {
                    window.location.href = "https://hyordi12.github.io/Guia/";
                }
            } else {
                alert("Correo o contraseña incorrectos.");
            }
        }
    </script>
</head>
<body>

<div class="container">
    <div id="registerForm">
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
            <div id="camposTurista" class="extra-fields">
                <div class="form-group">
                    <label for="nacionalidad">Nacionalidad:</label>
                    <input type="text" id="nacionalidad">
                </div>
            </div>
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
    </div>

    <div id="loginForm" style="display: none;">
        <h2>Iniciar Sesión</h2>
        <form onsubmit="iniciarSesion(event)">
            <div class="form-group">
                <label for="loginEmail">Correo Electrónico:</label>
                <input type="email" id="loginEmail" required>
            </div>
            <div class="form-group">
                <label for="loginPassword">Contraseña:</label>
                <input type="password" id="loginPassword" required>
            </div>
            <button type="submit">Iniciar Sesión</button>
        </form>
    </div>

    <div class="switch-container">
        <button onclick="toggleMode()">¿Ya tienes cuenta? Inicia Sesión</button>
    </div>
    <div id="mensajeCumple" class="mensaje-cumple"></div>
</div>

</body>
</html>


