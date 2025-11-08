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
