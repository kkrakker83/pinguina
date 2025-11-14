# 🛠️ Guía para Desarrolladores - Pinguina

## Introducción

Esta guía está diseñada para ayudar a desarrolladores futuros a entender, mantener y extender el juego Pinguina.

## Arquitectura del Juego

### Tecnologías Utilizadas
- **HTML5**: Estructura base
- **CSS3**: Estilos y diseño responsivo
- **JavaScript (ES6+)**: Lógica del juego
- **Canvas API**: Renderizado gráfico
- **Web Audio API**: Efectos de sonido

### Estructura de Archivos

```
pinguina/
├── PinguinaGame/
│   ├── index.html          # Archivo principal del juego
│   └── assets/
│       ├── llipsia.png     # Sprite de pingüina (legacy)
│       ├── obstaculo.png   # Imágenes de obstáculos (legacy)
│       └── obstaculo1.png
├── README.md               # Documentación principal
└── DEVELOPER_GUIDE.md      # Esta guía
```

## Componentes del Sistema

### 1. Estado del Juego

```javascript
let gameState = 'menu'; // Estados: menu, playing, gameover, levelcomplete
let difficulty = 'medium';
let currentLevel = 1;
let score = 0;
let lives = 3;
```

### 2. Sistema de Cuadrícula

El juego utiliza un sistema de cuadrícula para el laberinto:

```javascript
const CELL_SIZE = 40;  // Tamaño de cada celda en píxeles
const COLS = 20;       // 20 columnas
const ROWS = 15;       // 15 filas
```

Canvas total: 800x600 píxeles (20 * 40, 15 * 40)

### 3. Objetos del Juego

#### Jugador (Player)
```javascript
player = {
  x: posición X en píxeles,
  y: posición Y en píxeles,
  size: tamaño del sprite,
  speed: velocidad de movimiento,
  direction: dirección actual en radianes
}
```

#### Enemigos (Enemies)
```javascript
enemy = {
  x, y: posición,
  size: tamaño,
  speed: velocidad,
  direction: dirección de movimiento,
  color: color distintivo
}
```

#### Coleccionables (Collectibles)
```javascript
collectible = {
  x, y: posición,
  size: tamaño,
  collected: boolean
}
```

## Sistemas Principales

### Sistema de Laberinto

#### Generación Procedural
```javascript
function generateMaze(level) {
  // 1. Crear matriz vacía
  // 2. Establecer bordes
  // 3. Añadir paredes internas basadas en nivel
  // 4. Asegurar que la posición inicial esté despejada
}
```

**Algoritmo de Generación:**
1. Crea bordes exteriores (siempre son paredes)
2. Coloca paredes internas en patrón de cuadrícula (cada 2 celdas)
3. La densidad de paredes aumenta con el nivel: `0.15 + (level * 0.05)`
4. Garantiza que la posición inicial (1,1) esté libre

#### Detección de Colisiones con Paredes
```javascript
function isWalkable(x, y) {
  // Convierte coordenadas de píxeles a índices de cuadrícula
  // Verifica si la celda es transitable (0) o pared (1)
}
```

### Sistema de Movimiento

#### Jugador
- Lee input del teclado (flechas o WASD)
- Calcula nueva posición
- Verifica colisión en las 4 esquinas del sprite
- Aplica movimiento solo si todas las esquinas son válidas

#### Enemigos (IA)
- **Movimiento Aleatorio (98%)**: Mantiene dirección actual
- **Cambio Aleatorio (2%)**: Nueva dirección aleatoria
- **Persecución (10%)**: Calcula dirección hacia el jugador
- **Rebote en Paredes**: Cambia dirección al chocar

### Sistema de Colisiones

#### Detección de Distancia
```javascript
const dx = obj1.x - obj2.x;
const dy = obj1.y - obj2.y;
const dist = Math.sqrt(dx * dx + dy * dy);
if (dist < threshold) { /* colisión */ }
```

