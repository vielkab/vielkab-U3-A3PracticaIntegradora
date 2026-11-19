# API REST de práctica: SAST y SCA

API Flask local con fallos **intencionales** para el laboratorio: una clave fija en el código, una consulta SQL vulnerable y dependencias desactualizadas. Usa únicamente los datos ficticios incluidos; no agregues credenciales reales ni la expongas a una red pública.

## Requisitos y ejecución

1. Abre PowerShell en esta carpeta.
2. Crea y activa un entorno virtual:

   ```powershell
   py -m venv .venv
   .\.venv\Scripts\Activate.ps1
   ```

3. Instala las dependencias del escenario vulnerable:

   ```powershell
   python -m pip install -r requirements.txt
   ```

4. Inicia la API:

   ```powershell
   python app.py
   ```

La API escucha solo en `http://127.0.0.1:5000`. Al iniciar crea `practica.db` con usuarios ficticios.

## Endpoints

- `GET /api/health` — comprueba que la API está activa.
- `GET /api/profile` — requiere el encabezado `X-API-Key` con el valor de demostración que aparece en `app.py`.
- `POST /api/login` — recibe JSON con `username` y `password`; usa `admin` / `AdminDemo123!` o `estudiante` / `Practica123!` para el caso válido.

Ejemplo de cuerpo para `POST /api/login`:

```json
{"username":"admin","password":"AdminDemo123!"}
```

## Análisis de seguridad

Desde esta carpeta puedes analizar el código y las dependencias, por ejemplo:

```powershell
python -m pip install bandit pip-audit
bandit -r app.py -f txt
pip-audit -r requirements.txt
```

La práctica consiste en registrar los hallazgos, corregirlos (consultas parametrizadas, secretos desde variables de entorno y actualización de dependencias) y volver a ejecutar los análisis para comparar los resultados. No uses los ejemplos de inyección contra servicios externos.