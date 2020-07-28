---
title: Introducción al cliente de Microsoft Store
description: Los pasos básicos para la configuración del cliente de Escritorio remoto para Microsoft Store.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.topic: article
ms.assetid: 64f038e1-40ec-4c67-938b-72edea49e5d8
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 06/01/2020
ms.localizationpriority: medium
ms.openlocfilehash: 6ed87f2d03ef725c3efdfc2453b53201017e66e0
ms.sourcegitcommit: d99bc78524f1ca287b3e8fc06dba3c915a6e7a24
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/27/2020
ms.locfileid: "87181951"
---
# <a name="get-started-with-the-windows-store-client"></a>Introducción al cliente de Microsoft Store

>Se aplica a: Windows 10

El cliente de Escritorio remoto para Windows se puede usar con aplicaciones de Windows y escritorios de forma remota desde otro dispositivo de Windows.

Usa la siguiente información para comenzar. Asegúrate de consultar la sección de [Preguntas frecuentes](remote-desktop-client-faq.md) si te surge alguna duda.

> [!NOTE]
> - ¿Quieres más información acerca de las nuevas versiones del cliente de Microsoft Store? Consulta las [Novedades del cliente de Microsoft Store](windows-whatsnew.md)
> - El cliente se puede ejecutar en cualquier versión compatible de Windows 10.

## <a name="get-the-rd-client-and-start-using-it"></a>Obtención del cliente de Escritorio remoto y primer uso

Sigue estos pasos para empezar a usar Escritorio remoto en un dispositivo con Windows 10:

