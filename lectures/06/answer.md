# 第6回 解答

## 演習

- STEP 1 のツールの名前：glossary_search_glossary
- STEP 2 の [tool call]：`[tool call]   glossary_search_glossary({'query': '3レベル'})`
- STEP 3 の [tool call]：`[tool call]   fetch_fetch({'url': 'https://modelcontextprotocol.io/'})`
- STEP 4 の [tool call]：`[tool call]   aws_aws___search_documentation({'search_phrase': 'Amazon S3 バージョニング 概要', 'topics': ['general']})`（2回目：`[tool call]   aws_aws___search_documentation({'search_phrase': 'Using versioning S3 developer guide', 'topics': ['general']})`）
- 気づき・疑問：`fetch` は Codespaces 上に起動して外部サイトを取得するのに対し、`aws` は AWS 側で動いているサーバー（`url`）に接続して検索を行う違いがある。外部サーバー経由だとツール数が増え、入力トークン数も大きくなる（11,257 tokens）ことがわかった。

## 宿題

- 取り組んだレベル：基礎
- やったこと：`app/mcp_server.py` に `server.tool()(tools.get_lecture_summary)` を1行追加し、エージェント側（`app/mcp_servers.json`・`app/mcp_agent.py`）を変更せずに `--list-tools`、`--trace "第2回では何を学びますか？"`、`uv run pytest -q` を実行した。
- 結果：見えているツールが7個から8個に増え、`glossary_get_lecture_summary` が追加された。「第2回では何を学びますか？」の質問で `[tool call]   glossary_get_lecture_summary({'lecture': 2})` が呼ばれ、第2回の要点を踏まえた回答が得られた。`uv run pytest -q` も `21 passed` ですべて通った。
- 気づき・疑問：エージェント側を書き換えなくても、サーバー側でツールを公開して起動時に問い合わせる仕組みのため、新しいツールがそのままモデルに渡って利用できるようになる点を確認できた。