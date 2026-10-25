# Daily-palette-bot-GitHub-Action-name: Daily palette
on:
  schedule: [{ cron: "0 6 * * *" }]
  workflow_dispatch:
permissions:
  contents: write
jobs:
  run:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: python palette.py
      - run: |
          git config user.name "palette-bot"
          git config user.email "bot@users.noreply.github.com"
          git add README.md
          git commit -m "Daily palette" || exit 0
          git push
import colorsys, random, re

def palette():
    h = random.random()
    return ["#%02X%02X%02X" % tuple(int(v * 255) for v in colorsys.hls_to_rgb((h + i / 4) % 1, 0.55, 0.65)) for i in range(4)]

block = "<!--PALETTE-->\n" + " ".join(f"`{c}`" for c in palette()) + "\n<!--/PALETTE-->"
text = open("README.md").read()
open("README.md", "w").write(re.sub(r"<!--PALETTE-->.*?<!--/PALETTE-->", block, text, flags=re.S))
