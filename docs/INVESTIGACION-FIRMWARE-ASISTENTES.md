# Investigación: firmware alternativo y sustitución de Google Assistant

**Dispositivo de referencia:** Xiaomi Mi Smart Clock global, modelo X04G  
**Última consolidación:** 25 de noviembre de 2026  
**Estado:** investigación documental; no implica que se haya probado el procedimiento en un dispositivo físico.

## 1. Resumen ejecutivo

No se ha encontrado un método documentado y fiable para sustituir únicamente Google Assistant manteniendo intacto el firmware original del Mi Smart Clock. Las soluciones comunitarias cambian el sistema operativo completo y después instalan una aplicación o cliente alternativo.

La ruta más desarrollada para el modelo global X04G es:

1. Identificar la variante exacta y realizar una copia de seguridad completa.
2. Utilizar herramientas MediaTek/MTKClient para acceder al dispositivo y desbloquear el bootloader.
3. Instalar una compilación Android/LineageOS compatible con el diseño de particiones del X04G.
4. Instalar por ADB un cliente como View Assist Companion App (VACA), junto con Home Assistant, o configurar el reloj como panel.

**Limitación decisiva:** existe una incidencia abierta en el repositorio de herramientas X04G que describe un fallo del HAL de audio al cerrar la captura del micrófono en una compilación LineageOS 20/Android 13 GSI. Si el fallo afecta a la compilación elegida, puede impedir el uso fiable de escucha continua o activación por palabra clave. Hay que comprobar el estado de esa incidencia y las pruebas de la ROM concreta antes de considerar terminado el proyecto.

No hay evidencia suficiente para recomendar Alexa como sustituto manos libres plenamente funcional. Tampoco se ha encontrado una instalación de Google Assistant/GApps que funcione de forma fiable tras sustituir el sistema.

## 2. Identificación del dispositivo y hardware

La documentación comunitaria de X04G describe, en términos generales:

- SoC MediaTek MT8167 (en algunas notas aparece MT8167S; verificar la identificación exacta de la placa).
- CPU de cuatro núcleos Cortex-A35 alrededor de 1,5 GHz.
- 1 GB de RAM.
- Pantalla de 4 pulgadas, 800 × 480.
- Wi-Fi de 2,4 GHz y Bluetooth 5.0.
- Puerto micro-USB para alimentación y datos.
- Dos micrófonos y altavoz mono.
- Sensores de luz/proximidad.

El hardware es limitado. Una ROM completa y los servicios de Google pueden consumir demasiados recursos; las imágenes ligeras y una aplicación principal dedicada son más razonables que intentar convertirlo en una tableta Android convencional.

### Variantes: X04G y LX04

- **X04G:** variante global/internacional, objeto principal de esta investigación.
- **LX04:** variante china, asociada a firmware Xiao AI y a un diseño/particionado que puede diferir.

Aunque algunos repositorios indican que comparten gran parte del hardware, no se deben intercambiar imágenes ni seguir instrucciones de conversión sin comprobar el modelo, la tabla de particiones y los archivos exactos. La guía de View Assist indica que su imagen LineageOS espera el diseño de particiones X04G y que LX04 requiere conversión previa. Esta conversión es un procedimiento distinto y de mayor riesgo.

## 3. Opciones de modificación

| Opción | Qué aporta | Evaluación |
|---|---|---|
| Mantener firmware original | Conserva la integración de fábrica y las funciones actuales de Google | La opción de menor riesgo; no permite sustituir libremente Assistant |
| Android original + aplicaciones adicionales | En teoría permite instalar aplicaciones si se habilita ADB, pero no sustituye el asistente de sistema ni garantiza ejecución permanente | No se ha encontrado un método probado de sustitución |
| LineageOS/Android ligero + VACA/View Assist | Convierte el reloj en cliente de voz/panel conectado a Home Assistant | Ruta comunitaria más documentada; audio/micrófono debe probarse |
| LineageOS + Fully Kiosk + Home Assistant | Uso como panel de domótica en pantalla | Más adecuado si la prioridad es mostrar dashboards y no la escucha continua |
| LineageOS + Alexa | Instalación de APK y configuración de permisos/arranque | Experimental; no hay confirmación sólida de palabra de activación y funcionamiento estable con 1 GB RAM |
| GSI Android genérica | Permite experimentar con otras versiones Android | Requiere adaptación de imágenes y puede exigir cambios de seguridad; riesgo alto |
| Firmware Xiao AI/LX04 | Ecosistema chino de la variante LX04 | No recomendable para un X04G europeo como solución de asistente en español |
| Linux/postmarketOS | Investigación experimental de arranque Linux | No se ha encontrado una solución completa y mantenida con pantalla, Wi-Fi, audio y micrófono plenamente funcionales |

