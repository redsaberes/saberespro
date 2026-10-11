# Campo Matemático · Colegio Alemania Unificada IED

Sitio web estático (sin servidor) con los planes de mejoramiento por **ciclo → periodo → docente → estudiante**.
Los estudiantes solo descargan; al hacerlo registran su nombre y curso en una hoja de Google que el administrador exporta a Excel.

```
index.html          Sitio público
administrador.html  Herramienta para preparar planes (la usa solo el administrador)
planes.json         Lista de planes publicados
planes/             Archivos de los planes (PDF, Word, etc.)
config.js           Dirección del registro de descargas
apps-script/        Código del registro en Google Sheets
```

## Paso a paso

### 1. Crear la organización en GitHub (si aún no existe)
1. Entra a github.com con la cuenta institucional > icono **+** > **New organization**.
2. Elige el plan **Free**, escribe el nombre (ej. `cedalemaniaunificada`) y el correo del colegio.
3. Invita a los docentes que vayan a ayudar en **People > Invite member**.

### 2. Crear el repositorio dentro de la organización
1. En la organización: **Repositories > New repository**.
2. Nombre: `campo-matematico`. Visibilidad: **Public** (necesario para GitHub Pages en el plan Free; ver aviso de privacidad abajo).
3. No marques "Add a README". Pulsa **Create repository**.

### 3. Subir este proyecto
Sin instalar nada: en el repositorio vacío pulsa **uploading an existing file**, arrastra **todo el contenido de la carpeta** (incluida la carpeta `planes` y `apps-script`) y pulsa **Commit changes**.

Con Git (opcional):
```bash
cd campo-matematico
git init -b main
git add .
git commit -m "Campo Matemático: versión inicial"
git remote add origin https://github.com/NOMBRE-DE-LA-ORGANIZACION/campo-matematico.git
git push -u origin main
```
Si `planes/` está vacía, GitHub no la conserva: por eso incluimos `planes/.gitkeep`.

### 4. Activar GitHub Pages
1. En el repositorio: **Settings > Pages**.
2. En **Build and deployment** elige **Deploy from a branch**, rama `main`, carpeta `/ (root)` y **Save**.
3. En 1 o 2 minutos aparece la dirección: `https://NOMBRE-DE-LA-ORGANIZACION.github.io/campo-matematico/`.

### 5. Publicar planes de mejoramiento
1. Abre `administrador.html` en el sitio (`.../administrador.html`) o desde tu computador.
2. (Si ya hay planes) pulsa **Cargar planes.json actual** y elige el `planes.json` del repositorio.
3. Elige el archivo, ciclo, periodo, docente, **nombre completo del estudiante y curso** (obligatorios; el sitio mostrará solo el primer nombre y las iniciales de los apellidos, ej. "Ana M. P.") y **Agregar plan**.
4. Descarga `planes.json` y cada **archivo** (la herramienta les pone un nombre ordenado).
5. En GitHub: carpeta `planes/` > **Add file > Upload files** y sube los archivos. Luego en la raíz abre `planes.json` > lápiz (editar) y reemplaza su contenido, o súbelo de nuevo con el mismo nombre. **Commit changes**.
6. Para quitar un plan: borra su archivo en `planes/` y su entrada en `planes.json`.

Límite: 25 MB por archivo al subirlo desde el navegador.

### 6. Registro de descargas en Excel (Google Sheets)
1. Crea una hoja de cálculo nueva en Google Drive (ej. "Registro de descargas Campo Matemático").
2. **Extensiones > Apps Script**. Borra lo que haya y pega el contenido de `apps-script/registro.gs`. Guarda.
3. **Implementar > Nueva implementación** > tipo **Aplicación web**.
   - Ejecutar como: **Yo**.
   - Quién tiene acceso: **Cualquier persona**.
   - Pulsa **Implementar** y autoriza los permisos.
4. Copia la **URL de la aplicación web** (termina en `/exec`).
5. En GitHub edita `config.js` y pega la URL entre las comillas de `REGISTRO_URL`. **Commit changes**.
6. Prueba: descarga un plan desde el sitio y revisa que aparezca una fila en la hoja.
7. Para Excel: en la hoja, **Archivo > Descargar > Microsoft Excel (.xlsx)**.

Cada fila guarda: fecha y hora, quién descargó y su curso, de quién es el plan, ciclo, periodo, docente y archivo.
Si cambias el código del Apps Script, debes hacer **Implementar > Administrar implementaciones > Editar > Nueva versión**.

### 7. Dominio propio (opcional)
1. **Settings > Pages > Custom domain**: escribe tu dominio (ej. `campomatematico.edu.co` o `www.…`) y guarda.
2. En el panel de tu proveedor de dominio crea un registro **CNAME** de `www` hacia `NOMBRE-DE-LA-ORGANIZACION.github.io` (para el dominio raíz usa los registros A que indica la [documentación de GitHub Pages](https://docs.github.com/pages/configuring-a-custom-domain-for-your-github-pages-site)).
3. Marca **Enforce HTTPS** cuando esté disponible.

## Avisos importantes
- **Privacidad:** GitHub Free solo publica Pages desde repositorios públicos, así que cualquiera puede ver `planes.json` y `planes/`. Por eso el sitio y `planes.json` guardan solo el primer nombre y las iniciales de los apellidos, y los nombres de archivo no llevan el nombre del estudiante. Ojo: el contenido del propio PDF/Word sí puede traer el nombre completo; si eso es un problema, quítalo del documento antes de subirlo. Consulta con la rectoría la política de tratamiento de datos.
- **Registro de descargas:** la dirección del Apps Script es pública; no la compartas fuera del sitio. Solo quien administra la hoja puede leer los datos.
- Los registros los escribe el navegador del estudiante; no son una prueba infalible de identidad.
