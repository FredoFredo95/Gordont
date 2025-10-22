# Toma de Requerimientos — Sistema de Registro Nutricional para Gimnasio

## 1. Problema del negocio

El gimnasio enfrenta dificultades para llevar un control organizado de los planes de alimentación y entrenamiento de sus clientes.
Actualmente, los entrenadores registran la información manualmente o en aplicaciones separadas, lo que provoca pérdida de datos, falta de seguimiento y poca personalización en las rutinas.
El administrador necesita una herramienta centralizada que permita a los clientes registrar sus comidas y entrenamientos diarios, y a los entrenadores acceder a esta información para orientar sus planes de manera personalizada.

---
## 2. Usuarios
- **Administrador:** Supervisa el sistema, gestiona usuarios (clientes y entrenadores) y controla la base de datos.  
- **Entrenador:** Gestiona y revisa los registros de sus clientes, analiza su progreso y entrega recomendaciones.  
- **Cliente:** Registra su alimentación y entrenamientos, visualiza su progreso y accede a reportes personales.

---
## 3. Funciones necesarias (MVP)

- Registro e inicio de sesión de usuarios.  
- Registro diario de comidas con detalle de calorías, proteínas, grasas y carbohidratos.  
- Consulta a la base de datos de alimentos.  
- Registro de entrenamientos personalizados (ejercicios, sets, peso, repeticiones).  
- Cálculo automático del volumen total de entrenamiento (peso × repeticiones).  
- Historial de progreso con datos nutricionales y de entrenamiento.  
- Acceso del entrenador a la información de sus clientes.  
- Panel de administración para crear, editar o eliminar usuarios.  
- Búsqueda de alimentos y ejercicios por nombre o categoría.  
- Persistencia de datos en una base de datos NoSQL (MongoDB).

## 4. Datos a guardar

### Usuarios
- id_usuario (integer)  
- nombre (string)  
- email (string)  
- password (string cifrada)  
- tipo_usuario (string: cliente / entrenador / administrador)  
- edad (integer)  
- altura_cm (integer)  
- peso_kg (float)  
- genero (string)

### Alimentos
- id_producto (integer)  
- name (string)  
- brand (string o null)  
- calories (float)  
- protein (float)  
- fat (float)  
- carbs (float)  
- servingGrams (float)  
- servingUnit (string)  
- verified (boolean)

### Registro Diario de Comidas
- id_registro (integer)  
- id_usuario (integer)  
- fecha (date)  
- alimentos (array de alimentos consumidos)  
- total_calorias (float)  
- total_proteinas (float)  
- total_grasas (float)  
- total_carbohidratos (float)

### Entrenamientos
- id_entrenamiento (integer)  
- id_usuario (integer)  
- fecha (date)  
- duracion_min (integer)  
- cantidad_sets (integer)  
- volumen_total_kg (float)  
- ejercicios (array con detalle de ejercicios, sets, peso y repeticiones)

### Ejercicios
- id_ejercicio (integer)  
- nombre (string)  
- grupo_muscular (string)  
- equipamiento (string)  
- nivel (string)

---
## 5. Reglas de negocio


- Los entrenadores solo pueden acceder a los datos de los clientes que tengan asignados.  
- Los entrenadores deben ser creados y administrados por el administrador.  
- El cliente puede registrar múltiples comidas o entrenamientos por día, pero solo uno será el principal diario.  
- Los datos nutricionales se calculan automáticamente al registrar alimentos.  
- El volumen de entrenamiento se calcula automáticamente sumando (peso × repeticiones) de cada set.  
- Solo administradores pueden eliminar usuarios o datos históricos.  
- Las contraseñas deben almacenarse cifradas (hash seguro).  
- Cada registro debe estar asociado al usuario que lo generó.

---

## 6. Prioridades

*Alta:*
- Registro de usuarios y autenticación.  
- Registro de comidas y entrenamientos.  
- Cálculo automático de calorías y volumen de entrenamiento.  
- Historial y visualización de progreso.  
- Acceso del entrenador a los datos del cliente.

*Media:*
- Estadísticas visuales por semana o mes.  
- Edición de registros pasados.

*Baja:*
- Integración con dispositivos externos (smartwatch, básculas, etc.).  
- Modo offline o sincronización diferida.

---
## 7. Flujos Principales

### Cliente
1. Registro de usuario → inicio de sesión.  
2. Registrar comida → calcular totales → guardar en base de datos → visualizar historial.  
3. Registrar entrenamiento → añadir ejercicios → registrar sets (peso y repeticiones) → calcular volumen total.

### Entrenador
- Consultar clientes asignados → visualizar sus registros nutricionales y de entrenamiento → agregar observaciones.

