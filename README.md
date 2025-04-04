<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Xác Nhận Tiền Ăn</title>
    <style>
        body { font-family: Arial, sans-serif; margin: 20px; }
        table { width: 100%; border-collapse: collapse; margin-top: 20px; }
        th, td { border: 1px solid #ddd; padding: 8px; text-align: left; }
        th { background-color: #f4f4f4; }
        .paid { background-color: #c8e6c9; }
        .unpaid { background-color: #ffcdd2; }
        button { padding: 5px 10px; cursor: pointer; margin-right: 4px; }
        .history { margin-top: 40px; }
    </style>
</head>
<body>
    <h2>Xác Nhận Tiền Ăn</h2>
    <label for="restaurant">Tên Quán: </label>
    <input type="text" id="restaurant" placeholder="Nhập tên quán...">
    
    <table>
        <thead>
            <tr>
                <th>Tên Người Đặt</th>
                <th>Món Ăn</th>
                <th>Giá Tiền</th>
                <th>Trạng Thái</th>
                <th>Hành Động</th>
            </tr>
        </thead>
        <tbody id="order-list">
            <!-- Dữ liệu sẽ được thêm vào đây -->
        </tbody>
    </table>
    
    <h3>Thêm Đơn Hàng</h3>
    <input type="text" id="name" placeholder="Tên người đặt">
    <input type="text" id="food" placeholder="Tên món ăn">
    <input type="number" id="price" placeholder="Giá tiền">
    <button onclick="addOrder()">Thêm</button>

    <div class="history">
        <h3>Lịch Sử Đơn Hàng</h3>
        <ul id="history-list">
            <!-- Lịch sử sẽ hiển thị tại đây -->
        </ul>
    </div>

    <script>
        function addOrder() {
            const name = document.getElementById("name").value;
            const food = document.getElementById("food").value;
            const price = document.getElementById("price").value;
            const restaurant = document.getElementById("restaurant").value || "Chưa rõ";
            
            if (!name || !food || !price) {
                alert("Vui lòng nhập đầy đủ thông tin!");
                return;
            }
            
            const table = document.getElementById("order-list");
            const row = table.insertRow();
            row.innerHTML = `
                <td>${name}</td>
                <td>${food}</td>
                <td>${price} VND</td>
                <td class="unpaid">Chưa thanh toán</td>
                <td>
                    <button onclick="markPaid(this)">Đã thanh toán</button>
                    <button onclick="editOrder(this)">Sửa</button>
                    <button onclick="deleteOrder(this)">Xóa</button>
                </td>
            `;

            const history = document.getElementById("history-list");
            const item = document.createElement("li");
            const now = new Date().toLocaleString();
            item.textContent = `[${now}] ${name} đã đặt món '${food}' với giá ${price} VND tại quán '${restaurant}'.`;
            history.appendChild(item);

            document.getElementById("name").value = "";
            document.getElementById("food").value = "";
            document.getElementById("price").value = "";
        }

        function markPaid(button) {
            const row = button.parentElement.parentElement;
            row.cells[3].innerText = "Đã thanh toán";
            row.cells[3].className = "paid";
            button.remove();
        }

        function deleteOrder(button) {
            const row = button.parentElement.parentElement;
            row.remove();
        }

        function editOrder(button) {
            const row = button.parentElement.parentElement;
            const name = row.cells[0].innerText;
            const food = row.cells[1].innerText;
            const price = row.cells[2].innerText.replace(" VND", "");

            document.getElementById("name").value = name;
            document.getElementById("food").value = food;
            document.getElementById("price").value = price;

            deleteOrder(button);
        }
    </script>
</body>
</html>
