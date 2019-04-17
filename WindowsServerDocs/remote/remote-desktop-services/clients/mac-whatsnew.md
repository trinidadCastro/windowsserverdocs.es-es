---
title: Novedades para escritorio remoto en Mac
description: Obtén información sobre los cambios recientes en el cliente de escritorio remoto para Mac
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 03/06/2019
ms.localizationpriority: medium
ms.openlocfilehash: 11da8bc333da7f44b2732266837d2df216142f85
ms.sourcegitcommit: 971f6538e8d89af84ef50fc8aab2188bdf6f47cb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/02/2019
ms.locfileid: "9279126"
---
# <a name="whats-new-for-the-remote-desktop-client-on-macos"></a>¿Novedades para el cliente de escritorio remoto en Mac OS?

Periódicamente actualizamos el [cliente de escritorio remoto para macOS](remote-desktop-mac.md), agregando nuevas funcionalidades y corregir los problemas. Echa un vistazo a las actualizaciones más recientes a continuación.

Si se produce algún problema, se puede siempre en contacto con nosotros a través de la **Ayuda > informe de un problema**.

## <a name="updates-for-version-10210"></a>Actualizaciones de versión 10.2.10
*Fecha de publicación: 30/3/2019*

- En esta versión se trata de inestabilidad causado por la actualización de macOS 10.14.4 reciente. También hemos corregido mispaints que aparecieron cuando descodifican los datos de códec AVC codificados por un servidor con hardware NVIDIA.

## <a name="updates-for-version-1029"></a>Actualizaciones de versión 10.2.9
*Fecha de publicación: 6/3/2019*

- En esta versión que hemos corregido un problema de conectividad de puerta de enlace de escritorio remoto que se puede producir cuando realiza la redirección server coloca.
- También se abordan una regresión de puerta de enlace de escritorio remoto causado por la 10.2.8 update.

## <a name="updates-for-version-1028"></a>Actualizaciones de versión 10.2.8
*Fecha de publicación: 1/3/2019*

- Resolver problemas de conectividad que se exponen al usar una puerta de enlace de escritorio remoto.
- Advertencias de certificado incorrecto fijo que se han mostrado cuando se conecta.
- Resolver algunos casos donde se ha ocultar innecesariamente al inicio de aplicaciones remotas la barra de menús y dock.
- Rehacer el código de redirección de Portapapeles bloqueos de dirección y bloqueos que han sido asedia algunos usuarios.
- Corregido un error que causó el centro de conexión innecesariamente desplazarse al iniciar una conexión.

## <a name="updates-for-version-1027"></a>Actualizaciones de versión 10.2.7
*Fecha de publicación: 6/2/2019*

- En esta versión se tratan mispaints de gráficos (causados por un error de codificación de servidor) que aparecieron al usar el modo AVC444.

## <a name="updates-for-version-1026"></a>Actualizaciones de versión 10.2.6
*Fecha de publicación: 28/1/2019*

- Se agregó compatibilidad para el códec AVC (420 y 444), disponible cuando se conecta a las versiones actuales de Windows 10.
- En Ajustar a modo de ventana, ventana ahora se produce una actualización inmediatamente después de un cambio de tamaño para garantizar que el contenido se representa en el nivel de interpolación correcto.
- Corregido un error de diseño que causó los encabezados de fuente para superponer para algunos usuarios.
- Limpiar la interfaz de usuario de las preferencias de aplicación.
- Elegante la interfaz de usuario de escritorio de agregar o editar.
- Realiza una gran cantidad de ajuste y finalización ajustes en las vistas de icono y lista de centro de conexión para equipos de escritorio y fuentes.

