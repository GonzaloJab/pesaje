# Pesaje Diario

Una web estática mínima: un gráfico público con el peso diario de todos y una página privada donde tú metes los números. Sin servidor, sin base de datos, gratis para siempre en GitHub Pages.

## Archivos

| Archivo | Qué es |
|---|---|
| `index.html` | El gráfico público. Lo ve cualquiera que tenga el enlace. |
| `k7m2q-admin.html` | Tu formulario privado. Renómbralo con tu propia cadena aleatoria. |
| `data.json` | Todos los datos. La página de admin reescribe este archivo vía la API de GitHub. |

## Publicar (unos 5 minutos)

1. **Crea el repositorio.** En GitHub: New repository → nómbralo `pesaje` → **Public** (Pages es gratis en repos públicos) → Create.
2. **Sube los archivos.** Arrastra `index.html`, `k7m2q-admin.html` y `data.json` al repositorio → Commit.
3. **Activa Pages.** Repositorio → Settings → Pages → Source: *Deploy from a branch* → Branch: `main`, carpeta `/ (root)` → Save.
4. Espera ~1 minuto. Tu web queda en:
   - Gráfico público: `https://TU-USUARIO.github.io/pesaje/`
   - Tu página de admin: `https://TU-USUARIO.github.io/pesaje/k7m2q-admin.html`

## Crear el token (para que la página de admin pueda guardar)

GitHub → Settings → Developer settings → **Personal access tokens → Fine-grained tokens** → Generate new token:

- **Repository access:** Only select repositories → `pesaje`
- **Permissions:** Repository permissions → **Contents: Read and write** (nada más)
- **Expiration:** 1 año está bien

Copia el token, abre tu página de admin, rellena usuario / repositorio / rama / token y pulsa **Conectar y cargar**. El token se guarda en el localStorage de ese navegador: solo lo introduces una vez por dispositivo.

## Uso diario

Abres la página de admin → la fecha ya viene puesta a hoy → escribes cada peso → **Guardar en GitHub**. El gráfico se actualiza en menos de un minuto.

Para añadir a alguien a mitad de camino: *Añadir persona* → nombre y color → le pones su peso → Guardar. Su línea simplemente empieza en esa fecha; los días anteriores quedan en blanco. Si dejas un campo vacío, esa persona se salta ese día y la línea une directamente los días en los que sí se pesó.

## Sobre la privacidad, sin adornos

Las webs de GitHub Pages son públicas, así que la *página* de admin es accesible para quien adivine el nombre del archivo — pero es solo un formulario vacío. Para escribir hace falta tu token, que vive únicamente en tu navegador y nunca aparece en el repositorio. Aun así, renombra `k7m2q-admin.html` con tu propia cadena aleatoria: lo mantiene fuera de la vista.

Ten en cuenta también que `data.json` está en un repositorio público, así que los pesos son públicos. Esa es la idea aquí — pero no metas ahí nada que no publicarías abiertamente.

## Editar a mano

La página de admin no es imprescindible. `data.json` es JSON plano y se puede editar desde el editor web de GitHub:

```json
{
  "unit": "kg",
  "people": [{ "id": "vader", "name": "Vader", "color": "#6c8cff" }],
  "entries": {
    "2026-08-25": { "vader": 85.0 }
  }
}
```

Las fechas van en `AAAA-MM-DD`. Las claves dentro de cada fecha son los `id` de cada persona. Si una persona no aparece, ese día no tiene registro.
