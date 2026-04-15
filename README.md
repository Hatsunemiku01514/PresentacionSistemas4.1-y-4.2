<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Unidad 4: Arreglos en Java - Frutiger Aero</title>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Ubuntu:wght@400;500;700&family=Open+Sans:wght@400;600;700&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.1/css/all.min.css">
    
    <style>
/* =========================================
   1. THE SETUP & FRUTIGER AERO BACKGROUND
   ========================================= */
* {
    box-sizing: border-box;
}

body {
    margin: 0;
    padding: 0;
    height: 100vh;
    width: 100vw;
    overflow: hidden;
    display: flex;
    justify-content: center;
    align-items: center;
    /* Frutiger aero vibrant gradient */
    background: linear-gradient(135deg, #a8edea 0%, #80deea 30%, #a5d6a7 70%, #dcedc1 100%);
    font-family: 'Open Sans', sans-serif;
    position: relative;
}

/* Floating ambient bubbles */
body::before, body::after {
    content: '';
    position: fixed;
    border-radius: 50%;
    background: radial-gradient(circle at 30% 30%, rgba(255, 255, 255, 0.8), rgba(255, 255, 255, 0.1));
    box-shadow: 0 0 20px rgba(255,255,255,0.4), inset 0 0 20px rgba(255,255,255,0.8);
    z-index: 0;
}
body::before { width: 400px; height: 400px; top: -100px; left: -100px; }
body::after { width: 250px; height: 250px; bottom: 5%; right: 5%; }

/* =========================================
   2. PRESENTATION WRAPPER & SLIDE SYSTEM
   ========================================= */
#presentation-area {
    position: relative;
    width: 1280px;
    height: 720px;
    z-index: 1;
}

.slide-container {
    position: absolute;
    top: 0;
    left: 0;
    width: 1280px;
    height: 720px;
    margin: 0;
    padding: 0;
    opacity: 0;
    pointer-events: none;
    transition: opacity 0.6s ease, transform 0.6s cubic-bezier(0.25, 0.8, 0.25, 1);
    transform: scale(0.95);
    /* Glassmorphism effect */
    background: rgba(255, 255, 255, 0.45);
    backdrop-filter: blur(15px);
    -webkit-backdrop-filter: blur(15px);
    border: 1px solid rgba(255, 255, 255, 0.8);
    border-radius: 24px;
    box-shadow: 0 12px 32px 0 rgba(0, 150, 136, 0.15), inset 0 2px 5px rgba(255,255,255,0.8);
    display: flex;
    flex-direction: column;
    overflow: hidden;
}

.slide-container.active {
    opacity: 1;
    pointer-events: auto;
    transform: scale(1);
}

.slide-inner {
    padding: 50px 60px;
    height: 100%;
    width: 100%;
    display: flex;
    flex-direction: column;
}

/* =========================================
   3. CONSISTENT TYPOGRAPHY
   ========================================= */
h1, h2, h3, .number {
    color: #006064;
    font-family: 'Ubuntu', sans-serif;
    margin: 0;
    text-shadow: 1px 1px 2px rgba(255,255,255,0.8);
}

p, li, td, th, cite, .subtitle, .code-block {
    color: #263238;
    font-size: 20px;
    line-height: 1.6;
    margin-top: 0;
}

