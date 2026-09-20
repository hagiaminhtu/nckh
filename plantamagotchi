<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Plant Tamagotchi - Nuôi Cây Ảo Cảm Biến IoT</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>

    <div class="tamagotchi-container">
        <!-- Header thiết bị -->
        <div class="device-header">
            <div class="device-title">
                <span>🌱</span> PLANT-CHI v2.0
            </div>
            <div class="header-actions">
                <button id="soundToggle" class="icon-btn" title="Bật/Tắt âm thanh">🔊 Bật âm</button>
                <button id="btnOpenTsModal" class="icon-btn" title="Cài đặt ThingSpeak">⚙️ IoT</button>
            </div>
        </div>

        <!-- Màn hình Tamagotchi -->
        <div class="tamagotchi-screen">
            <!-- Khu vực chọn nhóm & loại cây -->
            <div class="plant-selector-card">
                <div class="selector-grid">
                    <div class="select-group">
                        <label for="groupSelect">Nhóm Cây</label>
                        <select id="groupSelect" class="custom-select">
                            <option value="ua_am">🌿 Cây Ưa Ẩm</option>
                            <option value="ua_kho">🌵 Cây Ưa Khô</option>
                        </select>
                    </div>
                    <div class="select-group">
                        <label for="plantSelect">Loài Cây</label>
                        <select id="plantSelect" class="custom-select">
                            <!-- Được render tự động từ app.js -->
                        </select>
                    </div>
                </div>

                <div class="plant-meta">
                    <span id="plantName" style="font-weight: 800; color: #166534;">🍅 Cây Cà Chua</span>
                    <span>Chuẩn ẩm: <b id="idealRange" style="color: #0284c7;">60% - 80%</b></span>
                </div>
                <div id="plantDesc" class="plant-desc-text">Thích đất ẩm đều, nhiều ánh sáng.</div>
            </div>

            <!-- Các thanh chỉ số sinh tồn (HP & Độ ẩm) -->
            <div class="stats-container">
                <!-- Buff 30 phút đếm ngược (nếu mở được từ rương) -->
                <div id="buffBadge" class="buff-badge">
                    <span>🛡️ BẢO HỘ 30 PHÚT:</span>
                    <span id="buffTimer">30:00</span>
                </div>

                <!-- Thanh máu HP -->
                <div class="stat-row">
                    <span class="stat-label">❤️ HP</span>
                    <div class="progress-track">
                        <div id="hpFill" class="progress-fill hp-healthy" style="width: 100%;"></div>
                    </div>
                    <span id="hpText" class="stat-value">100 / 100</span>
                </div>

                <!-- Thanh độ ẩm đất -->
                <div class="stat-row">
                    <span class="stat-label">💧 Độ ẩm</span>
                    <div class="progress-track">
                        <div id="moistureFill" class="progress-fill moisture-bar" style="width: 70%;"></div>
                    </div>
                    <span id="moistureValue" class="stat-value">70%</span>
                </div>

                <!-- Dòng trạng thái cảm xúc của độ ẩm -->
                <div class="moisture-status-wrapper">
                    <span>Trạng thái môi trường:</span>
                    <span id="moistureStatusText" class="moisture-status status-optimal">Lý tưởng (Độ ẩm chuẩn)</span>
                </div>
            </div>

            <!-- Sân khấu hiển thị nhân vật Cây Tamagotchi -->
            <div class="plant-stage" id="plantCharacter">
                <!-- SVG động sẽ được render tự động qua app.js -->
            </div>
        </div>

        <!-- Bảng điều khiển & Tương tác -->
        <div class="controls-section">
            <!-- Hàng nút hành động chính -->
            <div class="action-buttons-grid">
                <button id="chestBtn" class="btn-action btn-chest">
                    <span style="font-size: 1.4rem;">🎁</span>
                    <span>MỞ RƯƠNG HP</span>
                    <span id="chestAlertBadge" class="badge-alert">SẴN SÀNG!</span>
                </button>

                <button id="btnOpenMiniGame" class="btn-action btn-game">
                    <span style="font-size: 1.4rem;">🎮</span>
                    <span>VƯỢT CHƯỚNG NGẠI</span>
                </button>
            </div>

            <!-- Bảng chuyển đổi Giả lập / ThingSpeak Cloud -->
            <div class="simulator-panel">
                <div class="panel-header">
                    <span class="panel-title">📡 Nguồn Dữ Liệu Đo Đạc</span>
                    <div class="switch-wrapper">
                        <span>Giả lập</span>
                        <label class="switch">
                            <input type="checkbox" id="modeToggle">
                            <span class="slider-toggle"></span>
                        </label>
                        <span>ThingSpeak</span>
                    </div>
                </div>

                <div>
                    <span id="tsStatusBadge" class="status-badge demo">Chế độ Giả lập</span>
                </div>

                <!-- Khu vực thanh trượt giả lập để test ngay -->
                <div id="simulatorControls" class="slider-box">
                    <label>
                        <span>Thử nghiệm độ ẩm đất ảo:</span>
                        <b id="simSliderVal" style="color: #0284c7;">70%</b>
                    </label>
                    <input type="range" id="simMoistureSlider" class="sim-range" min="0" max="100" value="70">

                    <div class="quick-actions">
                        <button id="btnQuickWater" class="btn-secondary">💧 Tưới Nước Chuẩn</button>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- MODAL: RƯƠNG THẦN KỲ (HP CHEST) -->
    <div id="chestModal" class="modal">
        <div class="modal-content">
            <div class="modal-header">
                <h3 id="chestRewardTitle" class="modal-title">🎁 RƯƠNG THẦN KỲ MỞ RA!</h3>
                <button class="modal-close">&times;</button>
            </div>
            <div class="chest-visual-box">
                <div class="chest-icon-large">📦✨</div>
            </div>
            <div id="chestRewardDesc">
                <!-- Nội dung thưởng được nạp động từ app.js -->
            </div>
            <div style="text-align: center; margin-top: 18px;">
                <button class="btn-primary modal-close" style="width: 100%;">Tuyệt vời, nhận ngay!</button>
            </div>
        </div>
    </div>

    <!-- MODAL: MINI-GAME VƯỢT CHƯỚNG NGẠI VẬT -->
    <div id="miniGameModal" class="modal">
        <div class="modal-content" style="max-width: 520px; position: relative;">
            <div class="modal-header">
                <h3 class="modal-title">🌿 Cây Nhỏ Vượt Nguy Nan</h3>
                <button id="btnCloseMiniGame" class="modal-close">&times;</button>
            </div>

            <canvas id="miniGameCanvas" width="470" height="280"></canvas>

            <div class="game-instructions">
                💡 <b>Cách chơi:</b> Bấm phím <b>SPACE</b>, <b>Mũi tên Lên</b> hoặc <b>Chạm màn hình</b> để nhảy né ☁️ Mây ô nhiễm, 🧪 Hoá chất, 💨 Thuốc trừ sâu. Nhặt 💧 Giọt nước & 🚿 Bình nước để tăng máu!
            </div>

            <!-- Màn hình kết thúc game -->
            <div id="gameOverOverlay" class="game-over-overlay">
                <div class="game-over-title">☠️ CÂY BỊ TRÚNG ĐỘC!</div>
                <div class="game-stats-box">
                    <div>Tổng điểm né tránh: <b id="gameScoreDisplay">0</b></div>
                    <div>Nước tinh khiết thu được: <b id="gameWaterDisplay" style="color: #38bdf8;">0</b></div>
                    <div>Phần thưởng chuyển về Tamagotchi: <b id="gameHpRewardDisplay" style="color: #4ade80;">+0 HP</b></div>
                </div>
                <div class="game-btn-group">
                    <button id="btnRestartGame" class="btn-secondary" style="background: #ffffff; border: none; padding: 10px 18px;">🔄 Chơi lại</button>
                    <button id="btnClaimBonus" class="btn-primary">💖 Nhận Máu & Trở Về</button>
                </div>
            </div>
        </div>
    </div>

    <!-- MODAL: CÀI ĐẶT THINGSPEAK IOT -->
    <div id="tsModal" class="modal">
        <div class="modal-content">
            <div class="modal-header">
                <h3 class="modal-title">⚙️ Cấu hình ThingSpeak IoT</h3>
                <button class="modal-close">&times;</button>
            </div>
            <p style="font-size: 0.8rem; color: #64748b; margin-bottom: 14px;">
                Nhập thông tin Channel trên ThingSpeak của bạn. Vi điều khiển sẽ gửi dữ liệu độ ẩm vào <b>Field 1</b>.
            </p>
            <div class="form-group">
                <label for="tsChannelInput">Channel ID</label>
                <input type="text" id="tsChannelInput" class="form-input" placeholder="Ví dụ: 1234567">
                <div class="form-help">Xem trên trang chủ Channel của ThingSpeak.</div>
            </div>
            <div class="form-group">
                <label for="tsKeyInput">Read API Key (nếu Channel để Private)</label>
                <input type="text" id="tsKeyInput" class="form-input" placeholder="Ví dụ: 16 ký tự mã Read API Key">
                <div class="form-help">Nếu Channel của bạn để chế độ Public thì có thể để trống.</div>
            </div>
            <div style="display: flex; gap: 10px; margin-top: 20px;">
                <button id="btnSaveTsConfig" class="btn-primary" style="flex: 1;">Lưu & Kết nối</button>
            </div>
        </div>
    </div>

    <!-- Tải các kịch bản JavaScript -->
    <script src="audio.js"></script>
    <script src="minigame.js"></script>
    <script src="app.js"></script>
</body>
</html>
