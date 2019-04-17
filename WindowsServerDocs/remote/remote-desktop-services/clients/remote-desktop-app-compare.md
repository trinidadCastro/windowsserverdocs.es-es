---
title: 'Escritorio remoto: comparar las aplicaciones de cliente'
description: Obtén información sobre cómo se puede comparar las distintas aplicaciones de escritorio remoto cuando se trata de funciones y características compatibles.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 12efe858-6b76-4e08-9f72-b9603aceb0fc
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 06/22/2018
ms.localizationpriority: medium
ms.openlocfilehash: fc20d1a2c51abddb8ae014efc621f4f0b36c3677
ms.sourcegitcommit: d3f73936160505a40633ad8dd5931ac5fe3eccdb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/12/2019
ms.locfileid: "9297414"
---
# Comparar las aplicaciones de cliente

>Se aplica a: Windows 10, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2

A menudo se nos pide cómo se puede comparar las distintas aplicaciones de cliente de escritorio remoto entre sí. ¿Todos los hacen lo mismo? Estas son las respuestas a las preguntas.

## Compatibilidad con redirección

Las siguientes tablas comparan compatibilidad para el dispositivo y otros redireccionamientos en la aplicación de la conexión a Escritorio remoto, aplicación Universal, Android app, aplicación de iOS, cliente de macOS web y de la aplicación. Estas tablas cubren las redirecciones que puede tener acceso a la vez en una sesión remota. 

Si puedes remoto en el escritorio personal, hay redirecciones adicionales que puedes configurar en la **Configuración adicional** para la sesión. Si tu escritorio remoto o las aplicaciones se administran por la organización, el administrador habilitar o deshabilitar redireccionamientos a través de la configuración de directiva de grupo.

### Redirección de entrada

