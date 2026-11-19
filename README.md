# API REST de práctica: SAST y SCA

API Flask local para practicar controles de seguridad. La clave API se carga desde `.env` y el inicio de sesión usa consultas SQL parametrizadas. Usa únicamente los datos ficticios incluidos; no agregues credenciales reales ni la expongas a una red pública.

## Requisitos y ejecución

1. Abre PowerShell en esta carpeta.
2. Crea y activa un entorno virtual:

   ```powershell
   py -m venv .venv
   .\.venv\Scripts\Activate.ps1
   ```

3. Instala las dependencias actualizadas:

   ```powershell
   python -m pip install -r requirements.txt
   ```

4. Copia `.env.example` como `.env` y define una clave local de demostración:

   ```powershell
   Copy-Item .env.example .env
   notepad .env
   ```

   Asigna un valor ficticio a `API_KEY`. El archivo `.env` está excluido de Git.

5. Inicia la API:

   ```powershell
   python app.py
   ```

La API escucha solo en `http://127.0.0.1:5000`. Al iniciar crea `practica.db` con usuarios ficticios.

## Endpoints

- `GET /api/health` — comprueba que la API está activa.
- `GET /api/profile` — requiere el encabezado `X-API-Key` con el valor configurado en `.env`.
- `POST /api/login` — recibe JSON con `username` y `password`; usa `admin` / `AdminDemo123!` o `estudiante` / `Practica123!` para el caso válido.

Ejemplo de cuerpo para `POST /api/login`:

```json
{"username":"admin","password":"<contraseña-de-demostración>"}
```

## Análisis de seguridad

Desde esta carpeta puedes analizar el código y las dependencias, por ejemplo:

```powershell
python -m pip install bandit pip-audit
bandit -r app.py -f txt
pip-audit -r requirements.txt
```

La práctica registra los hallazgos y verifica las correcciones con análisis estático y de dependencias. No uses ejemplos de inyección contra servicios externos.

## Build Breaker

El workflow `.github/workflows/security.yml` ejecuta Bandit y `pip-audit` en cada Pull Request dirigido a `main` y en cada push a `main`. El gate falla si cualquiera de los dos análisis falla.

## Hook local para detectar secretos

Instala las herramientas y el hook en cada entorno de desarrollo:

```powershell
python -m pip install -r requirements-dev.txt
pre-commit install
pre-commit run --all-files
```

El hook `detect-secrets` revisa los archivos preparados antes de cada commit. `.secrets.baseline` registra los hallazgos ficticios que ya contiene la API para que nuevas credenciales sean bloqueadas.
