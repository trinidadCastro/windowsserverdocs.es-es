---
title: Introducción a Escritorio remoto en Windows
description: Basic pasos de configuración para el cliente de escritorio remoto para Windows.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 64f038e1-40ec-4c67-938b-72edea49e5d8
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 05/07/2018
ms.localizationpriority: medium
ms.openlocfilehash: 20879507e13ac7da566c95db7b59d88e0b5d8ce8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873576"
---
# <a name="get-started-with-remote-desktop-on-windows"></a>Introducción a Escritorio remoto en Windows

>Se aplica a: Windows 10, Windows 8.1, Windows Server 2012 R2, Windows Server 2016

Puede usar al cliente de escritorio remoto para Windows para trabajar con aplicaciones de Windows y escritorios de forma remota desde otro dispositivo de Windows.

Use la siguiente información para empezar a trabajar. Asegúrese de consultar la [preguntas más frecuentes sobre](remote-desktop-client-faq.md) si tiene alguna pregunta.

> [!NOTE]
> - ¿Tiene curiosidad acerca de las nuevas versiones para el cliente de Windows? ¿Consulte [Novedades de escritorio remoto en Windows?](windows-whatsnew.md)
> - Puede ejecutar al cliente en cualquier versión de Windows 10.

## <a name="get-the-rd-client-and-start-using-it"></a>Obtener al cliente de escritorio remoto y empezar a usar

Siga estos pasos para empezar a trabajar con Escritorio remoto en el dispositivo Windows 10:

