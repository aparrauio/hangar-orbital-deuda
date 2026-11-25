# Spec 08 · Unificar el Botón secundario

**Objetivo:** que el Botón secundario (DESIGN.md §6.3) se defina en un solo lugar, `src/styles/formularios.css`, junto a la base y al primario. Cierra el pendiente 2 de la Spec 04 y la parte del Botón secundario de DT-8 en `docs/deuda-tecnica.md`.
**Tipo:** refactorización de estilos, sin cambios visibles. **Tickets:** T-25 y T-26.
**Carpeta de la spec:** `docs/specs/08-unificar-boton-secundario/`. Las capturas van en su subcarpeta `capturas/`.

### 0. Reglas de esta spec
1. `.boton--secundario` DEBE definirse una sola vez en `src/`, en `src/styles/formularios.css`, justo después de `.boton--primario` y antes de `.boton:disabled`.
2. La regla DEBE conservar los mismos valores que tiene hoy en `src/styles/misiones.css`: `color: var(--color-texto)`, `background: transparent` y `border-color: var(--color-borde-control)`.
3. La regla DEBE salir de `src/styles/misiones.css` junto con su comentario («Botón secundario: DESIGN.md §6.3. Pendiente de pasarlo a formularios.css.»).
4. La última viñeta de DESIGN.md §6.3 DEBE decir que la base, el primario y el secundario viven en `formularios.css`, sin «de momento».
5. NO DEBE cambiar ningún `className`, componente, test ni otra regla de CSS.
6. NO DEBE modificar archivos fuera de la tabla de la sección 3.
7. Todos los tests DEBEN pasar sin modificarlos, y `npm run build` DEBE compilar.

**Por qué no cambia el aspecto:** `.boton--secundario` tiene la misma especificidad que `.boton` y sigue quedando después de esa regla. `.boton:disabled` y `.boton:focus-visible` tienen más especificidad, así que siguen ganando. Al escribir esta spec, ningún otro archivo de `src/styles/` asigna `color`, `background` ni `border-color` a `.boton`.

### 1. Criterios de aceptación
- **CA-08.1:** DADO el código de `src/`, CUANDO se ejecuta `grep -rn "\.boton--secundario {" src/`, ENTONCES devuelve una sola línea, en `src/styles/formularios.css`.
- **CA-08.2:** DADO `src/styles/formularios.css`, CUANDO se lee la sección «Botón», ENTONCES `.boton--secundario` está después de `.boton--primario` y antes de `.boton:disabled`, con los tres valores de la regla 2.
- **CA-08.3:** DADO `src/styles/misiones.css`, CUANDO se buscan «Botón secundario» y `.boton--secundario`, ENTONCES no aparece ninguno de los dos.
- **CA-08.4:** DADO DESIGN.md §6.3, CUANDO se lee su última viñeta, ENTONCES dice que la base, el primario y el secundario viven en `formularios.css`, y no contiene «de momento».
- **CA-08.5:** DADO el cambio terminado, CUANDO se ejecuta `git diff main --name-only`, ENTONCES solo lista archivos de la tabla de la sección 3, y ningún `className`, componente ni test cambia.
- **CA-08.6:** DADO el cambio terminado, CUANDO se ejecutan `npm test` y `npm run build`, ENTONCES todos los tests pasan sin modificarlos y el build compila.
- **CA-08.7:** DADO los mismos datos de prueba que las capturas `antes-` (los de la Spec 07: «Aurora» con «Titán», «Europa» y «Ceres»), CUANDO se toman las capturas `despues-misiones` y `despues-formulario` a 390 y a 1280 px, ENTONCES cada una se ve idéntica a su `antes-misiones` o `antes-formulario` del mismo ancho.

### 2. Verificación
| Criterio | Cómo |
| --- | --- |
| CA-08.1 | Comando: `grep -rn "\.boton--secundario {" src/` devuelve una sola línea |
| CA-08.2, CA-08.3, CA-08.4 | Revisión de `formularios.css`, `misiones.css` y DESIGN.md §6.3 |
| CA-08.5 | Comando: `git diff main --name-only` |
| CA-08.6 | `npm test` (sin tocar ningún test) y `npm run build` |
| CA-08.7 | Capturas `despues-*` comparadas con las `antes-*` |

