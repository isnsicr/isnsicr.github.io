# Chess Evaluation Explorer

iPhone / GitHub Pages 向けのオフライン対応チェス解析UIです。

- Search: Stockfish.js 19 lite single-thread
- Classical: Stockfish.js 16 single-thread の公式Handcrafted Evaluation (HCE)
- Stockfish 16を `Use NNUE=false` にして `eval` traceを取得
- HCEの Material / Imbalance / Pawns / Knights / Bishops / Rooks / Queens / Mobility / King safety / Threats / Passed / Space / Winnable をMG/EG別に表示
- `Δ = Search - Classical`（白視点）
- MultiPV候補手
- FEN入力、盤面反転
- 「1手戻る」「初期局面」で着手履歴を戻す
- 白駒・黒駒を明示的な色で表示
- タップで「駒 → 移動先」を選ぶ操作（iPhone向け）
- 合法手判定はStockfish 16で実施
- Service WorkerでUIをオフライン化
- 「エンジンをオフライン保存」を一度実行するとStockfish 19とStockfish 16もCache Storageに保存

Safariで `/chess/` を開き、オンライン時にまず「エンジンをオフライン保存」を実行してください。その後は「共有 → ホーム画面に追加」でアプリ化できます。

## 評価について

SearchはStockfish 19の探索評価です。Classicalは候補手を指した直後の局面をStockfish 16の公式HCEで静的評価した値です。

Stockfish 16のHCE traceはチェック中の局面を評価しないため、候補手によって相手キングがチェックされている場合、その候補手のClassicalとΔは表示しません。

Stockfish 16と19ではcentipawn評価の校正が完全に同一ではないため、Δは「同一評価関数内の厳密な分解」ではなく、現行探索評価と旧HCEの差を見る指標として扱ってください。

## License / upstream

- Stockfish.js 19: https://github.com/nmrugg/stockfish.js
- Stockfish.js 16 / npm package 16.0.0: https://www.npmjs.com/package/stockfish/v/16.0.0
- Stockfish: GPLv3