| Redirección | Escritorio remoto<br> Conexión | Universal | Android | iOS | macOS | cliente Web |
|-------------|-------------------------------|-----------|---------|-----|-------|------------|
| Teclado    | X                             | X         | X       | X   | X     | X          |
| Mouse       | X                             | X         | X       | X *    | X     | X          |
| Función táctil       | X                             | X         | X       | X   |       |            |
| Otros       | Lápiz                           |           |         |     |       |            |
* Ver la [lista de dispositivos de entrada admitidos para el cliente de escritorio remoto iOS Beta](remote-desktop-ios.md#supported-input-devices).

### Redirección de puertos   

| Redirección | Escritorio remoto <br>Conexión | Universal | Android | iOS | macOS | Cliente Web |
|-------------|-------------------------------|-----------|---------|-----|-------|------------|
| Puerto serie | X                             |           |         |     |       |            |
| USB         | X                             |           |         |     |       |            |

Al habilitar la redirección de puertos USB, los dispositivos USB conectados al puerto USB se reconocen automáticamente en la sesión remota.

### Otra redirección (dispositivos, etcetera)



| Redirección         | Conexión a Escritorio remoto | Universal   | Android | iOS         | macOS                                    | Cliente Web    |
|---------------------|---------------------------|-------------|---------|-------------|------------------------------------------|---------------|
| Cámaras             | X                         |             |         |             |                                          |               |
| Portapapeles           | X                         | texto, imagen | texto    | texto, imagen | X                                        | texto          |
| Unidad/almacenamiento local | X                         |             | X       |             | x                                        |               |
| Ubicación            | X                         |             |         |             |                                          |               |
| Micrófonos         | X                         |X            |         |             | X                                        |               |
| Impresoras            | X                         |             |         |             | X (solo TAZAS)                            | Imprimir PDF     |
| Escáneres            | X                         |             |         |             |                                          |               |
| Tarjetas inteligentes         | X                         |             |         |             | X (no admitida la autenticación de Windows) |               |
| Altavoces            | X                         | X           | X       | X           | X                                        | X (excepto IE) |

* Para el redireccionamiento de impresora - la aplicación de macOS es compatible con los controladores de impresora filmadora Publisher de manera predeterminada. No admiten redirigir controladores de impresora nativo.

### Configuración de RDP compatibles
Esta tabla incluye la lista de las configuraciones admitidas de archivo RDP que puede usarse con los clientes de Windows y HTML. Una "x" en la columna de la plataforma indica que el valor es compatible. Ten en cuenta que esto no es una lista exhaustiva de las configuraciones admitidas para los clientes de Windows y HTML5. Seguiremos actualizar en esta tabla para incluir más una configuración de RDP admitida para los clientes de Windows y HTML5, así como los macOS, iOS y Android clientes.

| Configuración de RDP                        | Descripción            | Valores                 | Valor predeterminado          | Escritorio Virtual de Windows | Windows | HTML5   |
|------------------------------------|------------------------|------------------------|:----------------------:|:-----------------------:|:-------:|:-------:|
| alternativas de dirección: s:value completo | Especifica un nombre alternativo o dirección IP del equipo remoto. | Cualquier nombre válido o dirección IP del equipo remoto, por ejemplo, "10.10.15.15" | | x | x | x |
| shell: s:value alternativo        | Determina si un programa se inicia automáticamente cuando se conecte con RDP. Este valor corresponde a la ruta de acceso y nombre cuadro programa en la ficha programas del cliente de conexión a Escritorio remoto. Para especificar un shell alternativo, escribe una ruta de acceso válida a un archivo ejecutable para el valor, por ejemplo "" C:\ProgramFiles\Office\word.exe"". Esta configuración también determina la ruta de acceso o el alias de la aplicación remota que se inicie en tiempo de conexión si está habilitado RemoteApplicationMode. | Por ejemplo, "C:\ProgramFiles\Office\word.exe" || x | x | x |
| audiocapturemode:i:value | Indica si está habilitada la redirección de entrada y salida de audio. Este valor corresponde al cuadro de Audio remoto del cliente de conexión a Escritorio remoto. | Captura de audio de deshabilitar (0) desde el dispositivo local. (1) permiten la captura de audio desde el dispositivo local y redirección a una aplicación de audio en la sesión remota | 0 | x | x | |
| audiomode:i:value | Determina qué máquina (es decir, locales o remotos) se reproduce audio. | Sonidos (0) en el equipo local (reproducir en este equipo); (1) reproducir sonidos en el equipo remoto (reproducir en el equipo remoto); (2) no se reproducen sonidos (no se reproducen) | 0 | x | x | x |
| nivel de autenticación: i:value | Define la configuración de nivel de autenticación de servidor. | (0) si se produce un error en la autenticación de servidor, conectar con el equipo sin previo aviso (Connect y no advertir); (1) si se produce un error en la autenticación de servidor, no establecer una conexión (no se conectan); (2) si se produce un error en la autenticación de servidor, mostrar una advertencia y permitirme conectar o rechazar la conexión (avisar); (3) ningún requisito de autenticación se especifica. | 3 | x | x ||
| autoreconnection habilitada: i:value | Esta configuración determina si el equipo cliente automáticamente intentará volver a conectar con el equipo remoto, si se interrumpe la conexión; Por ejemplo, cuando se produce una interrupción de conectividad de red. Este valor corresponde a la reconexión si la conexión es descartado casilla de verificación en la pestaña de la experiencia en Opciones de conexión a Escritorio remoto (RDC).| Equipo cliente (0) no automáticamente a intenta volver a conectar; (1) equipo cliente intenta volver a conectar automáticamente| 1 | x | x | x |
| bandwidthautodetect:i:value | Determina si se habilita la detección del tipo de red automática | Detección de tipo de red automática de deshabilitar (0); (1) habilitar la detección de tipo de red automática | 1 | x | x | x |
| compresión: i:value | Esta configuración determina si está habilitada la compresión de forma masiva cuando se transmite mediante RDP en el equipo local.|Compresión de masiva deshabilitar RDP (0); (1) habilitar la compresión de forma masiva RDP | 1 | x | x | x |
| Id. de tamaño de escritorio: i:value | Especifica las dimensiones del escritorio sesión remota de un conjunto de opciones predefinidas. Si se especifican desktopheight o desktopwidth reemplaza a esta configuración.| (0) 640 x 480; (1) 800 x 600; (2) 1024 x 768; (3) 1280 x 1024; (4) 1600 x 1200 | 0 | x | x | x |
| desktopheight:i:value | Esta configuración determina la altura de resolución (en píxeles) en el equipo remoto cuando se conecta mediante la conexión a Escritorio remoto (RDC). Este valor corresponde a la selección de theDisplay configurationslider en theDisplaytab underOptions en RDC. | Valor numérico de 200 2048 | El valor predeterminado se establece en la resolución en el equipo local | x | x | x |
| desktopwidth:i:value | Esta configuración determina el ancho de resolución (en píxeles) en el equipo remoto cuando se conecta mediante la conexión a Escritorio remoto (RDC). Este valor corresponde a la selección de theDisplay configurationslider en theDisplaytab underOptions en RDC. | Valor numérico de 200 a 4096 | El valor predeterminado se establece en la resolución en el equipo local | x | x | x |
| disableclipboardredirection:i:value | Esta configuración determina si se habilita la redirección de Portapapeles cuando se conecte al equipo remoto. | Se habilita la redirección del Portapapeles (0); (1) no está habilitada la redirección del Portapapeles | x | x | x |
| disableconnectionsharing:i:value | Determina si el cliente de escritorio remoto se vuelve a conectar a las conexiones existentes de abrir o iniciar una nueva conexión cuando se inicia un RemoteApp o el escritorio | Reconexión (0) para cualquier sesión existente; (1) iniciar nueva conexión | 0 | x | x | x |
| disableprinterredirection:i:value | Esta configuración determina si se habilita la redirección de impresora Easy Print cuando se conecte al equipo remoto. | Se habilita la redirección de impresoras fáciles de impresión (0); (1) una redirección de impresoras de impresión está deshabilitada | x | x | x |
| dominio: s:value | Esta configuración especifica el nombre del dominio donde se encuentra la cuenta de usuario que se usará para iniciar sesión en el equipo remoto mediante el uso de conexión a Escritorio remoto (RDC). El valor de esta configuración, junto con el valor de la configuración del nombre de usuario, aparecerá en el cuadro de nametext theUser en theGeneraltab underOptionsin RDC. | Un nombre de dominio válido, por ejemplo, "CONTOSO" | Ningún valor predeterminado | x | x | x |
| drivestoredirect:s:value | Determina qué unidades de disco locales en el equipo cliente se redirigidas y está disponible en la sesión remota. | Ningún valor especificado - redirigir las unidades; * - Redirigir todas las unidades de disco, incluidas las unidades que están conectadas a más adelante. DynamicDrives - redirigir las unidades que están conectadas a más adelante. La unidad y etiquetas para uno o varios discos - redirigir las unidades especificadas.| Ningún valor especificado - redirigir las unidades | x | x    | |
| enablecredsspsupport:i:value | Esta configuración determina si RDP usará el proveedor de soporte técnico de seguridad de credenciales (CredSSP) para la autenticación si está disponible. | RDP (0) no utilizará CredSSP, incluso si el sistema operativo es compatible con CredSSP; (1) RDP utilizará CredSSP si el sistema operativo son compatibles con CredSSP | 1 | x | x | |
| dirección: s:value completo | Esta configuración especifica el nombre o dirección IP del equipo remoto que quieres conectarte a | Un nombre de equipo válido, dirección IPv4 o IPv6 dirección: especifica el equipo remoto al que desea conectarse. | | x | x | x |
| gatewaycredentialssource:i:value | Especifica o recupera el método de autenticación de puerta de enlace de escritorio remoto. | (0) pedir contraseña (NTLM); (1) tarjeta inteligente de uso; (4) que el usuario pueda seleccionar más adelante | 0 | x | x | x |
| gatewayhostname:s:value | Especifica el nombre de host de puerta de enlace de escritorio remoto. | Dirección del servidor de puerta de enlace válida. ||x|x|x|
| gatewayprofileusagemethod:i:value | Especifica si vas a usar la configuración predeterminada de la puerta de enlace de escritorio remoto | (0) usa el modo de perfil predeterminado, como se especifica en el administrador; (1) usa la configuración explícita, según lo especificado por el usuario. | 0 | x | x | x |
| gatewayusagemethod:i:value | Especifica cuándo se debe usar el servidor de puerta de enlace de escritorio remoto | (0) no usar un servidor de puerta de enlace de escritorio remoto. (1) siempre usa un servidor de puerta de enlace de escritorio remoto. (2) usar un servidor de puerta de enlace de escritorio remoto si no se puede realizar una conexión directa en el Host de sesión de escritorio remoto; (3) usa la configuración predeterminada del servidor de puerta de enlace de escritorio remoto; (4) no usar una puerta de enlace de escritorio remoto, omitir el servidor para direcciones locales; Establecer este valor de propiedad a 0 o 4 son es realmente el equivalente, pero establecer esta propiedad en 4 habilitada la opción de Omitir direcciones locales. | | x | x | x |
| networkautodetect:i:value | Determina si se va a usar la detección de ancho de banda de red automática o no. Es necesario establecer el optionbandwidthautodetectto y correlaciona withconnection Tipo7. | (0) no usar detección de ancho de banda de red automática; (1) detección de ancho de banda de red automática de uso | 1 | x ||x|
| PromptCredentialOnce:i:value | Determina si se guardan credenciales de un usuario y se usa para la puerta de enlace de escritorio remoto y el equipo remoto.|Sesión remota (0) no usa las mismas credenciales; (1) sesión remota usa las mismas credenciales|1|x|x||
| redirectclipboard:i:value | Esta configuración determina si el Portapapeles en el equipo local será redirigidas y está disponible en la sesión remota. Este valor corresponde al cuadro theClipboardcheck en locales Resourcestab underOptionsin RDC. | Portapapeles (0) en el equipo local no está disponible en una sesión remota; (1) Portapapeles en el equipo local está disponible en una sesión remota|1|x|x|x|
| redirectdrives:i:value | Esta configuración determina si las unidades en el equipo cliente será redirigidas y está disponible en la sesión remota. Este valor corresponde a la underOptionsin de selecciones forDrivesunderMoreon locales Resourcestab RDC.|(0) no están disponibles en la sesión remota; las unidades en el equipo local (1) las unidades en el equipo local están disponibles en la sesión remota|0|x|x| |
| redirectprinters:i:value | Esta configuración determina si impresoras configuradas en el equipo cliente será redirigidas y está disponible en la sesión remota cuando se conecta a un equipo remoto mediante el uso de conexión a Escritorio remoto (RDC). Este valor corresponde a la selección en el cuadro de thePrinterscheck en locales Resourcestab underOptionsin RDC. | (0) no están disponibles en la sesión remota; las impresoras en el equipo local (1) las impresoras en el equipo local están disponibles en la sesión remota|1|x|x|x|
| redirectsmartcards:i:value | Esta configuración determina si los dispositivos de tarjetas inteligentes en el equipo cliente será redirigidas y está disponible en la sesión remota cuando se conecta a un equipo remoto mediante el uso de conexión a Escritorio remoto (RDC). Este valor corresponde a la selección en theSmart cardscheck cuadro underMore en locales Resourcestab underOptionsin RDC.|(0) el dispositivo de tarjetas inteligentes en el equipo local no está disponible en la sesión remota; (1) el dispositivo de tarjetas inteligentes en el equipo local está disponible en la sesión remota|1|x|x||
| remoteapplicationcmdline:s:value | Parámetros de línea de comandos opcionales para la RemoteApp.||x|x|x|
| remoteapplicationexpandcmdline:i:value| Determina si se deben expandir las variables de entorno incluidas en el parámetro de línea de comandos de RemoteApp local o remota.|Las variables de entorno (0) deben ampliarse a los valores del equipo local; (1) las variables de entorno se deben extender en el equipo remoto con los valores del equipo remoto||x|x|x|
| remoteapplicationexpandworkingdir | Determina si se deben expandir las variables de entorno incluidas en el parámetro de directorio de trabajo de RemoteApp local o remota. | Las variables de entorno (0) deben ampliarse a los valores del equipo local; (1) el equipo remoto para los valores del equipo remoto deben ampliarse variables de entorno. Nota: Se especifica el directorio de trabajo de RemoteApp a través del parámetro de directorio de trabajo de shell.||x|x|x|
|remoteapplicationfile:s:value | Especifica un archivo para el RemoteApp abrirse en el equipo remoto. Nota: Para que abrir los archivos locales, también debes habilitar redirección de la unidad de la unidad de origen.||x|x|x|
|remoteapplicationicon:s:value | Especifica el archivo de icono que se mostrará en el cliente de la interfaz de usuario al iniciar un RemoteApp. Si no se especifica ningún nombre de archivo, el cliente usará el icono estándar de escritorio remoto. Los archivos de ".ico" solo son compatibles.||x|x|x|
|remoteapplicationmode:i:value | Determina si se inicia una conexión de RemoteApp como una sesión de RemoteApp.| (0) no iniciar una sesión de RemoteApp; (1) iniciar una sesión de RemoteApp|1|x|x|x|
|remoteapplicationname:s:value | Especifica el nombre de la conexión de RemoteApp en la interfaz de cliente al iniciar la RemoteApp.| Por ejemplo, "Excel 2016"|x|x|x|
|remoteapplicationprogram:s:value | Especifica el nombre de alias o el archivo ejecutable de la RemoteApp. | Por ejemplo, "EXCEL" |x|x|x|
|Id. de modo de pantalla: i:value | Esta configuración determina si la ventana de la sesión remota aparece en pantalla completa cuando se conecta al equipo remoto mediante la conexión a Escritorio remoto (RDC). Este valor corresponde a la selección de theDisplay configurationslider en theDisplaytab underOptionsin RDC.|(1) la sesión remota, aparecerán en una ventana; (2) la sesión remota aparecerá en pantalla completa|2|x|x|x|
|tamaño: i:value inteligente | Esta configuración determina si el equipo cliente puede escalar el contenido en el equipo remoto para ajustar el tamaño de la ventana del equipo cliente.|(0) no se escalará la presentación de la ventana de cliente cuando cambia de tamaño; (1) la visualización de la ventana de cliente se escalará cuando cambia de tamaño|0|x|x||
| usar multimon:i:value | Este ajuste configura la compatibilidad con varios monitores cuando se conecta al equipo remoto mediante la conexión a Escritorio remoto (RDC).|(0) no habilitar la compatibilidad con varios monitores; (1) habilitar la compatibilidad con varios monitores|0|x|x||
| username:s:value | Esta configuración especifica el nombre de la cuenta de usuario que se usará para iniciar sesión en el equipo remoto mediante el uso de conexión a Escritorio remoto (RDC). El valor de esta configuración, junto con el valor de la configuración del dominio, aparecerá en el nombre de theUser en theGeneraltab underOptionsin RDC.| Cualquier nombre de usuario válido. ||x|x|x|
| videoplaybackmode:i:value| Esta configuración determina si conexión a Escritorio remoto (RDC) usará RDP eficaz transmisión por secuencias multimedia para la reproducción de vídeo.|(0) no usan RDP eficaz transmisión por secuencias multimedia para la reproducción de vídeo; (1) usar RDP eficaz transmisión por secuencias multimedia para la reproducción de vídeo cuando sea posible|1|x|x||
| workspaceid:s:value | Esta configuración define el RemoteApp y el identificador de escritorio asociado con el archivo RDP que contiene esta configuración. | Un Id. de la conexión de escritorio y una RemoteApp válidos|x|x||