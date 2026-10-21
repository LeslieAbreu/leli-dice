# 📱 Juego de Memoria: Simón Dice (React Native + Expo)
 
Proyecto desarrollado para la materia utilizando **React Native** con **Expo**[cite: 1]. Consiste en el clásico juego interactivo *Simón Dice*, donde el sistema genera patrones de colores de forma progresiva y evalúa la memoria del usuario ronda tras ronda[cite: 1].
 
---
 
## 🛠️ Guía de Instalación y Ejecución
 
### Prerrequisitos
* Contar con **Node.js** instalado (se sugiere la versión LTS)[cite: 1].
* Aplicación **Expo Go** instalada en el celular (Android / iOS) o un navegador web en la computadora[cite: 1].
 
### Pasos para iniciar la aplicación
 
1. **Descargar o clonar el repositorio:**
   ```bash
   git clone <URL_DEL_REPOSITORIO_DE_LELI>
   cd simon-dice
   ```
 
2. **Instalar los paquetes necesarios:**
   ```bash
   npm install
   ```
 
3. **Levantar el entorno de desarrollo:**
   ```bash
   npx expo start
   ```
 
4. **Probar el juego:**
   * **Desde el celular:** Escanea el código QR mostrado en la consola usando la app **Expo Go**[cite: 1].
   * **Desde la PC:** Oprime la tecla **`w`** en la consola para ejecutar la vista web directamente en el navegador[cite: 1].
 
---
 
## 📖 Descripción y Funcionamiento del Juego
 
El objetivo principal es memorizar y repetir la serie de colores que la aplicación ilumina en pantalla[cite: 1]:
* Al tocar el botón **"Empezar"**, el sistema elige un primer color al azar y lo enciende durante `500 ms`[cite: 1].
* Cuando finaliza la demostración, se habilita el turno del jugador para que presione los cuadros en la misma secuencia[cite: 1].
* Cada vez que el jugador acierta toda la serie de la ronda actual, se añade un nuevo color aleatorio al final de la lista y se reproduce toda la secuencia desde el principio[cite: 1].
* Durante el turno de la máquina, los botones quedan deshabilitados para evitar pulsaciones accidentales[cite: 1].
* Si el jugador comete un error en cualquier posición, la partida concluye inmediatamente y se muestra en pantalla el puntaje obtenido[cite: 1].
 
---
 
## 🧩 Arquitectura y Diseño del Estado
 
Para controlar el ciclo de vida del juego de manera reactiva dentro de `app/index.tsx`, se definieron los siguientes estados mediante el hook `useState`[cite: 1, 2]:
 
* **Secuencia del juego (`sequence` — `string[]`):**
  Almacena en un arreglo ordenado todos los colores que la máquina va generando en cada nivel (`"red"`, `"green"`, `"blue"`, `"yellow"`)[cite: 1, 2]. Se eligió un arreglo de strings porque permite tanto iterar sobre él para iluminar los botones en orden, como consultar su tamaño (`sequence.length`) para determinar el nivel actual y el puntaje final sin requerir variables extra[cite: 1].
 
* **Índice de progreso del usuario (`playerIndex` — `number`):**
  Guarda un número entero (iniciando en `0` cada ronda) que representa la posición actual que el jugador está intentando adivinar dentro del arreglo `sequence`[cite: 1]. Con cada toque válido se incrementa en `1`, permitiendo verificar fácilmente si el color pulsado coincide con `sequence[playerIndex]` y saber cuándo se completó la ronda[cite: 1].
 
* **Control de turno activo (`isPlayerTurn` — `boolean`):**
  Bandera booleana que determina quién tiene el control en ese momento (`false` para la máquina o estado inactivo, `true` para el usuario)[cite: 1]. Es fundamental para bloquear la entrada táctil en la función `pressColor` mientras la secuencia automática sigue ejecutándose[cite: 1].
 
* **Indicador visual de color (`activeColor` — `string | null`):**
  Guarda temporalmente el nombre del color que debe resaltarse con opacidad al `100%` (`styles.padActive`), regresando a `null` cuando los botones deben verse apagados (`opacity: 0.35`)[cite: 1, 2].
 
* **Mensaje de estado (`status` — `string`):**
  Cadena de texto encargada de actualizar el encabezado de la interfaz para guiar al jugador en cada etapa (inicio, observación, turno de juego y fin de partida con puntuación)[cite: 1, 2].
 
### ⏱️ Manejo de tiempos en la reproducción
Dado que los ciclos en JavaScript se ejecutan de manera síncrona, al recorrer la secuencia con `forEach` se programan múltiples `setTimeout` con un **retardo escalonado** (`(index + 1) * 800` ms)[cite: 1]. De esta forma, cada botón se ilumina por `500 ms` y deja un margen de `300 ms` antes del siguiente destello, logrando una animación fluida sin congelar la interfaz de usuario[cite: 1].
 
 
---
 
## 👩‍💻 Autora
 
* **Leslie Marisol Abreu**