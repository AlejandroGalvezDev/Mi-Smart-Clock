# Xiaomi Mi Smart Clock (X04G)

Investigación técnica y documentación comunitaria sobre el desbloqueo del Xiaomi Mi Smart Clock, las ROM alternativas y las posibilidades de sustituir Google Assistant.

## Estado del proyecto

- **Modelo de referencia:** Xiaomi Mi Smart Clock global, normalmente identificado como **X04G**.
- **Objetivo:** documentar opciones reales para cambiar el firmware y utilizar otro asistente o convertir el reloj en un panel de Home Assistant.
- **Conclusión provisional:** no se ha identificado un método fiable para reemplazar únicamente Google Assistant conservando el firmware original. La vía comunitaria consiste en sustituir el sistema completo, normalmente por una ROM basada en LineageOS, y ejecutar una aplicación de asistente/panel.
- **Advertencia principal:** hay incidencias comunitarias documentadas en la captura de audio/micrófono en algunas compilaciones Android 13 para este dispositivo. No debe darse por garantizado el funcionamiento manos libres.

## Documentos

- [Investigación de firmware y asistentes](docs/INVESTIGACION-FIRMWARE-ASISTENTES.md): opciones, hardware, procedimiento general, riesgos, incidencias y fuentes.
- [Notas del documento de partida](docs/NOTAS-DOCUMENTO-ORIGINAL.md): resumen de las hipótesis y propuestas incluidas en el archivo inicial aportado para el proyecto, separando lo confirmado de lo que requiere pruebas.

## Fuentes comunitarias principales

- [octathorp/x04g_tools](https://github.com/octathorp/x04g_tools)
- [Guía Xiaomi Smart Clock para View Assist](https://dinki.github.io/View-Assist/docs/supported-devices/android-devices/xiaomi-smart-clock)
- [AyraHikari/xiaomi-x04g-lx04](https://github.com/AyraHikari/xiaomi-x04g-lx04)
- [Hilo de Home Assistant: Hack Mi Smart Clock to display Lovelace UI](https://community.home-assistant.io/t/hack-mi-smart-clock-to-display-lovelace-ui/667007)
- [Incidencia de audio HAL y captura de micrófono](https://github.com/octathorp/x04g_tools/issues/8)
- [Documentación oficial de Google sobre el cambio de Assistant a Gemini para Home](https://support.google.com/googlehome/answer/18957422?hl=es)

## Criterio de seguridad

No flashear ninguna imagen hasta identificar con certeza la variante (X04G o LX04), obtener y verificar una copia completa de las particiones originales y comprobar que los archivos corresponden exactamente al dispositivo. Los procedimientos comunitarios pueden dejar el reloj inutilizable y no están respaldados por Xiaomi.
