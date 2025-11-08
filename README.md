# Despliegue del Proyecto PyBullet_Industrial_Robotics_Gym
## 1. Clonación del repositorio

Primero, se descarga el proyecto desde GitHub con:
```
git clone https://github.com/rparak/PyBullet_Industrial_Robotics_Gym.git
cd PyBullet_Industrial_Robotics_Gym
```
## 2. Creación del entorno virtual (Conda)

El autor recomienda crear un entorno virtual para mantener las dependencias aisladas.
Se utiliza Python 3.10 con Conda:
```
conda create -n pybullet_env python=3.10
conda activate pybullet_env
```
## 3. Instalación de dependencias

El autor indica los paquetes necesarios para ejecutar el proyecto.
Se instalaron uno por uno usando Conda desde el canal conda-forge:
```
conda install -c conda-forge matplotlib
conda install -c conda-forge scienceplots
conda install -c conda-forge pybullet
conda install -c conda-forge pandas
conda install -c conda-forge scipy
conda install pytorch::pytorch torchvision torchaudio -c pytorch
conda install -c conda-forge stable-baselines3
conda install -c conda-forge gymnasium
```
## 4. Ejecución del archivo principal

Una vez instaladas todas las dependencias, se intentó ejecutar el archivo principal del proyecto:
```
python src/PyBullet/Configuration/Environment.py
```
## 5. Problemas de ejecución

Durante la ejecución se detectó que el proyecto depende de un módulo externo llamado RoLE, el cual no está incluido en el repositorio ni disponible a través de pip o conda.

El error presentado fue el siguiente:
```
NameError: name 'HTM_Cls' is not defined
```

La clase HTM_Cls debería ser importada desde el módulo RoLE, pero el entorno no logra encontrar ni cargar dicho módulo, lo que impide la inicialización del entorno Environment E1.

Para intentar resolverlo, se clonó el repositorio complementario manualmente:
```
git clone https://github.com/rparak/RoLE.git
```

Sin embargo, al intentar enlazarlo manualmente al proyecto (mediante rutas absolutas o la variable de entorno PYTHONPATH), el error persistió, por lo que el entorno no logra reconocer las clases del módulo RoLE.

Esto sugiere que el autor podría haber modificado la estructura del código sin actualizar las rutas de importación o dependencias.

## 6. Despliegue mediante Docker

También se intentó ejecutar el proyecto dentro de un contenedor Docker.
Para ello, se creó un archivo Dockerfile con el siguiente contenido:

### Imagen base con Python 3.10
```
FROM python:3.10-slim
```
### Instalar dependencias del sistema
```
RUN apt-get update && apt-get install -y \
    git \
    python3-tk \
    xvfb \
    && rm -rf /var/lib/apt/lists/*
```
### Crear el directorio de trabajo
WORKDIR /app

### Copiar todo el código al contenedor
```
COPY . /app
```
### Instalar dependencias del proyecto
```
RUN pip install --upgrade pip
RUN pip install -r requirements.txt || true
```
### Comando por defecto
```
CMD ["python", "src/PyBullet/Configuration/Environment.py"]
```

Luego se construyó y ejecutó la imagen con:
```
docker build -t pybullet-gym .
docker run -it --rm pybullet-gym
```

El resultado fue el mismo: el programa no logra ejecutar el entorno Environment E1 debido a la falta del módulo RoLE.

## 7. Conclusión

A pesar de seguir las instrucciones del autor tanto en Conda como en Docker, el proyecto no puede ejecutarse correctamente debido a que depende de un módulo externo no integrado ni documentado adecuadamente (RoLE).

Incluso clonando dicho módulo por separado, no se logra importar correctamente la clase HTM_Cls ni establecer la conexión entre ambos repositorios.

Por tanto, el entorno no puede desplegarse completamente sin una intervención adicional del autor o una revisión profunda de las rutas de importación internas.

## 8. Imagenes



## 9. 5 patentes seleccionadas (fichas)
#### 1) Quantum reinforcement learning agent — US20210049498 A1 (IBM)

