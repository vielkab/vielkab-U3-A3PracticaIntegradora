# Auditoría de dependencias y decisiones de mitigación

Fecha de auditoría final: 19 de noviembre de 2026
Alcance: dependencias declaradas en `requirements.txt` y paquetes instalados en el entorno virtual de la API.

## Resultado inicial

La auditoría de la versión heredada encontró 54 registros de vulnerabilidad en cinco paquetes y 28 CVE únicos. La salida de pip-audit no incluye severidad CVSS; por ello no se asignan categorías de severidad que la herramienta no reportó. Los cambios se priorizaron para eliminar los hallazgos conocidos.

## Tabla de decisiones

| Paquete y versión vulnerable | CVE identificados inicialmente | Decisión de mitigación | Resultado final |
|---|---|---|---|
| Flask 2.2.2 | CVE-2023-30861, CVE-2026-27205 | Actualizar a Flask 3.1.3. | Corregido; sin vulnerabilidades conocidas en pip-audit. |
| Werkzeug 2.2.2 | CVE-2023-23934, CVE-2023-25577, CVE-2023-46136, CVE-2024-34069, CVE-2024-49766, CVE-2024-49767, CVE-2025-66221, CVE-2026-102598, CVE-2026-21860, CVE-2026-27199 | Actualizar a Werkzeug 3.1.9. | Corregido; sin vulnerabilidades conocidas en pip-audit. |
| requests 2.20.0 | CVE-2023-32681, CVE-2024-35195, CVE-2024-47081, CVE-2026-25645 | Actualizar a requests 2.34.2. | Corregido; sin vulnerabilidades conocidas en pip-audit. |
| idna 2.7 (transitiva) | CVE-2024-3651, CVE-2026-45409 | Fijar idna 3.20 para impedir que una instalación heredada conserve una versión transitiva vulnerable. | Corregido; sin vulnerabilidades conocidas en pip-audit. |
| urllib3 1.24.3 (transitiva) | CVE-2020-26137, CVE-2023-43804, CVE-2023-45803, CVE-2024-37891, CVE-2025-50181, CVE-2025-66418, CVE-2025-66471, CVE-2026-21441, CVE-2026-44431, CVE-2026-97689 | Se resuelve a urllib3 2.8.0 mediante requests actualizado. | Corregido; sin vulnerabilidades conocidas en pip-audit. |
| python-dotenv | No estaba presente en el inventario inicial. | Añadir python-dotenv 1.2.4 para cargar `API_KEY` desde `.env`, excluido de Git. | Instalado; sin vulnerabilidades conocidas en pip-audit. |

## Evidencia final

- `pip-audit-before.json`: resultado de la auditoría de dependencias vulnerables original.
- `pip-audit-after.json`: auditoría final de `requirements.txt`; no encontró vulnerabilidades conocidas.
- `pip-audit-after-local.json`: auditoría final del entorno virtual de la API; no encontró vulnerabilidades conocidas.
- `sbom.cdx.json`: inventario CycloneDX 1.6 del estado vulnerable original.
- `sbom-after.cdx.json`: inventario CycloneDX 1.6 del entorno actualizado.
- El SBOM posterior incluye un hash SHA-256 por componente, tomado del artefacto de distribución publicado para esa versión en PyPI.

La auditoría de salida no reporta un nivel CVSS. El resultado “sin vulnerabilidades conocidas” describe los datos disponibles al ejecutar pip-audit y no garantiza la ausencia de vulnerabilidades futuras.
