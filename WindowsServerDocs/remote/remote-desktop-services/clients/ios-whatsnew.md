---
title: Novedades del cliente de iOS
description: Obtén información sobre los cambios recientes en el cliente de Escritorio remoto para iOS
ms.topic: article
author: heidilohr
manager: lizross
ms.author: helohr
ms.date: 11/06/2020
ms.localizationpriority: medium
ms.openlocfilehash: beb97113165127c50c815ccca8a667a1654215fa
ms.sourcegitcommit: 5fc77b4325a18d8c22385d899b14fe724a662347
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/07/2020
ms.locfileid: "94361172"
---
# <a name="whats-new-in-the-ios-client"></a>Novedades del cliente de iOS

El [cliente de Escritorio remoto para iOS](remote-desktop-ios.md) se actualiza periódicamente, con lo que se agregan nuevas características y se corrigen problemas. En esta página encontrarás las actualizaciones más recientes.

## <a name="how-to-report-issues"></a>Notificación de problemas

Nos comprometemos a hacer que esta aplicación sea el mejor cliente posible de Escritorio remoto para iOS y, por ello, apreciamos tus comentarios. Puedes notificar cualquier problema en **Ayuda** > **Notificar un problema**.

## <a name="updates-for-version-1020"></a>Actualizaciones de la versión 10.2.0

*Fecha de publicación: 06/11/2020*

En esta versión, hemos abordado algunos problemas de compatibilidad con iOS y iPadOS 14. Además, hemos realizado las siguientes correcciones y actualizaciones de características:

- Se han solucionado los bloqueos en iOS y iPadOS 14 que se producían al escribir con el teclado.
- Se han agregado los accesos directos Cmd + S y Cmd + N para acceder a los procesos "Agregar área de trabajo" y "Agregar equipo", respectivamente.
- Se ha agregado el acceso directo Cmd + F para invocar la UI de búsqueda en el centro de conexiones.
- Se han agregado los comandos "Expandir todo" y "Contraer todo" a la pestaña Áreas de trabajo.
- Se resolvió un problema que causaba un error de protocolo 0xD06 al ejecutar Outlook como una aplicación remota.
- El teclado en pantalla ahora desaparecerá al desplazarse por los resultados de la búsqueda en el centro de conexiones.
- Se actualizó la animación usada al pasar el cursor por encima de los iconos del área de trabajo con un mouse o un trackpad en iPadOS 14.

## <a name="updates-for-version-1014"></a>Actualizaciones para la versión 10.1.4

*Fecha de publicación: 06/11/2020*

Agrupamos algunas correcciones de errores y actualizaciones de características menores para esta versión 10.1.4. Estas son las novedades:

- Se ha solucionado un problema en el que el cliente notificaba un mensaje de error 0x5000007 al intentar conectarse a un servidor de puerta de enlace de Escritorio remoto.
- Las contraseñas de cuentas de usuario actualizadas en la UI de credenciales se guardan después de iniciar sesión correctamente.
- Se solucionó un problema en que el intervalo y la selección múltiple con el mouse o el trackpad (Mayús + clic y Ctrl + clic) no funcionaban de forma coherente.
- Se ha solucionado un error en que las aplicaciones mostradas en la UI del conmutador en sesión no estaban sincronizadas con la sesión remota.
- Se realizaron algunos cambios estéticos en el diseño de los encabezados del área de trabajo del centro de conexiones.
- Se mejoró la visibilidad de los botones del teclado en pantalla para los fondos oscuros.
- Se corrigió un error de localización en el cuadro de diálogo de desconexión.

## <a name="updates-for-version-1013"></a>Actualizaciones para la versión 10.1.3

*Fecha de publicación: 06/11/2020*

Se agruparon algunas correcciones de errores y actualizaciones de características para esta versión 10.1.3. Estas son las novedades:

- El modo de entrada (puntero del mouse o modo táctil) ahora es global en todas las conexiones de aplicaciones remotas y de PC activas.
- Se corrigió un problema que impedía que el redireccionamiento del micrófono funcionara de manera uniforme.
- Se corrigió un error que causaba que la salida de audio se reprodujera desde el auricular del iPhone y no desde el altavoz interno.
- El cliente ahora admite el cambio automático de la salida de audio entre los altavoces internos del iPhone o el iPad, los altavoces Bluetooth y los Airpods.
- Ahora el audio sigue reproduciéndose en segundo plano cuando se separa del cliente o se bloquea el dispositivo.
- El modo de entrada cambia automáticamente al modo táctil cuando se usa un mouse SwiftPoint en dispositivos iPhone o iPad (que no ejecutan iPadOS, versión 13.4 o posterior).
- Se solucionaron los problemas de salida de gráficos que se producían cuando el servidor estaba configurado para usar el modo de pantalla completa AVC444.
- Se corrigieron algunos errores de VoiceOver.
- La panorámica en una sesión ampliada al usar un mouse externo o un trackpad ahora funciona de forma diferente. Para aplicar la panorámica en una sesión ampliada con un mouse externo o un trackpad, seleccione el mando de panorámica y, a continuación, arrastre el cursor del mouse mientras mantiene presionado el botón del mouse. Para aplicar la panorámica en el modo táctil, presione el mando de panorámica y mueva el dedo. La sesión se pegará al dedo y lo seguirá. En el modo de puntero del mouse, empuje el cursor del mouse virtual contra los lados de la pantalla.

## <a name="updates-for-version-1012"></a>Actualizaciones para la versión 10.1.2

*Fecha de publicación: 17/08/2020*

En esta actualización, se han solucionado problemas informados en la actualización de la versión 10.1.1.

- Se corrigió un bloqueo que se producía para algunos usuarios al suscribirse a una fuente de Windows Virtual Desktop mediante la autenticación no asíncrona.
- Se corrigió el diseño de los iconos del área de trabajo en iPhone X, iPhone XS y iPhone 11 Pro.

## <a name="updates-for-version-1011"></a>Actualizaciones para la versión 10.1.1

*Fecha de publicación: 06/11/2020*

Esto es lo que se incluye en esta versión:

- Se corrigió un error que impedía escribir en coreano.
- Se agregó la compatibilidad con las teclas F1 a F12, Inicio, Fin, Re Pág y Av Pág en los teclados de hardware.
- Se resolvió un error que dificultaba el desplazamiento del cursor del mouse a la parte superior de la pantalla en formato de pantalla ancha en dispositivos iPadOS.
- Se solucionó un problema en que, al presionar la tecla de retroceso después del espacio, se eliminaban dos caracteres.
- Se corrigió un error que hacía que el cursor del mouse de iPadOS apareciera encima del cursor del mouse del cliente de Escritorio remoto en el modo "Pulsar para hacer clic".
- Se resolvió un problema que impedía las conexiones a algunos servidores de puerta de enlace de Escritorio remoto (código de error 0x30000064).
- Se corrigió un error que hacía que el cursor del mouse se mostrara en la UI del conmutador en sesión en dispositivos iOS al usar un mouse SwiftPoint.
- Se cambió el tamaño del cursor del mouse del cliente de Escritorio remoto para ajustarlo al factor de escala del cliente actual.
- El cliente ahora comprueba la conectividad de red antes de iniciar un recurso de área de trabajo o una conexión de equipo.
- Al presionar el botón de escape reasignado o Cmd + . ahora se cancelan todas las peticiones de credenciales.
- Se agregaron algunas animaciones y el brillo que aparece al mover el cursor del mouse por los iPad que ejecutan iPadOS 13.4 o posterior.

## <a name="updates-for-version-1010"></a>Actualizaciones para la versión 10.1.0

*Fecha de publicación: 06/11/2020*

Las novedades de esta versión son las siguientes:

