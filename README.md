# smart-house-app[index.html](https://github.com/user-attachments/files/25909250/index.html)
<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Smart Room Monitor</title>
    
    <!-- PWA Settings -->
    <meta name="theme-color" content="#0f172a">
    <meta name="mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
    <meta name="apple-mobile-web-app-title" content="Smart Room">
    <link rel="manifest" href="manifest.json">
    <link rel="apple-touch-icon" href="icon-192.png">

    <!-- Google Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <!-- Phosphor Icons -->
    <script src="https://unpkg.com/@phosphor-icons/web"></script>
    <style>
        :root {
            --bg-color: #0f172a;
            --card-bg: rgba(30, 41, 59, 0.7);
            --card-border: rgba(255, 255, 255, 0.1);
            --text-primary: #f8fafc;
            --text-secondary: #94a3b8;
            --accent-blue: #3b82f6;
            --accent-green: #10b981;
            --accent-red: #ef4444;
            --accent-yellow: #f59e0b;
        }

        body {
            font-family: 'Outfit', sans-serif;
            margin: 0;
            padding: 0;
            background-color: var(--bg-color);
            background-image: 
                radial-gradient(at 0% 0%, rgba(59, 130, 246, 0.15) 0px, transparent 50%),
                radial-gradient(at 100% 0%, rgba(16, 185, 129, 0.15) 0px, transparent 50%);
            color: var(--text-primary);
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            align-items: center;
        }

        .header {
            margin-top: 3rem;
            text-align: center;
        }

        .header h1 {
            font-size: 2.5rem;
            font-weight: 700;
            margin: 0;
            background: linear-gradient(to right, #60a5fa, #34d399);
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
        }

        .connection-box {
            margin-top: 2rem;
            display: flex;
            gap: 10px;
            background: var(--card-bg);
            padding: 10px 20px;
            border-radius: 9999px;
            border: 1px solid var(--card-border);
            backdrop-filter: blur(10px);
            align-items: center;
        }

        .connection-box input {
            background: transparent;
            border: none;
            color: var(--text-primary);
            font-family: inherit;
            font-size: 1rem;
            outline: none;
            width: 140px;
        }

        .connection-box input::placeholder {
            color: var(--text-secondary);
        }

        .connection-status {
            display: flex;
            align-items: center;
            gap: 6px;
            font-size: 0.875rem;
            font-weight: 500;
        }

        .status-dot {
            width: 8px;
            height: 8px;
            border-radius: 50%;
            background-color: var(--accent-red);
            transition: background-color 0.3s ease;
        }
        
        .status-dot.connected {
            background-color: var(--accent-green);
            box-shadow: 0 0 10px var(--accent-green);
        }

        .dashboard {
            margin-top: 3rem;
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
            gap: 1.5rem;
            width: 100%;
            max-width: 1000px;
            padding: 0 2rem;
            box-sizing: border-box;
            margin-bottom: 3rem;
        }

        .card {
            background: var(--card-bg);
            border: 1px solid var(--card-border);
            border-radius: 24px;
            padding: 2rem;
            backdrop-filter: blur(12px);
            -webkit-backdrop-filter: blur(12px);
            display: flex;
            flex-direction: column;
            transition: transform 0.3s ease, box-shadow 0.3s ease;
            position: relative;
            overflow: hidden;
        }

        .card:hover {
            transform: translateY(-5px);
            box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.5), 0 8px 10px -6px rgba(0, 0, 0, 0.5);
        }

        .card-header {
            display: flex;
            align-items: center;
            gap: 12px;
            margin-bottom: 1.5rem;
        }

        .icon-container {
            width: 48px;
            height: 48px;
            border-radius: 16px;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            font-size: 24px;
        }

        .card-title {
            font-size: 1.125rem;
            font-weight: 500;
            color: var(--text-secondary);
            margin: 0;
        }

        .card-value {
            font-size: 3rem;
            font-weight: 700;
            margin: 0;
            display: flex;
            align-items: baseline;
            gap: 8px;
        }

        .card-unit {
            font-size: 1.25rem;
            font-weight: 500;
            color: var(--text-secondary);
        }

        /* Specific Card Styles */
        .temp-icon { background: rgba(239, 68, 68, 0.1); color: var(--accent-red); }
        .hum-icon { background: rgba(59, 130, 246, 0.1); color: var(--accent-blue); }
        .motion-icon { background: rgba(245, 158, 11, 0.1); color: var(--accent-yellow); }
        .device-icon { background: rgba(16, 185, 129, 0.1); color: var(--accent-green); }

        .motion-status {
            font-size: 1.5rem;
            font-weight: 600;
            margin-top: 10px;
        }

        .motion-alert {
            color: var(--accent-red);
            animation: pulse 2s infinite;
        }

        .motion-clear {
            color: var(--accent-green);
        }

        @keyframes pulse {
            0% { opacity: 1; }
            50% { opacity: 0.5; }
            100% { opacity: 1; }
        }

        .device-list {
            display: flex;
            flex-direction: column;
            gap: 1rem;
            margin-top: 10px;
        }

        .device-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            background: rgba(0, 0, 0, 0.2);
            padding: 12px 16px;
            border-radius: 12px;
        }

        .device-name {
            display: flex;
            align-items: center;
            gap: 10px;
            font-weight: 500;
        }

        .badge {
            padding: 4px 12px;
            border-radius: 9999px;
            font-size: 0.75rem;
            font-weight: 600;
            letter-spacing: 0.05em;
        }

        .badge.on {
            background: rgba(16, 185, 129, 0.2);
            color: var(--accent-green);
            box-shadow: 0 0 10px rgba(16, 185, 129, 0.2);
        }

        .badge.off {
            background: rgba(255, 255, 255, 0.1);
            color: var(--text-secondary);
        }

        /* Progress Circle for Temp/Humidity */
        .progress-bar {
            width: 100%;
            height: 6px;
            background: rgba(255, 255, 255, 0.1);
            border-radius: 3px;
            margin-top: 1.5rem;
            overflow: hidden;
        }

        .progress-fill {
            height: 100%;
            border-radius: 3px;
            transition: width 0.5s ease;
        }

        .temp-fill { background: linear-gradient(90deg, #f59e0b, #ef4444); }
        .hum-fill { background: linear-gradient(90deg, #60a5fa, #3b82f6); }
        
        .error-message {
            color: var(--accent-red);
            font-size: 0.875rem;
            margin-top: 10px;
            text-align: center;
            height: 20px;
        }

        /* Nút Điều Khiển */
        .control-group {
            display: flex;
            gap: 8px;
            margin-top: 8px;
            width: 100%;
        }

        .control-btn {
            flex: 1;
            padding: 8px 0;
            border: 1px solid rgba(255, 255, 255, 0.2);
            border-radius: 8px;
            background: rgba(0, 0, 0, 0.3);
            color: var(--text-secondary);
            font-family: inherit;
            font-size: 0.85rem;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.2s;
        }

        .control-btn:hover {
            background: rgba(255, 255, 255, 0.1);
        }

        .control-btn.active-on {
            background: rgba(16, 185, 129, 0.8);
            border-color: rgba(16, 185, 129, 1);
            color: white;
            box-shadow: 0 0 10px rgba(16, 185, 129, 0.4);
        }

        .control-btn.active-off {
            background: rgba(239, 68, 68, 0.8);
            border-color: rgba(239, 68, 68, 1);
            color: white;
            box-shadow: 0 0 10px rgba(239, 68, 68, 0.4);
        }

        .control-btn.active-auto {
            background: rgba(59, 130, 246, 0.8);
            border-color: rgba(59, 130, 246, 1);
            color: white;
            box-shadow: 0 0 10px rgba(59, 130, 246, 0.4);
        }

    </style>
</head>
<body>

    <div class="header">
        <h1>Phòng Thông Minh</h1>
        <div class="time-display" id="timeDisplay" style="font-size: 1.25rem; color: var(--text-secondary); margin-top: 10px; font-weight: 500;">
            --:--:--
        </div>
        <div class="connection-box">
            <i class="ph ph-wifi-high" style="color: var(--text-secondary);"></i>
            <input type="text" id="ipAddress" placeholder="e.g. 192.168.1.50" value="192.168.1.50">
            <div class="connection-status">
                <div class="status-dot" id="statusDot"></div>
                <span id="statusText">Đang chờ</span>
            </div>
        </div>
        <div class="error-message" id="errorMessage"></div>
    </div>

    <div class="dashboard">
        <!-- Temperature Card -->
        <div class="card">
            <div class="card-header">
                <div class="icon-container temp-icon">
                    <i class="ph ph-thermometer"></i>
                </div>
                <h2 class="card-title">Nhiệt độ</h2>
            </div>
            <div class="card-value">
                <span id="tempValue">--</span>
                <span class="card-unit">°C</span>
            </div>
            <div class="progress-bar">
                <div class="progress-fill temp-fill" id="tempBar" style="width: 0%"></div>
            </div>
        </div>

        <!-- Humidity Card -->
        <div class="card">
            <div class="card-header">
                <div class="icon-container hum-icon">
                    <i class="ph ph-drop"></i>
                </div>
                <h2 class="card-title">Độ ẩm</h2>
            </div>
            <div class="card-value">
                <span id="humValue">--</span>
                <span class="card-unit">%</span>
            </div>
            <div class="progress-bar">
                <div class="progress-fill hum-fill" id="humBar" style="width: 0%"></div>
            </div>
        </div>

        <!-- Motion Card -->
        <div class="card">
            <div class="card-header">
                <div class="icon-container motion-icon">
                    <i class="ph ph-person-simple-walk"></i>
                </div>
                <h2 class="card-title">Chuyển động</h2>
            </div>
            <div class="motion-status motion-clear" id="motionStatus">
                Chưa xác định
            </div>
            <p style="color: var(--text-secondary); font-size: 0.875rem; margin-top: 1rem;">
                Cảm biến PIR sẽ quét liên tục.
            </p>
        </div>

        <!-- Devices Card -->
        <div class="card">
            <div class="card-header">
                <div class="icon-container device-icon">
                    <i class="ph ph-plugs-connected"></i>
                </div>
                <h2 class="card-title">Thiết bị</h2>
            </div>
            <div class="device-list">
                <!-- Quạt -->
                <div class="device-item" style="flex-direction: column; align-items: stretch; gap: 10px;">
                    <div style="display: flex; justify-content: space-between; align-items: center;">
                        <div class="device-name">
                            <i class="ph ph-fan"></i> Quạt
                        </div>
                        <div class="badge off" id="fanBadge">TẮT</div>
                    </div>
                    <div class="control-group">
                        <button class="control-btn" id="btnFanOn" onclick="controlDevice('fan', 'on')">BẬT</button>
                        <button class="control-btn" id="btnFanOff" onclick="controlDevice('fan', 'off')">TẮT</button>
                        <button class="control-btn" id="btnFanAuto" onclick="controlDevice('fan', 'auto')">AUTO</button>
                    </div>
                </div>

                <!-- Đèn -->
                <div class="device-item" style="flex-direction: column; align-items: stretch; gap: 10px;">
                    <div style="display: flex; justify-content: space-between; align-items: center;">
                        <div class="device-name">
                            <i class="ph ph-lightbulb"></i> Đèn
                        </div>
                        <div class="badge off" id="lightBadge">TẮT</div>
                    </div>
                    <div class="control-group">
                        <button class="control-btn" id="btnLightOn" onclick="controlDevice('light', 'on')">BẬT</button>
                        <button class="control-btn" id="btnLightOff" onclick="controlDevice('light', 'off')">TẮT</button>
                        <button class="control-btn" id="btnLightAuto" onclick="controlDevice('light', 'auto')">AUTO</button>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <script>
        // DOM Elements
        const ipInput = document.getElementById('ipAddress');
        const statusDot = document.getElementById('statusDot');
        const statusText = document.getElementById('statusText');
        const errorMessage = document.getElementById('errorMessage');

        const tempValue = document.getElementById('tempValue');
        const tempBar = document.getElementById('tempBar');
        const humValue = document.getElementById('humValue');
        const humBar = document.getElementById('humBar');
        
        const motionStatus = document.getElementById('motionStatus');
        const fanBadge = document.getElementById('fanBadge');
        const lightBadge = document.getElementById('lightBadge');
        const timeDisplay = document.getElementById('timeDisplay');

        let fetchInterval;
        let clockInterval;

        // Cập nhật Đồng hồ thời gian thực
        function updateClock() {
            const now = new Date();
            const timeString = now.toLocaleTimeString('vi-VN', { hour12: false });
            const dateString = now.toLocaleDateString('vi-VN');
            timeDisplay.textContent = `${timeString} - ${dateString}`;
        }

        // Bắt đầu chạy đồng hồ
        function startClock() {
            updateClock();
            clockInterval = setInterval(updateClock, 1000);
        }

        function updateUI(data) {
            // Update Temp
            tempValue.textContent = data.temperature.toFixed(1);
            let tempPercent = (data.temperature / 50) * 100; // Assuming 50C is max
            tempBar.style.width = `${Math.min(tempPercent, 100)}%`;

            // Update Humidity
            humValue.textContent = data.humidity.toFixed(1);
            humBar.style.width = `${data.humidity}%`;

            // Update Motion
            if (data.motion) {
                motionStatus.textContent = "Phát hiện có người!";
                motionStatus.className = "motion-status motion-alert";
            } else {
                motionStatus.textContent = "Không có người";
                motionStatus.className = "motion-status motion-clear";
            }

            // Update Devices (Fan)
            if (data.fan) {
                fanBadge.textContent = "BẬT";
                fanBadge.className = "badge on";
            } else {
                fanBadge.textContent = "TẮT";
                fanBadge.className = "badge off";
            }

            // Update Control Buttons (Fan)
            document.getElementById('btnFanOn').className = "control-btn " + (data.manualFan && data.fan ? "active-on" : "");
            document.getElementById('btnFanOff').className = "control-btn " + (data.manualFan && !data.fan ? "active-off" : "");
            document.getElementById('btnFanAuto').className = "control-btn " + (!data.manualFan ? "active-auto" : "");

            // Update Devices (Light)
            if (data.light) {
                lightBadge.textContent = "BẬT";
                lightBadge.className = "badge on";
            } else {
                lightBadge.textContent = "TẮT";
                lightBadge.className = "badge off";
            }

            // Update Control Buttons (Light)
            document.getElementById('btnLightOn').className = "control-btn " + (data.manualLight && data.light ? "active-on" : "");
            document.getElementById('btnLightOff').className = "control-btn " + (data.manualLight && !data.light ? "active-off" : "");
            document.getElementById('btnLightAuto').className = "control-btn " + (!data.manualLight ? "active-auto" : "");
        }

        // Hàm gọi lệnh điều khiển thủ công
        async function controlDevice(device, state) {
            const ip = ipInput.value.trim();
            if (!ip) return;
            try {
                // Send form data logic
                const params = new URLSearchParams();
                params.append('device', device);
                params.append('state', state);

                await fetch(`http://${ip}/api/control`, {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/x-www-form-urlencoded' },
                    body: params
                });

                // Tăng tốc độ load lại sau khi vừa bấm nút (trải nghiệm mượt)
                fetchData();
            } catch (error) {
                console.error("Lỗi điều khiển thiết bị", error);
                errorMessage.textContent = "Lỗi đường truyền lệnh đến ESP32!";
            }
        }

        async function fetchData() {
            const ip = ipInput.value.trim();
            if (!ip) return;

            try {
                const response = await fetch(`http://${ip}/api/status`, {
                    method: 'GET',
                    headers: { 'Accept': 'application/json' },
                    signal: AbortSignal.timeout(3000) // 3 seconds timeout
                });

                if (response.ok) {
                    const data = await response.json();
                    updateUI(data);
                    
                    // Update connection status
                    statusDot.classList.add('connected');
                    statusText.textContent = "Đã kết nối";
                    statusText.style.color = "var(--accent-green)";
                    errorMessage.textContent = "";
                } else {
                    throw new Error("HTTP Error");
                }
            } catch (error) {
                // Connection failed
                statusDot.classList.remove('connected');
                statusText.textContent = "Mất kết nối";
                statusText.style.color = "var(--accent-red)";
                errorMessage.textContent = "Không thể kết nối đến ESP32. Vui lòng kiểm tra lại IP.";
            }
        }

        // Start fetching data every 2 seconds
        function startFetching() {
            if (fetchInterval) clearInterval(fetchInterval);
            fetchData(); // Fetch immediately
            fetchInterval = setInterval(fetchData, 2000);
        }

        // Listen for IP changes
        ipInput.addEventListener('change', startFetching);
        
        // Initial start
        startClock();
        startFetching();

        // Register Service Worker cho PWA (để cài đặt thành App)
        if ('serviceWorker' in navigator) {
            window.addEventListener('load', () => {
                navigator.serviceWorker.register('sw.js')
                    .then(registration => {
                        console.log('ServiceWorker Đã đăng ký thành công: ', registration.scope);
                    })
                    .catch(err => {
                        console.log('ServiceWorker Đăng ký thất bại: ', err);
                    });
            });
        }
    </script>
</body>
</html>
[manifest.json](https://github.com/user-attachments/files/25909285/manifest.json)
[sw.js](https://github.com/user-attachments/files/25909296/sw.js)const CACHE_NAME = 'smart-room-v1';
const urlsToCache = [
  './index.html',
  './manifest.json'
];

self.addEventListener('install', event => {
  event.waitUntil(
    caches.open(CACHE_NAME)
      .then(cache => {
        return cache.addAll(urlsToCache);
      })
  );
});

// Bỏ qua lấy dữ liệu cache với các lệnh gọi API (cần realtime)
self.addEventListener('fetch', event => {
  if (event.request.url.includes('/api/')) {
    event.respondWith(fetch(event.request)); // Luôn gọi API từ mạng
  } else {
    event.respondWith(
      caches.match(event.request)
        .then(response => {
          if (response) {
            return response;
          }
          return fetch(event.request);
        }
      )
    );
  }
});

