# AboutMe

graph TB
    %% ===== CAPA DE FUENTES DE DATOS =====
    subgraph FuentesDatos [CAPA DE FUENTES DE DATOS]
        WazuhManager[Wazuh Manager<br/>API REST]
        WazuhIndexer[Wazuh Indexer<br/>OpenSearch]
        Datasets[Datasets Historicos<br/>NSL-KDD, UNSW-NB15]
        ModelosPre[Modelos Pre-entrenados<br/>Autoencoder, RF, LSTM]
    end

    %% ===== CAPA DE EXTRACCIÓN =====
    subgraph Extraccion [CAPA DE EXTRACCIÓN]
        LibreriaAPI[Libreria API Wazuh v2.0<br/>Autenticacion/Paginacion]
        Consultas[Consultas API<br/>RESTful]
        Auth[Autenticacion<br/>API Keys]
    end

    %% ===== CAPA DE TRANSFORMACIÓN =====
    subgraph Transformacion [CAPA DE TRANSFORMACIÓN]
        Normalizacion[Normalizacion<br/>Estandarizacion]
        Limpieza[Limpieza Datos<br/>Outliers/Missing]
        FeatureEng[Feature Engineering<br/>Seleccion Caracteristicas]
    end

    %% ===== CAPA DE CLASIFICACIÓN =====
    subgraph Clasificacion [CAPA DE CLASIFICACIÓN]
        Severidad[Clasificacion Severidad<br/>Criticidad]
        Categoria[Clasificacion Categoria<br/>Tipo Amenaza]
        Anomalias[Deteccion Anomalias<br/>Comportamiento Inusual]
    end

    %% ===== CAPA DE MODELOS ML =====
    subgraph MLModels [CAPA DE MACHINE LEARNING]
        subgraph Entrenamiento [Entrenamiento Offline]
            CrossVal[Validacion Cruzada<br/>K-Fold]
            Hiperparams[Hiperparametros<br/>Optimizacion]
            ModelTraining[Entrenamiento Modelos<br/>Ajustar]
        end
        
        subgraph Inferencia [Inferencia Tiempo Real]
            Supervisados[Modelos Supervisados<br/>Clasificacion, Regresion]
            NoSupervisados[Modelos No Supervisados<br/>Clustering, Deteccion Anomalias]
        end
        
        subgraph Optimizacion [Optimizacion Continua]
            AprendizajeInc[Aprendizaje Incremental]
            FeedbackLoop[Feedback Loop<br/>Retroalimentacion]
            UpdateModel[Actualizacion Modelos]
        end
    end

    %% ===== CAPA DE IA EXPLICATIVA =====
    subgraph IAExplicativa [CAPA DE IA EXPLICATIVA]
        AnalisisContext[Analisis Contextual<br/>Correlacion Eventos]
        Explicacion[Explicacion Alertas<br/>Informe]
        Causalidad[Analisis Causal<br/>Root Cause]
    end

    %% ===== CAPA DE SALIDAS =====
    subgraph Salidas [CAPA DE SALIDAS Y SERVICIOS]
        Dashboard[Dashboard Web<br/>Monitoreo Tiempo Real]
        Alertas[Sistema Alertas<br/>Notificaciones]
        Reportes[Reportes Automaticos<br/>PDF/Excel]
        Acciones[Acciones Automaticas<br/>Mitigacion]
    end

    %% ===== BASE DE DATOS =====
    subgraph BaseDatos [BASE DE DATOS PERSISTENTE]
        DB[(PostgreSQL<br/>Modelos, Config, Historico)]
        Cache[Redis<br/>Cache]
        MLRepo[MLflow<br/>Repositorio Modelos]
    end

    %% ===== CONEXIONES PRINCIPALES =====
    %% Flujo principal de datos
    WazuhManager --> LibreriaAPI
    WazuhIndexer --> LibreriaAPI
    Datasets --> LibreriaAPI
    ModelosPre --> LibreriaAPI

    LibreriaAPI --> Normalizacion
    Normalizacion --> Limpieza
    Limpieza --> FeatureEng
    FeatureEng --> Severidad
    Severidad --> Categoria
    Categoria --> Anomalias

    Anomalias --> Supervisados
    Anomalias --> NoSupervisados

    Supervisados --> AnalisisContext
    Supervisados --> Explicacion
    NoSupervisados --> Causalidad
    NoSupervisados --> AnalisisContext

    AnalisisContext --> Dashboard
    Explicacion --> Alertas
    Causalidad --> Reportes
    Causalidad --> Acciones

    %% Conexiones base de datos
    LibreriaAPI --> DB
    FeatureEng --> DB
    Severidad --> DB
    Supervisados --> DB
    NoSupervisados --> DB
    AnalisisContext --> DB
    Dashboard --> DB

    %% Conexiones de entrenamiento
    DB --> CrossVal
    CrossVal --> Hiperparams
    Hiperparams --> ModelTraining
    ModelTraining --> Supervisados
    ModelTraining --> NoSupervisados

    %% Feedback loop
    Acciones --> FeedbackLoop
    Dashboard --> FeedbackLoop
    FeedbackLoop --> AprendizajeInc
    AprendizajeInc --> UpdateModel
    UpdateModel --> Supervisados
    UpdateModel --> NoSupervisados

    %% Cache y optimizacion
    LibreriaAPI --> Cache
    Supervisados --> Cache
    NoSupervisados --> Cache
    Dashboard --> Cache

    %% Repositorio modelos
    ModelTraining --> MLRepo
    UpdateModel --> MLRepo

    %% ===== ESTILOS Y FORMATOS =====
    classDef fuente fill:#e1f5fe,stroke:#01579b,stroke-width:2px
    classDef extraccion fill:#f3e5f5,stroke:#4a148c,stroke-width:2px
    classDef transformacion fill:#e8f5e8,stroke:#1b5e20,stroke-width:2px
    classDef clasificacion fill:#fff3e0,stroke:#e65100,stroke-width:2px
    classDef ml fill:#fce4ec,stroke:#880e4f,stroke-width:2px
    classDef ia fill:#e0f2f1,stroke:#004d40,stroke-width:2px
    classDef salida fill:#fff8e1,stroke:#ff6f00,stroke-width:2px
    classDef bd fill:#fafafa,stroke:#424242,stroke-width:3px

    class WazuhManager,WazuhIndexer,Datasets,ModelosPre fuente
    class LibreriaAPI,Consultas,Auth extraccion
    class Normalizacion,Limpieza,FeatureEng transformacion
    class Severidad,Categoria,Anomalias clasificacion
    class Entrenamiento,Inferencia,Optimizacion ml
    class AnalisisContext,Explicacion,Causalidad ia
    class Dashboard,Alertas,Reportes,Acciones salida
    class BaseDatos bd