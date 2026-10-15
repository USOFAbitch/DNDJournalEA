# DNDJournalEA
dnd journal EA
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>The Adventurer's Chronicle</title>
    <style>
        :root {
            --bg-dark: #120c06;
            --leather-dark: #2c1a0e;
            --leather-light: #4a2f1b;
            --gold: #c5a059;
            --gold-dim: #8c6f39;
            --parchment: #f2e3c6;
            --ink: #2b2214;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }

        body {
            background-color: var(--bg-dark);
            background-image: radial-gradient(circle, #22150b 0%, #0a0603 100%);
            font-family: 'Georgia', serif;
            color: var(--parchment);
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            padding: 20px;
            overflow-x: hidden;
        }

        /* Book Container */
        .book-wrapper {
            position: relative;
            width: 100%;
            max-width: 950px;
            perspective: 1500px;
        }

        .book-cover {
            background: linear-gradient(135deg, var(--leather-dark) 0%, var(--leather-light) 50%, var(--leather-dark) 100%);
            padding: 18px;
            border-radius: 12px;
            box-shadow: 0 25px 50px rgba(0, 0, 0, 0.8), inset 0 0 15px rgba(0,0,0,0.6);
            border: 3px solid var(--gold-dim);
            position: relative;
        }

        /* Decorative Corners */
        .book-cover::before, .book-cover::after {
            content: '';
            position: absolute;
            width: 30px;
            height: 30px;
            border: 2px solid var(--gold);
            border-radius: 4px;
            pointer-events: none;
        }

        .book-cover::before {
            top: 10px;
            left: 10px;
            border-right: none;
            border-bottom: none;
        }

        .book-cover::after {
            bottom: 10px;
            right: 10px;
            border-left: none;
            border-top: none;
        }

        .book-spine {
            position: absolute;
            left: 50%;
            top: 18px;
            bottom: 18px;
            width: 4px;
            background: rgba(0, 0, 0, 0.4);
            box-shadow: 0 0 10px rgba(0,0,0,0.8);
            transform: translateX(-50%);
            z-index: 10;
        }

        /* Inner Pages Layout */
        .book-pages {
            display: grid;
            grid-template-columns: 1fr 1fr;
            background-color: var(--parchment);
            color: var(--ink);
            border-radius: 4px;
            min-height: 650px;
            box-shadow: inset 0 0 40px rgba(43, 34, 20, 0.3);
            position: relative;
            background-image: radial-gradient(rgba(0,0,0,0.03) 1px, transparent 0);
            background-size: 24px 24px;
        }

        .page {
            padding: 40px 45px;
            display: flex;
            flex-direction: column;
            position: relative;
        }

        .page-left {
            border-right: 1px solid rgba(0, 0, 0, 0.15);
            box-shadow: inset -15px 0 20px rgba(0, 0, 0, 0.05);
        }

        .page-right {
            box-shadow: inset 15px 0 20px rgba(0, 0, 0, 0.05);
        }

        /* Typography & Header Elements */
        h1 {
            font-family: 'Times New Roman', Times, serif;
            font-size: 1.8rem;
            text-align: center;
            text-transform: uppercase;
            letter-spacing: 2px;
            color: #4a1505;
            margin-bottom: 5px;
            border-bottom: 1px double #4a1505;
            padding-bottom: 8px;
        }

        .sub-header {
            text-align: center;
            font-size: 0.85rem;
            font-style: italic;
            opacity: 0.8;
            margin-bottom: 25px;
        }

        .field-group {
            margin-bottom: 20px;
        }

        label {
            display: block;
            font-size: 0.85rem;
            font-weight: bold;
            text-transform: uppercase;
            letter-spacing: 1px;
            margin-bottom: 5px;
            color: #5c3a21;
        }

        /* Interactive Inputs */
        input[type="text"] {
            width: 100%;
            background: transparent;
            border: none;
            border-bottom: 1px dashed rgba(43, 34, 20, 0.5);
            font-family: 'Georgia', serif;
            font-size: 1.1rem;
            color: var(--ink);
            padding: 5px 0;
            outline: none;
            transition: border-color 0.3s;
        }

        input[type="text"]:focus {
            border-bottom-color: #4a1505;
        }

        /* Notebook Lining for Textarea */
        .textarea-container {
            flex-grow: 1;
            position: relative;
            margin-top: 10px;
        }

        textarea {
            width: 100%;
            height: 100%;
            min-height: 400px;
            background: transparent;
            border: none;
            resize: none;
            font-family: 'Georgia', serif;
            font-size: 1.1rem;
            line-height: 28px;
            color: var(--ink);
            outline: none;
            padding-top: 4px;
            background-image: linear-gradient(transparent 27px, rgba(43, 34, 20, 0.15) 27px);
            background-size: 100% 28px;
        }

        /* Footer / Controls */
        .page-footer {
            margin-top: auto;
            display: flex;
            justify-content: space-between;
            align-items: center;
            gap: 10px;
            padding-top: 15px;
            font-size: 0.8rem;
            border-top: 1px solid rgba(43, 34, 20, 0.1);
        }

        .footer-controls {
            display: flex;
            flex-wrap: wrap;
            justify-content: flex-end;
            gap: 6px;
        }

        .status-indicator {
            font-style: italic;
            color: #665233;
            display: flex;
            align-items: center;
            gap: 5px;
        }

        .status-dot {
            width: 6px;
            height: 6px;
            border-radius: 50%;
            background-color: #c5a059;
        }

        .status-saved .status-dot {
            background-color: #2e6f40;
        }

        .clear-btn {
            background: transparent;
            border: 1px solid rgba(74, 21, 5, 0.4);
            color: #4a1505;
            padding: 4px 10px;
            border-radius: 4px;
            font-family: 'Georgia', serif;
            font-size: 0.75rem;
            cursor: pointer;
            text-transform: uppercase;
            letter-spacing: 0.5px;
            transition: all 0.2s ease;
        }

        .clear-btn:hover {
            background-color: #4a1505;
            color: var(--parchment);
        }

        /* Responsive Breakpoint for Small Screens (Collapses to single page view) */
        @media (max-width: 768px) {
            .book-pages {
                grid-template-columns: 1fr;
            }
            .book-spine {
                display: none;
            }
            .page-left {
                border-right: none;
                border-bottom: 2px dashed rgba(43, 34, 20, 0.2);
                padding-bottom: 40px;
            }
        }
    </style>
</head>
<body>
    <div class="book-wrapper">
        <div class="book-cover">
            <div class="book-spine"></div>
            <div class="book-pages">
                <!-- Left Page: Campaign Metadata -->
                <div class="page page-left">
                    <h1>Campaign Log</h1>
                    <div class="sub-header">Chronicles of the Realm</div>
                    
                    <div class="field-group">
                        <label for="campaign-name">Campaign / Realm</label>
                        <input type="text" id="campaign-name" placeholder="e.g., Curse of Strahd">
                    </div>
                    
                    <div class="field-group">
                        <label for="current-location">Current Location</label>
                        <input type="text" id="current-location" placeholder="e.g., Vallaki Inn">
                    </div>
                    
                    <div class="field-group">
                        <label for="active-quest">Active Quest / Objective</label>
                        <input type="text" id="active-quest" placeholder="e.g., Find the Holy Symbol of Ravenkind">
                    </div>
                    
                    <div class="field-group">
                        <label for="party-status">Party Condition & Status</label>
                        <input type="text" id="party-status" placeholder="e.g., Level 5. Wizard down 10 HP.">
                    </div>
                </div>

                <!-- Right Page: The Note Pad -->
                <div class="page page-right">
                    <div class="field-group" style="margin-bottom: 10px;">
                        <label for="session-title">Session Header & Date</label>
                        <input type="text" id="session-title" placeholder="Session 14: The Ambush" style="font-weight: bold; font-size: 1.2rem;">
                    </div>
                    
                    <div class="textarea-container">
                        <textarea id="campaign-notes" placeholder="Write your session chronicles, NPC names, and dynamic loot updates here..."></textarea>
                    </div>
                    
                    <div class="page-footer">
                        <div id="save-status" class="status-indicator">
                            <span class="status-dot"></span>
                            <span id="status-text">Ready</span>
                        </div>
                        <div class="footer-controls">
                            <button class="clear-btn" id="backup-btn" type="button">Save Backup</button>
                            <button class="clear-btn" id="restore-btn" type="button">Restore</button>
                            <button class="clear-btn" id="clear-btn" type="button" title="Wipe current book entries">Purge Book</button>
                        </div>
                        <input id="restore-file" type="file" accept="application/json,.json" hidden>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <script>
        const storageKey = 'adventurers-chronicle';
        const fieldIds = [
            'campaign-name',
            'current-location',
            'active-quest',
            'party-status',
            'session-title',
            'campaign-notes'
        ];
        const fields = fieldIds.map((id) => document.getElementById(id));
        const saveStatus = document.getElementById('save-status');
        const statusText = document.getElementById('status-text');

        function showStatus(message, isSaved = false) {
            statusText.textContent = message;
            saveStatus.classList.toggle('status-saved', isSaved);
        }

        function getJournal() {
            return Object.fromEntries(fields.map((field) => [field.id, field.value]));
        }

        function saveJournal() {
            try {
                localStorage.setItem(storageKey, JSON.stringify(getJournal()));
                showStatus('Saved locally', true);
            } catch (error) {
                showStatus('Save failed; use Save Backup');
            }
        }

        try {
            const savedJournal = JSON.parse(localStorage.getItem(storageKey) || '{}');
            fields.forEach((field) => {
                if (typeof savedJournal[field.id] === 'string') {
                    field.value = savedJournal[field.id];
                }
            });
            if (Object.keys(savedJournal).length > 0) {
                showStatus('Saved locally', true);
            }
        } catch (error) {
            showStatus('Could not load saved notes');
        }

        fields.forEach((field) => field.addEventListener('input', saveJournal));

        document.getElementById('backup-btn').addEventListener('click', () => {
            const backup = new Blob([JSON.stringify(getJournal(), null, 2)], {
                type: 'application/json'
            });
            const downloadUrl = URL.createObjectURL(backup);
            const downloadLink = document.createElement('a');
            downloadLink.href = downloadUrl;
            downloadLink.download = 'adventurers-chronicle.json';
            downloadLink.click();
            URL.revokeObjectURL(downloadUrl);
            showStatus('Backup downloaded', true);
        });

        const restoreFile = document.getElementById('restore-file');
        document.getElementById('restore-btn').addEventListener('click', () => {
            restoreFile.click();
        });

        restoreFile.addEventListener('change', async () => {
            const file = restoreFile.files[0];
            restoreFile.value = '';
            if (!file) {
                return;
            }

            try {
                const savedJournal = JSON.parse(await file.text());
                if (!savedJournal || typeof savedJournal !== 'object' || Array.isArray(savedJournal)) {
                    throw new Error('Invalid backup format');
                }
                fields.forEach((field) => {
                    field.value = typeof savedJournal[field.id] === 'string'
                        ? savedJournal[field.id]
                        : '';
                });
                saveJournal();
            } catch (error) {
                showStatus('Could not restore backup');
            }
        });

        document.getElementById('clear-btn').addEventListener('click', () => {
            if (!window.confirm('Purge all campaign and session notes from this browser?')) {
                return;
            }
            fields.forEach((field) => {
                field.value = '';
            });
            try {
                localStorage.removeItem(storageKey);
                showStatus('Book cleared');
            } catch (error) {
                showStatus('Could not clear saved notes');
            }
        });
    </script>
</body>
</html>
