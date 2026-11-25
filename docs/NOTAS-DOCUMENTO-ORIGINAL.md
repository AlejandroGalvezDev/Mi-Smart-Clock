# Notas del documento de partida

Este documento resume el archivo inicial aportado al proyecto ("Xiaomi Smart Clock.md"). Se conserva aquí su contenido temático, pero se distinguen las propuestas del documento de partida de los resultados contrastados en la investigación posterior.

## Contenido que planteaba el documento

- Cambiar Google Assistant no parece posible desde los ajustes del firmware original.
- La modificación propuesta consiste en desbloquear el dispositivo MediaTek mediante MTKClient, hacer un backup completo e instalar Android/LineageOS o una GSI ligera.
- Después se propone instalar Home Assistant/View Assist, VACA, Fully Kiosk o, de forma más especulativa, Alexa.
- Se destaca que el hardware es limitado (aproximadamente 1 GB RAM y almacenamiento reducido).
- Se enumeran riesgos de brick, pérdida de garantía, problemas de audio, Wi-Fi y arranque.
- Se mencionan las variantes X04G (global) y LX04 (China).
- Se plantea optimizar Android mediante ADB y configurar arranque automático de una app.

## Matices y correcciones importantes

1. **Sistema original:** el documento de partida contiene dos caracterizaciones distintas: en un lugar habla de CastOS/Android Things cerrado y en otro de Android Go 10. La documentación comunitaria consultada para X04G describe Android Go 10. No se debe repetir la afirmación de CastOS/Android Things como hecho sin identificar la versión exacta del firmware de la unidad.
2. **Chipset:** aparecen las denominaciones MT8167 y MT8167S. Confirmar el identificador de la placa/unidad antes de usar imágenes o comandos.
3. **Alexa:** los comandos de instalación de APK, permisos, rol de asistente y autoarranque son una propuesta técnica, no una demostración de que Alexa funcione con escucha manos libres estable en el X04G. Mantenerla como experimental.
4. **Micrófono:** la instalación de una aplicación no garantiza la captura de audio. La incidencia abierta #8 en x04g_tools informa de un crash del HAL al cerrar la captura en una compilación LineageOS 20/Android 13 GSI concreta.
5. **GApps/Google Assistant:** no hay evidencia sólida de que instalar MindTheGapps recupere Google Assistant en este dispositivo tras LineageOS.
6. **Gemini para Home:** el anuncio general de Google no confirma que Xiaomi vaya a actualizar específicamente el X04G.
7. **Comandos de optimización:** cambios como desactivar servicios, forzar orientación, mantener pantalla encendida o alterar procesos en caché dependen de la ROM. No ejecutarlos como receta universal; pueden afectar funciones y estabilidad.
8. **SELinux permissive:** la documentación GSI describe esta adaptación por limitaciones del vendor, pero reduce la seguridad. No debe ocultarse ni recomendarse sin explicar el riesgo.

## Enlaces técnicos del documento inicial y la investigación

- [octathorp/x04g_tools](https://github.com/octathorp/x04g_tools)
- [AyraHikari/xiaomi-x04g-lx04](https://github.com/AyraHikari/xiaomi-x04g-lx04)
- [View Assist — Xiaomi Smart Clock](https://dinki.github.io/View-Assist/docs/supported-devices/android-devices/xiaomi-smart-clock)
- [Home Assistant Community](https://community.home-assistant.io/t/hack-mi-smart-clock-to-display-lovelace-ui/667007)
- [x04g_tools — incidencia de audio #8](https://github.com/octathorp/x04g_tools/issues/8)
- [Google Home Help](https://support.google.com/googlehome/answer/18957422?hl=es)

Para el análisis completo y la evaluación de cada opción, consultar [INVESTIGACION-FIRMWARE-ASISTENTES.md](INVESTIGACION-FIRMWARE-ASISTENTES.md).
