<!DOCTYPE html>
<html lang="nl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Verhuurders Netwerk Oost-Algarve</title>
</head>
<body>

    <main id="app-container">
        <section id="Verwijzingen" class="module actief">
            <header>
                <h1>🏘️ Verwijzingen Prikbord</h1>
                <button onclick="toonFormulier()">+ Aanbieden</button>
            </header>
            <div id="verwijzingen-lijst">
                </div>
            
            <div id="verwijzing-formulier" style="display: none;">
                </div>
        </section>

        <section id="Netwerk" class="module" style="display: none;">
            <h1>👥 Verhuurders Netwerk</h1>
            <div id="netwerk-lijst">
                </div>
        </section>

        <section id="Kennisbank" class="module" style="display: none;">
            <h1>📚 Kennisbank</h1>
            <div id="kennisbank-lijst">
                </div>
        </section>

        <section id="Berichten" class="module" style="display: none;">
            <h1>✉️ Berichten Inbox</h1>
            </section>
    </main>

    <nav id="nav-bar">
        <button onclick="wisselModule('Verwijzingen')">Verwijzingen</button>
        <button onclick="wisselModule('Netwerk')">Netwerk</button>
        <button onclick="wisselModule('Kennisbank')">Kennisbank</button>
        <button onclick="wisselModule('Berichten')">Berichten</button>
    </nav>
    
    <style>
        /* CSS voor mobile-first, kaarten en knoppen */
        body { font-family: sans-serif; margin: 0; background-color: #f8f8f8; }
        #app-container { padding: 10px; padding-bottom: 60px; }
        h1 { margin-top: 0; }
        
        /* Navigatiebalk */
        #nav-bar {
            position: fixed; bottom: 0; left: 0; right: 0;
            display: flex; justify-content: space-around;
            background-color: #ffffff; border-top: 1px solid #ddd;
            padding: 8px 0;
        }
        #nav-bar button {
            border: none; background: none; padding: 10px; cursor: pointer;
        }
        
        /* Kaart Styling (voorbeeld) */
        .verwijzing-kaart, .netwerk-kaart, .kennisbank-item {
            background-color: white; border-radius: 8px;
            box-shadow: 0 2px 4px rgba(0,0,0,0.1);
            margin-bottom: 15px; padding: 15px;
        }
        .knop-primair {
            background-color: #E67E22; color: white; border: none;
            padding: 10px 15px; border-radius: 5px; width: 100%; cursor: pointer;
        }
    </style>

    <script>
        // === 1. SIMULATIE VAN DATA (localStorage) ===
        localStorage.setItem('gebruikersData', JSON.stringify([
            { id: 101, Naam_Bedrijf: 'Villa Verde Tavira', Locatie_Focus: 'Tavira', Geverifieerd: true },
            { id: 102, Naam_Bedrijf: 'Appartementen Olhão', Locatie_Focus: 'Olhão', Geverifieerd: false },
        ]));
        localStorage.setItem('verwijzingen', JSON.stringify([
            { id: 1, titel: 'Koppel zoekt T2', startdatum: '2026-01-01', aangebodenDoor: 'Verhuurder X', status: 'Open' },
        ]));

        // === 2. NAVIGATIE LOGICA ===
        function wisselModule(moduleId) {
            document.querySelectorAll('.module').forEach(sec => {
                sec.style.display = 'none';
            });
            document.getElementById(moduleId).style.display = 'block';
            // Start de laadfunctie van de module wanneer deze geopend wordt
            if (moduleId === 'Verwijzingen') prikbordLaden();
            if (moduleId === 'Netwerk') netwerkLaden();
            if (moduleId === 'Kennisbank') kennisbankLaden();
            // Berichten is hier weggelaten voor de eenvoud
        }
        
        // === 3. MODULE LAADFUNCTIES (Kernen van de app) ===
        
        // Laadt de 'Verwijzingen' (Prikbord)
        function prikbordLaden() {
            const lijst = document.getElementById('verwijzingen-lijst');
            lijst.innerHTML = ''; // Leegmaken voor herladen
            const verwijzingen = JSON.parse(localStorage.getItem('verwijzingen')) || [];
            
            verwijzingen.forEach(post => {
                const kaartHTML = `
                    <article class="verwijzing-kaart">
                        <h3>${post.titel}</h3>
                        <p>Aangeboden door: ${post.aangebodenDoor}</p>
                        <button class="knop-secundair">Reageer</button>
                    </article>
                `;
                lijst.innerHTML += kaartHTML;
            });
        }
        
        // Laadt het 'Netwerk' (Profielen Lijst)
        function netwerkLaden() {
            const lijst = document.getElementById('netwerk-lijst');
            lijst.innerHTML = '';
            const gebruikers = JSON.parse(localStorage.getItem('gebruikersData')) || [];

            gebruikers.forEach(gebruiker => {
                const badge = gebruiker.Geverifieerd ? '<span style="color: green;">✔</span>' : '';
                const kaartHTML = `
                    <article class="netwerk-kaart">
                        <h3>${gebruiker.Naam_Bedrijf} ${badge}</h3>
                        <p>Locatie: ${gebruiker.Locatie_Focus}</p>
                        <button class="knop-profiel">Bekijk Profiel</button>
                    </article>
                `;
                lijst.innerHTML += kaartHTML;
            });
        }
        
        // Eerste keer laden bij het opstarten
        window.onload = function() {
            prikbordLaden();
        }
    </script>
</body>
</html>
