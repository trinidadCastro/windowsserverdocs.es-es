---
title: Novedades del cliente de macOS
description: Obtén información sobre los cambios recientes en el cliente de Escritorio remoto para Mac
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.topic: article
author: heidilohr
manager: lizross
ms.author: helohr
ms.date: 04/08/2020
ms.localizationpriority: medium
ms.openlocfilehash: c378d8c4a87b6aa0cf4f6b4f30f3bd5524dbb7a9
ms.sourcegitcommit: f95d0e6904b103fc30dcae13e9199e138c287e2b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/09/2020
ms.locfileid: "80980857"
---
# <a name="whats-new-in-the-macos-client"></a>Novedades del cliente de macOS

El [cliente de Escritorio remoto para macOS](remote-desktop-mac.md) se actualiza periódicamente, con lo que se agregan nuevas características y se corrigen problemas. Aquí puedes encontrar las actualizaciones más recientes.

Si tienes algún problema, puedes ponerte en contacto con nosotros a través **Ayuda** > **Notificar un problema**.

## <a name="updates-for-version-1039"></a>Actualizaciones para la versión 10.3.9

*Fecha de publicación: 6/4/20*

En esta versión realizamos algunos cambios para mejorar la interoperabilidad con el [servicio de Windows Virtual Desktop](https://azure.microsoft.com/services/virtual-desktop/). Además, incluimos las siguientes actualizaciones:

- Control+Opción+Eliminar ahora desencadena la secuencia Ctrl+Alt+Supr (antes se requería presionar la tecla Fn).
- Se corrigió la combinación de colores de notificación del modo de teclado para el modo claro.
- Se solucionaron los escenarios en los que las conexiones iniciadas con la propiedad GatewayAccessToken de archivo RDP no funcionaban.

>[!NOTE]
>Esta es la última versión que será compatible con macOS 10.12.

## <a name="updates-for-version-1038"></a>Actualizaciones de la versión 10.3.8

*Fecha de publicación: 12/2/20*

Es el momento de nuestra primera versión de 2020.

Con esta actualización, puedes conmutar entre los modos Scancode (Ctrl+comando+K) y Unicode (Ctrl+comando+U) al escribir la entrada en el teclado. El modo Unicode permite escribir caracteres extendidos mediante la tecla de opción de un teclado Mac. Por ejemplo, en un teclado para Mac de EE. UU., Opción+2 introducirá el símbolo de marca comercial (&trade;). También puedes escribir caracteres acentuados en el modo Unicode. Por ejemplo, en un teclado para Mac de EE. UU., si escribes Opción+E y la tecla "A" al mismo tiempo, se escribirá el carácter "á" en la sesión remota.

Otras actualizaciones de esta versión incluyen:

- Se limpió la experiencia de actualización del área de trabajo y la interfaz de usuario.
- Se solucionó un problema con la redirección de la tarjeta inteligente que hacía que la sesión remota dejara de responder en la pantalla de inicio de sesión cuando aparecía el mensaje "Comprobando estado".
- Se redujo el tiempo para crear archivos temporales utilizados para copiar y pegar archivos basados en el Portapapeles.
- Los archivos temporales que se usan para copiar y pegar archivos del Portapapeles se eliminan automáticamente al salir de la aplicación, en lugar de dejar que macOS los elimine.
- Ahora, las acciones de marcador de PC se representan en la esquina superior derecha de las miniaturas.
- Se realizaron correcciones para solucionar los problemas que se comunicaron a través de la telemetría de bloqueo.

## <a name="updates-for-version-1037"></a>Actualizaciones de la versión 10.3.7

*Fecha de publicación: 6/1/20*

En nuestra última actualización del año, ajustamos algún código y corregimos los siguientes comportamientos:

- Al copiar elementos de la sesión remota en un recurso compartido de red o en una unidad USB ya no se crean archivos vacíos.
- Al especificar una contraseña vacía en una cuenta de usuario, ya no se produce una solicitud de certificado doble.

## <a name="updates-for-version-1036"></a>Actualizaciones de la versión 10.3.6

*Fecha de publicación: 6/1/20*

En esta versión, se ha solucionado un problema en que se creaban archivos de longitud cero cada vez que se copiaba una carpeta de la sesión remota en el equipo local mediante la operación de copiar y pegar archivos.

## <a name="updates-for-version-1035"></a>Actualizaciones de la versión 10.3.5

*Fecha de publicación: 6/1/20*

Esta actualización se ha realizado con la ayuda de todos los usuarios que han notificado problemas. En esta versión, hemos realizado los cambios siguientes:

- Ahora, las carpetas redirigidas se pueden marcar como de solo lectura para evitar que su contenido se modifique en la sesión remota.
- Hemos corregido un error 0x607 que aparecía al establecer la conexión mediante escenarios de Puerta de enlace de Escritorio remoto RPC sobre HTTPS.
- Se han corregido los casos en que se realizaban solicitudes dobles de credenciales a los usuarios.
- Se han corregido los casos en que los usuarios recibían el mensaje de advertencia de certificado dos veces.
- Se ha agregado la heurística para mejorar el desplazamiento con el panel táctil.
- El cliente ya no muestra el grupo "Equipos de escritorio guardados" si no hay ningún grupo creado por el usuario.
- Se ha actualizado la interfaz de usuario para los iconos de la vista de PC.
- Se han realizado correcciones para solucionar los bloqueos que recibidos a través de la telemetría de la aplicación.

> [!NOTE]
> En esta versión, ahora solo se aceptan comentarios del cliente Mac a través de [UserVoice](https://remotedesktop.uservoice.com/forums/287834-remote-desktop-for-mac).

## <a name="updates-for-version-1034"></a>Actualizaciones de la versión 10.3.4

*Fecha de publicación: 18/11/19*

Hemos trabajado mucho basándonos en vuestros comentarios y hemos elaborado una colección de correcciones de errores y actualizaciones de características.

- Al conectarte a través de una puerta de enlace de Escritorio remoto con autenticación multifactor, la conexión de puerta de enlace se mantiene abierta para evitar varias solicitudes de MFA.
- Toda la interfaz de usuario del cliente es ahora totalmente accesible desde el teclado con compatibilidad con VoiceOver.
- Los archivos copiados en el Portapapeles en la sesión remota solo se transfieren ahora cuando se pegan en el equipo local.
- Las direcciones URL copiadas en el Portapapeles en la sesión remota se pegan ahora correctamente en el equipo local.
- El factor de escala remota para admitir pantallas de retina está disponible ahora para escenarios multimonitor.
- Se ha solucionado un problema de compatibilidad con los servidores de Escritorio remoto basados en FreeRDP que provocaban problemas de conectividad en escenarios de redireccionamiento.
- Se ha solucionado la compatibilidad de la redirección de tarjetas inteligentes con versiones futuras de Windows 10.
- Se ha solucionado un problema específico de macOS 10.15 por el que se comunicaba un espacio disponible incorrecto para las carpetas redirigidas.
- Las conexiones de equipos publicadas se representan con un nuevo icono en la pestaña Áreas de trabajo.
- Las "Fuentes" se denominan ahora "Áreas de trabajo" y los "Equipos de escritorio" se denominan ahora "Equipos".
- Se han corregido incoherencias y errores del control de cuentas de usuario en la interfaz de usuario de preferencias.
- Se han corregido gran cantidad de errores para una ejecución más fluida y confiable.

## <a name="updates-for-version-1033"></a>Actualizaciones de la versión 10.3.3

*Fecha de publicación: 18/11/19*

Hemos elaborado una actualización de características y se han corregido errores para la versión 10.3.3.

En primer lugar, hemos agregado valores predeterminados de usuario para deshabilitar la tarjeta inteligente, el Portapapeles, el micrófono, la cámara y la redirección de carpetas:

- ClientSettings.DisableSmartcardRedirection
- ClientSettings.DisableClipboardRedirection
- ClientSettings.DisableMicrophoneRedirection
- ClientSettings.DisableCameraRedirection
- ClientSettings.DisableFolderRedirection

Las correcciones de errores son las siguientes:

- Se ha resuelto un problema que provocaba que no se detectaran los cambios de tamaño de la ventana de la sesión mediante programación.
- Se ha arreglado un problema por el que el contenido de la ventana de la sesión aparecía en formato pequeño al conectar en modo de ventana (con la visualización dinámica habilitada).
- Se ha solucionado un parpadeo inicial que se producía al conectar a una sesión en modo de ventana con la visualización dinámica habilitada.
- Se han corregido las incorrecciones al dibujar gráficos que se producían al conectar a Windows 7 después de alternar ajustar a la ventana con la visualización dinámica habilitada.
- Se ha corregido un error que causaba el envío de un nombre de dispositivo incorrecto a la sesión remota (interrumpiendo las licencias en algunas aplicaciones de terceros).
- Se ha resuelto un problema por el que las ventanas de aplicación remota ocupaban todo un monitor al maximizarlas.
- Se ha solucionado un problema por el que la interfaz de usuario de los permisos de acceso aparecía debajo de las ventanas locales.
- Se ha limpiado parte del código de apagado para garantizar que el cliente se cierra de manera más confiable.

## <a name="updates-for-version-1032"></a>Actualizaciones de la versión 10.3.2

*Fecha de publicación: 18/11/19*

En esta versión, se ha corregido un error que hacía que la visualización fuera de baja resolución al conectar a una sesión.

## <a name="updates-for-version-1031"></a>Actualizaciones de la versión 10.3.1

*Fecha de publicación: 18/11/19*

Hemos elaborado algunas correcciones para solucionar las regresiones que consiguieron colarse en la versión 10.3.0.

- Se han solucionado problemas de conectividad con los servidores de puerta de enlace de Escritorio remoto que usaban claves asimétricas de 4096 bits.
- Se ha corregido un error que hacía que el cliente dejara de responder aleatoriamente al descargar recursos de fuentes.
- Se ha corregido un error que hacía que el cliente se bloqueara durante la apertura.
- Se ha corregido un error que provocaba el bloqueo del cliente al importar conexiones desde el Escritorio remoto, versión 8.

## <a name="updates-for-version-1030"></a>Actualizaciones de la versión 10.3.0

*Fecha de publicación: 27/8/19*

Han pasado algunas semanas desde la última actualización, pero estamos trabajando durante este tiempo. La versión 10.3.0 ofrece algunas nuevas características y muchas correcciones técnicas.

- Ahora es posible el redireccionamiento de la cámara al conectarse a Windows 10 1809, Windows Server 2019 y versiones posteriores.
- En Mojave y Catalina, hemos agregado un nuevo cuadro de diálogo que te pide permiso para usar el micrófono y la cámara para el redireccionamiento de dispositivos.
- Se ha reescrito el flujo de suscripción de fuentes para que sea más sencillo y más rápido.
- El redireccionamiento del Portapapeles incluye ahora el formato de texto enriquecido (RTF).
- Al escribir la contraseña, tienes la opción de revelarla con una casilla "Mostrar contraseña".
- Se abordaron los escenarios donde la ventana de la sesión saltaba entre monitores.
- El Centro de conexión muestra iconos de aplicaciones remotas de alta resolución (si están disponibles).
- CMD+A se asigna a Ctrl+A cuando se usan accesos directos del Portapapeles de Mac.
- CMD+R ahora actualiza todas las fuentes suscritas.
- Se han agregado nuevas opciones de clic secundario para expandir o contraer todos los grupos o fuentes del Centro de conexión.
- Se ha agregado una nueva opción de clic secundario para cambiar el tamaño de los iconos en la pestaña Fuentes del Centro de conexiones.
- Un nuevo icono de aplicación simplificado y claro.

## <a name="updates-for-version-10213"></a>Actualizaciones de la versión 10.2.13

*Fecha de publicación: 08/05/2019*

- Se corrigió un bloqueo que ocurría al conectarse a través de una Puerta de enlace de Escritorio remoto.
- Se agregó un aviso de privacidad en el cuadro de diálogo "Agregar fuente".

## <a name="updates-for-version-10212"></a>Actualizaciones de la versión 10.2.12

*Fecha de publicación: 16/04/2019*

- Se resolvieron desconexiones aleatorias (con el código de error 0x904) que tenían lugar al conectarse a través de una Puerta de enlace de Escritorio remoto.
- Se corrigió un error que provocaba que la lista de resoluciones de las preferencias de aplicación estuviera vacía después de la instalación.
- Se corrigió un error que provocaba que el cliente se bloqueara si se agregaban ciertas resoluciones a la lista de resoluciones.
- Se abordó un bucle de mensajes de autenticación de ADAL al conectarse a implementaciones de Escritorio virtual de Windows.

## <a name="updates-for-version-10210"></a>Actualizaciones de la versión 10.2.10

*Fecha de publicación: 30/03/2019*

- En esta versión se resolvió una inestabilidad causada por la actualización reciente de macOS 10.14.4. También corregimos errores de impresión que aparecían al descodificar datos de códec AVC codificados por un servidor con hardware NVIDIA.

## <a name="updates-for-version-1029"></a>Actualizaciones de la versión 10.2.9

*Fecha de publicación: 06/03/2019*

- En esta versión se corrigió un problema de conectividad de Puerta de enlace de Escritorio remoto que se puede producir cuando tiene lugar el redireccionamiento del servidor.
- También se corrigió una regresión de Puerta de enlace de Escritorio remoto causada por la actualización 10.2.8.

## <a name="updates-for-version-1028"></a>Actualizaciones de la versión 10.2.8

*Fecha de publicación: 03/01/2019*

- Se resolvieron problemas de conectividad que aparecían al usar una Puerta de enlace de Escritorio remoto.
- Se corrigieron advertencias de certificado incorrecto que aparecían al conectarse.
- Se trataron algunos casos donde la barra de menús y el Dock se ocultaban innecesariamente al iniciar aplicaciones remotas.
- Se modificó el código de redirección del Portapapeles para solucionar bloqueos y faltas de respuesta que molestaban a algunos usuarios.
- Se corrigió un error que provocaba que el centro de conexiones se desplazara innecesariamente al iniciar una conexión.

## <a name="updates-for-version-1027"></a>Actualizaciones de la versión 10.2.7

*Fecha de publicación: 06/02/2019*

- En esta versión se resolvieron errores de impresión de gráficos (causados por un error de codificación del servidor) que aparecían al usar el modo AVC444.

## <a name="updates-for-version-1026"></a>Actualizaciones de la versión 10.2.6

*Fecha de publicación: 28/01/2019*

- Se agregó compatibilidad para el códec AVC (420 y 444), disponible al conectarse a las versiones actuales de Windows 10.
- En el modo Ajustar a la ventana, ahora se produce una actualización de la ventana inmediatamente después de un cambio de tamaño para asegurarse de que el contenido se represente en el nivel correcto de interpolación.
- Se corrigió un error de diseño que provocaba que los encabezados de las fuentes se superpusieran para algunos usuarios.
- Se limpió la interfaz de usuario de Preferencias de aplicación.
- Se perfeccionó la interfaz de usuario de Agregar/editar escritorio.
- Se hizo una gran cantidad de ajustes y terminaciones en las vistas de mosaico y lista del centro de conexiones para escritorio y fuentes.

>[!NOTE]
>Hay un error en macOS 10.14.0 y 10.14.1 que puede provocar que la carpeta ".com.microsoft.rdc.application-data_SUPPORT/_EXTERNAL_DATA" (anidada en lo profundo de la carpeta ~/Library) consuma una gran cantidad de espacio en disco. Para resolver este problema, elimina el contenido de la carpeta y actualiza a macOS 10.14.2. Ten en cuenta que un efecto secundario de eliminar el contenido de la carpeta es que las imágenes instantáneas asignadas a los marcadores se eliminarán. Estas imágenes se volverán a generar al volver a conectarse al equipo remoto.

## <a name="updates-for-version-1024"></a>Actualizaciones de la versión 10.2.4

*Fecha de publicación: 18/12/2018*

- Se agregó compatibilidad con el modo oscuro para macOS Mojave 10.14.
- Ahora en el centro de conexiones aparece una opción para importar desde Escritorio remoto de Microsoft 8 si está vacío.
- Se trató la compatibilidad para el redireccionamiento de carpetas con algunas aplicaciones empresariales de terceros.
- Se resolvieron problemas donde los usuarios recibían un error 0x30000069 de la Puerta de enlace de Escritorio remoto debido problemas de reserva del protocolo de seguridad.
- Se corrigieron problemas de representación progresiva que experimentaban algunos usuarios con el modo de ajustar a la ventana.
- Se corrigió un error que impedía copiar y pegar archivos desde la versión más reciente de un archivo.
- Se mejoró el desplazamiento basado en mouse para diferencias de desplazamiento pequeño.

## <a name="updates-for-version-1023"></a>Actualizaciones de la versión 10.2.3

*Fecha de publicación: 06/11/2018*

- Se agregó compatibilidad para la configuración del archivo RDP "remoteapplicationcmdline" para escenarios de aplicación remota.
- El título de la ventana de sesión ahora incluye el nombre del archivo RDP (y el nombre del servidor) cuando se inicia desde un archivo RDP.
- Se corrigieron problemas de rendimiento notificados de Puerta de enlace de Escritorio remoto.
- Se corrigieron bloqueos notificados de Puerta de enlace de Escritorio remoto.
- Se corrigieron problemas donde la conexión dejaba de responder al conectarse a través de una Puerta de enlace de Escritorio remoto.
- Se mejoró el control de aplicaciones remotas de pantalla completa al ocultar la barra de menús y el Dock de manera inteligente.
- Se corrigieron escenarios en los que aplicaciones remotas permanecían ocultas después de iniciarlas.
- Se abordaron las actualizaciones de representación lenta cuando al usar "Ajustar a la ventana" con la aceleración de hardware deshabilitada.
- Se controlaron los errores de creación de bases de datos causados por permisos incorrectos cuando se inicia el cliente.
- Se corrigió un problema donde el cliente se bloqueaba constantemente en el inicio y no se iniciaba para algunos usuarios.
- Se corrigió un escenario donde las conexiones se importaban incorrectamente como pantalla completa desde Escritorio remoto 8.

## <a name="updates-for-version-1022"></a>Actualizaciones de la versión 10.2.2

*Fecha de publicación: 09/10/2018*

- Un nuevo centro de conexiones que admite arrastrar y colocar, la disposición manual de escritorio, columnas a las que se cambiar el tamaño en modo de vista de lista, ordenación basada en columnas y una administración de grupos más simple.
- El centro de conexiones ahora recuerda el último pivot activo (escritorios o fuentes) al cerrar la aplicación.
- Se mejoraron la interfaz de usuario y los flujos de pedido de credenciales.
- Los comentarios de Puerta de enlace de Escritorio remoto ahora forman parte de la interfaz de usuario del estado de conexión.
- Se mejoró la importación de configuración desde el cliente de la versión 8.
- Los archivos RDP que apuntan a puntos de conexión de RemoteApp ahora pueden importarse al centro de conexiones.
- Optimizaciones de pantalla Retina para escenarios de Escritorio remoto de un solo monitor.
- Compatibilidad para especificar el nivel de interpolación de gráficos (que afecta al desenfoque) si no se usan las optimizaciones de Retina.
- Compatibilidad para 256 colores para habilitar la conectividad a Windows 2000.
- Se corrigió el recorte de los bordes derecho e inferior de la pantalla al conectarse a Windows 7, Windows Server 2008 R2 y versiones anteriores.
- La copia de un archivo local en Outlook (que se ejecuta en una sesión remota) ahora agrega el archivo como adjunto.
- Se corrigió un problema que ralentizaba las transferencias de archivos basadas en el área de montaje si los archivos se originaban en un recurso compartido de red.
- Se abordó un error que provocaba que Excel (ejecutándose en una sesión remota) dejara de responder al guardar en un archivo en una carpeta redirigida.
- Se corrigió un problema que hacía que no se informara de espacio disponible para las carpetas redirigidas.
- Se corrigió un error que provocaba que las miniaturas consumieran demasiado almacenamiento en disco en macOS 10.14.
- Se agregó compatibilidad para aplicar las directivas de redirección de dispositivos para Puerta de enlace de Escritorio remoto.
- Se corrigió un problema que impedía que las ventanas de sesión se cerraran al desconectarse de una conexión con la Puerta de enlace de Escritorio remoto.
- Si el servidor no exige la Autenticación a nivel de red (NLA), ahora se te enrutará a la pantalla de inicio de sesión si la contraseña ha expirado.
- Se corrigieron problemas de rendimiento que aparecían cuando se transferían grandes cantidades de datos a través de la red.
- Correcciones de redirección de tarjetas inteligentes.
- Compatibilidad para todos los valores posibles de las opciones "Nivel de autenticación" y "EnableCredSspSupport" del archivo de RDP si la clave predeterminada de usuario ClientSettings.EnforceCredSSPSupport (en el dominio com.microsoft.rdc.macos) está establecida en 0.
- Compatibilidad para la opción "Pedir credenciales en el cliente" del archivo de RDP cuando no se negocia NLA.
- Compatibilidad para el inicio de sesión basado en tarjetas inteligentes a través de la redirección de tarjetas inteligentes en el símbolo de Winlogon cuando no se negocia NLA.
- Se corrigió un problema que impedía la descarga de los recursos de fuente que tenían espacios en la dirección URL.

## <a name="updates-for-version-1021"></a>Actualizaciones de la versión 10.2.1

*Fecha de publicación: 06/08/2018*

- Se habilitó la conectividad equipos unidos a Azure Active Directory (AAD). Para conectarse a un equipo unido a AAD, el nombre de usuario debe tener uno de los siguientes formatos: "AzureAD\usuario" o "AzureAD\user@domain".
- Se abordaron algunos errores al usar tarjetas inteligentes en una sesión remota.

## <a name="updates-for-version-1020"></a>Actualizaciones de la versión 10.2.0

*Fecha de publicación: 24/07/2018*

- Se incorporaron actualizaciones para el cumplimiento del RGPD.
- MicrosoftAccount\username@domain ahora se acepta como nombre de usuario válido.
- El uso compartido del Portapapeles se ha reescrito para que sea más rápido y admita más formatos.
- Al copiar y pegar texto, imágenes o archivos entre sesiones, ahora se omite el portapapeles del equipo local.
- Ahora puedes conectarte a través de un servidor de puerta de enlace de Escritorio remoto con un certificado que no sea de confianza (si aceptas los mensajes de advertencia).
- Ahora se usa la aceleración de hardware de Metal (si se admite) para acelerar la representación y optimizar el uso de la batería.
- Al usar la aceleración de hardware de Metal, intentamos algunas soluciones para que los gráficos de la sesión aparecieran más nítidos.
- Se eliminaron algunas instancias en las que las ventanas dejaban de responder después de cerrarlas.
- Se corrigieron errores que impedían iniciar programas de RemoteApp en algunos escenarios.
- Se corrigió un error de sincronización de canal de Puerta de enlace de Escritorio remoto que generaba errores 0x204.
- La forma del cursor del mouse ahora se actualiza correctamente al salir de una sesión o una ventana de RemoteApp.
- Se corrigió un error de redirección de carpeta que causaba la pérdida de datos al copiar y pegar carpetas.
- Se corrigió un problema de redirección de carpeta que provocaba informes incorrectos de los tamaños de las carpetas.
- Se corrigió una regresión que impedía iniciar sesión en un equipo unido a AAD mediante una cuenta local.
- Se corrigieron errores que provocaban que el contenido de la ventana de la sesión se recortara.
- Se agregó compatibilidad para los certificados de punto de conexión de Escritorio remoto que contienen claves asimétricas de curva elíptica.
- Se corrigió un error que impedía la descarga de recursos administrados en algunos escenarios.
- Se abordó un problema de recorte con el centro de conexiones anclado.
- Se corrigieron las casillas de verificación en la pestaña Display (Pantalla) de la ventana Add a Desktop (Agregar un escritorio) para que funcionen mejor juntas.
- Ahora está deshabilitado el bloqueo de la relación de aspecto cuando el cambio dinámico de presentación está en vigor.
- Se abordaron problemas de compatibilidad con la infraestructura de F5.
- Se actualizó el control de contraseñas en blanco para asegurarse de que se muestren los mensajes correctos en tiempo de conexión.
- Se corrigieron problemas de compatibilidad de desplazamiento del mouse con MapInfra Pro.
- Se corrigieron algunos problemas de alineación en el centro de conexiones al ejecutarse en Mojave.

## <a name="updates-for-version-1018"></a>Actualizaciones de la versión 10.1.8

*Fecha de publicación: 04/05/2018*

- Se agregó compatibilidad para cambiar la resolución remota al cambiar el tamaño de la ventana de sesión.
- Se corrigieron escenarios donde la descarga de fuentes del recurso remoto tardaba demasiado tiempo.
- Se resolvió el error 0x207 que podía producirse al conectarse a servidores en los que no se había aplicado la corrección de oráculo de cifrado de CredSSP (CVE-2018-0886).

## <a name="updates-for-version-1017"></a>Actualizaciones de la versión 10.1.7

*Fecha de publicación: 05/04/2018*

- Se hicieron correcciones de seguridad para incorporar las actualizaciones de corrección de oráculo de cifrado de CredSSP, como se describe en CVE-2018-0886.
- Se mejoró la representación del icono RemoteApp y el cursor del mouse para mejorar errores de impresión notificadas.
- Se abordaron problemas donde las ventanas de RemoteApp aparecían detrás del centro de conexiones.
- Se corrigió un problema que se producía al editar recursos locales después de importar desde Escritorio remoto 8.
- Ahora puedes iniciar una conexión presionando ENTRAR en un icono del escritorio.
- Cuando estés en la vista de pantalla completa, CMD+M ahora se asigna correctamente a WIN+M.
- Las ventanas Connection Center (Centro de conexiones), Preferences (Preferencias) y About (Acerca de) ahora responden a CMD+M.
- Ahora puedes comenzar a detectar fuentes al presionar ENTRAR en la página **Adición de recursos remotos*.
- Se corrigió un problema donde una nueva fuente de los recursos remotos aparecía vacía en el centro de conexiones hasta después de actualizar.

## <a name="updates-for-version-1016"></a>Actualizaciones de la versión 10.1.6

*Fecha de publicación: 26/03/2018*

- Se corrigió un problema por el que las ventanas de RemoteApp se reordenaban a sí mismas.
- Se resolvió un error que causaba que algunas ventanas de RemoteApp se atascaran detrás de su ventana principal.
- Se abordó un problema de desfase del puntero del mouse que afectaba algunos programas de RemoteApp.
- Se corrigió un problema por el que, al iniciar una nueva conexión, se ponía el foco en una sesión existente, en lugar de abrir la ventana de una nueva sesión.
- Se corrigió un error con un mensaje de error “You'll see the correct message now if we can't find your gateway” (Verá el mensaje correcto ahora si no podemos encontrar la puerta de enlace).
- El acceso directo de salir (⌘+Q) ahora se muestra de manera coherente en la interfaz de usuario.
- Se mejoró la calidad de imagen al ampliar en el modo de "ajustar a la ventana".
- Se corrigió una regresión que provocaba que aparecieran varias instancias de la carpeta particular en la sesión remota.
- Se actualizó el icono predeterminado para los iconos del escritorio.
