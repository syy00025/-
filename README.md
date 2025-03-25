<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Analog Clock</title>
    <style>
        .clock {
            width: 200px;
            height: 200px;
            background: #f0f0f0;
            border-radius: 50%;
            position: relative;
            border: 10px solid #333;
            box-shadow: 0 0 20px rgba(0,0,0,0.2);
            margin: 20px auto;
        }
        .hand {
            position: absolute;
            bottom: 50%;
            left: 50%;
            transform-origin: bottom;
            background: #333;
        }
        .hour-hand {
            width: 6px;
            height: 60px;
        }
        .minute-hand {
            width: 4px;
            height: 80px;
        }
        .second-hand {
            width: 2px;
            height: 90px;
            background: #ff4444;
        }
        .center {
            width: 10px;
            height: 10px;
            background: #333;
            border-radius: 50%;
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
        }
        .number {
            position: absolute;
            font-size: 18px;
            font-family: Arial, sans-serif;
            color: #333;
            text-align: center;
            width: 20px;
        }
    </style>
</head>
<body>
    <div class="clock">
        <div class="hand hour-hand"></div>
        <div class="hand minute-hand"></div>
        <div class="hand second-hand"></div>
        <div class="center"></div>
        <div class="number" style="top: 10px; left: 50%; transform: translateX(-50%);">12</div>
        <div class="number" style="bottom: 10px; left: 50%; transform: translateX(-50%);">6</div>
        <div class="number" style="top: 50%; right: 10px; transform: translateY(-50%);">3</div>
        <div class="number" style="top: 50%; left: 10px; transform: translateY(-50%);">9</div>
    </div>

    <script>
        function updateClock() {
            const now = new Date();
            const hours = now.getHours() % 12; // Convert to 12-hour format
            const minutes = now.getMinutes();
            const seconds = now.getSeconds();

            const hourDeg = (hours * 30) + (minutes * 0.5); // 30° per hour + 0.5° per minute
            const minuteDeg = minutes * 6; // 6° per minute
            const secondDeg = seconds * 6; // 6° per second

            document.querySelector('.hour-hand').style.transform = `translateX(-50%) rotate(${hourDeg}deg)`;
            document.querySelector('.minute-hand').style.transform = `translateX(-50%) rotate(${minuteDeg}deg)`;
            document.querySelector('.second-hand').style.transform = `translateX(-50%) rotate(${secondDeg}deg)`;
        }

        setInterval(updateClock, 1000);
        updateClock();
    </script>
</body>
</html>
