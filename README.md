<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>Platformer (converted from CodeHS)</title>
<style>
html,body{height:100%;margin:0;background:#000;display:flex;align-items:center;justify-content:center}
#gameWrap{width:800px;height:600px;position:relative;box-shadow:0 10px 30px rgba(0,0,0,.5)}
canvas{display:block;background:#000}
.uiOverlay{position:absolute;left:0;top:0;right:0;pointer-events:none}
.center{position:absolute;left:0;right:0;margin:auto;text-align:center}
#startScreen{pointer-events:auto;background:rgba(0,0,0,0.35);height:100%;display:flex;align-items:center;justify-content:center;flex-direction:column}
#startButton{padding:12px 28px;background:#28a745;border-radius:6px;color:white;border:none;cursor:pointer;font-weight:700}
#scoreLabel{position:absolute;left:10px;top:10px;font:20px Arial;color:#fff;text-shadow:0 1px 0 #000}
#gameOver{position:absolute;left:0;right:0;top:220px;text-align:center;color:white;font:40px Arial;text-shadow:0 2px 0 #000}
#controls{position:absolute;left:10px;bottom:8px;color:#eee;font:12px Arial}
</style>
</head>
<body>
<div id="gameWrap">
<canvas id="game" width="800" height="600"></canvas>
<div class="uiOverlay">
<div id="scoreLabel">Score: 0</div>
<div id="controls">Move: ← → or A/D • Jump: ↑ or W or Space</div>
<div id="startScreen" style="display:flex;">
<img src="https://codehs.com/uploads/678a45130afe1cf1e2e78b7cfb3551aa" style="max-width:100%;height:auto;margin-bottom:18px">
<button id="startButton">START GAME</button>
</div>
<div id="gameOver" style="display:none">Game Over</div>
</div>
</div>


<script>
// --- Constants (kept similar to your CodeHS version) ---
const WIDTH = 800;
const HEIGHT = 600;
const GRAVITY = 0.7;
const JUMP_STRENGTH = 16;
const PLAYER_W = 40;
const PLAYER_H = 60;
const GROUND_H = 40;
const MOVE_SPEED = 10;
const ENEMY_W = 40;
const ENEMY_H = 40;


// --- Images (using your CodeHS URLs) ---
const imgSrc = {
player: 'https://codehs.com/uploads/161a979d935937cfe673c76765375c73',
ground: 'https://codehs.com/uploads/cc727d840c5e1888a62682d99f410a86',
sky: 'https://codehs.com/uploads/7d84713450bf26d8d40c78a9a2adcf88',
platform: 'https://codehs.com/uploads/66ef6792d60484b85bbf2a10ce930cae',
enemy: 'https://codehs.com/uploads/50e6dc724d06cfceed826839b19082ce',
enemyGround: 'https://codehs.com/uploads/f96ea9e2aadb96f99a5104f7ace99458'
};


// --- Canvas setup ---
const canvas = document.getElementById('game');
const ctx = canvas.getContext('2d');


let images = {};
let keys = {};


// Game objects
let player;
let platforms = [];
let enemies = [];
let groundY = HEIGHT - GROUND_H;


let isJumping = false;
let vy = 0;
let score = 0;
let gameOver = false;
let running = false;


// Preload images
let loadCount = 0;
const totalImages = Object.keys(imgSrc).length;
for (let k in imgSrc) {
const im = new Image();
im.src = imgSrc[k];
im.onload = () => { loadCount++; };
images[k] = im;
}


// Utility: rectangle collision