### Administrador
- Crear, editar o eliminar usuarios → gestionar base de datos de alimentos y ejercicios.

---

## 8. Requisitos no funcionales

### Requisitos Funcionales
- Registrar usuarios y sus metas nutricionales.  
- El cliente puede registrar alimentos consumidos y cantidad.  
- Cálculo automático de macronutrientes.  
- Mostrar reportes diarios y mensuales.  
- Entrenadores pueden consultar datos de clientes.  
- Administrador puede gestionar alimentos y usuarios.

### Requisitos No Funcionales
- Acceso multiusuario con autenticación segura (JWT u otro mecanismo).  
- Respaldo diario en servidor o nube.  
- Interfaz usable y adaptada a móviles.  
- Seguridad: HTTPS, control de acceso por roles, contraseñas cifradas.  
- Rendimiento: tiempo de respuesta < 2s en consultas básicas.  
- Escalabilidad: soportar crecimiento de usuarios.  
- Disponibilidad: uptime ≥ 99%.  
- Persistencia: almacenamiento en MongoDB.

---
## 9. Plazo Deseado
Primera versión funcional del **MVP** (API + base de datos MongoDB) en un plazo de **4 semanas** desde la aprobación del alcance.

---

## 10. Definición de Alcance y Presupuesto

**Alcance acordado:**  
Desarrollo de una API con **MongoDB y Express** para gestionar usuarios, alimentos, comidas y entrenamientos.

**Incluye:**
- App móvil con roles (cliente, entrenador, admin).  
- Registro de usuarios, alimentos, entrenamientos, comidas y peso.  
- Reportes diarios y mensuales.  
- Sincronización y almacenamiento local.  
- Panel de administración interno.

**No incluye:**
- Recomendaciones automáticas de dieta.  
- Integraciones con dispositivos externos.  
- Exportación de datos (PDF, Excel).  
- Pasarelas de pago o funciones premium.

### Presupuesto Estimado

| Rol                      | Horas Estimadas | Tarifa (USD/h) | Subtotal (USD) |
|--------------------------|-----------------|----------------|----------------|
| Desarrollador Full-Stack | 300             | 5              | 1,500          |
| Diseñador UX/UI          | 40              | 5              | 200            |
| QA Tester                | 30              | 5              | 150            |
| Project Manager          | 30              | 5              | 150            |
| **Subtotal**             | —               | —              | **2,000**      |
| **Contingencia (10%)**   | —               | —              | **200**        |
| **Total Estimado**       | —               | —              | **≈ 2,200 USD** |

---

## 11. Propuesta formal y cronograma de trabajo
*Para el gimnasio:*
- Mayor fidelización de clientes.  
- Apoyo profesional para los entrenadores.  
- Valor agregado al servicio del gimnasio.  
- Potencial de monetización futura (versión premium, reportes exportables, coaching nutricional digital).

---

## 12. Criterios de aceptación
| Semana | Actividades Principales |
|--------|--------------------------|
| *1* | Definición de requerimientos, diseño del modelo de datos y configuración del repositorio (Gitflow). |
| *2* | Desarrollo de endpoints básicos de usuario y autenticación. |
| *3* | Implementación de módulos de comidas y entrenamientos. |
| *4* | Pruebas funcionales, documentación y preparación del MVP. |

---

## 13. Soporte y mantenimiento
- El sistema permite crear usuarios y diferenciarlos por rol.  
- Los clientes pueden registrar comidas y entrenamientos.  
- Los entrenadores visualizan los datos de sus clientes.  
- Los cálculos de calorías y volumen son correctos.  
- Los datos se almacenan correctamente en MongoDB.

---

## 14. Soporte y Mantenimiento

*Soporte Correctivo:*  
Resolución de bugs, errores o incidentes de uso.  

*Soporte Evolutivo:*  
Pequeñas mejoras en la funcionalidad o interfaz sin modificar la estructura base.  

*Soporte Preventivo:*  
Mantenimiento técnico periódico: optimización de la base de datos, limpieza de datos obsoletos, respaldo (backup) y monitoreo.

---

## 15. Próximos Pasos

1. *Validación del Documento:* Revisión y aprobación de requerimientos, plazos y presupuesto.  
2. *Diseño UX/UI:* Creación de prototipos visuales y navegación principal.  
3. *Desarrollo del MVP:* Implementación iterativa con revisiones semanales.  
4. *Pruebas y Validación Interna:* Testeo funcional y con usuarios reales.  
5. *Lanzamiento del MVP (Beta):* Uso real dentro del gimnasio y monitoreo.  
6. *Evaluación y Mejoras Posteriores:* Revisión de feedback y plan de ampliaciones futuras.

---

