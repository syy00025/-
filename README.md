<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Creative Clock and Calendar</title>
    <style>
        .container {
            text-align: center;
            font-family: Arial, sans-serif;
        }
        /* Clock Styles */
        .clock {
            width: 200px;
            height: 200px;
            background: linear-gradient(135deg, #e0eafc, #cfdef3); /* Gradient background */
            border-radius: 50%;
            position: relative;
            border: 8px solid #2c3e50;
            box-shadow: 0 0 20px rgba(0,0,0,0.3);
            margin: 20px auto;
        }
        .hand {
            position: absolute;
            bottom: 50%;
            left: 50%;
            transform-origin: bottom;
            background: #2c3e50;
        }
        .hour-hand {
            width: 6px;
            height: 50px;
            border-radius: 3px;
        }
        .minute-hand {
            width: 4px;
            height: 70px;
            border-radius: 2px;
        }
        .second-hand {
            width: 2px;
            height: 85px;
            background: #e74c3c; /* Vibrant red */
            border-radius: 1px;
        }
        .center {
            width: 12px;
            height: 12px;
            background: #2c3e50;
            border-radius: 50%;
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            box-shadow: 0 0 10px rgba(0,0,0,0.5);
        }
        .marker {
            position: absolute;
            width: 8px;
            height: 8px;
            background: #3498db; /* Blue glowing dots */
            border-radius: 50%;
            box-shadow: 0 0 5px #3498db, 0 0 10px #3498db;
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
            color: #2c3e50;
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
        <!-- Clock -->
        <div class="clock">
            <div class="hand hour-hand"></div>
            <div class="hand minute-hand"></div>
            <div class="hand second-hand"></div>
            <div class="center"></div>
            <!-- Hour Markers (12 dots) -->
            <div class="marker" style="top: 10px; left: 50%; transform: translateX(-50%);"></div>
            <div class="marker" style="top: 20px; right: 20%; transform: translateY(-50%);"></div>
            <div class="marker" style="top: 50%; right: 10px; transform: translateY(-50%);"></div>
            <div class="marker" style="bottom: 20%; right: 20px; transform: translateY(50%);"></div>
            <div class="marker" style="bottom: 10px; left: 50%; transform: translateX(-50%);"></div>
            <div class="marker" style="bottom: 20%; left: 20px; transform: translateY(50%);"></div>
            <div class="marker" style="top: 50%; left: 10px; transform: translateY(-50%);"></div>
            <div class="marker" style="top: 20%; left: 20px; transform: translateY(-50%);"></div>
            <div class="marker" style="top: 20%; right: 60%; transform: translateY(-50%);"></div>
            <div class="marker" style="top: 20%; left: 60%; transform: translateY(-50%);"></div>
            <div class="marker" style="bottom: 20%; right: 60%; transform: translateY(50%);"></div>
            <div class="marker" style="bottom: 20%; left: 60%; transform: translateY(50%);"></div>
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
