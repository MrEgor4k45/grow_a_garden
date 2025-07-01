<button id="google-signin-btn">Войти через Google</button>
<script src="https://www.gstatic.com/firebasejs/9.22.1/firebase-app-compat.js"></script>
<script src="https://www.gstatic.com/firebasejs/9.22.1/firebase-auth-compat.js"></script>
<script>
  const firebaseConfig = { /* твой конфиг */ };
  firebase.initializeApp(firebaseConfig);
  const auth = firebase.auth();

  document.getElementById("google-signin-btn").addEventListener("click", () => {
    const provider = new firebase.auth.GoogleAuthProvider();
    auth.signInWithPopup(provider).catch(err => alert("Ошибка входа: " + err.message));
  });
</script>
