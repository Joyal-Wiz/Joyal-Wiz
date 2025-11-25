# 👋 Hi there, I'm Joyal-Wiz
<div align="center">
## 🎮 Pac-Man is eating my contributions!
![Pac-Man Animation](https://raw.githubusercontent.com/Platane/snk/output/github-contribution-grid-snake.svg)
</div>
---
## 💻 Tech Stack:
<p align="center">
  <img src="https://img.shields.io/badge/javascript-%23323330.svg?style=for-the-badge&logo=javascript&logoColor=%23F7DF1E" />
  <img src="https://img.shields.io/badge/python-3670A0?style=for-the-badge&logo=python&logoColor=ffdd54" />
  <img src="https://img.shields.io/badge/Solidity-%23363636.svg?style=for-the-badge&logo=solidity&logoColor=white" />
  <img src="https://img.shields.io/badge/html5-%23E34F26.svg?style=for-the-badge&logo=html5&logoColor=white" />
  <img src="https://img.shields.io/badge/css3-%231572B6.svg?style=for-the-badge&logo=css3&logoColor=white" />
  <img src="https://img.shields.io/badge/c-%2300599C.svg?style=for-the-badge&logo=c&logoColor=white" />
</p>
---
## 📊 GitHub Stats:
<div align="center">
  <img src="https://github-readme-stats.vercel.app/api?username=Joyal-Wiz&theme=tokyonight&hide_border=false&include_all_commits=false&count_private=false" height="180em" />
  <img src="https://github-readme-stats.vercel.app/api/top-langs/?username=Joyal-Wiz&theme=tokyonight&hide_border=false&include_all_commits=false&count_private=false&layout=compact" height="180em" />
</div>
<div align="center">
  <img src="https://nirzak-streak-stats.vercel.app/?user=Joyal-Wiz&theme=tokyonight&hide_border=false" />
</div>
---
## 🐍 Watch the Snake eat my contributions!
<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/Joyal-Wiz/Joyal-Wiz/output/github-contribution-grid-snake-dark.svg">
  <source media="(prefers-color-scheme: light)" srcset="https://raw.githubusercontent.com/Joyal-Wiz/Joyal-Wiz/output/github-contribution-grid-snake.svg">
  <img alt="github contribution grid snake animation" src="https://raw.githubusercontent.com/Joyal-Wiz/Joyal-Wiz/output/github-contribution-grid-snake.svg">
</picture>
---
## 🏆 GitHub Trophies
<div align="center">
  <img src="https://github-profile-trophy.vercel.app/?username=Joyal-Wiz&theme=tokyonight&no-frame=false&no-bg=false&margin-w=4" />
</div>
---
## 📈 Activity Graph
<div align="center">
  <img src="https://github-readme-activity-graph.vercel.app/graph?username=Joyal-Wiz&theme=tokyo-night&hide_border=true&area=true" />
</div>
---
## 💡 Random Dev Quote
<div align="center">
  <img src="https://quotes-github-readme.vercel.app/api?type=horizontal&theme=tokyonight" />
</div>
---
## 🔥 Contribution Streak
<div align="center">
  <img src="https://github-readme-streak-stats.herokuapp.com/?user=Joyal-Wiz&theme=tokyonight&hide_border=true" />
</div>
---
<div align="center">
  
### 👀 Profile Views
[![](https://visitcount.itsvg.in/api?id=Joyal-Wiz&icon=0&color=0)](https://visitcount.itsvg.in)
### ⚡ Fun Fact
*I turn coffee into code!* ☕️💻
</div>
---
<div align="center">
  
**💼 Open for collaborations | 🚀 Building the future | 💡 Always learning**
</div>
<!-- Proudly created with GPRM ( https://gprm.itsvg.in ) -->
---
## 🛠️ Setup Snake Animation (Optional)
To get the snake animation working on your profile, follow these steps:
1. Create a new repository named `Joyal-Wiz` (same as your username)
2. Create a folder `.github/workflows/`
3. Create a file `snake.yml` inside with this content:
```yaml
name: Generate Snake
on:
  schedule:
    - cron: "0 */12 * * *"
  workflow_dispatch:
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: Platane/snk@master
        id: snake-gif
        with:
          github_user_name: Joyal-Wiz
          svg_out_path: dist/github-contribution-grid-snake.svg
      - uses: crazy-max/ghaction-github-pages@v2.1.3
        with:
          target_branch: output
          build_dir: dist
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```
This will generate an animated snake that eats your GitHub contributions!



