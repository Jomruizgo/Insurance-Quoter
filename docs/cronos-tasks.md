# Resumen documento para Cronos

## 1. Sección 1

# Extracción de Requerimientos

He analizado el proyecto y he identificado los siguientes elementos:

##  Épicas

| ID | Título | Descripción | Features Asociadas |
|---|---|---|---|
| **EP-001** | Gestión del Ciclo de Vida de Cotizaciones (Backend Core) | Esta épica se enfoca en las funcionalidades backend para crear, persistir y gestionar los datos fundamentales de una cotización de seguro de daños. Garantiza la integridad de los datos, maneja concurrencia y provee las APIs necesarias para información general, ubicaciones de riesgo y opciones de cobertura. | FT-001, FT-002, FT-003 |
| **EP-002** | Interfaz de Usuario y Experiencia (Frontend) | Esta épica cubre el desarrollo de la SPA (Single Page Application) que permite a los usuarios interactuar con el sistema de cotización. Incluye funcionalidades para captura de datos, navegación y visualización clara del avance de la cotización, resultados financieros y alertas relevantes. | FT-001, FT-002, FT-003, FT-005 |
| **EP-003** | Motor de Cálculo de Primas y Reglas de Negocio | Esta épica está dedicada a implementar la lógica de negocio principal para el cálculo de primas netas y comerciales. Incluye la validación de ubicaciones de riesgo, manejo adecuado de datos incompletos y asegura que el cálculo sea consistente, trazable y conforme a los componentes técnicos especificados. | FT-004 |
| **EP-004** | Integración con Servicios de Referencia | Esta épica se enfoca en la integración con el servicio plataforma-core-ohs (o su simulación) para obtener datos de referencia esenciales como catálogos (suscriptores, agentes, giros, códigos postales, clasificaciones de riesgo, garantías) y tarifas técnicas, así como la generación secuencial de folios. | FT-006 |
| **EP-005** | Aseguramiento de Calidad y Fundamentos Técnicos | Esta épica abarca todas las actividades relacionadas con asegurar la alta calidad, testabilidad, mantenibilidad y documentación integral de la solución. Incluye pruebas unitarias, pruebas automatizadas de flujos críticos, pruebas de aceptación Serenity BDD, modelado de datos claro, trazabilidad de la lógica de cálculo y mecanismos básicos de seguridad. | FT-007, FT-008 |

**Total:** 5 épicas

##  Features

| Epic ID | ID Feature | Título | Descripción | Prioridad | Historias |
|---|---|---|---|---|---|
| **EP-001** | **FT-001** | Gestión de Folios y Datos Generales | Permite crear, recuperar, capturar y guardar la información general de una cotización, incluyendo datos del asegurado, tipo de conducción y clasificación de riesgo, asegurando idempotencia en la creación de folios mediante un token de idempotencia gestionado por el backend. | Alta | US-001, US-002, US-003, US-004, US-005, US-006 |
| **EP-001** | **FT-002** | Administración de Ubicaciones de Riesgo | Facilita la definición de opciones de layout ('Ubicación única' y 'Múltiples ubicaciones') y el registro, consulta y edición de una o varias ubicaciones de riesgo asociadas a una cotización, con detalles como dirección, tipo constructivo y giro. | Alta | US-007, US-008, US-009, US-010, US-011 |
| **EP-001** | **FT-003** | Configuración y Gestión de Coberturas | Permite al usuario configurar y guardar opciones de cobertura para una cotización, así como consultar las existentes. | Media | US-012, US-013, US-014 |
| **EP-003** | **FT-004** | Motor de Cálculo de Primas | Componente backend responsable de ejecutar el cálculo de primas netas y comerciales para cada ubicación válida, consolidar la prima total y persistir el resultado financiero con versionado optimista usando un campo de versión en el modelo de datos, generando alertas para ubicaciones incompletas. | Alta | US-015, US-016, US-017, US-018 |
| **EP-002** | **FT-005** | Visualización de Resultados y Alertas | Proporciona una interfaz frontend para mostrar el desglose financiero (prima neta, prima comercial, primas por ubicación), el estado de la cotización y alertas relevantes, especialmente para ubicaciones con datos faltantes o inválidos. | Alta | US-019, US-020, US-021, US-022 |
| **EP-004** | **FT-006** | Integración con Servicios de Referencia | Gestiona la comunicación con el servicio plataforma-core-ohs (o su simulación) para obtener catálogos (suscriptores, agentes, giros, códigos postales, clasificación de riesgo, garantías), tarifas técnicas y generación secuencial de folios. | Alta | US-023, US-024, US-025, US-026, US-027 |
| **EP-005** | **FT-007** | Calidad Técnica y Trazabilidad | Asegura calidad de código, cobertura de pruebas unitarias y automatizadas (incluyendo Serenity BDD), completitud de la documentación técnica y operativa, claridad en el modelado de datos y trazabilidad de la lógica de cálculo, y mecanismos básicos de seguridad. | Alta | US-028, US-029, US-030, US-031, US-032, US-033 |
| **EP-005** | **FT-008** | Seguridad y Gestión de Accesos | Implementa mecanismos básicos de autenticación para controlar el acceso al sistema de cotización, asegurando que solo usuarios autorizados puedan interactuar con los servicios frontend y backend. | Alta | US-034 |

**Total:** 8 features

## Historias de Usuario

