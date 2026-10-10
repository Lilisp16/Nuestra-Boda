# Nuestra Boda — Lili & David

Invitación web personalizada, publicada en **GitHub Pages**. Cada invitación se abre **solo en los celulares registrados** de esa invitación. En cualquier otro celular se ve un sobre sellado con candado y el mensaje **"Lo siento, este es un evento privado"**.

## Cómo funciona
- Cada invitado recibe un **enlace único** por WhatsApp. No se le pide ningún dato.
- El primer celular que abre el enlace queda registrado (en las invitaciones con 2 celulares, los dos primeros). Un tercero ve el sobre con candado.
- Los datos de la boda van **cifrados** dentro de `docs/`. La clave para abrirlos solo se entrega a un celular registrado. Por eso el código publicado **no contiene ningún dato personal**, aunque el repositorio sea público.
- Para saber qué celulares ya abrieron hace falta un registro. Es un script pequeño en tu cuenta de Google (`servidor_registro.gs`): **sin hoja de cálculo, sin importar nada**.

## Qué hay en esta carpeta
- `docs/` — **lo único que se publica**: `index.html`, `d/` (invitaciones cifradas) y `a/` (foto y audio cifrados).
- `README.md` y `.gitignore` — para ti.
- La lista de invitados, los teléfonos, las claves y el registro están en la carpeta `2_PRIVADO` del proyecto. **Nunca la subas a GitHub.**

## Puesta en marcha (una sola vez)

### Paso 1 — Crear el registro (2 minutos, sin hoja de cálculo)
1. Entra a **script.google.com** y pulsa **Nuevo proyecto**.
2. Borra lo que aparezca, abre `2_PRIVADO/servidor_registro.gs` con el Bloc de notas, copia todo, pégalo y guarda (**Ctrl + S**).
3. Arriba a la derecha: **Implementar → Nueva implementación → engranaje → Aplicación web**.
   - Ejecutar como: **Yo**. Quién tiene acceso: **Cualquier persona**. Pulsa **Implementar**.
   - Google te pide permisos: *Revisar permisos → tu cuenta → Configuración avanzada → Ir a (proyecto) → Permitir*. Es tu propio script.
4. Copia la **URL de la aplicación web** (termina en `/exec`).

### Paso 2 — Poner esa dirección en la página
En la carpeta `2_PRIVADO`, ejecuta (la primera vez: `pip install cryptography`):
`python3 generar_invitaciones.py --api PEGA-AQUI-LA-URL`

### Paso 3 — Publicar en GitHub Pages
1. Sube a tu repositorio `Nuestra-Boda` (rama `main`): `docs`, `README.md` y `.gitignore`. **No subas nada de `2_PRIVADO`.**
2. GitHub Pages gratis solo funciona con repositorios **públicos**: *Settings → General → Danger Zone → Change repository visibility → Make public*. Es seguro porque todo lo personal va cifrado.
3. *Settings → Pages → Source: Deploy from a branch → Branch `main` y carpeta `/docs` → Save*.
4. Queda en `https://lilisp16.github.io/Nuestra-Boda/`

### Paso 4 — Probar y enviar
1. Abre `enviar_invitaciones.html` y confirma la dirección de arriba.
2. Prueba primero con **Lili (prueba)**: envíate el enlace y ábrelo en tu celular. Luego ábrelo en otro celular o computador: debe verse el sobre con candado.
3. Si todo sale bien, envía los demás con el botón de WhatsApp de cada fila.

## Cambiar algo
- **Una hora, un texto, la foto o el audio:** edita `2_PRIVADO/evento.csv`, `historia.csv` o `archivos/`, ejecuta `python3 generar_invitaciones.py` y vuelve a subir `docs`. Los enlaces **no cambian** y los celulares ya registrados siguen registrados. No hay que tocar el script de Google.
- **Una invitación nueva o un celular nuevo:** agrégala en `invitados.csv` (con un código de 8 letras/números sin repetir; 1 o 2 celulares), ejecuta el generador, vuelve a subir `docs` y, en script.google.com, pega el nuevo `servidor_registro.gs` (*Implementar → Gestionar implementaciones → lápiz → Versión: Nueva versión → Implementar*). La URL no cambia.
- **Ver quién ya abrió:** en script.google.com elige la función `ver` y pulsa **Ejecutar**; el resultado sale en el "Registro de ejecución".
- **Liberar una invitación** (alguien cambió de celular): en el script, abre la función `reiniciar`, pon el código de esa persona donde dice `PON-AQUI-EL-CODIGO`, y pulsa **Ejecutar**. El siguiente celular que la abra queda registrado.

## Límites
- La página no puede leer el número de teléfono del celular (los navegadores no lo permiten). Registra el celular **desde el que se abre el enlace por primera vez**: lo identifica con una marca guardada en ese navegador.
- Si alguien abre el enlace primero en el navegador interno de WhatsApp y luego en Chrome o Safari, puede contarse como otro celular y quedar bloqueado: se libera con `reiniciar`. Pídeles que siempre lo abran desde el mismo enlace del chat.
- Si alguien abre el enlace en modo privado, o borra los datos del navegador, también puede contarse como otro celular.
- Si el registro de Google no responde, la página muestra "No pudimos cargar tu invitación" con un botón para reintentar (no abre sin registro).
- Alternativas sin registro (menos seguras): `--modo numero` (pide el número de celular) o `--modo abierto` (cualquiera con el enlace).
