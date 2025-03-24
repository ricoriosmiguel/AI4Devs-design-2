# ATS Competitivo "LTI" (MVP)
## Product Requirements Document (PRD)

1. Introducción y Objetivos

1.1 Resumen del Producto

El ATS Competitivo es un sistema de seguimiento de candidatos diseñado para optimizar el proceso de reclutamiento mediante la automatización, IA predictiva y una experiencia de usuario intuitiva. Se desarrollará con Node.js (backend), PostgreSQL (base de datos) y React (frontend), siguiendo Clean Architecture, Domain Driven Design (DDD) y C4 Model.

1.2 Objetivos y Metas

Reducir el tiempo de contratación mediante flujos automatizados.

Mejorar la experiencia del usuario con una aplicación intuitiva y sin fricciones.

Ofrecer análisis predictivos para mejorar la selección de talento.

Integrarse con bolsas de empleo y sistemas de gestión de recursos humanos.

Garantizar la seguridad y cumplimiento normativo (GDPR, EEOC).

2. Stakeholders

Reclutadores / HR: Publican vacantes, gestionan candidatos y programan entrevistas.

Gerentes de Contratación: Aprueban contrataciones y revisan desempeño de candidatos.

Candidatos: Se postulan, realizan evaluaciones y dan seguimiento a su proceso.

Equipo de TI: Desarrolla y mantiene la plataforma.

Empresas Partner / Integraciones: Plataformas de evaluación, ERPs y bolsas de empleo.

3. Historias de Usuario

3.1 Reclutadores

Como reclutador, quiero crear y publicar vacantes para atraer talento.

Como reclutador, quiero filtrar y calificar candidatos para encontrar los mejores perfiles.

Como reclutador, quiero programar entrevistas y pruebas automáticamente.

3.2 Candidatos

Como candidato, quiero aplicar a vacantes con un clic desde LinkedIn o Google Jobs.

Como candidato, quiero recibir retroalimentación sobre mi proceso de selección.

Como candidato, quiero realizar pruebas en línea sin salir de la plataforma.

4. Arquitectura y Componentes Principales

4.1 Clean Architecture

Se implementará siguiendo los principios de Clean Architecture:

Capa de Presentación (React): UI basada en componentes, consumiendo APIs REST.

Capa de Aplicación (Node.js con NestJS/Express): Orquestación de casos de uso.

Capa de Dominio (DDD): Agregados, entidades y servicios de dominio.

Capa de Infraestructura (PostgreSQL, Redis, Integraciones): Persistencia y conectores externos.

4.2 Modelo C4

Nivel 1: Contexto

C4Context
  Person(Candidate, "Candidato", "Usuario que aplica a las vacantes")
  Person(Recruiter, "Reclutador", "Usuario que gestiona las vacantes")
  System(ATS, "ATS Competitivo", "Sistema que gestiona el reclutamiento")
  System_Ext(LinkedIn, "LinkedIn", "Fuente de reclutamiento externa")
  System_Ext(GoogleJobs, "Google Jobs", "Fuente de reclutamiento externa")
  
  Candidate --> ATS : "Postula a una vacante"
  Recruiter --> ATS : "Gestiona candidatos"
  ATS --> LinkedIn : "Publica vacantes"
  ATS --> GoogleJobs : "Publica vacantes"

Nivel 2: Contenedores

C4Container
  System_Boundary(ATS, "ATS Competitivo") {
    Container(Frontend, "React App", "SPA para usuarios")
    Container(Backend, "Node.js API", "NestJS para lógica de negocio")
    ContainerDB(Database, "PostgreSQL", "Almacenamiento de datos")
    Container(Queue, "Redis", "Cola de mensajes para eventos asíncronos")
  }
  Frontend --> Backend : "Consume APIs REST"
  Backend --> Database : "Guarda y obtiene datos"
  Backend --> Queue : "Envía eventos asíncronos"

Nivel 3: Componentes

C4Component
  Container(Backend, "Node.js API", "NestJS para lógica de negocio") {
    Component(JobModule, "Job Module", "Manejo de vacantes")
    Component(CandidateModule, "Candidate Module", "Manejo de postulaciones")
    Component(InterviewModule, "Interview Module", "Programación de entrevistas")
    Component(AnalyticsModule, "Analytics Module", "Análisis de contrataciones")
  }
  JobModule --> Database : "Guarda vacantes"
  CandidateModule --> Database : "Guarda postulaciones"
  InterviewModule --> Queue : "Publica eventos de entrevista"
  AnalyticsModule --> Database : "Consulta métricas"

5. Características y Funcionalidades Clave

5.1 Must-Have (MVP)

✅ Creación y publicación de vacantes.
✅ Postulación con un clic (LinkedIn, Google Jobs, Indeed).
✅ Parser de CV con IA.
✅ Flujos de contratación configurables.
✅ Evaluaciones en línea y agendamiento de entrevistas.
✅ Dashboard con métricas clave de reclutamiento.

5.2 Should-Have (Próximas versiones)

🔹 Firma digital de contratos (DocuSign o similar).
🔹 Predicción de tiempos de contratación con IA.
🔹 Multilenguaje (Español/Inglés/Portugués).

