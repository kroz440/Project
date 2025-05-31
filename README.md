// === Variables and Initial Setup ===
const player = document.getElementById('player');
const bullet = document.getElementById('bullet');
const missionsList = document.getElementById('missions-list');
const notifications = document.getElementById('notifications');
const playerLevel = document.getElementById('player-level');
const playerXP = document.getElementById('player-xp');
const playerWeapons = document.getElementById('player-weapons');
const playerSkills = document.getElementById('player-skills');

// Player state
let playerState = {
  x: 50,
  y: 50,
  speed: 5,
  level: 1,
  xp: 0,
  weapons: ['EMP Pistol'],
  skills: ['Basic Hacking'],
  canShoot: true,
  bulletSpeed: 10,
};

// === Function to update player position on screen ===
function updatePlayerPosition() {
  player.style.left = playerState.x + 'px';
  player.style.top = playerState.y + 'px';
}

// === Initial position update ===
updatePlayerPosition();

// === Keyboard Input Handling ===
const keysPressed = {};
document.addEventListener('keydown', (e) => {
  keysPressed[e.key.toLowerCase()] = true;
});
document.addEventListener('keyup', (e) => {
  keysPressed[e.key.toLowerCase()] = false;
});

// === Player Movement Loop ===
function playerMovement() {
  if (keysPressed['w'] || keysPressed['arrowup']) {
    playerState.y -= playerState.speed;
  }
  if (keysPressed['s'] || keysPressed['arrowdown']) {
    playerState.y += playerState.speed;
  }
  if (keysPressed['a'] || keysPressed['arrowleft']) {
    playerState.x -= playerState.speed;
  }
  if (keysPressed['d'] || keysPressed['arrowright']) {
    playerState.x += playerState.speed;
  }
  updatePlayerPosition();
  requestAnimationFrame(playerMovement);
}

// Start the movement loop
playerMovement();
// === Shooting System ===
function shootBullet() {
  if (!playerState.canShoot) return;

  playerState.canShoot = false;
  bullet.style.display = 'block';

  // Position bullet at player's current position
  bullet.style.left = (playerState.x + 15) + 'px'; // Adjust for bullet start position
  bullet.style.top = (playerState.y + 10) + 'px';

  let bulletX = playerState.x + 15;

  // Move bullet forward
  const bulletInterval = setInterval(() => {
    bulletX += playerState.bulletSpeed;
    bullet.style.left = bulletX + 'px';

    // If bullet goes off screen, hide it and clear interval
    if (bulletX > window.innerWidth) {
      bullet.style.display = 'none';
      clearInterval(bulletInterval);
      playerState.canShoot = true;
    }
  }, 16); // roughly 60fps
}

// Listen for space key to shoot
document.addEventListener('keydown', (e) => {
  if (e.key === ' ') {
    shootBullet();
  }
});
// === Player Movement System ===
document.addEventListener('keydown', (e) => {
  switch (e.key) {
    case 'ArrowUp':
      if (playerState.y > 0) playerState.y -= playerState.speed;
      break;
    case 'ArrowDown':
      if (playerState.y < window.innerHeight - 50) playerState.y += playerState.speed;
      break;
    case 'ArrowLeft':
      if (playerState.x > 0) playerState.x -= playerState.speed;
      break;
    case 'ArrowRight':
      if (playerState.x < window.innerWidth - 50) playerState.x += playerState.speed;
      break;
  }
  updatePlayerPosition();
});

function updatePlayerPosition() {
  player.style.top = playerState.y + 'px';
  player.style.left = playerState.x + 'px';
}

// === Initialize player state ===
const playerState = {
  x: 100,
  y: 100,
  speed: 5,
  canShoot: true,
  bulletSpeed: 10,
};

// Initial position update
updatePlayerPosition();
// === Shooting System ===
document.addEventListener('keydown', (e) => {
  if (e.key === ' ') {  // Spacebar to shoot
    shootBullet();
  }
});

function shootBullet() {
  if (!playerState.canShoot) return;
  playerState.canShoot = false;

  bullet.style.display = 'block';
  bullet.style.top = playerState.y + 20 + 'px';
  bullet.style.left = playerState.x + 40 + 'px';

  let bulletX = playerState.x + 40;
  const bulletInterval = setInterval(() => {
    bulletX += playerState.bulletSpeed;
    bullet.style.left = bulletX + 'px';

    if (bulletX > window.innerWidth) {
      clearInterval(bulletInterval);
      bullet.style.display = 'none';
      playerState.canShoot = true;
    }

    // TODO: Add collision detection with enemies here

  }, 20);
}

// === Missions System ===
const missions = [
  { id: 1, title: "Infiltrate Nova Tech Base", completed: false },
  { id: 2, title: "Rescue Hostage in Black Forest", completed: false },
  { id: 3, title: "Disable Government Surveillance", completed: false },
];

function loadMissions() {
  const missionsList = document.getElementById('missions-list');
  missionsList.innerHTML = '';
  missions.forEach(mission => {
    const li = document.createElement('li');
    li.textContent = mission.title + (mission.completed ? " ✅" : "");
    missionsList.appendChild(li);
  });
}

loadMissions();
// === Player Development System ===
const playerState = {
  x: 100,
  y: 300,
  speed: 5,
  level: 1,
  xp: 0,
  xpToLevelUp: 100,
  weapons: ['EMP Pistol'],
  skills: ['Basic Hacking'],
  canShoot: true,
  bulletSpeed: 15
};

function gainXP(amount) {
  playerState.xp += amount;
  if (playerState.xp >= playerState.xpToLevelUp) {
    levelUp();
  }
  updatePlayerStatsUI();
}

function levelUp() {
  playerState.level++;
  playerState.xp = playerState.xp - playerState.xpToLevelUp;
  playerState.xpToLevelUp = Math.floor(playerState.xpToLevelUp * 1.5);
  // Unlock new weapon or skill at certain levels
  if (playerState.level === 3) {
    playerState.weapons.push('Pulse Rifle');
  }
  if (playerState.level === 5) {
    playerState.skills.push('Advanced Hacking');
  }
  // Notify player about level up
  alert(`Level Up! You are now level ${playerState.level}`);
}

function updatePlayerStatsUI() {
  document.getElementById('player-level').textContent = playerState.level;
  document.getElementById('player-xp').textContent = playerState.xp;
  document.getElementById('player-weapons').textContent = playerState.weapons.join(', ');
  document.getElementById('player-skills').textContent = playerState.skills.join(', ');
}

updatePlayerStatsUI();
// === Player Movement and Controls ===
const keysPressed = {};

window.addEventListener('keydown', (e) => {
  keysPressed[e.key.toLowerCase()] = true;
});

window.addEventListener('keyup', (e) => {
  keysPressed[e.key.toLowerCase()] = false;
});

function movePlayer() {
  if (keysPressed['w'] || keysPressed['arrowup']) {
    playerState.y -= playerState.speed;
  }
  if (keysPressed['s'] || keysPressed['arrowdown']) {
    playerState.y += playerState.speed;
  }
  if (keysPressed['a'] || keysPressed['arrowleft']) {
    playerState.x -= playerState.speed;
  }
  if (keysPressed['d'] || keysPressed['arrowright']) {
    playerState.x += playerState.speed;
  }
  const playerElement = document.getElementById('player');
  playerElement.style.top = playerState.y + 'px';
  playerElement.style.left = playerState.x + 'px';
}

// === Shooting Mechanism ===
function shootBullet() {
  if (!playerState.canShoot) return;
  playerState.canShoot = false;
  const bullet = document.getElementById('bullet');
  bullet.style.display = 'block';
  bullet.style.top = (playerState.y + 15) + 'px'; // Adjust for player center
  bullet.style.left = (playerState.x + 40) + 'px';

  let bulletX = playerState.x + 40;
  const bulletInterval = setInterval(() => {
    bulletX += playerState.bulletSpeed;
    bullet.style.left = bulletX + 'px';
    // Remove bullet if it goes off screen
    if (bulletX > window.innerWidth) {
      bullet.style.display = 'none';
      clearInterval(bulletInterval);
      playerState.canShoot = true;
    }
  }, 20);
}

window.addEventListener('keydown', (e) => {
  if (e.key.toLowerCase() === ' ') {
    shootBullet();
  }
});

// Game loop to update movement
function gameLoop() {
  movePlayer();
  requestAnimationFrame(gameLoop);
}

gameLoop();
// === Missions System ===
const missions = [
  {
    id: 1,
    title: "Infiltrate Black Forest",
    description: "Enter the Black Forest and retrieve the encrypted data.",
    completed: false,
    rewardXP: 100,
  },
  {
    id: 2,
    title: "Sabotage Nova Tech Base",
    description: "Plant EMP devices to disable enemy communications.",
    completed: false,
    rewardXP: 150,
  },
  {
    id: 3,
    title: "Protect Resistance Camps",
    description: "Defend the camps from government raids.",
    completed: false,
    rewardXP: 120,
  },
];

