# 第3回 解答

## 演習

- 第1回（素のLLM）とエージェントの回答の違い：素のLLMは学習済みの知識だけで答えるため、「AIE2026の宿題の3レベル」のような講座固有の内容には正確に答えられない。
- `--trace` で見えたツール呼び出し（ツール名と引数）：「LoRA とQLoRAの違いは？」→ `search_glossary({'query': 'LoRA'})` と `search_glossary({'query': 'QLoRA'})` の2回。用語を1つずつ検索していた。
  - 「第2回の内容から確認問題を1問出して」→ `get_lecture_summary({'lecture': 2})` の1回。
- 1つの質問での LLM 呼び出し回数（`requests`）：2
- 追加したツールと、docstring に書いた説明：make_quiz """
　　# 1. load_glossary() から lecture が一致する用語を集める
　　# 2. 1 つ選ぶ（random.choice ）
　　# 3. 問題と答えを文字列で返す（ json.dumps など）

## 宿題

- 取り組んだレベル：基礎
- やったこと：app/tools.py の search_glossary の docstring にある「query には調べたい用語を1つだけ入れる（例: "LoRA"）」を「調べたい用語を複数まとめて入れてよい（例: "LoRA QLoRA"）」に書き換えた。「LoRA とQLoRAの違いは？」を --trace 付きで変更前と変更後に実行し、比較した。
- 結果：
　- 変更前：search_glossary を 'LoRA' と 'QLoRA' で2回呼んだ。
  - 変更後：search_glossary を 'LoRA QLoRA' で1回だけ呼んだ。
  　結果として LoRA の定義は返ってきた（QLoRA も含まれていたかは、trace の表示が途中で切れていて確認できなかった）。
  - docstring の1文を変えるだけで、モデルが渡す引数とツールの呼び出し回数が変わり、トークン数も減った。docstring はモデルにとってツールの使い方の説明書として働いている。
- 気づき・疑問：変更せずに同じ質問を2回実行したところ、回答の文章や output_tokens は毎回変わったが、ツールの呼び方は同じだった
