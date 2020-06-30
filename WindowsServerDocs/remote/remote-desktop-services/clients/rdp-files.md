---
title: Configuración admitida del archivo RDP de Escritorio remoto
description: Obtén información sobre la configuración del archivo RDP para Escritorio remoto
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.topic: article
author: heidilohr
manager: lizross
ms.author: helohr
ms.date: 06/16/2020
ms.localizationpriority: medium
ms.openlocfilehash: 4606938a6c01e20c847b3a6c198de8a1c61c59f0
ms.sourcegitcommit: 5bc5aaf341c711113ca03d1482f933b05b146007
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/19/2020
ms.locfileid: "85094532"
---
# <a name="supported-remote-desktop-rdp-file-settings"></a>Configuración admitida del archivo RDP de Escritorio remoto

En la tabla siguiente se incluye la lista de las configuraciones admitidas del archivo RDP que puedes usar con los clientes de Escritorio remoto. Al configurar las opciones, compruebe [Comparación de los clientes](./remote-desktop-app-compare.md) para ver qué redirecciones admite cada cliente.

En la tabla también se resalta qué configuración se admite como propiedades personalizadas con Windows Virtual Desktop. Consulta [esta documentación](https://go.microsoft.com/fwlink/?linkid=2098243&clcid=0x409) para más información sobre cómo usar PowerShell para personalizar las propiedades de RDP de los grupos de hosts de Windows Virtual Desktop.

## <a name="connection-information"></a>Información de conexión

| Configuración de RDP                        | Descripción            | Valores                 | Valor predeterminado          | Windows Virtual Desktop |
|------------------------------------|------------------------|------------------------|:----------------------:|:-----------------------:|
| full address:s:value | Nombre del equipo.</br>Esta opción especifica el nombre o la dirección IP del equipo remoto al que quieres conectarte.</br></br>Esta es la única configuración obligatoria en un archivo RDP. | Un nombre, una dirección IPv4 o una dirección IPv6 válidos. | | |
| alternate full address:s:value | Especifica un nombre alternativo o la dirección IP del equipo remoto. | Un nombre, una dirección IPv4 o una dirección IPv6 válidos. | | |
| username:s:value | Especifica el nombre de la cuenta de usuario que se utilizará para iniciar sesión en el equipo remoto. | Cualquier nombre de usuario válido. | | |
| domain:s:value | Especifica el nombre del dominio donde se encuentra la cuenta de usuario que se usará para iniciar sesión en el equipo remoto. | Un nombre de dominio válido, como "CONTOSO". | | |
| gatewayhostname:s:value | Especifica el nombre de host de Puerta de enlace de Escritorio remoto. | Un nombre, una dirección IPv4 o una dirección IPv6 válidos. | | |
| gatewaycredentialssource:i:value | Especifica el método de autenticación de Puerta de enlace de Escritorio remoto. | - 0: Pedir la contraseña (NTLM)</br>- 1: Usar tarjeta inteligente</br>- 2: usar las credenciales del usuario que tiene la sesión iniciada actualmente.</br>- 3: pedir al usuario sus credenciales y usar la autenticación básica.</br>- 4: Permitir al usuario seleccionar más tarde</br>- 5: usar la autenticación basada en cookies. | 0 | |
| gatewayprofileusagemethod:i:value | Especifica si se debe usar la configuración predeterminada de Puerta de enlace de Escritorio remoto. | - 0: Usar el modo de perfil predeterminado, según lo especificado por el administrador</br>- 1: Usar la configuración explícita, según lo especificado por el usuario | 0 | |
| gatewayusagemethod:i:value | Especifica cuándo se debe utilizar Puerta de enlace de Escritorio remoto para la conexión. | - 0: no usar Puerta de enlace de Escritorio remoto.</br>- 1: usar siempre Puerta de enlace de Escritorio remoto.</br>- 2: usar Puerta de enlace de Escritorio remoto si no se puede establecer una conexión directa al host de sesión de Escritorio remoto.</br>- 3: usar la configuración predeterminada de Puerta de enlace de Escritorio remoto.</br>- 4: no usar Puerta de enlace de Escritorio remoto, omitir la puerta de enlace para las direcciones locales.</br>Establecer el valor de esta propiedad en 0 o 4 es equivalente, pero establecer esta propiedad en 4 habilita la opción de omitir las direcciones locales. | 0 | |
| promptcredentialonce:i:value | Establece si las credenciales del usuario se guardan y se usan para la Puerta de enlace de Escritorio remoto y el equipo remoto. | - 0: La sesión remota no usará las mismas credenciales</br>- 1: La sesión remota usará las mismas credenciales | 1 | |
| authentication level:i:value | Define la configuración del nivel de autenticación del servidor. | - 0: Si se produce un error de autenticación de servidor, conectar al equipo sin advertencia (Conectar y no avisarme)</br>- 1: Si se produce un error de autenticación de servidor, no establecer una conexión (No conectar)</br>- 2: Si se produce un error de autenticación de servidor, mostrar una advertencia y permitirme conectarme o rechazar la conexión (Avisarme)</br>- 3: No se especifica ningún requisito de autenticación. | 3 | |
| enablecredsspsupport:i:value | Determina si el cliente usará el proveedor de compatibilidad para seguridad de credenciales (CredSSP) para la autenticación si está disponible. | - 0: RDP no usará CredSSP, incluso si el sistema operativo es compatible con CredSSP</br>- 1: RDP usará CredSSP si el sistema operativo es compatible con CredSSP | 1 | X |
| disableconnectionsharing:i:value | Determina si el cliente se vuelve a conectar a una sesión desconectada existente o si inicia una nueva conexión cuando se lanza una nueva conexión. | - 0: Volver a conectar a cualquier sesión existente</br>- 1: Iniciar nueva conexión | 0 | X |
| alternate shell:s:value | Especifica que un programa se inicie automáticamente en la sesión remota como el shell en lugar del explorador. | Ruta de acceso válida a un archivo ejecutable, como "C:\ProgramFiles\Office\word.exe". | | X |

## <a name="session-behavior"></a>Comportamiento de la sesión.

| Configuración de RDP                        | Descripción            | Valores                 | Valor predeterminado          | Windows Virtual Desktop |
|------------------------------------|------------------------|------------------------|:----------------------:|:-----------------------:|
| autoreconnection enabled:i:value | Determina si el cliente intentará automáticamente volver a conectarse al equipo remoto si se interrumpe la conexión, como cuando hay una interrupción en la conectividad de red. | - 0: el equipo cliente no intenta volver a conectarse automáticamente.</br>- 1: el equipo cliente intenta volver a conectarse automáticamente. | 1 | X |
| bandwidthautodetect:i:value | Determina si la detección automática del tipo de red está habilitada | - 0: Deshabilitar la detección automática del tipo de red</br>- 1: Habilitar la detección automática del tipo de red | 1 | X |
| networkautodetect:i:value | Establece si se usa la detección automática de ancho de banda de red o no. Requiere que bandwidthautodetect se establezca en 1. | - 0: No usar la detección automática de ancho de banda de red</br> - 1: Usar la detección automática de ancho de banda de red | 1 | X |
| compression:i:value | Determina si la compresión masiva está habilitada cuando RDP la transmite al equipo local.|- 0: Deshabilitar la compresión masiva de RDP</br>- 1: Habilitar la compresión masiva de RDP | 1 | X |
| videoplaybackmode:i:value| Determina si la conexión usará streaming multimedia eficaz para RDP para la reproducción de vídeo.|- 0: No usar streaming multimedia eficaz para RDP para la reproducción de vídeo</br>- 1: Usar streaming multimedia eficaz para RDP para la reproducción de vídeo cuando sea posible | 1 | X |

## <a name="device-redirection"></a>Redireccionamiento de dispositivos

| Configuración de RDP                        | Descripción            | Valores                 | Valor predeterminado          | Windows Virtual Desktop |
|------------------------------------|------------------------|------------------------|:----------------------:|:-----------------------:|
| audiocapturemode:i:value | Redireccionamiento del micrófono.</br>Indica el redireccionamiento de la entrada de audio si está habilitado. | - 0: Deshabilitar la captura de audio desde el dispositivo local</br>- 1: Habilitar la captura de audio desde el dispositivo local y redireccionar a una aplicación de audio en la sesión remota | 0 | X |
| encode redirected video capture:i:value | Habilita o deshabilita la codificación del vídeo redirigido. | - 0: deshabilita la codificación del vídeo redirigido.</br>- 1: habilita la codificación del vídeo redirigido. | 1 | X |
| redirected video capture encoding quality:i:value | Controla la calidad del vídeo codificado. | - 0: vídeo de compresión alta. La calidad puede verse afectada cuando hay mucho movimiento. </br>- 1: compresión media.</br>- 2: vídeo de compresión baja con alta calidad de imagen. | 0 | X |
| audiomode:i:value | Ubicación de salida.</br>Determina si el equipo local o remoto reproduce audio. | - 0: reproducir sonidos en el equipo local (Reproducir en este equipo).</br>- 1: reproducir sonidos en el equipo remoto (Reproducir en equipo remoto).</br>- 2: No reproducir sonidos (No reproducir) | 0 | X |
| camerastoredirect:s:value | Redireccionamiento de la cámara.</br>Configura las cámaras que se redirigirán. Esta configuración usa una lista delimitada por signos de punto y coma de las interfaces KSCATEGORY_VIDEO_CAMERA de las cámaras habilitadas para el redireccionamiento. | - * : redirige todas las cámaras.</br> - Lista de las cámaras, como camerastoredirect:s:\\?\usb#vid_0bda&pid_58b0&mi</br>- Para excluir una cámara específica, se puede anteponer "-" a la cadena de vínculo simbólico. | No redirigir ninguna cámara. | X |
| devicestoredirect:s:value | Redireccionamiento de dispositivos USB.</br>Determina qué dispositivos del equipo local se redirigirán a la sesión remota y estarán disponibles en ella. | - *: redirigir todos los dispositivos compatibles, incluidos los que se conectan posteriormente</br> - Identificador de hardware válido de uno o varios dispositivos | No redirigir ningún dispositivo. | X |
| drivestoredirect:s:value | Redireccionamiento de unidades y almacenamiento.</br>Determina qué unidades de disco del equipo local se redirigirán a la sesión remota y estarán disponibles en ella. | - Ningún valor especificado: no redirigir ninguna unidad</br>- * : Redirigir todas las unidades de disco, incluidas las unidades que se conecten más adelante</br>- DynamicDrives: redirigir las unidades que se conecten más adelante</br>- La unidad y las etiquetas para una o más unidades, como "drivestoredirect:s:C:;E:;": redirigir las unidades especificadas | No redirigir ninguna unidad. | X |
| redirectclipboard:i:value | Redireccionamiento del Portapapeles.</br>Determina si el redireccionamiento del Portapapeles está habilitado. | - 0: El Portapapeles del equipo local no está disponible en la sesión remota</br>- 1: El Portapapeles del equipo local está disponible en la sesión remota | 1 | X |
| redirectprinters:i:value | Redirección de impresoras.</br>Determina si las impresoras configuradas en el equipo local se redirigirán a la sesión remota y estarán disponibles en ella. | - 0: Las impresoras en el equipo local no están disponibles en la sesión remota</br>- 1: Las impresoras en el equipo local están disponibles en la sesión remota | 1 | X |
| redirectsmartcards:i:value | Redireccionamiento de tarjetas inteligentes.</br>Determina qué dispositivos de tarjeta inteligente del equipo local se redirigirán a la sesión remota y estarán disponibles en ella. |- 0: El dispositivo de tarjeta inteligente en el equipo local no está disponible en la sesión remota</br>- 1: El dispositivo de tarjeta inteligente en el equipo local está disponible en la sesión remota | 1 | X |

## <a name="display-settings"></a>Configuración de pantalla

| Configuración de RDP                        | Descripción            | Valores                 | Valor predeterminado          | Windows Virtual Desktop |
|------------------------------------|------------------------|------------------------|:----------------------:|:-----------------------:|
| use multimon:i:value | Determina si la sesión remota usará una o varias pantallas del equipo local. | - 0: no habilitar la compatibilidad con varias pantallas.</br>- 1: habilitar la compatibilidad con varias pantallas. | 0 | X |
| selectedmonitors:s:value | Especifica qué pantallas locales se van a utilizar desde la sesión remota. Las pantallas seleccionadas deben ser contiguas. Requiere que use multimon se establezca en 1.</br></br>Solo está disponible en los clientes de Bandeja de entrada de Windows (MSTSC) y Escritorio de Windows (MSRDC). | Lista separada por comas de identificadores de pantalla específicos del equipo. Los identificadores se pueden recuperar llamando a mstsc.exe/l. El primer identificador que se muestra se establecerá como la pantalla principal en la sesión. | Todas las pantallas | X |
| maximizetocurrentdisplays:i:value | Determina el modo en que la sesión remota pasa a pantalla completa al maximizar. Requiere que use multimon se establezca en 1.</br></br>Solo está disponible en el cliente de Escritorio de Windows (MSRDC). | - 0: la sesión pasa a pantalla completa en las pantallas seleccionadas inicialmente al maximizar.</br>- 1: la sesión pasa de forma dinámica a pantalla completa en las pantallas tocadas por la ventana de sesión al maximizar. | 0 | X |
| singlemoninwindowedmode:i:value | Determina si una sesión remota con varias pantallas cambia automáticamente a una sola pantalla al salir del modo de pantalla completa. Requiere que use multimon se establezca en 1.</br></br>Solo está disponible en el cliente de Escritorio de Windows (MSRDC). | - 0: la sesión conserva todas las pantallas al salir del modo de pantalla completa.</br>- 1: la sesión cambia a una sola pantalla al salir del modo de pantalla completa. | 0 | X |
| screen mode id:i:value | Determina si la ventana de la sesión remota aparece a pantalla completa al iniciar la conexión. | - 1: La sesión remota aparecerá en una ventana</br>- 2: La sesión remota aparecerá en pantalla completa | 2 | X |
| smart sizing:i:value | Determina si el equipo local escala el contenido de la sesión remota para ajustarse al tamaño de la ventana. | - 0: el contenido de la ventana local no se escalará al cambiar de tamaño.</br>- 1: el contenido de la ventana local se escalará al cambiar de tamaño. | 0 | X |
| dynamic resolution:i:value | Determina si la resolución de la sesión remota se actualiza automáticamente cuando se cambia el tamaño de la ventana local. | - 0: la resolución de la sesión permanece estática mientras dure la sesión.</br>- 1: la resolución de la sesión se actualiza a medida que cambia el tamaño de la ventana local. | 1 | X |
| desktop size id:i:value | Especifica las dimensiones del escritorio de la sesión remota a partir de un conjunto de opciones predefinidas. Esta configuración se anula si se especifican desktopheight y desktopwidth.| -0: 640×480</br>- 1: 800×600</br>- 2: 1024×768</br>- 3: 1280×1024</br>- 4: 1600×1200 | 1 | X |
| desktopheight:i:value | Especifica el alto de la resolución (en píxeles) de la sesión remota. | Valor numérico entre 200 y 8192. | Coincide con el del equipo local. | X |
| desktopwidth:i:value | Especifica el ancho de la resolución (en píxeles) de la sesión remota. | Valor numérico entre 200 y 8192. | Coincide con el del equipo local. | X |
| desktopscalefactor:i:value | Especifica el factor de escala de la sesión remota para que el contenido aparezca más grande. | Valor numérico de la lista siguiente: 100, 125, 150, 175, 200, 250, 300, 400, 500 | 100 | X |

## <a name="remoteapp"></a>RemoteApp

| Configuración de RDP                        | Descripción            | Valores                 | Valor predeterminado          | Windows Virtual Desktop |
|------------------------------------|------------------------|------------------------|:----------------------:|:-----------------------:|
| remoteapplicationcmdline:s:value | Parámetros opcionales de línea de comandos para RemoteApp. | Parámetros de línea de comandos válidos. | | |
| remoteapplicationexpandcmdline:i:value | Determina si las variables de entorno contenidas en el parámetro de línea de comandos de RemoteApp se deben expandir local o remotamente. | - 0: Las variables de entorno deben expandirse a los valores del equipo local</br>- 1: las variables de entorno deben expandirse a los valores del equipo remoto. | 1 | |
| remoteapplicationexpandworkingdir:i:value | Determina si las variables de entorno contenidas en el directorio de trabajo de RemoteApp se deben expandir local o remotamente. | - 0: Las variables de entorno deben expandirse a los valores del equipo local</br> - 1: las variables de entorno deben expandirse a los valores del equipo remoto.</br>El directorio de trabajo de RemoteApp se especifica mediante el parámetro de shell del directorio de trabajo. | 1 | |
| remoteapplicationfile:s:value | Especifica un archivo que RemoteApp abrirá en el equipo remoto.</br>Para abrir los archivos locales, también debes habilitar la redirección de unidad para la unidad de origen. | Ruta de acceso de archivo válida. | | |
| remoteapplicationicon:s:value | Especifica el archivo de icono que se mostrará en la interfaz de usuario del cliente al iniciar una conexión de RemoteApp. Si no se especifica ningún nombre de archivo, el cliente usará el icono estándar de Escritorio remoto. Solo se admiten archivos ".ico". | Ruta de acceso de archivo válida. | | |
| remoteapplicationmode:i:value | Determina si una conexión se inicia como sesión de RemoteApp. | - 0: No iniciar una sesión de RemoteApp</br>- 1: Iniciar una sesión de RemoteApp | 1 | |
| remoteapplicationname:s:value | Especifica el nombre de RemoteApp en la interfaz del cliente al iniciar RemoteApp.| Nombre para mostrar de la aplicación. Por ejemplo, "Excel 2016". | | |
| remoteapplicationprogram:s:value | Especifica el nombre de alias o ejecutable de RemoteApp. | Nombre o alias válidos. Por ejemplo, "EXCEL". | | |