function loadMissions() {
  const missionsList = document.getElementById('missions-list');
  missionsList.innerHTML = ''; // Clear current missions

  missions.forEach((mission) => {
    const li = document.createElement('li');
    li.textContent = mission.title + (mission.completed ? " (Completed)" : "");
    li.style.cursor = "pointer";
    li.onclick = () => {
      alert(mission.description);
    };
    missionsList.appendChild(li);
  });
}

// === Player Stats and Level Up ===
const playerState = {
  x: 100,
  y: 100,
  speed: 5,
  bulletSpeed: 10,
  canShoot: true,
  xp: 0,
  level: 1,
};

function addXP(amount) {
  playerState.xp += amount;
  if (playerState.xp >= playerState.level * 100) {
    playerState.xp = 0;
    playerState.level++;
    alert(`Level up! You reached level ${playerState.level}`);
    document.getElementById('player-level').textContent = playerState.level;
  }
  document.getElementById('player-xp').textContent = playerState.xp;
}

// === Initialize Missions on Load ===
window.onload = () => {
  loadMissions();
  // Initialize player position
  const player = document.getElementById('player');
  player.style.position = 'absolute';
  player.style.top = playerState.y + 'px';
  player.style.left = playerState.x + 'px';
};

// === Player Movement ===
document.addEventListener('keydown', (event) => {
  const player = document.getElementById('player');
  switch (event.key) {
    case 'ArrowUp':
    case 'w':
      playerState.y = Math.max(0, playerState.y - playerState.speed);
      break;
    case 'ArrowDown':
    case 's':
      playerState.y = Math.min(window.innerHeight - player.offsetHeight, playerState.y + playerState.speed);
      break;
    case 'ArrowLeft':
    case 'a':
      playerState.x = Math.max(0, playerState.x - playerState.speed);
      break;
    case 'ArrowRight':
    case 'd':
      playerState.x = Math.min(window.innerWidth - player.offsetWidth, playerState.x + playerState.speed);
      break;
    case ' ':
      shootBullet();
      break;
  }
  player.style.top = playerState.y + 'px';
  player.style.left = playerState.x + 'px';
});

// === Bullet Shooting ===
function shootBullet() {
  if (!playerState.canShoot) return;
  playerState.canShoot = false;

  const bullet = document.getElementById('bullet');
  bullet.style.display = 'block';
  bullet.style.position = 'absolute';
  bullet.style.top = (playerState.y + 20) + 'px'; // bullet spawn near player
  bullet.style.left = (playerState.x + 30) + 'px';

  let bulletX = playerState.x + 30;
  const bulletInterval = setInterval(() => {
    if (bulletX > window.innerWidth) {
      clearInterval(bulletInterval);
      bullet.style.display = 'none';
      playerState.canShoot = true;
      return;
    }
    bulletX += playerState.bulletSpeed;
    bullet.style.left = bulletX + 'px';
  }, 20);
}

// === Gangs Interaction ===
const gangs = [
  { name: "Black Guard", description: "Elite mercenaries controlling the black market." },
  { name: "Hackers Hunters", description: "Specialized team hunting rogue hackers." },
  { name: "Red Shield", description: "Rebel faction fighting government oppression." },
];

function loadGangs() {
  const gangsList = document.getElementById('gangs-list');
  gangsList.innerHTML = '';
  gangs.forEach(gang => {
    const li = document.createElement('li');
    li.textContent = gang.name;
    li.style.cursor = "pointer";
    li.onclick = () => alert(gang.description);
    gangsList.appendChild(li);
  });
}

window.onload = () => {
  loadMissions();
  loadGangs();
  const player = document.getElementById('player');
  player.style.position = 'absolute';
  player.style.top = playerState.y + 'px';
  player.style.left = playerState.x + 'px';
};
// === Gang Interaction System ===

// Each gang has reputation, missions, and special perks
const gangData = {
  "Black Guard": {
    reputation: 0,
    missionsCompleted: 0,
    perks: ["Black Market Access", "Stealth Support"],
  },
  "Hackers Hunters": {
    reputation: 0,
    missionsCompleted: 0,
    perks: ["Advanced Intel", "Cyber Defense"],
  },
  "Red Shield": {
    reputation: 0,
    missionsCompleted: 0,
    perks: ["Rebel Backup", "Improved Weapons"],
  },
};

// Function to increase reputation with a gang
function increaseReputation(gangName, amount) {
  if (gangData[gangName]) {
    gangData[gangName].reputation += amount;
    updateGangUI(gangName);
  }
}

// Function to update gang UI
function updateGangUI(gangName) {
  const gangsList = document.getElementById('gangs-list');
  gangsList.childNodes.forEach((li) => {
    if (li.textContent === gangName) {
      li.textContent = `${gangName} (Rep: ${gangData[gangName].reputation})`;
      if (gangData[gangName].reputation >= 10) {
        li.style.fontWeight = "bold";
        li.title = "Unlocked perks: " + gangData[gangName].perks.join(", ");
      }
    }
  });
}

// Function to complete a mission for a gang
function completeGangMission(gangName) {
  if (gangData[gangName]) {
    gangData[gangName].missionsCompleted += 1;
    increaseReputation(gangName, 5);
    showNotification(`Mission completed for ${gangName}. Reputation increased!`);
  }
}

// Notification helper
function showNotification(message) {
  const notifications = document.getElementById('notifications');
  const notification = document.createElement('div');
  notification.className = 'notification';
  notification.textContent = message;
  notifications.appendChild(notification);
  setTimeout(() => {
    notifications.removeChild(notification);
  }, 4000);
}

// Initialize gangs UI with reputation
function initGangUI() {
  const gangsList = document.getElementById('gangs-list');
  gangsList.innerHTML = '';
  for (const gangName in gangData) {
    const li = document.createElement('li');
    li.textContent = `${gangName} (Rep: ${gangData[gangName].reputation})`;
    li.style.cursor = "pointer";
    li.onclick = () => alert(`${gangName}\nPerks: ${gangData[gangName].perks.join(", ")}\nReputation: ${gangData[gangName].reputation}`);
    gangsList.appendChild(li);
  }
}

window.onload = () => {
  loadMissions();
  initGangUI();
  loadGangs(); // in case some old code still needed, can be removed after cleanup
  const player = document.getElementById('player');
  player.style.position = 'absolute';
  player.style.top = playerState.y + 'px';
  player.style.left = playerState.x + 'px';
};
// === Notifications System ===

const notificationsContainer = document.getElementById('notifications');

function showNotification(message, duration = 3000) {
  const notif = document.createElement('div');
  notif.className = 'notification';
  notif.textContent = message;
  notificationsContainer.appendChild(notif);

  setTimeout(() => {
    notif.classList.add('fade-out');
    notif.addEventListener('transitionend', () => {
      notificationsContainer.removeChild(notif);
    });
  }, duration);
}

// Example usage
// showNotification("New mission unlocked!", 4000);

// === Settings Handlers ===

const settingsPanel = document.getElementById('settings-panel');
const volumeControl = document.getElementById('volume-control');
const brightnessControl = document.getElementById('brightness-control');
const difficultySelect = document.getElementById('difficulty-select');
const applySettingsBtn = document.getElementById('apply-settings');
const closeSettingsBtn = document.getElementById('close-settings');

applySettingsBtn.addEventListener('click', () => {
  const volume = volumeControl.value;
  const brightness = brightnessControl.value;
  const difficulty = difficultySelect.value;

  // Apply settings logic here
  document.body.style.filter = `brightness(${brightness}%)`;
  // Volume setting would affect game sounds, to be implemented

  showNotification(`Settings applied: Volume ${volume}%, Brightness ${brightness}%, Difficulty ${difficulty}`, 4000);
});

closeSettingsBtn.addEventListener('click', () => {
  settingsPanel.style.display = 'none';
});

// Function to open settings panel
function openSettings() {
  settingsPanel.style.display = 'block';
  showNotification("Settings opened", 2000);
}

// === Keyboard Controls for Player Movement ===

const keysPressed = {};

window.addEventListener('keydown', (e) => {
  keysPressed[e.key.toLowerCase()] = true;
});

window.addEventListener('keyup', (e) => {
  keysPressed[e.key.toLowerCase()] = false;
});

function handlePlayerMovement() {
  const speed = 5;
  if (keysPressed['arrowup'] || keysPressed['w']) {
    playerState.y -= speed;
  }
  if (keysPressed['arrowdown'] || keysPressed['s']) {
    playerState.y += speed;
  }
  if (keysPressed['arrowleft'] || keysPressed['a']) {
    playerState.x -= speed;
  }
  if (keysPressed['arrowright'] || keysPressed['d']) {
    playerState.x += speed;
  }

  // Boundaries check (game-container size assumed 800x600 for example)
  playerState.x = Math.max(0, Math.min(playerState.x, 800 - 50));
  playerState.y = Math.max(0, Math.min(playerState.y, 600 - 50));

  const player = document.getElementById('player');
  player.style.left = playerState.x + 'px';
  player.style.top = playerState.y + 'px';
}

