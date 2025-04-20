# -<!DOCTYPE html>
<html>
<head>
    <title>هدى على الطريق</title>
    <link rel="stylesheet" href="style.css">
    <script src="https://d3js.org/d3.v7.min.js"></script>
</head>
<body>
    <div id="gameContainer">
        <div id="score">النقاط: 0</div>
        <svg id="gameBoard"></svg>
    </div>
    <script src="game.js"></script>
</body>
</html>
#gameContainer {
    width: 800px;
    margin: 0 auto;
}

#gameBoard {
    width: 800px;
    height: 600px;
    background: url('road-background.png');
    position: relative;
}

.car {
    fill: #FF69B4; /* لون السيارة */
}

.obstacle {
    fill: #FF0000; /* لون العقبات */
}

.hijab {
    fill: #FFFFFF; /* لون الحجاب */
}
const width = 800;
const height = 600;
let score = 0;
let carPosition = width / 2;

// إنشاء عناصر اللعبة
const svg = d3.select("#gameBoard")
    .attr("width", width)
    .attr("height", height);

// رسم السيارة مع الحجاب
const car = svg.append("g")
    .attr("transform", `translate(${carPosition}, ${height - 100})`);

car.append("rect") // هيكل السيارة
    .attr("class", "car")
    .attr("width", 50)
    .attr("height", 30);

car.append("circle") // الحجاب
    .attr("class", "hijab")
    .attr("cx", 25)
    .attr("cy", -10)
    .attr("r", 15);

// نظام العقبات
function createObstacle() {
    svg.append("rect")
        .attr("class", "obstacle")
        .attr("x", Math.random() * (width - 30))
        .attr("y", 0)
        .attr("width", 30)
        .attr("height", 30)
        .transition()
        .duration(2000)
        .attr("y", height)
        .remove();
}

// التحكم بالسيارة
document.addEventListener("keydown", (e) => {
    if (e.key === "ArrowLeft" && carPosition > 50) carPosition -= 20;
    if (e.key === "ArrowRight" && carPosition < width - 50) carPosition += 20;
    
    car.attr("transform", `translate(${carPosition}, ${height - 100})`);
});

// نظام النقاط والتصادم
setInterval(() => {
    score++;
    d3.select("#score").text(`النقاط: ${score}`);
}, 1000);

// إنشاء عقبات كل 2 ثانية
setInterval(createObstacle, 2000);
function checkCollision() {
    const obstacles = document.querySelectorAll(".obstacle");
    obstacles.forEach(obstacle => {
        const obsX = +obstacle.getAttribute("x");
        if (Math.abs(obsX - carPosition) < 40) {
            alert("Game Over! النقاط: " + score);
            location.reload();
        }
    });
}

setInterval(checkCollision, 100);
// في CSS:
.road-line {
    fill: #FFFF00;
}

// في JS:
function createRoad() {
    svg.append("rect")
        .attr("class", "road-line")
        .attr("x", width / 2 - 5)
        .attr("y", 0)
        .attr("width", 10)
        .attr("height", 30)
        .transition()
        .duration(2000)
        .attr("y", height)
        .remove();
}

setInterval(createRoad, 500);
لعبة سيارة 
