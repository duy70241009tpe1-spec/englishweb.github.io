<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Siêu Ứng Dụng Học Tập - Pro</title>
    <style>
        :root {
            --primary-color: #3498db; --secondary-color: #e74c3c; --bg-color: #f0f2f5;
            --en-color: #007bff; --cn-color: #e67e22; --success-color: #27ae60;
        }
        body { font-family: 'Segoe UI', Tahoma, sans-serif; background-color: var(--bg-color); display: flex; flex-direction: column; align-items: center; padding: 20px; margin: 0; }
        
        .file-manager { background: #fff; padding: 15px; border-radius: 15px; width: 100%; max-width: 500px; margin-bottom: 15px; box-shadow: 0 4px 10px rgba(0,0,0,0.05); }
        .file-controls { display: flex; gap: 10px; align-items: center; }
        select#file-selector { flex-grow: 1; padding: 10px; border-radius: 8px; border: 2px solid #ddd; outline: none; font-weight: bold; }
        .file-btn { padding: 10px 15px; border: none; border-radius: 8px; cursor: pointer; color: white; font-weight: bold; }
        
        .container { background: white; padding: 25px; border-radius: 20px; box-shadow: 0 10px 25px rgba(0,0,0,0.1); width: 100%; max-width: 500px; display: none; margin-bottom: 20px; }
        .container.active { display: block; }
        h2 { text-align: center; color: var(--primary-color); margin-top: 0; }
        
        .search-container { position: relative; width: 100%; margin-bottom: 10px; }
        .suggestions { position: absolute; top: 100%; left: 0; right: 0; background: white; border: 1px solid #ddd; border-radius: 0 0 10px 10px; z-index: 100; box-shadow: 0 5px 15px rgba(0,0,0,0.1); display: none; max-height: 200px; overflow-y: auto; }
        .suggestion-item { padding: 10px; cursor: pointer; border-bottom: 1px solid #eee; font-size: 0.9rem; }
        .suggestion-item:hover { background: #f0f7ff; }

        .filter-group { display: flex; gap: 5px; margin-bottom: 15px; justify-content: center; }
        .filter-btn { padding: 5px 12px; border: 1px solid #ddd; border-radius: 15px; background: white; font-size: 0.75rem; cursor: pointer; color: #666; }
        .filter-btn.active { background: #333; color: white; border-color: #333; }

        .input-group { margin-bottom: 12px; }
        label { font-size: 0.85rem; font-weight: bold; margin-bottom: 4px; display: block; color: #555; }
        input { width: 100%; padding: 10px; border: 2px solid #eee; border-radius: 8px; box-sizing: border-box; font-size: 1rem; }
        
        .addBtn { width: 100%; padding: 12px; background-color: var(--primary-color); color: white; border: none; border-radius: 10px; cursor: pointer; font-weight: bold; margin-top: 10px; }
        .nav-tabs { display: flex; gap: 15px; margin-bottom: 20px; }
        .tab-btn { padding: 12px 25px; border: none; border-radius: 10px; cursor: pointer; font-weight: bold; color: white; opacity: 0.6; transition: 0.3s; }
        .tab-btn.active { opacity: 1; transform: scale(1.1); }

        .word-item { background: white; margin-bottom: 10px; padding: 15px 15px 15px 45px; border-radius: 12px; display: flex; flex-direction: column; box-shadow: 0 2px 8px rgba(0,0,0,0.05); position: relative; border-left: 6px solid #ccc; transition: 0.3s; }
        .select-item-check { position: absolute; left: 15px; top: 50%; transform: translateY(-50%); width: 18px !important; height: 18px; cursor: pointer; }
        
        .action-btns { position: absolute; top: 10px; right: 10px; display: flex; gap: 10px; align-items: center; }
        .status-btn, .mini-flash-btn { border: none; background: none; cursor: pointer; font-size: 1.1rem; padding: 0; transition: 0.2s; }
        .status-btn:hover { transform: scale(1.2); }

        .bulk-delete-bar { display: none; background: #fff1f0; border: 1px solid #ffa39e; padding: 10px; border-radius: 10px; margin-bottom: 15px; justify-content: space-between; align-items: center; width: 100%; box-sizing: border-box; }
        
        .topic-tag { align-self: flex-start; font-size: 0.65rem; background: #e8f0fe; color: #1a73e8; padding: 2px 8px; border-radius: 10px; font-weight: bold; margin-bottom: 5px; }
        .ipa-box { color: #2980b9; font-size: 0.95rem; display: flex; align-items: center; gap: 8px; margin: 4px 0; }
        .speak-icon { cursor: pointer; font-size: 1.2rem; filter: grayscale(100%); }
        .speak-icon:hover { filter: grayscale(0%); }
        .cn-char { font-size: 1.8rem; color: #333; margin: 5px 0; font-family: "Microsoft YaHei", sans-serif; }

        /* Flashcard Area */
        .flashcard-area { perspective: 1000px; margin: 20px 0; display: none; position: relative; width: 100%; }
        .flashcard { width: 100%; height: 220px; transition: transform 0.6s; transform-style: preserve-3d; cursor: pointer; position: relative; }
        .flashcard.flipped { transform: rotateY(180deg); }
        .card-face { position: absolute; width: 100%; height: 100%; backface-visibility: hidden; display: flex; flex-direction: column; justify-content: center; align-items: center; border-radius: 15px; font-size: 1.5rem; font-weight: bold; box-shadow: 0 8px 20px rgba(0,0,0,0.15); padding: 20px; box-sizing: border-box; background: white; border: 3px solid var(--primary-color); text-align: center; }
        .card-back { transform: rotateY(180deg); border-style: dashed; color: #333; }
        .close-flash { position: absolute; top: -15px; right: -5px; background: #e74c3c; color: white; border: none; width: 35px; height: 35px; border-radius: 50%; cursor: pointer; font-weight: bold; z-index: 10; }
    </style>
</head>
<body>

    <div class="file-manager">
        <label style="margin-bottom: 8px; font-size: 0.8rem; font-weight: bold;">📂 QUẢN LÝ FILE HỌC:</label>
        <div class="file-controls">
            <select id="file-selector" onchange="switchFile()"></select>
            <button class="file-btn" style="background: #2ecc71;" onclick="createNewFile()">+</button>
            <button class="file-btn" style="background: #e74c3c;" onclick="deleteCurrentFile()">×</button>
        </div>
    </div>

    <div class="nav-tabs">
        <button class="tab-btn active" style="background:var(--en-color)" onclick="openTab(event, 'english')">Tiếng Anh</button>
        <button class="tab-btn" style="background:var(--cn-color)" onclick="openTab(event, 'chinese')">Tiếng Trung</button>
    </div>

    <div id="english" class="container active">
        <h2 style="color:var(--en-color)">English Studio</h2>
        
        <div id="en-flash-area" class="flashcard-area">
            <button class="close-flash" onclick="closeFlashcard('en')">✕</button>
            <div class="flashcard" onclick="this.classList.toggle('flipped')">
                <div class="card-face" id="en-front"></div>
                <div class="card-face card-back" id="en-back"></div>
            </div>
            <button class="addBtn" id="en-next-btn" style="background:#2d3436; display:none" onclick="startBulkFlashcard('en')">Từ tiếp theo ➔</button>
        </div>

        <div id="en-main-ui">
            <div class="search-container">
                <input type="text" id="en-search" placeholder="🔍 Tìm từ hoặc Topic..." onkeyup="handleSearch('en')" onfocus="showSuggestions('en')">
                <div id="en-suggestions" class="suggestions"></div>
            </div>
            <div class="filter-group">
                <button class="filter-btn active" onclick="setFilter('en', 'all')">Tất cả</button>
                <button class="filter-btn" onclick="setFilter('en', 'learning')">⏳ Đang học</button>
                <button class="filter-btn" onclick="setFilter('en', 'learned')">✅ Đã thuộc</button>
            </div>

            <button class="addBtn" style="background:#2d3436; margin-bottom:15px;" onclick="startBulkFlashcard('en')">🎴 Ôn tập mục "Đang học"</button>

            <div id="en-bulk-bar" class="bulk-delete-bar"><span id="en-count">0 mục</span><button onclick="bulkDelete('en')">Xóa mục chọn</button></div>
            
            <div id="en-input-fields">
                <div class="input-group"><label>Từ mới</label><input id="en-w"></div>
                <div class="input-group"><label>IPA & Nghĩa</label>
                    <div style="display:flex; gap:5px"><input id="en-ipa" placeholder="IPA" style="width:40%"><input id="en-m" placeholder="Nghĩa" style="width:60%"></div>
                </div>
                <div class="input-group"><label>Topic</label><input id="en-t"></div>
                <button class="addBtn" style="background:var(--en-color)" onclick="saveWord('en')">Lưu từ vựng</button>
            </div>
            <div id="en-list" style="margin-top:20px"></div>
        </div>
    </div>

    <div id="chinese" class="container">
        <h2 style="color:var(--cn-color)">Hán Ngữ Studio</h2>
        
        <div id="cn-flash-area" class="flashcard-area">
            <button class="close-flash" onclick="closeFlashcard('cn')">✕</button>
            <div class="flashcard" onclick="this.classList.toggle('flipped')">
                <div class="card-face" id="cn-front" style="border-color:var(--cn-color)"></div>
                <div class="card-face card-back" id="cn-back" style="border-color:var(--cn-color)"></div>
            </div>
            <button class="addBtn" id="cn-next-btn" style="background:#2d3436; display:none" onclick="startBulkFlashcard('cn')">Từ tiếp theo ➔</button>
        </div>

        <div id="cn-main-ui">
            <div class="search-container">
                <input type="text" id="cn-search" placeholder="🔍 Tìm Hán tự hoặc Topic..." onkeyup="handleSearch('cn')" onfocus="showSuggestions('cn')">
                <div id="cn-suggestions" class="suggestions"></div>
            </div>
            <div class="filter-group">
                <button class="filter-btn active" onclick="setFilter('cn', 'all')">Tất cả</button>
                <button class="filter-btn" onclick="setFilter('cn', 'learning')">⏳ Đang học</button>
                <button class="filter-btn" onclick="setFilter('cn', 'learned')">✅ Đã thuộc</button>
            </div>

            <button class="addBtn" style="background:#2d3436; margin-bottom:15px;" onclick="startBulkFlashcard('cn')">🎴 Ôn tập mục "Đang học"</button>

            <div id="cn-bulk-bar" class="bulk-delete-bar"><span id="cn-count">0 mục</span><button onclick="bulkDelete('cn')">Xóa mục chọn</button></div>
            
            <div id="cn-input-fields">
                <div class="input-group"><label>Hán tự</label><input id="cn-w"></div>
                <div class="input-group"><label>Pinyin & Nghĩa</label>
                    <div style="display:flex; gap:5px"><input id="cn-p" placeholder="Pinyin" style="width:40%"><input id="cn-m" placeholder="Nghĩa" style="width:60%"></div>
                </div>
                <div class="input-group"><label>Topic</label><input id="cn-t"></div>
                <button class="addBtn" style="background:var(--cn-color)" onclick="saveWord('cn')">Lưu Hán tự</button>
            </div>
            <div id="cn-display-list" style="margin-top:20px"></div>
        </div>
    </div>

    <script>
        let synth = window.speechSynthesis;
        let currentFilters = { en: 'all', cn: 'all' };
        let currentFileName = "File_Mac_Dinh";

        // Quản lý Files
        function initFiles() {
            let files = JSON.parse(localStorage.getItem('APP_FILES')) || ["File_Mac_Dinh"];
            localStorage.setItem('APP_FILES', JSON.stringify(files));
            currentFileName = localStorage.getItem('CURRENT_FILE_NAME') || files[0];
            updateFileSelector();
        }

        function updateFileSelector() {
            const files = JSON.parse(localStorage.getItem('APP_FILES'));
            document.getElementById('file-selector').innerHTML = files.map(f => `<option value="${f}" ${f===currentFileName?'selected':''}>${f.replace(/_/g,' ')}</option>`).join('');
        }

        function createNewFile() {
            let name = prompt("Tên file mới (không dấu, dùng _ thay cách):");
            if (!name) return;
            name = name.trim().replace(/\s+/g, '_');
            let files = JSON.parse(localStorage.getItem('APP_FILES'));
            if (files.includes(name)) return alert("Đã có tên này!");
            files.push(name);
            localStorage.setItem('APP_FILES', JSON.stringify(files));
            currentFileName = name; localStorage.setItem('CURRENT_FILE_NAME', name);
            updateFileSelector(); renderAll();
        }

        function switchFile() {
            currentFileName = document.getElementById('file-selector').value;
            localStorage.setItem('CURRENT_FILE_NAME', currentFileName);
            renderAll();
        }

        function deleteCurrentFile() {
            let files = JSON.parse(localStorage.getItem('APP_FILES'));
            if (files.length <= 1) return alert("Không thể xóa file cuối cùng!");
            if (!confirm(`Xóa toàn bộ dữ liệu file "${currentFileName}"?`)) return;
            localStorage.removeItem(`${currentFileName}_EN`); localStorage.removeItem(`${currentFileName}_CN`);
            files = files.filter(f => f !== currentFileName);
            localStorage.setItem('APP_FILES', JSON.stringify(files));
            currentFileName = files[0]; localStorage.setItem('CURRENT_FILE_NAME', currentFileName);
            updateFileSelector(); renderAll();
        }

        // Lưu và Trạng thái
        function saveWord(lang) {
            const w = document.getElementById(`${lang}-w`).value, m = document.getElementById(`${lang}-m`).value, t = document.getElementById(`${lang}-t`).value || 'Chung';
            const extra = lang === 'en' ? document.getElementById('en-ipa').value : document.getElementById('cn-p').value;
            if(!w || !m) return alert("Vui lòng điền đủ từ và nghĩa!");
            const key = `${currentFileName}_${lang.toUpperCase()}`;
            const data = JSON.parse(localStorage.getItem(key)) || [];
            data.push({w, m, t, extra, id: Date.now(), isLearned: false});
            localStorage.setItem(key, JSON.stringify(data));
            renderAll();
            [`${lang}-w`, `${lang}-m`, `${lang}-t`, lang==='en'?'en-ipa':'cn-p'].forEach(id => document.getElementById(id).value = '');
        }

        function toggleStatus(lang, id) {
            const key = `${currentFileName}_${lang.toUpperCase()}`;
            let data = JSON.parse(localStorage.getItem(key));
            data = data.map(item => { if(item.id === id) item.isLearned = !item.isLearned; return item; });
            localStorage.setItem(key, JSON.stringify(data));
            renderAll();
        }

        // Flashcard Logic
        function startSingleFlashcard(lang, id) {
            const data = JSON.parse(localStorage.getItem(`${currentFileName}_${lang.toUpperCase()}`)) || [];
            const item = data.find(i => i.id === id);
            showFlashcardUI(lang, item, false);
        }

        function startBulkFlashcard(lang) {
            const data = (JSON.parse(localStorage.getItem(`${currentFileName}_${lang.toUpperCase()}`)) || []).filter(i => !i.isLearned);
            if(!data.length) return alert("Không có từ nào trong mục 'Đang học'!");
            const randomItem = data[Math.floor(Math.random() * data.length)];
            showFlashcardUI(lang, randomItem, true);
        }

        function showFlashcardUI(lang, item, isBulk) {
            document.getElementById(`${lang}-main-ui`).style.display = 'none';
            document.getElementById(`${lang}-flash-area`).style.display = 'block';
            document.getElementById(`${lang}-next-btn`).style.display = isBulk ? 'block' : 'none';
            
            const card = document.querySelector(`#${lang}-flash-area .flashcard`);
            card.classList.remove('flipped');
            document.getElementById(`${lang}-front`).innerHTML = `<div style="${lang==='cn'?'font-size:2.2rem':''}">${item.w}</div><small style="color:#666">${item.extra}</small>`;
            document.getElementById(`${lang}-back`).innerText = item.m;
        }

        function closeFlashcard(lang) {
            document.getElementById(`${lang}-main-ui`).style.display = 'block';
            document.getElementById(`${lang}-flash-area`).style.display = 'none';
        }

        // Render
        function renderAll() {
            ['en', 'cn'].forEach(lang => {
                const key = `${currentFileName}_${lang.toUpperCase()}`;
                const listId = lang === 'en' ? 'en-list' : 'cn-display-list';
                const searchVal = document.getElementById(`${lang}-search`).value.toLowerCase();
                let data = JSON.parse(localStorage.getItem(key)) || [];

                if(currentFilters[lang] === 'learning') data = data.filter(i => !i.isLearned);
                if(currentFilters[lang] === 'learned') data = data.filter(i => i.isLearned);

                const filtered = data.filter(i => i.w.toLowerCase().includes(searchVal) || i.t.toLowerCase().includes(searchVal)).reverse();

                document.getElementById(listId).innerHTML = filtered.map(i => `
                    <div class="word-item" style="border-left-color:${lang==='en'?'var(--en-color)':'var(--cn-color)'}; opacity: ${i.isLearned ? 0.7 : 1}">
                        <input type="checkbox" class="select-item-check ${lang}-item-cb" value="${i.id}" onchange="updateBulkBar('${lang}')">
                        <div class="action-btns">
                            <button class="mini-flash-btn" onclick="startSingleFlashcard('${lang}', ${i.id})" title="Học từ này">🎴</button>
                            <button class="status-btn" onclick="toggleStatus('${lang}', ${i.id})" title="Đổi trạng thái">
                                ${i.isLearned ? '✅' : '⏳'}
                            </button>
                        </div>
                        <span class="topic-tag">${i.t}</span>
                        ${lang==='en' ? `<b>${i.w}</b>` : `<div class="cn-char">${i.w}</div>`}
                        <div class="ipa-box">${i.extra} <span class="speak-icon" onclick="speak('${i.w}', '${lang}')">🔊</span></div>
                        <div style="margin-top:5px; color:#444; font-weight:500">${i.m}</div>
                    </div>`).join('');
                updateBulkBar(lang);
            });
        }

        function speak(text, lang) {
            synth.cancel();
            const msg = new SpeechSynthesisUtterance(text);
            msg.lang = (lang === 'en') ? 'en-US' : 'zh-CN';
            msg.rate = 0.85; synth.speak(msg);
        }

        function handleSearch(lang) { renderAll(); showSuggestions(lang); }
        function showSuggestions(lang) {
            const data = JSON.parse(localStorage.getItem(`${currentFileName}_${lang.toUpperCase()}`)) || [];
            const input = document.getElementById(`${lang}-search`).value.toLowerCase();
            const suggestBox = document.getElementById(`${lang}-suggestions`);
            const topics = [...new Set(data.map(item => item.t))];
            const filtered = topics.filter(t => t.toLowerCase().includes(input));
            if (filtered.length > 0 && input !== "") {
                suggestBox.innerHTML = filtered.map(t => `<div class="suggestion-item" onclick="applyFilter('${lang}', '${t}')">Topic: ${t}</div>`).join('');
                suggestBox.style.display = 'block';
            } else suggestBox.style.display = 'none';
        }
        function applyFilter(lang, topic) { document.getElementById(`${lang}-search`).value = topic; document.getElementById(`${lang}-suggestions`).style.display = 'none'; renderAll(); }
        function openTab(evt, name) {
            document.querySelectorAll('.container').forEach(c => c.classList.remove('active'));
            document.querySelectorAll('.tab-btn').forEach(t => t.classList.remove('active'));
            document.getElementById(name).classList.add('active');
            evt.currentTarget.classList.add('active');
            closeFlashcard('en'); closeFlashcard('cn');
        }
        function setFilter(lang, type) {
            currentFilters[lang] = type;
            const btns = document.querySelectorAll(`#${lang==='en'?'english':'chinese'} .filter-btn`);
            btns.forEach(b => b.classList.remove('active'));
            event.target.classList.add('active');
            renderAll();
        }
        function updateBulkBar(lang) {
            const checked = document.querySelectorAll(`.${lang}-item-cb:checked`);
            const bar = document.getElementById(`${lang}-bulk-bar`);
            bar.style.display = checked.length > 0 ? 'flex' : 'none';
            if(checked.length > 0) document.getElementById(`${lang}-count`).innerText = `${checked.length} mục chọn`;
        }
        function bulkDelete(lang) {
            if(!confirm("Xóa các từ đã chọn?")) return;
            const key = `${currentFileName}_${lang.toUpperCase()}`;
            let data = JSON.parse(localStorage.getItem(key));
            const ids = Array.from(document.querySelectorAll(`.${lang}-item-cb:checked`)).map(cb => parseInt(cb.value));
            data = data.filter(item => !ids.includes(item.id));
            localStorage.setItem(key, JSON.stringify(data));
            renderAll();
        }

        window.onload = () => { initFiles(); renderAll(); };
    </script>
</body>
</html>
