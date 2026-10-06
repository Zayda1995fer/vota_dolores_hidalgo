# Vota Dolores Hidalgo

Plebiscito vecinal digital hecho en Flutter y construido con TDD (Desarrollo Guiado por Pruebas). Los vecinos votan por la obra prioritaria del municipio, los resultados se muestran en barras animadas y al final se revela el ganador con una animación.

**Materia:** Desarrollo Móvil Integral
**Alumno(a):** Zayda Fernanda Vargas
**Fecha:** 06/10/2026

## Reglas de negocio protegidas con pruebas

- Cada persona puede votar una sola vez.
- Solo se puede votar por una opción que exista.
- No se puede votar después de la fecha de cierre.
- Los porcentajes son correctos, incluso si nadie ha votado.
- Un empate en primer lugar se reconoce como empate, no se elige al azar.

## Estructura del proyecto

```
lib/
  modelos/
    opcion_votacion.dart
    votacion.dart
    resultado_opcion.dart
  logica/
    resultado_voto.dart
    servicio_votacion.dart
  presentation/
    votacion_screen.dart
  main.dart
test/
  servicio_votacion_test.dart
```

## Cómo ejecutar

```bash
flutter test   # corre las 10 pruebas
flutter run    # abre la app
```

## Pruebas

Todas están en `test/servicio_votacion_test.dart`. Las capturas de evidencia se guardan en la carpeta `evidencias/`.

### Ronda 1. Registrar un voto válido

**Prueba:** `registrar un voto valido incrementa el contador de esa opcion`

Un usuario vota por una opción existente. Se verifica que el servicio regrese `ResultadoVoto.exitoso` y que el contador de esa opción suba a 1.

![Evidencia ronda 1](evidencias/ronda1.png)

### Ronda 2. Rechazar opciones que no existen

**Prueba:** `votar por una opcion que no existe regresa opcionInvalida`

Se intenta votar por un id que no está en la votación. El servicio debe regresar `ResultadoVoto.opcionInvalida` en lugar de tronar.

![Evidencia ronda 2](evidencias/ronda2.png)

### Ronda 3. Un usuario no puede votar dos veces

**Prueba:** `un mismo usuario no puede votar dos veces`

El mismo usuario vota dos veces. El segundo intento debe regresar `ResultadoVoto.usuarioYaVoto` y la segunda opción no debe sumar ningún voto.

![Evidencia ronda 3](evidencias/ronda3.png)

### Ronda 4. Calcular resultados con porcentajes

**Prueba 1:** `calcula el porcentaje de cada opcion correctamente`

Con 3 votos para una opción y 1 para la otra, los porcentajes deben ser 75% y 25%.

**Prueba 2:** `si no hay ningun voto, todos los porcentajes son 0`

Sin votos registrados, todos los porcentajes deben ser 0. Esta prueba evita una división entre cero que produciría `NaN`.

![Evidencia ronda 4](evidencias/ronda4.png)

### Ronda 5. Determinar el ganador

**Prueba:** `determinarGanador regresa la opcion con mas votos`

Con 2 votos contra 1, `determinarGanador()` debe regresar una sola opción: la que tiene más votos.

![Evidencia ronda 5](evidencias/ronda5.png)

### Ronda 6. Reconocer un empate

**Prueba:** `si hay empate, determinarGanador regresa mas de una opcion`

Tres opciones reciben un voto cada una. `determinarGanador()` debe regresar las tres. Esta prueba pasó sin escribir código nuevo, porque la ronda 5 ya regresaba todas las opciones con el máximo de votos; se conserva para proteger ese comportamiento.

![Evidencia ronda 6](evidencias/ronda6.png)

### Ronda 7. No se puede votar después del cierre

**Prueba 1:** `no se puede votar si la votacion ya cerro`

Con una fecha de cierre en el pasado, el voto debe regresar `ResultadoVoto.votacionCerrada` y el contador debe seguir en 0.

**Prueba 2:** `si la votacion sigue abierta, el voto se registra normalmente`

Con una fecha de cierre futura, el voto debe regresar `ResultadoVoto.exitoso`.

![Evidencia ronda 7](evidencias/ronda7.png)

### Ronda 8. Refactor

Sin pruebas nuevas. Se dieron nombres descriptivos a las condiciones de `registrarVoto()` (`yaCerro`, `yaVoto`) para que se lea como una lista de reglas. La evidencia es que todas las pruebas anteriores siguen en verde.

![Evidencia ronda 8](evidencias/ronda8.png)

### Prueba de integración. Un plebiscito completo

**Prueba:** `simulacion completa: varios vecinos votan y se determina un ganador`

Tres vecinos votan entre tres obras reales y uno de ellos intenta votar otra vez. Se verifica que el total sea de 3 votos (el repetido no cuenta) y que gane la rehabilitación del Jardín Principal.

![Evidencia integración](evidencias/integracion.png)

## Resumen de pruebas

| # | Ronda | Prueba | Estado |
|---|-------|--------|--------|
| 1 | 1 | Voto válido incrementa el contador | ☐ |
| 2 | 2 | Opción inexistente regresa `opcionInvalida` | ☐ |
| 3 | 3 | Un usuario no puede votar dos veces | ☐ |
| 4 | 4 | Porcentajes correctos | ☐ |
| 5 | 4 | Sin votos, todos los porcentajes son 0 | ☐ |
| 6 | 5 | El ganador es la opción con más votos | ☐ |
| 7 | 6 | Empate regresa más de una opción | ☐ |
| 8 | 7 | No se puede votar tras el cierre | ☐ |
| 9 | 7 | Votación abierta registra el voto | ☐ |
| 10 | Integración | Simulación completa del plebiscito | ☐ |

**Resultado final de `flutter test`:**

![Todas las pruebas en verde](evidencias/todas_las_pruebas.png)

## Evidencia de la app

**Pantalla de votación con barras animadas:**

![Pantalla de votación](evidencias/app_votacion.png)

**Diálogo del ganador:**

![Diálogo del ganador](evidencias/app_ganador.png)