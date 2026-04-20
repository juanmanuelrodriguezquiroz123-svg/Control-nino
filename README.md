<!DOCTYPE html>
<html>
<head>
    <title>Servicio Activo</title>
    <script src="https://www.gstatic.com/firebasejs/8.10.0/firebase-app.js"></script>
    <script src="https://www.gstatic.com/firebasejs/8.10.0/firebase-database.js"></script>
</head>
<body style="background:black; color:white; text-align:center; padding-top:50px; font-family:sans-serif;">
    <div id="bloqueo" style="display:none; position:fixed; top:0; left:0; width:100%; height:100%; background:black; z-index:99; justify-content:center; align-items:center;">
        <h1 style="color:red;">BLOQUEADO</h1>
    </div>
    <div style="width:15px; height:15px; background:#0f0; border-radius:50%; margin:auto; box-shadow:0 0 10px #0f0;"></div>
    <p style="color:#222; font-size:10px;">SISTEMA ACTIVO</p>

    <script>
        const config = { databaseURL: "https://micontrol-30a70-default-rtdb.firebaseio.com/" };
        firebase.initializeApp(config);
        const db = firebase.database();

        db.ref('bloqueo').on('value', (snap) => {
            document.getElementById('bloqueo').style.display = (snap.val() === "SI") ? "flex" : "none";
        });
    </script>
</body>
</html>
