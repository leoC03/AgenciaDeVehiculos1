<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Pisteando Motors</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <style>
        body {
            font-family: 'Times New Roman', Times, serif; /* Cambiar a Times New Roman */
            margin: 0;
            padding: 0;
            background-image:url(imagenes/DeWatermark.ai_1740486707601.png)
        }
        header {
            background: #360752;
            color: white;
            padding: 10px 20px;
            text-align: center;
        }
        .container {
            width: 90%;
            max-width: 1200px;
            margin: auto;
            overflow: hidden;
        }
        .card {
            background: white;
            border-radius: 5px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
            margin: 10px;
            padding: 20px;
            text-align: center;
            transition: transform 0.3s;
            flex: 1 1 calc(33.333% - 20px);
        }
        .card:hover {
            transform: scale(1.05);
        }
        .login-form, .register-form {
            background: rgb(255, 255, 255);
            padding: 20px;
            border-radius: 5px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
            margin: 20px 0;
        }
        input[type="text"], input[type="password"], .btn {
            width: 100%;
            padding: 10px;
            margin: 5px 0;
            border: 1px solid #ccc;
            border-radius: 5px;
            cursor: pointer;
        }
        .btn {
            background: #35424a;
            color: white;
            border: none;
        }
        .btn:hover {
            background: #2c3e50;
        }
        .vehicles {
            display: flex;
            flex-wrap: wrap;
            justify-content: center;
            margin-top: 40px; /* Separar del registro y login */
            display: none; /* Oculto por defecto */
        }
        .cart {
            display: none; /* Oculto por defecto */
        }
        .cart-items {
            list-style: none;
            padding: 0;
        }
        .cart-items li {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin: 10px 0;
        }
        .cart-items button {
            background: #e74c3c;
            border: none;
            color: white;
            padding: 5px 10px;
            cursor: pointer;
        }
        .cart-items button:hover {
            background: #c0392b;
        }
        .error {
            color: red;
            margin-top: 10px;
        }
        .cart-actions {
            display: flex;
            justify-content: space-between;
            margin-top: 20px;
        }
        .empty-cart-btn, .checkout-btn {
            flex: 1;
            margin: 0 5px;
        }
        h2 {
            margin: 20px 0; /* Espacio entre título y cards */
            text-align: center; /* Centrado del título */
        }
        .vehicles h2 {
            margin-bottom: 20px; /* Espacio entre el título y las cards */
            text-align: center; /* Centrar el título */
        }
        @media (max-width: 768px) {
            .card {
                flex: 1 1 calc(50% - 20px);
            }
        }
        @media (max-width: 480px) {
            .card {
                flex: 1 1 100%;
            }
        }
    </style>
    <script>
        let cart = [];
        let loggedIn = false;

        function addToCart(vehicle) {
            if (!loggedIn) {
                alert("Por favor, inicie sesión para agregar vehículos al carrito.");
                return;
            }
            cart.push(vehicle);
            updateCartDisplay();
            alert("Vehículo agregado al carrito: " + vehicle.name);
        }

        function removeFromCart(index) {
            cart.splice(index, 1);
            updateCartDisplay();
        }

        function emptyCart() {
            cart = [];
            updateCartDisplay();
            alert("Carrito vaciado.");
        }

        function checkout() {
            if (cart.length === 0) {
                alert("El carrito está vacío. Agregue vehículos antes de finalizar la compra.");
                return;
            }
            alert("Compra finalizada. Gracias por su compra!");
            emptyCart();  // Clear the cart after checkout
        }

        function updateCartDisplay() {
            const cartCount = document.querySelector('.cart p');
            cartCount.innerText = "Actualmente hay " + cart.length + " vehículos en el carrito.";

            const cartItemsList = document.querySelector('.cart-items');
            cartItemsList.innerHTML = '';
            cart.forEach((vehicle, index) => {
                const listItem = document.createElement('li');
                listItem.innerHTML = `${vehicle.name} - $${vehicle.price} <button onclick="removeFromCart(${index})">Eliminar</button>`;
                cartItemsList.appendChild(listItem);
            });
        }

        function loadCart() {
            const cartJSON = localStorage.getItem('cart');
            if (cartJSON) {
                cart = JSON.parse(cartJSON);
                updateCartDisplay();
            }
        }

        function saveCart() {
            localStorage.setItem('cart', JSON.stringify(cart));
        }

        function register() {
            const username = document.querySelector('#reg-username').value;
            const password = document.querySelector('#reg-password').value;
            const users = JSON.parse(localStorage.getItem('users')) || {};

            if (users[username]) {
                alert("El usuario ya existe.");
            } else {
                users[username] = password;
                localStorage.setItem('users', JSON.stringify(users));
                alert("Registro exitoso. Puede iniciar sesión.");
                document.querySelector('.register-form').reset();
            }
        }

        function login() {
            const username = document.querySelector('#username').value;
            const password = document.querySelector('#password').value;
            const users = JSON.parse(localStorage.getItem('users')) || {};
            const errorMsg = document.querySelector('.error');

            if (users[username] && users[username] === password) {
                loggedIn = true;
                errorMsg.innerText = "";
                alert("Bienvenido, " + username + "!");
                document.querySelector('.login-form').style.display = 'none';
                document.querySelector('.register-form').style.display = 'none';  // Ocultar registro
                document.querySelector('.vehicles').style.display = 'flex'; // Mostrar vehículos
                document.querySelector('.cart').style.display = 'block'; // Mostrar carrito
                document.querySelector('.cart h2').innerText = "Carrito";
            } else {
                errorMsg.innerText = "Credenciales incorrectas. Intente de nuevo.";
            }
        }

        window.onbeforeunload = saveCart;  // Guardar carrito al cerrar la página
    </script>
