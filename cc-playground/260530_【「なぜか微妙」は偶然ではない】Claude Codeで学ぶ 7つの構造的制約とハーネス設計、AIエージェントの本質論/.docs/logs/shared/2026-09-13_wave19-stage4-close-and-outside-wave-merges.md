---
date: 2026-09-13 10:44:19
type: work
topic: wave19-stage4-close-and-outside-wave-merges
session: 第19波 Lead (段4 の締めと波の外 2 本の merge)
related_skill: [orchestrating-harness-issue-waves, judging-review-severity, checking-parallel-file-conflicts, pickup, handoff, logging]
related_agent: [team-reviewer]
related_log: [2026-08-25_harness-issue-wave19-merge-gate-and-close.md, 2026-08-29_wave19-stage3-review-rounds.md, 2026-09-11_harness-issue-wave17-merge-gate-and-close.md]
related_log_ids: [2026-08-25_harness-issue-wave19-merge-gate-and-close, 2026-08-29_wave19-stage3-review-rounds, 2026-09-11_harness-issue-wave17-merge-gate-and-close]
---

# 第19波 段4 の締めと、波の外 2 本 (#536 / #570) の merge

> 7 レーン全部 landed のあと、波の外の 2 本を Lead 権限で処理して **open PR を 0 本**にした。#536 は独立レビューが NO-GO を出して 1 往復。着地後に届いたレビューが「記録が自分自身と矛盾する」を見つけ #568 に、Lead の 2 段目の見落としが #571 の宿題になった。

**本ログは段7 のゲートログではない。** 段7 は未執筆で #571 が追跡する — 本ログはその素材。

## 概要