| Epic ID | Feature ID | ID HU | Título | Descripción | Prioridad |
|---|---|---|---|---|---|
| **EP-001** | **FT-001** | **US-001** | Crear nuevo folio de cotización | Como usuario de cotización, quiero iniciar una nueva cotización para obtener un folio único y comenzar a registrar información. | Alta |
| **EP-001** | **FT-001** | **US-002** | Recuperar folio de cotización existente | Como usuario de cotización, quiero abrir un folio de cotización previamente guardado para continuar o revisar una cotización existente. | Alta |
| **EP-001** | **FT-001** | **US-003** | Capturar datos generales de la cotización | Como usuario de cotización, quiero ingresar la información general de la cotización (asegurado, conducción, clasificación de riesgo) para establecer el contexto inicial de la póliza. | Alta |
| **EP-001** | **FT-001** | **US-004** | Consultar datos generales de la cotización | Como usuario de cotización, quiero ver los datos generales de una cotización para revisar la información básica del asegurado y el riesgo. | Media |
| **EP-001** | **FT-001** | **US-005** | Guardar datos generales de la cotización | Como usuario de cotización, quiero guardar los cambios realizados en los datos generales de una cotización para asegurar que la información actualizada se persista. | Alta |
| **EP-001** | **FT-001** | **US-006** | Asegurar idempotencia en la creación de folios | Como desarrollador backend, quiero crear folios de cotización con idempotencia, donde el backend genere y gestione un token de idempotencia para cada solicitud de creación, para evitar duplicidad de folios si la operación se reintenta. | Alta |
| **EP-001** | **FT-002** | **US-007** | Definir estructura de ubicaciones (layout) | Como usuario de cotización, quiero establecer la configuración de layout para ubicaciones de riesgo, eligiendo entre opciones de 'Ubicación única' y 'Múltiples ubicaciones', para organizar cómo se presentarán y gestionarán las ubicaciones en la cotización. | Alta |
| **EP-001** | **FT-002** | **US-008** | Registrar una nueva ubicación de riesgo | Como usuario de cotización, quiero capturar los detalles de una nueva ubicación de riesgo para agregar un punto de riesgo a la cotización. | Alta |
| **EP-001** | **FT-002** | **US-009** | Editar una ubicación de riesgo existente | Como usuario de cotización, quiero modificar los detalles de una ubicación de riesgo ya registrada para corregir o actualizar la información del riesgo. | Alta |
| **EP-001** | **FT-002** | **US-010** | Consultar ubicaciones de riesgo de un folio | Como usuario de cotización, quiero ver todas las ubicaciones de riesgo asociadas a un folio para revisar el conjunto completo de riesgos a cotizar. | Media |
| **EP-001** | **FT-002** | **US-011** | Visualizar resumen de ubicaciones | Como usuario de cotización, quiero ver un resumen del estado de las ubicaciones de riesgo para entender rápidamente cuáles están completas o requieren atención. | Media |
| **EP-001** | **FT-003** | **US-012** | Configurar opciones de cobertura para la cotización | Como usuario de cotización, quiero seleccionar y ajustar opciones de cobertura para la póliza, para personalizar la protección ofrecida al asegurado. | Media |
| **EP-001** | **FT-003** | **US-013** | Guardar opciones de cobertura seleccionadas | Como usuario de cotización, quiero persistir las opciones de cobertura configuradas para asegurar que la selección de coberturas se mantenga para el folio. | Media |
| **EP-001** | **FT-003** | **US-014** | Consultar opciones de cobertura de un folio | Como usuario de cotización, quiero ver las opciones de cobertura previamente configuradas para un folio para revisar la protección que se está cotizando. | Baja |
| **EP-003** | **FT-004** | **US-015** | Ejecutar cálculo de prima neta y comercial | Como usuario de cotización, quiero iniciar el proceso de cálculo de primas para un folio para obtener los valores financieros de la cotización. | Alta |
| **EP-003** | **FT-004** | **US-016** | Persistir resultado financiero con versionado optimista | Como desarrollador backend, quiero guardar el resultado del cálculo de primas sin sobrescribir otras secciones de la cotización, usando un campo de versión en el modelo de datos para versionado optimista, para mantener la integridad de los datos y permitir actualizaciones concurrentes. | Alta |
| **EP-003** | **FT-004** | **US-017** | Generar alertas para ubicaciones incompletas | Como desarrollador backend, quiero identificar y marcar ubicaciones de riesgo con datos faltantes o inválidos para informar al usuario sobre información pendiente sin detener el cálculo general. | Alta |
| **EP-003** | **FT-004** | **US-018** | Validar ubicaciones para el cálculo de primas | Como desarrollador backend, quiero verificar la completitud y validez de los datos de cada ubicación de riesgo, asegurando que dirección y código postal estén presentes y sean válidos, para que solo ubicaciones elegibles se incluyan en el cálculo de primas. | Alta |
| **EP-002** | **FT-005** | **US-019** | Mostrar prima neta y comercial | Como usuario de cotización, quiero ver la prima neta y comercial total calculada para entender el costo total del seguro. | Alta |
| **EP-002** | **FT-005** | **US-020** | Mostrar desglose de primas por ubicación | Como usuario de cotización, quiero ver el detalle de primas calculadas para cada ubicación de riesgo para entender cómo se distribuye el costo por punto de riesgo. | Media |
| **EP-002** | **FT-005** | **US-021** | Mostrar alertas para ubicaciones incompletas | Como usuario de cotización, quiero ser informado sobre ubicaciones de riesgo con datos faltantes o inválidos para saber qué información debo completar o corregir. | Alta |
| **EP-002** | **FT-005** | **US-022** | Visualize folio progress and status | As a quotation user, I want to see the current status of the quotation process, So that I can know what stage the folio is in and what steps are missing. | Media |
| **EP-004** | **FT-006** | **US-023** | Consult subscriber, agent, and business line catalogs | As a quotation user, I want to select values from catalogs for subscribers, agents, and business lines, So that I can ensure the consistency and validity of the data entered in the quote. | Alta |
| **EP-004** | **FT-006** | **US-024** | Consult and validate postal codes | As a quotation user, I want to enter a postal code and have the system validate and autocomplete information, So that I can ensure the accuracy of the risk locations' addresses. | Alta |
| **EP-004** | **FT-006** | **US-025** | Consult risk classification and guarantee catalogs | As a quotation user, I want to access risk classification and guarantee catalogs, So that I can select the correct values that influence premium calculation. | Media |
| **EP-004** | **FT-006** | **US-026** | Consult tariffs and technical factors for calculation | As a backend developer, I want to obtain the necessary tariffs and technical factors from the reference service, So that I can use them in the premium calculation engine. | Alta |
| **EP-004** | **FT-006** | **US-027** | Generate sequential folios via reference service | As a backend developer, I want to request the generation of a new folio from the reference service, So that I can ensure folios are unique and follow a defined sequence. | Alta |
| **EP-005** | **FT-007** | **US-028** | Implement unit tests with 80% coverage | As a developer, I want to write unit tests for the backend and frontend code, So that I can ensure a minimum coverage of 80% and code quality. | Alta |
| **EP-005** | **FT-007** | **US-029** | Implement automated critical flow tests | As a QA developer, I want to automate at least 3 critical business flows: 1) Folio creation, general data capture, and complete location registration; 2) Incomplete location registration, coverage configuration, and calculation execution; 3) Results visualization (calculated premium for valid location and alert for incomplete location) and final folio status consultation, So that I can validate the end-to-end functionality of the application. | Alta |
| **EP-005** | **FT-007** | **US-030** | Implement acceptance tests with Serenity BDD | As a QA developer, I want to develop acceptance tests using Serenity BDD in a separate repository, So that I can validate the system's behavior from a business perspective and generate clear reports. | Alta |
| **EP-005** | **FT-007** | **US-031** | Generate complete technical and operational documentation | As a development team member, I want to create exhaustive technical and operational documentation, including API contracts in OpenAPI/Swagger or Postman Collection format, So that I can facilitate the understanding, maintenance, and deployment of the solution. | Alta |
| **EP-005** | **FT-007** | **US-032** | Model Quote and Location data | As a data architect, I want to design a clear data model for Quote and Location, including essential attributes for Quote such as creation date, last modification date, folio status ('Draft', 'Calculated'), insured ID, insured name, driving type, and risk classification, So that I can ensure efficient persistence and data integrity. | Alta |
| **EP-005** | **FT-007** | **US-033** | Ensure traceability of calculation logic | As a backend developer, I want to implement the premium calculation logic in a consistent and documented manner, proposing a simplified logic that is arbitrary but yields a numeric result, So that I can facilitate its auditing and future understanding. | Alta |
| **EP-005** | **FT-008** | **US-034** | Implement Basic User Authentication | As a system administrator, I want to configure basic user authentication (username/password), So that I can control access to the frontend and backend of the quoter for demonstration purposes. | Alta |

