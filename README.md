<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Clock and Calendar</title>
    <style>
        .container {
            text-align: center;
            font-family: Arial, sans-serif;
        }
        /* Clock Styles */
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
            color: #333;
            text-align: center;
            width: 20px;
        }
        /* Calendar Styles */
        .calendar {
            display: inline-block;
            margin-top: 20px;
        }
        .month {
            font-size: 20px;
            font-weight: bold;
            margin-bottom: 10px;
        }
        table {
            border-collapse: collapse;
            background: #fff;
            box-shadow: 0 0 10px rgba(0,0,0,0.1);
        }
        th, td {
            width: 40px;
            height: 40px;
            text-align: center;
            border: 1px solid #ddd;
        }
        th {
            background: #333;
            color: #fff;
        }
        td {
            background: #f9f9f9;
        }
        .today {
            background: #ff4444;
            color: #fff;
            font-weight: bold;
        }
    </style>
</head>
<body>
    <div class="container">
        <!-- Clock -->
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

        <!-- Calendar -->
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
    </div>

    <script>
        // Clock Update
        function updateClock() {
            const now = new Date();
            const hours = now.getHours() % 12;
            const minutes = now.getMinutes();
            const seconds = now.getSeconds();

            const hourDeg = (hours * 30) + (minutes * 0.5);
            const minuteDeg = minutes * 6;
            const secondDeg = seconds * 6;

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
            for (let i = 0; i < 6; i++) { // Max 6 weeks
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