Assignee: International Business Machines Corp (IBM). Publicación: 2021.
Resumen: Describe un agente de reinforcement learning que codifica estados del entorno en qubits, usa componentes variacionales y muestreo cuántico para seleccionar acciones; se orienta a entornos de estado continuo (aplicaciones explícitas en robótica). 
Patentes de Google

Por qué interesa para robótica con sistemas digitales + computación cuántica: propone usar ventajas de muestreo/capacidad cuántica para acelerar el aprendizaje de políticas (p. ej. navegación, manipulación). 
Patentes de Google

Aplicaciones industriales: robots móviles que aprenden rutas en entornos no estructurados; drones en búsqueda/rescate que deben aprender políticas rápidamente; manipuladores que optimizan agarres en un espacio continuo. 
Patentes de Google

Claim destacado: método para mapear un estado del entorno a qubits y combinar una política de RL con muestreos cuánticos para producir una distribución probabilística de acciones. 
Patentes de Google

#### 2) Quantum computing machine learning module — US10275721 B2 (Accenture)

Assignee: Accenture Global Solutions Ltd. Publicación: 2019 (familia iniciada 2017).
Resumen: Módulo de ML que decide/dirige cuándo y cómo enrutar tareas computacionales entre recursos clásicos y cuánticos (gate, annealers, simuladores), optimizando costos/tiempos; contempla aplicaciones de optimización en tiempo real (mencionan ejemplos que incluyen sistemas de conducción y evitar colisiones). 
Patentes de Google

Por qué interesa para robótica + computación cuántica: sirve como middlewareque decide si una tarea (p. ej. planificación de trayectoria compleja, optimización multi-robot) debe ejecutarse en un back-end cuántico o clásico — crítico en sistemas robóticos híbridos con componentes digitales y conectividad a la nube/cuántica. 
Patentes de Google

Aplicaciones industriales: flotas de AGVs que delegan optimización de rutas; sistemas de control que seleccionan simulación cuántica para problemas NP-hard de planificación o asignación. 
Patentes de Google

Claim destacado: método para entrenar un modelo que rote tareas entre recursos cuánticos y clásicos usando historial de tareas, propiedades de recursos y coste/tiempo estimado. 
Patentes de Google

#### 3) Quantum computing-enhanced systems and methods for large-scale constrained optimization — US20230419155 A1 (Cornell University)

Assignee: Cornell University. Publicación: 2023 (familia iniciada 2019).
Resumen: Plataforma híbrida clásica/cuántica que descompone problemas de optimización en subproblemas, asignando ciertos subproblemas a procesador cuántico (p. ej. QUBO) y otros a clásico; incluye métodos iterativos de convergencia y ejemplos concretos (vehicle routing, manufactura celular). 
Patentes de Google

Por qué interesa para robótica: muchos problemas robóticos industriales (asignación de tareas multi-robot, vehicle routing para flotas autónomas, scheduling en fábricas robotizadas) son optimizaciones con restricciones; este enfoque híbrido acelera o mejora soluciones prácticas. 
Patentes de Google

Aplicaciones industriales: planificación/dispatching de flotas autónomas; optimización de línea de producción con robots colaborativos; diseño óptimo de trayectorias multi-robot con restricciones. 
Patentes de Google

Claim destacado: método iterativo donde se descompone el problema, resuelve subproblemas en classical/quantum y se reitera hasta convergencia (mencionan vehicle routing y problemas MILP/QUBO). 
Patentes de Google

#### 4) Techniques for control of quantum systems and related systems and methods (waveform processor para control de qubits) — WO2017139683 A8 (Yale University)

Assignee: Yale University (inventores: Schoelkopf, Devoret, Ofek, etc.). Publicación: 2017 (familia 2016–2017).
Resumen: Procesador de formas de onda y secuenciadores para generar/analizar señales analógicas/digitales usadas en control fino de sistemas cuánticos (p. ej. qubits). Incluye generación de secuencias digitales que se transforman a analógicas, análisis de respuesta, retroalimentación en tiempo real. 
Patentes de Google

