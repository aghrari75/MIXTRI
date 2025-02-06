# MIXTRI<!DOCTYPE html>
<html lang="ar">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>متجري الإلكتروني</title>
    <link rel="stylesheet" href="styles.css">
    <script src="https://www.gstatic.com/firebasejs/9.6.1/firebase-app.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.6.1/firebase-database.js"></script>
</head>
<body>
    <header>
        <h1>متجري الإلكتروني</h1>
        <nav>
            <ul>
                <li><a href="#">الرئيسية</a></li>
                <li><a href="#products">المنتجات</a></li>
                <li><a href="#cart">سلة المشتريات</a></li>
            </ul>
        </nav>
    </header>
    
    <section id="products">
        <h2>المنتجات المتاحة</h2>
        <div class="product-list" id="product-list">
            <!-- المنتجات سيتم تحميلها ديناميكيًا من Firebase -->
        </div>
    </section>
    
    <section id="cart">
        <h2>سلة المشتريات</h2>
        <ul id="cart-items"></ul>
        <p>المجموع: <span id="total-price">0</span> درهم</p>
        <button id="checkout">إتمام الشراء</button>
    </section>
    
    <script>
        // Firebase configuration
        const firebaseConfig = {
            apiKey: "YOUR_API_KEY",
            authDomain: "YOUR_AUTH_DOMAIN",
            databaseURL: "YOUR_DATABASE_URL",
            projectId: "YOUR_PROJECT_ID",
            storageBucket: "YOUR_STORAGE_BUCKET",
            messagingSenderId: "YOUR_MESSAGING_SENDER_ID",
            appId: "YOUR_APP_ID"
        };
        
        firebase.initializeApp(firebaseConfig);
        const database = firebase.database();
        const productList = document.getElementById("product-list");
        
        database.ref("products").once("value", function(snapshot) {
            snapshot.forEach(function(childSnapshot) {
                let product = childSnapshot.val();
                let productElement = document.createElement("div");
                productElement.classList.add("product");
                productElement.innerHTML = `
                    <img src="${product.image}" alt="${product.name}">
                    <h3>${product.name}</h3>
                    <p>${product.description}</p>
                    <span>السعر: ${product.price} درهم</span>
                    <button onclick="addToCart('${product.name}', ${product.price})">إضافة إلى السلة</button>
                `;
                productList.appendChild(productElement);
            });
        });
        
        function addToCart(name, price) {
            let cartItems = document.getElementById("cart-items");
            let newItem = document.createElement("li");
            newItem.textContent = `${name} - ${price} درهم`;
            cartItems.appendChild(newItem);
            
            let totalPrice = document.getElementById("total-price");
            totalPrice.textContent = parseInt(totalPrice.textContent) + price;
        }
    </script>
</body>
</html>
