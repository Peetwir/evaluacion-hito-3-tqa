# Escenarios Gherkin (BDD) — SauceDemo

**Materia:** Testing & Quality Assurance (TQA) · **Examen Práctico Hito 3**
**Estudiante:** Cris Kleber Ballon Chipana
**Aplicación:** SauceDemo (Swag Labs) — https://www.saucedemo.com/

---

## 1. ¿Qué es Gherkin?

Gherkin es un lenguaje de texto plano para describir el **comportamiento esperado** de una funcionalidad mediante ejemplos legibles para todo el equipo (BDD, *Behavior-Driven Development*). Cada escenario se compone de:

| Palabra clave | Función |
|---|---|
| `Feature` | Nombra la funcionalidad que se describe |
| `Background` | Pasos comunes a todos los escenarios de la funcionalidad |
| `Scenario` | Un ejemplo concreto de comportamiento |
| `Scenario Outline` + `Examples` | Un mismo escenario repetido con distintos datos |
| `Given` | Contexto o estado inicial |
| `When` | Acción que realiza el usuario |
| `Then` | Resultado observable que se verifica |
| `And` / `But` | Continúan o contrastan el paso anterior |

> Gherkin describe escenarios; por sí solo **no automatiza** las pruebas. Para ejecutarlos se necesitaría una herramienta como Cucumber o Behave con código que implemente cada paso.

## 2. Convenciones usadas

- Palabras clave en inglés (`Feature`, `Scenario`, `Given`, `When`, `Then`) y texto de los pasos en español.
- Los pasos son claros, verificables y escritos desde la perspectiva del usuario.
- Cada escenario verifica **un solo comportamiento**.
- Las etiquetas (`@login`, `@carrito`, `@checkout`) permiten agrupar y filtrar escenarios.

---

## 3. Feature: Inicio de sesión

```gherkin
@login
Feature: Inicio de sesión en SauceDemo
  Como usuario de la tienda
  Quiero iniciar sesión con mis credenciales
  Para acceder al catálogo de productos

  Background:
    Given que estoy en la página de login de SauceDemo

  @exitoso
  Scenario: Acceso exitoso
    When ingreso el usuario "standard_user" y la contraseña "secret_sauce"
    And presiono el botón "Login"
    Then debería ver la página de catálogo "Products"
    And debería ver la lista de productos disponibles

  @error
  Scenario: Usuario bloqueado
    When ingreso el usuario "locked_out_user" y la contraseña "secret_sauce"
    And presiono el botón "Login"
    Then debería ver el mensaje de error "Epic sadface: Sorry, this user has been locked out."
    But no debería acceder al catálogo

  @error
  Scenario: Contraseña incorrecta
    When ingreso el usuario "standard_user" y la contraseña "clave_incorrecta"
    And presiono el botón "Login"
    Then debería ver un mensaje de error indicando que el usuario y la contraseña no coinciden
    And debería permanecer en la página de login

  @error
  Scenario Outline: Campos obligatorios del login
    When ingreso el usuario "<usuario>" y la contraseña "<contraseña>"
    And presiono el botón "Login"
    Then debería ver el mensaje de error "<mensaje>"

    Examples:
      | usuario       | contraseña   | mensaje                          |
      |               | secret_sauce | Username is required             |
      | standard_user |              | Password is required             |
```

## 4. Feature: Carrito de compras

```gherkin
@carrito
Feature: Carrito de compras
  Como usuario autenticado
  Quiero agregar y quitar productos del carrito
  Para preparar mi compra

  Background:
    Given que inicié sesión con el usuario "standard_user"
    And el carrito está vacío

  @exitoso
  Scenario: Agregar un producto al carrito
    When presiono "Add to cart" en el producto "Sauce Labs Backpack"
    Then el botón del producto debería cambiar a "Remove"
    And el ícono del carrito debería mostrar "1"
    And el producto debería aparecer al abrir el carrito

  @exitoso
  Scenario: Eliminar un producto del carrito
    Given que agregué el producto "Sauce Labs Backpack" al carrito
    When abro el carrito
    And presiono "Remove" en ese producto
    Then el carrito debería quedar vacío
    And el ícono del carrito no debería mostrar ningún número

  @exitoso
  Scenario: Agregar varios productos
    When agrego los productos "Sauce Labs Backpack" y "Sauce Labs Bike Light"
    Then el ícono del carrito debería mostrar "2"
```

## 5. Feature: Checkout

```gherkin
@checkout
Feature: Checkout de la compra
  Como usuario autenticado con productos en el carrito
  Quiero completar mis datos de envío y finalizar la compra
  Para confirmar mi pedido

  Background:
    Given que inicié sesión con el usuario "standard_user"
    And tengo el producto "Sauce Labs Backpack" en el carrito
    And estoy en el formulario de checkout

  @exitoso
  Scenario: Compra completa exitosa
    When ingreso First Name "Kleber", Last Name "Ballon" y Postal Code "0000"
    And presiono "Continue"
    And reviso el resumen del pedido
    And presiono "Finish"
    Then debería ver el mensaje "Thank you for your order!"
    And el carrito debería quedar vacío

  @error
  Scenario: Campo obligatorio vacío
    When dejo el campo "First Name" vacío
    And completo "Last Name" y "Postal Code"
    And presiono "Continue"
    Then debería ver el mensaje "Error: First Name is required"
    And no debería avanzar al resumen del pedido

  @error
  Scenario Outline: Cada campo del formulario es obligatorio
    When ingreso First Name "<nombre>", Last Name "<apellido>" y Postal Code "<codigo>"
    And presiono "Continue"
    Then debería ver el mensaje "<mensaje>"

    Examples:
      | nombre | apellido | codigo | mensaje                        |
      |        | Ballon   | 0000   | Error: First Name is required  |
      | Kleber |          | 0000   | Error: Last Name is required   |
      | Kleber | Ballon   |        | Error: Postal Code is required |
```

---

## 6. Relación con los casos de caja negra

| Escenario Gherkin | Caso de caja negra |
|---|---|
| Acceso exitoso | CN-01 |
| Usuario bloqueado | CN-02 |
| Agregar un producto al carrito | CN-03 |
| Campo obligatorio vacío | CN-04 |
| Compra completa exitosa | CN-05 |

Los escenarios adicionales (contraseña incorrecta, eliminar producto, campos obligatorios del login y del checkout) describen comportamientos que pueden convertirse en casos de prueba futuros.

## 7. Resumen

| Feature | Escenarios | Exitosos | Alternativos / de error |
|---|---|---|---|
| Inicio de sesión | 4 | 1 | 3 |
| Carrito de compras | 3 | 3 | 0 |
| Checkout | 3 | 1 | 2 |
| **Total** | **10** | **5** | **5** |
