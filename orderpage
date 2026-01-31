<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>极简点餐系统</title>
    <style>
        :root { --primary-color: #ff4757; --bg-color: #f1f2f6; }
        body { font-family: 'Segoe UI', sans-serif; background: var(--bg-color); margin: 0; display: flex; height: 100vh; }
        
        /* 菜单区域 */
        .menu-container { flex: 2; padding: 20px; overflow-y: auto; display: grid; grid-template-columns: repeat(auto-fill, minmax(220px, 1fr)); gap: 20px; }
        .food-card { background: white; padding: 15px; border-radius: 12px; box-shadow: 0 4px 6px rgba(0,0,0,0.1); text-align: center; display: flex; flex-direction: column; justify-content: space-between; }
        .food-card img { max-width: 100%; height: 150px; object-fit: cover; border-radius: 8px; margin-bottom: 10px; } /* 新增图片样式 */
        .food-card h3 { margin: 10px 0; font-size: 1.2em; }
        .food-card .price { color: var(--primary-color); font-weight: bold; font-size: 1.3em; }
        .add-btn { background: var(--primary-color); color: white; border: none; padding: 8px 15px; border-radius: 20px; cursor: pointer; margin-top: 10px; transition: 0.2s; align-self: center; }
        .add-btn:hover { transform: scale(1.05); }

        /* 购物车区域 */
        .cart-container { flex: 1; background: white; border-left: 2px solid #ddd; display: flex; flex-direction: column; padding: 20px; }
        .cart-items { flex: 1; overflow-y: auto; margin-top: 20px; }
        .cart-item { display: flex; justify-content: space-between; align-items: center; padding: 10px 0; border-bottom: 1px solid #eee; }
        .cart-item button { background: #ff7675; color: white; border: none; padding: 3px 8px; border-radius: 5px; cursor: pointer; font-size: 0.8em; margin-left: 10px; }
        .cart-item button:hover { background: #eb4d4b; }
        .total-section { border-top: 2px solid #eee; padding-top: 20px; }
        .checkout-btn { width: 100%; background: #2ed573; color: white; border: none; padding: 15px; border-radius: 8px; font-size: 1.1em; cursor: pointer; }
        .checkout-btn:hover { background: #28a745; }
    </style>
</head>
<body>

    <div class="menu-container" id="menu">
        </div>

    <div class="cart-container">
        <h2>我的订单 🛒</h2>
        <div class="cart-items" id="cart-items">
            <p style="color: #999;">购物车是空的</p>
        </div>
        <div class="total-section">
            <h3>总计: <span id="total-price">￥0</span></h3>
            <button class="checkout-btn" onclick="checkout()">提交订单</button>
        </div>
    </div>

    <script>
        // 1. 数据定义 (增加了图片链接)
        const foods = [
            { id: 1, name: '招牌红烧肉', price: 58, image: 'https://images.unsplash.com/photo-1549611096-76a0846503b4?crop=entropy&cs=tinysrgb&fit=max&fm=jpg&ixid=MnwzNTY3MHwwfDF8c2VhcmNofDR8fGZhbmN5JTIwZm9vZHxlbnwwfHx8fDE2MTg2NzI4NTU&ixlib=rb-1.2.1&q=80&w=400' }, // 红烧肉
            { id: 2, name: '清蒸鲈鱼', price: 68, image: 'https://images.unsplash.com/photo-1579584288018-2430097c5c0c?crop=entropy&cs=tinysrgb&fit=max&fm=jpg&ixid=MnwzNTY3MHwwfDF8c2VhcmNofDR8fGNvb2tlZCUyMGZpc2h8ZW58MHx8fHwxNjE4Njc3MjU1&ixlib=rb-1.2.1&q=80&w=400' }, // 蒸鱼
            { id: 3, name: '手撕包菜', price: 22, image: 'https://images.unsplash.com/photo-1572018873729-ff45b3648e58?crop=entropy&cs=tinysrgb&fit=max&fm=jpg&ixid=MnwzNTY3MHwwfDF8c2VhcmNofDN8fGdlcm1hbiUyMGZvb2R8ZW58MHx8fHwxNjE4Njc4MzU2&ixlib=rb-1.2.1&q=80&w=400' }, // 包菜
            { id: 4, name: '扬州炒饭', price: 25, image: 'https://images.unsplash.com/photo-1603894520443-4c5750849303?crop=entropy&cs=tinysrgb&fit=max&fm=jpg&ixid=MnwzNTY3MHwwfDF8c2VhcmNofDJ8fGNoaW5lc2UlMjBmcmllZCUyMHJpY2V8ZW58MHx8fHwxNjE4Njc4NDI1&ixlib=rb-1.2.1&q=80&w=400' }, // 炒饭
            { id: 5, name: '冰镇酸梅汤', price: 12, image: 'https://images.unsplash.com/photo-1551025534-8c01e6a72e7b?crop=entropy&cs=tinysrgb&fit=max&fm=jpg&ixid=MnwzNTY3MHwwfDF8c2VhcmNofDR8fGRyaW5rfGVufDB8fHx8MTYxODY3ODQ4Nw&ixlib=rb-1.2.1&q=80&w=400' } // 饮料
        ];

        let cart = [];

        // 2. 初始化菜单渲染 (更新了图片显示)
        const menuEl = document.getElementById('menu');
        foods.forEach(food => {
            menuEl.innerHTML += `
                <div class="food-card">
                    <img src="${food.image}" alt="${food.name}">
                    <h3>${food.name}</h3>
                    <p class="price">￥${food.price}</p>
                    <button class="add-btn" onclick="addToCart(${food.id})">加入订单</button>
                </div>
            `;
        });

        // 3. 购物车逻辑
        function addToCart(foodId) {
            const food = foods.find(f => f.id === foodId);
            const existItem = cart.find(item => item.id === foodId);

            if (existItem) {
                existItem.quantity += 1;
            } else {
                cart.push({ ...food, quantity: 1 });
            }
            renderCart();
        }
        
        // 增加从购物车移除单品的功能
        function removeFromCart(foodId) {
            cart = cart.filter(item => item.id !== foodId);
            renderCart();
        }

        function renderCart() {
            const cartItemsEl = document.getElementById('cart-items');
            const totalEl = document.getElementById('total-price');
            
            if (cart.length === 0) {
                cartItemsEl.innerHTML = '<p style="color: #999;">购物车是空的</p>';
                totalEl.innerText = '￥0';
                return;
            }

            cartItemsEl.innerHTML = cart.map(item => `
                <div class="cart-item">
                    <span>${item.name} x ${item.quantity}</span>
                    <span>
                        ￥${item.price * item.quantity}
                        <button onclick="removeFromCart(${item.id})">移除</button>
                    </span>
                </div>
            `).join('');

            const total = cart.reduce((sum, item) => sum + (item.price * item.quantity), 0);
            totalEl.innerText = `￥${total}`;
        }

        // 4. 结算
        function checkout() {
            if (cart.length === 0) {
                alert("请先点餐！");
                return;
            }
            alert("订单提交成功！感谢光临。");
            cart = [];
            renderCart();
        }
    </script>
</body>
</html>
