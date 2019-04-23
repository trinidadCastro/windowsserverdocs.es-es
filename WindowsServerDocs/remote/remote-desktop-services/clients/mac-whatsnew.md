---
title: Novedades de escritorio remoto en Mac
description: Obtenga información sobre los cambios recientes en el cliente de escritorio remoto para Mac
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
ms.openlocfilehash: d0cf81f24374e81ca28c2d2cfd83a394e096706c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59844796"
---
# <a name="whats-new-for-the-remote-desktop-client-on-macos"></a>Novedades del cliente de escritorio remoto en macOS

Se actualiza con regularidad el [cliente de escritorio remoto para macOS](remote-desktop-mac.md), agregar nuevas características y solución de problemas. Consulte las actualizaciones más recientes a continuación.

Si tiene algún problema, puede ponerse en contacto con nosotros a través **Ayuda > notificar un problema**.

## <a name="updates-for-version-1029"></a>Actualizaciones de versión 10.2.9
*Fecha de publicación: 3/6/2019*

- En esta versión que se ha corregido un problema de conectividad de puerta de enlace de escritorio remoto que se puede producir cuando se toma la redirección del servidor colocar.
- También hemos hecho frente una regresión de la puerta de enlace de escritorio remoto causada por la 10.2.8 update.

## <a name="updates-for-version-1028"></a>Actualizaciones de versión 10.2.8
*Fecha de publicación: 3/1/2019*

- Resolver problemas de conectividad que aparecen cuando se usa una puerta de enlace de escritorio remoto.
- Advertencias de certificado incorrecto fijo que se han mostrado al conectarse.
- Resolver algunos de los casos donde la barra de menús y acoplar innecesariamente ocultaría al iniciar aplicaciones remotas.
- Rehacer el código de redirección del Portapapeles a dirección bloqueos y bloqueos que han sido asedia algunos usuarios.
- Se ha corregido un error que provocó el centro de conexiones para desplazarse innecesariamente al iniciar una conexión.

## <a name="updates-for-version-1027"></a>Actualizaciones de versión 10.2.7
*Fecha de publicación: 2/6/2019*

- En esta versión se han resuelto mispaints de gráficos (causados por un error de codificación del servidor) que aparece cuando se usa el modo AVC444.

## <a name="updates-for-version-1026"></a>Actualizaciones de versión 10.2.6
*Fecha de publicación: 1/28/2019*

- Se agregó compatibilidad para el códec AVC (420 y 444), disponible al conectarse a las versiones actuales de Windows 10.
- En Ajustar al modo de ventana, una actualización de la ventana ahora se produce inmediatamente después de un cambio de tamaño para asegurarse de que el contenido se representa en el nivel correcto de interpolación.
- Se ha corregido un error de diseño que provocó la fuentes encabezados se superpongan para algunos usuarios.
- Limpia la interfaz de usuario de las preferencias de aplicación.
- Perfeccionadas la interfaz de usuario de escritorio de agregar o modificar.
- Realiza una gran cantidad de ajuste y finalización ajustes a las vistas de mosaico y lista de centro de conexiones de equipos de escritorio y las fuentes de distribución.

>[!NOTE]
>Hay un error en carpeta macOS 10.14.0 y 10.14.1 que puede provocar el ".com.microsoft.rdc.application-data_SUPPORT/_EXTERNAL_DATA" (anidado profunda en la carpeta ~/Library) para consumir una gran cantidad de espacio en disco. Para resolver este problema, elimine el contenido de la carpeta y actualizar a macOS 10.14.2. Tenga en cuenta que un efecto de eliminar el contenido de la carpeta que asigna a los marcadores de imágenes de instantáneas se eliminarán. Estas imágenes se volverá a generar al volver a conectarse al equipo remoto.

## <a name="updates-for-version-1024"></a>Actualizaciones de versión 10.2.4
*Fecha de publicación: 12/18/2018*

- Agrega compatibilidad con el modo oscuro para macOS Mojave 10.14.
- Una opción para importar desde el 8 de escritorio remoto de Microsoft ahora aparece en el centro de conexiones si está vacía.
- Compatibilidad de redirección de carpeta tratados con algunas aplicaciones empresariales de terceros.
- Resolver problemas donde los usuarios obtención de un error de la puerta de enlace de escritorio remoto 0x30000069 debido a la seguridad de los problemas de reserva de protocolo.
- Problemas de representación progresiva fijo con que algunos usuarios experimentaban ajustarán al modo de ventana.
- Se ha corregido un error que impedía que copiar y pegar desde la versión más reciente de un archivo de copia de archivos.
- Mejora basada en mouse desplazamiento para diferencias de desplazamiento pequeño.

## <a name="updates-for-version-1023"></a>Actualizaciones de versión 10.2.3
*Fecha de publicación: 11/06/2018*

