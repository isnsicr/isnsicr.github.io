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
- UI内の着手履歴をStockfish Searchへ渡し、三fold repetitionを探索に反映
- 50手/75手、三fold/五fold、デッドポジション、ステイルメイトを別枠表示
- HCE補間後寄与を正負の横棒グラフで表示
- Coverage表示: 両方 / 白のみ / 黒のみ / OFF を切替。各マスへの擬似攻撃枚数を可視化（ピン等は無視）
- Coverageを見やすくするため盤面のベース色を淡色化
- Coverageでは絶対ピンはピン線上のみ有効。相対ピンはピン線上に加え、背後で守る味方駒より高価な敵駒への攻撃coverageも有効
- coverageの合法性は、coverage先の駒が取られた後に取り返す局面で判定。自玉を危険にする取り返しはcoverageに数えない
- 敵駒が斜線上で手前から強→弱と並ぶ場合は奥の弱い駒までpressureを継続。味方駒は原則として斜線を遮断し、同一直線のスライダーによるバッテリーだけ貫通して重複加算
- アンパッサン可能時は、斜め前の到達マスに加えて実際に取られる横隣の敵ポーンのマスもcoverageに加算
- ダークモードを無効化し、常にライト配色を使用
- 着手確定後にStockfish解析を自動実行
- 候補手がない場合はチェックメイト、ステイルメイト、ドロー等の理由を表示
- Stockfish 19の現在局面評価を白/黒の横棒グラフで表示
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

- 相対ピンでは、背後の保護駒より高価な敵駒へのcoverageに加え、敵の後続駒が敵側から合法にcoverageされている交換列でもcoverageを維持

- ナイトは1手先に加え、合法な1手目を経由して2手先までcoverageに含める。同一ナイトが同じマスへ複数経路で届いても+1まで

- アンパッサンが規則上合法なら、通常の斜め前だけでなく実際に取り除かれる横隣の敵ポーンのマスもcoverageに含める
