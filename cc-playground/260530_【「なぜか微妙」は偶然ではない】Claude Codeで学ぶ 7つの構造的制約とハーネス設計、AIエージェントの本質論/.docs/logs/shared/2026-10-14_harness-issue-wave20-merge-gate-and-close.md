---
type: gate-log
date: 2026-10-14
scope: ハーネス issue 第20波 (段1 選抜 2026-10-08 〜 段5 の着地 2026-10-14)。初版 (commit 1346d18) は段5 の着地前に書き、段5 の merge 後に最終値へ更新した
base: 3a473ad (選抜時の origin/main)
head: 8dc4f43 (段5 = PR #623 の merge 後の origin/main。初版は 0037988)
---

# 第20波 マージゲートとクローズ (2026-10-14)

ハーネス repo `mirinodev/claude-harness` の第20波。Lead は本体 `~/.claude` のセッション。**詳細の正本はハーネス repo の `.docs/logs/shared/wave-20-progress.md`** (段ごとの実測・判定・HITL の逐語)。本ログはその要約と、段7 で初めて測った「申し送りの反映監査」を持つ。

## 1. 結果 (確定値)

| 項目 | 値 | 測った日 |
|---|---|---|
| レーン | 12 (A〜L) + 回収レーン M (`issue-523-558-560-collect`) + 段5 レーン N (`issue-610-wave20-l1`) | 2026-10-14 |
| merge した PR | 14 本: #595 #596 #597 #598 #600 #601 #602 #603 #604 #605 #606 #607 #620 #623 (`gh pr view --json state` で全部 MERGED)。波の外で #609 (#608 = キット更新 hook) も merge | 2026-10-14 |
| main | `3a473ad` → `8dc4f43`、`git rev-list --count` で 157 commit (merge commit と進捗表の commit を含む) | 2026-10-14 22:5x |
| close (段5 で同時に) | #610 (波の外の issue。同じ rule へのマークなのでレーン N が一緒に計上し、PR #623 の merge で閉じた) | 2026-10-14 |
| close (選抜時) | 5 件: #479 #399 #342 #441 #538 (理由は各 issue のコメント) | 2026-10-08 |
| close (レーンの 26 件のうち) | 21 件: PR の `Closes` で 19 件 (#508 #514 #523 #530 #541 #545 #549 #550 #551 #553 #558 #559 #560 #561 #562 #564 #567 #568 #576) + Lead が段4 で閉じた 2 件 (#491 #537) | 2026-10-14 |
| open のまま (レーンの 26 件のうち) | 5 件: #528 (残り 4 点、一部は #617) / #511 (案1・案3・案2 の本体) / #571 (タスク 1・3・4) / #574 (#599 待ち) / #546 (issue §3 の据置 2 件)。理由は各 issue の 2026-10-14 の Lead コメント | 2026-10-14 |
| 波の中で起票 | 9 件: #593 (L) / #594 (J) / #599 (H) / #615 (B) / #616 (G) / #617 (A) / #618 (D) / #619 (J) / #621 (Lead) | 2026-10-14 |
| 同じ期間に波の外で起票 | #608 #610 #612 #613 #622 | 2026-10-14 |
| open issue | 着手時 35 → 22 (`gh issue list --state open`。22 件には波の外の #622 を含む) | 2026-10-14 22:5x |
| post-merge `run-all` | `8dc4f43` (14 本の merge 後) を clone で full run: **95 file(s) passed, 0 failed**、rc=0、676s。その前に `52a5fcf` (13 本の後) でも 95/0、merge 前に GO の鎖を 2 回 (6 本の鎖 94/0、8 本の鎖 95/0) | 2026-10-14 23:08 |
| 段6 | worktree 14 本を `gtr rm` (殻 dir 0)、Keychain item 14 件を回収 (台帳へ `reclaimed` 14 件)。権威スキャン: 孤児 0 / 帰属不能 0 / 判定不能 1 (`~/.claude-verify-111` = 既知の置き場の外) | 2026-10-14 |

## 2. レーン配分と diff 規模 (`gh pr view --json additions,deletions,changedFiles`)

| PR | branch | 追加 / 削除 | ファイル | merge (UTC) | 閉じた issue |
|---|---|---|---|---|---|
| #595 | `issue-567` | +343 / -34 | 15 | 10-08 12:47 | #567 |
| #596 | `issue-576-530` | +536 / -38 | 8 | 10-08 23:10 | #530 #576 |
| #597 | `issue-562-553` | +1260 / -14 | 10 | 10-08 23:04 | #553 #562 |
| #598 | `issue-537` | +865 / -0 | 4 | 10-14 07:27 | (段4 で #537 を手で) |
| #600 | `issue-508-528-564` | +712 / -39 | 5 | 10-14 09:18 | #508 #564 |
| #601 | `issue-514-511` | +1244 / -37 | 8 | 10-14 09:17 | #514 |
| #602 | `issue-559` | +639 / -27 | 6 | 10-14 09:18 | #559 |
| #603 | `issue-523-558-560` | +1273 / -136 | 11 | 10-14 09:25 | (#620 で) |
| #604 | `issue-571-550-551-568` | +567 / -26 | 8 | 10-14 09:18 | #550 #551 #568 |
| #605 | `issue-546-541` | +1918 / -541 | 36 | 10-14 09:25 | #541 |
| #606 | `issue-549-491-561` | +838 / -10 | 6 | 10-14 09:18 | #549 #561 (#491 は段4 で手で) |
| #607 | `issue-574-545` | +774 / -65 | 4 | 10-14 09:18 | #545 |
| #620 | `issue-523-558-560-collect` | +242 / -17 | 5 | 10-14 11:29 | #523 #558 #560 |
| #623 | `issue-610-wave20-l1` | +99 / -8 | 2 | 10-14 13:56 | #610 |

レビューの巡回 (進捗表の各節の見出しから): 先に立った 4 本は #595 #596 #597 が 2R で GO、#598 が本文だけの 3R で GO (10-08〜10-09)。残る 8 本 (#600-#607) は 1R で 8 本とも差し戻し、2R で GO 6 本 / 3R 2 本 (#603 #605)、3R で 2 本とも GO (10-14)。回収レーン #620 は 1R で GO。段5 の #623 は 1R・2R とも GO だったが、文面の誤り (台帳とノート / merge commit に残る PR タイトル) を 2R・3R で直させてから merge (3R は Lead が差分で確認)。10-08 12:48 に利用上限で 10 レーンが止まり (解除 15:50)、8 本の 2R の投入は 10-14 まで空いた。

## 3. 本波の主題 — 「反映先で測らずに『済んだ』と扱う」

本波で支配的だった失敗は、**書いたもの・渡したものが宛先に届いたかを、宛先で測らずに済んだことにする**形。

- **2 巡目の検証役 8 体のうち 6 体の NO-GO / High が、PR 本文の更新より前に本文を取った測定だった** (#600 #601 #603 #604 #606 #607)。レーンは「push した」の後に本文を反映する作りで、Lead は push を見た時点で検証役を起動していた。Lead が live の本文で測り直して 6 本とも解消した。3R からは「本文の反映を確かめてから起動」に変えた
- **申し送り 69 件のうち 43 件が、どこにも反映されないまま merge された** (段7 の監査。下の 7)。中でも E (#603) → F (#606) の 4 件は #523 #558 #560 を閉じる条件そのもので、宛先の F は本文に「受け取る申し送り (本 PR には入れていない)」と書いて止まっていた。段4 で気づき、回収レーン M (#620) を立てて閉じた
- 隣の形: #605 は 2R・3R と、是正のたびに同じ判定関数へ新しい退行が入った (直した入力だけで確かめ、前の巡回で正しかった入力を当て直していない)。3R の検証役に「前の巡回で正しかった入力を当て直す」を重く見させて、残りを #619 へ送った

## 4. レーンが装置側で塞いだもの / 運用者 (Lead) が踏んだもの

### 4-1. レーンが塞いだもの (PR ごとに 1 行)

| PR | 塞いだもの |
|---|---|
| #595 (K) | authoring-skills と orchestrator SKILL.md の直列 / 並列の記述 (#567) |
| #596 (I) | adding-plugin-to-marketplace / authoring-claude-md の修正 (#576 #530) |
| #597 (C) | commit 本文の closing keyword + `#番号` を止める grounds gate (#562)、stop-words の「はず」単体の取りこぼし (#553) |
| #598 (L) | committer を repo へ取り込み、Test Gate が macOS で走らない件を修正 (#537) |
| #600 (A) | essence gate の verdict 抽出を Observability 節へ絞る (#528 の gate 側)、suite が live の records dir を汚す件 (#564)、前提の記述 (#508) |
| #601 (B) | `pr_body_claims_check --pr` の取得失敗を rc=2 で返す (#514)、PR body の hash 無し `path:行番号` を形で禁じる rule C (#511 の一部) |
| #602 (D) | Bash guard 2 枚が結合フラグ (`-Ei` / `-lpi`) と `>&`・zsh の `>!` 系を書込と見ない件 (#559) |
| #603 (E) | 検査器が全モードの 1 行目で `CHECKER blob=` を名乗る (#523)、検査器コメントと SKILL.md 機械層の古い記述 + 層6 の字句ガード (#558)、検査器と `NEW:` 記法の後始末 (#560) |
| #604 (G) | 第19波ぶんの L1 計上 + 台帳と rule のマーク突合の契約テスト (#571 タスク 2)、command-handoff の手順の移設 (#550)、harness-policy の must (#551)、record の自己矛盾 (#568) |
| #605 (J) | coding-responsive-layouts の後片付け (#546 の一部)、crafting-frontend-interfaces の新設と CSS gate (#541) |
| #606 (F) | 2-1 の ignored 判定を `check-ignore -q` へ (#549)、2-5 の射程差表の書込先の開示 (#491)、申し送りの回収側 3-3c と 2-5 抽出の run-all 配線 (#561) |
| #607 (H) | `gh pr create` / `gh repo create` の ask 規則 (#574。#599 が直るまで効かない)、`/effort` の settings.json の churn を正規化 (#545) |
| #620 (M) | 3-3b の fence を fetch 後の base の先端から測る形へ + `CHECKER` 行の転記 + `NEW-UNUSED` の扱い + 言い回しの統一 (#523 #558 #560 の閉じる条件)、SKILL.md 機械層の開示 (#561)、#546 のテスト床 34 → 47 |
| #623 (N) | 段5 の L1 計上: 分母 61 件 (Lead 18 + レーン 9 本の 37 + 波の外 #610 の 6) を `rules/grounds-not-approval.md` の再発マーク 31 個 (20/16/1/3 → 32/25/1/13) と台帳のエントリへ (#610 を含む) |

### 4-2. 運用者 (Lead) が踏んだもの — W20-1〜W20-9

三値は 5-6 のとおり (PR #623 本文「Lead の三値」の値。初版の時点では 0 件計上だった):

| 値 | 件数 | 中身 |
|---|---|---|
| 踏んだ | 21 | 一次記録の 17 件 (W20-1〜W20-9 を 1 回ずつ数えた値 = 1+1+1+1+2+1+1+2+7。W20-5 は 2 回、W20-8 は一次記録の 2 回、W20-9 は 7 か所) + レーン N が数えた W20-8-3' (3 回目) の 1 件 + 分母の外の 3 件 (X1〜X3) |
| 本波で計上した | 17 | W20-7 を除く 17 件 (W20-7 は「値・測定器・理由・読んだ警告のどれでもない」として非計上) |
| 跨いだ | 3 | X1 = 10-14 18:31 に heredoc で書いて 1 枚目の guard に止められた / X2 = 同 18:33、回収レーンの起動の 1 行を `!` で打つ形で渡した / X3 = 同 16:2x、2 巡目の検証役 8 体を本文の更新を確かめる前に起動した。レーン N が裏取りの途中で見つけた (Lead の transcript の全数監査はしていない)。計上するかは次の波の段5 で Lead が決める |

| # | 形 | 当たる bullet (初版の Lead の見立て。最終の分類は PR #623) | 踏んだ回数 |
|---|---|---|---|
| W20-1 | `wc -l` の `total` 行を 1 本の行数と読んで handoff へ確定形で書いた (948 → 実測 474) | 1 つ目 | 1 |
| W20-2 | パイプの後ろで `$?` を読んだ (`grep` の rc) | 2 つ目 寄り | 1 |
| W20-3 | 自分で指示書へ運んだ #440 guard の警告 (変数参照で例外が外れる) を、直後に同じ形で踏んだ | 4 つ目 | 1 |
| W20-4 | 利用上限の停止時刻を transcript の mtime から確定形で書いた (13:13 → 実測 12:48) | 1 つ目 | 1 |
| W20-5 | stop-words の probe を、hook の rc を読む / head の rules を注入しない形で 2 回撃った (陽性対照で検知) | 2 つ目 | 2 |
| W20-6 | 実行ビットを落とした複製で変異を測り「41 failed」と確定形で書いた (実測 6) | 2 つ目 | 1 |
| W20-7 | #604 の指示書の分母に W20-1〜3 を入れなかった (Lead 側の 2 文書の食い違い。レビュアーが検出) | (レーン N が判断) | 1 |
| W20-8 | close 指定の語 + 番号を main の commit 用 heredoc に書き、#562 の gate に止められた | 4 つ目 | 3 (10-09 / 10-14 / 10-14) |
| W20-9 | 進捗表の時刻ラベルと merge 時刻を時計を見ずに書いた | 1 つ目 | 7 か所 (10-14) |

## 5. ゲートの型に足した点

1. **検証役は、PR 本文の反映を確かめてから起動する** (2 巡目の 6 件の後。3R の差し戻し書にも「本文の反映まで済ませてから止まる」を入れた)
2. **merge の前に、GO の全本を merge 順に `merge-tree` → `commit-tree` で重ねた模擬の木で full run を撃つ**。merge 後の木がその鎖と同じ tree hash になることを確かめ、merge 後の full run を兼ねた
3. **回収レーン・段5 レーンは別 spec で縄張りを宣言する** (本波の spec に足すと、merge 済みのレーンと交差して段1 が COLLIDE を出すため)
4. **段7 で申し送りを全件監査する** (送り元 PR の本文から全行を抜き、反映先を main / issue / 未反映で判定。下の 7)
5. **レーンの見張り script** (`prompt/wave-20/monitor-lanes.py`。transcript と origin の先端と PR の状態を 60 秒ごとに見て、質問・許可待ち・push 待ち・push・PR の変化を 1 行ずつ出す)。push 待ちは「fence の中に push が在る時だけ」と絞った (本文の予告文で誤報したため)。各 worktree の `.claude/settings.json` に、質問と許可待ちで鳴る通知音を置いた
6. **差し戻しの入口ファイルは上書きしない** (`next.md` → `next-2.md`)。読み終えたレーンへの追加は別ファイル (`lane-n-add1.md`) で渡す

## 6. HITL 痕跡

Lead の `AskUserQuestion` (**呼出と設問を分けて書く**。逐語は進捗表の各節):

| 日 | 呼出 | 設問 | 中身 → 実回答 | 機械で辿れる転記先 |
|---|---|---|---|---|
| 10-08 | 記録に無い | 4 | レーンの立て方 →「11 本を一気に立てる」/ 閉じてよい 4 件 → 4 つとも / #537 の直し方 →「repo に取り込んで PR で直す」/ #538 →「上げないと決めて閉じる」 | 進捗表「HITL の痕跡」節 (repo に commit 済み)。#479 #399 #342 #441 #538 は各 issue のコメント |
| 10-08 | 1 | 1 | 第19波までの worktree 15 本の撤収 →「15 本とも撤収する」 | 進捗表「段6 (第19波までの撤収)」節 |
| 10-10 | 1 | 1 | 本体の settings.json の未 commit →「その変更を commit して取り込む」 | 進捗表 |
| 10-14 | 1 | 2 | 回収レーン →「今、回収レーンを 1 本立てる」/ W20-8 →「段5 の計上で再発マークを刻む」 | 回収はレーン M の PR #620 本文 / W20-8 はレーン N の PR 本文 (転記させる) |
| 10-14 | 1 | 1 | 段5〜7 をどこまで →「今日はここで止める」(直後にチャットで「あーじゃあ続けて」と覆した) | HITL は経たが機械で辿れる痕跡はまだ無い (進捗表のみ) |
| 10-14 | 1 | 1 | 段6 の撤収 14 本 →「14 本とも撤収する」 | 進捗表のみ |
| 10-14 | 1 | 1 | W20-9 →「段5 の計上で再発マークを刻む」 | レーン N の PR 本文 (転記させる) |

レーンが聞いたもの (#595 #596 #597 #601 #602 #605 #606 #607 #620 ほか) は、各 PR 本文の「HITL の痕跡」節に逐語で在る = 既に痕跡。

## 7. 申し送り — 反映監査 (段7 で初めて全件を測った)

送り元 PR 13 本の本文から「申し送り」節を全行抜き、1 件ずつ反映先を判定した (調査役 1 体。基準は `origin/main` `52a5fcf`、途中で `0037988` へ進んだが差は進捗表と spec だけ)。**69 行: 反映済み 11 (main 8 / machine-local の台帳 3) / issue へ 14 / 未反映 43 / 判定不能 1**。

**未反映 43 件は「8. 未処理」にも載せる。** 根が同じものでまとめると 4 つで 17 件になる: 層3d 台帳の再測の対象 (5 件) / committer のパス・行番号の引用 (6 件) / fork の `D=~/.claude` が権限チェックで拒否される (4 件) / worktree の `defaultMode` の churn (4 件)。

以下は監査の全表 (調査役の出力をそのまま写した。行番号の版は表の列見出しのとおり):

### 7-表

| # | 送り元 PR | 宛先 | 対象ファイル (`:N` は PR 本文の版) | 中身 | 反映先 | 根拠 |
|---|---|---|---|---|---|---|
| 595-1 | #595 | 波の外 | `skills/essence-reviewing-orchestrator/references/step-1-2.md` + 3 fork の `SKILL.md` | 所要時間の記述が fork の background 起動に追随していない | 未反映 → 次波: orchestrator の Step 2-1 と 3 fork の「duration 計測は master の職分」節 | main の fork 3 本の :205 / :217 / :235 に「所要時間は master が Skill 呼出を挟んで計測する」が残る。step-1-2.md は `background` 0 件 (対照: orchestrator `SKILL.md:86` に 1 件)、最終変更は `82997b4` (#463、第20波より前)。open issue に `所要時間` / `step-1-2` の該当 0 件 |
| 595-2 | #595 | 波の外 (観測) | worktree の `settings.json` | 起動時から `defaultMode` が `default` → `auto` の差分 (commit していない) | 未反映 → 次波: 607-6 と同じ (churn hook の凍結対象か新 issue) | main `settings.json:292` は `"default"` で、差分は main に入っていない。churn hook の凍結対象 (`FREEZE_KEY_LIST=(model effortLevel modelSettings)`、main :175) に無い。open issue での `defaultMode` の言及は #571 の 1 行 (第19波 worktree の dirty の推測) だけ |
| 596-1 | #596 | 波の外 (orchestrator / 3 fork の持ち主) | orchestrator と 3 fork の `SKILL.md` の `!` 構文 | `D=~/.claude` が Skill 読込側の権限チェックで拒否され、3 体とも起動しない | 未反映 → 次波: 新 issue (4 本の `!` 構文) | main で `D=~/.claude` を含む行: orchestrator 6 / harness-fork 4 / skill-fork 5 / ui-fork 7。open issue に `Tilde` / `D=~` の該当 0 件 (検索のヒットは別件)。開示は essence record だけ (例 `2026-10-08_123850_authoring-skills_self-eval-v3.md:5`) |
| 596-2 | #596 | 波の外 | orchestrator の self-eval | Step 1.5 の推奨 (`NORMAL_REVIEWS_SINCE=38`) を見送った。着地後の版で回す | 未反映 → 次波: orchestrator self-eval v19 | main の records で orchestrator の self-eval は v18 (`2026-08-21_190026`) が最後。以後 0 本 |
| 596-3 | #596 | H (観測) | worktree の `settings.json` | 起動時点で `defaultMode: auto` へ変わっていた | 未反映 → 次波: 607-6 と同じ | 595-2 と同じ |
| 597-1 | #597 | F | `skills/orchestrating-harness-issue-waves/references/selection-and-launch.md:238` | 「`Closes #N` はバッククォートで囲むと発火しない」を PR body に限り、commit message は git-workflow.md を指す | 未反映 → 次波: selection-and-launch.md 2-5 (運ぶブロックの中なので、直したら `test-extract-lane-payload.sh` を撃ち直す) | main :243 に同じ文が限定なしで残る (`バッククォート` 1 件)。main `git-workflow.md:19` は「バッククォートで囲んでも gate は止める」。open issue に該当 0 件 |
| 597-2a | #597 | C (L の着地後) | `.docs/progressive-disclosure/git-workflow.md:17` | committer の行を直す | 未反映 → 次波: git-workflow.md:17 (598-1a と同じ行) | main :17 は「Commit helper が PATH にある: `committer` (bash)」のまま。同ファイルに `committer.sh` / `Test Gate` / `skip-tests` は 0 件 (`committer` は :17 / :18 / :20 で拾えている)。最終変更は #597 自身の `98d96ed` |
| 597-2b | #597 | C (L の着地後) | `hooks/test/cases/test-hook_pre_commit_grounds_gate.sh` (`local/bin/committer` を引く箇所) | 正本のパスへ直す | 未反映 → 次波: 同テストの :366 (直す要否は小。598-1b) | main :366 に `~/.local/bin/committer` が残る。live の `~/.local/bin/committer` は `hooks/lib/committer.sh` への symlink (readlink で確認) なので、指す実体は同じ |
| 597-3 | #597 | 波の外 (#495) | `hooks/test/cases/test-run-all-dispatch.sh` の層3d 台帳 | grounds gate のテストを変えたので、指紋と所要時間が動く | issue へ #495 (と #571 タスク 1) | main の `HEAVY_TESTS` 6 行 (:559-564) は #571 本文の 2026-09-13 の写しと同じ = 未更新。#495 は受入基準に「`HEAVY_TESTS` の値を更新」を持ち、reopen コメントに「台帳の更新と静穏再測は未達」。#571 の 10-14 コメントが「タスク 1 (層3d 台帳の再測) は第20波の全 merge の後」 |
| 597-4a | #597 | 次の issue 候補 | `hooks/hook_pre_commit_grounds_gate.sh` | 根拠捏造の message 検査 (`MSG_HITS` / `AUQ_HITS`) にも `-F` の中身を読ませる | 未反映 → 次波: 新 issue (grounds gate) | main :559 `MSG_HITS` と :611 `AUQ_HITS` はどちらも `$COMMAND` だけを走査する。`-F` のファイルを読むのは :748 の closing keyword の検査だけ。open issue に `MSG_HITS` / `AUQ_HITS` 0 件 |
| 597-4b | #597 | 次の issue 候補 | 3 fork の `SKILL.md` | fork 3 体が `!` 構文の権限チェックで起動できない (#581 / #553 / #562 で 3 回連続) | 未反映 → 次波: 596-1 と同じ新 issue | 596-1 と同じ |
| 598-1a | #598 | C | `.docs/progressive-disclosure/git-workflow.md` 17 行 | 正本が `hooks/lib/committer.sh` になったこと・Test Gate の発火条件・時間上限が無い環境・`--skip-tests` は人の明示時だけ・告知行を書き足す | 未反映 → 次波: git-workflow.md:17 | 597-2a と同じ (main :17 のまま。`committer.sh` / `Test Gate` / `skip-tests` / `vitest` 0 件) |
| 598-1b | #598 | C (観測) | `hooks/test/cases/test-hook_pre_commit_grounds_gate.sh` 366 行 | `~/.local/bin/committer` を引く (張り替え後も同じ実体を指すので古くならない見込み) | 未反映 → 次波: 対応は任意 (597-2b と同じ行) | main :366 のまま。symlink は `hooks/lib/committer.sh` を指す (readlink) ので、見込みどおり実体は同じ |
| 598-2 | #598 | H | `hooks/hook_pre_commit_settings_churn_normalize.sh` 374 / 554 行、同テスト 408 行 | `committer:106,111` の行番号引用を、行番号でなく行の中身で指す形へ (実体は committer.sh の 204 / 209 行) | 未反映 → 次波: churn hook とテストの 3 か所 (607-3 と同じ) | main の hook :496 / :701 とテスト :408 に `committer:106,111` が残る。main `hooks/lib/committer.sh` の該当 2 行は :204 / :209。open issue に `106,111` 0 件 |
| 600-1a | #600 | B | `hooks/lib/pr_body_claims_check.sh:385-403` | 3 パーサの比較表の gate 行が「ファイル全体を `head -1`」「追跡は #528」のまま | issue へ #528 | main :532「ファイル全体を `head -1`」/ :546「追跡は #528」が残る。#528 は受入基準「3 装置の走査範囲が揃ったことを比較表へ反映」と 10-14 のコメント ① で運ぶ。main の orchestrator `gotchas.md:62-64` も stale を開示 |
| 600-1b | #600 | B | checker と validator の間 | gate↔validator と同じ「実物同士の同期 assert」が無い | 未反映 → 次波: #528 の残りへ追記 (`test-pr_body_claims_check.sh`) | main `gotchas.md:69`「checker との間は縛っていない」と `test-hook_pre_commit_gate.sh:2341`「checker と validator の鏡像は射程外」= 開示だけ。open issue に該当 0 件 (#528 も比較表と fence の話だけ) |
| 600-2 | #600 | Lead | `skills/essence-reviewing-orchestrator/references/output-format.md` 191 行 | 「書く時の注意」② を #528 の後の事実へ (書き換えると suite の上限表の測り直しも要る) | 未反映 → 次波: #528 の残りへ追記 | main :191 の ② は「essence-gate だけが…ファイル全体を `head -1` で読む…追跡 #528」のまま。最終変更は `2bc3135` (2026-08-26)。`gotchas.md:63` が stale を開示。#528 の本文とコメントに `output-format` 0 件 |
| 600-3 | #600 | 波の外 (#485) | 層3d 台帳 | gate 本体とテストを変えたので gate テストの指紋が動いた | issue へ #485 (と #571 タスク 1) | 台帳は 597-3 と同じく未更新。#485 は受入基準「静穏条件で測り直した値を台帳へ」を持ち、reopen コメントで「未達」 |
| 600-4 | #600 | Lead (開示) | `hooks/hook_pre_commit_gate.sh` | 分割条件 B (単一の段が 300 行超) が base で成立。1 ファイルのままの理由を line-limit 台帳へ記録した | 反映済み (git 外: machine-local の hook-fire 台帳) | 台帳 `2026-10.jsonl` に judgment 行 (2662 行の時点 = 10-08T07:21:13Z、2700 行の時点 = 10-14T09:21:25Z。理由文に「分割条件 B は base 時点で (c) 段 3…」)。main の gate は 2700 行。分割そのものを追う issue は無い |
| 600-5 | #600 | follow-up 候補 | gate / validator / checker の 3 本 | 節の開始より前の fence を追わない・`~~~` fence と HTML コメントを知らない | issue へ #528 | #528 の 10-14 コメント ③「節より前の `~~~` fence / HTML コメントに節と例示を置くと 3 本とも例示を読む」 |
| 600-6a | #600 | follow-up 候補 | `skills/essence-reviewing-orchestrator/references/gotchas.md` | 「2 層防御パターン」が「Step 0 で `cd` 実行」と書くが、fork は `cd` を打たない (#276) | 未反映 → 次波: gotchas.md:29 | main :29 に「Layer 2 = sub-skill Step 0 で `cd` 実行」が残る。open issue に `2 層防御` 0 件 |
| 600-6b | #600 | follow-up 候補 | 同 `gotchas.md` | 冒頭と「Lead 統合判断の罠」が、今の CLAUDE.md に無い節 (`## Skills` / `## Harness`) を参照 | 未反映 → 次波: gotchas.md:3 と :37 | main :3 に `## Skills`、:37 に `## Harness`。main の CLAUDE.md の `## ` 見出しは 6 個 (Username / Persona / Response / Prohibition / Interview / Stack) で、どちらも無い |
| 600-7 | #600 | Lead (merge 後) | line-limit 台帳 (machine-local) | 最終形の行数 (2700 / 3216 / 2539) で `record` を撃ち直す | 反映済み (git 外: machine-local の hook-fire 台帳) | 台帳に judgment 行 2700 / 3216 / 2539 (10-14T09:21:25-27Z)。main の実ファイルの `wc -l` も 2700 / 3216 / 2539 で一致 |
| 601-1 | #601 | Lead | `skills/reviewing-pr-with-google-engineering-practices/references/team-development-pr-gate-playbook.md` Step B-2 | #514 の新しい契約への移行 4 点 (`--repo` / `src=` と `head_match=` / rc=2 の原因 / head 不一致の WARN) | issue へ #615 | #615 §1 ①〜④ と受入基準。main の playbook に `head_match` / `src=` 0 件 (対照: 同ファイルの `pr_body_claims_check` は 1 件) |
| 601-2 | #601 | Lead | `skills/creating-pr/SKILL.md` 規約 1-4 | rule C (`path:行番号` に hash を添える) を規約 5 として足す | issue へ #615 | #615 §2。main の creating-pr に該当の規約なし (:40 の「5.」は「main ブランチ上でないこと」で別物) |
| 601-4 | #601 | G | `.docs/hook-authoring-guide.md` 188 / 260 行 (`600fcde` 時点) | 「`pr_body_claims_check.sh --pr` は検証不能で rc=0」は再現で成立しなかった (取得失敗は rc=2) | 未反映 → 次波: 段5 レーン N の縄張り (`hook-authoring-guide.md`)。台帳は記録層なので訂正の注記で | main :205 / :287 に同じ文が残る (`600fcde` の :188 / :260 と同文 = 既知陽性)。open issue に `検証不能` 0 件 |
| 601-5 | #601 | A | `6e1405d:hooks/lib/pr_body_claims_check.sh:518` 〜 `:536` | A から来る「追跡は #528」行の対象が動いた | issue へ #528 | main :546 に「追跡は #528」(`6e1405d` では :536 = 既知陽性)。運び先は 600-1a と同じ #528 の受入基準とコメント ① |
| 601-6 | #601 | 波の外 | essence orchestrator の fork 3 体と master | `D=~/.claude` の権限チェック (Tilde in assignment value) で起動前に落ちる | 未反映 → 次波: 596-1 と同じ新 issue | 596-1 と同じ |
| 601-7 | #601 | 開示 | `pr_body_claims_check.sh` / `test-pr_body_claims_check.sh` | 2 本とも 500 行超。「1 ファイル維持」を line-limit 台帳へ記録済み | 反映済み (git 外: machine-local の hook-fire 台帳) | 台帳に judgment 行 778 / 1148 (10-13T20:30:25Z)。main の `wc -l` も 778 / 1148 |
| 602-1a | #602 | F | `skills/orchestrating-harness-issue-waves/references/selection-and-launch.md` L201 の 3 か所 | `-i` の字句探索 ①・in-place 置換 ②・結合フラグと `>&` が「KNOWN GAP に未記載。#559」③ が #602 の着地で古くなる | 未反映 → 次波: selection-and-launch.md 2-5 (運ぶブロックの中。直したら `test-extract-lane-payload.sh` を撃ち直す) | main :206 に 3 句とも残る (「KNOWN GAP に未記載。#559」1 件)。open issue に `結合フラグ` 0 件 (#621 は同ファイルの fence の行末コメントだけ) |
| 602-1b | #602 | F | 同 L209 | 書込形の列挙の `>&` と結合フラグ (2R で zsh の `>!` 系も拾うようになった) | 未反映 → 次波: 同 2-5 | main :214 に「`>&` は 2 枚目では当たらない」「`-Ei` / `-lpi` の結合フラグは当たらない — #559」が残り、`>!` の言及は 0 件 |
| 602-2 | #602 | 波の外 (#485 / #495) | 層3d 台帳 | `test-hook_pre_commands.sh` の変更で指紋が動く | 未反映 → 次波: #571 タスク 1 (全 entry の指紋更新として足す) | 台帳は未更新 (597-3)。#485 / #495 は別のテスト (gate / grounds gate) を追い、#571 タスク 1 の手順も 2 本の再測だけで pre_commands の指紋に触れない |
| 602-3a | #602 | Lead 判断 (新 issue 候補 ①) | `hooks/test/lib/` の共有の事例表 | 2 層の in-place の綴りの食い違いを検知する | issue へ #618 | #618 §4 と「方向 4」(両テストが source する事例表を `hooks/test/lib/` に置く) |
| 602-3b | #602 | Lead 判断 (新 issue 候補 ②) | Bash guard 2 枚 | find -exec / xargs で書込先を渡す形の塞ぎ方と「一括置換禁止」の綴りの拡張 | 未反映 → 次波: #618 へ追記か新 issue | open issue に `xargs` / `find -exec` / `一括置換` 0 件 (#618 は改行・`;`・`~user` と綴りのテストだけ)。main の 2 枚目 guard :83 の KNOWN GAP に find -exec・xargs の開示はある |
| 603-1 | #603 | F | `references/review-and-merge.md` 3-3b 末尾 | 検査器の `CHECKER blob=… path=…` 行を fence の `echo` と並べて転記する形へ (#523) | 反映済み `8ac52a2` (PR #620) | main :71 に `CHECKER blob=` 1 件 (#620 の前の `2fa60db` では 0 件) |
| 603-2 | #603 | F | 同 3-3b | 「`NEW-UNUSED` は warn で rc を変えない」の 1 行 (#560) | 反映済み `2ae8bb9` / `b08e912` (PR #620) | main :81 (`NEW-UNUSED` 2 件。`2fa60db` では 0 件) |
| 603-3 | #603 | F | 同 3-3b | `NEW:SHARED` の在る波では全レーンの changed を連結して `uniq -d` で重複を見る | issue へ #621 | #621 §5 の 1 点目。main の review-and-merge.md に `uniq -d` 0 件 |
| 603-4 | #603 | F | `selection-and-launch.md` 完了条件 9 と `review-and-merge.md` 3-3 | 言い回しを片方の語へ揃える (#558) | 反映済み `85684ca` / `e7f9358` (PR #620) | main の両ファイルに「検出器は無」が 1 件ずつ (`2fa60db` の selection-and-launch.md では 0 件)。#558 は PR #620 の merge で CLOSED |
| 603-5 | #603 | F | 同 3-3b の `<base>` (54 行付近の前提を含む) | `<base>` は fetch 後の base branch の先端、branch 名の変数 1 本 + 前置の `git fetch` (#523) | 反映済み `8ac52a2` / `c53d17d` / `e8dcea9` (PR #620) | main :39 に `git fetch`、:63 に「`B` は fetch 後の base branch の先端 (branch 名) で、hash を入れない」。`2fa60db` :54 の「`<base>` が検査器の移設 (PR #536) より前の commit だと」は main で 0 件 (`8ac52a2` が外した) |
| 603-6 | #603 | F | `selection-and-launch.md` 1-3 | 段1 の呼出が相対パス・出力トークンの列挙が `CHECKER` 行に追いついていない | issue へ #621 | #621 §5 の 2 点目。main :37 の呼出は `bash skills/checking-parallel-file-conflicts/…` の相対形、:40 のトークン列挙に `CHECKER` 無し |
| 603-7 | #603 | Lead | `hooks/test/cases/test-lane-territories.sh` | 26〜60s かかり commit gate から届くのに層3d 台帳に無い | 未反映 → 次波: #571 タスク 1 (再測の対象に足す) | main の `HEAVY_TESTS` 6 行に無い。#621 §4 は同テストが 3-3b の fence を縛らない話で、所要時間ではない。#485 / #495 / #571 にも無い |
| 603-8 | #603 | Lead | 縄張り検査器 (`check-lane-territories.sh`) | 全レーンの changed の積集合を出すモードなら、人へ委ねた 2 つの穴を決定論で測れる (起票候補) | 未反映 → 次波: 新 issue (#621 §5 の隣) | open issue に `積集合` 0 件。#621 §5 が運ぶのは手作業の `uniq -d` (603-3) で、検査器の新モードではない |
| 603-9a | #603 | Lead | orchestrator の self-eval | Step 1.5 が `SKILL_MODIFIED=yes` / `NORMAL_REVIEWS_SINCE=40` | 未反映 → 次波: 596-2 と同じ | 596-2 と同じ |
| 603-9b | #603 | Lead | fork の起動 | Skill 経由の fork 起動が `Tilde in assignment value` で拒否される (同日 2 回) | 未反映 → 次波: 596-1 と同じ新 issue | 596-1 と同じ |
| 603-10 | #603 | Lead | 検査器のヘッダ | ヘッダの減量 (3 本の record に共通の残存 Medium) を独立の改修として切り出す | 未反映 → 次波: 新 issue (`check-lane-territories.sh` のヘッダ) | open issue に `減量` 0 件。#621 §5 の「検査器ヘッダを指す形へ」はヘッダを参照先にする話で、減らす話ではない |
| 604-1 | #604 | E | `skills/orchestrating-harness-issue-waves/SKILL.md` の Gotcha「指示書は 1 ファイル + 3 行プロンプト」 | マーク 2 個 (2026-08-21 / 2026-09-11) を足す (HITL で決定済み。記入で通算 4 回目) | 未反映 → 次波: 段5 の計上 (:113 の bullet と :119 の注記を同じ commit で) | main の SKILL.md の `(再発:` は :113 の 1 個 (2026-08-24) だけ。:119 の注記は「マーク 1 個 (通算 2 回目)」のまま |
| 604-2 | #604 | Lead / 波 skill | 縄張りの計画 | L1 計上のレーンは `rules/grounds-not-approval.md` と `.docs/hook-authoring-guide.md` の両方を縄張りに持つ | 未反映 → 次波: `references/closeout.md` 段5 か `selection-and-launch.md` 1-3 | main の波 skill に `hook-authoring-guide` / `台帳も同じ` / `両ファイル` 0 件 (closeout.md の段5 節にも無い)。運用では守った: `0037988` の progress :443 と `wave-20-l1-territories.spec` が段5 レーン N の縄張りをこの 2 本 + 契約テストにしている。手順の側には未記載 |
| 604-3 | #604 | 波の外 (#485 / #495) | 層3d 台帳 | `test-hook-authoring-guide-contract.sh` の指紋 (PR 本文の申告値 3066473150) と秒数を測り直す | 未反映 → 次波: #571 タスク 1 | main の台帳は `test-hook-authoring-guide-contract.sh:10:131231057` のまま。#485 / #495 は別のテスト |
| 604-4 | #604 | 次の issue 候補 | `hooks/test/cases/test-hook-authoring-guide-contract.sh` の (5) | (5) を独立したテストファイルへ分ける | 未反映 → 次波: 新 issue (同テストは段5 レーン N の縄張り) | main の `hooks/test/cases/` で `authoring-guide` を名に含むのは contract 1 本だけ。open issue に該当 0 件 |
| 604-5 | #604 | E | 波 skill の差し戻し書テンプレ | 「直しを commit する前に `harness-policy.md`「手順を書き換える時」を Read し、生まれた分岐を列挙して通す」の 1 行 | 未反映 → 次波: #526 案 3 (差し戻し書テンプレ) か review-and-merge.md | main の波 skill に `harness-policy` 0 件 (対照: 同じ grep が `skills/` の別ファイルで 1 件、`差し戻し` は SKILL.md 4 / review-and-merge.md 2 件)。open issue に `手順を書き換える` 0 件 |
| 604-6 | #604 | Lead | `.docs/logs/shared/wave-20-progress.md` | 見出し「レーン G が分類する」の直し | 未反映 → 次波: 段7 で Lead が直す | `52a5fcf` と `0037988` のどちらでも :96 の見出しがそのまま |
| 605-1 | #605 | Lead (#546 の Medium / M-9) | `hooks/test/cases/test-responsive-lint-skill-tests.sh:80-84` | assertion 床を 34 → 47 へ | 反映済み `fd410d5` (PR #620) | main :83 が `-ge 47` (`2fa60db` は :81 が `-ge 34`)。「変わらず」節の M-9 も同じ中身なので 1 行に畳んだ |
| 605-2 | #605 | Lead (#485 / #495) | 層3d 台帳 | `test-crafting-frontend-interfaces.sh` を足した。dispatch は既存の指紋ずれを warn していた | 未反映 → 次波: #571 タスク 1 | 台帳 6 行は未更新 (597-3) |
| 605-3 | #605 | 開示 | 配線 wrapper (`test-crafting-frontend-interfaces.sh`) | `templates/decision-ledger.yml` は引き金にしない (.yml は gate の (c) の手前で除外)。template だけの commit は gate で検査されない | 反映済み `8247d22` (PR #605) | main :22-25 に同じ開示と「template を直したら gates.test.sh を手で撃つ」 |
| 605-4 | #605 | 開示 | `skills/coding-responsive-layouts/SKILL.md` の Composition 表 | `crafting-frontend-interfaces` 行を「草案・未導入」から実態へ | 反映済み `00cf1b6` (PR #605) | main :132 は「Owns the frontend build workflow…」、`草案` 0 件 (base `3a473ad` :130 は「Draft, not installed」) |
| 606-1 | #606 | E | `skills/orchestrating-harness-issue-waves/SKILL.md` の「機械層」節 | 新 script `extract-lane-payload.sh` とテストの開示 | 反映済み `6789ce0` / `066d458` (PR #620) | main :79 (`extract-lane-payload` 1 件。`2fa60db` では 0 件) |
| 606-2 | #606 | 次波 | `selection-and-launch.md` 2-5 | 運ばない probe 保管庫を別の reference へ移し、本文は結論とポインタだけ | issue へ #621 | #621 §6 |
| 606-3 | #606 | Lead (merge 順) | — | D (#602) → F (#606) の順で merge する | 判定不能 | merge の時点だけの依頼で、ファイルにも issue にも載る性質のものでない (下の「判定できなかった行」) |
| 606-4 | #606 | Lead (観測) | orchestrator の self-eval | Step 1.5 が `NORMAL_REVIEWS_SINCE=38` / `SKILL_MODIFIED=yes` | 未反映 → 次波: 596-2 と同じ | 596-2 と同じ |
| 607-1 | #607 | D / Lead | `hooks/hook_pre_commands.sh` | `{"decision": "approve"}` が `permissions.ask` と default mode の確認を飛ばす。直すと運用影響の HITL が要る | issue へ #599 | #599 (open) の本文と 10-14 のコメント (根拠の訂正) |
| 607-2 | #607 | Lead | 計測の手順 | 途中で足した permission 規則がそのセッションで効かない (計測⑥) | issue へ #599 | #599 本文「付随して分かったこと」の 1 点目。ただし波 skill の計測手順には入っていない (波 skill に `途中で足した` 0 件) |
| 607-3 | #607 | L | churn hook の 2 か所 (374 → 496 行 / 554 → 701 行) とテスト 408 行 | `committer:106,111` の行番号引用 | 未反映 → 次波: 598-2 と同じ | 598-2 と同じ (main の hook :496 / :701、テスト :408) |
| 607-4 | #607 | D | `hooks/test/cases/test-hook_pre_commands.sh:399` | 「内訳: test-hook_pre_commit_settings_churn_normalize.sh:547」が消えた行を指す。同じ行の「残る 3 行」も 1 つ減るはず | 未反映 → 次波: test-hook_pre_commands.sh (#618 と同じテスト群) | main :481「残る 3 行」/ :483「内訳: …churn_normalize.sh:547」が残る。main の churn テスト :547 は `mk_repo` の jq 行で、引いていた DUP の literal は 0 件 (`600fcde` では 1 件 = 既知陽性) |
| 607-5 | #607 | 波の外 (#485) | churn のテスト | 20 秒 (30 秒未満)。dispatch は本 branch で 53 / 0 | 未反映 → 次波: #571 タスク 1 の入力 (30 秒未満なら登録不要) | main の台帳に churn テストは無い。対応が要らない観測の可能性が高い |
| 607-6 | #607 | 観測 (HITL 待ち) | `settings.json` / churn hook | `defaultMode` もアプリが追跡ファイルへ書く (#545 と同型の 3 例目)。凍結対象に足すかは別途 HITL | 未反映 → 次波: 新 issue (HITL 込み、churn hook の `FREEZE_KEY_LIST`) | main hook :175 の `FREEZE_KEY_LIST=(model effortLevel modelSettings)` に無い。open issue に該当 0 件 |
| 607-7 | #607 | 観測 | churn hook | merge の衝突を解いた後の bare `git commit` で、上流が変えた `modelSettings` も `index-churn` で止まり、deny が勧める restore が上流の変更を落としうる | 未反映 → 次波: churn hook の KNOWN GAP か新 issue | main hook が持つのは「unmerged の衝突中 = `index-unreadable`」(:73 / :522) だけで、解いた後の形の開示は無い。open issue に `index-churn` 0 件 |
| 607-8 | #607 | 観測 (縄張り外) | `hooks/hook_post_file_line_limit.sh.bak-260521` | git 管理下にある | 未反映 → 次波: 削除の判断 (`agent-protocol.md` の削除手順) | origin/main の `git ls-tree` に在る |
| 620-1 | #620 | `checking-parallel-file-conflicts` の持ち主 (Lead / 次波) | `references/verify-and-reassign.md` 3 節の段3 の fence | 行末コメント 7 行で、対話 zsh に貼ると `B` が空のまま走る | issue へ #621 | #621 §1 (d)。main の fence に行末コメントが残る (:97 / :103 / :108 / :115 / :116 ほか)。最終変更は `f5ee29f` (#603) |
| 620-4 | #620 | Lead | worktree の `settings.json` | 未 commit の `defaultMode` = `auto` (本レーンの変更ではない) | 未反映 → 次波: 607-6 と同じ | 595-2 と同じ |

### 7-集計

(下の数は、上の表の行を awk で数えた値。表の行は 69、全行が 7 列)

### 送り元 PR ごと

| 送り元 PR | 行数 | 反映済み (main) | 反映済み (git 外) | issue へ | 未反映 | 判定不能 |
|---|---|---|---|---|---|---|
| #595 | 2 | 0 | 0 | 0 | 2 | 0 |
| #596 | 3 | 0 | 0 | 0 | 3 | 0 |
| #597 | 6 | 0 | 0 | 1 | 5 | 0 |
| #598 | 3 | 0 | 0 | 0 | 3 | 0 |
| #600 | 9 | 0 | 2 | 3 | 4 | 0 |
| #601 | 6 | 0 | 1 | 3 | 2 | 0 |
| #602 | 5 | 0 | 0 | 1 | 4 | 0 |
| #603 | 11 | 4 | 0 | 2 | 5 | 0 |
| #604 | 6 | 0 | 0 | 0 | 6 | 0 |
| #605 | 4 | 3 | 0 | 0 | 1 | 0 |
| #606 | 4 | 1 | 0 | 1 | 1 | 1 |
| #607 | 8 | 0 | 0 | 2 | 6 | 0 |
| #620 | 2 | 0 | 0 | 1 | 1 | 0 |
| **計** | **69** | **8** | **3** | **14** | **43** | **1** |

### 反映先ごと

- **反映済み 11** — main へ 8 (PR #620 の回収で 6 = 603-1 / 603-2 / 603-4 / 603-5 / 605-1 / 606-1、PR #605 自身で 2 = 605-3 / 605-4) + git 外の台帳へ 3 (600-4 / 600-7 / 601-7)
- **issue へ 14** — #528 が 3 (600-1a / 600-5 / 601-5)、#621 が 4 (603-3 / 603-6 / 606-2 / 620-1)、#615 が 2 (601-1 / 601-2)、#599 が 2 (607-1 / 607-2)、#485 / #495 / #618 が 1 ずつ (600-3 / 597-3 / 602-3a)
- **未反映 43** — 根が同じものをまとめると次のとおり (次波の宛先つき)
  - 層3d 台帳の再測の対象が #571 タスク 1 に載っていない 5 (602-2 / 603-7 / 604-3 / 605-2 / 607-5) → #571 タスク 1 へ「全 entry の指紋 + 追加候補 2 本」として足す
  - committer の正本パス・行番号の引用 6 (597-2a / 597-2b / 598-1a / 598-1b / 598-2 / 607-3) → `git-workflow.md:17`、grounds gate テスト :366、churn hook :496 / :701 とテスト :408
  - fork の `!` 構文 `D=~/.claude` 4 (596-1 / 597-4b / 601-6 / 603-9b) → 新 issue
  - `defaultMode` の起動時の書き換え 4 (595-2 / 596-3 / 607-6 / 620-4) → 新 issue (凍結対象に足すかの HITL 込み)
  - orchestrator の self-eval 3 (596-2 / 603-9a / 606-4) → self-eval v19
  - 波 skill の手順・Gotcha 7 (597-1 / 602-1a / 602-1b / 604-1 / 604-2 / 604-5 / 604-6) → `selection-and-launch.md` 2-5 (運ぶブロック)・`SKILL.md` Gotcha・`closeout.md` 段5・差し戻し書テンプレ (#526)・progress の見出し
  - essence orchestrator の docs 4 (595-1 / 600-2 / 600-6a / 600-6b) → `step-1-2.md` と fork 3 本の duration 節・`output-format.md:191`・`gotchas.md:3` / `:29` / `:37`
  - 検査器・テストの起票候補 5 (600-1b / 602-3b / 603-8 / 603-10 / 604-4) → #528 / #618 への追記か新 issue
  - その他 5 (597-4a grounds gate の `-F` / 601-4 guide 台帳の訂正注記 / 607-4 pre_commands テストの内訳 / 607-7 churn の merge 後の形 / 607-8 `.bak` の tracked)
- **判定不能 1** — 606-3

### 7-判定できなかった行

- **606-3 (merge 順 D → F)**: merge の時点だけの依頼で、反映先になるファイルも issue も無い。守られてはいない (progress の段4 の記録では #606 が #602 より先に merge)。ただし懸念 (D の改修で F のテストが赤くなる) は、8 本を重ねた full run で `test-extract-lane-payload.sh` 44/0 と progress に記録されている (この値は本監査では測り直していない)

### 7-3 値の定義に収まりきらない行 (判定はしたが、反映先が main でない)

- **600-4 / 600-7 / 601-7**: 反映先は machine-local の hook-fire 台帳 (`~/.claude/.docs/hook-fire-ledger/2026-10.jsonl`) で git の commit は無い。台帳の judgment 行と main のファイル行数の一致で「反映済み」とした
- **598-1b / 607-5**: 「未反映」としたが、PR 本文自身が「古くならない見込み」「30 秒未満」と書いており、対応が要らない可能性が高い

### 7-除いたもの (表に行を作っていない)

- 段5 (L1 計上) の候補 (別に扱う): #595「段5 の計上候補」/ #601 申し送り 3 / #604 2R の Lead 宛のうち「W20 の分類 (段5)」の半分 / #606 申し送り 5 / #620 申し送り 2・3
- 宛先が無い自己検知の開示: #595「作業中に踏んだ測定の罠」(zsh の `tail` の rc。段5 の材料と同じ性質)
- 中身が無い: #595「他レーンへ: なし」/ #596「K への申し送りは無し」
- 申し送りでない: #606 の「#561 — 申し送りの回収側と 2-5 抽出の run-all 配線」「項目 1 (#517)」の 2 節 (PR が何をしたかの説明。見出しの語「申し送り」に当たって抜き出されただけ)
- 重複: #605「申し送り (変わらず)」の M-9 (605-1 に畳んだ) / #606「受け取る申し送り (本 PR には入れていない)」(受け手側の受領の記載。中身は 602-1a・602-1b と 603-1・603-2・603-4 の行と同じ)

## 8. 未処理 (次波以降へ)

次波の段1 は、本節と第20波の指示書 (ハーネス repo の `prompt/wave-20/lane-full.md` は Lead の作業場で git 管理外。要点は進捗表の段1・段2 節) を読んでから選抜を始める。

1. **申し送りの未反映 43 件** — 上の 7 の表で反映先が「未反映」の行。とくに根が同じ 4 つ (層3d 台帳の再測の対象 5 件 → #571 タスク 1 へ追記 / committer の引用 6 件 / fork の `D=~/.claude` 4 件 = どの issue も運んでいない / `defaultMode` の churn 4 件 = どの issue も運んでいない)
2. **申し送りの判定不能 1 件** — #606 の「D → F の順で merge」。実際は F (#606) が D (#602) より先に merge された。懸念 (D の改修で F のテストが赤くなる) は、8 本を重ねた full run で `test-extract-lane-payload.sh` 44/0 を見て起きていないと判断した
3. **段5 で跨いだ Lead の 3 件 (X1〜X3)** — 上の 4-2 の表。次の波の段5 で、計上するかと、Lead の transcript を全数で監査するかを決める。段5 そのものは PR #623 で着地した (分母 61 件 / マーク 31 個)。PR #623 の申し送り (4 つ目の bullet が 3 → 13 へ増えたので L2/L3 へ上げる issue の起票の要否、ほか) も次の波で読む
4. **open のまま残した wave の issue 5 件** — #528 / #511 / #571 / #574 / #546 (理由は各 issue の 2026-10-14 の Lead コメント)
5. **本波に入れなかったもの** — #563 (旧ユーザー名の置換。全レーンの着地後に単独で) / #485 #495 (層3d 台帳の静穏再測。全レーンの着地後に 1 回で = #571 タスク 1) / #526 (段3 の Workflow script 化。E / F の着地後)
6. **波の中で起票した 9 件** — #593 #594 #599 #615 #616 #617 #618 #619 #621
7. **#608 の受入 ③** — キット更新 hook が `/hooks` に出るかを Mirino が見る (①② は Lead が実測で rc 0)
8. **第19波のゲートログの残り** (#571 タスク 4) — 再開後 (2026-09-11〜13) の段3-4 は未執筆のまま
