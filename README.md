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
<img width="1917" height="1077" alt="1 ronda 1" src="https://github.com/user-attachments/assets/63d472ff-ad23-4cb1-9bdc-b9fd7adedda9" />
<img width="927" height="65" alt="2 ronda 1" src="https://github.com/user-attachments/assets/98849ef1-360a-4404-a2d7-daa77768c984" />

### Ronda 2. Rechazar opciones que no existen

**Prueba:** `votar por una opcion que no existe regresa opcionInvalida`

Se intenta votar por un id que no está en la votación. El servicio debe regresar `ResultadoVoto.opcionInvalida` en lugar de tronar.
<img width="1542" height="260" alt="1 ronda 2" src="https://github.com/user-attachments/assets/8320831e-ca87-46c0-8849-2edf968ce93a" />
<img width="1550" height="65" alt="2 ronda 2" src="https://github.com/user-attachments/assets/0557753f-9868-459b-973d-3eec082c5a78" />


### Ronda 3. Un usuario no puede votar dos veces

**Prueba:** `un mismo usuario no puede votar dos veces`

El mismo usuario vota dos veces. El segundo intento debe regresar `ResultadoVoto.usuarioYaVoto` y la segunda opción no debe sumar ningún voto.

<img width="1542" height="317" alt="1 ronda 3" src="https://github.com/user-attachments/assets/41060945-a30c-40ff-988c-616179bdb585" />
<img width="925" height="66" alt="2 ronda 3" src="https://github.com/user-attachments/assets/7920763d-d748-4d21-9a74-c0e35b4ed836" />


### Ronda 4. Calcular resultados con porcentajes

**Prueba 1:** `calcula el porcentaje de cada opcion correctamente`

Con 3 votos para una opción y 1 para la otra, los porcentajes deben ser 75% y 25%.
<img width="1555" height="952" alt="1 ronda 4" src="https://github.com/user-attachments/assets/6eedd245-6fda-491c-9ede-e64f4d0e94ea" />

**Prueba 2:** `si no hay ningun voto, todos los porcentajes son 0`

Sin votos registrados, todos los porcentajes deben ser 0. Esta prueba evita una división entre cero que produciría `NaN`.
<img width="892" height="77" alt="2 ronda 4" src="https://github.com/user-attachments/assets/a249b6a6-1c30-4558-8313-7a7264cdc6b8" />


### Ronda 5. Determinar el ganador

**Prueba:** `determinarGanador regresa la opcion con mas votos`

Con 2 votos contra 1, `determinarGanador()` debe regresar una sola opción: la que tiene más votos.

<img width="1536" height="507" alt="1 ronda 5" src="https://github.com/user-attachments/assets/c9e75706-18fd-4990-a7f2-86051b25b1bc" />
<img width="1047" height="85" alt="2 ronda 5" src="https://github.com/user-attachments/assets/d61242f0-e71a-4958-a00d-14ab4b096565" />


### Ronda 6. Reconocer un empate

**Prueba:** `si hay empate, determinarGanador regresa mas de una opcion`

Tres opciones reciben un voto cada una. `determinarGanador()` debe regresar las tres. Esta prueba pasó sin escribir código nuevo, porque la ronda 5 ya regresaba todas las opciones con el máximo de votos; se conserva para proteger ese comportamiento.

<img width="912" height="77" alt="ronda 6" src="https://github.com/user-attachments/assets/fcab2352-b9d2-407b-8f37-fb5b0164e510" />


### Ronda 7. No se puede votar después del cierre

**Prueba 1:** `no se puede votar si la votacion ya cerro`

Con una fecha de cierre en el pasado, el voto debe regresar `ResultadoVoto.votacionCerrada` y el contador debe seguir en 0.
<img width="1537" height="320" alt="1 ronda 7" src="https://github.com/user-attachments/assets/5b1b35bc-3507-4c29-a4dd-5ce59b7817ff" />

**Prueba 2:** `si la votacion sigue abierta, el voto se registra normalmente`

Con una fecha de cierre futura, el voto debe regresar `ResultadoVoto.exitoso`.

<img width="1092" height="96" alt="2 ronda 7" src="https://github.com/user-attachments/assets/0bd4e73c-543d-478e-8a0d-42dc37f23ce3" />


### Ronda 8. Refactor

Sin pruebas nuevas. Se dieron nombres descriptivos a las condiciones de `registrarVoto()` (`yaCerro`, `yaVoto`) para que se lea como una lista de reglas. La evidencia es que todas las pruebas anteriores siguen en verde.

<img width="892" height="65" alt="ronda 9" src="https://github.com/user-attachments/assets/13148751-c92f-4474-bf55-dc31d171ee1a" />

### Prueba de integración. Un plebiscito completo

**Prueba:** `simulacion completa: varios vecinos votan y se determina un ganador`

Tres vecinos votan entre tres obras reales y uno de ellos intenta votar otra vez. Se verifica que el total sea de 3 votos (el repetido no cuenta) y que gane la rehabilitación del Jardín Principal.

<img width="797" height="52" alt="paso 4" src="https://github.com/user-attachments/assets/938766b3-4609-44d1-a8a6-31d46cf7cfa1" />


## Resumen de pruebas

| # | Ronda | Prueba | 
|---|-------|--------|
| 1 | 1 | Voto válido incrementa el contador | 
| 2 | 2 | Opción inexistente regresa `opcionInvalida` | 
| 3 | 3 | Un usuario no puede votar dos veces | 
| 4 | 4 | Porcentajes correctos | 
| 5 | 4 | Sin votos, todos los porcentajes son 0 | 
| 6 | 5 | El ganador es la opción con más votos | 
| 7 | 6 | Empate regresa más de una opción | 
| 8 | 7 | No se puede votar tras el cierre | 
| 9 | 7 | Votación abierta registra el voto | 
| 10 | Integración | Simulación completa del plebiscito | 

**Resultado final de `flutter test`:**
<img width="1012" height="137" alt="paso 5" src="https://github.com/user-attachments/assets/aa4bee56-34d7-4fa1-a45f-39f7b9f9ebd9" />


## Evidencia de la app

**Pantalla de votación con barras animadas:**

<img width="1917" height="1077" alt="final" src="https://github.com/user-attachments/assets/6f147149-6908-4cb4-9cb4-d09d6ee0d2fd" />
