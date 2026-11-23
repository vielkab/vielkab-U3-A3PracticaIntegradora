# API REST de práctica: controles DevSecOps

API Flask local para la práctica integradora de DevSecOps. El historial y los reportes `*-before` conservan la evidencia del escenario vulnerable; el estado actual externaliza secretos, parametriza la consulta SQL y fija dependencias auditadas. Usa únicamente los datos ficticios incluidos y no expongas la API a una red pública.

## Requisitos y ejecución

1. Abre PowerShell en esta carpeta.
2. Crea y activa un entorno virtual:

   ```powershell
   py -m venv .venv
   .\.venv\Scripts\Activate.ps1
   ```

3. Instala las dependencias fijadas:

   ```powershell
   python -m pip install -r requirements.txt
   ```

4. Crea tu archivo local de configuración y establece una clave de demostración:

   ```powershell
   Copy-Item .env.example .env
   notepad .env
   ```

   Cambia el valor de `API_KEY` por una clave local aleatoria. El archivo `.env` está excluido de Git.

5. Inicia la API:

   ```powershell
   python app.py
   ```

La API escucha solo en `http://127.0.0.1:5000`. Al iniciar crea `practica.db` con usuarios ficticios.

## Endpoints

- `GET /api/health` — comprueba que la API está activa.
- `GET /api/profile` — requiere el encabezado `X-API-Key` con el valor configurado localmente en `.env` como `API_KEY`.
- `POST /api/login` — recibe JSON con `username` y `password`; usa `admin` / `AdminDemo123!` o `estudiante` / `Practica123!` para el caso válido.

Ejemplo de cuerpo para `POST /api/login`:

```json
{"username":"admin","password":"AdminDemo123!"}
```

## Análisis de seguridad

Desde esta carpeta puedes repetir los análisis del pipeline:

```powershell
python -m pip install bandit pip-audit
bandit -r app.py -ll
pip-audit -r requirements.txt
```

GitHub Actions ejecuta Bandit (SAST) y pip-audit (SCA) en cada Pull Request dirigido a `main` y en cada `push` a `main`. La carpeta `security/` conserva los resultados antes y después de la remediación, la tabla de decisiones y los SBOM CycloneDX. No uses los ejemplos de inyección contra servicios externos.
