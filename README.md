# Ecosistema Autónomo de Calificación de Leads y Generación de Propuestas B2B

Pipeline de automatización empresarial desarrollado sobre **Make**, **Airtable** y **Google Gemini**, diseñado para captar prospectos, inyectar directrices comerciales mediante RAG, generar propuestas cotizadas y asegurar la supervisión mediante una malla de control humano (Human-in-the-Loop - HITL) con tolerancia a fallos.

---

## 🛠️ Arquitectura del Sistema

El flujo opera de manera desacoplada en dos fases independientes para garantizar control de calidad y trazabilidad:

1. **Escenario A (Ingesta, RAG y Pausa HITL):**
   * Monitorea nuevos prospectos en Airtable (`Leads`).
   * Recupera directrices, alcances y rangos de inversión desde la base de conocimiento (`Servicios`).
   * Genera el borrador comercial con **Google Gemini 2.5 Flash**.
   * Dispone de un manejador de errores con directiva **`Resume`** ante interrupciones de API.
   * Actualiza el lead al estado `En Revisión HITL` y dispara una notificación interna por correo al supervisor.

2. **Escenario B (Distribución Condicional y Cierre):**
   * Evalúa periódicamente registros aprobados mediante la condición `{Estatus} = 'En Revisión HITL'` y `{Aprobado} = 1`.
   * Pasa por un filtro booleano estricto que previene despachos no autorizados ("efecto metralleta").
   * Despacha la propuesta final personalizada vía **Gmail API**.
   * Actualiza el estatus a `Enviado` y estampa la marca temporal de cierre (`Fecha de Envio`).

---

## 📁 Archivos y Evidencias en este Repositorio

* `Ecosistema Autónomo de Calificación de Leads...pdf`: Documento técnico integral que detalla el mapa de arquitectura, diccionario de datos relacionales, análisis de optimización de costos de modelos LLM y políticas de resiliencia y privacidad.
* `Captura escenario A.jpg`: Ejecución completa del Escenario A en Make, mostrando la inyección contextual y la ruta de error handler (`Resume`).
* `Captura Escenario B.jpg`: Ejecución completa del Escenario B en Make, validando el paso por el filtro de seguridad HITL (`✔ 1`) y el despacho multicanal.
* `Captura Airtable.jpg`: Estado de la base de datos relacional mostrando los campos de trazabilidad (`Propuesta Generada`, `Estatus: Enviado`, `Aprobado` y marca temporal).
* `Captura correo recibido.jpg`: Evidencia del correo final recibido por el cliente con la propuesta formateada y personalizada.

---

## ⚙️ Stack Tecnológico

* **Orquestador Central:** Make (Integromat)
* **Base de Datos Relacional:** Airtable (`Leads`, `Servicios`, `System_Logs`)
* **Modelo Cognitivo (LLM):** Google Gemini 2.5 Flash
* **Mensajería y Notificaciones:** Gmail API
