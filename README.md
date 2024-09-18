<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Login</title>
    <link rel="stylesheet" href="style.css">
    <script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>
</head>
<body>

<div id="main">
    <h2>Iniciar Sesión</h2>
    <input type="text" v-model="username" placeholder="Nombre de usuario">
    <input type="password" v-model="password" placeholder="Contraseña">
    <button @click="login">Entrar</button>

    <hr>

    <h2>Usuarios Conectados</h2>
    <ul>
        <li v-for="(user, index) in loggedUsers" :key="index">
            {{ user }}
        </li>
    </ul>
</div>

<script>
    const { createApp } = Vue;

    createApp({
        data() {
            return {
                username: '',
                password: '',
                loggedUsers: [] 
            }
        },
        methods: {
            async login() {
                if (this.username && this.password) {
                    try {
                        const response = await fetch('https://example.com/api/login', {
                            method: 'POST',
                            headers: {
                                'Content-Type': 'application/json'
                            },
                            body: JSON.stringify({
                                username: this.username,
                                password: this.password
                            })
                        });
                        
                        if (response.ok) {
                            const data = await response.json();
                            this.loggedUsers.push(data.username);
                            this.username = '';
                            this.password = '';
                        } else {
                            alert("Credenciales incorrectas. Inténtalo de nuevo.");
                        }
                    } catch (error) {
                        console.error('Error en la solicitud:', error);
                        alert("Error al conectar con el servidor.");
                    }
                } else {
                    alert("Por favor, completa ambos campos.");
                }
            }
        }
    }).mount('#main');
</script>

</body>
</html>