#### Tipos de Colisión
1. **Jugador-Coleccionable**: Aumenta puntuación, marca como recolectado
2. **Jugador-Enemigo**: Reduce vidas, reinicia posición del jugador
3. **Entidad-Pared**: Previene movimiento

### Sistema de Audio

#### Estructura de Sonidos
```javascript
function playSound(frequency, duration) {
  // Crea oscilador
  // Configura frecuencia y ganancia
  // Aplica envolvente (fade out)
  // Reproduce sonido
}
```

#### Sonidos Implementados
- `collectSound()`: 800 Hz, 0.1s - Recoger pez
- `hitSound()`: 150 Hz, 0.2s - Colisión con enemigo
- `gameOverSound()`: 200 Hz, 0.5s - Fin del juego
- `levelCompleteSound()`: Secuencia de 3 notas (523, 659, 784 Hz)

## Configuración de Dificultad

### Parámetros por Nivel

| Dificultad | Enemigos | Velocidad | Vidas | Puntos/Pez |
|------------|----------|-----------|-------|------------|
| Fácil      | 2        | 1.5       | 5     | 10         |
| Medio      | 3        | 2.0       | 3     | 20         |
| Difícil    | 4        | 2.5       | 2     | 30         |

### Añadir Nueva Dificultad

```javascript
const DIFFICULTY = {
  // ... existentes ...
  extreme: { 
    enemyCount: 5, 
    enemySpeed: 3, 
    lives: 1, 
    pointsPerFish: 50 
  }
};
```

Luego añadir botón en HTML:
```html
<button onclick="startGame('extreme')">Extremo</button>
```

## Renderizado

### Orden de Dibujado
1. Fondo del canvas
2. Laberinto (paredes)
3. Coleccionables (peces)
4. Jugador (pingüina)
5. Enemigos

### Pipeline de Renderizado
```
gameLoop() 
  → drawMaze()
  → drawCollectibles()
  → drawPlayer()
  → drawEnemies()
  → requestAnimationFrame(gameLoop)
```

## Ciclo del Juego (Game Loop)

```javascript
function gameLoop() {
  if (gameState === 'playing') {
    updatePlayer();      // Procesa input y movimiento
    updateEnemies();     // Actualiza IA de enemigos
    checkCollisions();   // Detecta todas las colisiones
    updateUI();          // Actualiza interfaz
  }
  
  // Renderiza siempre, independiente del estado
  drawMaze();
  drawCollectibles();
  drawPlayer();
  drawEnemies();
  
  // Continúa el loop si está jugando
  if (gameState === 'playing') {
    requestAnimationFrame(gameLoop);
  }
}
```

## Tareas Comunes

### Cambiar Velocidad del Jugador
```javascript
function initPlayer() {
  player = {
    // ...
    speed: 3,  // Modificar este valor (1-5 recomendado)
  };
}
```

### Modificar Generación de Coleccionables
```javascript
function initCollectibles() {
  // Cambiar probabilidad de spawn
  if (maze[y][x] === 0 && Math.random() < 0.15) { // Modificar 0.15
    // ...
  }
  
  // Cambiar cantidad mínima
  while (collectibles.length < 20) { // Modificar 20
    // ...
  }
}
```

### Ajustar IA de Enemigos

Para hacerlos más agresivos:
```javascript
function updateEnemies() {
  enemies.forEach(enemy => {
    if (Math.random() < 0.02) {  // Reducir este valor
      enemy.direction = Math.random() * Math.PI * 2;
    } else if (Math.random() < 0.3) {  // Aumentar este valor
      // Perseguir al jugador más frecuentemente
      const dx = player.x - enemy.x;
      const dy = player.y - enemy.y;
      enemy.direction = Math.atan2(dy, dx);
    }
    // ...
  });
}
```

### Añadir Nuevos Power-ups