// Start player movement loop
setInterval(handlePlayerMovement, 30);
// === Bullet Mechanics ===

let bulletActive = false;
const bulletSpeed = 15;

function shootBullet() {
  if (bulletActive) return; // Only one bullet at a time for simplicity

  bulletActive = true;
  const bullet = document.getElementById('bullet');
  bullet.style.display = 'block';
  bullet.style.left = playerState.x + 20 + 'px'; // Position bullet near player center
  bullet.style.top = playerState.y + 20 + 'px';

  let bulletX = playerState.x + 20;
  let bulletY = playerState.y + 20;

  // Direction bullet will move (to the right for example)
  const bulletDirectionX = 1;
  const bulletDirectionY = 0;

  const bulletInterval = setInterval(() => {
    bulletX += bulletDirectionX * bulletSpeed;
    bulletY += bulletDirectionY * bulletSpeed;

    bullet.style.left = bulletX + 'px';
    bullet.style.top = bulletY + 'px';

    // Check if bullet is out of bounds (assuming 800x600)
    if (bulletX > 800 || bulletX < 0 || bulletY > 600 || bulletY < 0) {
      clearInterval(bulletInterval);
      bullet.style.display = 'none';
      bulletActive = false;
    }

    // TODO: Add collision detection with enemies or map zones here

  }, 30);
}

// Listen for spacebar to shoot
window.addEventListener('keydown', (e) => {
  if (e.key === ' ' || e.code === 'Space') {
    shootBullet();
  }
});

// === Initialize Player Position ===

const playerState = {
  x: 375, // Centered initially (800/2 - player width/2)
  y: 275, // Centered initially (600/2 - player height/2)
};

const player = document.getElementById('player');
player.style.position = 'absolute';
player.style.width = '50px';
player.style.height = '50px';
player.style.left = playerState.x + 'px';
player.style.top = playerState.y + 'px';
player.style.backgroundColor = '#00f'; // Temporary color for player box

const bullet = document.getElementById('bullet');
bullet.style.position = 'absolute';
bullet.style.width = '10px';
bullet.style.height = '5px';
bullet.style.backgroundColor = '#ff0'; // Temporary bullet color
// === Enemy AI System ===

class Enemy {
  constructor(id, x, y, patrolPath = []) {
    this.id = id;
    this.x = x;
    this.y = y;
    this.width = 40;
    this.height = 40;
    this.patrolPath = patrolPath; // Array of points [{x, y}, ...]
    this.currentPatrolIndex = 0;
    this.speed = 2;
    this.element = null;
    this.isAlive = true;
  }

  createElement() {
    const enemyDiv = document.createElement('div');
    enemyDiv.id = this.id;
    enemyDiv.className = 'enemy';
    enemyDiv.style.position = 'absolute';
    enemyDiv.style.width = this.width + 'px';
    enemyDiv.style.height = this.height + 'px';
    enemyDiv.style.backgroundColor = '#f00'; // Red enemy box
    enemyDiv.style.left = this.x + 'px';
    enemyDiv.style.top = this.y + 'px';
    document.getElementById('game-container').appendChild(enemyDiv);
    this.element = enemyDiv;
  }

  move() {
    if (this.patrolPath.length === 0) return;

    const target = this.patrolPath[this.currentPatrolIndex];
    const dx = target.x - this.x;
    const dy = target.y - this.y;
    const dist = Math.sqrt(dx * dx + dy * dy);

    if (dist < this.speed) {
      // Arrived at target
      this.x = target.x;
      this.y = target.y;
      this.currentPatrolIndex = (this.currentPatrolIndex + 1) % this.patrolPath.length;
    } else {
      // Move toward target
      this.x += (dx / dist) * this.speed;
      this.y += (dy / dist) * this.speed;
    }
    this.updatePosition();
  }

  updatePosition() {
    if (!this.element) return;
    this.element.style.left = this.x + 'px';
    this.element.style.top = this.y + 'px';
  }

  takeDamage() {
    this.isAlive = false;
    if (this.element) {
      this.element.style.backgroundColor = '#555'; // Dead enemy color
      this.element.style.opacity = '0.5';
    }
  }
}

// === Instantiate Enemies ===

const enemies = [];

const enemy1 = new Enemy('enemy1', 100, 100, [
  { x: 100, y: 100 },
  { x: 300, y: 100 },
  { x: 300, y: 300 },
  { x: 100, y: 300 },
]);
enemy1.createElement();
enemies.push(enemy1);

const enemy2 = new Enemy('enemy2', 500, 150, [
  { x: 500, y: 150 },
  { x: 600, y: 250 },
  { x: 500, y: 350 },
]);
enemy2.createElement();
enemies.push(enemy2);

// === Enemy Movement Loop ===

function enemyMovementLoop() {
  enemies.forEach((enemy) => {
    if (enemy.isAlive) {
      enemy.move();
    }
  });
}

setInterval(enemyMovementLoop, 50);

// === Bullet Collision with Enemies ===

function checkBulletCollision() {
  if (!bulletActive) return;
  const bulletRect = bullet.getBoundingClientRect();

  enemies.forEach((enemy) => {
    if (!enemy.isAlive) return;
    const enemyRect = enemy.element.getBoundingClientRect();

    if (
      bulletRect.left < enemyRect.right &&
      bulletRect.right > enemyRect.left &&
      bulletRect.top < enemyRect.bottom &&
      bulletRect.bottom > enemyRect.top
    ) {
      // Collision detected
      enemy.takeDamage();
      bullet.style.display = 'none';
      bulletActive = false;
    }
  });
}

setInterval(checkBulletCollision, 30);
// === Missions System ===

const missions = [
  {
    id: 1,
    title: "Infiltrate Nova Tech Base",
    description: "Gain access to the Nova Tech Base and retrieve secret data.",
    isCompleted: false,
    rewardXP: 150,
  },
  {
    id: 2,
    title: "Sabotage Government Centers",
    description: "Disable security systems at the Government & Financial Centers.",
    isCompleted: false,
    rewardXP: 200,
  },
  {
    id: 3,
    title: "Protect Resistance Camps",
    description: "Defend the Resistance Camps from enemy attack.",
    isCompleted: false,
    rewardXP: 100,
  },
];

function displayMissions() {
  const missionsList = document.getElementById("missions-list");
  missionsList.innerHTML = "";
  missions.forEach((mission) => {
    const li = document.createElement("li");
    li.textContent = mission.title + (mission.isCompleted ? " (Completed)" : "");
    li.style.color = mission.isCompleted ? "green" : "white";
    li.onclick = () => {
      alert(mission.description);
    };
    missionsList.appendChild(li);
  });
}

displayMissions();

// === Mission Completion Logic ===

function completeMission(missionId) {
  const mission = missions.find((m) => m.id === missionId);
  if (mission && !mission.isCompleted) {
    mission.isCompleted = true;
    // Add XP to player
    playerXP += mission.rewardXP;
    document.getElementById("player-xp").textContent = playerXP;
    displayMissions();
    showNotification(`Mission "${mission.title}" completed! +${mission.rewardXP} XP`);
  }
}

// === Notifications System ===

function showNotification(message) {
  const notifications = document.getElementById("notifications");
  const notif = document.createElement("div");
  notif.className = "notification";
  notif.textContent = message;
  notifications.appendChild(notif);

  setTimeout(() => {
    notifications.removeChild(notif);
  }, 4000);
}

// === Player XP and Leveling System ===

let playerLevel = 1;
let playerXP = 0;
const xpToNextLevel = 500;

function checkLevelUp() {
  if (playerXP >= xpToNextLevel) {
    playerLevel++;
    playerXP -= xpToNextLevel;
    document.getElementById("player-level").textContent = playerLevel;
    document.getElementById("player-xp").textContent = playerXP;
    showNotification(`Level up! You reached Level ${playerLevel}`);
  }
}

setInterval(checkLevelUp, 3000);
// === Missions Rewards & Player Upgrades ===

function upgradePlayerWeapon(newWeapon) {
  const weaponsSpan = document.getElementById("player-weapons");
  weaponsSpan.textContent = newWeapon;
  showNotification(`Weapon upgraded to ${newWeapon}!`);
}

function upgradePlayerSkill(newSkill) {
  const skillsSpan = document.getElementById("player-skills");
  skillsSpan.textContent = newSkill;
  showNotification(`Skill acquired: ${newSkill}!`);
}

function grantMissionRewards(missionId) {
  const mission = missions.find(m => m.id === missionId);
  if (!mission || !mission.isCompleted) return;

  // Example upgrades based on mission
  if (mission.id === 1) {
    upgradePlayerWeapon("EMP Rifle");
  } else if (mission.id === 2) {
    upgradePlayerSkill("Advanced Hacking");
  } else if (mission.id === 3) {
    upgradePlayerWeapon("Energy Shield");
  }
}