>[!NOTE]
>Hay un error en la carpeta de macOS 10.14.0 y 10.14.1 que pueden provocar que el ".com.microsoft.rdc.application-data_SUPPORT/_EXTERNAL_DATA" (anidado profunda dentro de la carpeta de ~/Library) consumir una gran cantidad de espacio en disco. Para resolver este problema, elimina el contenido de la carpeta y actualizar a macOS 10.14.2. Ten en cuenta que un efecto secundario de eliminación de contenido de la carpeta que se eliminarán asignadas a los marcadores de imágenes de instantáneas. Estas imágenes se volverá a generar al conectarse de nuevo al equipo remoto.

## <a name="updates-for-version-1024"></a>Actualizaciones de versión 10.2.4
*Fecha de publicación: 18/12/2018*

- Agrega compatibilidad con el modo oscuro para macOS Mojave 10.14.
- Una opción para importar de 8 de escritorio remoto de Microsoft ahora aparece en el centro de la conexión si está vacío.
- Solucionado compatibilidad de redirección de carpetas con algunas aplicaciones empresariales de terceros.
- Problemas resueltos donde los usuarios han obtención de un error de la puerta de enlace de escritorio remoto 0x30000069 debido a la seguridad problemas de reserva de protocolo.
- Problemas de representación progresiva fijo con que algunos usuarios experimentaban se ajustan a modo de ventana.
- Se ha corregido un error que impidió que copiar y pegar la versión más reciente de un archivo de copia archivos.
- Mejorado en función del mouse de desplazamiento para deltas pequeña de desplazamiento.

## <a name="updates-for-version-1023"></a>Actualizaciones de versión 10.2.3
*Fecha de publicación: 06/11/2018*

- Se agregó compatibilidad para la configuración del archivo RDP "remoteapplicationcmdline" para escenarios de la aplicación remota.
- El título de la ventana de sesión ahora incluye el nombre del archivo RDP (y el nombre de servidor) cuando se inicia desde un archivo RDP.
- Corregido RD notificado problemas de rendimiento de la puerta de enlace.
- Bloquea la puerta de enlace de escritorio remoto notificado fijo.
- Problemas corregidos donde se bloqueará la conexión cuando se conecta a través de una puerta de enlace de escritorio remoto.
- Mejor control de aplicaciones remotas de pantalla completa al inteligente ocultar la barra de menús y dock.
- Corregido escenarios donde las aplicaciones remotas se ha conservado ocultadas después de que se inicia.
- Solucionado representación lenta actualizaciones al usar "Ajustar a la ventana" con aceleración de hardware deshabilitada.
- Controlar errores de creación de base de datos causados por permisos incorrectos cuando se inicia el cliente. 
- Corregido un problema donde el cliente estaba bloquee de forma coherente en el inicio y no se inicia para algunos usuarios.
- Corregido un escenario donde se han importado correctamente conexiones en pantalla completa de 8 de escritorio remoto.

## <a name="updates-for-version-1022"></a>Actualizaciones de versión 10.2.2
*Fecha de publicación: 09/10/2018*