</head>
<body onload="loadCart()">

<header>
    <h1>PISTEANDO MOTORS</h1>
</header>

<div class="container">
    <div class="register-form">
        <h2>Registrarse</h2>
        <input type="text" id="reg-username" placeholder="Usuario" required>
        <input type="password" id="reg-password" placeholder="Contraseña" required>
        <button class="btn" onclick="register()">Registrarse</button>
    </div>

    <div class="login-form">
        <h2>Iniciar Sesion</h2>
        <input type="text" id="username" placeholder="Usuario" required>
        <input type="password" id="password" placeholder="Contraseña" required>
        <button class="btn" onclick="login()">Iniciar Sesion</button>
        <p class="error"></p>
    </div>    
 
                <div><h2>Vehiculos Disponibles</h2>
    <div class="vehicles"> 
      
        </div>
        <div class="card">
            <h3>Sirocco</h3>
            <img src="imagenes/rocco.jpg" alt="Sirocco" style="width: 100%; max-width: 350px;">
            <p>Volkswagen Sirocco, motor 1.4t, DSG, 30mil km, año 2014</p>
            <p>Precio: $20,000</p>
            <button class="btn" onclick="addToCart({name: 'Sirocco', price: 20000})">Agregar al Carrito</button>
        </div>
        <div class="card">
            <h3>Golf</h3>
            <img src="imagenes/volkswagen-golf-2024-restyling---foto.jpg" alt="Golf" style="width: 100%; max-width: 350px;">
            <p>Volkswagen Golf, motor 1.4t, DSG, 0 km, año 2024</p>
            <p>Precio: $45,000</p>
            <button class="btn" onclick="addToCart({name: 'Golf', price: 45000})">Agregar al Carrito</button>
        </div>
        <div class="card">
            <h3>Amarok</h3>
            <img src="imagenes/volkswagen-amarok-976605.jpg" alt="Amarok" style="width: 100%; max-width: 350px;">
            <p>Volkswagen Amarok, motor V6 3.0, DSG, 0 km, año 2024</p>
            <p>Precio: $65,000</p>
            <button class="btn" onclick="addToCart({name: 'Amarok', price: 65000})">Agregar al Carrito</button>
        </div>
        <div class="card">
            <h3>Peugeot 308</h3>
            <img src="imagenes/3-Peugeot_308-estatica-4-812x530.jpg" alt="Peugeot 308" style="width: 100%; max-width: 350px;">
            <p>Peugeot 308 GT, motor 1.6THP, automático, 7.000km, año 2022</p>
            <p>Precio: $65,000</p>
            <button class="btn" onclick="addToCart({name: 'Peugeot 308', price: 65000})">Agregar al Carrito</button>
        </div>
        <div class="card">
            <h3>Peugeot e208</h3>
            <img src="imagenes/nuevo-peugeot-208-argentina-4-1.jpg" alt="Peugeot e208" style="width: 100%; max-width: 350px;">
            <p>Peugeot e208, motor 1.0T, híbrido, automático, 0 km, año 2024</p>
            <p>Precio: $18,000</p>
            <button class="btn" onclick="addToCart({name: 'Peugeot e208', price: 18000})">Agregar al Carrito</button>
        </div>
        <div class="card">
            <h3>Peugeot RCZ</h3>
            <img src="imagenes/rcz.jpg" alt="Peugeot RCZ" style="width: 100%; max-width: 350px;">
            <p>Peugeot RCZ, motor 1.6THP, tiptronic, 50.000 km, año 2014</p>
            <p>Precio: $21,000</p>
            <button class="btn" onclick="addToCart({name: 'Peugeot RCZ', price: 21000})">Agregar al Carrito</button>
        </div>
        
    </div>

    <div class="cart">
        <h2>Carrito</h2>
        <p>Actualmente hay 0 vehículos en el carrito.</p>
        <ul class="cart-items"></ul>
        <div class="cart-actions">
            <button class="empty-cart-btn btn" onclick="emptyCart()">Vaciar Carrito</button>
            <button class="checkout-btn btn" onclick="checkout()">Finalizar Compra</button>
                </div>
        </div>
       
    </div>
    
</div>

</body>
</html>