// === Enemy AI - Basic Implementation ===

class Enemy {
  constructor(name, health, attackPower, position) {
    this.name = name;
    this.health = health;
    this.attackPower = attackPower;
    this.position = position; // {x, y}
    this.isAlive = true;
  }

  takeDamage(damage) {
    this.health -= damage;
    if (this.health <= 0) {
      this.isAlive = false;
      this.health = 0;
      showNotification(`${this.name} defeated!`);
    }
  }

  attack(target) {
    if (!this.isAlive) return;
    // Simple attack logic: reduce target HP (assuming target has health)
    if (target.health !== undefined) {
      target.health -= this.attackPower;
      showNotification(`${this.name} attacks! ${this.attackPower} damage dealt.`);
      if (target.health <= 0) {
        target.health = 0;
        showNotification(`You died! Game Over.`);
        // Trigger game over screen here
      }
    }
  }

  moveTowards(targetPosition) {
    if (!this.isAlive) return;
    // Move enemy closer to targetPosition by 1 unit per call (simple logic)
    if (this.position.x < targetPosition.x) this.position.x++;
    else if (this.position.x > targetPosition.x) this.position.x--;

    if (this.position.y < targetPosition.y) this.position.y++;
    else if (this.position.y > targetPosition.y) this.position.y--;
  }
}

// Example player object for AI targetting
const player = {
  health: 100,
  position: { x: 10, y: 10 }
};

// Example enemies array
const enemies = [
  new Enemy("Black Guard Soldier", 50, 10, { x: 0, y: 0 }),
  new Enemy("Hackers Hunter Drone", 40, 15, { x: 20, y: 5 }),
];

// Basic enemy AI loop (called every second)
function enemyAILoop() {
  enemies.forEach(enemy => {
    if (enemy.isAlive) {
      enemy.moveTowards(player.position);
      // If enemy close enough, attack player
      const dx = Math.abs(enemy.position.x - player.position.x);
      const dy = Math.abs(enemy.position.y - player.position.y);
      if (dx <= 1 && dy <= 1) {
        enemy.attack(player);
      }
    }
  });
}

setInterval(enemyAILoop, 1000);
// === Player Controls and Shooting Mechanics ===

const playerElement = document.getElementById("player");
const bulletElement = document.getElementById("bullet");

let playerPosition = { x: 100, y: 100 }; // Starting position
let bulletPosition = { x: 0, y: 0 };
let bulletSpeed = 10;
let bulletActive = false;
let bulletDirection = { x: 0, y: 0 };

// Update player position on screen
function updatePlayerPosition() {
  playerElement.style.left = playerPosition.x + "px";
  playerElement.style.top = playerPosition.y + "px";
}

// Update bullet position on screen
function updateBulletPosition() {
  if (!bulletActive) return;
  bulletElement.style.left = bulletPosition.x + "px";
  bulletElement.style.top = bulletPosition.y + "px";
}

// Handle key presses for movement and shooting
document.addEventListener("keydown", function (event) {
  const key = event.key.toLowerCase();

  switch (key) {
    case "w": // Move up
      playerPosition.y -= 10;
      break;
    case "s": // Move down
      playerPosition.y += 10;
      break;
    case "a": // Move left
      playerPosition.x -= 10;
      break;
    case "d": // Move right
      playerPosition.x += 10;
      break;
    case " ": // Space to shoot
      if (!bulletActive) shootBullet();
      break;
  }
  updatePlayerPosition();
});

// Shoot bullet from player position in a fixed direction (right for example)
function shootBullet() {
  bulletActive = true;
  bulletPosition.x = playerPosition.x + 20; // start just in front of player
  bulletPosition.y = playerPosition.y + 10;
  bulletDirection = { x: 1, y: 0 }; // moving right
  bulletElement.style.display = "block";
  moveBullet();
}

// Move bullet continuously until off-screen or hits enemy
function moveBullet() {
  if (!bulletActive) return;

  bulletPosition.x += bulletSpeed * bulletDirection.x;
  bulletPosition.y += bulletSpeed * bulletDirection.y;
  updateBulletPosition();

  // Check collision with enemies
  enemies.forEach(enemy => {
    if (
      enemy.isAlive &&
      bulletPosition.x >= enemy.position.x &&
      bulletPosition.x <= enemy.position.x + 30 &&
      bulletPosition.y >= enemy.position.y &&
      bulletPosition.y <= enemy.position.y + 30
    ) {
      enemy.takeDamage(20);
      deactivateBullet();
    }
  });

  // If bullet goes off screen (example width 800px), deactivate
  if (bulletPosition.x > 800 || bulletPosition.x < 0 || bulletPosition.y > 600 || bulletPosition.y < 0) {
    deactivateBullet();
  } else {
    requestAnimationFrame(moveBullet);
  }
}

function deactivateBullet() {
  bulletActive = false;
  bulletElement.style.display = "none";
}

// Initialize positions on load
updatePlayerPosition();
updateBulletPosition();
// === Dynamic Missions System ===

const missionsListElement = document.getElementById("missions-list");

let missions = [
  {
    id: 1,
    title: "Infiltrate Black Forest",
    description: "Enter the Black Forest and gather intel on enemy movements.",
    isCompleted: false,
  },
  {
    id: 2,
    title: "Sabotage Nova Tech Base",
    description: "Disable the power generators at Nova Tech Base.",
    isCompleted: false,
  },
  {
    id: 3,
    title: "Protect Resistance Camps",
    description: "Defend the Resistance Camps from incoming attacks.",
    isCompleted: false,
  },
];

// Render missions dynamically
function renderMissions() {
  missionsListElement.innerHTML = ""; // Clear existing
  missions.forEach((mission) => {
    const li = document.createElement("li");
    li.textContent = `${mission.title} - ${mission.isCompleted ? "Completed" : "In Progress"}`;
    if (!mission.isCompleted) {
      const completeBtn = document.createElement("button");
      completeBtn.textContent = "Complete";
      completeBtn.onclick = () => completeMission(mission.id);
      li.appendChild(completeBtn);
    }
    missionsListElement.appendChild(li);
  });
}

// Mark a mission as completed
function completeMission(id) {
  const mission = missions.find((m) => m.id === id);
  if (mission) {
    mission.isCompleted = true;
    renderMissions();
    showNotification(`Mission "${mission.title}" completed!`);
  }
}

// Simple notification system
const notificationsElement = document.getElementById("notifications");
function showNotification(message) {
  const notif = document.createElement("div");
  notif.className = "notification";
  notif.textContent = message;
  notificationsElement.appendChild(notif);
  setTimeout(() => {
    notificationsElement.removeChild(notif);
  }, 3000);
}

// Initial render
renderMissions();
// === Enemy AI Basic Framework ===

class Enemy {
  constructor(id, name, health, attackPower, position) {
    this.id = id;
    this.name = name;
    this.health = health;
    this.attackPower = attackPower;
    this.position = position; // {x: number, y: number}
    this.isAlive = true;
  }

  // Move enemy towards player
  moveTowards(playerPosition) {
    if (!this.isAlive) return;

    const dx = playerPosition.x - this.position.x;
    const dy = playerPosition.y - this.position.y;
    const dist = Math.sqrt(dx * dx + dy * dy);

    if (dist > 0) {
      this.position.x += dx / dist; // move 1 unit towards player
      this.position.y += dy / dist;
    }
  }

  // Attack player
  attack(player) {
    if (!this.isAlive) return;

    const distanceToPlayer = Math.hypot(
      player.position.x - this.position.x,
      player.position.y - this.position.y
    );

    if (distanceToPlayer < 2) {
      player.takeDamage(this.attackPower);
      showNotification(`${this.name} attacked you! -${this.attackPower} HP`);
    }
  }

  // Take damage from player or other sources
  takeDamage(amount) {
    this.health -= amount;
    if (this.health <= 0) {
      this.isAlive = false;
      showNotification(`${this.name} was defeated!`);
    }
  }
}

// Example player object (for AI interaction)
const player = {
  position: { x: 10, y: 10 },
  health: 100,
  takeDamage(amount) {
    this.health -= amount;
    if (this.health <= 0) {
      showNotification("You died! Game Over.");
      // Trigger game over logic here
    }
  },
};

// Example: create enemies array and update their behavior each game tick
const enemies = [
  new Enemy(1, "Black Guard Soldier", 50, 10, { x: 5, y: 5 }),
  new Enemy(2, "Hackers Hunter", 40, 8, { x: 15, y: 8 }),
];

// Game loop update for enemies
function updateEnemies() {
  enemies.forEach((enemy) => {
    if (enemy.isAlive) {
      enemy.moveTowards(player.position);
      enemy.attack(player);
    }
  });
}

// Start a simple interval game loop (example, 30 FPS)
setInterval(() => {
  updateEnemies();
  // Other game update logic can go here
}, 1000 / 30);
// === Advanced Enemy AI Behaviors ===

