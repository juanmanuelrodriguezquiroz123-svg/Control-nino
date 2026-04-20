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

    <div style="text-align:center;">
        <div style="width:20px; height:20px; background:#0f0; border-radius:50%; margin:auto; box-shadow:0 0 10px #0f0;"></div>
        <p style="color:#222; font-size:10px;">SISTEMA ACTIVO</p>
    </div>

    <script>
        const firebaseConfig = { databaseURL: "https://micontrol-30a70-default-rtdb.firebaseio.com/" };
        firebase.initializeApp(firebaseConfig);
        const db = firebase.database();

        db.ref('bloqueo').on('value', (snap) => {
            // SI el comando es "SI", bloquea. Si es "NO" (o cualquier otra cosa), desbloquea.
            if (snap.val() === "SI") {
                document.getElementById('bloqueo').style.display = "flex";
            } else {
                document.getElementById('bloqueo').style.display = "none";
            }
        });
    </script>
</body>
</html>