- Un nuevo centro de conexión que admita arrastrar y colocar, disposición manual de los equipos de escritorio, puede cambiar de tamaño columnas en modo de vista de lista, la ordenación basada en la columna y administración más sencilla de grupo.
- El centro de conexión ahora recuerda el último control dinámico activo (equipos de escritorio o fuentes) al cierre de la aplicación.
- Solicitud de la interfaz de usuario y flujos de credenciales se han mejorado.
- Comentarios de puerta de enlace de escritorio remoto ahora son parte de conexión del estado de la interfaz de usuario.
- Se ha mejorado la importación de la configuración de la versión 8 del cliente.
- Ahora se pueden importar archivos RDP apunta a los puntos de conexión de RemoteApp en el centro de conexión.
- Optimizaciones de pantalla retina para escenarios de escritorio remoto de monitor único.
- Compatibilidad para especificar el nivel de la interpolación de gráficos (que afecta a desenfoque) cuando no se utiliza las optimizaciones de Retina.
- soporte de 256 colores para habilitar la conectividad a Windows 2000.
- Corrige el recorte de los bordes derecho e inferior de la pantalla cuando se conecta a Windows 7, Windows Server 2008 R2 y versiones anteriores.
- Copiar un archivo local en Outlook (que se ejecuta en una sesión remota) ahora agrega el archivo como un archivo adjunto.
- Corrige un problema que se ralentiza transferencias de archivos basados en el área de trabajo si los archivos se originaron desde un recurso compartido de red.
- Resolver un error que hacía que a Excel (que se ejecuta en una sesión remota) que se bloquea cuando se guarda en un archivo en una carpeta redirigida.
- Corrige un problema que no hacía que ningún espacio libre notificarse para las carpetas redirigidas.
- Corregido un error que causó miniaturas para consumir demasiado almacenamiento de disco en macOS 10.14.
- Se agregó compatibilidad para aplicar las directivas de redirección de dispositivo de puerta de enlace de escritorio remoto.
- Corrige un problema que impidió que sesión windows cierre al desconectarse de una conexión con la puerta de enlace de escritorio remoto.
- Si no lo exige por el servidor de autenticación de nivel de red (NLA), ahora se enrutarán a la pantalla de inicio de sesión si ha expirado tu contraseña.
- Fijo problemas de rendimiento que aparecieron cuando una gran cantidad de datos se transfieren a través de la red.
- Corrige el redireccionamiento de tarjetas inteligentes.
- Compatibilidad con todos los valores posibles de la "EnableCredSspSupport" y "Nivel de autenticación" configuración del archivo RDP si la clave de ClientSettings.EnforceCredSSPSupport usuario predeterminado (en el dominio com.microsoft.rdc.macos) se establece en 0.
- Compatibilidad con la configuración del archivo RDP "Símbolo del sistema para las credenciales de cliente" cuando no se ha negociado NLA.
- Compatibilidad con inicio de sesión con tarjeta inteligente a través de la redirección de tarjeta inteligente en el símbolo de Winlogon cuando no se ha negociado NLA.
- Corrige un problema que no podrán descargar los recursos que tengan espacios en la dirección URL de la fuente.

## <a name="updates-for-version-1021"></a>Actualizaciones de versión 10.2.1
*Fecha de publicación: 06/08/2018*

- Conectividad habilitada a Azure Active Directory (AAD) se unió a equipos. Para conectarse a un AAD Unidos a un equipo, el nombre de usuario debe estar en uno de los siguientes formatos: "AzureAD\user" o "AzureAD\user@domain".
- Resolver algunos errores que afectan al uso de tarjetas inteligentes en una sesión remota.

## <a name="updates-for-version-1020"></a>Actualizaciones de versión 10.2.0
*Fecha de publicación: 24/07/2018*

- Actualizaciones incorporadas para el cumplimiento de GDPR.
- MicrosoftAccount\username@domainAhora se acepta como un nombre de usuario válido.
- Portapapeles compartido se ha vuelto a escribir para ser más rápido y admiten varios formatos.
- Copiar y pegar texto, imágenes o archivos entre sesiones ahora omite el Portapapeles del equipo local.
- Ahora puede conectarse a través de un servidor de puerta de enlace de escritorio remoto con un certificado de confianza (si se aceptan los mensajes de advertencia).
- Ahora se usa la aceleración de hardware de reconstrucción completa (si se admite) para aumentar la velocidad de representación y optimizar el uso de la batería.
- Al usar la aceleración de hardware de reconstrucción completa que intentamos funcionan algunos mágico para hacer que los gráficos de sesión aparecen más nítidos.
- Obtuvimos elimina algunos casos donde se bloquean windows después de que se cierre.
- Errores fijos que se impide que el inicio de los programas de RemoteApp en algunos escenarios.
- Corregido un error de sincronización de canal de puerta de enlace de escritorio remoto que se lo que provoca que 0x204 errores.
- La forma de cursor del mouse ahora se actualiza correctamente cuando se mueve fuera de una sesión o una ventana de RemoteApp.
- Se corrige un error de redireccionamiento de carpetas que estaba causando pérdida de datos al copiar y pegar carpetas.
- Corregido un problema de redirección de carpetas que causó informes incorrectos de tamaños de las carpetas.
- Corregido una regresión que se impide que iniciar sesión en un equipo unido a AAD con una cuenta local.
- Errores fijos que provocan la sesión de contenido de la ventana de recorte.
- Se agregó compatibilidad para los certificados de punto de conexión a Escritorio remoto que contienen las claves asimétricas de curva elíptica.
- Corregido un error que se impide que la descarga de recursos administrados en algunos escenarios.
- Solucionado un problema de recorte con el centro de conexión anclado.
- Corrige las casillas de verificación en la hoja de propiedades de visualización para funcionan mejor juntos.
- Bloqueo de relación de aspecto ahora está deshabilitado cuando los cambios de pantalla dinámico están en vigor.
- Resolver problemas de compatibilidad con la infraestructura de F5.
- Actualiza el control de las contraseñas en blanco para garantizar que se muestran los mensajes correctos en tiempo de conexión.
- Mouse fijo desplazamiento problemas de compatibilidad con MapInfra Pro.
- Corregido algunos problemas de alineación en el centro de conexión cuando se ejecuta en Mojave.

