<!DOCTYPE html>
<html lang="zh">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>超时费用计算器</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            padding: 20px;
            background-color: #f4f4f4;
            text-align: center;
        }
        input, button, select {
            padding: 10px;
            margin: 10px;
            font-size: 16px;
        }
        .result {
            margin-top: 20px;
            font-size: 20px;
            color: #333;
        }
    </style>
</head>
<body>
    <h1>超时费用计算器</h1>
    <div>
        <label for="landingTime">航班降落时间 (格式: HH:MM):</label>
        <input type="time" id="landingTime" required>
    </div>
    <div>
        <label for="customerExitTime">客人出来时间 (格式: HH:MM):</label>
        <input type="time" id="customerExitTime" required>
    </div>
    <div>
        <label for="waitingTime">免费等待时间:</label>
        <select id="waitingTime">
            <option value="45">45分钟</option>
            <option value="60">60分钟</option>
            <option value="90" selected>90分钟</option>
        </select>
    </div>
    <button onclick="calculateOverage()">计算超时费用</button>
    <div class="result" id="result"></div>

    <script>
        function calculateOverage() {
            const landingTime = document.getElementById("landingTime").value;
            const customerExitTime = document.getElementById("customerExitTime").value;
            const waitingMinutes = parseInt(document.getElementById("waitingTime").value);

            // 计算免费等待时间结束时间
            const freeEndTime = new Date();
            freeEndTime.setHours(landingTime.split(":")[0], parseInt(landingTime.split(":")[1]) + waitingMinutes);

            // 客人出来时间
            const customerExit = new Date();
            customerExit.setHours(customerExitTime.split(":")[0], customerExitTime.split(":")[1]);

            // 计算超时分钟数
            const diffMs = customerExit - freeEndTime; // 时间差（毫秒）
            const diffMins = Math.ceil(diffMs / 60000); // 时间差（分钟）

            if (diffMins > 0) {
                // 每10分钟收费$10，向上取整
                const overageUnits = Math.ceil(diffMins / 10);
                const overageCost = overageUnits * 10;
                document.getElementById("result").innerHTML = `超时时间: ${diffMins}分钟<br>超时费用: $${overageCost} AUD`;
            } else {
                document.getElementById("result").innerHTML = "没有超时。";
            }
        }
    </script>
</body>
</html>