**Total:** 34 historias de usuario

###  Criterios de Aceptación

**US-001 - Create new quotation folio:**
- ✓ Given the user accesses the quoter, when they select "Create new folio", then the system generates a new unique folio.
- ✓ Given a new folio is generated, when the system persists it, then the general data capture screen is displayed.

**US-002 - Retrieve existing quotation folio:**
- ✓ Given the user accesses the quoter, when they enter an existing and valid folio number, then the system loads the quote information associated with that folio.
- ✓ Given an existing folio is loaded, when the system displays it, then the user can view and edit the previously recorded data.

**US-003 - Capture general quote data:**
- ✓ Given the user is on the general data screen, when they enter the required information (e.g., insured's name, driving type, risk classification), then the system validates the entered data.
- ✓ Given the data is valid, when the user saves the information, then the general data is correctly persisted and associated with the folio.

**US-004 - Consult general quote data:**
- ✓ Given the user has retrieved a folio, when they navigate to the general data section, then the system displays the previously captured information.
- ✓ Given the data is displayed, when the user reviews it, then they can identify if the information is correct or needs editing.

**US-005 - Save general quote data:**
- ✓ Given the user has modified the general data, when they click the "Save" button, then the system persists the changes.
- ✓ Given the changes have been saved, when the user consults the folio again, then the general data reflects the latest modifications.

**US-006 - Ensure idempotency in folio creation:**
- ✓ Given a request is made to create a folio with a specific identifier, when the request is processed for the first time, then a new folio is created.
- ✓ Given the same request is made to create a folio with the same identifier, when the request is processed again, then the system returns the existing folio without creating a new one.

**US-007 - Define location structure (layout):**
- ✓ Given the user is in the locations section, when they select a layout option (e.g., single or multiple locations), then the system adjusts the interface to allow capture according to the layout.
- ✓ Given a layout is defined, when the user saves the configuration, then the layout is persisted for the current folio.

**US-008 - Register a new risk location:**
- ✓ Given the user is in the locations section, when they select "Add location" and enter the details (address, constructive type, business line), then the system validates the information.
- ✓ Given the information is valid, when the user saves the location, then the new location is associated with the folio and persisted.

**US-009 - Edit an existing risk location:**
- ✓ Given the user has selected an existing location for editing, when they modify its details (e.g., address, constructive type) and save, then the system validates the changes.
- ✓ Given the changes are valid, when the user saves the location, then the location information is updated in the folio with optimistic versioning.

**US-010 - Consult risk locations for a folio:**
- ✓ Given the user has retrieved a folio, when they navigate to the locations section, then the system displays a list of all registered locations for that folio.
- ✓ Given the locations are displayed, when the user reviews them, then they can identify their status and main details.

**US-011 - Visualize location summary:**
- ✓ Given the user is in the locations section, when the system loads the information, then a summary is displayed indicating the number of locations and their status (e.g., complete, incomplete).
- ✓ Given the summary is displayed, when the user reviews it, then they can identify if there are locations with missing or invalid data.

**US-012 - Configure coverage options for the quote:**
- ✓ Given the user is in the coverages section, when they select or deselect coverage options and adjust their parameters, then the system allows configuration.
- ✓ Given coverages are configured, when the user saves, then the selected options are associated with the folio.

**US-013 - Save selected coverage options:**
- ✓ Given the user has configured the coverage options, when they click "Save", then the system persists the selected options for the folio.
- ✓ Given the options have been saved, when the user consults the folio, then the configured coverages are displayed correctly.

**US-014 - Consult coverage options for a folio:**
- ✓ Given the user has retrieved a folio, when they navigate to the coverage options section, then the system displays the previously selected coverages and their parameters.
- ✓ Given the coverages are displayed, when the user reviews them, then they can confirm the current configuration.

**US-015 - Execute net and commercial premium calculation:**
- ✓ Given the user has completed the necessary information (general data, locations, coverages), when they click "Calculate", then the backend executes the net and commercial premium calculation for each valid location.
- ✓ Given the calculation is executed, when there are incomplete locations, then the system identifies them and generates alerts without blocking the calculation of others.

**US-016 - Persist financial result with optimistic versioning:**
- ✓ Given a premium calculation has been executed, when the backend attempts to persist the financial result, then it saves it as a partial update of the folio.
- ✓ Given a concurrent edit operation is attempted, when the system detects a conflict, then it applies an optimistic versioning mechanism to resolve it.

**US-017 - Generate alerts for incomplete locations:**
- ✓ Given the premium calculation is running, when a risk location has incomplete or invalid data, then the system generates a specific alert for that location.
- ✓ Given alerts are generated, when the calculation finishes, then the alerts are included in the result to be displayed to the user.

**US-018 - Validate locations for premium calculation:**
- ✓ Given the premium calculation starts, when the system processes each location, then it validates that all mandatory fields (address, postal code) are present and valid.
- ✓ Given a location does not meet the validation criteria, when the system evaluates it, then it marks it as incomplete and does not include it in the premium calculation, but allows the calculation of others.

**US-019 - Display net and commercial premium:**
- ✓ Given the premium calculation has finished, when the user accesses the results section, then the system clearly displays the total net premium and total commercial premium.
- ✓ Given the premiums are displayed, when the user reviews them, then they can identify the final values of the quote.

**US-020 - Display premium breakdown by location:**
- ✓ Given the premium calculation has finished, when the user accesses the results section, then the system displays a breakdown of the net and commercial premium for each valid location.
- ✓ Given the breakdown is displayed, when the user reviews it, then they can see each location's contribution to the total cost.

**US-021 - Display alerts for incomplete locations:**
- ✓ Given the premium calculation has finished and there are incomplete locations, when the user accesses the results section, then the system displays clear alerts for each problematic location.
- ✓ Given the alerts are displayed, when the user reviews them, then they can identify the specific locations that require attention.

**US-022 - Visualize folio progress and status:**
- ✓ Given the user is working on a folio, when they navigate through the different sections, then the system displays a visual indicator of progress (e.g., progress bar, completed steps).
- ✓ Given the status is displayed, when the user reviews it, then they can identify if the folio is in draft, calculated, or pending information.

**US-023 - Consult subscriber, agent, and business line catalogs:**
- ✓ Given the user is capturing general or location data, when they need to select a subscriber, agent, or business line, then the system consults the reference service to obtain available options.
- ✓ Given the options are obtained, when the user selects one, then the field is populated with a valid catalog value.

**US-024 - Consult and validate postal codes:**
- ✓ Given the user enters a postal code in the locations section, when the system processes it, then it consults the reference service to validate its existence.
- ✓ Given the postal code is valid, when the system validates it, then it can suggest or autocomplete relevant geographical information.

**US-025 - Consult risk classification and guarantee catalogs:**
- ✓ Given the user is configuring a quote, when they need to select a risk classification or a guarantee, then the system consults the reference service to obtain the options.
- ✓ Given the options are obtained, when the user selects one, then the value is correctly applied to the quote or location.

**US-026 - Consult tariffs and technical factors for calculation:**
- ✓ Given the premium calculation engine is running, when it needs tariffs or technical factors (e.g., Fire, CATTEV, CATFHM), then the backend consults the plataforma-core-ohs service.
- ✓ Given the tariffs and factors are obtained, when the calculation uses them, then the values are correct and updated.

**US-027 - Generate sequential folios via reference service:**
- ✓ Given a new folio is required, when the backend makes a request to the plataforma-core-ohs service, then the service returns a unique and sequential folio identifier.
- ✓ Given the folio is received, when the backend uses it, then it is correctly associated with the new quote.

**US-028 - Implement unit tests with 80% coverage:**
- ✓ Given a functionality is developed, when unit tests are written, then code coverage for that functionality is high.
- ✓ Given the coverage analysis is executed, when the report is reviewed, then the total project coverage is equal to or greater than 80%.

**US-029 - Implement automated critical flow tests:**
- ✓ Given the 3 critical flows have been identified (creation of folio, general data capture, and complete location registration; incomplete location registration, coverage configuration, and calculation execution; results visualization and final folio status consultation), when automated tests are developed, then they cover the complete scenarios.
- ✓ Given automated tests are executed, when results are reviewed, then critical flows pass successfully.

**US-030 - Implement acceptance tests with Serenity BDD:**
- ✓ Given acceptance scenarios have been defined, when tests are implemented with Serenity BDD, then they are located in an independent repository.
- ✓ Given Serenity BDD tests are executed, when reports are reviewed, then they are clear and reflect compliance with acceptance criteria.

**US-031 - Generate complete technical and operational documentation:**
- ✓ Given the project progresses, when documentation is generated, then it includes architecture, technical decisions, installation, environment variables, API contracts (in OpenAPI/Swagger or Postman Collection format with request/response examples and HTTP status codes), data model, calculation logic, testing strategy, assumptions, and limitations.
- ✓ Given the documentation is reviewed, when its quality is evaluated, then it is complete, accurate, and of high quality.

**US-032 - Model Quote and Location data:**
- ✓ Given the business domain is defined, when the data model is created, then it includes the minimum required fields for Quote (creation date, last modification date, folio status, insured ID, insured name, driving type, risk classification) and Location.
- ✓ Given the model is implemented, when persistence operations are performed, then data integrity is maintained.

**US-033 - Ensure traceability of calculation logic:**
- ✓ Given the calculation engine is implemented, when the logic is developed, then it is consistent with the specified technical components (Fire, CATTEV, etc.) and produces a numeric result.
- ✓ Given the calculation logic is documented, when the documentation is reviewed, then it allows for complete traceability of how results are obtained, even if the logic itself is simplified/arbitrary.

**US-034 - Implement Basic User Authentication:**
- ✓ Given the system is deployed, when a user attempts to access the frontend, then they are prompted for a username and password.
- ✓ Given valid predefined credentials (e.g., from environment variables or configuration) are provided, when the user logs in, then they gain access to the application.
- ✓ Given an invalid username or password is provided, when the user attempts to log in, then access is denied, and an appropriate error message is displayed.
- ✓ Given the backend receives a request, when it requires authentication, then it validates the provided credentials against the predefined basic authentication mechanism.

##  Requerimientos Funcionales (21)

| ID Requerimiento | Descripción |
|---|---|
| **RF-001** | Crear folios con idempotencia. |
| **RF-002** | Consultar y guardar datos generales de una cotización. |
| **RF-003** | Consultar y guardar la configuración del layout de ubicaciones. |
| **RF-004** | Registrar, consultar y editar ubicaciones. |
| **RF-005** | Consultar el estado de la cotización. |
| **RF-006** | Consultar y guardar opciones de cobertura. |
| **RF-007** | Ejecutar el cálculo de prima neta y prima comercial. |
| **RF-008** | Persistir el resultado financiero sin sobrescribir otras secciones de la cotización. |
| **RF-009** | Manejar versionado optimista en operaciones de edición. |
| **RF-010** | Endpoints mínimos: POST /v1/folios, GET /v1/quotes/{folio}/general-info, PUT /v1/quotes/{folio}/general-info, GET /v1/quotes/{folio}/locations/layout, PUT /v1/quotes/{folio}/locations/layout, GET /v1/quotes/{folio}/locations, PUT /v1/quotes/{folio}/locations, PATCH /v1/quotes/{folio}/locations/{índice}, GET /v1/quotes/{folio}/locations/summary, GET /v1/quotes/{folio}/state, GET /v1/quotes/{folio}/coverage-options, PUT /v1/quotes/{folio}/coverage-options, POST /v1/quotes/{folio}/calculate. |
| **RF-011** | Crear o abrir un folio. |
| **RF-012** | Capturar datos generales. |
| **RF-013** | Consultar suscriptores, agentes, giros y códigos postales. |
| **RF-014** | Capturar una o varias ubicaciones. |
| **RF-015** | Editar una ubicación puntual. |
| **RF-016** | Visualizar el progreso y estado del folio. |
| **RF-017** | Configurar opciones de cobertura. |
| **RF-018** | Ejecutar el cálculo. |
| **RF-019** | Mostrar la prima neta, la prima comercial y el desglose por ubicación. |
| **RF-020** | Mostrar alertas de ubicaciones incompletas sin bloquear completamente el folio. |
| **RF-021** | Rutas funcionales mínimas sugeridas: /cotizador, /quotes/{folio}/general-info, /quotes/{folio}/locations, /quotes/{folio}/technical-info, /quotes/{folio}/terms-and-conditions. |

##  Requerimientos No Funcionales (10)

| ID | Categoría | Descripción |
|---|---|---|
| **RNF-001** | General | Calidad de Código: Adherencia a buenas prácticas de codificación y diseño. |
| **RNF-002** | General | Pruebas: Cobertura mínima del 80% en pruebas unitarias y al menos 3 flujos críticos automatizados. Las pruebas automatizadas de aceptación se desarrollarán con Serenity BDD en un repositorio desacoplado de la aplicación principal. |
| **RNF-003** | General | Documentación: Completa documentación técnica y operativa. |
| **RNF-004** | General | Modelado de Datos: Claridad en el modelado del dominio de cotización y ubicación. |
| **RNF-005** | General | Integración: Capacidad de integración entre servicios (backend y core de referencia). |
| **RNF-006** | Rendimiento | Rendimiento: Tiempos de respuesta adecuados para las operaciones de cotización y cálculo. |
| **RNF-007** | Usabilidad | Usabilidad: Experiencia de usuario intuitiva en el frontend. |
| **RNF-008** | General | Consistencia: Consistencia en APIs y manejo de errores. |
| **RNF-009** | Auditabilidad | Trazabilidad: La lógica de cálculo debe ser trazable y documentada. |
| **RNF-010** | Operabilidad | Operabilidad: Facilidad de ejecución local y despliegue. |

---

## Resumen de Extracción

- **Épicas:** 5
- **Features:** 8
- **Historias de Usuario:** 34
- **Requerimientos Funcionales:** 21
- **Requerimientos No Funcionales:** 10

---

**¿La información extraída del documento es correcta?**

Por favor revisa la información extraída y confírmame si puedo continuar con:

- Generación de funcionalidades.
- Generación de tareas.
- Generación de subtareas.
- Identificación de roles (puedes solicitarla en cualquier momento antes de la estimación).
- Estimación del proyecto.


---

## 2. Sección 2

###  Funcionalidades (8)

| ID | Nombre | Descripción | Complejidad | Prioridad | Justificación |
|---|---|---|---|---|---|
| **FN-001** | Gestión de Folios y Datos Generales | Permite la creación, recuperación, captura y guardado de la información general de una cotización, incluyendo datos del asegurado, tipo de conducción y clasificación de riesgo, asegurando la idempotencia en la creación de folios. | MEDIA | ALTA | Es fundamental para iniciar y gestionar cualquier cotización, estableciendo el contexto inicial y asegurando la unicidad de cada folio. Agrupa las operaciones básicas de un folio y sus datos principales. |
| **FN-002** | Administración de Ubicaciones de Riesgo | Facilita la definición del layout de ubicaciones (única o múltiple), así como el registro, consulta y edición de una o varias ubicaciones de riesgo asociadas a una cotización, con detalles como dirección y tipo constructivo. | MEDIA | ALTA | Permite al usuario especificar los puntos de riesgo a asegurar, lo cual es crítico para la evaluación y cálculo de la prima. Es un módulo central para la configuración del riesgo. |
| **FN-003** | Configuración y Gestión de Coberturas | Permite al usuario configurar y guardar las opciones de cobertura para una cotización, así como consultar las existentes, personalizando la protección ofrecida al asegurado. | MEDIA | MEDIA | Es esencial para definir el alcance de la póliza y adaptar la oferta a las necesidades del cliente. Impacta directamente en el cálculo de la prima y la propuesta de valor. |
| **FN-004** | Motor de Cálculo de Primas | Componente backend responsable de ejecutar el cálculo de primas netas y comerciales para cada ubicación válida, consolidar la prima total y persistir el resultado financiero con versionado optimista, generando alertas para ubicaciones incompletas. | ALTA | ALTA | Constituye el corazón del sistema de cotización, transformando los datos de riesgo y cobertura en valores financieros. Es crítico para la funcionalidad principal del negocio. |
| **FN-005** | Visualización de Resultados y Alertas | Proporciona una interfaz frontend para mostrar el desglose financiero (prima neta, prima comercial, primas por ubicación), el estado de la cotización y alertas relevantes, especialmente para ubicaciones con datos faltantes o inválidos. | MEDIA | ALTA | Es crucial para que el usuario final comprenda el resultado de la cotización y las posibles deficiencias en los datos ingresados, facilitando la toma de decisiones y correcciones. |
| **FN-006** | Integración con Servicios de Referencia | Gestiona la comunicación con el servicio plataforma-core-ohs (o su simulación) para obtener datos de referencia esenciales como catálogos (suscriptores, agentes, giros, códigos postales, clasificaciones de riesgo, garantías) y tarifas técnicas, así como para la generación de folios secuenciales. | MEDIA | ALTA | Asegura la consistencia y validez de los datos maestros utilizados en la cotización, evitando errores y facilitando la captura de información estandarizada. Es una dependencia clave para varias funcionalidades. |
| **FN-007** | Calidad Técnica y Trazabilidad | Abarca la implementación de pruebas unitarias y automatizadas (incluyendo Serenity BDD), la generación de documentación técnica y operativa completa, el modelado de datos claro y la trazabilidad de la lógica de cálculo, así como mecanismos básicos de seguridad. | ALTA | ALTA | Garantiza la robustez, mantenibilidad y auditabilidad del sistema, elementos críticos para la sostenibilidad a largo plazo y la confianza en los resultados del negocio. |
| **FN-008** | Gestión de proyectos | Permite la planificación, ejecución, seguimiento y control de las actividades del proyecto, asegurando la entrega de valor dentro del alcance, tiempo y presupuesto definidos. | BAJA | ALTA | Es esencial para organizar y coordinar todos los esfuerzos del equipo, optimizar recursos y garantizar que el proyecto cumpla con sus objetivos estratégicos y operativos. |
**¿Las funcionalidades identificadas son correctas?**

Por favor revisa la información y confírmame si puedo continuar con:
- Generación de Tareas
- Generación de Subtareas
- Identificación de Roles
- Estimación del proyecto.

Puedes modificar cualquier detalle o agregar más información si es necesario.

## 3. Sección 3

### Tareas (26)

| Funcionalidad ID | ID | Nombre | Complejidad | Tipo | Dependencias |
|---|---|---|---|---|---|
| FN-001 | **T-001** | Implementar API para la creación y recuperación de folios con idempotencia. | MEDIA | BACKEND | - |
| FN-001 | **T-002** | Desarrollar interfaz de usuario para la captura y visualización de datos generales del asegurado, tipo de conducción y clasificación de riesgo. | MEDIA | FRONTEND | - |
| FN-001 | **T-003** | Diseñar y crear esquema de base de datos para almacenar folios y datos generales de cotización. | MEDIA | DATABASE | - |
| FN-002 | **T-004** | Desarrollar interfaz de usuario para definir el layout de ubicaciones (única/múltiple) y gestionar el registro, consulta y edición de ubicaciones de riesgo. | MEDIA | FRONTEND | FN-001 |
| FN-002 | **T-005** | Implementar API para la gestión (CRUD) de ubicaciones de riesgo, incluyendo validación de datos como dirección y tipo constructivo. | MEDIA | BACKEND | FN-001 |
| FN-002 | **T-006** | Diseñar y crear esquema de base de datos para almacenar la información de ubicaciones de riesgo. | MEDIA | DATABASE | FN-001 |
| FN-003 | **T-007** | Desarrollar interfaz de usuario para la configuración, visualización y edición de opciones de cobertura. | MEDIA | FRONTEND | FN-001 |
| FN-003 | **T-008** | Implementar API para la gestión (CRUD) de configuraciones de cobertura asociadas a una cotización. | MEDIA | BACKEND | FN-001 |
| FN-003 | **T-009** | Diseñar y crear esquema de base de datos para almacenar las configuraciones de cobertura. | MEDIA | DATABASE | FN-001 |
| FN-004 | **T-010** | Desarrollar el motor de cálculo de primas netas y comerciales por ubicación. | ALTA | BACKEND | FN-001, FN-002, FN-003, FN-006 |
| FN-004 | **T-011** | Implementar lógica de consolidación de prima total y persistencia de resultados financieros con versionado optimista. | ALTA | BACKEND | FN-001, FN-002, FN-003, FN-006 |
| FN-004 | **T-012** | Desarrollar sistema de generación de alertas para ubicaciones con datos incompletos o inválidos. | ALTA | BACKEND | FN-001, FN-002, FN-003, FN-006 |
| FN-004 | **T-013** | Crear pruebas unitarias y de integración para el motor de cálculo de primas. | ALTA | TESTING | FN-001, FN-002, FN-003, FN-006 |
| FN-005 | **T-014** | Desarrollar interfaz de usuario para mostrar el desglose financiero (prima neta, comercial, por ubicación). | MEDIA | FRONTEND | FN-004 |
| FN-005 | **T-015** | Implementar visualización del estado general de la cotización y las alertas generadas por el motor de cálculo. | MEDIA | FRONTEND | FN-004 |
| FN-006 | **T-016** | Desarrollar conectores y adaptadores para la integración con el servicio plataforma-core-ohs (o su simulación) para catálogos y tarifas. | MEDIA | INTEGRACIÓN | - |
| FN-006 | **T-017** | Implementar lógica para la generación de folios secuenciales a través del servicio de referencia. | MEDIA | BACKEND | - |
| FN-006 | **T-018** | Crear pruebas de integración para verificar la correcta comunicación y recuperación de datos de referencia. | MEDIA | TESTING | - |
| FN-007 | **T-019** | Implementar framework de pruebas unitarias y automatizadas (Serenity BDD) para todas las funcionalidades. | ALTA | TESTING | FN-001, FN-002, FN-003, FN-004, FN-005, FN-006 |
| FN-007 | **T-020** | Configurar pipelines de CI/CD para la ejecución automática de pruebas y despliegue. | ALTA | DEVOPS | FN-001, FN-002, FN-003, FN-004, FN-005, FN-006 |
| FN-007 | **T-021** | Generar documentación técnica y operativa completa del sistema. | ALTA | GESTIÓN | FN-001, FN-002, FN-003, FN-004, FN-005, FN-006 |
| FN-007 | **T-022** | Realizar modelado de datos y asegurar la trazabilidad de la lógica de cálculo en la base de datos. | ALTA | DATABASE | FN-001, FN-002, FN-003, FN-004, FN-005, FN-006 |
| FN-007 | **T-023** | Implementar mecanismos básicos de seguridad (autenticación/autorización). | ALTA | BACKEND | FN-001, FN-002, FN-003, FN-004, FN-005, FN-006 |
| FN-008 | **T-024** | Establecer y mantener el plan de proyecto, incluyendo alcance, cronograma y presupuesto. | BAJA | GESTIÓN | - |
| FN-008 | **T-025** | Realizar seguimiento y control de las actividades del proyecto y los recursos asignados. | BAJA | GESTIÓN | - |
| FN-008 | **T-026** | Gestionar la comunicación y los stakeholders del proyecto. | BAJA | GESTIÓN | - |
**¿Las tareas identificadas son correctas?**

Por favor revisa la información y confírmame si puedo continuar con:
- Generación de Subtareas
- Identificación de Roles
- Estimación del proyecto.

Puedes modificar cualquier detalle o agregar más información si es necesario.

## 4. Sección 4

### Subtareas (104)

| Tarea ID | ID | Nombre | Dependencias |
|---|---|---|---|
| **T-001** | **ST-001** | Diseñar y definir los endpoints de la API para la creación y recuperación de folios. | - |
| **T-001** | **ST-002** | Implementar la lógica de creación de folios, asegurando la generación de identificadores únicos. | ST-001, ST-011, ST-013 |
| **T-001** | **ST-003** | Desarrollar el mecanismo de idempotencia para la creación de folios, manejando solicitudes duplicadas. | ST-002 |
| **T-001** | **ST-004** | Implementar la lógica de recuperación de folios por ID. | ST-001, ST-011, ST-013 |
| **T-001** | **ST-005** | Escribir pruebas unitarias y de integración para los endpoints de la API de folios. | ST-002, ST-003, ST-004 |
| **T-002** | **ST-006** | Diseñar la interfaz de usuario para la captura de datos generales del asegurado, tipo de conducción y clasificación de riesgo. | - |
| **T-002** | **ST-007** | Implementar los componentes de UI para la entrada de datos del asegurado (nombre, dirección, etc.). | ST-006 |
| **T-002** | **ST-008** | Desarrollar los controles de UI para seleccionar el tipo de conducción y la clasificación de riesgo. | ST-006 |
| **T-002** | **ST-009** | Implementar la lógica de visualización de los datos capturados en la interfaz de usuario. | ST-007, ST-008 |
| **T-002** | **ST-010** | Integrar la UI con la API de folios para asociar los datos al folio correspondiente. | ST-002, ST-004, ST-007, ST-008, ST-009, ST-012, ST-013 |
| **T-003** | **ST-011** | Diseñar el esquema de la tabla para almacenar folios, incluyendo campos para identificadores únicos y metadatos. | - |
| **T-003** | **ST-012** | Diseñar el esquema de la tabla para almacenar datos generales de cotización (asegurado, tipo de conducción, riesgo). | - |
| **T-003** | **ST-013** | Crear las tablas de base de datos y definir las relaciones entre folios y datos de cotización. | ST-011, ST-012 |
| **T-003** | **ST-014** | Implementar scripts de migración para la creación inicial del esquema de base de datos. | ST-013 |
| **T-004** | **ST-015** | Diseñar la interfaz de usuario para seleccionar el layout de ubicaciones (única o múltiple). | - |
| **T-004** | **ST-016** | Implementar los componentes de UI para el registro de nuevas ubicaciones de riesgo. | ST-015 |
| **T-004** | **ST-017** | Desarrollar la funcionalidad de consulta y visualización de ubicaciones de riesgo existentes. | ST-015 |
| **T-004** | **ST-018** | Implementar la funcionalidad de edición de los datos de ubicaciones de riesgo. | ST-015, ST-016, ST-017 |
| **T-004** | **ST-019** | Integrar la UI con la API de gestión de ubicaciones de riesgo. | ST-016, ST-017, ST-018, ST-021, ST-022, ST-023, ST-024 |
| **T-005** | **ST-020** | Diseñar y definir los endpoints de la API para las operaciones CRUD de ubicaciones de riesgo. | - |
| **T-005** | **ST-021** | Implementar la lógica de creación y validación de ubicaciones de riesgo (dirección, tipo constructivo). | ST-020, ST-025, ST-026 |
| **T-005** | **ST-022** | Desarrollar la lógica de recuperación de ubicaciones de riesgo por ID o criterios de búsqueda. | ST-020, ST-025, ST-026 |
| **T-005** | **ST-023** | Implementar la lógica de actualización de ubicaciones de riesgo, incluyendo validaciones. | ST-020, ST-025, ST-026 |
| **T-005** | **ST-024** | Desarrollar la lógica de eliminación de ubicaciones de riesgo. | ST-020, ST-025, ST-026 |
| **T-006** | **ST-025** | Diseñar el esquema de la tabla para almacenar los datos de ubicaciones de riesgo (dirección, tipo constructivo, etc.). | - |
| **T-006** | **ST-026** | Crear la tabla de base de datos para ubicaciones de riesgo y definir sus índices. | ST-025 |
| **T-006** | **ST-027** | Implementar scripts de migración para la creación inicial del esquema de base de datos de ubicaciones. | ST-026 |
| **T-007** | **ST-028** | Diseñar la interfaz de usuario para la configuración de opciones de cobertura. | - |
| **T-007** | **ST-029** | Implementar los componentes de UI para seleccionar y ajustar las opciones de cobertura. | ST-028 |
| **T-007** | **ST-030** | Desarrollar la funcionalidad de visualización de las coberturas configuradas. | ST-028 |
| **T-007** | **ST-031** | Implementar la funcionalidad de edición de las opciones de cobertura existentes. | ST-028, ST-029, ST-030 |
| **T-007** | **ST-032** | Integrar la UI con la API de gestión de configuraciones de cobertura. | ST-029, ST-030, ST-031, ST-034, ST-035, ST-036, ST-037 |
| **T-008** | **ST-033** | Diseñar y definir los endpoints de la API para las operaciones CRUD de configuraciones de cobertura. | - |
| **T-008** | **ST-034** | Implementar la lógica de creación de configuraciones de cobertura, asociándolas a una cotización. | ST-033, ST-038, ST-039 |
| **T-008** | **ST-035** | Desarrollar la lógica de recuperación de configuraciones de cobertura por ID de cotización. | ST-033, ST-038, ST-039 |
| **T-008** | **ST-036** | Implementar la lógica de actualización de configuraciones de cobertura. | ST-033, ST-038, ST-039 |
| **T-008** | **ST-037** | Desarrollar la lógica de eliminación de configuraciones de cobertura. | ST-033, ST-038, ST-039 |
| **T-009** | **ST-038** | Diseñar el esquema de la tabla para almacenar las configuraciones de cobertura, incluyendo su asociación a una cotización. | - |
| **T-009** | **ST-039** | Crear la tabla de base de datos para configuraciones de cobertura y definir sus relaciones. | ST-038 |
| **T-009** | **ST-040** | Implementar scripts de migración para la creación inicial del esquema de base de datos de coberturas. | ST-039 |
| **T-010** | **ST-041** | Diseñar la arquitectura del motor de cálculo de primas, identificando los componentes clave. | - |
| **T-010** | **ST-042** | Implementar la lógica para el cálculo de primas netas, considerando factores de riesgo y coberturas. | ST-010, ST-012, ST-013, ST-019, ST-021, ST-022, ST-023, ST-024, ST-025, ST-026, ST-032, ST-034, ST-035, ST-036, ST-037, ST-038, ST-039, ST-041 |
| **T-010** | **ST-043** | Desarrollar la lógica para el cálculo de primas comerciales, aplicando recargos y descuentos. | ST-041, ST-042 |
| **T-010** | **ST-044** | Implementar la funcionalidad para calcular primas por ubicación, integrando datos de ubicaciones y coberturas. | ST-019, ST-021, ST-022, ST-023, ST-024, ST-025, ST-026, ST-032, ST-034, ST-035, ST-036, ST-037, ST-038, ST-039, ST-041, ST-042, ST-043 |
| **T-010** | **ST-045** | Escribir pruebas unitarias y de integración exhaustivas para el motor de cálculo de primas. | ST-042, ST-043, ST-044 |
| **T-011** | **ST-046** | Diseñar e implementar el modelo de datos para resultados financieros con soporte para versionado optimista. | - |
| **T-011** | **ST-047** | Desarrollar la lógica de negocio para la consolidación de la prima total, considerando diferentes componentes. | ST-042, ST-043, ST-044 |
| **T-011** | **ST-048** | Implementar la capa de persistencia para guardar los resultados financieros consolidados, aplicando el control de concurrencia optimista. | ST-046, ST-047 |
| **T-011** | **ST-049** | Crear pruebas unitarias para la lógica de consolidación y persistencia de resultados financieros. | ST-047, ST-048 |
| **T-012** | **ST-050** | Definir los criterios y reglas para identificar datos incompletos o inválidos en las ubicaciones. | - |
| **T-012** | **ST-051** | Implementar el servicio o componente encargado de evaluar las ubicaciones y detectar anomalías. | ST-019, ST-021, ST-022, ST-023, ST-024, ST-025, ST-026, ST-050 |
| **T-012** | **ST-052** | Desarrollar el mecanismo de generación y almacenamiento de alertas para las ubicaciones identificadas. | ST-051 |
| **T-012** | **ST-053** | Crear pruebas unitarias y de integración para el sistema de generación de alertas. | ST-052 |
| **T-013** | **ST-054** | Identificar los escenarios clave y casos de borde para las pruebas unitarias del motor de cálculo de primas. | - |
| **T-013** | **ST-055** | Implementar pruebas unitarias exhaustivas para cada componente o función del motor de cálculo. | ST-042, ST-043, ST-044, ST-054 |
| **T-013** | **ST-056** | Diseñar y desarrollar pruebas de integración para verificar el flujo completo del cálculo de primas. | ST-042, ST-043, ST-044, ST-054 |
| **T-013** | **ST-057** | Configurar la ejecución automática de las pruebas unitarias y de integración en el entorno de desarrollo. | ST-055, ST-056 |
| **T-014** | **ST-058** | Diseñar la maqueta y el flujo de la interfaz de usuario para el desglose financiero. | - |
| **T-014** | **ST-059** | Implementar los componentes de UI para mostrar la prima neta, comercial y por ubicación. | ST-058 |
| **T-014** | **ST-060** | Integrar la interfaz de usuario con los servicios backend para obtener los datos del desglose financiero. | ST-047, ST-048, ST-059 |
| **T-015** | **ST-061** | Diseñar los elementos visuales para representar el estado general de la cotización (ej. semáforos, indicadores). | - |
| **T-015** | **ST-062** | Desarrollar los componentes de UI para mostrar las alertas generadas por el motor de cálculo. | ST-052, ST-061 |
| **T-015** | **ST-063** | Conectar la interfaz de usuario con los servicios backend para obtener el estado de la cotización y las alertas. | ST-052, ST-062 |
| **T-016** | **ST-064** | Analizar la especificación del servicio plataforma-core-ohs para catálogos y tarifas. | - |
| **T-016** | **ST-065** | Implementar los conectores y adaptadores para consumir los servicios de catálogos y tarifas. | ST-064 |
| **T-016** | **ST-066** | Desarrollar una simulación del servicio plataforma-core-ohs para facilitar el desarrollo y las pruebas. | ST-064 |
| **T-017** | **ST-067** | Diseñar la integración con el servicio de referencia para la generación de folios secuenciales. | - |
| **T-017** | **ST-068** | Implementar la lógica de negocio para solicitar y gestionar la asignación de folios. | ST-067 |
| **T-017** | **ST-069** | Crear pruebas unitarias para la lógica de generación de folios secuenciales. | ST-068 |
| **T-018** | **ST-070** | Identificar los escenarios de prueba para la integración con los servicios de datos de referencia. | - |
| **T-018** | **ST-071** | Desarrollar pruebas de integración que validen la comunicación y el formato de los datos recuperados. | ST-065, ST-068, ST-070 |
| **T-018** | **ST-072** | Configurar un entorno de prueba para ejecutar las pruebas de integración de datos de referencia. | ST-071 |
| **T-019** | **ST-073** | Investigar y seleccionar las herramientas y dependencias necesarias para Serenity BDD. | - |
| **T-019** | **ST-074** | Configurar el proyecto para integrar el framework Serenity BDD para pruebas unitarias y de aceptación. | ST-073 |
| **T-019** | **ST-075** | Desarrollar un ejemplo de prueba automatizada utilizando Serenity BDD para una funcionalidad existente. | ST-074 |
| **T-019** | **ST-076** | Documentar el uso y las mejores prácticas del framework Serenity BDD para el equipo. | ST-074, ST-075 |
| **T-020** | **ST-077** | Seleccionar la herramienta de CI/CD (GitHub Actions) y configurar el repositorio. | - |
| **T-020** | **ST-078** | Diseñar el pipeline de CI para la compilación, ejecución de pruebas unitarias y de integración. | ST-005, ST-045, ST-049, ST-053, ST-055, ST-056, ST-069, ST-071, ST-077 |
| **T-020** | **ST-079** | Implementar el pipeline de CD para el despliegue automático en entornos de desarrollo y staging. | ST-078 |
| **T-020** | **ST-080** | Configurar notificaciones y reportes de estado para los pipelines de CI/CD. | ST-078, ST-079 |
| **T-021** | **ST-081** | Definir la estructura y el contenido de la documentación técnica y operativa del sistema. | - |
| **T-021** | **ST-082** | Redactar la documentación técnica de arquitectura, diseño e implementación del software. | ST-001, ST-002, ST-003, ST-004, ST-011, ST-012, ST-020, ST-021, ST-022, ST-023, ST-024, ST-025, ST-033, ST-034, ST-035, ST-036, ST-037, ST-038, ST-041, ST-042, ST-043, ST-044, ST-046, ST-047, ST-048, ST-050, ST-051, ST-052, ST-064, ST-065, ST-067, ST-068, ST-081, ST-086, ST-087, ST-088, ST-089, ST-091, ST-092, ST-093, ST-094 |
| **T-021** | **ST-083** | Elaborar la documentación operativa para la instalación, configuración y mantenimiento del sistema. | ST-013, ST-014, ST-026, ST-027, ST-039, ST-040, ST-079, ST-081, ST-087, ST-088, ST-089, ST-094 |
| **T-021** | **ST-084** | Crear manuales de usuario y guías de solución de problemas para los usuarios finales. | ST-007, ST-008, ST-009, ST-010, ST-016, ST-017, ST-018, ST-019, ST-029, ST-030, ST-031, ST-032, ST-059, ST-060, ST-062, ST-063, ST-081 |
| **T-021** | **ST-085** | Revisar y validar la documentación generada con los equipos técnicos y operativos. | ST-082, ST-083, ST-084 |
| **T-022** | **ST-086** | Diseñar el modelo entidad-relación (ERD) de la base de datos, incluyendo tablas, relaciones y tipos de datos. | ST-011, ST-012, ST-025, ST-038, ST-046 |
| **T-022** | **ST-087** | Implementar el esquema de la base de datos en el motor de base de datos seleccionado. | ST-086 |
| **T-022** | **ST-088** | Desarrollar scripts o procedimientos almacenados para la lógica de cálculo crítica. | ST-042, ST-043, ST-044, ST-047, ST-087 |
| **T-022** | **ST-089** | Establecer mecanismos de auditoría y logging para la trazabilidad de los cálculos en la base de datos. | ST-042, ST-043, ST-044, ST-047, ST-087 |
| **T-022** | **ST-090** | Realizar pruebas unitarias y de integración para validar el modelo de datos y la lógica de cálculo. | ST-042, ST-043, ST-044, ST-047, ST-087, ST-088 |
| **T-023** | **ST-091** | Seleccionar e integrar una librería o framework de autenticación (ej. JWT, OAuth2). | - |
| **T-023** | **ST-092** | Implementar el flujo de registro y login de usuarios con almacenamiento seguro de credenciales. | ST-091 |
| **T-023** | **ST-093** | Desarrollar un sistema de roles y permisos para la autorización de acceso a recursos. | ST-091, ST-092 |
| **T-023** | **ST-094** | Integrar los mecanismos de seguridad en las APIs y la interfaz de usuario. | ST-002, ST-004, ST-010, ST-019, ST-021, ST-022, ST-023, ST-024, ST-032, ST-034, ST-035, ST-036, ST-037, ST-060, ST-063, ST-092, ST-093 |
| **T-023** | **ST-095** | Realizar pruebas de seguridad para verificar la robustez de la autenticación y autorización. | ST-094 |
| **T-024** | **ST-096** | Definir el alcance detallado del proyecto y sus entregables clave. | - |



## Resumen de la Estimación

- **Días hábiles estimados:** 82.2 días

- **Funcionalidades:** 8
- **Tareas:** 26
- **Subtareas:** 104