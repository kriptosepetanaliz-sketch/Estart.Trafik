<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Estart Trafik</title>
    <style>
        body { background-color: #0b0e14; margin: 0; font-family: sans-serif; color: white; display: flex; flex-direction: column; align-items: center; height: 100vh; justify-content: space-evenly; -webkit-tap-highlight-color: transparent; }
        .header { text-align: center; }
        .header p { color: #00bcd4; font-size: 10px; margin: 0; font-weight: bold; letter-spacing: 2px; }
        .header h1 { font-size: 24px; margin: 5px 0; font-weight: 900; }
        #connectBtn { background: transparent; border: 2.5px solid #00bcd4; color: #00bcd4; padding: 12px 30px; border-radius: 30px; font-weight: bold; cursor: pointer; position: relative; z-index: 999; }
        #colorDisplay { width: 150px; height: 150px; background-color: #1a1d23; border-radius: 40px; border: 1px solid #333; transition: 0.3s; box-shadow: inset 0 0 15px rgba(0,0,0,0.5); }
        .grid { display: grid; grid-template-columns: 1fr 1fr; gap: 15px; width: 85%; }
        .grid button { border: none; border-radius: 20px; font-weight: bold; font-size: 18px; height: 70px; color: white; cursor: pointer; touch-action: manipulation; position: relative; z-index: 999; }
        .btn-red { background-color: #ff3b30; } .btn-blue { background-color: #007aff; }
        .btn-green { background-color: #34c759; } .btn-yellow { background-color: #ffcc00; color: black !important; }
        .btn-close { background-color: #2c2c2e; width: 85%; border: none; border-radius: 20px; height: 70px; color: white; font-weight: bold; font-size: 18px; cursor: pointer; z-index: 999; }
        .footer-link { border: 2px solid #00bcd4; color: #00bcd4; padding: 10px 30px; border-radius: 25px; font-size: 12px; text-decoration: none; font-weight: bold; }
    </style>
</head>
<body>
    <div class="header"><p>BU PROGRAM</p><h1>MEHMET EMİN ERTAŞ</h1><p>TARAFINDAN YAPILMIŞTIR</p></div>
    <button id="connectBtn" onclick="initBT()">Bluetooth'a Bağlan</button>
    <div id="colorDisplay"></div>
    <div class="grid">
        <button class="btn-red" onclick="control('R', '#ff3b30')">Kırmızı</button>
        <button class="btn-blue" onclick="control('B', '#007aff')">Mavi</button>
        <button class="btn-green" onclick="control('G', '#34c759')">Yeşil</button>
        <button class="btn-yellow" onclick="control('Y', '#ffcc00')">Sarı</button>
    </div>
    <button class="btn-close" onclick="control('S', '#1a1d23')">Kapat</button>
    <a href="https://www.estart.com.tr" class="footer-link">www.estart.com.tr</a>
    <script>
        let char;
        async function initBT() {
            if (!navigator.bluetooth) {
                alert("HATA: Güvenlik engeli! Lütfen 'localhost' veya HTTPS ile açın.");
                return;
            }
            try {
                const device = await navigator.bluetooth.requestDevice({ acceptAllDevices: true, optionalServices: ['00001101-0000-1000-8000-00805f9b34fb'] });
                const server = await device.gatt.connect();
                const service = await server.getPrimaryService('00001101-0000-1000-8000-00805f9b34fb');
                char = await service.getCharacteristic('00001101-0000-1000-8000-00805f9b34fb');
                document.getElementById('connectBtn').innerText = "Bağlantı Başarılı";
            } catch (e) { alert("Hata: " + e.message); }
        }
        async function control(cmd, color) {
            document.getElementById('colorDisplay').style.backgroundColor = color;
            if (char) { try { await char.writeValue(new TextEncoder().encode(cmd)); } catch (e) {} }
        }
    </script>
</body>
</html>
