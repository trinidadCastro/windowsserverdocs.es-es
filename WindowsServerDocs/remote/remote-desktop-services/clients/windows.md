---
title: Introducción al cliente de Microsoft Store
description: Las instrucciones básicas para la configuración del cliente de Escritorio remoto para Microsoft Store.
ms.topic: article
ms.assetid: 64f038e1-40ec-4c67-938b-72edea49e5d8
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 09/17/2020
ms.localizationpriority: medium
ms.openlocfilehash: 07e778fef6a944107ab9cb66f06c8c5d980f3675
ms.sourcegitcommit: 877d6db73d9520e3a23738d6528016235493cff3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2020
ms.locfileid: "90779259"
---
# <a name="get-started-with-the-microsoft-store-client"></a>Introducción al cliente de Microsoft Store

>Se aplica a: Windows 10

El cliente de Escritorio remoto para Windows se puede usar con PC y aplicaciones de Windows de forma remota desde otro dispositivo Windows.

Usa la siguiente información para comenzar. Asegúrate de consultar la sección de [Preguntas frecuentes](remote-desktop-client-faq.md) si te surge alguna duda.

> [!NOTE]
> - ¿Quiere más información acerca de las nuevas versiones del cliente de Microsoft Store? Consulte [Novedades del cliente de Microsoft Store](windows-whatsnew.md).
> - El cliente se puede ejecutar en cualquier versión compatible de Windows 10.

## <a name="get-the-rd-client-and-start-using-it"></a>Obtención del cliente de Escritorio remoto y primer uso

Sigue estos pasos para empezar a usar Escritorio remoto en un dispositivo con Windows 10:

