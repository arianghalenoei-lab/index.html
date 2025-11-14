👾 Platform Jumper: CodeHS Conversion link: https://arianghalenoei-lab.github.io/index.html/

This is a classic 2D platformer game converted from the CodeHS environment's proprietary graphics library into a single, self-contained HTML/JavaScript file using the standard Canvas API.

The goal is to navigate the landscape, jump on enemies to score points, and avoid being hit.

🕹️ How to Play

The game is controlled using common keyboard inputs:

Action

Keys

Move Left

Left Arrow (←) or A

Move Right

Right Arrow (→) or D

Jump

Up Arrow (↑) or W or Spacebar

Objective: Jump on top of the enemies to defeat them and earn 100 points per enemy. If an enemy touches you from the side, the game is over.

⚠️ Important Note on Images (CORS Issue)

This game attempts to load all sprite assets (player, enemies, ground, platforms, etc.) directly from https://codehs.com/uploads/ URLs.

Due to modern browser security restrictions (CORS), these images will likely fail to load when running the game outside the CodeHS platform
