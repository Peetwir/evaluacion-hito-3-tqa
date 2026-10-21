# Casos de prueba de Caja Negra — SauceDemo

**Materia:** Testing & Quality Assurance (TQA) · **Examen Práctico Hito 3**
**Estudiante:** Cris Kleber Ballon Chipana
**Aplicación bajo prueba:** SauceDemo (Swag Labs) — https://www.saucedemo.com/

---

> **EJEMPLO ILUSTRATIVO.** Los resultados, la fecha, el entorno y el defecto de este archivo son datos de muestra para práctica en clase. No provienen de una ejecución real, no incluyen capturas reales y no deben presentarse como evidencia.

## 1. Alcance y enfoque

Las pruebas de caja negra evalúan el comportamiento de la aplicación **desde la perspectiva del usuario**, sin conocer el código fuente. Se verifica que, dadas ciertas entradas, el sistema produzca las salidas esperadas.

**Dentro del alcance:** inicio de sesión, catálogo de productos, carrito de compras y checkout.
**Fuera del alcance:** rendimiento, seguridad, accesibilidad y pruebas en dispositivos móviles.

### Técnicas de diseño aplicadas

| Técnica | Dónde se aplica |
|---|---|
| Partición de equivalencia | Clases válidas e inválidas de usuario en el login (CN-01, CN-02) |
| Análisis de campos obligatorios | Formulario de checkout con un campo vacío (CN-04) |
| Prueba de flujo de extremo a extremo | Compra completa (CN-05) |
| Prueba de transición de estado | Botón "Add to cart" → "Remove" y contador del carrito (CN-03) |

## 2. Entorno de pruebas

| Dato | Detalle |
|---|---|
| URL | https://www.saucedemo.com/ |
| Navegador y versión | Google Chrome 141 |
| Sistema operativo | Windows 11 |
| Fecha de ejecución | 20/10/2026 |
| Usuarios de demostración | `standard_user`, `locked_out_user`, `problem_user` |
| Contraseña común | `secret_sauce` |

## 3. Resumen de resultados

| ID | Caso | Funcionalidad | Prioridad | Estado |
|---|---|---|---|---|
| CN-01 | Inicio de sesión válido | Login | Alta | Passed |
| CN-02 | Inicio de sesión con usuario bloqueado | Login | Alta | Passed |
| CN-03 | Agregar producto al carrito | Carrito | Alta | Passed |
| CN-04 | Campo obligatorio vacío en checkout | Checkout | Media | Passed |
| CN-05 | Compra completa | Checkout | Alta | Passed |

---

## 4. Casos de prueba

### CN-01 — Inicio de sesión válido

| Campo | Contenido |
|---|---|
| **Objetivo** | Verificar que un usuario con credenciales válidas accede al catálogo |
| **Técnica** | Partición de equivalencia (clase válida) |
| **Precondiciones** | Navegador abierto en la página de login; no hay sesión iniciada |
| **Datos de prueba** | Usuario: `standard_user` · Contraseña: `secret_sauce` |
| **Pasos** | 1. Ingresar a https://www.saucedemo.com/<br>2. Escribir el usuario en el campo *Username*<br>3. Escribir la contraseña en el campo *Password*<br>4. Presionar el botón **Login** |
| **Resultado esperado** | Se muestra la página de catálogo con el título "Products" y la lista de productos |
| **Resultado obtenido** | Se redirigió al catálogo (Products) y se listaron 6 productos. |
| **Estado** | Passed |
| **Evidencia** | `evidencias/capturas/CN-01.png` |
| **Postcondición** | Sesión iniciada; el usuario permanece en el catálogo |

### CN-02 — Inicio de sesión con usuario bloqueado

| Campo | Contenido |
|---|---|
| **Objetivo** | Verificar que el sistema rechaza el acceso de un usuario bloqueado y muestra un mensaje claro |
| **Técnica** | Partición de equivalencia (clase inválida) |
| **Precondiciones** | Navegador abierto en la página de login; no hay sesión iniciada |
| **Datos de prueba** | Usuario: `locked_out_user` · Contraseña: `secret_sauce` |
| **Pasos** | 1. Ingresar a https://www.saucedemo.com/<br>2. Escribir el usuario bloqueado<br>3. Escribir la contraseña<br>4. Presionar **Login** |
| **Resultado esperado** | No se accede al catálogo. Aparece el mensaje "Epic sadface: Sorry, this user has been locked out." |
| **Resultado obtenido** | Se mostró el mensaje "Epic sadface: Sorry, this user has been locked out." y se permaneció en el login. |
| **Estado** | Passed |
| **Evidencia** | `evidencias/capturas/CN-02.png` |
| **Postcondición** | El usuario permanece en la página de login |

