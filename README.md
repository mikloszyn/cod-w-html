<!DOCTYPE html>
<html lang="pl">
<head>
    <meta charset="UTF-8">
    <title>Edytor HTML Live</title>
    
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
            --shadow: 0 4px 6px rgba(0,0,0,0.1);
        }

        body, html {
            margin: 0; padding: 0; height: 100%;
            font-family: 'Segoe UI', sans-serif;
            background-color: var(--bg-color);
            color: var(--text-color);
        }

        #main-app {
            display: flex;
            flex-direction: column;
            height: 100vh;
            padding: 20px;
            box-sizing: border-box;
            gap: 20px;
        }

        header {
            padding-bottom: 10px;
            border-bottom: 2px solid var(--border-color);
        }

        .workspace {
            display: flex;
            flex-direction: column;
            flex: 1;
            gap: 20px;
        }

        /* --- LIMIT 10 LINII I SUWAK --- */
        #code-area { 
            height: 250px; /* Wysokość dla ok. 10-12 linii */
            border-radius: 10px; 
            overflow: hidden; 
            border: 2px solid #444;
            flex-shrink: 0;
        }

        .CodeMirror { 
            height: 100% !important; 
            font-size: 16px; 
        }

        .CodeMirror-scroll {
            overflow: auto !important;
        }

        /* --- PANEL PODGLĄDU --- */
        .output-panel {
            flex: 1;
            background: white;
            border-radius: 10px;
            border: 2px solid var(--border-color);
            overflow: hidden;
            display: flex;
            flex-direction: column;
            box-shadow: var(--shadow);
        }

        .preview-label {
            background: #eee;
            padding: 5px 15px;
            font-size: 12px;
            font-weight: bold;
            border-bottom: 1px solid var(--border-color);
        }

        #html-preview {
            flex: 1;
            width: 100%;
            border: none;
            background: white;
        }
    </style>
</head>
<body>

<div id="main-app">
    <header>
        <h2 style="margin: 0;">HTML Live Editor (Max 10 linii)</h2>
    </header>

    <div class="workspace">
        <div id="code-area">
            <textarea id="real-editor"><h1>Witaj!</h1>
<p>Zacznij pisać kod HTML, aby zobaczyć podgląd na żywo poniżej.</p></textarea>
        </div>

        <div class="output-panel">
            <div class="preview-label">PODGLĄD NA ŻYWO</div>
            <iframe id="html-preview"></iframe>
        </div>
    </div>
</div>

<script>
    // Inicjalizacja CodeMirror dla HTML
    var editor = CodeMirror.fromTextArea(document.getElementById("real-editor"), {
        lineNumbers: true,
        mode: "htmlmixed",
        theme: "dracula",
        indentUnit: 4,
        autoCloseTags: true,
        autoCloseBrackets: true
    });

    // Funkcja aktualizująca podgląd
    function updatePreview() {
        var previewFrame = document.getElementById('html-preview');
        var previewDoc = previewFrame.contentDocument || previewFrame.contentWindow.document;
        previewDoc.open();
        previewDoc.write(editor.getValue());
        previewDoc.close();
    }

    // Nasłuchiwanie zmian w edytorze (Live Preview)
    editor.on("change", function() {
        updatePreview();
    });

    // Uruchomienie podglądu na starcie
    updatePreview();
</script>

</body>
</html>