1. Descarga el cliente de Escritorio remoto de [Microsoft Store](https://www.microsoft.com/store/p/microsoft-remote-desktop/9wzdncrfj3ps).
2. [Configura tu equipo para que acepte conexiones remotas](remote-desktop-allow-access.md).
3. Agregue una conexión de PC remota o un área de trabajo. Use una conexión para conectarse directamente a un PC Windows y un área de trabajo para usar un programa de RemoteApp, un escritorio basado en sesión o un escritorio virtual publicado por el administrador.
4. Ancla los elementos para poder acceder a Escritorio remoto rápidamente.

### <a name="add-a-remote-pc-connection"></a>Adición de una conexión de PC remota

Para crear una conexión de PC remota:

1. En el Centro de conexión, pulse **+ Agregar** y, después, **PC**.
2. Escribe la siguiente información del equipo al que quieres conectarte:
   - **Nombre del equipo**: el nombre del equipo. Puede ser un nombre de equipo Windows, un nombre de dominio de Internet o una dirección IP. También puedes anexar la información del puerto al nombre de equipo (por ejemplo, **MyDesktop:3389** o **10.0.0.1:3389**).
   - **Cuenta de usuario**: la cuenta de usuario que se utiliza para acceder al equipo remoto. Pulsa **+** para agregar una cuenta nueva o selecciona una cuenta existente. Puedes usar los siguientes formatos para el nombre de usuario: *nombre_de_usuario*, *dominio\nombre_de_usuario* o <em>user_name@domain.com</em>. También puede especificar si se deben solicitar credenciales durante la conexión. Para ello, debe seleccionar la opción **Preguntar cada vez**.
3. También puede establecer opciones adicionales pulsando en **Mostrar más**:
   - **Nombre para mostrar**: un nombre fácil de recordar para el equipo al que se va a conectar. Puede utilizar cualquier cadena, pero si no especifica ningún nombre descriptivo, se mostrará el nombre del equipo.
   - **Grupo**: especifica un grupo para que sea más fácil encontrar las conexiones posteriormente. Para agregar un grupo, pulsa **+** o selecciónalo en la lista.
   - **Puerta de enlace**: la puerta de enlace de PC remoto que quiere usar para conectarse a PC virtuales, programas de RemoteApp y PC basados en sesión de una red corporativa interna. La información acerca de la puerta de enlace te la puede proporcionar el administrador del sistema.
   - **Conectar a sesión del administrador**: esta opción se utiliza para conectarse a una sesión de consola para administrar un servidor de Windows.
   - **Intercambiar botones del mouse**: esta opción se usa para cambiar las funciones del botón izquierdo del ratón al botón derecho. Dicho intercambio es necesario cuando usa un equipo configurado para un usuario zurdo, pero solo dispone de un mouse para usuarios diestros.
   - **Establecer la resolución de la sesión remota en:** : selecciona la resolución que quieres utilizar en la sesión. **Elegir por mí** establecerá la resolución en función del tamaño del cliente.
   - **Change the size of the display** (Cambiar el tamaño de la pantalla): si selecciona una resolución estática alta para la sesión, puede usar esta configuración para que los elementos de la pantalla parezcan mayores, con el fin de mejorar la legibilidad. Esta configuración solo se aplica cuando se conecta a Windows 8.1 o una versión posterior.
   - **Actualizar la resolución de la sesión remota al cambiar tamaño**: si se habilita, el cliente actualizará dinámicamente la resolución de la sesión en función del tamaño del cliente. Esta configuración solo se aplica cuando se conecta a Windows 8.1 o una versión posterior.
   - **Portapapeles** : cuando está habilitado, permite tanto copiar texto e imágenes al equipo remoto como desde este.
   - **Reproducción de audio**: selecciona el dispositivo que vas a usar para el audio durante la sesión remota. Puedes elegir reproducir el sonido en los dispositivos locales, en un equipo remoto o no reproducirlo.
   - **Grabación de audio** : cuando está habilitada, permite usar un micrófono local con las aplicaciones en el equipo remoto.
4. Pulsa **Guardar**.

¿Es necesario editar esta configuración? Pulse el menú de desbordamiento ( **...** ) junto al nombre del PC y, después, pulse **Editar**.

¿Quieres eliminar la conexión? Vuelve a pulsar el menú de desbordamiento ( **...** ) y, después, pulsa **Quitar**.

### <a name="add-a-workspace"></a>Adición de un área de trabajo

Las áreas de trabajo son programas de RemoteApp, escritorios basados en sesión y escritorios virtuales publicados por el administrador mediante Servicios de Escritorio remoto.

Para agregar un área de trabajo:

1. En el Centro de conexión, pulse **+ Agregar** y, después, pulse **Áreas de trabajo**.
2. Escribe la **URL de fuente**, que te ha proporcionado el administrador, y pulsa **Buscar fuentes**.
3. Cuando se le soliciten las credenciales, especifíquelas para suscribirse a la fuente.

Las áreas de trabajo se mostrarán en el Centro de conexión.

Para eliminar las áreas de trabajo:

1. En el Centro de conexión, pulse el menú de desbordamiento ( **...** ) que hay al lado del área de trabajo.
2. Pulsa **Quitar**.

### <a name="pin-a-saved-pc-to-your-start-menu"></a>Anclaje de un PC guardado al menú Inicio

Para anclar una conexión al menú Inicio, pulse el menú de desbordamiento ( **...** ) que hay al lado del nombre del PC y, después, pulse **Anclar a Inicio**.

Ahora puede iniciar la conexión a un PC directamente desde el menú Inicio al pulsar en él.

## <a name="connect-to-an-rd-gateway-to-access-internal-assets"></a>Conexión a una puerta de enlace de Escritorio remoto para acceder a recursos internos

Las puertas de enlace de Escritorio remoto te permiten conectarte a equipos remotos de una red corporativa desde cualquier lugar de Internet. Para crear y administrar dichas puertas de enlace, usa el cliente de Escritorio remoto.

Para configurar una puerta de enlace nueva:

1. En el Centro de conexión, pulsa **Configuración**.
2. Junto a Puerta de enlace, pulsa **+** para agregar una nueva puerta de enlace.

      >[!NOTE]
      >También puede agregar una puerta de enlace cuando agregue una nueva conexión.

3. Escribe la siguiente información:
   - **Nombre del servidor**: el nombre del equipo que quieres usar como puerta de enlace. El nombre del servidor puede ser un nombre de equipo Windows, un nombre de dominio de Internet o una dirección IP. También puedes agregar información del puerto al nombre del servidor (por ejemplo: **RDGateway:443** o **10.0.0.1:443**).
   - **Cuenta de usuario**: seleccione o agregue una cuenta de usuario para usarla con la puerta de enlace de PC remoto a la que se va a conectar. También puede seleccionar **Use desktop user account** (Usar la cuenta de usuario de escritorio) para utilizar las mismas credenciales que usó para la conexión de PC remota.
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

- **Mostrar vistas previas de PC**: permite obtener una vista previa de un PC en el Centro de conexión antes de conectarse a él. Esta configuración está activada de manera predeterminada.
- **Ayudar a mejorar Escritorio remoto**: envía datos anónimos a Microsoft. Estos datos se usan para mejorar el cliente. Para obtener más información sobre cómo tratamos estos datos privados y anónimos, consulte la [Declaración de privacidad de Microsoft](https://privacy.microsoft.com/privacystatement). Esta configuración está activada de manera predeterminada.

### <a name="manage-your-user-accounts"></a>Administración de cuentas de usuario

Cuando se conecte a un PC o área de trabajo, puede guardar la información de la cuenta para conectarse a ella más adelante. También puede definir cuentas de usuario en el cliente, en lugar de guardar los datos del usuario al conectarse a un PC.

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

## <a name="navigate-your-remote-session"></a>Navegación por la sesión remota

En esta sección se describen las herramientas disponibles para ayudarle a navegar por la sesión remota una vez que se haya conectado al servicio.

### <a name="start-a-remote-session"></a>Inicio de una sesión remota

1. Pulse en el nombre de la conexión que quiere usar para iniciar la sesión.
2. Si no ha guardado las credenciales de la conexión, se le solicitará que escriba un **nombre de usuario** y una **contraseña**.
3. Si se le pide que verifique el certificado del área de trabajo o PC, revise la información y asegúrese de que confía en este PC antes de pulsar **Conectar**. También puedes seleccionar **Don’t ask about this certificate again** (No volver a pedir este certificado) para aceptarlo siempre.

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

### <a name="use-direct-touch-gestures-and-mouse-modes"></a>Uso de gestos de toque directo y modos de mouse

Puede interactuar con la sesión con los dos modos de mouse disponibles:

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
| Puntero | Zoom                 | Con dos dedos, acérquelos para alejar el zoom y sepárelos para acercar. |

## <a name="give-us-feedback"></a>Envíenos sus comentarios.

¿Tienes alguna sugerencia de característica o quieres informar de un problema? Para indicárnoslo, utilice el [Centro de opiniones](https://aka.ms/rdstorefeedback).

También puede enviarnos sus comentarios seleccionando el botón de puntos suspensivos ( **...** ) en la aplicación cliente y luego seleccionando **Comentarios**, tal como se muestra en la siguiente imagen.

> [!div class="mx-imgBorder"]
> ![Captura de pantalla que muestra el botón de puntos suspensivos resaltado en rojo. Se abre un menú desplegable debajo del botón y la opción "Comentarios" también se resalta en rojo.](../media/ellipsis-icon.png)

>[!NOTE]
>Para que podamos ayudarle mejor, es necesario que nos proporcione la información más detallada posible sobre el problema. Por ejemplo, puede incluir capturas de pantallas o un registro de las acciones que le han conducido al problema. Para más sugerencias sobre cómo proporcionar comentarios útiles, vea [Comentarios](/windows-insider/at-home/feedback#add-new-feedback).