- Se agregó compatibilidad para la configuración del archivo RDP "remoteapplicationcmdline" para escenarios de aplicación remota.
- El título de la ventana de sesión ahora incluye el nombre del archivo RDP (y el nombre del servidor) cuando se inicia desde un archivo RDP.
- Se ha corregido RD notificado problemas de rendimiento de la puerta de enlace.
- Puerta de enlace de RD notificada fijo se bloquea.
- Problemas corregidos en la conexión dejaba de responder cuando se conecta a través de una puerta de enlace de escritorio remoto.
- Mejor control de aplicaciones remotas de pantalla completa al ocultar la barra de menús de forma inteligente y acoplar.
- Se ha corregido escenarios donde aplicaciones remotas permanecen ocultas después de que se va a iniciar.
- Dirigirse a las actualizaciones de representación lento cuando se usa "Ajustar a la ventana" aceleración de hardware deshabilitada.
- Controlar errores de creación de base de datos causados por permisos incorrectos cuando se inicia el cliente. 
- Se corrigió un problema donde el cliente no se bloquea constantemente en el lanzamiento y no se inicia para algunos usuarios.
- Se ha corregido un escenario donde las conexiones se importaron incorrectamente como de pantalla completa desde el 8 de escritorio remoto.

## <a name="updates-for-version-1022"></a>Actualizaciones de versión 10.2.2
*Fecha de publicación: 10/09/2018*

- Un nuevo centro de conexiones que admite arrastrar y colocar, disposición manual de equipos de escritorio, puede cambiar de tamaño columnas en modo de vista de lista, la ordenación basada en la columna y administración más simple de grupo.
- El centro de conexiones ahora recuerda la última pivot activo (equipos de escritorio o fuentes) al cerrar la aplicación.
- Se han mejorado la credencial que se pida confirmación de la interfaz de usuario y los flujos.
- Comentarios de la puerta de enlace de escritorio remoto ahora forma parte de conexión del estado de la interfaz de usuario.
- Se mejoró la importación de configuración desde el cliente de la versión 8.
- Los archivos RDP para los puntos de conexión de RemoteApp ahora pueden importarse en el centro de conexiones.
- Optimizaciones de pantalla de retina para escenarios de escritorio remoto solo monitor.
- Compatibilidad para especificar el nivel de interpolación de gráficos (que afecta al desenfoque) si no usa las optimizaciones de Retina.
- soporte técnico de 256 colores para habilitar la conectividad a Windows 2000.
- Se ha corregido recortes de los bordes derecho e inferior de la pantalla cuando se conecta a Windows 7, Windows Server 2008 R2 y versiones anteriores.
- Copiar un archivo local en Outlook (que se ejecuta en una sesión remota) ahora agrega el archivo como datos adjuntos.
- Se ha corregido un problema que se ralentice las transferencias de archivos basado en la mesa de trabajo si los archivos que se originaron desde un recurso compartido de red.
- Dirigirse a un error que provocaba que a Excel (que se ejecuta en una sesión remota) deje de responder cuando se guarda en un archivo en una carpeta redirigida.
- Se ha corregido un problema que hacía que no hay espacio disponible ser notificados para las carpetas redirigidas.
- Se ha corregido un error que provocó las miniaturas consumir demasiada almacenamiento en disco en macOS 10.14.
- Se agregó compatibilidad para aplicar las directivas de redirección de dispositivo de puerta de enlace de escritorio remoto.
- Se ha corregido un problema que impedía ventanas de sesión de cierre al desconectar de una conexión mediante la puerta de enlace de escritorio remoto.
- Si el servidor no se exige la autenticación de nivel de red (NLA), ahora se enrutará a la pantalla de inicio de sesión si la contraseña ha expirado.
- Se han corregido los problemas de rendimiento que aparecen cuando se transfieren grandes cantidades de datos a través de la red.
- Se corrige la redirección de tarjeta inteligente.
- Soporte técnico para todos los valores posibles del "Nivel de autenticación" y "EnableCredSspSupport" configuración del archivo RDP si la clave de ClientSettings.EnforceCredSSPSupport usuario predeterminada (en el dominio com.microsoft.rdc.macos) se establece en 0.
- Compatibilidad con la configuración del archivo RDP "Símbolo del sistema para las credenciales de cliente" cuando no se negocia NLA.
- Compatibilidad con inicio de sesión basado en tarjeta inteligente a través de la redirección de tarjeta inteligente en el símbolo de Winlogon cuando no se negocia NLA.
- Se ha corregido un problema que impedía la descarga de los recursos que tengan espacios en la dirección URL de fuente.

## <a name="updates-for-version-1021"></a>Actualizaciones de versión 10.2.1
*Fecha de publicación: 08/06/2018*

- Equipos unidos a un habilitado conectividad a Azure Active Directory (AAD). Para conectarse al equipo unido a AAD, el nombre de usuario debe estar en uno de los siguientes formatos: "AzureAD\user" o "AzureAD\user@domain".
- Solucionado algunos errores al uso de tarjetas inteligentes en una sesión remota.

## <a name="updates-for-version-1020"></a>Actualizaciones de versión 10.2.0
*Fecha de publicación: 07/24/2018*

