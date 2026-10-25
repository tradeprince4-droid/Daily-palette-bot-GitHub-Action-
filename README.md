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
