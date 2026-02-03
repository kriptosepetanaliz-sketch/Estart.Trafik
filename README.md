<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <meta name="mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
    <title>Estart Trafik - Mehmet Emin Ertaş</title>
    <style>
        /* Tasarım ve Hizalama Ayarları */
        body { 
            background-color: #0b0e14; 
            margin: 0; 
            font-family: sans-serif; 
            color: white; 
            display: flex; 
            justify-content: center; 
            align-items: flex-start; 
            height: 100vh; 
            overflow: hidden; 
            -webkit-tap-highlight-color: transparent;
        }

        .container { 
            display: flex; 
            flex-direction: column; 
            align-items: center; 
            width: 100%; 
            max-width: 400px; 
            padding-top: 35px; /* S23 Ultra için ideal yukarı hizalama */
            text-align: center; 
        }
        
        .header { margin-bottom: 18px; }
        .header p { color: #00bcd4; font-size: 10px; margin: 0; font-weight: bold; letter-spacing: 1.5px; }
        .header h1 { font-size: 24px; margin: 5px 0; font-weight: 900; text-transform: uppercase; }

        /* Bluetooth Butonu */
        #connectBtn { 
            background: none; 
            border: 2px solid #00bcd4; 
            color: #00bcd4; 
            padding: 11px 26px; 
            border-radius: 25px; 
            font-weight: bold; 
            cursor: pointer; 
            margin-bottom: 18px; 
            font-size: 14px; 
            transition: 0.3s;
        }

        #colorDisplay { 
            width: 150px; 
            height: 150px; 
            background-color: #1a1d23; 
            border-radius: 38px; 
            border: 1px solid #333; 
            transition: 0.3s ease; 
            box-shadow: inset 0 0 15px rgba(0,0,0,0.5); 
        }

        .grid { 
            display: grid; 
            grid-template-columns: 1fr 1fr; 
            gap: 12px; 
            width: 88%; 
            margin-top: 22px; 
        }
        
        .grid button { 
            border: none; 
            border-radius: 18px; 
            font-weight: bold; 
            font-size: 18px; 
            cursor: pointer; 
            height: 65px; 
            color: white; 
            touch-action: manipulation;
        }
        
        .btn-red { background-color: #ff3b30; }
        .btn-blue { background-color: #007aff; }
        .btn-green { background-color: #34c759; }
        .btn-yellow { background-color: #ffcc00; color: black !important; }
        
        .btn-close { 
            background-color: #2c2c2e; 
            width: 88%; 
            border: none; 
            border-radius: 18px; 
            height: 65px; 
            color: white; 
            font-weight: bold; 
            font-size: 18px; 
            margin-top: 12px; 
            cursor: pointer; 
        }

        /* Bluetooth Butonu ile Aynı Formatta Alt Link */
        .footer-btn { 
            background: none; 
            border: 2px solid #00bcd4; 
            color: #00bcd4; 
            padding: 11px 26px; 
            border-radius: 25px; 
            font-size: 14px; 
            font-weight: bold; 
            margin-top: 30px; 
            text-decoration: none; 
            display: inline-block; 
        }
    </style>
</head>
<body>

    <div class="container">
        <div class="header">
            <p>BU PROGRAM</p>
            <h1>MEHMET EMİN ERTAŞ</h1>
            <p>TARAFINDAN YAPILMIŞTIR</p>
        </div>

        <button id="connectBtn" onclick="connectBluetooth()">Bluetooth'a Bağlan</button>
        <div id="colorDisplay"></div>

        <div class="grid">
            <button class="btn-red" onclick="updateUI('#ff3b30', 'R')">Kırmızı</button>
            <button class="btn-blue" onclick="updateUI('#007aff', 'B')">Mavi</button>
            <button class="btn-green" onclick="updateUI('#34c759', 'G')">Yeşil</button>
            <button class="btn-yellow" onclick="updateUI('#ffcc00', 'Y')">Sarı</button>
        </div>

        <button class="btn-close" onclick="updateUI('#1a1d23', 'S')">Kapat</button>
        <a href="https://www.estart.com.tr" class="footer-btn">www.estart.com.tr</a>
    </div>

    <script>
        let bleChar;

        async function connectBluetooth() {
            // Tarayıcı ve Protokol Kontrolü
            if (!navigator.bluetooth) {
                alert("Bluetooth erişimi için güvenli bağlantı (HTTPS) gereklidir. Lütfen APK içinde bu adresi yüklediğinizden emin olun.");
                return;
            }

            try {
                const device = await navigator.bluetooth.requestDevice({
                    acceptAllDevices: true,
                    optionalServices: ['00001101-0000-1000-8000-00805f9b34fb']
                });
                
                const server = await device.gatt.connect();
                const service = await server.getPrimaryService('00001101-0000-1000-8000-00805f9b34fb');
                bleChar = await service.getCharacteristic('00001101-0000-1000-8000-00805f9b34fb');
                
                document.getElementById('connectBtn').innerText = "Bağlandı ✅";
                document.getElementById('connectBtn').style.borderColor = "#34c759";
                document.getElementById('connectBtn').style.color = "#34c759";
            } catch (error) {
                console.log("Bağlantı iptal edildi veya hata: " + error);
            }
        }

        async function updateUI(color, char) {
            const display = document.getElementById('colorDisplay');
            display.style.backgroundColor = color;
            display.style.boxShadow = (char === 'S') ? "none" : "0 0 35px " + color;

            if (bleChar) {
                try {
                    await bleChar.writeValue(new TextEncoder().encode(char));
                } catch (err) {
                    console.error("Veri gönderilemedi:", err);
                }
            }
        }
    </script>
</body>
</html>