h1 { font-size: 72px; font-weight: 700; line-height: 1.1; }
.subtitle { font-size: 26px; color: #00838f; margin-top: 20px; }
.slide-title { 
    font-size: 46px; 
    font-weight: 700; 
    margin-bottom: 30px; 
    width: 100%; 
    text-align: left; 
    border-bottom: 2px solid rgba(255,255,255,0.6); 
    padding-bottom: 10px; 
}
h3 { font-size: 28px; margin-bottom: 15px; color: #00796b; }

.content-area {
    display: flex;
    flex-direction: column;
    flex-grow: 1;
    width: 100%;
    justify-content: center;
}

/* =========================================
   4. LAYOUT DEFINITIONS
   ========================================= */
.title-layout { text-align: center; margin: auto; }

.two-column {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 40px;
    width: 100%;
    align-items: center;
}

.image-wrapper {
    border-radius: 16px;
    box-shadow: 0 8px 20px rgba(0,0,0,0.1);
    height: 380px;
    overflow: hidden;
    width: 100%;
    border: 3px solid rgba(255,255,255,0.7);
}
.image-wrapper img {
    height: 100%;
    object-fit: cover;
    width: 100%;
}
.two-column .image-wrapper img { object-fit: contain; background: rgba(255,255,255,0.5); }

/* TEAM GRID */
.team-grid {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 20px;
    width: 100%;
    align-items: stretch;
}
.team-card {
    background: linear-gradient(180deg, rgba(255,255,255,0.9) 0%, rgba(224,247,250,0.6) 100%);
    border-radius: 16px;
    padding: 25px 20px;
    text-align: center;
    border: 1px solid rgba(255,255,255,0.9);
    box-shadow: 0 6px 12px rgba(0,0,0,0.05);
    transition: transform 0.3s ease;
}
.team-card:hover { transform: translateY(-10px); }
.team-card .icon {
    font-size: 45px;
    color: #00bcd4;
    margin-bottom: 15px;
    background: radial-gradient(circle, #fff, transparent);
    border-radius: 50%;
    height: 80px;
    width: 80px;
    line-height: 80px;
    display: inline-block;
    box-shadow: 0 4px 10px rgba(0,188,212,0.2);
}

/* SECTION TITLE */
.section-title-layout {
    text-align: center;
    margin: auto;
}
.section-title-layout hr {
    width: 120px;
    border: 3px solid #00bcd4;
    border-radius: 5px;
    margin: 20px auto;
}
.section-title-layout h2 { font-size: 64px; }

/* BLEED IMAGE RIGHT */
.bleed-image-layout .slide-inner {
    padding: 0;
    display: grid;
    grid-template-columns: repeat(2, minmax(0, 1fr));
    align-items: start;
}
.bleed-image-layout .content-container { padding: 60px 0 60px 60px; display: flex; flex-direction: column; height: 100%; }
.bleed-image-layout .image-container { height: 720px; overflow: hidden; width: 100%; }
.bleed-image-layout img.bleed-image-side {
    height: 720px;
    object-fit: cover;
    width: 100%;
}

/* STYLED BULLET POINTS */
.bullet-list ul { list-style: none; padding: 0; margin: 0; }
.bullet-list li {
    padding-left: 45px;
    position: relative;
    margin-bottom: 20px;
}
.bullet-list i {
    position: absolute;
    left: 0;
    top: 3px;
    color: #00bcd4;
    font-size: 26px;
    text-shadow: 0 2px 4px rgba(0,188,212,0.3);
}

/* HIGHLIGHTED NUMBERS */
.highlight-numbers-layout > div:first-child { text-align: center; flex: 0 0 35%; }
.highlight-numbers-layout .number {
    font-size: 130px;
    color: #00838f;
    line-height: 1;
}
.highlight-numbers-layout .number-label {
    font-size: 24px;
    font-weight: 700;
    color: #004d40;
}

/* CODE BLOCK STYLING */
.code-block {
    background: rgba(236, 240, 241, 0.85);
    border-radius: 12px;
    padding: 20px;
    font-family: monospace;
    font-size: 17px;
    border-left: 5px solid #00bcd4;
    color: #2c3e50;
    white-space: pre-wrap;
    box-shadow: inset 0 2px 5px rgba(0,0,0,0.05);
}

/* =========================================
   5. INTERACTIVE DYNAMICS CSS
   ========================================= */

/* DINAMICA 1: Casilleros Mágicos (Arreglos) */
.casilleros-container {
    display: flex;
    gap: 15px;
    justify-content: center;
    margin-top: 30px;
}
.casillero {
    width: 140px;
    height: 160px;
    position: relative;
    perspective: 800px;
    cursor: pointer;
}
.casillero-door {
    position: absolute;
    width: 100%;
    height: 100%;
    background: linear-gradient(135deg, #4dd0e1, #00acc1);
    color: white;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    border-radius: 12px;
    border: 2px solid #fff;
    font-weight: 700;
    font-size: 22px;
    transition: transform 0.6s cubic-bezier(0.4, 0, 0.2, 1);
    transform-origin: left;
    box-shadow: 0 5px 15px rgba(0,0,0,0.2);
    z-index: 2;
}
.casillero-inside {
    position: absolute;
    width: 100%;
    height: 100%;
    background: #fff;
    color: #006064;
    display: flex;
    justify-content: center;
    align-items: center;
    border-radius: 12px;
    border: 2px dashed #00acc1;
    font-weight: 700;
    font-size: 20px;
    box-shadow: inset 0 0 10px rgba(0,0,0,0.1);
    z-index: 1;
}
.casillero:hover .casillero-door {
    transform: rotateY(-110deg);
}

/* DINAMICA 2: Flip Cards (Unidimensionales) */
.tiled-content {
    display: flex;
    gap: 30px;
    width: 100%;
    justify-content: center;
    align-items: stretch;
}
.flip-card {
    background-color: transparent;
    width: 33%;
    height: 340px;
    perspective: 1000px;
}
.flip-card-inner {
    position: relative;
    width: 100%;
    height: 100%;
    text-align: center;
    transition: transform 0.7s;
    transform-style: preserve-3d;
    box-shadow: 0 8px 16px rgba(0,0,0,0.1);
    border-radius: 16px;
}
.flip-card:hover .flip-card-inner {
    transform: rotateY(180deg);
}
.flip-card-front, .flip-card-back {
    position: absolute;
    width: 100%;
    height: 100%;
    -webkit-backface-visibility: hidden;
    backface-visibility: hidden;
    border-radius: 16px;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    padding: 30px;
    border: 2px solid rgba(255,255,255,0.9);
}
.flip-card-front {
    background: linear-gradient(135deg, rgba(255,255,255,0.95), rgba(224,247,250,0.9));
    color: #006064;
}
.flip-card-back {
    background: linear-gradient(135deg, #00bcd4, #00796b);
    color: white;
    transform: rotateY(180deg);
}
.flip-card-back h3, .flip-card-back p { color: white; text-shadow: none; }
.flip-card-front .icon { font-size: 55px; color: #00bcd4; margin-bottom: 20px; }

/* TILED IMAGES */
.image-tile { flex: 1; text-align: center; }
.image-tile .image-wrapper { height: 220px; margin-bottom: 15px; border-radius: 12px; }

/* QUOTE */
.quote-layout { text-align: center; width: 85%; margin: 0 auto; }
.quote-layout blockquote {
    font-size: 42px;
    color: #004d40;
    line-height: 1.4;
    position: relative;
    margin: 0 0 30px 0;
    padding: 0 40px;
    font-style: italic;
}
.quote-layout blockquote::before, .quote-layout blockquote::after {
    color: #80deea;
    content: '"';
    font-size: 100px;
    position: absolute;
    line-height: 0;
}
.quote-layout blockquote::before { left: -20px; top: 40px; }
.quote-layout blockquote::after { right: -20px; bottom: 0; }

/* QA LAYOUT */
.qa-layout { text-align: center; margin: auto; }
.qa-layout h2 { font-size: 72px; margin-bottom: 20px; }

/* =========================================
   6. CONTROLS & OVERLAYS (Frutiger Aero Aesthetic)
   ========================================= */

/* Initial Start Screen Overlay */
#start-overlay {
    position: fixed;
    top: 0; left: 0; width: 100vw; height: 100vh;
    background: rgba(255,255,255,0.8);
    backdrop-filter: blur(10px);
    z-index: 999999;
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
}
#start-btn {
    padding: 20px 50px;
    font-size: 28px;
    font-family: 'Ubuntu', sans-serif;
    font-weight: 700;
    color: white;
    background: linear-gradient(180deg, #4dd0e1, #00838f);
    border: 2px solid white;
    border-radius: 50px;
    cursor: pointer;
    box-shadow: 0 10px 20px rgba(0,131,143,0.3), inset 0 2px 5px rgba(255,255,255,0.6);
    transition: transform 0.2s, box-shadow 0.2s;
}
#start-btn:hover {
    transform: scale(1.05);
    box-shadow: 0 12px 25px rgba(0,131,143,0.4), inset 0 2px 5px rgba(255,255,255,0.8);
}

/* Floating Control Dock */
#control-dock {
    position: fixed;
    bottom: 20px;
    left: 50%;
    transform: translateX(-50%);
    background: rgba(255, 255, 255, 0.7);
    backdrop-filter: blur(15px);
    border: 2px solid rgba(255, 255, 255, 0.9);
    border-radius: 40px;
    padding: 10px 25px;
    box-shadow: 0 8px 32px rgba(0,0,0,0.15);
    z-index: 1000;
    display: none; /* Hidden until start */
    align-items: center;
    gap: 15px;
}
.ctrl-btn {
    background: transparent;
    border: none;
    font-size: 20px;
    color: #00796b;
    cursor: pointer;
    font-family: 'Open Sans', sans-serif;
    font-weight: 600;
    padding: 10px 15px;
    border-radius: 20px;
    transition: background 0.3s;
    display: flex;
    align-items: center;
    gap: 8px;
}
.ctrl-btn:hover { background: rgba(0, 188, 212, 0.2); }
.ctrl-btn.active { background: #00bcd4; color: white; }
.divider { width: 2px; height: 30px; background: rgba(0,121,107,0.3); }

/* Hidden YouTube Player (Now off-screen but with standard dimensions to trick browsers) */
#yt-player-container { 
    position: absolute; 
    left: -9999px; 
    top: -9999px; 
    opacity: 0.01; 
    pointer-events: none; 
}

/* =========================================
   7. SPEAKER BADGE
   ========================================= */
.speaker-badge {
    position: absolute;
    top: 25px;
    right: 35px;
    background: linear-gradient(135deg, rgba(255,255,255,0.95), rgba(224,247,250,0.95));
    border: 2px solid #00bcd4;
    color: #00796b;
    padding: 8px 20px;
    border-radius: 30px;
    font-family: 'Ubuntu', sans-serif;
    font-weight: 700;
    font-size: 16px;
    box-shadow: 0 4px 15px rgba(0,0,0,0.1);
    display: flex;
    align-items: center;
    gap: 10px;
    z-index: 50;
}

/* =========================================
   8. PRANK OVERLAY (PANTALLA AZUL FULL SCREEN)
   ========================================= */
#fake-error-overlay {
    position: fixed;
    top: 0;
    left: 0;
    width: 100vw;
    height: 100vh;
    background-color: #0078D7; /* Windows 10/11 classic BSOD blue */
    color: white;
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    padding: 8vw;
    cursor: pointer;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: flex-start;
    z-index: 99999; /* Asegura que tape los controles y todo lo demás */
}
#fake-error-overlay h1 {
    color: white;
    font-size: 15vh;
    margin-bottom: 20px;
    text-shadow: none;
    font-family: 'Segoe UI', Tahoma, sans-serif;
}
#fake-error-overlay p {
    color: white;
    font-size: 3.5vh;
    margin-bottom: 15px;
    line-height: 1.3;
}
    </style>
