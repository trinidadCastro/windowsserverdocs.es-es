---
title: Introducción al cliente de Microsoft Store
description: Los pasos básicos para la configuración del cliente de Escritorio remoto para Microsoft Store.
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
ms.date: 08/27/2019
ms.localizationpriority: medium
ms.openlocfilehash: 03927cd531617c6e0c9572fc4ce74768e10bc66a
ms.sourcegitcommit: 51eaab0f860312d97293fd90f3e632e7caee3df1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/29/2019
ms.locfileid: "70150930"
---
# <a name="get-started-with-the-windows-store-client"></a>Introducción al cliente de Microsoft Store

>Se aplica a: Windows 10

El cliente de Escritorio remoto para Windows se puede usar con aplicaciones de Windows y escritorios de forma remota desde otro dispositivo de Windows.

Usa la siguiente información para comenzar. Asegúrate de consultar la sección de [Preguntas frecuentes](remote-desktop-client-faq.md) si te surge alguna duda.

> [!NOTE]
> - ¿Quieres más información acerca de las nuevas versiones del cliente Windows? Consulta [Novedades de Escritorio remoto en Windows](windows-whatsnew.md)
> - El cliente se puede ejecutar en cualquier versión de Windows 10.

## <a name="get-the-rd-client-and-start-using-it"></a>Obtención del cliente de Escritorio remoto y primer uso

Sigue estos pasos para empezar a usar Escritorio remoto en un dispositivo con Windows 10:

1. Descarga el cliente de Escritorio remoto de [Microsoft Store](https://www.microsoft.com/store/p/microsoft-remote-desktop/9wzdncrfj3ps). 
2. [Configura el equipo para que acepte conexiones remotas](remote-desktop-allow-access.md).
3. Agrega una conexión a Escritorio remoto o un recurso remoto. Usa una conexión para conectarse directamente a un equipo Windows y un recurso remoto para usar un programa de RemoteApp, un escritorio basado en sesión o un escritorio virtual publicado por el administrador. 
4. Ancla los elementos para poder acceder a Escritorio remoto rápidamente.

### <a name="add-a-remote-desktop-connection"></a>Adición de una conexión a Escritorio remoto

Para crear una conexión a Escritorio remoto:

1. En el Centro de conexión, pulsa **+ Agregar**y, después, pulsa **Escritorio**.
2. Escribe la siguiente información del equipo al que quieres conectarte:
   - **Nombre del equipo**: el nombre del equipo. Puede ser un nombre de equipo Windows, un nombre de dominio de Internet o una dirección IP. También puedes anexar la información del puerto al nombre de equipo (por ejemplo, **MyDesktop:3389** o **10.0.0.1:3389**).
   - **Cuenta de usuario**: la cuenta de usuario que se utiliza para acceder al equipo remoto. Pulsa **+** para agregar una cuenta nueva o selecciona una cuenta existente. Puedes usar los siguientes formatos para el nombre de usuario: *nombre_de_usuario*, *dominio\nombre_de_usuario* o <em>user_name@domain.com</em>. También puedes especificar si se debe solicitar un nombre de usuario y una contraseña durante la conexión. Para ello, debe seleccionar **Preguntar cada vez**.
3. También puede establecer opciones adicionales pulsando en **Mostrar más**:
   - **Nombre para mostrar**: un nombre fácil de recordar para el equipo al que te vas a conectar. Puedes utilizar cualquier cadena, pero si no especificas un nombre descriptivo, se muestra el nombre del equipo.
   - **Grupo**: especifica un grupo para que sea más fácil encontrar las conexiones posteriormente. Para agregar un grupo, pulsa **+** o selecciónalo en la lista.
   - **Puerta de enlace**: la puerta de enlace de Escritorio remoto que quieres usar para conectarte a escritorios virtuales, programas RemoteApp y escritorios basados en sesión de una red corporativa interna. La información acerca de la puerta de enlace te la puede proporcionar el administrador del sistema.
   - **Conectar a sesión del administrador**: esta opción se utiliza para conectarse a una sesión de consola para administrar un servidor de Windows.
   - **Intercambiar botones del mouse**: esta opción se usa para cambiar las funciones del botón izquierdo del ratón al botón derecho. (es especialmente útil si el equipo remoto está configurado para usuarios zurdos, pero usa un ratón para diestros).
   - **Establecer la resolución de la sesión remota en:** : selecciona la resolución que quieres utilizar en la sesión. **Elegir por mí** establecerá la resolución en función del tamaño del cliente.
   - **Cambiar el tamaño de la pantalla:** : si seleccionas una resolución estática alta para la sesión, tienes la opción de hacer que los elementos de la pantalla parezcan mayores, con el fin de mejorar la legibilidad. Nota: Solo se aplica cuando se conecta a Windows 8.1, o a cualquier versión posterior.
   - **Actualizar la resolución de la sesión remota al cambiar tamaño**: si se habilita, el cliente actualizará dinámicamente la resolución de la sesión en función del tamaño del cliente. Nota: Solo se aplica cuando se conecta a Windows 8.1, o a cualquier versión posterior.
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
3. Cuando se soliciten, especifica las credenciales para suscribirse a la fuente.

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
2. Junto a Puerta de enlace, pulsa **+** para agregar una nueva puerta de enlace. Nota: También se pueden agregar puertas de enlace cuando se agrega una nueva conexión.
3. Escribe la siguiente información:
   - **Nombre del servidor**: el nombre del equipo que quieres usar como puerta de enlace. Puede ser un nombre de equipo Windows, un nombre de dominio de Internet o una dirección IP. También puedes agregar información del puerto al nombre del servidor (por ejemplo: **RDGateway:443** o **10.0.0.1:443**).
   - **Cuenta de usuario**: selecciona o agrega la cuenta de usuario que vas a usar con la puerta de enlace de Escritorio remoto a la que te vas a conectar. También puedes seleccionar **Usar la cuenta de usuario de escritorio** para usar las mismas credenciales que se utilizaron para la conexión a Escritorio remoto.
4. Pulsa **Guardar**.  

## <a name="global-app-settings"></a>Configuración de aplicaciones global

Para establecer la siguiente configuración global en el cliente, pulsa **Configuración**:

ELEMENTOS ADMINISTRADOS

- **Cuenta de usuario**: permite agregar, editar y eliminar cuentas de usuario guardadas en el cliente. Esta es una buena forma de actualizar la contraseña de una cuenta después de que haya cambiado.
- **Puerta de enlace**: permite agregar, editar y eliminar servidores de puerta de enlace guardados en el cliente.
- **Grupo**: permite agregar, editar y eliminar grupos guardados en el cliente. Estos elementos permiten agrupar conexiones fácilmente.

CONFIGURACIÓN DE SESIÓN

- **Iniciar conexiones en modo de pantalla completa**: si se habilita, cada vez que se inicie una conexión, el cliente utilizará toda la pantalla del monitor actual.
- **Empezar cada conexión en una ventana nueva**: si se habilita, cada conexión se inicia en una ventana independiente, lo que permite colocarlas en distintos monitores y cambiar de una a otra desde la barra de tareas.
- **Al cambiar el tamaño de la aplicación:** : permite controlar lo que sucede cuando se cambia el tamaño de la ventana del cliente. El valor predeterminado es **Ampliar el contenido, manteniendo la relación de aspecto**.
- **Usar comandos de teclado con:** : permite especificar dónde se usan comandos del teclado como *WIN* o *ALT+TAB*. El valor predeterminado es que se envíen solo a la sesión cuando la conexión esté en pantalla completa.
- **Impedir que se agote el tiempo de espera de la pantalla**: permite que la pantalla nunca supere el tiempo de espera cuando una sesión está activa. Resulta muy útil cuando la conexión no requiere ninguna interacción durante períodos prolongados.

CONFIGURACIÓN DE APLICACIONES

- **Mostrar vistas previas de escritorio**: permite obtener una vista previa de un escritorio en el Centro de conexión antes de conectarse a él. De manera predeterminada, se establece en **Activado**.
- **Ayudar a mejorar Escritorio remoto**: envía datos anónimos a Microsoft. Estos datos se usan para mejorar el cliente. Para más información acerca de cómo se tratan estos datos privados anónimos, consulta la [declaración de privacidad de Microsoft](https://privacy.microsoft.com/en-us/privacystatement). De manera predeterminada, este valor está en la posición **Activado**.

### <a name="manage-your-user-accounts"></a>Administración de cuentas de usuario

Cuando te conectas a un escritorio o a recursos remotos, puedes guardar las cuentas de usuario para volver a seleccionarlas más adelante. Las cuentas de usuario también se pueden definir en el propio cliente, en lugar de guardar los datos del usuario al conectarse a un escritorio.

Para crear una cuenta de usuario:

1. En el Centro de conexión, pulsa **Configuración**.
2. Junto a Cuenta de usuario, pulsa **+** para agregar una cuenta de usuario nueva.
3. Escribe la siguiente información:
   - **Nombre de usuario**: el nombre del usuario que se guarda para usarlo con una conexión remota. El nombre de usuario se puede escribir en cualquiera de los siguientes formatos: nombre_de_usuario, dominio\nombre_de_usuario user_name@domain.com.
   - **Contraseña**: la contraseña del usuario que has especificado. Lo puedes dejar en blanco si se quieres que se solicite la contraseña durante la conexión.
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
2. Si no has guardado las credenciales de la conexión, se te solicitará que escribas un **nombre de usuario** y una **contraseña**.
3. Si se te pide que compruebes el certificado de Escritorio remoto, examina la información y asegúrate de que se trata de un equipo de confianza antes de pulsar **Conectar**. También puedes seleccionar **Don’t ask about this certificate again** (No volver a pedir este certificado) para aceptarlo siempre.

### <a name="connection-bar"></a>Barra de conexión

La barra de conexión te permite acceder a más controles de navegación. De manera predeterminada, la barra de conexión se coloca en la parte central de la parte superior de la pantalla. Pulsa la barra y arrástrala hacia la izquierda o derecha para moverla.

- **Pan Control** (Control de panorámica): el control de panorámica permite ampliar la pantalla y moverse alrededor de ella. Ten en cuenta que el control de panorámica solo está disponible en los dispositivos de entrada táctil y cuando se usa el modo de toque directo.
   - Habilitar o deshabilitar el control de panorámica: pulsa el icono de panorámica de la barra de conexión para mostrar el control de panorámica y aplicar zoom a la pantalla. Vuelve a pulsar el icono de panorámica en la barra de conexión nuevo para ocultar el control y devolver la pantalla a su resolución original.
   - Usar el control de panorámica: mantén pulsado el control de panorámica y arrástralo en la dirección en que deseas mover la pantalla.
   - Mover el control de panorámica: pulsa dos veces y mantén pulsado el control de panorámica para mover el control en la pantalla.
- **Opciones adicionales**: pulsa el icono de opciones adicionales para mostrar la barra de selección y la barra de comandos de la sesión (ver abajo).
- **Teclado**: pulsa el icono del teclado para mostrar u ocultar el teclado en pantalla. El control de panorámica se muestra automáticamente cuando se muestra el teclado.

### <a name="command-bar"></a>Barra de comandos

Pulsa el botón **...**  en la barra de conexión para mostrar la barra de comandos en el lado derecho de la pantalla.

- **Inicio**: usa el botón Inicio para volver al Centro de conexión desde la barra de comandos.
  - También puedes usar el botón Atrás para realizar la misma acción.
  - La sesión activa no se desconectará.
  - Esto te permite iniciar conexiones adicionales.
- **Desconectar**: use el botón Desconectar para finalizar la conexión.
  - Las aplicaciones permanecerá activas mientras no se termine la sesión en el equipo remoto.
- **Pantalla completa** : entra o sale del modo de pantalla completa.
- **Táctil o ratón**: puedes cambiar entre los modos de ratón (puntero y toque directo).

### <a name="use-direct-touch-gestures-and-mouse-modes-in-a-remote-session"></a>Uso de gestos de toque directo y de modos de ratón en una sesión remota

Existen dos modos de ratón para interactuar con la sesión.

- **Toque directo**: pasa todos los contactos táctiles a la sesión para que se interpreten de forma remota.
  - Se usa en la misma manera que utilizaría Windows con una pantalla táctil.
- **Puntero**: Transforma la pantalla táctil local en un panel táctil grande que permite mover el puntero en la sesión.
  - Se usa en la misma manera que usarías Windows con un panel táctil.

> [!NOTE]
> En la interacción con Windows 8, o las versiones más recientes, se admiten los gestos táctiles nativos en modo de toque directo.

| Modo de mouse    | Operación del ratón      | Gesto                                                               |
|---------------|----------------------|-----------------------------------------------------------------------|
| Toque directo  | Hacer clic con el botón izquierdo           | Pulsación con un dedo                                                          |
| Toque directo  | Hacer clic con el botón derecho          | Mantener pulsado con un dedo                                                 |
| Puntero | Hacer clic con el botón izquierdo           | Pulsación con un dedo                                                          |
| Puntero | Hacer clic con el botón izquierdo y arrastrar  | Pulsar dos veces con un dedo y mantener pulsado, y luego arrastrar                               |
| Puntero | Hacer clic con el botón derecho          | Pulsación con dos dedos                                                          |
| Puntero | Hacer clic con el botón derecho y arrastrar | Pulsar dos veces con dos dedos y mantener pulsado, y luego arrastrar                               |
| Puntero | Rueda del ratón          | Pulsar con dos dedos y mantener pulsado, y luego arrastrar hacia arriba o abajo                           |
| Puntero | Zoom                 | Utiliza dos dedos y acércalos para acercar o sepáralos para alejar. |

> [!TIP]
> Tanto las preguntas como los comentarios son siempre bienvenidos. Sin embargo, NO publiques una solicitud de ayuda para solucionar problemas con la característica de comentario del final de este artículo. En su lugar, ve al [foro del cliente de Escritorio remoto](https://social.technet.microsoft.com/forums/windowsserver/en-us/home?forum=winrdc) e inicia una nueva conversación. ¿Tienes alguna sugerencia de característica? Para indicárnoslo, utiliza el [Centro de opiniones](feedback-hub://?tabid=2&contextid=605).