// Enemy patrol behavior along waypoints
class PatrollingEnemy extends Enemy {
  constructor(id, name, health, attackPower, position, waypoints) {
    super(id, name, health, attackPower, position);
    this.waypoints = waypoints; // Array of {x, y} points
    this.currentWaypointIndex = 0;
    this.patrolSpeed = 1.5;
  }

  patrol() {
    if (!this.isAlive) return;

    const target = this.waypoints[this.currentWaypointIndex];
    const dx = target.x - this.position.x;
    const dy = target.y - this.position.y;
    const dist = Math.sqrt(dx * dx + dy * dy);

    if (dist < 0.5) {
      this.currentWaypointIndex =
        (this.currentWaypointIndex + 1) % this.waypoints.length;
    } else {
      this.position.x += (dx / dist) * this.patrolSpeed;
      this.position.y += (dy / dist) * this.patrolSpeed;
    }
  }

  update(playerPosition) {
    if (!this.isAlive) return;

    const distanceToPlayer = Math.hypot(
      playerPosition.x - this.position.x,
      playerPosition.y - this.position.y
    );

    if (distanceToPlayer < 8) {
      // Chase player if close
      this.moveTowards(playerPosition);
      this.attack({ ...player, takeDamage: player.takeDamage.bind(player) });
    } else {
      // Otherwise patrol
      this.patrol();
    }
  }
}

// Example patrolling enemy
const patrollingEnemy = new PatrollingEnemy(
  3,
  "Red Shield Scout",
  60,
  12,
  { x: 20, y: 20 },
  [
    { x: 20, y: 20 },
    { x: 25, y: 20 },
    { x: 25, y: 25 },
    { x: 20, y: 25 },
  ]
);

function updateEnemiesAdvanced() {
  enemies.forEach((enemy) => {
    if (enemy.isAlive) {
      if (enemy instanceof PatrollingEnemy) {
        enemy.update(player.position);
      } else {
        enemy.moveTowards(player.position);
        enemy.attack(player);
      }
    }
  });
}

// Adjust game loop for advanced enemies
setInterval(() => {
  updateEnemiesAdvanced();
  // Other game logic here
}, 1000 / 30);
// === Player Controls and Shooting System ===

const keysPressed = {};
document.addEventListener("keydown", (e) => {
  keysPressed[e.key.toLowerCase()] = true;
});
document.addEventListener("keyup", (e) => {
  keysPressed[e.key.toLowerCase()] = false;
});

const bulletSpeed = 10;
let bulletActive = false;

function movePlayer() {
  const speed = 5;
  if (keysPressed["w"] || keysPressed["arrowup"]) player.position.y -= speed;
  if (keysPressed["s"] || keysPressed["arrowdown"]) player.position.y += speed;
  if (keysPressed["a"] || keysPressed["arrowleft"]) player.position.x -= speed;
  if (keysPressed["d"] || keysPressed["arrowright"]) player.position.x += speed;

  // Limit player within game boundaries (assuming 0 to 100 for both axes)
  player.position.x = Math.max(0, Math.min(100, player.position.x));
  player.position.y = Math.max(0, Math.min(100, player.position.y));
}

function shootBullet() {
  if (bulletActive) return; // One bullet at a time for now

  bulletActive = true;
  const bullet = document.getElementById("bullet");
  bullet.style.display = "block";

  let bulletPos = { x: player.position.x, y: player.position.y };
  const direction = { x: 1, y: 0 }; // For example shooting right; later improve with mouse or facing direction

  const bulletInterval = setInterval(() => {
    bulletPos.x += direction.x * bulletSpeed * 0.1;
    bulletPos.y += direction.y * bulletSpeed * 0.1;

    // Update bullet UI position (assuming map scale)
    bullet.style.left = bulletPos.x + "vw";
    bullet.style.top = bulletPos.y + "vh";

    // Check collision with enemies
    enemies.forEach((enemy) => {
      if (
        enemy.isAlive &&
        Math.abs(enemy.position.x - bulletPos.x) < 2 &&
        Math.abs(enemy.position.y - bulletPos.y) < 2
      ) {
        enemy.takeDamage(25);
        clearInterval(bulletInterval);
        bullet.style.display = "none";
        bulletActive = false;
      }
    });

    // Remove bullet if out of bounds
    if (bulletPos.x > 100 || bulletPos.y > 100) {
      clearInterval(bulletInterval);
      bullet.style.display = "none";
      bulletActive = false;
    }
  }, 20);
}

document.addEventListener("keydown", (e) => {
  if (e.key.toLowerCase() === " ") {
    shootBullet();
  }
});

function gameLoop() {
  movePlayer();
  updateEnemiesAdvanced();
  updateUI();
  requestAnimationFrame(gameLoop);
}

gameLoop();
// === UI Updates ===

function updateUI() {
  // Update player position on screen
  const playerDiv = document.getElementById("player");
  playerDiv.style.left = player.position.x + "vw";
  playerDiv.style.top = player.position.y + "vh";

  // Update player stats display
  document.getElementById("player-level").textContent = player.level;
  document.getElementById("player-xp").textContent = player.xp;
  document.getElementById("player-weapons").textContent = player.weapons.join(", ");
  document.getElementById("player-skills").textContent = player.skills.join(", ");

  // Update missions list
  const missionsList = document.getElementById("missions-list");
  missionsList.innerHTML = "";
  missions.forEach((mission, index) => {
    const li = document.createElement("li");
    li.textContent = `${mission.title} - ${mission.status}`;
    if (mission.status === "completed") li.style.textDecoration = "line-through";
    li.addEventListener("click", () => {
      selectMission(index);
    });
    missionsList.appendChild(li);
  });

  // Update gangs list - highlight player's gang if any
  const gangsList = document.getElementById("gangs-list");
  gangsList.childNodes.forEach((li) => {
    if (player.gang && li.textContent === player.gang) {
      li.style.fontWeight = "bold";
      li.style.color = "#ff4500";
    } else {
      li.style.fontWeight = "normal";
      li.style.color = "inherit";
    }
  });

  // Notifications - could be improved later
  const notificationsDiv = document.getElementById("notifications");
  if (notifications.length > 0) {
    notificationsDiv.textContent = notifications[0];
    // Remove first notification after 3 seconds
    setTimeout(() => {
      notifications.shift();
    }, 3000);
  } else {
    notificationsDiv.textContent = "";
  }
}

function selectMission(index) {
  const mission = missions[index];
  if (mission.status !== "completed") {
    notifications.push(`Mission Selected: ${mission.title}`);
    currentMission = mission;
  }
}
// === Gameplay Mechanics ===

// Collision Detection between player and zones
function checkZoneCollision() {
  const playerPos = player.position;
  for (const zone of mapZones) {
    if (
      playerPos.x >= zone.x &&
      playerPos.x <= zone.x + zone.width &&
      playerPos.y >= zone.y &&
      playerPos.y <= zone.y + zone.height
    ) {
      if (currentZone !== zone.name) {
        currentZone = zone.name;
        notifications.push(`Entered Zone: ${zone.displayName}`);
        // Trigger zone-specific events
        handleZoneEvents(zone.name);
      }
      return;
    }
  }
  currentZone = null;
}

// Example zones definition (coordinates in vw, vh)
const mapZones = [
  { name: "forest-black", displayName: "Black Forest", x: 5, y: 10, width: 20, height: 20 },
  { name: "city-noran", displayName: "Noran City", x: 30, y: 10, width: 25, height: 25 },
  { name: "nova-tech", displayName: "Nova Tech Base", x: 60, y: 15, width: 20, height: 15 },
  { name: "suburbs", displayName: "The Suburbs", x: 10, y: 40, width: 25, height: 20 },
  { name: "resistance-camps", displayName: "Resistance Camps", x: 40, y: 50, width: 30, height: 20 },
  { name: "government-centers", displayName: "Government & Financial Centers", x: 75, y: 45, width: 20, height: 25 },
  { name: "secret-labs", displayName: "Underground Secret Labs", x: 15, y: 70, width: 30, height: 20 },
];

// Current zone player is in
let currentZone = null;

function handleZoneEvents(zoneName) {
  switch (zoneName) {
    case "forest-black":
      // Forest might have ambush mission unlock
      unlockMission("Ambush in Black Forest");
      break;
    case "city-noran":
      // City missions or gang encounters
      unlockMission("Protect the Citizens of Noran");
      break;
    case "nova-tech":
      unlockMission("Infiltrate Nova Tech Base");
      break;
    // Add other zones as needed
    default:
      break;
  }
}

function unlockMission(title) {
  const mission = missions.find(m => m.title === title);
  if (mission && mission.status === "locked") {
    mission.status = "available";
    notifications.push(`New Mission Available: ${title}`);
  }
}

