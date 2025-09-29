|  |  |  |  |
|---|---|---|---|
| Nombre de la Clase: | ADJUNTO | | |
| Superclase: | — | | |
| Subclases: | — | | |
| **Responsabilidades** | **Colaboradores** | **Pensamiento del objeto** | **Propiedad** |
|---|---|---|---|
| Registrar mis metadatos de archivo | — | Necesito identificarme y describirme para ser útil en la etapa. | titulo, url, tipo, fechaAdjunto |
| Validar mi tipo de archivo permitido | — | Verifico que mi tipo sea aceptado antes de usarse. | tipo |
| Generar una vista previa cuando sea posible | — | Facilito inspeccionar mi contenido sin descargarme. | url, tipo |
| Permitir mi descarga | — | Habilito que me obtengan desde mi ubicación. | url |
| Permitir mi eliminación | — | Puedo ser removido cuando dejo de ser necesario. | — |
| Vincularme a una etapa del proyecto | Etapa | Me asocio a la etapa donde aporto contexto o material de trabajo. | — (relación con Etapa) |