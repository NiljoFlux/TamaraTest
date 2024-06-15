<!DOCTYPE html>
<html lang="de">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Drag and Drop Beispiel</title>
    <style>
        #container {
            width: 500px;
            height: 500px;
            border: 2px solid #000;
            position: relative;
        }
        .draggable {
            width: 50px;
            height: 50px;
            position: absolute;
            cursor: move;
        }
        .rectangle {
            background-color: red;
            height: 50px; /* Höhe eines Rechtecks */
            width: 100px; /* Breite eines Rechtecks */
        }
        .square {
            background-color: blue;
            height: 50px; /* Höhe eines Quadrats */
            width: 50px; /* Breite eines Quadrats */
        }
    </style>
</head>
<body>
    <div id="container">
        <div class="draggable rectangle" style="top: 10px; left: 10px;"></div>
        <div class="draggable rectangle" style="top: 70px; left: 10px;"></div>
        <div class="draggable rectangle" style="top: 130px; left: 10px;"></div>
        <div class="draggable rectangle" style="top: 190px; left: 10px;"></div>
        <div class="draggable square" style="top: 250px; left: 10px;"></div>
        <div class="draggable square" style="top: 310px; left: 10px;"></div>
        <div class="draggable square" style="top: 370px; left: 10px;"></div>
        <div class="draggable square" style="top: 430px; left: 10px;"></div>
    </div>

    <script>
        document.querySelectorAll('.draggable').forEach(item => {
            let isDragging = false;
            let isRotating = false;
            let rotationAngle = 0;

            item.addEventListener('mousedown', function(e) {
                if (e.button === 2) { // Right-click for rotation
                    isRotating = true;
                    document.addEventListener('mousemove', rotateElement);
                    document.addEventListener('mouseup', stopRotatingElement);
                } else {
                    isDragging = true;
                    let shiftX = e.clientX - item.getBoundingClientRect().left;
                    let shiftY = e.clientY - item.getBoundingClientRect().top;

                    function moveAt(pageX, pageY) {
                        item.style.left = pageX - shiftX + 'px';
                        item.style.top = pageY - shiftY + 'px';
                    }

                    function onMouseMove(event) {
                        if (isDragging) {
                            moveAt(event.pageX, event.pageY);
                        }
                    }

                    document.addEventListener('mousemove', onMouseMove);

                    item.onmouseup = function() {
                        document.removeEventListener('mousemove', onMouseMove);
                        isDragging = false;
                        item.onmouseup = null;
                    };
                }
            });

            function rotateElement(event) {
                rotationAngle += event.movementX;
                item.style.transform = `rotate(${rotationAngle}deg)`;
            }

            function stopRotatingElement() {
                document.removeEventListener('mousemove', rotateElement);
                isRotating = false;
            }

            item.ondragstart = function() {
                return false;
            };
        });

        document.addEventListener('contextmenu', event => event.preventDefault());
    </script>
</body>
</html>