## <a name="updates-for-version-1018"></a>Actualizaciones de versión 10.1.8
*Fecha de publicación: 05/04/2018*

- Se agregó compatibilidad para cambiar la resolución remota al cambiar el tamaño de la ventana de sesión.
- Fijos escenarios donde recursos remotos fuente descarga llevaría demasiado tiempo.
- Resolver el error 0x207 que podría ocurrir cuando se conecta a servidores no revisados con la actualización de la corrección de oracle de cifrado CredSSP (CVE-2018-0886).

## <a name="updates-for-version-1017"></a>Actualizaciones de versión 10.1.7
*Fecha de publicación: 05/04/2018*

- Realizar revisiones de seguridad para incorporar las actualizaciones de corrección de CredSSP cifrado oracle como se describe en CVE-2018-0886.
- Mejorar RemoteApp icono y mouse cursor representación para satisfacer mispaints notificados.
- Resolver problemas que aparecieron windows RemoteApp detrás del centro de conexión.
- Corrige un problema que haya ocurrido mientras editas recursos locales tras la importación de 8 de escritorio remoto.
- Ya puedes empezar una conexión al presionar ENTRAR en un icono de escritorio.
- Cuando estés en la vista de pantalla completa, CMD + M ahora correctamente se asigna a WIN + M.
- El centro de conexión, preferencias y acerca de windows ahora responden a CMD + M.
- Ya puedes empezar a descubrir fuentes al presionar ENTRAR en la página de **Agregar recursos remotos** .
- Corregido un problema donde un nuevo suministro de recursos remotos mostraba vacío en el centro de conexión hasta después de que actualiza.
 
## <a name="updates-for-version-1016"></a>Actualizaciones de versión 10.1.6
*Fecha de publicación: 26/03/2018*

- Corregido un problema donde RemoteApp windows podrían reordenar a sí mismos.
- Resuelve un error que causó algunas ventanas RemoteApp sigues teniendo problemas detrás de su ventana primaria.
- Solucionado un problema de desplazamiento de puntero del mouse que se ven afectados algunos programas de RemoteApp.
- Corregido un problema donde a partir de una nueva conexión dio el foco a una sesión existente, en lugar de abrir una ventana de sesión nuevo.
- Hemos corregido un error con un mensaje de error, verás el mensaje correcto ahora si no podemos encontrar la puerta de enlace.
- El acceso directo de salir (⌘ + Q) ahora se muestra en la interfaz de usuario de forma coherente.
- Mejora la calidad de imagen cuando estiramiento en modo "Ajustar a la ventana".
- Corregido una regresión que causó varias instancias de la carpeta principal aparezca en la sesión remota.
- Actualiza el icono predeterminado para los iconos de escritorio.
