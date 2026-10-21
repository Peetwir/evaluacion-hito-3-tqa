# Registro de Defectos — SauceDemo

**Materia:** Testing & Quality Assurance (TQA) · **Examen Práctico Hito 3**
**Estudiante:** Cris Kleber Ballon Chipana
**Aplicación:** SauceDemo (Swag Labs) — https://www.saucedemo.com/

---

> **EJEMPLO ILUSTRATIVO.** Los resultados, la fecha, el entorno y el defecto de este archivo son datos de muestra para práctica en clase. No provienen de una ejecución real, no incluyen capturas reales y no deben presentarse como evidencia.

## 1. Reglas de registro

- Solo se documentan **comportamientos reproducibles**, observados durante la ejecución de los casos de prueba.
- Si una prueba **pasa**, se registra como *Passed* y **no se inventa un bug**.
- Si una prueba **falla**, se registra lo observado y se documenta el defecto con evidencia.
- Un defecto se considera reproducible cuando se repite siguiendo los mismos pasos, con los mismos datos y en el mismo navegador (se recomienda reproducirlo al menos dos veces).
- Cada defecto debe tener su propio **Issue** en GitHub enlazado a este archivo y a su captura.

## 2. Resumen de defectos

| ID | Título | Caso relacionado | Severidad | Prioridad | Estado | Issue |
|---|---|---|---|---|---|---|
| BUG-01 | Imágenes repetidas en el catálogo con problem_user | CN-01 (variante con problem_user) | Baja | Baja | Abierto | #1 |

En este ejemplo los cinco casos de caja negra pasaron y se documenta un solo defecto ilustrativo, hallado en una variante del login con `problem_user`.

## 3. Guía de severidad y prioridad

| Severidad | Criterio | Ejemplo |
|---|---|---|
| **Crítica** | Bloquea una función principal y no hay alternativa | No se puede completar la compra |
| **Alta** | Afecta una función importante, con alternativa difícil | Un producto no se puede agregar al carrito |
| **Media** | La función opera, pero con comportamiento incorrecto | El orden del catálogo no coincide con el filtro elegido |
| **Baja** | Problema cosmético o de texto, sin impacto funcional | Imagen o texto desalineado |

| Prioridad | Significado |
|---|---|
| **Alta** | Debe corregirse antes de la siguiente entrega |
| **Media** | Debe corregirse en el ciclo actual |
| **Baja** | Puede posponerse |

## 4. Reportes de defectos

Duplicar el siguiente bloque por cada defecto encontrado.

### BUG-01 — Imágenes repetidas en el catálogo con problem_user

| Campo | Contenido |
|---|---|
| **ID** | BUG-01 |
| **Título** | Imágenes repetidas en el catálogo con problem_user |
| **Caso relacionado** | CN-01 (variante con problem_user) |
| **Descripción** | Con el usuario `problem_user`, las imágenes de los productos del catálogo no corresponden a cada producto: se repite la misma imagen. |
| **Entorno** | Google Chrome 141, Windows 11 |
| **Precondiciones** | Navegador abierto en la página de login; no hay sesión iniciada |
| **Datos de prueba** | Usuario: `problem_user` · Contraseña: `secret_sauce` |
| **Pasos para reproducirlo** | 1. Ingresar a https://www.saucedemo.com/<br>2. Iniciar sesión con `problem_user`<br>3. Observar las imágenes de los productos en el catálogo |
| **Resultado esperado** | Cada producto muestra su propia imagen |
| **Resultado actual** | Los productos del catálogo muestran la misma imagen repetida |
| **Severidad** | Baja |
| **Prioridad** | Baja |
| **Frecuencia** | Siempre (reproducido 2 veces) |
| **Evidencia** | `evidencias/capturas/BUG-01.png` |
| **Estado** | Abierto |

---

## 5. Guía de trabajo (borrar esta sección antes de entregar)

### 5.1 Lista de verificación antes de reportar

- [ ] El resultado obtenido realmente difiere del esperado.
- [ ] Reproduje el problema al menos dos veces con los mismos pasos.
- [ ] No es un comportamiento documentado del sitio (por ejemplo, los usuarios de demostración tienen comportamientos intencionales).
- [ ] Tengo una captura que muestra el resultado actual.
- [ ] Los pasos son claros para que otra persona los repita.

### 5.2 Dónde buscar defectos reales

SauceDemo incluye usuarios de demostración pensados para practicar. Los siguientes comportamientos se reportan con frecuencia, pero **son hipótesis a verificar tú mismo**: si no los observas, no los reportes.

| Usuario | Qué revisar |
|---|---|
| `problem_user` | Imágenes de los productos en el catálogo; botones "Add to cart" y "Remove" en distintos productos; orden del catálogo al aplicar el filtro; campos del formulario de checkout |
| `error_user` | Botones del carrito y finalización de la compra |
| `performance_glitch_user` | Tiempo de respuesta al iniciar sesión |
| `visual_user` | Diferencias visuales en la interfaz |

### 5.3 Cómo vincular el bug con GitHub

1. Crear un Issue con el título `BUG-01 — [título]` y la etiqueta `bug`.
2. Pegar en el Issue los pasos, resultado esperado y actual, y adjuntar la captura.
3. Enlazar este archivo y el caso fallido (por ejemplo, `docs/casos_caja_negra.md`).
4. Mover el Issue en el Board: *To Do* → *In Progress* → *In Review* → *Done*.
