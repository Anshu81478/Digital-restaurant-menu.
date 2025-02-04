<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Digital Menu</title>
<style>
    body { font-family: Arial, sans-serif; background-color: #f8f1e4; text-align: center; margin: 0; padding: 0; }
    header { background-color: #8B4513; color: white; padding: 15px; font-size: 24px; font-weight: bold; }
    .menu, .cart { display: flex; flex-wrap: wrap; justify-content: center; padding: 20px; }
    .category, .cart-container { background-color: white; padding: 15px; margin: 10px; border-radius: 10px; width: 300px; box-shadow: 0px 0px 8px rgba(0,0,0,0.2); text-align: left; }
    .cart-btn, .order-btn { background-color: #ff9800; color: white; border: none; padding: 8px 12px; border-radius: 5px; cursor: pointer; transition: transform 0.2s, background-color 0.3s; }
    .cart-btn:hover, .order-btn:hover { background-color: #e68900; transform: scale(1.1); }
    .cart-total { font-weight: bold; margin-top: 10px; }
</style>
</head>
<body>
<header>चाय एक रिश्ता है</header>

<!-- Table Number Input -->
<section>
    <input type="number" id="tableNumber" placeholder="Enter Table Number" required>
</section>

<!-- Menu Section -->
<section class="menu">
    <div class="category">
        <h2>Cold Coffee</h2>
        <ul>
            <li>
                <span>Cold Coffee - ₹15</span>
                <input type="number" class="quantity" id="qty-cold-coffee" value="1" min="1" max="100">
                <button class="cart-btn" onclick="addToCart('Cold Coffee', 15, 'qty-cold-coffee')">Add to Cart</button>
            </li>
            <li>
                <span>Chocolate Cold Coffee - ₹25</span>
                <input type="number" class="quantity" id="qty-choco-coffee" value="1" min="1" max="100">
                <button class="cart-btn" onclick="addToCart('Chocolate Cold Coffee', 25, 'qty-choco-coffee')">Add to Cart</button>
            </li>
        </ul>
    </div>
</section>

<!-- Cart Section -->
<section class="cart">
    <div class="cart-container">
        <h2>Your Cart</h2>
        <ul id="cart-items"></ul>
        <p class="cart-total" id="cart-total">Total: ₹0</p>
        <button class="order-btn" onclick="placeOrder()">Place Order</button>
    </div>
</section>

<script>
let cart = [];

function addToCart(item, price, quantityId) {
    let quantity = parseInt(document.getElementById(quantityId).value);
    if (quantity > 100) {
        alert("You can only order a maximum of 100 per item.");
        return;
    }
    let existingItem = cart.find(cartItem => cartItem.name === item);
    if (existingItem) {
        if (existingItem.quantity + quantity > 100) {
            alert("Maximum limit reached (100 per item).");
            return;
        }
        existingItem.quantity += quantity;
    } else {
        cart.push({ name: item, price: price, quantity: quantity });
    }
    updateCart();
}

function updateCart() {
    let cartList = document.getElementById("cart-items");
    let totalAmount = 0;
    cartList.innerHTML = "";
    cart.forEach((item, index) => {
        totalAmount += item.price * item.quantity;
        let listItem = document.createElement("li");
        listItem.innerHTML = `${item.name} x${item.quantity} - ₹${item.price * item.quantity} 
            <button class="cart-btn" onclick="removeFromCart(${index})">Remove</button>`;
        cartList.appendChild(listItem);
    });
    document.getElementById("cart-total").innerText = `Total: ₹${totalAmount}`;
}

function removeFromCart(index) {
    cart.splice(index, 1);
    updateCart();
}

function placeOrder() {
    let tableNumber = document.getElementById("tableNumber").value;
    if (!tableNumber) {
        alert("Please enter your Table Number before ordering.");
        return;
    }
    if (cart.length === 0) {
        alert("Your cart is empty.");
        return;
    }

    let orderMessage = `Table No: ${tableNumber}\nOrder Details:\n`;
    cart.forEach(item => {
        orderMessage += `${item.name} x${item.quantity} - ₹${item.price * item.quantity}\n`;
    });

    let restaurantPhone = "6355188402"; // Restaurant owner's phone number
    let smsLink = `sms:${restaurantPhone}?body=${encodeURIComponent(orderMessage)}`;

    // Check if the user is on a mobile device
    if (/Mobi|Android/i.test(navigator.userAgent)) {
        // If on mobile, open SMS app with pre-filled message
        window.location.href = smsLink;
    } else {
        // For desktop, show the order details for the user to copy and send manually
        alert(`Order Details:\n\n${orderMessage}\n\nPlease copy this message and send it to the restaurant owner (6355188402).`);
    }

    alert("Order sent successfully!");
    cart = [];
    updateCart();
}
</script>

</body>
</html>