### CN-03 — Agregar producto al carrito

| Campo | Contenido |
|---|---|
| **Objetivo** | Verificar que un producto se agrega correctamente al carrito y el contador se actualiza |
| **Técnica** | Transición de estado |
| **Precondiciones** | Sesión iniciada con `standard_user`; carrito vacío |
| **Datos de prueba** | Producto: *Sauce Labs Backpack* · Cantidad: 1 |
| **Pasos** | 1. En el catálogo, ubicar el producto<br>2. Presionar **Add to cart**<br>3. Observar el botón del producto y el ícono del carrito<br>4. Abrir el carrito |
| **Resultado esperado** | El botón cambia a **Remove**; el ícono del carrito muestra **1**; el producto aparece en la lista del carrito |
| **Resultado obtenido** | El botón cambió a Remove, el ícono del carrito mostró 1 y la mochila apareció en el carrito. |
| **Estado** | Passed |
| **Evidencia** | `evidencias/capturas/CN-03.png` |
| **Postcondición** | El carrito contiene un producto |

### CN-04 — Campo obligatorio vacío en el checkout

| Campo | Contenido |
|---|---|
| **Objetivo** | Verificar que el formulario de checkout exige el campo *First Name* |
| **Técnica** | Análisis de campos obligatorios |
| **Precondiciones** | Sesión iniciada con `standard_user`; un producto en el carrito |
| **Datos de prueba** | First Name: *(vacío)* · Last Name: `Ballon` · Postal Code: `0000` |
| **Pasos** | 1. Abrir el carrito y presionar **Checkout**<br>2. Dejar *First Name* vacío<br>3. Completar *Last Name* y *Postal Code*<br>4. Presionar **Continue** |
| **Resultado esperado** | No se avanza al resumen. Aparece el mensaje "Error: First Name is required" |
| **Resultado obtenido** | Se mostró "Error: First Name is required" y no se avanzó al resumen. |
| **Estado** | Passed |
| **Evidencia** | `evidencias/capturas/CN-04.png` |
| **Postcondición** | El usuario permanece en el formulario de checkout |

### CN-05 — Compra completa

| Campo | Contenido |
|---|---|
| **Objetivo** | Verificar que el flujo completo de compra finaliza correctamente |
| **Técnica** | Prueba de flujo de extremo a extremo |
| **Precondiciones** | Sesión iniciada con `standard_user`; un producto en el carrito |
| **Datos de prueba** | First Name: `Kleber` · Last Name: `Ballon` · Postal Code: `0000` |
| **Pasos** | 1. Abrir el carrito y presionar **Checkout**<br>2. Completar los tres campos<br>3. Presionar **Continue**<br>4. Revisar el resumen (producto, precio, total)<br>5. Presionar **Finish** |
| **Resultado esperado** | Se muestra la pantalla de confirmación "Thank you for your order!" y el carrito queda vacío |
| **Resultado obtenido** | Se mostró "Thank you for your order!" y el carrito quedó vacío. |
| **Estado** | Passed |
| **Evidencia** | `evidencias/capturas/CN-05.png` |
| **Postcondición** | Pedido confirmado; el contador del carrito vuelve a 0 |

---

## 5. Matriz de trazabilidad

| Caso | Funcionalidad probada | Escenario Gherkin relacionado | Bug asociado |
|---|---|---|---|
| CN-01 | Login exitoso | Acceso exitoso | BUG-01 (variante con problem_user) |
| CN-02 | Login con usuario bloqueado | Usuario bloqueado | ninguno |
| CN-03 | Agregar al carrito | Agregar un producto al carrito | ninguno |
| CN-04 | Validación de campos en checkout | Campo obligatorio vacío | ninguno |
| CN-05 | Compra completa | Compra completa exitosa | ninguno |

## 6. Criterios de aceptación de la ejecución

- Cada caso se ejecuta como mínimo una vez con los datos indicados.
- El resultado obtenido se registra tal como se observó, sin modificarlo para que coincida con el esperado.
- Un caso es **Passed** si el resultado obtenido coincide con el esperado; es **Failed** si difiere.
- Todo caso **Failed** se documenta como defecto en [`../bugs/bugs_encontrados.md`](../bugs/bugs_encontrados.md) con evidencia.

## 7. Conclusiones de la ejecución

Los 5 casos pasaron (5 de 5, 100 %). Los resultados obtenidos coincidieron con los esperados en login, carrito, validación de campos y compra completa. Como observación adicional, en una variante del login con `problem_user` se registró un defecto de ejemplo (BUG-01, severidad Baja) sobre imágenes repetidas en el catálogo.