1. Descargue el cliente de escritorio remoto desde [Microsoft Store](https://www.microsoft.com/store/p/microsoft-remote-desktop/9wzdncrfj3ps). 
2. [Configuración de su PC para aceptar conexiones remotas](remote-desktop-allow-access.md).
3. Agregar una conexión a Escritorio remoto o un recurso remoto. Usar una conexión para conectarse directamente a un equipo de Windows y un recurso remoto para usar un programa de RemoteApp, basados en sesión de escritorio o un escritorio virtual publicadas por el administrador. 
4. Anclar elementos para que pueda empezar rápidamente a Escritorio remoto.

### <a name="add-a-remote-desktop-connection"></a>Agregar una conexión a Escritorio remoto

Para crear una conexión a Escritorio remoto:

1. En la derivación de centro de conexiones **+ agregar**y, a continuación, puntee en **Desktop**.
2. Escriba la siguiente información para el equipo que desea conectarse:
  - **Nombre de equipo** : el nombre del equipo. Esto puede ser un nombre de equipo de Windows, un nombre de dominio de Internet o una dirección IP. También puede anexar información del puerto en el nombre de equipo (por ejemplo, **MyDesktop:3389** o **10.0.0.1:3389**).
  - **Cuenta de usuario** : la cuenta de usuario a usar para obtener acceso al equipo remoto. Pulse **+** para agregar una cuenta nueva o seleccione una cuenta existente. Puede usar los siguientes formatos para el nombre de usuario: *user_name*, *DOMINIO\nombre_de_usuario*, o *user_name@domain.com*. También puede especificar si se debe solicitar un nombre de usuario y contraseña durante la conexión seleccionando **Preguntarme cada vez**.
3. También puede establecer opciones adicionales al pulsar **mostrar más**:
  - **Nombre para mostrar** : un nombre fácil de recordar para el equipo se conecta a. Puede utilizar cualquier cadena, pero si no especifica un nombre descriptivo, se muestra el nombre de equipo.
  - **Grupo** : especificar un grupo para que sea más fácil encontrar sus conexiones más adelante. Puede agregar un nuevo grupo, puntee en **+** o seleccione uno de la lista.
  - **Puerta de enlace** : puerta de enlace de escritorio remoto el que desea usar para conectarse a escritorios virtuales, programas RemoteApp y escritorios basados en una red corporativa interna. Obtenga la información acerca de la puerta de enlace desde el administrador del sistema.
  - **Conectarse a la sesión de administrador** -Utilice esta opción para conectarse a una sesión de consola para administrar un servidor de Windows.
  - **Intercambiar botones del mouse** : Use esta opción para cambiar el primario del mouse funciones del botón para el botón secundario del mouse. (Esto es especialmente útil si el equipo remoto está configurado para un usuario zurdo pero usa un mouse diestro).
  - **Configure la resolución de mi sesión remota en:** : seleccione la resolución que desee utilizar en la sesión. **Elija para mí** establecerá la resolución en función del tamaño del cliente.
  - **Cambiar el tamaño de la pantalla:** : al seleccionar una alta resolución estática para la sesión, tiene la opción para que los elementos en la pantalla parezcan de mayor tamaño para mejorar la legibilidad. Nota: Esto solo se aplica cuando se conecta a Windows 8.1 o versiones posteriores.
  - **Actualizar la resolución de la sesión remota en el cambio de tamaño** : cuando se habilita, el cliente actualizará dinámicamente la resolución de la sesión en función del tamaño del cliente. Nota: Esto solo se aplica cuando se conecta a Windows 8.1 o versiones posteriores.
  - **Portapapeles** : cuando está habilitado, le permite copiar texto e imágenes hacia y desde el equipo remoto.
  - **Reproducción de audio** : seleccione el dispositivo que se usará para audio durante la sesión remota. Puede reproducir un sonido en los dispositivos locales, el equipo remoto, o no en absoluto.
  - **Grabación de audio** : cuando está habilitada, permite usar un micrófono local con las aplicaciones en el equipo remoto.
4. Pulse **guardar**.

¿Es necesario modificar esta configuración? Puntee en el menú de desbordamiento (**...** ) junto al nombre del escritorio y, a continuación, en **editar**.

¿Desea eliminar la conexión? De nuevo, puntee en el menú de desbordamiento (**...** ) y, a continuación, puntee en **quitar**.

### <a name="add-a-remote-resource"></a>Agregar un recurso remoto
Los recursos remotos son programas RemoteApp, escritorios basados en sesión y escritorios virtuales publicados por el Administrador de servicios de escritorio remoto.

Para agregar un recurso remoto:

1. En la pantalla del centro de conexiones, pulse **+ agregar**y, a continuación, puntee en **recursos remotos**. 
2. Escriba el **dirección URL de fuente** proporcionada por el administrador y pulse **buscar fuentes**.
3. Cuando se le solicite, proporcione las credenciales para suscribirse a la fuente.

Los recursos remotos se mostrará en el centro de conexiones.


Para eliminar los recursos remotos:

1. En el centro de conexiones, pulse el menú de desbordamiento (**...** ) junto al recurso remoto.
2. Pulse **quitar**.

### <a name="pin-a-saved-desktop-to-your-start-menu"></a>Anclar un escritorio guardado en el menú Inicio

Para anclar una conexión con el menú Inicio, puntee en el menú de desbordamiento (**...** ) junto al nombre del escritorio y, a continuación, en **anclar a inicio**.

Ahora puede iniciar la conexión a escritorio remota directamente desde el menú Inicio, puntee en él.

## <a name="connect-to-an-rd-gateway-to-access-internal-assets"></a>Conectarse a una puerta de enlace de escritorio remoto para tener acceso a recursos internos

Una puerta de enlace de escritorio remoto (puerta de enlace de RD) le permite conectarse a un equipo remoto en una red corporativa desde cualquier lugar en Internet. Puede crear y administrar las puertas de enlace mediante el cliente de escritorio remoto.

Para configurar una puerta de enlace:

1. En el centro de conexiones, pulse **configuración**.
2. Junto a la puerta de enlace, pulse **+** para agregar una nueva puerta de enlace. Nota: También se puede agregar una puerta de enlace cuando se agrega una nueva conexión.
3. Escriba la siguiente información:
  - **Nombre del servidor** : el nombre del equipo que desea usar como una puerta de enlace. Esto puede ser un nombre de equipo de Windows, un nombre de dominio de Internet o una dirección IP. También puede agregar información de puerto al nombre del servidor (por ejemplo: **RDGateway:443** o **10.0.0.1:443**).
  - **Cuenta de usuario** : seleccione o agregue una cuenta de usuario para usarla con la puerta de enlace de escritorio remoto se conecta a. También puede seleccionar **usar cuenta de usuario de escritorio** para usar las mismas credenciales que se utilizaron para la conexión a escritorio remota.
4. Pulse **guardar**.  

## <a name="global-app-settings"></a>Configuración de la aplicación global

Puede establecer la configuración global siguiente en el cliente pulsando **configuración**:

ADMINISTRA LOS ELEMENTOS
- **Cuenta de usuario** : permite agregar, editar y eliminar cuentas de usuario que se guardan en el cliente. Esto es una buena forma de actualizar la contraseña de una cuenta después de ha cambiado.
- **Puerta de enlace** : permite agregar, editar y eliminar servidores de puerta de enlace que se guardan en el cliente.
- **Grupo** : permite agregar, editar y eliminar grupos guardados en el cliente. Estos le permiten fácilmente las conexiones de grupo.

CONFIGURACIÓN DE LA SESIÓN
- **Iniciar conexiones en pantalla completa** -cuando se habilita, cada vez que se inicia una conexión, el cliente utilizará toda la pantalla del monitor actual.
- **Iniciar cada conexión de una nueva ventana** -cuando se habilita, cada conexión se inicia en una ventana independiente, lo que permite colocarlos en distintos monitores y cambiar entre ellos mediante la barra de tareas.
- **Al cambiar el tamaño de la aplicación:** -permite controlar lo que ocurre cuando se cambia el tamaño de la ventana del cliente. El valor predeterminado es **estirar el contenido, conservando la relación de aspecto**.
- **Use los comandos del teclado con:** -permite especificar que los comandos de teclado como *ganar* o *ALT+TAB* se usan. El valor predeterminado es que les envíe solo a la sesión cuando la conexión está en pantalla completa.
- **Evitar que la pantalla de tiempo de espera** -le permite mantener la pantalla de tiempo de espera cuando una sesión está activa. Esto resulta útil cuando la conexión no requiere ninguna interacción durante largos períodos de tiempo.

CONFIGURACIÓN DE LA APLICACIÓN
- **Mostrar vistas previas de escritorio** -le permite obtener una vista previa de un escritorio en el centro de conexiones antes de conectarse a él. De forma predeterminada, se establece en **en**.
- **Ayude a mejorar el escritorio remoto** -envía datos anónimos a Microsoft. Usamos estos datos para mejorar al cliente. Para más información acerca de cómo se tratan estos datos anónimos, privados, consulte el [declaración de privacidad de Microsoft](https://privacy.microsoft.com/en-us/privacystatement). De forma predeterminada, esta configuración es **en**.

### <a name="manage-your-user-accounts"></a>Administrar las cuentas de usuario

Cuando se conecta a un recursos remotos o de escritorio, puede guardar las cuentas de usuario para seleccionar de nuevo. También puede definir las cuentas de usuario en el cliente, en lugar de guardar los datos de usuario cuando se conecta a un equipo de escritorio.

Para crear una nueva cuenta de usuario:

1. En el centro de conexiones, pulse **configuración**.
2. Junto a la cuenta de usuario, pulse **+** para agregar una nueva cuenta de usuario.
3. Escriba la siguiente información:
   - **Nombre de usuario** -el nombre del usuario para guardar para su uso con una conexión remota. Puede escribir el nombre de usuario en cualquiera de los siguientes formatos: user_name, DOMINIO\nombre_de_usuario, o user_name@domain.com.
   - **Contraseña** -la contraseña para el usuario especificado. Esto puede dejarse en blanco que se le solicite una contraseña durante la conexión.
4. Pulse **guardar**.

Para eliminar una cuenta de usuario:

1. En el centro de conexiones, pulse **configuración**.
2. Seleccione la cuenta para eliminar de la lista bajo la cuenta de usuario.
3. Junto a la cuenta de usuario, pulse el icono de edición.
4. Pulse **quitar esta cuenta** en la parte inferior para eliminar la cuenta de usuario.
5. También puede editar la cuenta de usuario y pulse **guardar**.

## <a name="navigate-the-remote-desktop-session"></a>Navegar por la sesión de escritorio remoto
Cuando se inicia una conexión a escritorio remota, hay herramientas disponibles que puede usar para navegar por la sesión.

### <a name="start-a-remote-desktop-connection"></a>Iniciar una conexión a Escritorio remoto

1. Puntee en la conexión a Escritorio remoto para iniciar la sesión.
2. Si no ha guardado las credenciales para la conexión, se le pedirá que proporcione un **Username** y **contraseña**.
3. Si se le pide que compruebe el certificado para el escritorio remoto, revise la información y asegurarse de que se trata de un equipo de confianza antes de pulsar **Connect**. También puede seleccionar **no volver a preguntar sobre este certificado** siempre Aceptar este certificado.

### <a name="connection-bar"></a>Barra de conexión

La conexión barra da como resultado que el acceso a los controles de navegación adicionales. De forma predeterminada, la barra de conexión se coloca en la parte central en la parte superior de la pantalla. Pulse y arrastre la barra hacia la izquierda o derecha para moverlo.

- **Control de panorámica** -el control de panorámica permite a se amplía y se mueven por la pantalla. Tenga en cuenta que el control de panorámica solo está disponible en dispositivos táctiles y utilizando el modo de toque directo con.
   - Habilitar o deshabilitar el control de panorámica: Pulse el icono de panorámica en la barra de conexión para mostrar el control de panorámica y zoom en la pantalla. Pulse el icono de panorámica en la barra de conexión nuevo para ocultar el control y devolver la pantalla para su resolución original.
   - Usar el control de panorámica - pulse y mantenga el control de panorámica y, a continuación, arrastre en la dirección que desea mover la pantalla.
   - Mueva el control panorámico - doble punteo y mantenga el control de panorámica para mover el control en la pantalla.
- **Opciones adicionales** -pulse el icono de opciones adicionales para mostrar la selección de la sesión de la barra y comando barra (ver abajo).
- **Teclado** -pulse el icono de teclado para mostrar u ocultar el teclado en pantalla. El control de panorámica se muestra automáticamente cuando se muestra el teclado.

### <a name="command-bar"></a>Barra de comandos

Pulse el **...**  en la barra de conexión para mostrar la barra de comandos en el lado derecho de la pantalla.

- **Inicio** -Use el botón de inicio para volver al centro de conexión de la barra de comandos.
  - También puede usar el botón Atrás para la misma acción.
  - No se desconectará la sesión activa.
  - Esto le permite iniciar conexiones adicionales.
- **Desconectar** -Use el botón de desconexión para terminar la conexión.
  - Las aplicaciones permanecerá activas mientras no finalice la sesión en el equipo remoto.
- **Pantalla completa** : entra o sale del modo de pantalla completa.
- **Táctil o Mouse** -puede cambiar entre los modos del ratón (puntero del Mouse y toque directo).

### <a name="use-direct-touch-gestures-and-mouse-modes-in-a-remote-session"></a>Uso directo touch gestos y modos de mouse (ratón) en una sesión remota

Existen dos modos de mouse interactuar con la sesión.
- **Dirigir el toque**: Pasa todos los contactos táctiles para la sesión se interpreta de forma remota.
  - Se usa en la misma manera que utilizaría Windows con una pantalla táctil.
- **Puntero del mouse**: Transforma la pantalla táctil local en un panel táctil grande que permite mover el puntero del mouse en la sesión.
  - Se usa en la misma manera que utilizaría Windows con un panel táctil.

> [!NOTE]
> Interactúan con Windows 8 o versiones más recientes se admiten los gestos táctiles nativas en toque directo con el modo. 

| Modo de mouse    | Operación del mouse      | Gesto                                                               |
|---------------|----------------------|-----------------------------------------------------------------------|
| Toque directo con  | El botón primario           | Pulse 1 dedo                                                          |
| Toque directo con  | Hacer clic con el botón derecho          | 1 tap dedo y suspensión                                                 |
| Puntero del mouse | El botón primario           | Pulse 1 dedo                                                          |
| Puntero del mouse | Hacer clic con el botón primario y arrastrar  | pulsar dos veces 1 dedo y mantener presionado, arrastre                               |
| Puntero del mouse | Hacer clic con el botón derecho          | Pulse dedo 2                                                          |
| Puntero del mouse | Hacer clic con el botón derecho y arrastrar | 2 pulsar dos veces dedo y mantener presionado, arrastre                               |
| Puntero del mouse | Rueda del mouse          | dedo 2 pulse y mantenga presionado, y luego arrastre hacia arriba o hacia abajo                           |
| Puntero del mouse | Zoom                 | Utilice 2 dedos y acerca los dedos para acercar o mover separar los dedos para alejar. |

> [!TIP]
> Preguntas y comentarios siempre son bienvenidos. Sin embargo, no publique una solicitud de ayuda para solucionar problemas con la característica de comentario al final de este artículo. En su lugar, vaya a la [foro de cliente de escritorio remoto](https://social.technet.microsoft.com/forums/windowsserver/en-us/home?forum=winrdc) e iniciar un nuevo subproceso. ¿Tiene alguna sugerencia de característica? Cuéntenos utilizando el [centro comentarios](feedback-hub://?tabid=2&contextid=605).
