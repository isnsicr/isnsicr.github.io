# Chess Evaluation Explorer

iPhone / GitHub Pages 向けのオフライン対応チェス解析UIです。

- Stockfish.js 19 lite single-thread をブラウザ内で実行
- MultiPV候補手を表示
- Search評価と説明用Classical評価を白視点に統一
- `Δ = Search - Classical`
- FEN入力、盤面反転、候補手の移動元・移動先表示
- Service WorkerでUIをオフライン化
- 「Stockfishをオフライン保存」を一度実行するとエンジン本体もCache Storageに保存

Safariで `/chess/` を開き、まず「Stockfishをオフライン保存」を実行してください。その後は「共有 → ホーム画面に追加」でアプリ化できます。

Classicalは説明可能性のための独自ヒューリスティックで、Stockfish旧版HCEの完全再現ではありません。

Stockfish.js 19.0.0 lite single-thread: https://github.com/nmrugg/stockfish.js (GPLv3)
