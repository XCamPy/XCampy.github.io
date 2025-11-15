<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Redirection OAuth2 Discord</title>
</head>
<body>
    <h1>Authentification Discord en cours...</h1>
    <p>Nous traitons votre demande...</p>
    <script>
        // Récupérer le code de l'URL
        const urlParams = new URLSearchParams(window.location.search);
        const code = urlParams.get('code');
        if (code) {
            // Préparer la requête pour échanger le code contre un token Discord
            const clientId = process.env.DISCORD_CLIENT_ID;  // Utilisation de la variable d'environnement
            const clientSecret = process.env.DISCORD_CLIENT_SECRET;  // Utilisation de la variable d'environnement
            const redirectUri = "https://ton-github-page.com";  // L'URL de redirection de ton OAuth2 (ton GitHub Page)
            // Faire la requête pour obtenir le token
            fetch('https://discord.com/api/oauth2/token', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/x-www-form-urlencoded'
                },
                body: new URLSearchParams({
                    'client_id': clientId,
                    'client_secret': clientSecret,
                    'grant_type': 'authorization_code',
                    'code': code,
                    'redirect_uri': redirectUri,
                    'scope': 'identify'  // Choisis les scopes nécessaires ici
                })
            })
            .then(response => response.json())
            .then(data => {
                if (data.access_token) {
                    // Si le token est récupéré, redirige vers la page finale
                    window.location.href = `https://ton-dashboard.com/success?token=${data.access_token}`;
                } else {
                    // Si une erreur survient, affiche un message
                    alert("Erreur lors de la récupération du token. Code invalide ou expiré.");
                }
            })
            .catch((error) => {
                // En cas d'erreur réseau ou autre
                console.error('Erreur lors de la requête :', error);
                alert("Une erreur est survenue. Veuillez réessayer.");
            });
        } else {
            // Si le code n'est pas présent dans l'URL
            alert("Code d'autorisation manquant dans l'URL.");
        }
    </script>
</body>
</html>