Por qué interesa para robótica cuántica/digital: hardware de control cuántico de alta precisión es la base para que recursos cuánticos (qpu) estén disponibles para sistemas robóticos conectados; además, técnicas de control y medición en tiempo real pueden trasladarse a sensores cuánticos integrados en plataformas robóticas. 
Patentes de Google

Aplicaciones industriales: control fiable de qpus integrados en centros de datos para servicios de planificación/optimización de robots; sensores cuánticos (magnetometría, gravimetría) integrados en vehículos robóticos para inspección de precisión. 
Patentes de Google

Claim destacado: arquitectura con secuenciadores duales (master digital y generador de forma de onda analógica) y analizador de forma de onda para control y medición de sistemas cuánticos en lazo de retroalimentación. 
Patentes de Google

#### 5) Radar systems and methods using entangled quantum particles — EP1750145 (familia Lockheed Martin)

Assignee: Lockheed Martin Corporation. Publicación (EP): 2007 (familia desde 2005).
Resumen: Propone un sistema de radar basado en pares/beam de partículas entrelazadas; la técnica busca combinar alcance (long wavelength) y resolución (short wavelength) aprovechando propiedades cuánticas para detección en entornos cubiertos o con contramedidas. 
patentimages.storage.googleapis.com
+1

Por qué interesa para robótica + computación cuántica: sensores cuánticos avanzados (como el quantum radar o sensores basados en entrelazamiento) podrían incorporarse a plataformas robóticas para detección de objetos ocultos, navegación en entornos adversos o inspección industrial con mayor precisión. 
patentimages.storage.googleapis.com

Aplicaciones industriales: vehículos robóticos de inspección en entornos con polvo/obstáculos, sistemas de vigilancia autónoma de infraestructuras críticas, integración en UAVs/robots para detección avanzada. 
patentimages.storage.googleapis.com

Claim destacado: generador de partículas entrelazadas que forma la señal transmitida y procesamiento para determinar características del objetivo usando partículas retornadas; se propone variar frecuencias/número de partículas para adaptarse al medio. 
patentimages.storage.googleapis.com

### Breve análisis de tendencias (observadas en estas patentes)

#### Híbrido clásico–cuántico: muchas invenciones buscan un balance entre recursos clásicos y cuánticos (routing de tareas, descomposición de optimización). Esto es clave para robótica industrial donde la latencia, coste y disponibilidad del QPU varían. 
(https://patents.google.com/patent/US10275721B2/en)

#### Aplicación a optimización y RL: problemas típicos de robótica (planning, asignación, control) aparecen como casos de uso documentados en las patentes. 
(https://patents.google.com/patent/US20210049498A1/en)

#### Capas de infraestructura y control: hay inversión en hardware de control cuántico y en middleware (waveform processors, routing modules) para que la computación cuántica sea práctica desde el punto de vista operativo. 
(https://patents.google.com/patent/WO2017139683A8/en)

#### Sensórica cuántica: sensores basados en principios cuánticos (p. ej. radar cuántico) abren nuevas funcionalidades para robots móviles. 
(https://patentimages.storage.googleapis.com/cc/7d/e3/44cac7998ffd0c/EP1750145A3.pdf?utm_source=chatgpt.com)

## 10. Referencias (patentes citadas — abreviadas)

US20210049498 A1 — Quantum reinforcement learning agent (IBM). 
Patentes de Google

US10275721 B2 — Quantum computing machine learning module (Accenture). 
Patentes de Google

US20230419155 A1 — Quantum computing-enhanced systems for large-scale constrained optimization (Cornell University). 
Patentes de Google

WO2017139683 A8 — Techniques for control of quantum systems (waveform processor) (Yale). 
Patentes de Google

EP1750145 (familia) — Radar systems and methods using entangled quantum particles (Lockheed Martin). 
patentimages.storage.googleapis.com