### 3.1. LineageOS y View Assist

La guía de View Assist para Xiaomi Smart Clock propone instalar LineageOS en el X04G y utilizar **View Assist Companion App (VACA)**. El reloj actúa como dispositivo cliente conectado a Home Assistant. La documentación contempla la copia de seguridad, desbloqueo, flasheo de las imágenes y posterior instalación de la aplicación por ADB.

Referencias:
- [Guía de View Assist para Xiaomi Smart Clock](https://dinki.github.io/View-Assist/docs/supported-devices/android-devices/xiaomi-smart-clock)
- [Repositorio x04g_tools](https://github.com/octathorp/x04g_tools)
- [Hilo de Home Assistant sobre el Mi Smart Clock](https://community.home-assistant.io/t/hack-mi-smart-clock-to-display-lovelace-ui/667007)

La documentación comunitaria también menciona HassMic y el uso de Fully Kiosk con Home Assistant. Deben tratarse como casos de uso distintos: cliente de voz frente a panel visual.

### 3.2. GSI y adaptación de particiones

Android 10 y Treble permiten explorar imágenes GSI, pero en este dispositivo no basta con descargar una imagen y flashearla como si fuera un móvil estándar. La guía de x04g_tools explica que las particiones lógicas system/product/vendor se alojan dentro de `super`, por lo que hay que adaptar/reempaquetar la imagen según el diseño del dispositivo.

La guía también describe restricciones de SELinux en el vendor original y un método de arranque con SELinux permissive. Esto reduce la seguridad del sistema y no debe presentarse como una configuración recomendable para un dispositivo expuesto a redes o servicios sensibles.

Referencia: [Working with GSI — x04g_tools](https://github.com/octathorp/x04g_tools/blob/master/howtos/WorkingWithGSI.md).

## 4. Procedimiento general documentado

Este apartado es un mapa del proceso, no una instrucción de flasheo lista para ejecutar. Antes de seguirlo hay que leer las guías originales y verificar que las imágenes siguen disponibles y corresponden al X04G concreto.

1. **Confirmar variante y conexión.** Identificar X04G/LX04 y disponer de un cable micro-USB que admita datos.
2. **Preparar el entorno.** Las guías usan MTKClient; Linux suele ser una opción habitual para evitar problemas de controladores.
3. **Entrar en modo BROM/Preloader.** Las combinaciones de botones varían según guía y estado del dispositivo. Confirmar la enumeración MediaTek en el ordenador.
4. **Copia de seguridad integral.** Guardar las particiones originales, especialmente las específicas del dispositivo. Conservar más de una copia y comprobar que los ficheros no están vacíos.
5. **Desbloqueo.** Las guías de X04G citan MTKClient y el comando `python mtk.py da seccfg unlock`. No ejecutarlo sin haber seguido la versión actual de la documentación y realizado backup.
6. **Flasheo.** Las instrucciones comunitarias citan imágenes como `super.img`, `vbmeta` y `boot.img`. El orden y los parámetros dependen de la ROM y su guía exacta.
7. **Primer arranque.** Puede tardar varios minutos. Utilizar alimentación estable, no depender del puerto USB del ordenador como única fuente de alimentación.
8. **Configuración Android.** Habilitar ADB y configurar la pantalla y el sistema para el uso previsto.
9. **Instalar cliente.** Instalar VACA u otra aplicación compatible por ADB y vincularla con Home Assistant.
10. **Validar cada periférico.** Probar Wi-Fi, salida de audio, captura de micrófono, reposo, reanudación, reinicio y arranque automático. No asumir que porque Android arranque todos los periféricos funcionan.

Repositorio de herramientas: [octathorp/x04g_tools](https://github.com/octathorp/x04g_tools).

Guía de conversión/root para variantes: [AyraHikari/xiaomi-x04g-lx04](https://github.com/AyraHikari/xiaomi-x04g-lx04). Sus instrucciones de conversión LX04/X04G no deben aplicarse indistintamente.

## 5. Asistentes alternativos y viabilidad

### Home Assistant Assist / View Assist / VACA

Es la alternativa con mejor soporte documental para este dispositivo. Requiere un servidor Home Assistant y una aplicación cliente compatible en el reloj. Permite construir un asistente conectado al ecosistema de domótica; las capacidades concretas de voz dependen de los servicios de voz configurados en Home Assistant.

**Punto crítico:** comprobar micrófono y audio en la ROM elegida. La existencia de una app instalada no demuestra que la captura de voz continua funcione.

### Fully Kiosk + Home Assistant

Adecuado para un panel visual de control. No equivale por sí mismo a un asistente de voz ni sustituye a Google Assistant.

### Amazon Alexa

Se han propuesto procedimientos basados en instalar el APK, conceder permisos de micrófono, seleccionar la aplicación como asistente y usar una utilidad de autoarranque. Sin embargo, estas instrucciones no demuestran que la app actual sea compatible con esa ROM, que la activación manos libres funcione en segundo plano ni que sobreviva a las restricciones de memoria y energía.

Por tanto, Alexa debe considerarse **experimental/no verificada**, no una solución garantizada. La pérdida de procesamiento acústico original y las limitaciones de RAM pueden afectar a la experiencia.

### Google Assistant y GApps

No se ha encontrado una configuración confirmada y fiable de Google Assistant tras instalar LineageOS en este reloj. En un caso comunitario, la instalación de MindTheGapps no hizo aparecer las aplicaciones de Google después del reinicio. No recomendar instalar paquetes GApps sin comprobar compatibilidad exacta de arquitectura, particiones y versión Android.

### Otros asistentes o modelos de IA

Un cliente Android o un panel web podría conectarse a servicios de terceros, pero eso no significa que exista integración de micrófono manos libres. El uso de un modelo de IA remoto requeriría una aplicación/cliente compatible y un servicio backend; no se ha identificado una imagen lista para instalar que lo resuelva todo.

## 6. Incidencias y riesgos conocidos

### Micrófono / HAL de audio

La incidencia [#8 de x04g_tools](https://github.com/octathorp/x04g_tools/issues/8) describe un fallo SIGSEGV del HAL de audio MediaTek al detener la captura del micrófono. El informe se reproduce en una configuración LineageOS 20/Android 13 GSI concreta y genera grabaciones vacías o truncadas. El patrón puede afectar a aplicaciones que abren y cierran repetidamente el micrófono, como las que implementan detección de palabra clave.

Es una incidencia abierta en la información consultada; no debe generalizarse automáticamente a todas las ROM, pero sí obliga a probar el audio antes de elegir la solución.

### Otras incidencias relatadas por la comunidad

- Algunos usuarios reportan problemas de Wi-Fi y necesidad de usar WPA2 en lugar de modos mixtos WPA2/WPA3.
- Hay relatos de sesiones fastboot inestables durante el flasheo.
- Alimentar el reloj desde un puerto USB de ordenador puede provocar reinicios; se recomienda una fuente estable adecuada.
- El primer arranque puede tardar varios minutos.
- Se han descrito limitaciones para remapear el botón de silencio.
- Algunos usuarios reportan que el Wi-Fi puede desconectarse o quedar apagado tras cierto tiempo.
- Algunos archivos antiguos de las guías pueden desaparecer; conviene obtener y verificar los ficheros antes de empezar.

Estos puntos son experiencias comunitarias, no una lista de fallos que necesariamente sufrirá cada unidad.

### Riesgos generales

- Pérdida total de arranque (*brick*) por imagen incorrecta o interrupción del flasheo.
- Pérdida de datos y de la configuración original.
- Pérdida de garantía o soporte del fabricante.
- Menor seguridad si se desactiva SELinux o se instala software sin mantenimiento.
- Rendimiento limitado por 1 GB de RAM y almacenamiento reducido.
- Funciones de micrófono, audio, sensores y Wi-Fi incompletas.

## 7. Google Assistant y Gemini para Home

Google ha anunciado la transición de Google Assistant a Gemini para Home en dispositivos domésticos compatibles, con retirada de Assistant prevista para 2027. La documentación oficial menciona también dispositivos de terceros compatibles, pero en la investigación no se ha encontrado confirmación específica de que Xiaomi vaya a proporcionar una actualización para el modelo X04G.

Fuente oficial: [Ayuda de Google Home (español)](https://support.google.com/googlehome/answer/18957422?hl=es).

No se debe afirmar que el Mi Smart Clock X04G vaya a recibir Gemini para Home ni que vaya a dejar de funcionar en una fecha concreta sin un anuncio específico de Xiaomi/Google para ese modelo.

## 8. Música y Spotify

La documentación comercial del firmware original ha asociado el reloj con reproducción de Spotify/YouTube Music y Chromecast, normalmente mediante envío desde el móvil. No se ha confirmado una experiencia equivalente en las ROM alternativas.

En particular, no se ha verificado Spotify funcionando de forma nativa en LineageOS/VACA en el X04G. Debe probarse por separado la salida de audio, el método de reproducción y el control remoto antes de considerar que la migración conserva esa función.

## 9. Linux / postmarketOS

Existe trabajo experimental de arranque Linux en este hardware, por ejemplo [esta investigación](https://gist.github.com/lscorcia/58c575a5d11657e17abca21950749c59). No se ha localizado una distribución lista para uso diario que documente de forma completa pantalla, Wi-Fi, audio y micrófono en el X04G. No es la primera opción para un asistente doméstico funcional.

## 10. Recomendación provisional

- Si la prioridad es **conservar las funciones originales y minimizar riesgos**, mantener el firmware oficial hasta que haya una ROM probada con audio y micrófono estables.
- Si la prioridad es **domótica y voz con Home Assistant**, estudiar LineageOS + VACA/View Assist, pero no flashear hasta verificar el estado de la incidencia de audio y probar una compilación concreta.
- Si la prioridad es **panel visual**, LineageOS + Fully Kiosk/Home Assistant puede ser más realista que un asistente siempre a la escucha.
- No recomendar Alexa como resultado garantizado ni prometer que se puede recuperar Google Assistant con GApps.
- No convertir LX04 a X04G ni viceversa sin una guía específica para la variante y backup verificado.

## 11. Fuentes y repositorios

1. [octathorp/x04g_tools](https://github.com/octathorp/x04g_tools) — herramientas, copias, imágenes y documentación X04G.
2. [Working with GSI](https://github.com/octathorp/x04g_tools/blob/master/howtos/WorkingWithGSI.md) — adaptación de imágenes GSI y limitaciones.
3. [View Assist: Xiaomi Smart Clock](https://dinki.github.io/View-Assist/docs/supported-devices/android-devices/xiaomi-smart-clock) — guía para LineageOS y VACA.
4. [View Assist source documentation](https://github.com/dinki/View-Assist/blob/main/wiki/docs/supported-devices/android-devices/xiaomi-smart-clock.md).
5. [AyraHikari/xiaomi-x04g-lx04](https://github.com/AyraHikari/xiaomi-x04g-lx04) — backup, variantes, conversión y root.
6. [Home Assistant Community thread](https://community.home-assistant.io/t/hack-mi-smart-clock-to-display-lovelace-ui/667007) — experiencias de instalación y uso.
7. [x04g_tools issue #8](https://github.com/octathorp/x04g_tools/issues/8) — fallo de HAL de audio/micrófono.
8. [Investigación experimental Linux](https://gist.github.com/lscorcia/58c575a5d11657e17abca21950749c59).
9. [Google Home Help](https://support.google.com/googlehome/answer/18957422?hl=es) — transición Assistant/Gemini para Home.

## 12. Próximas tareas de investigación

- [ ] Confirmar el identificador de hardware y la variante del reloj físico.
- [ ] Comprobar la última actividad y resolución de la incidencia #8 de audio.
- [ ] Identificar una compilación exacta de LineageOS/GSI y su estado de mantenimiento.
- [ ] Verificar enlaces y hashes de todos los archivos necesarios antes de descargarlos.
- [ ] Confirmar si VACA permite una experiencia de voz utilizable con el HAL disponible.
- [ ] Probar Wi-Fi, reproducción de audio, micrófono, reinicio y arranque automático.
- [ ] Investigar compatibilidad real de Spotify/Chromecast y no asumir que se conserva.
- [ ] Vigilar anuncios oficiales de Xiaomi/Google sobre Gemini para Home en X04G.