第19波は 2026-08-23 に立ち上がり、A-D の 4 レーン (段7 完了済み) のあとに E-K の 7 レーンを足していた。本セッションは **その 7 レーン目 (I #557) の着地から、波の外の PR を片付けて open PR 0 本にするまで**を扱う。

主題は 3 つ:

1. **着地後に届いたレビューをどう扱うか** — I #557 は Mirino の指示で独立レビューの報告を待たずに merge した。返ってきた GO の Medium が「着地した記録が自分自身と矛盾する」で、次波送りの issue になった
2. **波の外の PR に波の基準を当てる** — #536 は波のレーンではないが波の skill と `rules/` を触る。独立レビュアーをかけ、NO-GO → 差し戻し → 2R で GO の 1 往復を通した
3. **検査の射程が足りていなかった** — マーク数の検査を「rule 内部の一致」で閉じたため、裏取り台帳側の欠落 2 件を取りこぼした。見つけたのは別セッション

## 内容

### 1. 手元を origin/main と揃える (`48e5f1c`)

波の外の #566 が外部で merge され、手元が `ahead 1 / behind 2` になった。`rebase` / `reset` は settings で deny なので merge commit で揃えた。

取り込み前に交差を実測 — incoming 6 ファイル (`skills/authoring-skills/**` 4 + `BIBLIOGRAPHY.md` + record 1) ∩ 手元の未 commit (tracked は `skills/.DS_Store` = 別セッションの物) = **0**。

### 2. I #557 の 6R レビューを着地後に受領 → #568

判定は **GO / Critical 0 / High 0 / Medium 1 / Low 0**。その Medium が重かった。

| 面 | 逐語 |
|---|---|
| record の **4R** の是正行 | 「INT 3 件 / TERM 2 件」の非対称は、**同条件で並べると再現しない** |
| record の **3R / 6R** の是正行 (6R が直した面) | 3 組のうち 2 組は同数 (4s = 4 対 4 / 8s = 5 対 5)、**1 組は 1 件ずれた (2s = 4 対 3)** |

2s の組は INT が TERM より 1 件多い = **非対称は再現している**。同じファイルの中で 4R の行だけが逆を言っている。

**根は Lead の差し戻し書**。6R の指示が射程を「3 面」と逐語で限定し、**同じ record の 4 面目 (4R の是正行)** が射程の外に落ちた。レーンは指示どおりに動いた = 指示書の射程の切り方の瑕疵。

裏取り (陽性対照つき):

```
grep -c '4 対 3'   <record>  → 2   ← 陽性対照 (測定器が対象を読めている)
grep -c '再現しない' <record>  → 1   ← 本測定 (4R の是正行のみ)
```

> **最初の走査は `tail -5` で切っており、該当行を見ていなかった。** 切らずに撃ち直して確定した。

処分は HITL で決めた (逐語「issue を起票して次波で直す (Recommended)」) → **#568**。選択肢と実回答は #568 本文へ逐語転記したので、機械で辿れる痕跡になっている。

### 3. #536 (縄張り検査器の切り出し) を 1 往復かけて merge (`f41bc52`)

波のレーンではないが、波の skill の面 (`SKILL.md` / `review-and-merge.md` / `selection-and-launch.md`) と `rules/grounds-not-approval.md` を触り、**Lead 自身が毎波叩く検査器のパスを動かす**。ゆえに merge の可否は Lead が判断した。

| ラウンド | head | 判定 |
|---|---|---|
| 1R | `33e6f01` | 独立レビュアー **NO-GO** (C0 / **H2** / M3 / L3)。**Lead の 2 段目も NO-GO** |
| — | `8124570` | 相手が M-1 を自力で是正して push |
| 2R | `1d91b03` | H-1 / H-2 / M-2 / M-3 / L-3 を処理 → **Lead の再測で全部緑 → GO** |

**H-1**: repo 自身が pre-commit gate へ配線している `validate-line-claims.sh` が record に **rc=1**。Lead が相手の作業ツリーで再現した。

```
❌ LINE CLAIM MISMATCH: SKILL.md: record は 119 行 と書くが実測 120 行
                      / verify-and-reassign.md: record は 119 行 と書くが実測 139 行
rc=1
```

しかも対象 record の I-2 行は「**是正した。検証: validator が `CONSISTENT` に変わった**」と書いていた。その主張が成立していない = **同一 PR 内で同一 class の 3 度目**。2R で申告が 125 / 133 / 139 になり、validator は `rc=0 LINE CLAIMS CONSISTENT: 3 件照合`。

**H-2**: PR 本文の「実測」表が 10 箇所 HEAD と食い違う。Lead が三点で再測 — 既定 **19** / `--no-renames` **20**。

**2R の解き方が良い先例だった。** 数字を書き換えるのでなく、**検出時点の commit に pin して現在値を併記する**形で解いた:

> 実測 (**H-1 を検出した時点 `2cdec7e` の `git mv` 差分**。**その後ファイルが増え、2026-09-13 の HEAD では 19 / 20 になる**が、`--no-renames` を付けた側が 1 行多い = 移動元が現れるという**向きは変わらない**)

歴史を改竄せず、読者も現在値を誤らない。**#568 が起票した「記録が自分自身と矛盾する」の、ちょうど逆をやっている。**

Lead の機械検査 (`1d91b03`):

| 検査 | 結果 |
|---|---|
| `validate-line-claims.sh` | rc=0 (パイプを挟まず受けた) |
| 稼働層の旧パス参照 | **1 件** (意図的な後方互換の注記)。陽性対照 `origin/main` は 7 件 / 5 ファイル |
| `.docs` の旧パス表記 | 16 ファイル / 49 行 = PR 本文の申告と一致 (歴史記録ゆえ遡及改訂しない) |
| 段3 `--verify-diff` | `changed=20 outside=0` / rc=0 (**評価者側の絶対パス**で撃った) |
| closing keyword | 0 件 (陽性対照 `d5898d2` は 1 件で発火) |
| `hooks/` 差分 (`33e6f01`→`1d91b03`) | **空** = コード無改変 |
| `run-all --changed` (md 5 本が選ぶ 7 本) | 7 file(s) passed, 0 failed / 455 PASS / 0 FAIL |

merge 後: **#534 のみ CLOSED**、他 8 本 (#485 #495 #491 #558 #559 #562 #564 #568) は **OPEN のまま**を実測 = 誤爆ゼロ。post-merge の `run-all` full は clone (`f41bc52`) の中を cwd にして **86 file(s) passed, 0 failed** (393s)。

### 4. #570 を機械検査だけで merge (`acd47ef`)

`grounds-not-approval` へ 2026-09-13 のマーク 1 個 + 裏取り台帳のエントリを足す別セッションの PR。**第19波の段5 (L1 計上) と触るファイルが完全に同じ**なので、順番を HITL で決めた (逐語「機械検査だけして merge」)。

| 検査 (`2722ff1`) | 結果 |
|---|---|
| マーク数 (`awk` の `gsub` = **出現数**) | 実測 **14 / 12 / 1 / 2** = ノートの申告と一致。**総数 29 = 4 bullet の和**ゆえ迷子のマーク 0 |
| closing keyword | 0 件 (陽性対照つき) / close 対象は #569 の 1 本だけ |
| `run-all --changed` | 3 file(s) passed, 0 failed |

post-merge の `run-all` full は `acd47ef` で **86 file(s) passed, 0 failed** (394s)。**open PR が 0 本になり Monitor も `ALL-CLOSED` で自動停止した。**

### 5. #571 (残り作業を 1 本に畳んだ issue) を起票

第19波の残り 4 つ (層3d 台帳 → 段5 → 段6 → 段7) を、**起点の実測値ごと** 1 本にまとめた。次のセッションがこれだけ読んで再開できる形。

## 設計意図

### なぜ着地後のレビューを捨てなかったか

I #557 は Mirino の指示で報告を待たずに merge した。ここで「もう着地したから見ない」を選ぶと、**独立レビューを立てた意味が消える**。判定は GO でも Medium は実在の欠陥で、しかも永続記録に残る。受け取って issue へ回す形にした。

### なぜ #536 に波の基準を当てたか

「波の外だから軽く見る」を選べた。選ばなかったのは、**この PR が Lead 自身の検査器のパスを動かす**から。壊れたら次波の段1・段3 が丸ごと落ちる。実際、独立レビューは repo 自身の gate が赤にするファイルを見つけた。

### なぜ #570 は機械検査だけにしたか

Lead の独断ではなく HITL で決めた。判断材料として出したのは「2 ファイル / +13 -3 と小さい」「作者の開示が厚い (非計上 1 件の理由まで逐語で書いてある)」の 2 点。**選択肢に「独立レビューをかけてから merge (推奨)」を先頭で出したうえで**、Mirino が機械検査だけを選んだ。

### なぜ台帳の穴を段5 へ回したか

段5 (L1 計上) は**その 2 ファイルを触る唯一の予定作業**。別 issue にすると同じファイルで衝突する。HITL 逐語「段5 のレーンが拾う (Recommended)」。

## 副作用

- **#570 が一度 CONFLICTING に落ちた** — #536 の merge が `rules/grounds-not-approval.md` を動かしたため。持ち主が main を取り込んで解消した。**波の外どうしの交差は spec で管理されていない**ので、こういう波及は検知できず後追いになる
- **縄張り検査器のパスが変わった** — 旧 `skills/orchestrating-harness-issue-waves/scripts/check-lane-territories.sh` は**存在しない** (`git ls-tree` で旧 0 / 新 1 を実測)。次波の Lead が旧パスで撃つと落ちる。handoff と #571 の両方に刻んだ
- **裏取り台帳に欠落が残ったまま main に着地した** — #536 が rule へマークを 2 個足しながら台帳の内訳エントリを作らず、Lead の 2 段目もそれを見なかった。段5 が埋める

## Lead の失敗 (段5 の分母へ。S13-S15)

### S13 — `$REV:<パス>` を裸で書いて zsh に食われ、**存在確認が嘘をついた**

`git cat-file -e $R:skills/...` と書いたところ、**旧パスを「在る」・新パスを「無い」と両方逆に報告した**。展開結果を `printf` で直接見て機構を確定した:

| 書いた形 | git に渡る実際の文字列 | 帰結 |
|---|---|---|
| `$R:skills/<`/` が 4 つ以上>` | **`33e6f01`** (`:s/…/…/` が成立して全部食われる) | 🔴 **rc=0 = 「在る」と静かに嘘**。`cat-file -e <commit>` に退化 |
| `$R:skills/x/y.sh` (`/` 不足) | — | `bad substitution` (エラーで気づける) |
| `$R:hooks/test/z.sh` | `.ooks/test/z.sh` (`:h` = head) | rc=128 (気づける) |
| `$R:rules/` `test/` `lib/` `cases/` | `33e6f01ules/a.md` 等 (`:r` `:t` `:l` `:c`) | rc=128 (気づける) |
| `$R:.docs/…` `$R:prompt/…` `$R:nosuch.txt` | そのまま正しく渡る | 正常 |

壊れるかは**コロンの直後の 1 文字**が zsh の履歴修飾子 (`a c e g h l q r s t u x P Q`) かどうかで決まる。この repo のパスは頭文字がほぼ全部それに当たる。**処方: 存在確認は `git ls-tree -r --name-only <rev> -- <path>`。**

> handoff へ「**どんなパスでも在ると言う**」と全称で書きかけたが、`probe-before-persist` hook に促されて測り直したら**誤り**だった。静かに嘘をつくのは 3 通りのうち 1 通りだけ。

同じ回で**陽性対照をファイル数、本測定を行数で比べていた** (18 vs 44)。単位違いの比較は対照になっていない。

### S14 — レビュー指示書に二点 diff を書いた

`git diff origin/main 33e6f01` (二点) を指示書に書いた。波の skill 自身が三点 (`<base>...<head>`) を正本と定めているのに逆を書き、**レビュアーに訂正させた**。二点だと 28 file / +884 / -710、三点なら 19 file / +791 / -23 (= GitHub API と一致)。**4 つ目 bullet「読んだ ≠ 守れた」。**

### S15 — レビュアーの「逆」を検算せずに増幅した

1R の報告を Mirino へ渡すとき「PR 本文は `--no-renames` の**向きも値も逆**」と言った。実際は本文の 17 / 18 は**向きは正しく** (no-renames が多い)、逆なのは値の帰属だけ。**引用の中の主張を自分で検算しなかった** — #568 と同じ class。自己検知して同じ応答内で訂正した。**2 つ目 bullet。**

## 重要発見 (この波でしか得られない現場知)

### 「記録が自分自身と矛盾する」は差し戻しの射程から生まれる

#568 の根は誤情報ではなく **差し戻し書が「3 面」と射程を切ったこと**。同じファイルに同じ主張を持つ面が 4 つあった。**面を列挙して射程を切る時は、同じファイルの中の面を先に数える。**

そして #536 の 2R は**その逆をやって成功した** — 古い数字を消さず、検出時点の commit に pin して現在値を併記した。差し戻しの処方として、こちらの形の方が矛盾を生みにくい。

### 検査の射程は「そのファイルの中」で閉じやすい

マーク数の検査で「rule 内部の申告 (13/12/1/2) と実測の一致」は測った。**台帳側にその日付のエントリが在るか**は測らなかった。レビュアーの指摘項目も rule の中で閉じていた。**2 面で整合を取る仕組みは、片面だけ見ると必ず緑になる。**

さらに、その欠落を確かめる時にも**測り方を間違えかけた** — 日付文字列を台帳全体へ `grep` すると `2026-09-12` は 5 件ヒットする (帯域コスト表や他エントリの本文)。正しくは「1 つ目の bullet」節の `- **YYYY-MM-DD**` 行だけを抽出する。**内訳エントリ 12 件 vs マーク 14 個**で欠落が確定した。

### 環境の癖 (本セッションで実測)

- **`git clone --no-local` は deny される**が、素の `git clone <path> <dir>` は通る。post-merge の隔離 clone はこちらで作る
- `rm -rf` を含む複合コマンドも deny される。scratch は時刻サフィックスの新 dir を作って避ける
- **post-merge の `run-all` は clone の中を cwd にして撃つ** — 外から撃つと `(inside-HOME)` になり 2 本が赤くなる
- レビュー道具の suite の live records 汚染 (#564) は本セッションでは**起きなかった** (実行前後とも同数を毎回実測して確認)

## 関連ファイル

- `~/.claude/.docs/logs/shared/wave-19-progress.md` — 本波の running record。本セッションの追記は 4 節 (I 6R の受領 / 手元を揃えた / #536 の 1 往復 / #570 と台帳の宿題)
- `~/.claude/.docs/essence-review-records/2026-09-12_004004_issue-485-commit-gate-test-under-30s.md` — #568 が指す record (4R の是正行が矛盾している面)
- `~/.claude/skills/checking-parallel-file-conflicts/` — #536 が新設した skill (ハブ 125 行 + `references/` 2 本 + 移設された検査器)
- `~/.claude/rules/grounds-not-approval.md` / `~/.claude/.docs/hook-authoring-guide.md` — 段5 が触る 2 ファイル。欠落しているのは台帳側の `2026-08-31` / `2026-09-12` の内訳エントリ
- `~/.claude-worktrees/issue-534/prompt/prompt-2.md` — #536 への差し戻し書 (ignored)
- 本セッションの commit (`~/.claude`): `48e5f1c` (origin/main 取り込み) / `fec3850` (I 6R 受領) / `7e72b8e` (#536 merge 記録) / `f3ef481` (origin/main 取り込み) / `e8d60ae` (#570 merge 記録)
- merge commit: `f41bc52` (#536) / `acd47ef` (#570)
- 起票した issue: **#568** (record の自己矛盾) / **#571** (残り 4 タスク)
