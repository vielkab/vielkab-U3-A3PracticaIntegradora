# Auditoría inicial de dependencias y decisiones de mitigación

Fecha de auditoría: 18 de noviembre de 2026
Alcance: dependencias declaradas en `requirements.txt` antes de remediar.

## Resultado inicial

`pip-audit -r requirements.txt` encontró 54 registros de vulnerabilidad en cinco paquetes resueltos. Al deduplicar por identificador CVE, el inventario contiene 28 CVE únicos. El JSON de `pip-audit` no informa una severidad CVSS; por eso esta tabla no asigna severidades que el escáner no entregó. La decisión técnica fue actualizar las dependencias afectadas después de verificar el Build Breaker con el código original.

## Tabla de decisiones

| Paquete y versión detectada | CVE identificados | Decisión de mitigación | Estado |
|---|---|---|---|
| Flask 2.2.2 | CVE-2023-30861, CVE-2026-27205 | Actualizar a Flask 3.1.3, que cubre los hallazgos del paquete. | Corregido; 0 vulnerabilidades en auditoría final |
| Werkzeug 2.2.2 | CVE-2023-23934, CVE-2023-25577, CVE-2023-46136, CVE-2024-34069, CVE-2024-49766, CVE-2024-49767, CVE-2025-66221, CVE-2026-102598, CVE-2026-21860, CVE-2026-27199 | Actualizar a Werkzeug 3.1.9, que incluye las correcciones publicadas para los hallazgos. | Corregido; 0 vulnerabilidades en auditoría final |
| requests 2.20.0 | CVE-2023-32681, CVE-2024-35195, CVE-2024-47081, CVE-2026-25645 | Actualizar a requests 2.34.2. | Corregido; 0 vulnerabilidades en auditoría final |
| idna 2.7 (transitiva) | CVE-2024-3651, CVE-2026-45409 | Resolver mediante la actualización de requests; validar versión transitiva con auditoría final. | Corregido: idna 3.20; 0 vulnerabilidades |
| urllib3 1.24.3 (transitiva) | CVE-2020-26137, CVE-2023-43804, CVE-2023-45803, CVE-2024-37891, CVE-2025-50181, CVE-2025-66418, CVE-2025-66471, CVE-2026-21441, CVE-2026-44431, CVE-2026-97689 | Resolver mediante la actualización de requests; validar versión transitiva con auditoría final. | Corregido: urllib3 2.8.0; 0 vulnerabilidades |
| python-dotenv 1.2.4 | Sin hallazgos en la auditoría actual | Se incorpora para cargar la variable `API_KEY` desde el archivo `.env` local. | Sin vulnerabilidades conocidas en `pip-audit` |

## Evidencia

- `pip-audit-before.json`: salida JSON inicial de `pip-audit`.
- `sbom.cdx.json`: SBOM CycloneDX 1.6 del entorno de dependencias vulnerable, con 13 componentes.
- `pip-audit-after.json`: auditoría posterior a la actualización; 13 paquetes directos/transitivos, todos con `vulns: []`.
- `sbom-after.cdx.json`: SBOM CycloneDX con el inventario actualizado y hashes SHA-256 de los 13 componentes.

## Integridad del SBOM

Se incorporó un hash SHA-256 por componente, correspondiente a un artefacto de distribución oficial de la versión publicada en PyPI. El hash identifica ese artefacto; puede variar entre wheel y distribución fuente, y no implica que se haya actualizado o remediado el paquete. Los componentes y versiones del inventario permanecen sin cambios.