**Capturas** (con `npm run dev` corriendo; se guardan en `docs/specs/08-unificar-boton-secundario/capturas/`). Las `antes-` ya están tomadas. Después del cambio, se toman en este orden:
1. **despues-misiones:** `npm run captura -- 08-unificar-boton-secundario despues-misiones "clic:Ver misiones" "esperar:Titán"`
2. **despues-formulario:** `npm run captura -- 08-unificar-boton-secundario despues-formulario "clic:Ver misiones" "esperar:Titán" "clic:Completar"`

Cada captura `despues-*-movil.png` y `despues-*-escritorio.png` se abre junto a su `antes-*` del mismo ancho. Cualquier diferencia es un fallo de CA-08.7.

**Fechas:** los datos de la Spec 07 son relativos a `current_date`. Si las capturas `despues-` se toman otro día que las `antes-`, cambian los textos de fecha de «Titán» y «Ceres» («Despegó el DD/MM/AAAA»). Conviene tomarlas el mismo día. Si no se puede, se anota la diferencia en «Resultados».

### 3. Tickets y archivos
| Ticket | Crea | Modifica |
| --- | --- | --- |
| T-25 · Mover el Botón secundario a formularios.css | — | `src/styles/formularios.css`, `src/styles/misiones.css`, `DESIGN.md` (§6.3) |
| T-26 · Verificar que nada cambia | `docs/specs/08-unificar-boton-secundario/capturas/despues-*.png` | Esta spec (resultados) |

### Resultados
**Automático, 2026-11-25:**
- `npm test` antes de cambiar código: 4 archivos, 94 de 94 tests. Después del cambio: los mismos 94 de 94, sin modificar ningún test (`git diff --name-only -- '*.test.ts'` no lista nada).
- `npm run build`: compila sin errores.
- CA-08.1: `grep -rn "\.boton--secundario {" src/` devuelve una sola línea: `src/styles/formularios.css:57:.boton--secundario {`.
- CA-08.2: la regla está entre `.boton--primario` y `.boton:disabled`, con `color: var(--color-texto)`, `background: transparent` y `border-color: var(--color-borde-control)`.
- CA-08.3: ni «Botón secundario» ni `boton--secundario` aparecen en `src/styles/misiones.css`.
- CA-08.4: la última viñeta de DESIGN.md §6.3 dice «La base, el primario y el secundario viven en `formularios.css`.» y no contiene «de momento».
- CA-08.5: el cambio solo modifica `src/styles/formularios.css`, `src/styles/misiones.css` y `DESIGN.md`, y añade esta spec y las capturas `despues-*`. No cambia ningún `className`, componente ni test.

**Capturas contra las `antes-` (CA-08.7), 2026-11-25:** tomadas el mismo día que las `antes-` y con los mismos datos.

| Captura | Comparada con | A simple vista | `cmp` byte a byte |
| --- | --- | --- | --- |
| `despues-misiones-movil.png` | `antes-misiones-movil.png` | Idéntica | Iguales (123 211 bytes) |
| `despues-misiones-escritorio.png` | `antes-misiones-escritorio.png` | Idéntica | Iguales (104 536 bytes) |
| `despues-formulario-movil.png` | `antes-formulario-movil.png` | Idéntica | Iguales (126 815 bytes) |
| `despues-formulario-escritorio.png` | `antes-formulario-escritorio.png` | Idéntica | Iguales (108 524 bytes) |

- **Borde de los botones secundarios:** en el navegador, con el formulario de completar abierto, «Registrar revisión», «Guardar», «Ocultar misiones», «Ver misiones» y «Cancelar» tienen un borde `1px solid rgb(107, 124, 158)`, que es `--color-borde-control` (`#6b7c9e`). También tienen fondo transparente y texto `rgb(232, 238, 247)` (`--color-texto`). Solo se carga una regla `.boton--secundario`.
- **Diferencias con lo que prevé la spec:**
  - **Puerto:** las capturas se tomaron con `--url http://localhost:5174`, porque el puerto 5173 lo ocupa otra app. El servidor del 5174 es el de este proyecto y ya servía el `misiones.css` nuevo, sin la regla.
  - **Datos de prueba:** no son exactamente los de la Spec 07. «Titán» ya estaba completada, Aurora al 90 % y «Europa» es la misión pendiente. Son los mismos datos que las capturas `antes-`, así que la comparación vale.

### Pendientes
(La implementación anota aquí lo que encuentre fuera de alcance.)
- **Comentario desactualizado:** la primera línea de `src/styles/misiones.css` dice «bloque 7 (spec 04)», pero el bloque «Misiones» ya es el 9. La regla 5 no permite tocarlo en esta spec.
