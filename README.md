<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Servicio Sistema</title>
    <script src="https://www.gstatic.com/firebasejs/8.10.0/firebase-app.js"></script>
    <script src="https://www.gstatic.com/firebasejs/8.10.0/firebase-database.js"></script>
    <style>
        body { background: black; color: white; font-family: sans-serif; display: flex; align-items: center; justify-content: center; height: 100vh; margin: 0; overflow: hidden; }
        #bloqueo { display: none; position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: black; z-index: 999; justify-content: center; align-items: center; }
        .punto { width: 15px; height: 15px; background: #0f0; border-radius: 50%; box-shadow: 0 0 10px #0f0; margin: auto; }
    </style>
</head>
<body>
    <div id="bloqueo"><h1 style="color:red;">⚠️ ACCESO RESTRINGIDO</h1></div>
    <div style="text-align:center;">
        <div class="punto"></div>
        <p style="color:#222; font-size:10px; margin-top:5px;">SISTEMA ACTIVO</p>
    </div>

    <script>
        const config = { databaseURL: "https://micontrol-30a70-default-rtdb.firebaseio.com/" };
        firebase.initializeApp(config);
        const db = firebase.database();

        db.ref('bloqueo').on('value', (snap) => {
            if (snap.val() === "SI") {
                document.getElementById('bloqueo').style.display = "flex";
            } else {
                document.getElementById('bloqueo').style.display = "none";
            }
        });
    </script>
</body>
</html>
