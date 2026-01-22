<!DOCTYPE html>
<html lang="pl">
<head>
    <meta charset="UTF-8">
    <title>HTML Live Editor - Pliki</title>
    
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/codemirror/5.65.13/codemirror.min.css">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/codemirror/5.65.13/theme/dracula.min.css">
    <script src="https://cdnjs.cloudflare.com/ajax/libs/codemirror/5.65.13/codemirror.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/codemirror/5.65.13/mode/xml/xml.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/codemirror/5.65.13/mode/javascript/javascript.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/codemirror/5.65.13/mode/css/css.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/codemirror/5.65.13/mode/htmlmixed/htmlmixed.min.js"></script>

    <style>
        :root {
            --bg-color: #f8f9fa;
            --text-color: #212529;
            --panel-color: #ffffff;
            --border-color: #dee2e6;
            --accent-color: #007bff;
            --success-color: #28a745;
        }

        body, html {
            margin: 0; padding: 0; height: 100%;
            font-family: 'Segoe UI', sans-serif;
            background-color: var(--bg-color);
        }

        #main-app {
            display: flex; flex-direction: column;
            height: 100vh; padding: 20px; box-sizing: border-box; gap: 15px;
        }

        .editor-toolbar {
            display: flex; gap: 10px; padding-bottom: 10px;
            border-bottom: 2px solid var(--border-color);
        }

        .workspace { display: flex; flex-direction: column; flex: 1; gap: 15px; }

        /* LIMIT 10 LINII I SUWAK */
        #code-area { 
            height: 250px; border-radius: 10px; 
            overflow: hidden; border: 2px solid #444; flex-shrink: 0;
        }

        .CodeMirror { height: 100% !important; font-size: 16px; }
        .CodeMirror-scroll { overflow: auto !important; }

        .output-panel {
            flex: 1; background: white; border-radius: 10px;
            border: 2px solid var(--border-color); overflow: hidden;
            display: flex; flex-direction: column;
        }

        #html-preview { flex: 1; width: 100%; border: none; }

        .btn {
            padding: 8px 15px; border: none; border-radius: 5px;
            cursor: pointer; color: white; font-weight: bold;
        }
        .btn-save { background: var(--success-color); }
        .btn-load { background: var(--accent-color); }
        
        /* Ukryty input dla plików */
        #file-input { display: none; }
    </style>
</head>
<body>

<div id="main-app">
    <div class="editor-toolbar">
        <button class="btn btn-save" onclick="saveToFile()">Zapisz plik .html</button>
        <button class="btn btn-load" onclick="document.getElementById('file-input').click()">Wczytaj plik</button>
        <input type="file" id="file-input" accept=".html,.txt,.cnt" onchange="loadFile(this)">
    </div>

    <div class="workspace">
        <div id="code-area">
            <textarea id="real-editor"><h1>Moja Strona</h1>
<p>Edytuj ten kod i zapisz go na dysku!</p></textarea>
        </div>

        <div class="output-panel">
            <iframe id="html-preview"></iframe>
        </div>
    </div>
</div>

<script>
    var editor = CodeMirror.fromTextArea(document.getElementById("real-editor"), {
        lineNumbers: true,
        mode: "htmlmixed",
        theme: "dracula",
        autoCloseTags: true,
        autoCloseBrackets: true
    });

    function updatePreview() {
        var previewFrame = document.getElementById('html-preview');
        var previewDoc = previewFrame.contentDocument || previewFrame.contentWindow.document;
        previewDoc.open();
        previewDoc.write(editor.getValue());
        previewDoc.close();
    }

    editor.on("change", updatePreview);
    updatePreview();

    // --- FUNKCJA ZAPISYWANIA ---
    function saveToFile() {
        const code = editor.getValue();
        const blob = new Blob([code], { type: "text/html" });
        const a = document.createElement("a");
        a.href = URL.createObjectURL(blob);
        a.download = "moj-projekt.html"; // Nazwa pliku
        a.click();
    }

    // --- FUNKCJA WCZYTYWANIA ---
    function loadFile(input) {
        const file = input.files[0];
        if (!file) return;

        const reader = new FileReader();
        reader.onload = function(e) {
            const content = e.target.result;
            editor.setValue(content); // Wstawia treść pliku do edytora
            input.value = ''; // Resetuje input, by móc wczytać ten sam plik ponownie
        };
        reader.readAsText(file);
    }
</script>

</body>
</html>
