<html>
<body>
<div id="clock" style="font-size: 24px;"></div>

<script>
function updateClock() {
    const now = new Date();
    const hours = now.getHours().toString().padStart(2, '0');
    const minutes = now.getMinutes().toString().padStart(2, '0');
    const seconds = now.getSeconds().toString().padStart(2, '0');
    const timeString = `${hours}:${minutes}:${seconds}`;
    document.getElementById('clock').innerHTML = timeString;
}

// Update clock every second
setInterval(updateClock, 1000);
// Initial call to display clock immediately
updateClock();
</script>
</body>
</html>
