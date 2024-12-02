
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
            text-align: center;
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
        .hidden {
            display: none;
        }
        #birthdayMessage {
            font-size: 20px;
            font-weight: bold;
            color: #FF5722;
        }
    </style>
    <script>
        let isRegisterMode = true;

        function toggleMode() {
            isRegisterMode = !isRegisterMode;
            document.getElementById("registerForm").style.display = isRegisterMode ? "block" : "none";
            document.getElementById("loginForm").style.display = isRegisterMode ? "none" : "block";
        }

        function registrarUsuario(event) {
            event.preventDefault();
            const nombre = document.getElementById("nombre").value;
            const fechaNacimiento = new Date(document.getElementById("fechaNacimiento").value);
            const email = document.getElementById("email").value;
            const password = document.getElementById("password").value;
            const tipoUsuario = document.getElementById("tipoUsuario").value;

            const usuario = {
                nombre,
                fechaNacimiento: fechaNacimiento.toISOString(),
                email,
                password,
                tipoUsuario
            };

            const usuarios = JSON.parse(localStorage.getItem("usuarios")) || [];
            usuarios.push(usuario);
            localStorage.setItem("usuarios", JSON.stringify(usuarios));
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
                verificarCumpleaños(usuario);
            } else {
                alert("Correo o contraseña incorrectos.");
            }
        }

        function verificarCumpleaños(usuario) {
            const fechaNacimiento = new Date(usuario.fechaNacimiento);
            const hoy = new Date();

            if (fechaNacimiento.getDate() === hoy.getDate() && fechaNacimiento.getMonth() === hoy.getMonth()) {
                mostrarPantallaCumpleaños(usuario);
            } else {
                redirigirSegunTipo(usuario);
            }
        }

        function mostrarPantallaCumpleaños(usuario) {
            document.body.innerHTML = `
                <div class="container">
                    <p id="birthdayMessage">¡Feliz cumpleaños, ${usuario.nombre}!</p>
                    <button onclick="redirigirSegunTipo('${JSON.stringify(usuario).replace(/'/g, "\\'")}')">Continuar</button>
                </div>
            `;
        }

        function redirigirSegunTipo(usuario) {
            usuario = JSON.parse(usuario);
            if (usuario.tipoUsuario === "turista") {
                window.location.href = "https://hyordi12.github.io/Turista/#1-%C2%BFqu%C3%A9-es-las-coloradas";
            } else if (usuario.tipoUsuario === "guia") {
                window.location.href = "https://hyordi12.github.io/Guia/";
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
                <select id="tipoUsuario" required>
                    <option value="">Seleccione una opción</option>
                    <option value="turista">Turista</option>
                    <option value="guia">Guía</option>
                </select>
            </div>
            <button type="submit">Registrarse</button>
        </form>
    </div>

    <div id="loginForm" class="hidden">
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
        <button onclick="toggleMode()">Registrarse</button>
    </div>
</div>

</body>
</html>