- Actualizaciones incorporadas de cumplimiento del RGPD.
- MicrosoftAccount\username@domain Ahora se acepta como un nombre de usuario válido.
- Uso compartido del Portapapeles se ha reescrito para ser más rápido y admite varios formatos.
- Copiar y pegar texto, imágenes o archivos entre sesiones ahora omite el Portapapeles del equipo local.
- Ahora puede conectarse a través de un servidor de puerta de enlace de escritorio remoto con un certificado de confianza (si acepta los mensajes de advertencia).
- Aceleración de hardware sin sistema ahora se usa (si se admite) para acelerar el procesamiento y optimizar el uso de la batería.
- Cuando se usa la aceleración de hardware de Metal que intentamos para que funcione el arte de magia para realizar los gráficos de la sesión aparecen más nítidos.
- Me ha permitido eliminar algunas instancias donde windows dejaba de responder en torno a después de cerrarse.
- Errores corregidos que se impide el inicio de programas de RemoteApp en algunos escenarios.
- Se ha corregido un error de sincronización de canal de puerta de enlace de escritorio remoto que generaba 0x204 errores.
- La forma del cursor del mouse ahora se actualiza correctamente cuando sale de una sesión o una ventana de RemoteApp.
- Se corrigió un error de redirección de carpeta que hacía que la pérdida de datos al copiar y pegar las carpetas.
- Se ha corregido un problema de redirección de carpeta que provocaba informes incorrectos de tamaños de las carpetas.
- Se ha corregido una regresión que evitaba iniciando sesión en un equipo unido a AAD mediante una cuenta local.
- Errores corregidos que provocaban la sesión de contenido de la ventana para que se recorte.
- Se agregó compatibilidad para los certificados de punto de conexión de escritorio remoto que contienen las claves asimétricas de curva elíptica.
- Se ha corregido un error que impedía que la descarga de recursos administrados en algunos escenarios.
- Solucionar un problema de recorte con el centro de conexiones anclados.
- Se ha corregido las casillas de verificación en la hoja de propiedades de presentación para que funcione mejor juntos.
- Ahora está deshabilitado el bloqueo de la relación de aspecto cuando cambio dinámico de presentación está en vigor.
- Solucionar problemas de compatibilidad con la infraestructura de F5.
- Actualiza el control de contraseñas en blanco para asegurarse de que se muestran los mensajes correctos en tiempo de conexión.
- Desplazamiento de problemas de compatibilidad con MapInfra Pro de mouse fijo.
- Se ha corregido algunos problemas de alineación en el centro de conexiones cuando se ejecuta en Mojave.

## <a name="updates-for-version-1018"></a>Actualizaciones de versión 10.1.8
*Fecha de publicación: 05/04/2018*

- Se agregó compatibilidad para cambiar la resolución remota al cambiar el tamaño de la ventana de sesión.
- Fijos escenarios donde el recurso remoto fuente descarga tardaría demasiado tiempo.
- Puede resolver el error 0x207 que podría producirse al conectarse a servidores que no haya revisado con la actualización de la corrección de oracle de cifrado CredSSP (CVE-2018-0886).

## <a name="updates-for-version-1017"></a>Actualizaciones de versión 10.1.7
*Fecha de publicación: 04/05/2018*

- Realizar revisiones de seguridad para incorporar las actualizaciones de corrección de CredSSP cifrado oracle como se describe en CVE-2018-0886.
- RemoteApp icono y el mouse cursor representación mejorada para abordar mispaints notificadas.
- Se han solucionado problemas donde aparecieron windows RemoteApp detrás del centro de conexiones.
- Se ha corregido un problema que se produjo al editar recursos locales después de importar desde el 8 de escritorio remoto.
- Ahora puede iniciar una conexión, presione ENTRAR en un icono del escritorio.
- Cuando esté en la vista de pantalla completa, CMD + M ahora se asigna correctamente a WIN + M.
- El centro de conexiones, las preferencias y acerca de windows ahora responden a CMD + M.
- Ahora puede comenzar a detectar las fuentes de distribución al presionar ENTRAR en el **agregando recursos remotos** página.
- Se ha corregido un problema donde una nueva fuente de los recursos remotos aparecía vacío en el centro de conexiones hasta después de que actualice.
 
## <a name="updates-for-version-1016"></a>Actualizaciones de versión 10.1.6
*Fecha de publicación: 03/26/2018*

- Se ha corregido un problema donde deben reordenar windows RemoteApp a sí mismos.
- Resuelve un error que causaba algunas ventanas de RemoteApp que se atasca detrás de su ventana primaria.
- Solucionar un problema desplazamiento del puntero del mouse que afectados algunos programas de RemoteApp.
- Se ha corregido un problema donde iniciar una nueva conexión dio el foco a una sesión existente, en lugar de abrir una ventana nueva sesión.
- Se ha corregido un error con un mensaje de error, verá el mensaje correcto ahora si no podemos encontrar la puerta de enlace.
- El acceso directo de Quit (⌘ + Q) no se muestra en la interfaz de usuario coherente.
- Cuando ajuste en el modo "Ajustar a la ventana" ha mejorado la calidad de imagen.
- Se ha corregido una regresión que ha provocado varias instancias de la carpeta principal que aparezcan en la sesión remota.
- Actualiza el icono predeterminado para los iconos de escritorio.