</head>
<body>

<!-- OVERLAY DE LA PANTALLA AZUL (Oculto al inicio) -->
<div id="fake-error-overlay" onclick="revealPrank()" style="display: none;">
    <h1>:(</h1>
    <p>Your Netbeans presentation ran into a problem and needs to restart.</p>
    <p>We're just collecting some error info, and then we'll fail your grade.</p>
    <br><br>
    <p style="font-size: 2vh;">Stop code: UNEXPECTED_ARRAY_INDEX_OUT_OF_BOUNDS_EXCEPTION</p>
    <p style="font-size: 2vh;">If you call a support person, give them this info: PROFE_AYUDA_POR_FAVOR</p>
    <br><br>
    <p style="font-size: 2vh; opacity: 0.6;">(Clickea en la pantalla para continuar...)</p>
</div>

<!-- START OVERLAY (Requires user interaction to start audio) -->
<div id="start-overlay">
    <h1 style="margin-bottom: 20px; font-size: 50px;">Arreglos en Java - Unidad 4</h1>
    <button id="start-btn"><i class="fa-solid fa-play"></i> Iniciar Presentación</button>
</div>

<!-- FLOATING CONTROL DOCK -->
<div id="control-dock">
    <button class="ctrl-btn" onclick="prevSlide()" title="Anterior"><i class="fa-solid fa-chevron-left"></i></button>
    <span id="slide-counter" style="font-weight:700; color:#006064;">1/17</span>
    <button class="ctrl-btn" onclick="nextSlide()" title="Siguiente"><i class="fa-solid fa-chevron-right"></i></button>
    
    <div class="divider"></div>
    
    <button class="ctrl-btn" id="btn-track1" onclick="playMusic('Nj9czfiYd2E', 'btn-track1')"><i class="fa-solid fa-music"></i> Pista 1</button>
    <button class="ctrl-btn" id="btn-track2" onclick="playMusic('xsdWibwQdZE', 'btn-track2')"><i class="fa-solid fa-music"></i> Pista 2</button>
    <button class="ctrl-btn" onclick="pauseMusic()"><i class="fa-solid fa-pause"></i></button>
</div>

<!-- HIDDEN YOUTUBE PLAYER -->
<div id="yt-player-container"></div>

<!-- PRESENTATION AREA -->
<div id="presentation-area">

    <!-- Slide 1: Title_Slide -->
    <div class="slide-container active" id="slide1">
        <div class="slide-inner">
            <div class="speaker-badge"><i class="fa-solid fa-microphone"></i> Expone: Wilhelm</div>
            <div class="title-layout">
                <i class="fa-brands fa-java" style="font-size: 100px; color: #00838f; margin-bottom: 20px;"></i>
                <h1>Arreglos en Java Netbeans</h1>
                <p class="subtitle">Unidad 4: Estructuras de Datos Unidimensionales</p>
                <p style="margin-top:40px; font-size:16px;"><em>Usa las flechas del teclado (← →) o la barra inferior para navegar</em></p>
            </div>
        </div>
    </div>

    <!-- Slide 2: Two_Column_Tiled_Text (Team Grid) -->
    <div class="slide-container" id="slide2">
        <div class="slide-inner">
            <div class="speaker-badge"><i class="fa-solid fa-microphone"></i> Expone: Wilhelm</div>
            <h2 class="slide-title">Conoce a Nuestro Equipo</h2>
            <div class="content-area">
                <div class="team-grid">
                    <div class="team-card">
                        <div class="icon"><i class="fa-solid fa-headset"></i></div>
                        <h3>Daniel Cuitlauac</h3>
                        <p>Análisis y Revisión. Se asegura de que la lógica tenga sentido y sea clara paso a paso.</p>
                    </div>
                    <div class="team-card">
                        <div class="icon"><i class="fa-solid fa-code"></i></div>
                        <h3>Emanuel Serra</h3>
                        <p>Desarrollador Neutral. Verifica que el código fluya correctamente y asimila los conceptos base.</p>
                    </div>
                    <div class="team-card">
                        <div class="icon"><i class="fa-solid fa-microchip"></i></div>
                        <h3>Armando Jaimes</h3>
                        <p>Experto Técnico. Conoce los temas a profundidad; listo para los retos de optimización.</p>
                    </div>
                    <div class="team-card">
                        <div class="icon"><i class="fa-solid fa-user-astronaut"></i></div>
                        <h3>Wilhelm (Edwin)</h3>
                        <p>Presentador principal. Apoyo integral para el equipo y puente de comunicación con ustedes.</p>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- Slide 3: Section_Title -->
    <div class="slide-container" id="slide3">
        <div class="slide-inner">
            <div class="speaker-badge"><i class="fa-solid fa-microphone"></i> Expone: Daniel</div>
            <div class="section-title-layout">
                <h2>4.1 Arreglos</h2>
                <hr>
                <p>Conceptos fundamentales y dinámicas de entendimiento</p>
            </div>
        </div>
    </div>

    <!-- Slide 4: Bleed_Image_Right -->
    <div class="slide-container bleed-image-layout" id="slide4">
        <div class="slide-inner">
            <div class="speaker-badge"><i class="fa-solid fa-microphone"></i> Expone: Daniel</div>
            <div class="content-container">
                <h2 class="slide-title" style="border:none;">¿Qué es un Arreglo?</h2>
                <div class="content-area">
                    <h3>La base de la organización</h3>
                    <p>Un arreglo (array) en Java es una estructura de datos que nos permite almacenar una colección de elementos del <strong>mismo tipo</strong>.</p>
                    <p>A diferencia de las variables normales que guardan un solo dato, un arreglo puede guardar múltiples valores bajo un mismo nombre, ordenados de forma contigua en la memoria.</p>
                </div>
            </div>
            <div class="image-container">
                <img class="bleed-image-side" src="https://images.unsplash.com/photo-1618005182384-a83a8bd57fbe?q=80&w=1000" alt="Frutiger aero glossy water background">
            </div>
        </div>
    </div>

    <!-- Slide 5: Image_Right_Text_Left (Dinámica 1 MEJORADA) -->
    <div class="slide-container" id="slide5">
        <div class="slide-inner">
            <div class="speaker-badge"><i class="fa-solid fa-microphone"></i> Exponen: Daniel y Emanuel</div>
            <h2 class="slide-title">Dinámica 1: El Casillero Mágico</h2>
            <div class="content-area">
                <div class="two-column">
                    <div>
                        <h3>Comprendiendo la memoria interactiva</h3>
                        <p><strong>Daniel y Emanuel:</strong> Pasen el ratón sobre los casilleros. Así es como la memoria de la computadora guarda la información de forma contigua.</p>
                        <ul class="bullet-list" style="margin-top: 20px;">
                            <li><i class="fa-solid fa-box-open"></i><strong>El Mueble:</strong> Es nuestro Arreglo completo.</li>
                            <li><i class="fa-solid fa-tag"></i><strong>Las Puertas:</strong> Son los Índices (empiezan en 0).</li>
                            <li><i class="fa-solid fa-cubes"></i><strong>El Interior:</strong> Es el Valor o Dato.</li>
                        </ul>
                    </div>
                    <div>
                        <div style="text-align:center;">
                            <h3 style="color:#00838f;">Arreglo: `String[] equipo`</h3>
                            <!-- Casilleros Interactivos -->
                            <div class="casilleros-container">
                                <div class="casillero">
                                    <div class="casillero-door">Índice 0<br><i class="fa-solid fa-hand-pointer" style="font-size:14px; margin-top:10px;"></i></div>
                                    <div class="casillero-inside">"Daniel"</div>
                                </div>
                                <div class="casillero">
                                    <div class="casillero-door">Índice 1<br><i class="fa-solid fa-hand-pointer" style="font-size:14px; margin-top:10px;"></i></div>
                                    <div class="casillero-inside">"Emanuel"</div>
                                </div>
                                <div class="casillero">
                                    <div class="casillero-door">Índice 2<br><i class="fa-solid fa-hand-pointer" style="font-size:14px; margin-top:10px;"></i></div>
                                    <div class="casillero-inside">"Armando"</div>
                                </div>
                            </div>
                            <p style="margin-top:20px; font-size:16px;"><em>(Pasa el cursor sobre ellos para abrirlos)</em></p>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- Slide 6: Section_Title -->
    <div class="slide-container" id="slide6">
        <div class="slide-inner">
            <div class="speaker-badge"><i class="fa-solid fa-microphone"></i> Expone: Emanuel</div>
            <div class="section-title-layout">
                <h2>4.2 Unidimensionales</h2>
                <hr>
                <p>Conceptos básicos, operaciones y aplicaciones en Java (Vectores)</p>
            </div>
        </div>
    </div>

    <!-- Slide 7: Highlighted_Numbers -->
    <div class="slide-container" id="slide7">
        <div class="slide-inner">
            <div class="speaker-badge"><i class="fa-solid fa-microphone"></i> Expone: Emanuel</div>
            <h2 class="slide-title">Anatomía de un Arreglo Unidimensional</h2>
            <div class="content-area">
                <div class="two-column highlight-numbers-layout">
                    <div>
                        <div class="number">0</div>
                        <div class="number-label">El Índice Inicial</div>
                    </div>
                    <div>
                        <h3>La regla de oro de la programación</h3>
                        <p>En Java (y la mayoría de los lenguajes), los arreglos unidimensionales o "vectores" no empiezan a contar en 1, <strong>empiezan en 0</strong>.</p>
                        <ul class="bullet-list" style="margin-top:20px;">
                            <li><i class="fa-solid fa-arrow-right"></i>Si el arreglo tiene tamaño de 5 elementos...</li>
                            <li><i class="fa-solid fa-arrow-right"></i>El primer elemento está en el índice <code>0</code>.</li>
                            <li><i class="fa-solid fa-arrow-right"></i>El último elemento está en el índice <code>4</code> (N-1).</li>
                        </ul>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- Slide 8: Image_Right_Text_Left (Netbeans Code) -->
    <div class="slide-container" id="slide8">
        <div class="slide-inner">
            <div class="speaker-badge"><i class="fa-solid fa-microphone"></i> Expone: Armando</div>
            <h2 class="slide-title">Creación en Java Netbeans</h2>
            <div class="content-area">
                <div class="two-column">
                    <div>
                        <h3>Sintaxis Básica</h3>
                        <p>Para usar un arreglo en Netbeans, primero lo declaramos y luego instanciamos su tamaño en memoria usando la palabra clave <code>new</code>.</p>
                        <div class="code-block">
// Declaración e instanciación
int[] calificaciones = new int[5];

// Asignación de valores
calificaciones[0] = 95;
calificaciones[1] = 88;
calificaciones[2] = 100;

// Lectura de un valor
System.out.println(calificaciones[2]);
                        </div>
                    </div>
                    <div>
                        <div class="image-wrapper" style="border:none; box-shadow:none;">
                            <img src="https://images.unsplash.com/photo-1542831371-29b0f74f9713?q=80&w=1000" alt="Netbeans IDE code interface">
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- Slide 9: Styled_Bullet_Points -->
    <div class="slide-container" id="slide9">
        <div class="slide-inner">
            <div class="speaker-badge"><i class="fa-solid fa-microphone"></i> Expone: Armando</div>
            <h2 class="slide-title">Operaciones Básicas</h2>
            <div class="content-area">
                <div class="bullet-list">
                    <ul>
                        <li><i class="fa-solid fa-plus-circle"></i><strong>1. Declaración e Inicialización:</strong> Reservar la memoria requerida especificando el tipo de dato y la longitud exacta. El tamaño ya no puede cambiar en ejecución.</li>
                        <li><i class="fa-solid fa-pencil-alt"></i><strong>2. Asignación (Escritura):</strong> Guardar un dato en un índice específico. Ejemplo: <code>nombres[1] = "Wilhelm";</code></li>
                        <li><i class="fa-solid fa-eye"></i><strong>3. Acceso (Lectura):</strong> Recuperar un dato usando su índice para mostrarlo o usarlo en una fórmula matemática.</li>
                        <li><i class="fa-solid fa-route"></i><strong>4. Recorrido:</strong> Utilizar estructuras repetitivas (como el ciclo <code>for</code>) para pasar por todos los elementos desde el índice 0 hasta el final.</li>
                    </ul>
                </div>
            </div>
        </div>
    </div>

    <!-- Slide 10: Tiled_Text_With_Icons (Dinámica Interactiva 2 - FACIL) -->
    <div class="slide-container" id="slide10">
        <div class="slide-inner">
            <div class="speaker-badge"><i class="fa-solid fa-microphone"></i> Expone: Wilhelm (Moderador)</div>
            <h2 class="slide-title">Dinámica 2: Retos de Netbeans (Pasa el cursor)</h2>
            <div class="content-area">
                <div class="tiled-content">
                    <!-- Card for Daniel -->
                    <div class="flip-card">
                        <div class="flip-card-inner">
                            <div class="flip-card-front">
                                <div class="icon"><i class="fa-solid fa-question-circle"></i></div>
                                <h3>Para Daniel</h3>
                                <p><strong>Pasa el ratón aquí.</strong><br>Si creo un arreglo <code>String[] nombres</code>, ¿puedo meter un número ahí?</p>
                            </div>
                            <div class="flip-card-back">
                                <h3>¡Para nada!</h3>
                                <p>Java es estricto. Si le dijiste que era de texto (String), solo acepta texto. ¡Un número marcaría error en rojo!</p>
                            </div>
                        </div>
                    </div>
                    <!-- Card for Emanuel -->
                    <div class="flip-card">
                        <div class="flip-card-inner">
                            <div class="flip-card-front">
                                <div class="icon"><i class="fa-solid fa-code"></i></div>
                                <h3>Para Emanuel</h3>
                                <p><strong>Pasa el ratón aquí.</strong><br>Si ya sé qué valores voy a guardar desde el inicio, ¿a fuerza tengo que usar <code>new</code> y llenarlos uno por uno?</p>
                            </div>
                            <div class="flip-card-back">
                                <h3>¡Claro que no!</h3>
                                <p>Puedes usar llaves para hacerlo directo y súper rápido: <br><code>int[] nums = {5, 10, 15};</code><br>¡Java calcula el tamaño solito!</p>
                            </div>
                        </div>
                    </div>
                    <!-- Card for Armando -->
                    <div class="flip-card">
                        <div class="flip-card-inner">
                            <div class="flip-card-front">
                                <div class="icon"><i class="fa-solid fa-bolt"></i></div>
                                <h3>Reto Armando</h3>
                                <p><strong>Pasa el ratón aquí.</strong><br>¿Cuál es el PRIMER índice de cualquier arreglo en Java?</p>
                            </div>
                            <div class="flip-card-back">
                                <h3>¡El cero (0)!</h3>
                                <p>Nunca lo olviden, los programadores siempre empezamos a contar desde cero, no desde uno.</p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- Slide 11: Two_Column_Tiled_Text (Recorrido) -->
    <div class="slide-container" id="slide11">
        <div class="slide-inner">
            <div class="speaker-badge"><i class="fa-solid fa-microphone"></i> Expone: Armando</div>
            <h2 class="slide-title">Recorriendo el Arreglo</h2>
            <div class="content-area">
                <div class="two-column">
                    <div>
                        <h3>El mejor amigo del arreglo: El ciclo FOR</h3>
                        <p>Para no escribir el código de impresión elemento por elemento, la estructura <code>for</code> se combina perfectamente con los arreglos unidimensionales.</p>
                        <p>Creamos una variable <code>i</code> que empieza en 0, y termina antes del tamaño total del arreglo usando la propiedad <code>.length</code>.</p>
                    </div>
                    <div>
                        <div class="code-block">
String[] frutas = {"Manzana", "Pera", "Uva"};

// Recorrer el arreglo
for(int i = 0; i < frutas.length; i++) {
    System.out.println("Fruta " + i + ": " + frutas[i]);
}

// Salida:
// Fruta 0: Manzana
// Fruta 1: Pera
// Fruta 2: Uva
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- Slide 12: NUEVA DINAMICA PARA LA CLASE -->
    <div class="slide-container" id="slide12">
        <div class="slide-inner">
            <div class="speaker-badge"><i class="fa-solid fa-microphone"></i> Exponen: Wilhelm y Equipo</div>
            <h2 class="slide-title">Dinámica: ¡Trivia Rápida para la Clase!</h2>
            <div class="content-area">
                <p style="text-align: center; margin-bottom: 30px; font-size: 24px; color: #006064;">A ver si nos pusieron atención... <strong>(Pasen el ratón para ver la respuesta)</strong></p>
                <div class="tiled-content">
                    <div class="flip-card" style="height: 280px;">
                        <div class="flip-card-inner">
                            <div class="flip-card-front" style="padding: 20px;">
                                <div class="icon" style="font-size:35px; height:60px; width:60px; line-height:60px; margin-bottom:15px;"><i class="fa-solid fa-1"></i></div>
                                <h3 style="font-size: 22px;">Pregunta 1</h3>
                                <p style="font-size: 18px;">¿Con qué número de índice empieza SIEMPRE un arreglo en Java?</p>
                            </div>
                            <div class="flip-card-back" style="padding: 20px;">
                                <h3 style="font-size: 34px;">¡Con el Cero (0)!</h3>
                            </div>
                        </div>
                    </div>
                    <div class="flip-card" style="height: 280px;">
                        <div class="flip-card-inner">
                            <div class="flip-card-front" style="padding: 20px;">
                                <div class="icon" style="font-size:35px; height:60px; width:60px; line-height:60px; margin-bottom:15px;"><i class="fa-solid fa-2"></i></div>
                                <h3 style="font-size: 22px;">Pregunta 2</h3>
                                <p style="font-size: 18px;">Si un arreglo tiene 10 espacios en total, ¿cuál es el último índice válido?</p>
                            </div>
                            <div class="flip-card-back" style="padding: 20px;">
                                <h3 style="font-size: 34px;">¡El Nueve (9)!</h3>
                                <p style="font-size: 18px;">(Siempre es el tamaño total menos 1)</p>
                            </div>
                        </div>
                    </div>
                    <div class="flip-card" style="height: 280px;">
                        <div class="flip-card-inner">
                            <div class="flip-card-front" style="padding: 20px;">
                                <div class="icon" style="font-size:35px; height:60px; width:60px; line-height:60px; margin-bottom:15px;"><i class="fa-solid fa-3"></i></div>
                                <h3 style="font-size: 22px;">Pregunta 3</h3>
                                <p style="font-size: 18px;">¿Verdadero o Falso? Un arreglo puede guardar números enteros y textos mezclados.</p>
                            </div>
                            <div class="flip-card-back" style="padding: 20px;">
                                <h3 style="font-size: 34px;">¡Falso!</h3>
                                <p style="font-size: 18px;">Debe ser todo exactamente del mismo tipo de dato.</p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- Slide 13: Tiled_Images -->
    <div class="slide-container" id="slide13">
        <div class="slide-inner">
            <div class="speaker-badge"><i class="fa-solid fa-microphone"></i> Expone: Wilhelm</div>
            <h2 class="slide-title">Aplicaciones Prácticas Unidimensionales</h2>
            <div class="content-area">
                <div class="tiled-content">
                    <div class="image-tile">
                        <div class="image-wrapper"><img src="https://images.unsplash.com/photo-1614729939124-032f0b56c9ce?q=80&w=1000" alt="Frutiger aero 3D spheres"></div>
                        <h3>Videojuegos</h3>
                        <p>Inventarios de personajes, listas de puntajes (High Scores) o registro de esferas recolectadas.</p>
                    </div>
                    <div class="image-tile">
                        <div class="image-wrapper"><img src="https://images.unsplash.com/photo-1527066579998-dbbae57f45ce?q=80&w=1000" alt="Frutiger aero water splash"></div>
                        <h3>Matemáticas y Físicas</h3>
                        <p>Procesamiento de señales de audio, simulación de fluidos o cálculo de promedios estadísticos.</p>
                    </div>
                    <div class="image-tile">
                        <div class="image-wrapper"><img src="https://images.unsplash.com/photo-1500382017468-9049fed747ef?q=80&w=1000" alt="Frutiger aero green grass"></div>
                        <h3>Sistemas de Gestión</h3>
                        <p>Listas de empleados cargadas a la memoria RAM para realizar búsquedas secuenciales hiper rápidas.</p>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- Slide 14: Quote -->
    <div class="slide-container" id="slide14">
        <div class="slide-inner">
            <div class="speaker-badge"><i class="fa-solid fa-microphone"></i> Exponen: Todos</div>
            <h2 class="slide-title">Reflexión del Equipo</h2>
            <div class="content-area">
                <div class="quote-layout">
                    <blockquote>Comprender cómo funcionan los arreglos en la memoria es el primer gran paso para dominar la lógica de programación y las estructuras de datos complejas en Java.</blockquote>
                    <cite>— Conclusión de Armando, Emanuel, Daniel y Wilhelm.</cite>
                </div>
            </div>
        </div>
    </div>

    <!-- Slide 15: Styled_Bullet_Points (Bibliografía APA) -->
    <div class="slide-container" id="slide15">
        <div class="slide-inner">
            <div class="speaker-badge"><i class="fa-solid fa-microphone"></i> Expone: Wilhelm</div>
            <h2 class="slide-title">Bibliografía (Formato APA)</h2>
            <div class="content-area">
                <div class="bullet-list" style="font-size: 19px;">
                    <ul>
                        <li><i class="fa-solid fa-book"></i>Deitel, P., & Deitel, H. (2017). <em>Java: Cómo programar</em> (10a ed.). Pearson Educación.</li>
                        <li><i class="fa-solid fa-book"></i>Eckel, B. (2006). <em>Thinking in Java</em> (4th ed.). Prentice Hall.</li>
                        <li><i class="fa-solid fa-book"></i>Joyanes Aguilar, L. (2011). <em>Fundamentos de programación: Algoritmos, estructura de datos y objetos</em> (4a ed.). McGraw-Hill.</li>
                        <li><i class="fa-solid fa-globe"></i>Oracle. (2023). <em>The Java Tutorials: Arrays</em>. Recuperado de docs.oracle.com</li>
                        <li><i class="fa-solid fa-book"></i>Schildt, H. (2018). <em>Java: The Complete Reference</em> (11th ed.). McGraw-Hill Education.</li>
                    </ul>
                </div>
            </div>
        </div>
    </div>

    <!-- Slide 16: LA BROMA DEL PROFE (Sorpresa VIP) MOVIDA ANTES DE PREGUNTAS -->
    <div class="slide-container" id="slide16">
        <div class="slide-inner">
            
            <!-- El verdadero Reto VIP (Se mostrará después del click en la Pantalla Azul) -->
            <div id="real-prank" style="display: none; height: 100%; flex-direction: column; justify-content: center; align-items: center; text-align: center;">
                <div class="speaker-badge" style="top: 25px; right: 35px; position: absolute;"><i class="fa-solid fa-star"></i> Expone: El Profe</div>
                <h2 style="font-size: 70px; color: #00bcd4; font-family: 'Ubuntu', sans-serif; margin-bottom: 0;">¡Era Broma, Profe! 😜</h2>
                <h3 style="font-size: 30px; color: #00796b; margin-top: 10px;">Tenemos una Dinámica VIP Exclusiva para usted</h3>
                
                <div class="flip-card" style="width: 50%; height: 300px; margin-top: 40px;">
                    <div class="flip-card-inner">
                        <div class="flip-card-front" style="padding: 40px;">
                            <div class="icon"><i class="fa-solid fa-graduation-cap"></i></div>
                            <h3 style="font-size: 28px;">Reto para el Maestro</h3>
                            <p style="font-size: 22px;"><strong>Pase el ratón por aquí, profe.</strong><br>Si tenemos un arreglo de tamaño genérico "N", ¿Cuál es siempre el índice de la ÚLTIMA posición válida?</p>
                        </div>
                        <div class="flip-card-back" style="padding: 40px;">
                            <h3 style="font-size: 40px;">¡ N - 1 !</h3>
                            <p style="font-size: 22px;">Como empezamos en cero, la última posición siempre es el tamaño total menos uno.<br><br>¡Esperamos sacar un 10! 💯</p>
                        </div>
                    </div>
                </div>
            </div>

        </div>
    </div>

    <!-- Slide 17: Q&A (MOVIDO AL FINAL) -->
    <div class="slide-container" id="slide17">
        <div class="slide-inner">
            <div class="speaker-badge"><i class="fa-solid fa-microphone"></i> Expone: Wilhelm</div>
            <div class="qa-layout">
                <h2>¿Preguntas y Respuestas?</h2>
                <p>¡Agradecemos mucho su atención en nombre de Daniel, Emanuel, Armando y Wilhelm!</p>
                <div style="margin-top: 30px; color: #00796b;">
                    <i class="fa-brands fa-java fa-3x" style="margin-bottom: 10px;"></i><br>
                    <strong>¡Es hora de programar en Netbeans!</strong>
                </div>
            </div>
        </div>
    </div>

</div> <!-- End presentation area -->

<!-- =========================================
     JAVASCRIPT: SLIDE CONTROLLER & YOUTUBE PLAYER
     ========================================= -->
<script>
    // --- Slide Navigation Logic ---
    let currentSlide = 1;
    const totalSlides = 17;
    let prankRevealed = false; // Variable para saber si ya asustamos al profe
    
    function updateSlides() {
        document.querySelectorAll('.slide-container').forEach(slide => {
            slide.classList.remove('active');
        });
        document.getElementById('slide' + currentSlide).classList.add('active');
        document.getElementById('slide-counter').innerText = currentSlide + '/' + totalSlides;

        // LÓGICA DE LA BROMA DEL PROFE EN LA DIAPOSITIVA 16
        if (currentSlide === 16 && !prankRevealed) {
            // Mostrar Pantallazo Azul Fijo y Full Screen
            document.getElementById('fake-error-overlay').style.display = 'flex';
            // PAUSAR LA MÚSICA DE GOLPE PARA DRAMA
            pauseMusic();
        } else {
            document.getElementById('fake-error-overlay').style.display = 'none';
        }
    }

    function nextSlide() {
        if(currentSlide < totalSlides) {
            currentSlide++;
            updateSlides();
        }
    }

    function prevSlide() {
        if(currentSlide > 1) {
            currentSlide--;
            updateSlides();
        }
    }

    // Keyboard navigation
    document.addEventListener('keydown', (e) => {
        if(e.key === 'ArrowRight' || e.key === 'Space') { nextSlide(); }
        if(e.key === 'ArrowLeft') { prevSlide(); }
    });

    // --- Fuerza Bruta: Inyección de Iframe ---
    document.getElementById('start-btn').addEventListener('click', function() {
        document.getElementById('start-overlay').style.display = 'none';
        document.getElementById('control-dock').style.display = 'flex';
        updateSlides();
        playMusic('Nj9czfiYd2E', 'btn-track1');
    });

    function playMusic(videoId, btnId) {
        const container = document.getElementById('yt-player-container');
        container.innerHTML = `<iframe width="560" height="315" src="https://www.youtube.com/embed/${videoId}?autoplay=1&rel=0&playsinline=1&enablejsapi=1" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>`;
        
        document.querySelectorAll('.ctrl-btn').forEach(btn => btn.classList.remove('active'));
        if(btnId) document.getElementById(btnId).classList.add('active');
    }

    function pauseMusic() {
        document.getElementById('yt-player-container').innerHTML = '';
        document.querySelectorAll('.ctrl-btn').forEach(btn => btn.classList.remove('active'));
    }

    // --- Lógica de la Broma del Profe ---
    function revealPrank() {
        // Quitar pantalla azul
        document.getElementById('fake-error-overlay').style.display = 'none';
        // Mostrar dinámica real dentro de la slide 16
        document.getElementById('real-prank').style.display = 'flex';
        prankRevealed = true; // Para que no vuelva a salir si retroceden de slide
    }
</script>

</body>
</html>