// Player movement control (example with arrow keys)
document.addEventListener("keydown", (e) => {
  const step = 2; // movement step in vw/vh units
  switch(e.key) {
    case "ArrowUp":
      if (player.position.y > 0) player.position.y -= step;
      break;
    case "ArrowDown":
      if (player.position.y < 90) player.position.y += step;
      break;
    case "ArrowLeft":
      if (player.position.x > 0) player.position.x -= step;
      break;
    case "ArrowRight":
      if (player.position.x < 90) player.position.x += step;
      break;
  }
  checkZoneCollision();
  updateUI();
});

// Storyline progression logic

function progressStory() {
  if (!currentMission) {
    notifications.push("Select a mission to begin.");
    return;
  }

  if (currentMission.status === "completed") {
    notifications.push("Mission already completed. Choose another.");
    return;
  }

  switch (currentMission.title) {
    case "Ambush in Black Forest":
      // Example story event trigger
      notifications.push("You encounter enemy ambushers in the forest!");
      // Battle logic or decision can be triggered here
      break;
    case "Protect the Citizens of Noran":
      notifications.push("The city is under attack! Defend the citizens.");
      break;
    case "Infiltrate Nova Tech Base":
      notifications.push("Stealth mission: infiltrate undetected.");
      break;
    default:
      notifications.push("Mission in progress...");
  }
}

// Initialize game loop
function gameLoop() {
  updateUI();
  // Additional game logic (AI, battles, cooldowns) can be added here
  requestAnimationFrame(gameLoop);
}

gameLoop();
// === Combat System ===

const enemies = [
  { id: 1, name: "Black Guard Scout", health: 50, damage: 10, zone: "forest-black" },
  { id: 2, name: "Noran City Thug", health: 70, damage: 15, zone: "city-noran" },
  { id: 3, name: "Nova Tech Security Bot", health: 100, damage: 20, zone: "nova-tech" },
];

let currentEnemy = null;
let inCombat = false;

function startCombat(enemyId) {
  currentEnemy = enemies.find(e => e.id === enemyId);
  if (!currentEnemy) {
    notifications.push("No enemy found!");
    return;
  }
  inCombat = true;
  notifications.push(`Engaged in combat with ${currentEnemy.name}!`);
  updateUI();
}

function playerAttack() {
  if (!inCombat || !currentEnemy) return;
  // Simple damage mechanic
  const damageDealt = Math.floor(Math.random() * 20) + 10;
  currentEnemy.health -= damageDealt;
  notifications.push(`You hit ${currentEnemy.name} for ${damageDealt} damage.`);
  if (currentEnemy.health <= 0) {
    notifications.push(`${currentEnemy.name} defeated!`);
    inCombat = false;
    currentEnemy = null;
    completeMissionIfNeeded();
  } else {
    enemyAttack();
  }
  updateUI();
}

function enemyAttack() {
  const damage = Math.floor(Math.random() * currentEnemy.damage) + 5;
  player.health -= damage;
  notifications.push(`${currentEnemy.name} hits you for ${damage} damage.`);
  if (player.health <= 0) {
    player.health = 0;
    inCombat = false;
    notifications.push("You have been defeated...");
    showGameOver();
  }
  updateUI();
}

function completeMissionIfNeeded() {
  if (currentMission && currentMission.title.includes(currentEnemy?.zone) && currentEnemy === null) {
    currentMission.status = "completed";
    notifications.push(`Mission "${currentMission.title}" completed!`);
  }
}

// Player object extended with health
player.health = 100;

// Show Game Over
function showGameOver() {
  document.getElementById("game-over-screen").style.display = "block";
  document.getElementById("game-container").style.display = "none";
}
// بيانات اللاعب
const player = {
  level: 1,
  xp: 0,
  weapons: ["EMP Pistol"],
  skills: ["Basic Hacking"],
  health: 100,
};

// المهام
const missions = [
  { title: " infiltrate Black Forest", status: "incomplete" },
  { title: "Secure Nova Tech Base", status: "incomplete" },
  { title: "Destroy Government Centers", status: "incomplete" },
];
let currentMissionIndex = 0;
let currentMission = missions[currentMissionIndex];

// إشعارات اللعبة (تُعرض آخر 5 فقط)
const notifications = [];

// تحديث واجهة المستخدم
function updateUI() {
  // Player stats
  document.getElementById("player-level").textContent = player.level;
  document.getElementById("player-xp").textContent = player.xp;
  document.getElementById("player-weapons").textContent = player.weapons.join(", ");
  document.getElementById("player-skills").textContent = player.skills.join(", ");

  // Player health border effect
  const healthRatio = player.health / 100;
  document.getElementById("player").style.border = `2px solid rgba(255, 0, 0, ${1 - healthRatio})`;

  // Missions list
  const missionsList = document.getElementById("missions-list");
  missionsList.innerHTML = "";
  missions.forEach((mission, idx) => {
    const li = document.createElement("li");
    li.textContent = `${mission.title} [${mission.status}]`;
    if (idx === currentMissionIndex) li.style.fontWeight = "bold";
    missionsList.appendChild(li);
  });

  // Notifications (show last 5)
  const notificationsDiv = document.getElementById("notifications");
  notificationsDiv.innerHTML = notifications
    .slice(-5)
    .map(note => `<p>${note}</p>`)
    .join("");
}

// إعادة ضبط اللعبة
function resetGame() {
  player.level = 1;
  player.xp = 0;
  player.weapons = ["EMP Pistol"];
  player.skills = ["Basic Hacking"];
  player.health = 100;

  missions.forEach(m => m.status = "incomplete");
  currentMissionIndex = 0;
  currentMission = missions[currentMissionIndex];
  notifications.length = 0;

  updateUI();
}

// أحداث الأزرار
document.getElementById("start-game").addEventListener("click", () => {
  document.getElementById("loading-screen").style.display = "block";
  setTimeout(() => {
    document.getElementById("loading-screen").style.display = "none";
    document.getElementById("game-container").style.display = "block";
    notifications.push("Game started! Welcome to BLACKLINE.");
    updateUI();
  }, 1500);
});

document.getElementById("restart-game").addEventListener("click", () => {
  resetGame();
  document.getElementById("game-over-screen").style.display = "none";
  document.getElementById("victory-screen").style.display = "none";
  document.getElementById("game-container").style.display = "block";
});

document.getElementById("exit-to-menu").addEventListener("click", () => {
  resetGame();
  document.getElementById("game-over-screen").style.display = "none";
  document.getElementById("victory-screen").style.display = "none";
  document.getElementById("game-container").style.display = "none";
  document.getElementById("start-menu").style.display = "block";
});

document.getElementById("exit-to-menu-victory").addEventListener("click", () => {
  resetGame();
  document.getElementById("victory-screen").style.display = "none";
  document.getElementById("game-container").style.display = "none";
  document.getElementById("start-menu").style.display = "block";
});

document.getElementById("next-mission").addEventListener("click", () => {
  if (currentMissionIndex + 1 < missions.length) {
    currentMissionIndex++;
    currentMission = missions[currentMissionIndex];
    notifications.push(`Next mission started: ${currentMission.title}`);
    updateUI();
  } else {
    notifications.push("No more missions available.");
    updateUI();
  }
});

document.getElementById("apply-settings").addEventListener("click", () => {
  const volume = document.getElementById("volume-control").value;
  const brightness = document.getElementById("brightness-control").value;
  const difficulty = document.getElementById("difficulty-select").value;
  notifications.push(`Settings applied: Volume ${volume}, Brightness ${brightness}, Difficulty ${difficulty}`);
  updateUI();
  // هنا يمكن تطبيق تأثير الإعدادات على اللعبة لاحقاً
});

document.getElementById("close-settings").addEventListener("click", () => {
  document.getElementById("settings-panel").style.display = "none";
});

// نداء أولي لتحديث الواجهة
updateUI();
// تحريك اللاعب باستخدام الأسهم أو WASD
const playerElement = document.getElementById("player");
let playerPosition = { x: 100, y: 100 };
const moveStep = 10;

function movePlayer(dx, dy) {
  playerPosition.x += dx;
  playerPosition.y += dy;
  // حدود الخريطة (مثلاً 0 إلى 800 بكسل عرض و600 بكسل ارتفاع)
  playerPosition.x = Math.max(0, Math.min(800, playerPosition.x));
  playerPosition.y = Math.max(0, Math.min(600, playerPosition.y));
  playerElement.style.left = playerPosition.x + "px";
  playerElement.style.top = playerPosition.y + "px";
}

// الاستماع للأزرار لتحريك اللاعب
window.addEventListener("keydown", (e) => {
  switch (e.key) {
    case "ArrowUp":
    case "w":
    case "W":
      movePlayer(0, -moveStep);
      break;
    case "ArrowDown":
    case "s":
    case "S":
      movePlayer(0, moveStep);
      break;
    case "ArrowLeft":
    case "a":
    case "A":
      movePlayer(-moveStep, 0);
      break;
    case "ArrowRight":
    case "d":
    case "D":
      movePlayer(moveStep, 0);
      break;
    case " ":
      fireBullet();
      break;
  }
});