6. Requisitos Técnicos

Backend: Node.js (NestJS/Express), PostgreSQL, Prisma ORM.

Frontend: React + Vite + TailwindCSS.

Infraestructura: Docker, Kubernetes, AWS/GCP.

Integraciones: LinkedIn API, Google Jobs, Zapier, ERPs.

Seguridad: Autenticación JWT, OAuth2, Cifrado AES256.

En el contexto de Domain Driven Design (DDD) para tu ATS Competitivo, definimos los Bounded Contexts para segmentar claramente las responsabilidades dentro del sistema.

📌 Bounded Contexts del ATS Competitivo
Los Bounded Contexts representan diferentes dominios dentro del sistema y permiten modularidad, escalabilidad y una mejor organización del código.

1️⃣ Recruiting (Reclutamiento)
📌 Responsable de la Creación y Gestión de Vacantes

📍 Propósito
Maneja la creación, publicación y gestión de ofertas laborales. Se encarga de las integraciones con bolsas de empleo y los flujos de aprobación interna.

📌 Entidades y Agregados
Vacante (JobPosting) → Agregado raíz

Reclutador (Recruiter)

Fuente de Reclutamiento (JobSource)

Estado de Vacante (Abierta, Cerrada, En Proceso)

📌 Casos de Uso
✅ Crear y editar una vacante
✅ Publicar la vacante en portales externos
✅ Cerrar o pausar una vacante
✅ Filtrar y buscar vacantes

2️⃣ Application Management (Gestión de Candidatos y Postulaciones)
📌 Responsable del Registro, Filtrado y Seguimiento de Candidatos

📍 Propósito
Administra todas las postulaciones, permitiendo que los reclutadores gestionen candidatos y avancen en los procesos de selección.

📌 Entidades y Agregados
Candidato (Candidate) → Agregado raíz

Postulación (Application)

Historial de Aplicaciones (ApplicationHistory)

Calificación de Candidato (CandidateScore)

📌 Casos de Uso
✅ Postulación con un clic (LinkedIn, Google Jobs)
✅ Parser de CV y creación automática de perfil
✅ Filtros y scoring automático de candidatos
✅ Envío de notificaciones a candidatos

3️⃣ Evaluation & Interview (Evaluaciones y Entrevistas)
📌 Responsable de Pruebas, Evaluaciones y Entrevistas

📍 Propósito
Gestiona evaluaciones técnicas, pruebas psicométricas y entrevistas, integrándose con plataformas externas.

📌 Entidades y Agregados
Evaluación (Assessment) → Agregado raíz

Entrevista (Interview)

Entrevistador (Interviewer)

Resultados de Evaluación (AssessmentResult)

📌 Casos de Uso
✅ Creación y asignación de pruebas a candidatos
✅ Revisión y almacenamiento de resultados
✅ Agendamiento automático de entrevistas
✅ Grabación y análisis de entrevistas

4️⃣ Hiring & Offer Management (Contratación y Ofertas)
📌 Responsable del Proceso Final de Contratación

📍 Propósito
Maneja la creación, envío y firma de ofertas laborales, además de la integración con procesos de onboarding.

📌 Entidades y Agregados
Oferta de Trabajo (JobOffer) → Agregado raíz

Estado de la Oferta (Enviada, Aceptada, Rechazada)

Firma Digital (DigitalSignature)

📌 Casos de Uso
✅ Generar y personalizar ofertas laborales
✅ Enviar y gestionar aceptación/rechazo de ofertas
✅ Integración con firma electrónica (DocuSign)
✅ Generar contrato final automáticamente

5️⃣ Analytics & Insights (Analítica y Reportes)
📌 Responsable del Análisis de Reclutamiento y Predicción de Datos

📍 Propósito
Genera informes sobre rendimiento de contrataciones, efectividad de fuentes de reclutamiento y predicción de tiempos de contratación.

📌 Entidades y Agregados
Reporte de Contratación (HiringReport) → Agregado raíz

Predicción de Tiempo de Contratación (HiringPrediction)

Fuente de Reclutamiento Efectiva (RecruitmentSourceEfficiency)

📌 Casos de Uso
✅ Generar reportes de conversión por etapa
✅ Evaluar el desempeño de cada fuente de reclutamiento
✅ Predicción de tiempos de contratación basada en datos históricos

los diagramas C4 en formato Mermaid siguiendo la estructura definida en los Bounded Contexts y Clean Architecture.

Voy a incluir:

Diagrama de Contexto (Nivel 1) – Visión general del ATS y sus interacciones.

Diagrama de Contenedores (Nivel 2) – Componentes principales del sistema.

Diagramas de Componentes (Nivel 3) – Estructura interna de cada módulo clave.

Voy a generarlos ahora. ⏳

edición

Responde en el chat en su lugar
He agregado los diagramas C4 en formato Mermaid a tu PRD. Ahora incluye:

Nivel 1: Contexto – Relación del ATS con usuarios y sistemas externos.

Nivel 2: Contenedores – Estructura del frontend, backend y base de datos.

Nivel 3: Componentes – Módulos principales dentro del backend.

Si necesitas más detalles o ajustes en los diagramas, dime. 🚀