- Si usa iPadPOS 13.4 o posterior, ahora puede controlar la sesión remota con un mouse o un trackpad.
- El cliente ahora admite los siguientes gestos de Apple Magic Mouse 2 y Apple Magic Trackpad 2: clic con el botón izquierdo, arrastre a la izquierda, clic con el botón derecho, arrastre a la derecha, desplazamiento horizontal y vertical, y zoom local.
- En el caso de los mouse externos, el cliente ahora admite los gestos de clic con el botón derecho, arrastre a la izquierda, clic con el botón derecho, arrastre a la derecha, clic con el botón central y desplazamiento vertical.
- El cliente ahora admite los métodos abreviados de teclado que usan las teclas Ctrl, Alt o Mayús con el mouse o el trackpad, incluida la selección múltiple y la selección de intervalo.
- El cliente ahora admite la característica "Pulsar para hacer clic" para el trackpad.
- Se actualizó el gesto de clic con el botón derecho del modo de puntero del mouse (no mantener presionado y soltar). En el cliente de iPhone, hemos generado algunos comentarios de Taptic cuando se detecta el gesto de clic con el botón derecho.
- Se agregó una opción para deshabilitar el cumplimiento de NLA en **Configuración de iOS** > **Cliente de Escritorio remoto**.
- Se asignó Control + Mayús + Escape a CTRL+Mayús+Esc, donde el escape se genera mediante una clave reasignada en iPadOS o Command + .
- Se asignó Command + F a Ctrl + F.
- Se corrigió un problema en que el botón central del mouse SwiftPoint no funcionaba en la versión de iPadOS 13.3.1 o anterior ni en iOS.
- Se corrigieron algunos errores que impedían que el cliente reconociera el URI "rdp:".
- Se solucionó un problema en el que la UI del conmutador envolvente en sesión mostraba entradas de aplicación obsoletas si el servidor iniciaba una desconexión.
- El cliente ahora admite la versión integrada en Azure Resource Manager de Windows Virtual Desktop.

## <a name="updates-for-version-1007"></a>Actualizaciones de la versión 10.0.7

*Fecha de publicación: 29/4/2020*

En esta actualización, se ha agregado la capacidad de ordenar la vista de lista de equipos (disponible en iPhone) por nombre u hora de la última conexión.

## <a name="updates-for-version-1006"></a>Actualizaciones de la versión 10.0.6

*Fecha de publicación: 31/3/2020*

Es el momento de una actualización rápida con algunas correcciones de errores. Las novedades de esta versión son las siguientes:

- Se han corregido varios problemas de accesibilidad de VoiceOver.
- Se ha corregido un problema por el que los usuarios no podían conectarse con credenciales en turco.
- Las sesiones que se muestran en la interfaz de usuario del conmutador ahora se ordenan por fecha y hora de inicio.
- Al seleccionar el botón atrás en el Centro de conexión, ahora se vuelve a la última sesión activa.
- Los mouse de Swiftpoint ahora se liberan al cambiar del cliente a otra aplicación.
- Se ha mejorado la interoperabilidad con el servicio Windows Virtual Desktop.
- Se han corregido bloqueos que aparecían en los informe de errores.

Agradecemos todos los comentarios que nos enviasteis a través de App Store, en la aplicación y por correo electrónico. Queremos dar las gracias especialmente a todos los usuarios que trabajaron con nosotros para diagnosticar los problemas.

## <a name="updates-for-version-1005"></a>Actualizaciones para la versión 10.0.5

*Fecha de publicación: 09/03/20*

Agrupamos algunas correcciones de errores y actualizaciones de características para esta versión de 10.0.5. Estas son las novedades:

- Los archivos RDP iniciados se importan automáticamente (busca el interruptor en la configuración general).
- Se pueden iniciar los archivos RDP basados en iCloud que no se han descargado aún en la aplicación de archivos.
- La sesión remota puede extenderse por debajo del indicador de inicio en los dispositivos iPhone (busca el interruptor en la configuración de pantalla).
- Se agregó compatibilidad para escribir caracteres compuestos con varias pulsaciones de tecla, como é.
- Se agregó compatibilidad con el teclado flotante en pantalla de iPad.
- Se agregó compatibilidad para ajustar las propiedades de las cámaras redirigidas desde una sesión remota.
- Se corrigió un error en el reconocedor de gestos que hacía que el cliente dejara de responder al conectarse a una sesión remota.
- Ahora puedes entrar en el modo Cambio de aplicación con un solo deslizamiento rápido hacia arriba (excepto cuando estás en modo Táctil con la sesión extendida en el área del indicador de inicio).
- El indicador de inicio se ocultará automáticamente cuando te conectes a una sesión remota y volverá a aparecer cuando puntees en la pantalla.
- Se agregó un método abreviado de teclado para acceder a la configuración de la aplicación en el centro de conexiones ( **comando +,** ).
- Se agregó un método abreviado de teclado para actualizar todas las áreas de trabajo en el centro de conexiones ( **comando + R** ).
- Se enlazó el método abreviado de teclado del sistema para Escape al conectarse a una sesión remota ( **comando + .** ).
- Se corrigieron los escenarios en los que el teclado en pantalla de Windows en la sesión remota era demasiado pequeño.
- Se implementó el foco de teclado automático en todo el centro de conexiones para que la entrada de datos sea más fluida.
- Al presionar **Entrar** en una solicitud de credenciales, el mensaje se descarta y se reanuda el flujo actual.
- Se corrigió un escenario en el que el cliente se bloqueaba al presionar las teclas Mayús+opción+flecha izquierda, arriba o abajo.
- Se corrigió un bloqueo que se producía al quitar un dispositivo SwiftPoint.
- Se corrigieron otros bloqueos que nos comunicaron los usuarios desde la última versión.

Damos las gracias a todos los usuarios que comunicaron errores y trabajaron con nosotros para diagnosticar problemas.

## <a name="updates-for-version-1004"></a>Actualizaciones de la versión 10.0.4

*Fecha de publicación: 03/02/20*

Es el momento de otra actualización. Damos las gracias a todos los usuarios que comunicaron errores y trabajaron con nosotros para diagnosticar problemas. Las novedades de esta versión son las siguientes:

- Ahora se muestra la UI de confirmación cuando se eliminan las puertas de enlace y las cuentas de usuario.
- La UI de búsqueda del centro de conexiones se ha rehecho ligeramente.
- La sugerencia de nombre de usuario, si existe, se muestra ahora en la UI de solicitud de credenciales cuando se inicia desde un archivo o URI de RDP.
- Se corrigió un problema que hacía que el teclado en pantalla extendido se ampliara por debajo de la muesca de iPhone.
- Se corrigió un error por el que los teclados externos dejaban de funcionar cuando se desconectaban y volvían a conectar.
- Se agregó compatibilidad con la tecla Esc en teclados externos.
- Se corrigió un error en el que aparecían caracteres ingleses al escribir caracteres chinos.
- Se corrigió un error en el que entradas en chino permanecían en la sesión remota después de su eliminación.
- Se corrigieron otros bloqueos que nos comunicaron los usuarios desde la última versión.

Agradecemos todos los comentarios que nos enviasteis a través de App Store, comentarios en la aplicación y correo electrónico. Seguiremos centrándonos en mejorar esta aplicación con cada versión.

## <a name="updates-for-version-1003"></a>Actualizaciones de la versión 10.0.3

*Fecha de publicación: 16/01/20*

Estamos en 2020 y es el momento para la primera versión del año, lo que significa nuevas características y correcciones de errores. Esto es lo que se incluye en esta actualización:

- Compatibilidad para iniciar conexiones desde archivos RDP y URI de RDP.
- Los encabezados de área de trabajo se pueden contraer.
- Se admite el zoom y el movimiento panorámico al mismo tiempo en el modo Puntero del mouse.
- Un gesto de mantener presionado en el modo Puntero del mouse desencadenará un clic con el botón secundario en la sesión remota.
- Se quitó el gesto Force Touch para hacer clic con el botón secundario en el modo Puntero del mouse.
- La pantalla del conmutador en sesión admite ahora la desconexión, incluso si no hay ninguna aplicación conectada.
- Ahora se admite el cierre del elemento por cambio de foco en la pantalla del conmutador en sesión.
- Los equipos y las aplicaciones ya no se reordenan automáticamente en la pantalla del conmutador en sesión.
- Se amplió la zona de prueba de pulsación para el menú de puntos suspensivos de la vista en miniaturas del equipo.
- La página de configuración de dispositivos de entrada contiene ahora un vínculo a los dispositivos compatibles.
- Se corrigió un error que hacía que la interfaz de usuario de permisos de Bluetooth apareciera repetidamente en el inicio para algunos usuarios.
- Se corrigieron otros bloqueos que nos comunicaron los usuarios desde la última versión.

## <a name="updates-for-version-1002"></a>Actualizaciones para la versión 10.0.2

*Fecha de publicación: 20/12/19*

Hemos trabajado mucho para corregir errores y agregar características útiles. Las novedades de esta versión son las siguientes:

- Compatibilidad con la entrada en japonés y chino en teclados de hardware.
- La vista de lista del equipo muestra ahora el nombre descriptivo de la cuenta de usuario asociada, si existe.
- La interfaz de usuario de permisos de la experiencia del primer inicio se representa ahora correctamente en modo claro.
- Se corrigió un bloqueo que se producía cuando alguien presionaba las teclas de opción y de flecha arriba o abajo al mismo tiempo en un teclado de hardware.
- Se actualizó la distribución del teclado en pantalla usada en la interfaz de usuario de solicitud de contraseña para facilitar la búsqueda de la tecla de barra diagonal inversa.
- Se corrigieron otros bloqueos que nos comunicaron los usuarios desde la última versión.

## <a name="updates-for-version-1001"></a>Actualizaciones para la versión 10.0.1

*Fecha de publicación: 15/12/19*

Las novedades de esta versión son las siguientes:

- Soporte técnico para el servicio Windows Virtual Desktop.
- Se actualizó la interfaz de usuario del centro de conexiones.
- Se actualizó la interfaz de usuario en sesión.

## <a name="updates-for-version-1000"></a>Actualizaciones de la versión 10.0.0

*Fecha de publicación: 13/12/19*

Ha transcurrido un año desde la última vez que se actualizó el cliente de Escritorio remoto para iOS. Pero aquí estamos con una nueva e impresionante actualización. A partir de ahora, se publicarán muchas más actualizaciones de forma periódica. Estas son las novedades de la versión 10.0.0:

- Soporte técnico para el servicio Windows Virtual Desktop.
- Nueva interfaz de usuario del Centro de conexión.
- Nueva interfaz de usuario en la sesión que puede cambiar entre aplicaciones y equipos conectados.
- Nuevo diseño para el teclado en pantalla auxiliar.
- Compatibilidad mejorada con el teclado externo.
- Compatibilidad con el mouse Bluetooth SwiftPoint.
- Compatibilidad con el redireccionamiento del micrófono.
- Compatibilidad con el redireccionamiento del almacenamiento local.
- Compatibilidad con el redireccionamiento de la cámara (solo disponible para Windows 10, versión 1809 o posterior).
- Compatibilidad con los nuevos dispositivos iPhone y iPad.
- Compatibilidad con los temas oscuro y claro.
- Control de la posibilidad de bloquear el teléfono cuando se conecta a una aplicación o un equipo remoto.
- Ahora puedes contraer la barra de conexión en la sesión. Para hacerlo, mantén pulsado el botón del logotipo de Escritorio remoto.

## <a name="updates-for-version-8142"></a>Actualizaciones de la versión 8.1.42

*Fecha de publicación: 20/06/2018*

- Mejoras en el rendimiento y corrección de errores.

## <a name="updates-for-version-8141"></a>Actualizaciones de la versión 8.1.41

*Fecha de publicación: 28/03/2018*

- Actualizaciones para abordar la corrección de oráculo de cifrado de CredSSP que se describe en CVE-2018-0886.