```javascript
// 1. Crear array de power-ups
let powerups = [];

// 2. Generar power-ups
function initPowerups() {
  powerups.push({
    x: ...,
    y: ...,
    type: 'speed_boost',
    duration: 5000,  // 5 segundos
    collected: false
  });
}

// 3. Detectar colisión
function checkCollisions() {
  // ... código existente ...
  
  powerups.forEach(powerup => {
    if (!powerup.collected) {
      const dist = calculateDistance(player, powerup);
      if (dist < threshold) {
        powerup.collected = true;
        applyPowerup(powerup);
      }
    }
  });
}

// 4. Aplicar efecto
function applyPowerup(powerup) {
  if (powerup.type === 'speed_boost') {
    player.speed *= 1.5;
    setTimeout(() => {
      player.speed /= 1.5;
    }, powerup.duration);
  }
}
```

## Depuración

### Herramientas de Desarrollo

#### Mostrar Cuadrícula de Depuración
```javascript
function drawDebugGrid() {
  ctx.strokeStyle = 'rgba(255, 255, 255, 0.2)';
  ctx.lineWidth = 1;
  for (let x = 0; x <= COLS; x++) {
    ctx.beginPath();
    ctx.moveTo(x * CELL_SIZE, 0);
    ctx.lineTo(x * CELL_SIZE, canvas.height);
    ctx.stroke();
  }
  for (let y = 0; y <= ROWS; y++) {
    ctx.beginPath();
    ctx.moveTo(0, y * CELL_SIZE);
    ctx.lineTo(canvas.width, y * CELL_SIZE);
    ctx.stroke();
  }
}
```

#### Modo de Depuración
```javascript
let DEBUG_MODE = true;

if (DEBUG_MODE) {
  // Mostrar posiciones
  console.log('Player:', player.x, player.y);
  console.log('Enemies:', enemies.map(e => ({x: e.x, y: e.y})));
  
  // Invencibilidad
  // Comentar la reducción de vidas en checkCollisions()
}
```

### Problemas Comunes

#### Enemigos se Quedan Atascados
- Aumentar la frecuencia de cambio de dirección
- Mejorar detección de colisión con paredes
- Añadir sistema de pathfinding

#### Rendimiento Bajo
- Reducir número de coleccionables
- Optimizar detección de colisiones
- Usar técnicas de spatial partitioning

## Mejoras Futuras

### Sugerencias de Características

1. **Sistema de Power-ups**
   - Invencibilidad temporal
   - Velocidad aumentada
   - Congelar enemigos

2. **Mejores Gráficos**
   - Sprites animados
   - Efectos de partículas
   - Transiciones suaves

3. **Sonidos Mejorados**
   - Música de fondo
   - Efectos de sonido variados
   - Control de volumen

4. **Persistencia**
   - Guardar puntuación alta (localStorage)
   - Sistema de logros
   - Progreso de niveles guardado

5. **Multijugador**
   - Modo cooperativo local
   - Competición de puntuaciones
   - Tablas de clasificación

## Testing

### Casos de Prueba Sugeridos

1. **Movimiento**
   - [ ] Jugador se mueve en todas direcciones
   - [ ] Jugador no atraviesa paredes
   - [ ] Movimiento diagonal funciona correctamente

2. **Colisiones**
   - [ ] Coleccionables se recogen correctamente
   - [ ] Enemigos causan pérdida de vidas
   - [ ] Paredes detienen movimiento

3. **Progresión**
   - [ ] Nivel se completa al recoger todos los peces
   - [ ] Siguiente nivel es más difícil
   - [ ] Game Over ocurre sin vidas

4. **Dificultades**
   - [ ] Cada dificultad tiene configuración correcta
   - [ ] Dificultad Fácil es accesible
   - [ ] Dificultad Difícil es desafiante

## Recursos Adicionales

- [Canvas API Documentation](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API)
- [Web Audio API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Audio_API)
- [Game Development Patterns](https://gameprogrammingpatterns.com/)

## Contacto y Contribuciones

Para contribuir al proyecto:
1. Fork el repositorio
2. Crea una rama feature
3. Realiza commits con mensajes descriptivos
4. Abre un Pull Request

¡Esperamos tus contribuciones!
