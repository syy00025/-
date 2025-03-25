<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Time Display</title> <!-- Custom title to avoid "clock-widget" -->
    <style>
        body {
            margin: 0;
            padding: 0;
            display: flex;
            justify-content: center;
            align-items: flex-start; /* Align to the top */
            min-height: 100vh;
            background: #f5f5f5;
        }
        .container {
            display: flex;
            align-items: center; /* Center vertically within the container */
            font-family: Arial, sans-serif;
            transform: scale(0.6); /* Keep the 60% scale */
            transform-origin: top center;
        }
        /* Clock Container (Digital + Analog) */
        .clock-container {
            display: flex;
            flex-direction: column;
            align-items: center;
            margin-left: 20px;
        }
        /* Digital Clock Styles */
        .digital-clock {
            font-size: 58px;
            font-family: 'Courier New', monospace;
            color: #2c3e50;
            background: #ecf0f1;
            padding: 12px 24px;
            border-radius: 10px;
            box-shadow: 0 0 15px rgba(0,0,0,0.2);
            margin-bottom: 12px;
        }
        /* Analog Clock Styles */
        .analog-clock {
            width: 144px; /* Match the height to make it round */
            height: 144px; /* Already at 144px */
            background: linear-gradient(135deg, #dfe4ea, #ced6e0);
            border-radius: 50%;
            position: relative;
            border: 5px solid #2c3e50;
            box-shadow: 0 0 15px rgba(0,0,0,0.2);
        }
        .hand {
            position: absolute;
            bottom: 50%;
            left: 50%;
            transform-origin: bottom;
            background: #2c3e50;
        }
        .hour-hand {
            width: 4px;
            height: 36px;
            border-radius: 2px;
        }
        .minute-hand {
            width: 3px;
            height: 48px;
            border-radius: 1.5px;
        }
        .second-hand {
            width: 1px;
            height: 54px;
            background: #e74c3c;
            border-radius: 0.5px;
        }
        .center {
            width: 8px;
            height: 8px;
            background: #2c3e50;
            border-radius: 50%;
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            box-shadow: 0 0 5px rgba(0,0,0,0.5);
        }
        /* Calendar Styles */
        .calendar {
            display: inline-block;
            text-align: center;
        }
        .month {
            font-size: 24px;
            font-weight: bold;
            margin-bottom: 12px;
            color: #2c3e50;
        }
        table {
            border-collapse: collapse;
            background: #fff;
            box-shadow: 0 0 10px rgba(0,0,0,0.1);
        }
        th, td {
            width: 40px;
            height: 48px;
            text-align: center;
            border: 1px solid #ddd;
        }
        th {
            background: #2c3e50;
            color: #fff;
        }
        td {
            background: #f9f9f9;
        }
        .today {
            background: #e74c3c;
            color: #fff;
            font-weight: bold;
        }
    </style>
</head>
<body>
    <div class="container">
        <!-- Calendar on the Left -->
        <div class="calendar">
            <div class="month" id="month-year"></div>
            <table>
                <thead>
                    <tr>
                        <th>Su</th><th>Mo</th><th>Tu</th><th>We</th><th>Th</th><th>Fr</th><th>Sa</th>
                    </tr>
                </thead>
                <tbody id="calendar-body"></tbody>
            </table>
        </div>

        <!-- Clock Section on the Right -->
        <div class="clock-container">
            <!-- Digital Clock -->
            <div class="digital-clock" id="digital-time"></div>
            <!-- Analog Clock -->
            <div class="analog-clock">
                <div class="hand hour-hand"></div>
                <div class="hand minute-hand"></div>
                <div class="hand second-hand"></div>
                <div class="center"></div>
            </div>
        </div>
    </div>

    <script>
        // Clock Update (Digital and Analog)
        function updateClock() {
            const now = new Date();
            const hours = now.getHours().toString().padStart(2, '0');
            const minutes = now.getMinutes().toString().padStart(2, '0');
            const seconds = now.getSeconds().toString().padStart(2, '0');

            // Update Digital Clock
            document.getElementById('digital-time').textContent = `${hours}:${minutes}:${seconds}`;

            // Update Analog Clock
            const analogHours = now.getHours() % 12;
            const analogMinutes = now.getMinutes();
            const analogSeconds = now.getSeconds();

            const hourDeg = (analogHours * 30) + (analogMinutes * 0.5);
            const minuteDeg = analogMinutes * 6;
            const secondDeg = analogSeconds * 6;

            document.querySelector('.hour-hand').style.transform = `translateX(-50%) rotate(${hourDeg}deg)`;
            document.querySelector('.minute-hand').style.transform = `translateX(-50%) rotate(${minuteDeg}deg)`;
            document.querySelector('.second-hand').style.transform = `translateX(-50%) rotate(${secondDeg}deg)`;
        }

        // Calendar Generation
        function generateCalendar() {
            const now = new Date();
            const year = now.getFullYear();
            const month = now.getMonth();
            const today = now.getDate();

            const months = ["January", "February", "March", "April", "May", "June", 
                          "July", "August", "September", "October", "November", "December"];
            document.getElementById('month-year').textContent = `${months[month]} ${year}`;

            const firstDay = new Date(year, month, 1).getDay();
            const daysInMonth = new Date(year, month + 1, 0).getDate();

            let calendarBody = document.getElementById('calendar-body');
            calendarBody.innerHTML = '';

            let date = 1;
            for (let i = 0; i < 6; i++) {
                let row = document.createElement('tr');
                for (let j = 0; j < 7; j++) {
                    let cell = document.createElement('td');
                    if (i === 0 && j < firstDay) {
                        cell.textContent = '';
                    } else if (date > daysInMonth) {
                        break;
                    } else {
                        cell.textContent = date;
                        if (date === today) {
                            cell.classList.add('today');
                        }
                        date++;
                    }
                    row.appendChild(cell);
                }
                calendarBody.appendChild(row);
                if (date > daysInMonth) break;
            }
        }

        // Initial calls and updates
        setInterval(updateClock, 1000);
        updateClock();
        generateCalendar();
    </script>
</body>
</html>
