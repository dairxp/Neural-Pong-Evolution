# Manual Técnico: T-Rex AI (Algoritmos Genéticos y Redes Neuronales)

Este documento detalla la arquitectura, algoritmos, lógica matemática y procedimientos de ingeniería utilizados para automatizar el clásico juego del T-Rex mediante Inteligencia Artificial iterativa.

---

## 1. ALGORITMO USADO: Neuroevolución y Algoritmos Genéticos
El sistema central de inteligencia utiliza una aproximación de **Neuroevolución**. En lugar de usar frameworks como TensorFlow o PyTorch con retropropagación (Backpropagation), el modelo construye una topología neuronal simple y utiliza un **Algoritmo Genético** para evolucionar los pesos de la red. 

Esta técnica se basa en la teoría de la evolución de Darwin adaptada al código:
1. **Generación de Población Inicial:** Se instancia un número "N" de dinosaurios simultáneos (ej. 100), cada uno con una red neuronal inicializada con pesos aleatorios.
2. **Evaluación (Fitness):** Se ejecuta el juego. El puntaje o distancia recorrida por cada dinosaurio antes de chocar determina su nivel de "aptitud" matemáticamente.
3. **Selección y Cruce (Crossover):** Cuando todos los dinosaurios colisionan, el algoritmo selecciona a la élite (los que llegaron más lejos). Sus "cerebros" (matrices de pesos) se combinan para concebir la próxima generación.
4. **Mutación:** Un pequeño porcentaje de aleatoriedad es inyectado a los nuevos genes para asegurar la exploración constante de nuevas tácticas (ej. evitar saltar muy pronto).

---

## 2. ARQUITECTURA DE LA RED NEURONAL PREDICITIVA
Cada dinosaurio posee un "cerebro" basado en una **Red Neuronal Artificial (Perceptrón Multicapa)** integrado nativamente en TypeScript.

### El Espacio de Observación (Inputs)
Para que el agente pueda tomar una decisión, sus sensores captan la física métrica del entorno en tiempo real:
- **Distancia:** Proximidad en el eje X hacia el obstáculo más cercano.
- **Ancho del obstáculo:** Para inferir cuánto tiempo debe mantener el salto presionado.
- **Altura del obstáculo:** Clave para distinguir si debe saltar (Cactus altos) o agacharse (Pterodáctilos aéreos).
- **Velocidad del entorno:** El juego acelera progresivamente, por ende la IA necesita la velocidad como input paramétrico para ajustar los tiempos de reacción de sus acciones.

### El Espacio de Acción (Outputs)
La capa de salida de la red de cada individuo se procesa a través de una función de activación, típicamente resultando en dimensiones discretas binarias:
- **Acción 1:** Ejecutar Salto.
- **Acción 2:** Ejecutar Agache.
- **Acción 3:** Mantener posición de carrera base.

---

## 3. PROGRAMACIÓN ORIENTADA A OBJETOS (POO) EN VITE + TYPESCRIPT
El juego base monolítico en JavaScript fue convertido a TypeScript y dividido en módulos rígidos orientados a objetos. Esta refactorización hace posible la inyección asincrónica del algoritmo evolutivo:

- **Clase Trex:** Administra la física métrica individual (gravedad, fuerza de salto), la máquina de estado (corriendo, saltando o estrellado) y guarda la referencia de la Red Neuronal de ese individuo específico.
- **Gestión de Población:** Un controlador sobre la clase base genera un array vectorial de objetos `Trex`, y sobreescribe los "inputs de teclado" nativos con las deducciones generadas por las matrices de peso en cada ciclo de dibujo (`requestAnimationFrame`).

---

## 4. PROCESO DE ENTRENAMIENTO SIN SUPERVISIÓN HUMANA

El modelo se entrena en la máquina del cliente sin servidor externo (a diferencia de PPO en Pong). 
- **Simulación Ciega Celerada:** Gracias a que se independizó la lógica matemática del renderizado visual (Canvas), el entrenamiento puede desvincularse de la barrera de 60 fotogramas por segundo. El algoritmo puede procesar múltiples generaciones de cientos de entidades por segundo.
- **La Solución al Óptimo Local:** Al principio, toda la población se estrella en el primer cactus. Por mera entropía probabilidad, un individuo pulsará "salto". Este simple logro le otorgará un Fitness superlativo, volviéndose el padre del 90% de la siguiente ola. Posteriormente, el desafío será que esos descendientes aprendan a detener la caída a tiempo o agacharse en obstáculos en el aire.

---

## 5. MÉTRICAS DE RECOMPENSA (FITNESS REWARD)
El moldeado funcional de la recompensa no se penaliza por muerte. 
En la Neuroevolución del T-Rex:
* `Puntuacion Fitness = Distancia Total en Píxeles Sobrevivida`
El agente evoluciona de manera puramente egoísta. No hay recompensas interaccionales; la métrica de éxito es lineal a la cantidad de obstáculos evadidos con éxito sobre el horizonte virtual.
