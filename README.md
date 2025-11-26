# Implementación y Análisis de un Clúster Hadoop HDFS

Este repositorio contiene los archivos de configuración, el informe final y la documentación del proyecto de implementación de un clúster de almacenamiento distribuido HDFS, realizado para el curso de **Computación de Alto Desempeño** en la Pontificia Universidad Javeriana.

El objetivo fue configurar un clúster HDFS funcional sobre un entorno multi-nodo con Rocky Linux, validar su operación y analizar su arquitectura y comportamiento.

## Arquitectura del Clúster

El clúster implementado sigue una arquitectura maestro/esclavo:
*   **1 Nodo Maestro (NameNode):** `cadhead01`
*   **5 Nodos Trabajadores (DataNodes):** `cad01-nfs01`, `cadcliente01`, `cad01-w000`, `cad01-w001`, `cad01-w002`.

Se configuró un factor de replicación (`dfs.replication`) de **3**, garantizando que cada bloque de datos se almacene en tres nodos distintos para una alta tolerancia a fallos.

## Estructura del Repositorio

```
.
├── conf/
│   ├── core-site.xml         # Configuración central (URI del NameNode)
│   ├── hadoop-env.sh         # Variables de entorno de Hadoop (JAVA_HOME, Opciones JVM)
│   ├── hdfs-site.xml         # Configuración de HDFS (rutas, replicación)
│   └── workers               # Lista de nodos DataNode
├── report/
│   ├── informe.pdf           # Informe final del taller en formato IEEE
│   ├── informe.tex           # Código fuente del informe en LaTeX
│   ├── arquitectura_hdfs.png # Diagramas e imágenes utilizadas
│   └── ...
├── sample_data/
│   ├── recuento.txt          # Archivos de texto de ejemplo para probar
│   └── CHARLIE.txt           # comandos HDFS
└── README.md                 # Este archivo
```

## Configuración

A continuación, se destacan las configuraciones más importantes implementadas en los archivos del directorio `conf/`.

### `core-site.xml`
Define el NameNode (`cadhead01`) como el punto de entrada para todo el sistema de ficheros.
```xml
<property>
    <name>fs.defaultFS</name>
    <value>hdfs://cadhead01:9000</value>
</property>```

### `hdfs-site.xml`
Especifica las rutas locales para los datos del NameNode y DataNodes, y establece el factor de replicación en 3.
```xml
<property>
    <name>dfs.namenode.name.dir</name>
    <value>file:/mnt/hadoop/NameNode</value>
</property>
<property>
    <name>dfs.datanode.data.dir</name>
    <value>file:/mnt/hadoop/DataNode</value>
</property>
<property>
    <name>dfs.replication</name>
    <value>3</value>
</property>
```

## Cómo Replicar la Implementación

Para replicar este clúster, se requiere un entorno similar (múltiples nodos con Linux) y seguir los pasos detallados en la sección "Implementación del Clúster HDFS" del informe (`/report/InformeHadoop.pdf`). Los pasos clave incluyen:
1.  Configuración de red (`/etc/hosts`) y autenticación SSH sin contraseña.
2.  Instalación de una JRE compatible (se utilizó OpenJDK 8).
3.  Ubicación de los archivos de este repositorio en el directorio `~/hadoop/etc/hadoop`.
4.  Creación de los directorios de datos (`/mnt/hadoop/...`) y asignación de permisos.
5.  Formateo del NameNode (`hdfs namenode -format`) e inicio de los servicios (`start-dfs.sh`).

## Autores

*   **Sebastián Acosta Lasso**
*   **Sebastian Fanchi Lopez**
*   **Sergio Ernesto Mesa Bernal**
*   **Juan Diego Peña Mayorga**
*   **Lizeth Portela Lozano**
*   **Jean Paul Rodríguez Moreno**