// إطلاق الرصاص
const bulletElement = document.getElementById("bullet");
let bulletActive = false;
let bulletPosition = { x: 0, y: 0 };
const bulletSpeed = 20;

function fireBullet() {
  if (bulletActive) return; // لا يمكن إطلاق أكثر من رصاصة واحدة في الوقت نفسه

  bulletActive = true;
  bulletPosition.x = playerPosition.x + 20; // بداية الرصاصة من منتصف اللاعب تقريباً
  bulletPosition.y = playerPosition.y + 10;
  bulletElement.style.left = bulletPosition.x + "px";
  bulletElement.style.top = bulletPosition.y + "px";
  bulletElement.style.display = "block";

  // تحريك الرصاصة للأمام تلقائياً
  const bulletInterval = setInterval(() => {
    bulletPosition.x += bulletSpeed;
    if (bulletPosition.x > 800) {
      bulletActive = false;
      bulletElement.style.display = "none";
      clearInterval(bulletInterval);
    } else {
      bulletElement.style.left = bulletPosition.x + "px";
    }
  }, 30);
}

// تحديث مستمر للعبة (يمكن لاحقاً توسيعها للتحكم بالذكاء الاصطناعي، الاصطدامات، الخ)
function gameLoop() {
  // تحديثات متقدمة هنا
  requestAnimationFrame(gameLoop);
}
gameLoop();
// إضافة حدث لتحريك اللاعب باستخدام مفاتيح الأسهم أو WASD
document.addEventListener('keydown', function(event) {
  const player = document.getElementById('player');
  let left = parseInt(player.style.left) || 0;
  let top = parseInt(player.style.top) || 0;
  const step = 10;

  switch(event.key) {
    case 'ArrowLeft':
    case 'a':
    case 'A':
      left -= step;
      break;
    case 'ArrowRight':
    case 'd':
    case 'D':
      left += step;
      break;
    case 'ArrowUp':
    case 'w':
    case 'W':
      top -= step;
      break;
    case 'ArrowDown':
    case 's':
    case 'S':
      top += step;
      break;
    case ' ':
      shootBullet();
      break;
  }
  player.style.left = left + 'px';
  player.style.top = top + 'px';
});

// وظيفة إطلاق الرصاصة
function shootBullet() {
  const bullet = document.getElementById('bullet');
  const player = document.getElementById('player');
  bullet.style.display = 'block';

  // تحديد موقع البداية للرصاصة بناءً على موقع اللاعب
  let bulletX = parseInt(player.style.left) + 20;
  let bulletY = parseInt(player.style.top) + 10;
  bullet.style.left = bulletX + 'px';
  bullet.style.top = bulletY + 'px';

  // تحريك الرصاصة إلى اليمين بسرعة ثابتة
  let bulletInterval = setInterval(() => {
    bulletX += 15;
    bullet.style.left = bulletX + 'px';

    if (bulletX > window.innerWidth) {
      bullet.style.display = 'none';
      clearInterval(bulletInterval);
    }
  }, 50);
}

// تحديث الإشعارات
function showNotification(message) {
  const notif = document.getElementById('notifications');
  notif.textContent = message;
  notif.style.opacity = 1;
  setTimeout(() => {
    notif.style.opacity = 0;
  }, 3000);
}

// مثال إضافة مهمة جديدة
function addMission(title, description) {
  const missionsList = document.getElementById('missions-list');
  const missionItem = document.createElement('li');
  missionItem.textContent = title + ": " + description;
  missionsList.appendChild(missionItem);
  showNotification("New mission added: " + title);
}

// مثال: إضافة مهمة عند بدء اللعبة
addMission("First Steps", "Explore the Black Forest and find the hidden cache.");
// نظام ذكاء اصطناعي بسيط للعصابات - تحرك عشوائي داخل الخريطة
class GangMember {
  constructor(name, elementId) {
    this.name = name;
    this.element = document.getElementById(elementId);
    this.x = parseInt(this.element.style.left) || Math.floor(Math.random() * 500);
    this.y = parseInt(this.element.style.top) || Math.floor(Math.random() * 500);
    this.speed = 5; // سرعة الحركة
    this.direction = this.getRandomDirection();
    this.moveInterval = null;
  }

  getRandomDirection() {
    const directions = ['left', 'right', 'up', 'down'];
    return directions[Math.floor(Math.random() * directions.length)];
  }

  move() {
    switch (this.direction) {
      case 'left':
        this.x -= this.speed;
        if (this.x < 0) this.direction = 'right';
        break;
      case 'right':
        this.x += this.speed;
        if (this.x > window.innerWidth - 50) this.direction = 'left';
        break;
      case 'up':
        this.y -= this.speed;
        if (this.y < 0) this.direction = 'down';
        break;
      case 'down':
        this.y += this.speed;
        if (this.y > window.innerHeight - 50) this.direction = 'up';
        break;
    }
    this.element.style.left = this.x + 'px';
    this.element.style.top = this.y + 'px';
  }

  startMoving() {
    this.moveInterval = setInterval(() => this.move(), 100);
  }

  stopMoving() {
    clearInterval(this.moveInterval);
  }
}

// إنشاء أعضاء العصابات وربطهم بالعناصر في الصفحة (تأكد من وجود العناصر في الـ HTML مع IDs مثل gang1, gang2, ...)
const gang1 = new GangMember('Black Guard', 'gang1');
const gang2 = new GangMember('Hackers Hunters', 'gang2');
const gang3 = new GangMember('Red Shield', 'gang3');

// بدء الحركة
gang1.startMoving();
gang2.startMoving();
gang3.startMoving();

// مراقبة تصادم الرصاصة مع العصابات (مبدئي)
function checkBulletCollision() {
  const bullet = document.getElementById('bullet');
  if (bullet.style.display === 'none') return;

  const bulletRect = bullet.getBoundingClientRect();

  [gang1, gang2, gang3].forEach(gang => {
    const gangRect = gang.element.getBoundingClientRect();

    if (
      bulletRect.left < gangRect.right &&
      bulletRect.right > gangRect.left &&
      bulletRect.top < gangRect.bottom &&
      bulletRect.bottom > gangRect.top
    ) {
      // اصطدم الرصاصة بالعصابة
      showNotification(`${gang.name} hit!`);
      bullet.style.display = 'none';
      gang.stopMoving();
      gang.element.style.display = 'none';
    }
  });
}

// فحص التصادم بشكل دوري
setInterval(checkBulletCollision, 50);
// نظام المهام (Mission System)
const missionData = [
  {
    id: 1,
    title: "Eliminate the Black Guard",
    description: "Find and eliminate the Black Guard gang member roaming the city.",
    targetGang: "Black Guard",
    completed: false,
  },
  {
    id: 2,
    title: "Disable Hackers Hunters",
    description: "Track and stop the cyber gang before they reach the central server.",
    targetGang: "Hackers Hunters",
    completed: false,
  },
  {
    id: 3,
    title: "Stop Red Shield Ambush",
    description: "Red Shield is preparing an ambush. Stop them before they act.",
    targetGang: "Red Shield",
    completed: false,
  }
];

let currentMissionIndex = 0;

function loadMission(index) {
  const mission = missionData[index];
  if (!mission) return;
  document.getElementById("mission-title").innerText = mission.title;
  document.getElementById("mission-desc").innerText = mission.description;
}

function completeMission(gangName) {
  const mission = missionData[currentMissionIndex];
  if (mission && !mission.completed && mission.targetGang === gangName) {
    mission.completed = true;
    showNotification(`Mission "${mission.title}" Completed!`);
    currentMissionIndex++;
    loadMission(currentMissionIndex);
  }
}

// تعديل checkBulletCollision لتفعيل إكمال المهمة:
function checkBulletCollision() {
  const bullet = document.getElementById('bullet');
  if (bullet.style.display === 'none') return;

  const bulletRect = bullet.getBoundingClientRect();

  [gang1, gang2, gang3].forEach(gang => {
    const gangRect = gang.element.getBoundingClientRect();

    if (
      bulletRect.left < gangRect.right &&
      bulletRect.right > gangRect.left &&
      bulletRect.top < gangRect.bottom &&
      bulletRect.bottom > gangRect.top
    ) {
      // اصطدام
      showNotification(`${gang.name} hit!`);
      bullet.style.display = 'none';
      gang.stopMoving();
      gang.element.style.display = 'none';
      completeMission(gang.name);
    }
  });
}

// واجهة عرض المهمة (افترض أنها موجودة في HTML)
loadMission(currentMissionIndex);
// إضافة خصائص الهجوم لكل عصابة
class Gang {
  constructor(name, id, speed = 1.5) {
    this.name = name;
    this.element = document.getElementById(id);
    this.speed = speed;
    this.interval = null;
    this.attackCooldown = false;
    this.moveRandomly();
    this.setupAttack();
  }

  moveRandomly() {
    this.interval = setInterval(() => {
      const x = Math.random() * window.innerWidth;
      const y = Math.random() * window.innerHeight;
      this.element.style.left = x + 'px';
      this.element.style.top = y + 'px';
    }, this.speed * 1000);
  }

