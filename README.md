<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Transmisión ESP32-CAM</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            margin: 0;
            padding: 20px;
            background-color: #f0f0f0;
        }
        h1 {
            color: #333;
        }
        .video-container {
            margin: 20px auto;
            max-width: 800px;
            background-color: #fff;
            padding: 10px;
            border-radius: 8px;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
        }
        .controls {
            margin: 20px auto;
            max-width: 800px;
            padding: 15px;
            background-color: #fff;
            border-radius: 8px;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
        }
        button {
            background-color: #4CAF50;
            color: white;
            border: none;
            padding: 10px 15px;
            margin: 5px;
            border-radius: 4px;
            cursor: pointer;
            font-size: 16px;
        }
        button:hover {
            background-color: #45a049;
        }
        #videoStream {
            width: 100%;
            max-height: 600px;
            background-color: #000;
        }
    </style>
</head>
<body>
    <h1>Transmisión en vivo ESP32-CAM</h1>
    
    <div class="video-container">
        <img id="videoStream" src="" alt="Transmisión en vivo">
    </div>
    
    <div class="controls">
        <button id="startStream">Iniciar Transmisión</button>
        <button id="stopStream">Detener Transmisión</button>
        <button id="capturePhoto">Capturar Foto</button>
    </div>
    
    <script>
        // Configuración - Reemplaza con la IP de tu ESP32-CAM
        const esp32camIP = "192.168.1.100"; 
        let streamInterval;
        
        // Elementos del DOM
        const videoStream = document.getElementById('videoStream');
        const startBtn = document.getElementById('startStream');
        const stopBtn = document.getElementById('stopStream');
        const captureBtn = document.getElementById('capturePhoto');
        
        // Iniciar transmisión
        startBtn.addEventListener('click', () => {
            videoStream.src = `http://${esp32camIP}/stream`;
            startBtn.disabled = true;
            stopBtn.disabled = false;
        });
        
        // Detener transmisión
        stopBtn.addEventListener('click', () => {
            videoStream.src = "";
            startBtn.disabled = false;
            stopBtn.disabled = true;
        });
        
        // Capturar foto
        captureBtn.addEventListener('click', () => {
            if(videoStream.src) {
                const link = document.createElement('a');
                link.href = `http://${esp32camIP}/capture?_cb=${Date.now()}`;
                link.target = '_blank';
                link.click();
            } else {
                alert("Primero inicia la transmisión");
            }
        });
        
        // Deshabilitar botón de detener al cargar la página
        stopBtn.disabled = true;
    </script>
</body>
</html>