1. Descarga el cliente de Escritorio remoto de [Microsoft Store](https://www.microsoft.com/store/p/microsoft-remote-desktop/9wzdncrfj3ps).
2. [Configura tu equipo para que acepte conexiones remotas](remote-desktop-allow-access.md).
3. Agrega una conexión a Escritorio remoto o un recurso remoto. Usa una conexión para conectarse directamente a un equipo Windows y un recurso remoto para usar un programa de RemoteApp, un escritorio basado en sesión o un escritorio virtual publicado por el administrador.
4. Ancla los elementos para poder acceder a Escritorio remoto rápidamente.

### <a name="add-a-remote-desktop-connection"></a>Adición de una conexión a Escritorio remoto

Para crear una conexión a Escritorio remoto:

1. En Connection Center (Centro de conexión), pulse **+ Agregar** y, después, **Escritorio**.
2. Escribe la siguiente información del equipo al que quieres conectarte:
   - **Nombre del equipo**: el nombre del equipo. Puede ser un nombre de equipo Windows, un nombre de dominio de Internet o una dirección IP. También puedes anexar la información del puerto al nombre de equipo (por ejemplo, **MyDesktop:3389** o **10.0.0.1:3389**).
   - **Cuenta de usuario**: la cuenta de usuario que se utiliza para acceder al equipo remoto. Pulsa **+** para agregar una cuenta nueva o selecciona una cuenta existente. Puedes usar los siguientes formatos para el nombre de usuario: *nombre_de_usuario*, *dominio\nombre_de_usuario* o <em>user_name@domain.com</em>. También puede especificar si se deben solicitar credenciales durante la conexión. Para ello, debe seleccionar la opción **Preguntar cada vez**.
3. También puede establecer opciones adicionales pulsando en **Mostrar más**:
   - **Nombre para mostrar**: un nombre fácil de recordar para el equipo al que se va a conectar. Puede utilizar cualquier cadena, pero si no especifica ningún nombre descriptivo, se mostrará el nombre del equipo.
   - **Grupo**: especifica un grupo para que sea más fácil encontrar las conexiones posteriormente. Para agregar un grupo, pulsa **+** o selecciónalo en la lista.
   - **Puerta de enlace**: la puerta de enlace de Escritorio remoto que quieres usar para conectarte a escritorios virtuales, programas RemoteApp y escritorios basados en sesión de una red corporativa interna. La información acerca de la puerta de enlace te la puede proporcionar el administrador del sistema.
   - **Conectar a sesión del administrador**: esta opción se utiliza para conectarse a una sesión de consola para administrar un servidor de Windows.
   - **Intercambiar botones del mouse**: esta opción se usa para cambiar las funciones del botón izquierdo del ratón al botón derecho. Dicho intercambio es necesario cuando usa un equipo configurado para un usuario zurdo, pero solo dispone de un mouse para usuarios diestros.
   - **Establecer la resolución de la sesión remota en:** : selecciona la resolución que quieres utilizar en la sesión. **Elegir por mí** establecerá la resolución en función del tamaño del cliente.
   - **Change the size of the display** (Cambiar el tamaño de la pantalla): si selecciona una resolución estática alta para la sesión, puede usar esta configuración para que los elementos de la pantalla parezcan mayores, con el fin de mejorar la legibilidad. Esta configuración solo se aplica cuando se conecta a Windows 8.1 o una versión posterior.
   - **Actualizar la resolución de la sesión remota al cambiar tamaño**: si se habilita, el cliente actualizará dinámicamente la resolución de la sesión en función del tamaño del cliente. Esta configuración solo se aplica cuando se conecta a Windows 8.1 o una versión posterior.
   - **Portapapeles** : cuando está habilitado, permite tanto copiar texto e imágenes al equipo remoto como desde este.
   - **Reproducción de audio**: selecciona el dispositivo que vas a usar para el audio durante la sesión remota. Puedes elegir reproducir el sonido en los dispositivos locales, en un equipo remoto o no reproducirlo.
   - **Grabación de audio** : cuando está habilitada, permite usar un micrófono local con las aplicaciones en el equipo remoto.
4. Pulsa **Guardar**.

¿Es necesario editar esta configuración? Pulsa el menú de desbordamiento ( **...** ) junto al nombre del escritorio y, después, pulsa **Editar**.

¿Quieres eliminar la conexión? Vuelve a pulsar el menú de desbordamiento ( **...** ) y, después, pulsa **Quitar**.

### <a name="add-a-remote-resource"></a>Adición de recursos remotos

Los recursos remotos son programas RemoteApp, escritorios basados en sesión y escritorios virtuales publicados por el administrador mediante Servicios de Escritorio remoto.

Para agregar un recurso remoto:

1. En la pantalla del Centro de conexión, pulsa **+ Agregar** y, después, pulsa **Recursos remotos**.
2. Escribe la **URL de fuente**, que te ha proporcionado el administrador, y pulsa **Buscar fuentes**.
3. Cuando se le soliciten las credenciales, especifíquelas para suscribirse a la fuente.

Los recursos remotos se mostrarán en el Centro de conexión.

Para eliminar los recursos remotos:

1. En el Centro de conexión, pulsa el menú de desbordamiento ( **...** ) que hay al lado del recurso remoto.
2. Pulsa **Quitar**.

### <a name="pin-a-saved-desktop-to-your-start-menu"></a>Anclaje de un escritorio guardado al menú Inicio

Para anclar una conexión al menú Inicio, pulsa el menú de desbordamiento ( **...** ) que hay al lado del nombre del escritorio y, después, pulsa **Anclar a Inicio**.

Ya puedes iniciar la conexión a Escritorio remoto directamente desde el menú Inicio pulsando en él.

## <a name="connect-to-an-rd-gateway-to-access-internal-assets"></a>Conexión a una puerta de enlace de Escritorio remoto para acceder a recursos internos

Las puertas de enlace de Escritorio remoto te permiten conectarte a equipos remotos de una red corporativa desde cualquier lugar de Internet. Para crear y administrar dichas puertas de enlace, usa el cliente de Escritorio remoto.

Para configurar una puerta de enlace nueva:

1. En el Centro de conexión, pulsa **Configuración**.
2. Junto a Puerta de enlace, pulsa **+** para agregar una nueva puerta de enlace.

      >[!NOTE]
      >También puede agregar una puerta de enlace cuando agregue una nueva conexión.

3. Escribe la siguiente información:
   - **Nombre del servidor**: el nombre del equipo que quieres usar como puerta de enlace. El nombre del servidor puede ser un nombre de equipo Windows, un nombre de dominio de Internet o una dirección IP. También puedes agregar información del puerto al nombre del servidor (por ejemplo: **RDGateway:443** o **10.0.0.1:443**).
   - **Cuenta de usuario**: seleccione o agregue una cuenta de usuario para usar con la Puerta de enlace de Escritorio remoto a la que se va a conectar. También puede seleccionar **Use desktop user account** (Usar la cuenta de usuario de escritorio) para utilizar las mismas credenciales que usó para la Conexión a Escritorio remoto.
4. Pulsa **Guardar**.

## <a name="global-app-settings"></a>Configuración de aplicaciones global

Para establecer la siguiente configuración global en el cliente, pulsa **Configuración**:

### <a name="managed-items"></a>Elementos administrados

- **Cuenta de usuario**: permite agregar, editar y eliminar cuentas de usuario guardadas en el cliente. También puede actualizar la contraseña de una cuenta después de que haya cambiado.
- **Puerta de enlace**: permite agregar, editar y eliminar servidores de puerta de enlace guardados en el cliente.
- **Grupo**: permite agregar, editar y eliminar grupos guardados en el cliente. También puede agrupar las conexiones aquí.

### <a name="session-settings"></a>Configuración de sesión

- **Iniciar conexiones en modo de pantalla completa**: si se habilita, cada vez que se inicie una conexión, el cliente utilizará toda la pantalla del monitor actual.
- **Empezar cada conexión en una ventana nueva**: si se habilita, cada conexión se inicia en una ventana independiente, lo que permite colocarlas en distintos monitores y cambiar de una a otra desde la barra de tareas.
- **Al cambiar el tamaño de la aplicación:** : permite controlar lo que sucede cuando se cambia el tamaño de la ventana del cliente. El valor predeterminado es **Ampliar el contenido, manteniendo la relación de aspecto**.
- **Use keyboard commands with** (Usar comandos de teclado con): permite especificar dónde se usan comandos del teclado como *WIN* o *ALT+TAB*. El valor predeterminado es que se envíen solo a la sesión cuando la conexión esté en pantalla completa.
- **Impedir que se agote el tiempo de espera de la pantalla**: permite que la pantalla nunca supere el tiempo de espera cuando una sesión está activa. Resulta útil cuando la conexión no requiere ninguna interacción durante períodos prolongados.

### <a name="app-settings"></a>Configuración de aplicaciones

- **Mostrar vistas previas de escritorio**: permite obtener una vista previa de un escritorio en el Centro de conexión antes de conectarse a él. Esta configuración está activada de manera predeterminada.
- **Ayudar a mejorar Escritorio remoto**: envía datos anónimos a Microsoft. Estos datos se usan para mejorar el cliente. Para obtener más información sobre cómo tratamos estos datos privados y anónimos, consulte la [Declaración de privacidad de Microsoft](https://privacy.microsoft.com/privacystatement). Esta configuración está activada de manera predeterminada.

### <a name="manage-your-user-accounts"></a>Administración de cuentas de usuario

Cuando se conecte a un escritorio o a recursos remotos, puede guardar la información de la cuenta para conectarse a ella más adelante. También puede definir cuentas de usuario en el cliente, en lugar de guardar los datos del usuario al conectarse a un escritorio.

Para crear una cuenta de usuario:

1. En el Centro de conexión, pulsa **Configuración**.
2. Junto a Cuenta de usuario, pulsa **+** para agregar una cuenta de usuario nueva.
3. Escribe la siguiente información:
   - **Nombre de usuario**: el nombre del usuario que se guarda para usarlo con una conexión remota. El nombre de usuario se puede escribir en cualquiera de los siguientes formatos: nombre_de_usuario, dominio\nombre_de_usuario user_name@domain.com.
   - **Contraseña**: la contraseña del usuario que has especificado. Puede dejar este campo en blanco si quiere que se le solicite la contraseña durante la conexión.
4. Pulsa **Guardar**.

Para eliminar una cuenta de usuario:

1. En el Centro de conexión, pulsa **Configuración**.
2. Selecciona la cuenta que quieres eliminar en la lista de Cuenta de usuario.
3. Junto a Cuenta de usuario, pulsa el icono de edición.
4. Pulsa **Quitar esta cuenta** en la parte inferior para eliminar la cuenta de usuario.
5. También puedes editar la cuenta de usuario y pulsar **Guardar**.

## <a name="navigate-the-remote-desktop-session"></a>Desplazamiento a la sesión de Escritorio remoto

Al iniciar una conexión con Escritorio remoto, hay varias herramientas disponibles que se pueden usar para desplazarse por la sesión.

### <a name="start-a-remote-desktop-connection"></a>Inicio de una conexión a Escritorio remoto

1. Pulsa en la conexión con Escritorio remoto para iniciar la sesión.
2. Si no ha guardado las credenciales de la conexión, se le solicitará que escriba un **nombre de usuario** y una **contraseña**.
3. Si se le pide que verifique el certificado de Escritorio remoto, revise la información y asegúrese de que confía en este equipo antes de pulsar **Conectar**. También puedes seleccionar **Don’t ask about this certificate again** (No volver a pedir este certificado) para aceptarlo siempre.

### <a name="connection-bar"></a>Barra de conexión

La barra de conexión te permite acceder a más controles de navegación. De manera predeterminada, la barra de conexión se coloca en la parte central de la parte superior de la pantalla. Pulsa la barra y arrástrala hacia la izquierda o derecha para moverla.

- **Pan Control** (Control de panorámica): el control de panorámica permite ampliar la pantalla y moverse alrededor de ella. Solo está disponible en los dispositivos de entrada táctil y cuando se usa el modo de toque directo.
   - Para habilitarlo o deshabilitarlo, pulse el icono de panorámica de la barra de conexión a fin de mostrar dicho control. La pantalla se acercará mientras el control de panorámica esté activo. Vuelve a pulsar el icono de panorámica en la barra de conexión nuevo para ocultar el control y devolver la pantalla a su resolución original.
   - Para usar el control de panorámica, manténgalo pulsado y, a continuación, arrástrelo en la dirección en que quiera mover la pantalla.
   - Para mover el control de panorámica, púlselo dos veces y manténgalo pulsado a fin de mover el control en la pantalla.
- **Opciones adicionales**: pulse el icono de opciones adicionales para mostrar la barra de selección y la barra de comandos de la sesión.
- **Teclado**: pulsa el icono del teclado para mostrar u ocultar el teclado en pantalla. El control de panorámica se muestra automáticamente cuando se muestra el teclado.

### <a name="command-bar"></a>Barra de comandos

Pulse el botón **...** en la barra de conexión para mostrar la barra de comandos en el lado derecho de la pantalla.

- **Inicio**: usa el botón Inicio para volver al Centro de conexión desde la barra de comandos.
  - También puede usar el botón Atrás para realizar la misma acción. Si usa el botón Atrás, la sesión activa no se desconectará, lo que le permitirá iniciar conexiones adicionales.
- **Desconectar**: use el botón Desconectar para desconectarse de la sesión. Las aplicaciones permanecerán activas mientras la sesión siga activa en el equipo remoto.
- **Pantalla completa** : entra o sale del modo de pantalla completa.
- **Touch or Mouse** (Función táctil o mouse): puede cambiar entre los modos de mouse (puntero y toque directo).

### <a name="use-direct-touch-gestures-and-mouse-modes-in-a-remote-session"></a>Uso de gestos de toque directo y de modos de ratón en una sesión remota

Existen dos modos de ratón para interactuar con la sesión.

- **Toque directo**: pasa todos los contactos táctiles a la sesión para que se interpreten de forma remota.
  - Se usa en la misma manera que utilizaría Windows con una pantalla táctil.
- **Puntero**: Transforma la pantalla táctil local en un panel táctil grande que permite mover el puntero del mouse en la sesión.
  - Se usa en la misma manera que usarías Windows con un panel táctil.

> [!NOTE]
> En Windows 8 o una versión posterior, los gestos táctiles nativos se admiten en el modo de toque directo.

| Modo del mouse    | Operación del ratón      | Gesto                                                               |
|---------------|----------------------|-----------------------------------------------------------------------|
| Toque directo  | Clic con el botón izquierdo           | Pulsar con un dedo                                                          |
| Toque directo  | Haga clic con el botón secundario en          | Mantener pulsado con un dedo                                                |
| Puntero | Clic con el botón izquierdo           | Pulsar con un dedo                                                          |
| Puntero | Hacer clic con el botón izquierdo y arrastrar  | Pulsar dos veces con un dedo y mantener pulsado y, a continuación, arrastrar                               |
| Puntero | Haga clic con el botón secundario en          | Pulsar con dos dedos                                                          |
| Puntero | Clic con el botón derecho y arrastrar | Pulsar dos veces con dos dedos y mantener pulsado y, a continuación, arrastrar                              |
| Puntero | Rueda del ratón          | Mantener pulsado con dos dedos y, a continuación, arrastrar hacia arriba o hacia abajo.                          |
| Puntero | Zoom                 | Con dos dedos, acérquelos para acercar el zoom y sepárelos para alejar. |

> [!TIP]
> Tanto las preguntas como los comentarios son siempre bienvenidos. Sin embargo, si publica solicitudes de soporte técnico o comentarios sobre el producto en la sección Comentarios de este artículo, no podremos responder a sus comentarios. Si necesita ayuda o quiere solucionar problemas con su cliente, le recomendamos que vaya al [foro de clientes de Escritorio remoto](https://docs.microsoft.com/answers/topics/windows-remote-desktop-client.html) e inicie una nueva conversación. Si tiene alguna sugerencia de características, puede indicárnosla mediante el [Centro de opiniones](feedback-hub://?tabid=2&contextid=605).
