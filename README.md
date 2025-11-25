import React, { useState, useEffect, useRef } from 'react';

const PacManGame = () => {
  const canvasRef = useRef(null);
  const [score, setScore] = useState(0);
  const [gameStarted, setGameStarted] = useState(false);
  const [gameOver, setGameOver] = useState(false);

  // GitHub-style contribution grid (52 weeks × 7 days)
  const GRID_COLS = 52;
  const GRID_ROWS = 7;
  const CELL_SIZE = 12;
  const CELL_GAP = 3;
  const GRID_WIDTH = GRID_COLS * (CELL_SIZE + CELL_GAP);
  const GRID_HEIGHT = GRID_ROWS * (CELL_SIZE + CELL_GAP);

  useEffect(() => {
    const canvas = canvasRef.current;
    if (!canvas) return;

    const ctx = canvas.getContext('2d');
    canvas.width = GRID_WIDTH + 100;
    canvas.height = GRID_HEIGHT + 100;

    // Game state
    let contributions = [];
    let pacman = {
      x: 50,
      y: GRID_HEIGHT / 2 + 50,
      radius: 15,
      mouthAngle: 0.2,
      mouthSpeed: 0.1,
      direction: 0, // 0: right, 1: down, 2: left, 3: up
      speed: 2
    };

    let ghosts = [];
    let animationId;

    // Initialize contribution grid
    const initContributions = () => {
      contributions = [];
      const intensities = [0, 1, 2, 3, 4];
      const colors = ['#161b22', '#0e4429', '#006d32', '#26a641', '#39d353'];
      
      for (let col = 0; col < GRID_COLS; col++) {
        for (let row = 0; row < GRID_ROWS; row++) {
          const intensity = intensities[Math.floor(Math.random() * intensities.length)];
          contributions.push({
            x: col * (CELL_SIZE + CELL_GAP) + 50,
            y: row * (CELL_SIZE + CELL_GAP) + 50,
            size: CELL_SIZE,
            color: colors[intensity],
            eaten: false,
            intensity: intensity
          });
        }
      }
    };

    // Initialize ghosts
    const initGhosts = () => {
      const ghostColors = ['#ff0000', '#ffb8ff', '#00ffff', '#ffb852'];
      ghosts = ghostColors.map((color, i) => ({
        x: GRID_WIDTH - 100 + (i * 20),
        y: 50 + (i * 40),
        radius: 14,
        color: color,
        speed: 1 + Math.random() * 0.5,
        direction: Math.random() * Math.PI * 2
      }));
    };

    // Draw Pac-Man
    const drawPacMan = () => {
      const angle = pacman.direction * Math.PI / 2;
      
      ctx.fillStyle = '#ffff00';
      ctx.beginPath();
      ctx.arc(pacman.x, pacman.y, pacman.radius, 
        angle + pacman.mouthAngle, 
        angle + (Math.PI * 2 - pacman.mouthAngle));
      ctx.lineTo(pacman.x, pacman.y);
      ctx.fill();

      // Animate mouth
      pacman.mouthAngle += pacman.mouthSpeed;
      if (pacman.mouthAngle > 0.5 || pacman.mouthAngle < 0.1) {
        pacman.mouthSpeed *= -1;
      }
    };

    // Draw ghost
    const drawGhost = (ghost) => {
      ctx.fillStyle = ghost.color;
      
      // Body
      ctx.beginPath();
      ctx.arc(ghost.x, ghost.y, ghost.radius, Math.PI, 0);
      ctx.lineTo(ghost.x + ghost.radius, ghost.y + ghost.radius);
      
      // Wavy bottom
      for (let i = 0; i < 3; i++) {
        ctx.lineTo(ghost.x + ghost.radius - (i * 2 + 1) * (ghost.radius / 3), 
                   ghost.y + ghost.radius + (i % 2 === 0 ? 5 : 0));
      }
      ctx.lineTo(ghost.x - ghost.radius, ghost.y);
      ctx.fill();

      // Eyes
      ctx.fillStyle = '#fff';
      ctx.beginPath();
      ctx.arc(ghost.x - 5, ghost.y - 3, 4, 0, Math.PI * 2);
      ctx.arc(ghost.x + 5, ghost.y - 3, 4, 0, Math.PI * 2);
      ctx.fill();

      ctx.fillStyle = '#000';
      ctx.beginPath();
      ctx.arc(ghost.x - 5, ghost.y - 3, 2, 0, Math.PI * 2);
      ctx.arc(ghost.x + 5, ghost.y - 3, 2, 0, Math.PI * 2);
      ctx.fill();
    };

    // Draw contributions
    const drawContributions = () => {
      contributions.forEach(contrib => {
        if (!contrib.eaten) {
          ctx.fillStyle = contrib.color;
          ctx.fillRect(contrib.x, contrib.y, contrib.size, contrib.size);
        }
      });
    };

    // Update Pac-Man position
    const updatePacMan = () => {
      const dx = [pacman.speed, 0, -pacman.speed, 0];
      const dy = [0, pacman.speed, 0, -pacman.speed];
      
      pacman.x += dx[pacman.direction];
      pacman.y += dy[pacman.direction];

      // Wrap around edges
      if (pacman.x < 0) pacman.x = canvas.width;
      if (pacman.x > canvas.width) pacman.x = 0;
      if (pacman.y < 30) pacman.y = canvas.height - 20;
      if (pacman.y > canvas.height - 20) pacman.y = 30;

      // Check collision with contributions
      contributions.forEach(contrib => {
        if (!contrib.eaten) {
          const dist = Math.hypot(pacman.x - (contrib.x + contrib.size/2), 
                                  pacman.y - (contrib.y + contrib.size/2));
          if (dist < pacman.radius + contrib.size/2) {
            contrib.eaten = true;
            setScore(s => s + (contrib.intensity + 1) * 10);
          }
        }
      });

      // Check if all eaten
      if (contributions.every(c => c.eaten)) {
        setGameOver(true);
      }
    };

    // Update ghosts
    const updateGhosts = () => {
      ghosts.forEach(ghost => {
        // Simple AI: move towards Pac-Man with some randomness
        const angle = Math.atan2(pacman.y - ghost.y, pacman.x - ghost.x);
        ghost.direction = angle + (Math.random() - 0.5) * 0.5;
        
        ghost.x += Math.cos(ghost.direction) * ghost.speed;
        ghost.y += Math.sin(ghost.direction) * ghost.speed;

        // Wrap around
        if (ghost.x < 0) ghost.x = canvas.width;
        if (ghost.x > canvas.width) ghost.x = 0;
        if (ghost.y < 30) ghost.y = canvas.height - 20;
        if (ghost.y > canvas.height - 20) ghost.y = 30;

        // Check collision with Pac-Man
        const dist = Math.hypot(pacman.x - ghost.x, pacman.y - ghost.y);
        if (dist < pacman.radius + ghost.radius) {
          setGameOver(true);
        }
      });
    };

    // Main game loop
    const gameLoop = () => {
      if (!gameStarted || gameOver) return;

      ctx.fillStyle = '#0d1117';
      ctx.fillRect(0, 0, canvas.width, canvas.height);

      drawContributions();
      drawPacMan();
      ghosts.forEach(drawGhost);
      
      updatePacMan();
      updateGhosts();

      animationId = requestAnimationFrame(gameLoop);
    };

    // Handle keyboard input
    const handleKeyPress = (e) => {
      if (!gameStarted) return;
      
      switch(e.key) {
        case 'ArrowRight': pacman.direction = 0; break;
        case 'ArrowDown': pacman.direction = 1; break;
        case 'ArrowLeft': pacman.direction = 2; break;
        case 'ArrowUp': pacman.direction = 3; break;
      }
    };

    window.addEventListener('keydown', handleKeyPress);

    if (gameStarted && !gameOver) {
      initContributions();
      initGhosts();
      gameLoop();
    }

    return () => {
      window.removeEventListener('keydown', handleKeyPress);
      if (animationId) cancelAnimationFrame(animationId);
    };
  }, [gameStarted, gameOver]);

  const startGame = () => {
    setGameStarted(true);
    setGameOver(false);
    setScore(0);
  };

  const resetGame = () => {
    setGameStarted(false);
    setGameOver(false);
    setScore(0);
  };

  return (
    <div className="min-h-screen bg-gradient-to-br from-gray-900 via-purple-900 to-gray-900 flex flex-col items-center justify-center p-8">
      <div className="text-center mb-8">
        <h1 className="text-5xl font-bold mb-4 bg-gradient-to-r from-yellow-400 via-pink-500 to-purple-500 text-transparent bg-clip-text">
          🎮 Pac-Man Contribution Eater
        </h1>
        <p className="text-gray-300 text-lg">Use arrow keys to eat all contributions!</p>
      </div>

      <div className="bg-gray-800 rounded-2xl p-6 shadow-2xl border-2 border-purple-500">
        <div className="flex justify-between items-center mb-4 px-4">
          <div className="text-2xl font-bold text-yellow-400">
            Score: <span className="text-3xl">{score}</span>
          </div>
          <div className="text-lg text-gray-300">
            {gameOver ? (
              <span className="text-red-400 font-bold">Game Over!</span>
            ) : gameStarted ? (
              <span className="text-green-400">Playing...</span>
            ) : (
              <span>Ready!</span>
            )}
          </div>
        </div>

        <canvas 
          ref={canvasRef}
          className="border-4 border-purple-600 rounded-lg bg-gray-900"
        />

        <div className="mt-6 flex gap-4 justify-center">
          {!gameStarted || gameOver ? (
            <button
              onClick={startGame}
              className="px-8 py-3 bg-gradient-to-r from-green-500 to-emerald-600 hover:from-green-600 hover:to-emerald-700 text-white font-bold rounded-lg shadow-lg transform hover:scale-105 transition-all"
            >
              {gameOver ? '🔄 Play Again' : '▶️ Start Game'}
            </button>
          ) : (
            <button
              onClick={resetGame}
              className="px-8 py-3 bg-gradient-to-r from-red-500 to-pink-600 hover:from-red-600 hover:to-pink-700 text-white font-bold rounded-lg shadow-lg transform hover:scale-105 transition-all"
            >
              ⏹️ Reset
            </button>
          )}
        </div>

        <div className="mt-6 p-4 bg-gray-900 rounded-lg border border-purple-400">
          <h3 className="text-yellow-400 font-bold mb-2 text-center">🕹️ Controls</h3>
          <div className="grid grid-cols-2 gap-2 text-gray-300 text-sm">
            <div>⬆️ Up Arrow - Move Up</div>
            <div>⬇️ Down Arrow - Move Down</div>
            <div>⬅️ Left Arrow - Move Left</div>
            <div>➡️ Right Arrow - Move Right</div>
          </div>
          <div className="mt-3 text-center text-xs text-gray-400">
            Eat all contributions while avoiding the ghosts! 👻
          </div>
        </div>
      </div>

      {gameOver && (
        <div className="mt-8 text-center">
          <h2 className="text-3xl font-bold text-yellow-400 mb-2">
            {contributions.every(c => c.eaten) ? '🎉 You Won!' : '👻 Caught by Ghosts!'}
          </h2>
          <p className="text-gray-300 text-lg">Final Score: {score}</p>
        </div>
      )}
    </div>
  );
};

export default PacManGame;
