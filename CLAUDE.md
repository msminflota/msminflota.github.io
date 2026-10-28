# Flota MSMIN · Check list diario de camionetas — instrucciones

Dueño: Alonso Saavedra. Responde en español de Chile, simple y paso a paso (suele escribir desde el celular).
Antes de cambiar algo grande o que no se pueda deshacer (tablas, permisos, borrar datos), pregunta.
Este repo es PÚBLICO (GitHub Pages): nunca subir claves secretas (service_role, contraseñas). La clave anon de Supabase sí puede ir.

## Arquitectura
- `index.html`: toda la app (CSS + HTML + JS en un solo archivo, ~1.570 líneas, 150 KB). Sin compilación: se edita y se publica.
- Base de datos: Supabase (proyecto "Check List camionetas"). Tablas `perfiles`, `vehiculos`, `checks` en `supabase/1-base-de-datos.sql`;
  primer admin en `2-primer-admin.sql`; registro/borrado en `3-registro-y-borrado.sql`; función `supabase/functions/admin-usuarios/index.ts`.
  Los cambios de SQL o de la función los pega Alonso en Supabase: entrégale el texto y dile dónde pegarlo (ver `supabase/INSTALAR.md`).
- Roles: conductor (crea y ve sus checks) y administrador (panel, vehículos, usuarios, Excel).
- `sw.js`: service worker. Subir `VERSION = "flota-vNN"` en cada publicación para que los teléfonos bajen la versión nueva.
- `correos/`: plantillas de correo de Supabase Auth (se pegan a mano, ver `correos/COMO-PEGAR.md`).

## Mapa de index.html (busca por el título, las líneas cambian)
`grep -n "// ----------" index.html` lista las secciones:
- `<style>` al inicio (CSS) · línea ~232: logos en base64 (una línea de 25 KB: NO leerla; usa `cut -c1-200`).
- Definición del check (puntos del formulario) · Estado (`S`) · Conexión con Supabase · Modo sin conexión
- Utilidades · Cálculo de estado por vehículo (`estadoVehiculo`, `calcularFlota`, mantención por km)
- Render raíz (`render`, tabs) · Vista conductor: Check list (`renderCheck`, `enviarCheck`, firma, `descargarPDF` con jsPDF)
- Vista admin: Panel (`renderPanel`, `exportarExcel` con ExcelJS) · Vista admin: Vehículos (`guardarVeh`, `importar`)
- Login, clave y usuarios (`renderLogin`, `ingresar`, `renderUsuarios`, `cargar`, `sincronizar`) · Eventos · Arranque (`start`)

## Ahorro de contexto
- No leas index.html entero: `grep -n "nombreFuncion" index.html | cut -c1-200` y lee solo ese tramo (offset/limit).
- Ignora los .png (íconos y logos de correo).

## Publicar
Commit + push a `main` (GitHub Pages publica solo, en 1–2 min). Verifica en https://msminflota.github.io que cargue la versión nueva.