  stopMoving() {
    clearInterval(this.interval);
  }

  setupAttack() {
    setInterval(() => {
      if (this.attackCooldown) return;

      const player = document.getElementById('player');
      const playerRect = player.getBoundingClientRect();
      const gangRect = this.element.getBoundingClientRect();

      const distance = Math.hypot(
        playerRect.left - gangRect.left,
        playerRect.top - gangRect.top
      );

      if (distance < 150) {
        this.attackPlayer();
        this.attackCooldown = true;
        setTimeout(() => {
          this.attackCooldown = false;
        }, 2000); // Cooldown
      }
    }, 500);
  }

  attackPlayer() {
    const damage = Math.floor(Math.random() * 10) + 5;
    reducePlayerHealth(damage);
    showNotification(`${this.name} attacked you! -${damage} HP`);
  }
}

// الصحة (Health)
let playerHealth = 100;

function reducePlayerHealth(amount) {
  playerHealth -= amount;
  if (playerHealth <= 0) {
    triggerGameOver();
  } else {
    document.getElementById('player-health').innerText = playerHealth;
  }
}

// افترض أن لديك في HTML:
// <p>Health: <span id="player-health">100</span></p>
function fireBullet() {
  if (!canFire) return;

  const bullet = document.getElementById('bullet');
  bullet.style.display = 'block';
  bullet.style.left = playerX + 20 + 'px';
  bullet.style.top = playerY + 20 + 'px';

  let crit = Math.random() < 0.2; // 20% فرصة لضربة حرجة
  let bulletDamage = crit ? 40 : 20;

  let moveInterval = setInterval(() => {
    bullet.style.left = parseInt(bullet.style.left) + 15 + 'px';

    gangs.forEach(gang => {
      const bulletRect = bullet.getBoundingClientRect();
      const gangRect = gang.element.getBoundingClientRect();

      if (isColliding(bulletRect, gangRect)) {
        clearInterval(moveInterval);
        bullet.style.display = 'none';
        showNotification(`Hit ${gang.name}${crit ? ' with a CRITICAL SHOT!' : ''}`);
        damageGang(gang, bulletDamage);
      }
    });

    if (parseInt(bullet.style.left) > window.innerWidth) {
      clearInterval(moveInterval);
      bullet.style.display = 'none';
    }
  }, 30);
}
const medkit = document.getElementById('medkit');

setInterval(() => {
  const playerRect = document.getElementById('player').getBoundingClientRect();
  const medkitRect = medkit.getBoundingClientRect();

  if (isColliding(playerRect, medkitRect)) {
    healPlayer(30);
    showNotification("You picked up a medkit! +30 HP");
    medkit.style.display = 'none';
  }
}, 300);

function healPlayer(amount) {
  playerHealth += amount;
  if (playerHealth > 100) playerHealth = 100;
  document.getElementById('player-health').innerText = playerHealth;
}
function updateGangBehavior() {
  gangs.forEach(gang => {
    const gangEl = gang.element;
    const playerEl = document.getElementById('player');
    const gangRect = gangEl.getBoundingClientRect();
    const playerRect = playerEl.getBoundingClientRect();

    const dx = playerRect.left - gangRect.left;
    const dy = playerRect.top - gangRect.top;
    const distance = Math.sqrt(dx * dx + dy * dy);

    // إذا اللاعب اقترب أقل من 250 بكسل
    if (distance < 250 && gang.health > 30) {
      gangEl.style.left = parseInt(gangEl.style.left) + dx * 0.05 + 'px';
      gangEl.style.top = parseInt(gangEl.style.top) + dy * 0.05 + 'px';
    }
    // إذا العصابة جُرحت كثيرًا، تهرب
    else if (gang.health <= 30) {
      gangEl.style.left = parseInt(gangEl.style.left) - dx * 0.05 + 'px';
      gangEl.style.top = parseInt(gangEl.style.top) - dy * 0.05 + 'px';
    }

    // هجوم دوري
    if (distance < 80 && Math.random() < 0.01) {
      damagePlayer(10);
      showNotification(`${gang.name} attacked you!`);
    }
  });
}
setInterval(updateGangBehavior, 200);
function saveGame() {
  const gameData = {
    playerHealth,
    playerScore,
    gangsDefeated
  };
  localStorage.setItem('gameSave', JSON.stringify(gameData));
}
function loadGame() {
  const saved = localStorage.getItem('gameSave');
  if (saved) {
    const data = JSON.parse(saved);
    playerHealth = data.playerHealth;
    playerScore = data.playerScore;
    gangsDefeated = data.gangsDefeated;
    document.getElementById('player-health').innerText = playerHealth;
  }
}
window.onload = loadGame;
setInterval(saveGame, 10000); // حفظ كل 10 ثوانٍ
let playerMoney = 300;
let playerWeapon = "Pistol";
let playerSpeed = 1;
let playerDamage = 1;

function buyWeapon(name, cost) {
  if (playerMoney >= cost) {
    playerWeapon = name;
    playerMoney -= cost;
    showNotification(`You bought ${name}`);
  } else {
    showNotification("Not enough money!");
  }
}

function upgradeSkill(skill) {
  if (skill === 'Speed' && playerMoney >= 50) {
    playerSpeed += 0.2;
    playerMoney -= 50;
    showNotification("Speed upgraded!");
  } else if (skill === 'Damage' && playerMoney >= 70) {
    playerDamage += 0.3;
    playerMoney -= 70;
    showNotification("Damage upgraded!");
  } else {
    showNotification("Not enough money!");
  }
}

function closeShop() {
  document.getElementById('shop-panel').style.display = 'none';
}
const storyMissions = [
  {
    title: "Escape the Hood",
    objective: "Defeat 3 gangsters and find the escape car.",
    completed: false,
    requirement: () => gangsDefeated >= 3
  },
  {
    title: "Ambush at the Bridge",
    objective: "Survive a wave of enemies for 60 seconds.",
    completed: false,
    requirement: () => missionTimers["bridge"] >= 60
  },
  {
    title: "Final Stand",
    objective: "Defeat the gang boss.",
    completed: false,
    requirement: () => bossDefeated
  }
];

let currentMissionIndex = 0;
let missionTimers = { bridge: 0 };
let bossDefeated = false;
function checkMissionProgress() {
  const mission = storyMissions[currentMissionIndex];
  if (!mission.completed && mission.requirement()) {
    mission.completed = true;
    showNotification(`Mission Complete: ${mission.title}`);
    currentMissionIndex++;
    if (currentMissionIndex < storyMissions.length) {
      displayCurrentMission();
    } else {
      triggerVictory(); // نهاية اللعبة
    }
  }
}
setInterval(checkMissionProgress, 2000);
function startBridgeMission() {
  let time = 0;
  const timer = setInterval(() => {
    time++;
    missionTimers["bridge"] = time;
    if (storyMissions[1].completed || currentMissionIndex > 1) {
      clearInterval(timer);
    }
  }, 1000);
}
#mission-panel {
  position: absolute;
  top: 10px;
  right: 10px;
  background: rgba(0,0,0,0.5);
  color: white;
  padding: 12px;
  border-radius: 10px;
  max-width: 300px;
  font-family: monospace;
  z-index: 10;
}
const sounds = {
  gunshot: new Audio("assets/sounds/gunshot.mp3"),
  explosion: new Audio("assets/sounds/explosion.mp3"),
  victory: new Audio("assets/sounds/victory.mp3"),
  gameOver: new Audio("assets/sounds/gameover.mp3"),
  missionComplete: new Audio("assets/sounds/mission.mp3")
};

function playSound(name) {
  if (sounds[name]) {
    sounds[name].currentTime = 0;
    sounds[name].play();
  }
}
// عند إطلاق النار
shootButton.onclick = () => {
  playSound("gunshot");
  // بقية الأكشن
};

// عند نهاية المهمة
showNotification("Mission Complete!");
playSound("missionComplete");
function screenFlash(color = 'white') {
  const flash = document.createElement('div');
  flash.style.position = 'fixed';
  flash.style.top = 0;
  flash.style.left = 0;
  flash.style.width = '100%';
  flash.style.height = '100%';
  flash.style.background = color;
  flash.style.opacity = '0.5';
  flash.style.zIndex = 1000;
  document.body.appendChild(flash);
  setTimeout(() => flash.remove(), 150);
}

function shakeScreen(duration = 500) {
  const gameContainer = document.getElementById("game-container");
  gameContainer.style.animation = `shake ${duration}ms`;
  setTimeout(() => gameContainer.style.animation = "", duration);
}

function explosionEffect(x, y) {
  const explosion = document.createElement("div");
  explosion.className = "explosion";
  explosion.style.left = `${x}px`;
  explosion.style.top = `${y}px`;
  document.body.appendChild(explosion);
  setTimeout(() => explosion.remove(), 500);
}
