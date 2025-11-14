# 🐧 Pinguina - Juego de Laberinto Estilo Pac-Man

Un juego de laberinto con temática de pingüinos donde debes recolectar peces mientras evitas enemigos.

## 📖 Descripción

Pinguina es un juego estilo Pac-Man donde controlas a una pingüina que debe navegar a través de un laberinto para recolectar todos los peces (puntos dorados) mientras evita ser capturada por los enemigos. El juego incluye tres niveles de dificultad y múltiples niveles progresivos.

## 🎮 Cómo Jugar

### Controles
- **Flechas (↑ ↓ ← →)** o **WASD**: Mover la pingüina
- El juego se controla completamente con el teclado

### Objetivo
1. Recoge todos los peces dorados en el laberinto
2. Evita a los enemigos de colores
3. Completa el nivel para avanzar al siguiente
4. Acumula la mayor puntuación posible

### Mecánicas
- **Vidas**: Pierdes una vida al chocar con un enemigo
- **Puntuación**: Ganas puntos por cada pez recolectado (varía según dificultad)
- **Niveles**: Los niveles se vuelven más complejos con más enemigos y laberintos más difíciles

## 🎯 Niveles de Dificultad

### Fácil
- 2 enemigos
- Velocidad de enemigos: Lenta
- 5 vidas
- 10 puntos por pez

### Medio
- 3 enemigos
- Velocidad de enemigos: Moderada
- 3 vidas
- 20 puntos por pez

### Difícil
- 4 enemigos
- Velocidad de enemigos: Rápida
- 2 vidas
- 30 puntos por pez

## 🚀 Instalación y Ejecución

### Requisitos
- Navegador web moderno (Chrome, Firefox, Safari, Edge)
- No requiere instalación de software adicional

### Instrucciones
1. Clona el repositorio:
   ```bash
   git clone https://github.com/kkrakker83/pinguina.git
   ```

2. Abre el archivo del juego:
   ```bash
   cd pinguina/PinguinaGame
   ```

3. Abre `index.html` en tu navegador web preferido:
   - Doble clic en el archivo, o
   - Usa un servidor local: `python -m http.server 8000`
   - Luego visita: `http://localhost:8000`

## 🎨 Características

- ✅ **Sistema de puntuación**: Acumula puntos recolectando peces
- ✅ **Tres niveles de dificultad**: Fácil, Medio y Difícil
- ✅ **Control por teclado**: Flechas o WASD
- ✅ **Múltiples niveles**: Laberintos generados proceduralmente
- ✅ **Enemigos con IA**: Movimiento inteligente con persecución ocasional
- ✅ **Efectos de sonido**: Audio básico para acciones del juego
- ✅ **Interfaz gráfica**: Diseño temático con pingüinos

## 🛠️ Documentación para Desarrolladores

### Estructura del Código

El juego está construido con HTML5, CSS3 y JavaScript vanilla. No requiere dependencias externas.

#### Componentes Principales

1. **Motor del Juego (`gameLoop`)**
   - Actualiza el estado del juego en cada frame
   - Maneja la lógica de movimiento y colisiones

2. **Sistema de Laberinto**
   - `generateMaze()`: Genera laberintos proceduralmente
   - `isWalkable()`: Verifica si una posición es transitable

3. **Personajes**
   - `player`: Objeto de la pingüina controlada por el jugador
   - `enemies`: Array de enemigos con IA básica

4. **Sistema de Audio**
   - Usa Web Audio API para efectos de sonido
   - Funciones: `collectSound()`, `gameOverSound()`, `levelCompleteSound()`, `hitSound()`

### Configuración del Juego

```javascript
const DIFFICULTY = {
  easy: { enemyCount: 2, enemySpeed: 1.5, lives: 5, pointsPerFish: 10 },
  medium: { enemyCount: 3, enemySpeed: 2, lives: 3, pointsPerFish: 20 },
  hard: { enemyCount: 4, enemySpeed: 2.5, lives: 2, pointsPerFish: 30 }
};
```

### Modificar el Juego

#### Cambiar el tamaño del laberinto
```javascript
const CELL_SIZE = 40;  // Tamaño de cada celda
const COLS = 20;        // Columnas del laberinto
const ROWS = 15;        // Filas del laberinto
```

#### Agregar nuevos niveles de dificultad
Añade una nueva entrada en el objeto `DIFFICULTY` con las propiedades deseadas.

#### Modificar la IA de los enemigos
Edita la función `updateEnemies()` para cambiar el comportamiento de persecución.

### Funciones Clave

- `startGame(difficulty)`: Inicia un nuevo juego con la dificultad especificada
- `initLevel()`: Inicializa un nivel con laberinto, jugador, enemigos y coleccionables
- `updatePlayer()`: Maneja el input del teclado y movimiento del jugador
- `updateEnemies()`: Actualiza la posición y comportamiento de los enemigos
- `checkCollisions()`: Detecta colisiones entre jugador, enemigos y coleccionables

## 📝 Licencia

Este proyecto es de código abierto y está disponible bajo la licencia MIT.

## 🤝 Contribuciones

Las contribuciones son bienvenidas. Por favor:
1. Haz fork del repositorio
2. Crea una rama para tu característica (`git checkout -b feature/nueva-caracteristica`)
3. Commit tus cambios (`git commit -m 'Añadir nueva característica'`)
4. Push a la rama (`git push origin feature/nueva-caracteristica`)
5. Abre un Pull Request

## 🐛 Reportar Bugs

Si encuentras un bug, por favor abre un issue en el repositorio con:
- Descripción del bug
- Pasos para reproducirlo
- Comportamiento esperado vs actual
- Navegador y versión

## 📧 Contacto

Para preguntas o sugerencias, abre un issue en el repositorio de GitHub.