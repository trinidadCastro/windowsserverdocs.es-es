---
title: Introducción a Escritorio remoto en Windows
description: Basic configura pasos para el cliente de escritorio remoto para Windows.
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
ms.openlocfilehash: 1c06e2eca725a825ac0fa4c7b617a26d89c898ff
ms.sourcegitcommit: d3f73936160505a40633ad8dd5931ac5fe3eccdb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/12/2019
ms.locfileid: "9297464"
---
# Introducción a Escritorio remoto en Windows

>Se aplica a: Windows 10, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2

Puedes usar al cliente de escritorio remoto para Windows para trabajar con aplicaciones de Windows y escritorios remotamente desde un dispositivo de Windows diferente.

Usa la siguiente información para empezar a trabajar. Asegúrate de echar un vistazo a las [preguntas más frecuentes sobre](remote-desktop-client-faq.md) si tienes alguna pregunta.

> [!NOTE]
> - ¿Curiosidad acerca de las nuevas versiones del cliente de Windows? Echa un vistazo a [Novedades para escritorio remoto en Windows?](windows-whatsnew.md)
> - El cliente puede ejecutar en cualquier versión de Windows 10.

## Obtener al cliente de escritorio remoto y empezar a usar

Sigue estos pasos para empezar a trabajar con Escritorio remoto en tu dispositivo Windows 10:

1. Descargar al cliente de escritorio remoto de [Microsoft Store](https://www.microsoft.com/store/p/microsoft-remote-desktop/9wzdncrfj3ps). 
2. [Configurar el equipo para que acepte las conexiones remotas](remote-desktop-allow-access.md).
3. Agregar una conexión a Escritorio remoto o un recurso remoto. Usar una conexión para conectarse directamente a un equipo con Windows y un recurso remoto a usar un programa de RemoteApp, basada en la sesión de escritorio o un escritorio virtual publicados por el administrador. 
4. Anclar elementos para que pueda empezar rápidamente a Escritorio remoto.

### Agregar una conexión a Escritorio remoto

Para crear una conexión a Escritorio remoto:

1. En el centro de conexión pulsa **+ Agregar**y, a continuación, pulsa **escritorio**.
2. Escribe la siguiente información para el equipo que desea conectarse para:
  - **Nombre de equipo** : el nombre del equipo. Esto puede ser un nombre de equipo de Windows, un nombre de dominio de Internet o una dirección IP. También puede agregar información de puerto en el nombre de equipo (por ejemplo, **MyDesktop:3389** o **10.0.0.1:3389**).
  - **Cuenta de usuario** : la cuenta de usuario para tener acceso al equipo remoto. Pulsa **+** para agregar una nueva cuenta o seleccionar una cuenta existente. Puedes usar los siguientes formatos para el nombre de usuario: *nombre de usuario*, *DOMINIO\nombre_usuario*o *user_name@domain.com*. También puedes especificar si se debe solicitar un nombre de usuario y contraseña durante la conexión seleccionando **Preguntar cada vez**.
3. También puedes establecer las opciones adicionales pulsar en **Mostrar más**:
  - **Nombre para mostrar** : un nombre fácil de recordar para el equipo se conecta a. Puedes usar cualquier cadena, pero si no se especifica un nombre descriptivo, se muestra el nombre de equipo.
  - **Grupo** : especificar un grupo para que sea más fácil de encontrar las conexiones más adelante. Puedes agregar un nuevo grupo pulsando **+** o seleccionar uno de la lista.
  - **Puerta de enlace de** puerta de enlace de escritorio remoto el que quieres usar para conectarse a los escritorios virtuales, los programas de RemoteApp y escritorios basados en sesión en una red corporativa interna. Obtener la información acerca de la puerta de enlace desde el administrador del sistema.
  - **Conectarse a la sesión del administrador** : usa esta opción para conectarse a una sesión de consola para administrar un servidor de Windows.
  - **Botones del mouse intercambio** : usa esta opción para intercambio de funciones de botón izquierdo del mouse para el botón derecho del ratón. (Esto es especialmente útil si el equipo remoto está configurado para un usuario a la izquierda, pero usan un mouse a la derecha).
  - **Establecer la resolución de la sesión remota:** : selecciona la resolución que quieras usar en la sesión. **Elegir para mí** establecerá la resolución en función del tamaño del cliente.
  - **Cambiar el tamaño de la pantalla:** : al seleccionar una resolución alta estática de la sesión, tienes la opción para que los elementos en la pantalla aparecen más grandes para mejorar la legibilidad. Nota: Esto solo se aplica cuando se conecta a Windows 8.1 o posterior.
  - **Actualizar la sesión remota, cambiar el tamaño de resolución en** : cuando está habilitada, el cliente actualizará dinámicamente la resolución de sesión en función del tamaño del cliente. Nota: Esto solo se aplica cuando se conecta a Windows 8.1 o posterior.
  - **Portapapeles** : cuando está habilitada, te permite copiar texto e imágenes desde el equipo remoto.
  - **Reproducción de audio** : selecciona el dispositivo que se usará para audio durante la sesión remota. Puedes elegir reproducir el sonido en los dispositivos locales, el equipo remoto, o no en absoluto.
  - **Grabación de audio** : cuando está habilitada, te permite usar un micrófono local con las aplicaciones en el equipo remoto.
4. Pulsa en **Guardar**.

¿Es necesario modificar estas opciones de configuración? Pulsa el menú de desbordamiento (**…**) junto al nombre del escritorio y, a continuación, pulsa **Editar**.

¿Quieres eliminar la conexión? De nuevo, pulsa el menú de desbordamiento (**…**) y, a continuación, pulsa **Quitar**.

### Agregar un recurso remoto
Recursos remotos son los programas de RemoteApp, escritorios basados en la sesión y escritorios virtuales publicados por el Administrador con los servicios de escritorio remoto.

Para agregar un recurso remoto:

1. En la pantalla del centro de conexión, pulsa **+ Agregar**y, a continuación, pulsa **recursos remotos**. 
2. Escribe la **Dirección URL de fuente** proporcionada por el administrador y pulsa **encontrar fuentes**.
3. Cuando se te solicite, proporcione las credenciales para usar para suscribirse a la fuente.

Los recursos remotos se mostrará en el centro de conexión.


Para eliminar recursos remotos:

1. En el centro de conexión, pulsa el menú de desbordamiento (**…**) junto a los recursos remotos.
2. Pulsa **Quitar**.

### Anclar un escritorio guardado en el menú Inicio

Para anclar una conexión con el menú Inicio, pulsa el menú de desbordamiento (**…**) junto al nombre del escritorio y, a continuación, pulsa **Anclar a inicio**.

Ahora la conexión a Escritorio remoto para iniciar directamente desde el menú Inicio, si se presionara.

## Conectarse a una puerta de enlace de escritorio remoto para acceder a recursos internos

Una puerta de enlace de escritorio remoto (puerta de enlace de escritorio remoto) te permite conectarte a un equipo remoto en una red corporativa desde cualquier lugar en Internet. Puedes crear y administrar las puertas de enlace con el cliente de escritorio remoto.

Para configurar una puerta de enlace nuevo:

1. En el centro de conexión, pulsa en **configuración**.
2. Junto a la puerta de enlace, pulsa **+** para agregar una puerta de enlace nuevo. Nota: También se puede agregar una puerta de enlace cuando se agrega una nueva conexión.
3. Escribe la siguiente información:
  - **Nombre del servidor** : el nombre del equipo que quieras usar como una puerta de enlace. Esto puede ser un nombre de equipo de Windows, un nombre de dominio de Internet o una dirección IP. También puedes agregar información de puerto para el nombre del servidor (por ejemplo: **RDGateway:443** o **10.0.0.1:443**).
  - **Cuenta de usuario** , selecciona o agregar una cuenta de usuario para usar con la puerta de enlace de escritorio remoto se conecta a. También puedes seleccionar **usar la cuenta de usuario de escritorio** para usar las mismas credenciales como los que se usan para la conexión a Escritorio remoto.
4. Pulsa en **Guardar**.  

## Configuración de la aplicación global

Puedes establecer la siguiente configuración global del cliente pulsando en **configuración**:

ADMINISTRA LOS ELEMENTOS
- **Cuenta de usuario** : te permite agregar, editar y eliminar las cuentas de usuario que se guardan en el cliente. Se trata de una buena manera de actualizar la contraseña de una cuenta después de que ha cambiado.
- **Puerta de enlace** : te permite agregar, editar y eliminar los servidores de puerta de enlace que se guardan en el cliente.
- **Grupo** - te permite agregar, editar y eliminar los grupos que se guardan en el cliente. Estas te permiten fácilmente las conexiones de grupo.

CONFIGURACIÓN DE LA SESIÓN
- **Iniciar conexiones en pantalla completa** - cuando está habilitada, cada vez que se inicia una conexión, el cliente usará toda la pantalla del monitor actual.
- **Iniciar cada conexión en una nueva ventana** - cuando está habilitada, cada conexión se inicia en una ventana independiente, lo que permite que aparezcan en distintos monitores y cambiar entre ellos mediante la barra de tareas.
- **Al cambiar el tamaño de la aplicación:** -permite controlar qué sucede cuando se cambia el tamaño de la ventana del cliente. Valores predeterminados para **expandir el contenido, conserva la relación de aspecto**.
- **Usar comandos de teclado con:** -vamos a especifiques donde se usan los comandos de teclado como *ganar* o *ALT + TAB* . El valor predeterminado es solo enviarlas a la sesión cuando la conexión está completa de los servicios.
- **Evitar que la pantalla de tiempo de espera de** - te permite mantener la pantalla de tiempo de espera cuando una sesión está activa. Esto es útil cuando la conexión no requiere ninguna interacción durante largos períodos de tiempo.

CONFIGURACIÓN DE LA APLICACIÓN
- **Mostrar vistas previas de escritorio** : te permite ver una vista previa de un equipo de escritorio en el centro de la conexión antes de que conectarse a ella. De manera predeterminada, se establece en **activado**.
- **Ayudar a mejorar el escritorio remoto** : envía datos anónima a Microsoft. Usamos estos datos para mejorar al cliente. Para obtener más información acerca de cómo tratamos esta anónimos datos privados, consulta la [Declaración de privacidad de Microsoft](https://privacy.microsoft.com/en-us/privacystatement). De manera predeterminada, esta opción está **activada**.

### Administrar las cuentas de usuario

Cuando se conecta a los recursos de un escritorio o remoto, puedes guardar las cuentas de usuario para seleccionar de nuevo. También puedes definir las cuentas de usuario en el cliente, en lugar de guardar los datos de usuario cuando se conecta a un equipo de escritorio.

Para crear una nueva cuenta de usuario:

1. En el centro de conexión, pulsa en **configuración**.
2. Junto a la cuenta de usuario, pulsa **+** para agregar una nueva cuenta de usuario.
3. Escribe la siguiente información:
   - **Nombre de usuario** - el nombre del usuario para guardar para su uso con una conexión remota. Puedes escribir el nombre de usuario en cualquiera de los siguientes formatos: nombre de usuario, DOMINIO\nombre_usuario, o user_name@domain.com.
   - **Contraseña** : la contraseña para el usuario especificado. Esto puede dejarse en blanco que se le solicite una contraseña durante la conexión.
4. Pulsa en **Guardar**.

Para eliminar una cuenta de usuario:

1. En el centro de conexión, pulsa en **configuración**.
2. Seleccionar la cuenta para eliminar en la lista de cuenta de usuario.
3. Junto a la cuenta de usuario, pulsa el icono de edición.
4. Pulsa **quitar esta cuenta** en la parte inferior para eliminar la cuenta de usuario.
5. También puedes editar la cuenta de usuario y pulsa en **Guardar**.

## Navegar por la sesión de escritorio remoto
Cuando se inicia una conexión a Escritorio remoto, hay herramientas disponibles que puedes usar para navegar a la sesión.

### Iniciar una conexión a Escritorio remoto

1. Pulsa la conexión a Escritorio remoto para iniciar la sesión.
2. Si no has guardado credenciales para la conexión, se te pedirá que proporcione un **nombre de usuario** y **contraseña**.
3. Si se le pide que comprueba el certificado para el escritorio remoto, revisa la información y asegurarse de que se trata de un equipo confías antes de pulsar **Connect**. También puedes seleccionar **no solicites sobre este certificado nuevo** siempre Aceptar este certificado.

### Barra de conexión

La conexión barra da como resultado que acceso a los controles de navegación adicionales. De manera predeterminada, la barra de conexión se coloca en el medio en la parte superior de la pantalla. Pulsa y arrastrar la barra a la izquierda o derecha para moverlo.

- **Control de movimiento panorámico** - el control de movimiento panorámico permite a se amplió y se muevan alrededor de la pantalla. Ten en cuenta que el control de movimiento panorámico solo está disponible en los dispositivos táctiles y con el modo de pulsación directa.
   - Habilitar o deshabilitar el control de movimiento panorámico: pulsa el icono de movimiento panorámico en la barra de conexión para mostrar el control de movimiento panorámico y zoom de la pantalla. Pulsa en el icono de movimiento panorámico en la barra de conexión para ocultar el control y devolver la pantalla a su resolución original.
   - Usar el control de movimiento panorámico - pulsación y mantenga el control de movimiento panorámico y, a continuación, arrastra en la dirección que quieras mover la pantalla.
   - Mover el control de movimiento panorámico: pulsa dos veces y mantenga el control de movimiento panorámico para mover el control en la pantalla.
- **Las opciones adicionales** : pulsa el icono de las opciones adicionales para mostrar la selección de sesión comandos y la barra de la barra (consulta más adelante).
- **Teclado** - punteo en el icono de teclado para mostrar u ocultar el teclado en pantalla. El control de movimiento panorámico se muestra automáticamente cuando se muestre el teclado.

### Barra de comandos

Pulsa el **...** en la barra de conexión para mostrar la barra de comandos en el lado derecho de la pantalla.

- **Home** , usa el botón de inicio para volver al centro de conexión de la barra de comandos.
  - También puedes usar el botón Atrás para la misma acción.
  - No se desconectará la sesión activa.
  - Esto te permite iniciar conexiones adicionales.
- **Desconectar** - usa el botón Desconectar para terminar la conexión.
  - Las aplicaciones seguirán activas siempre y cuando no finaliza la sesión en el equipo remoto.
- **Pantalla completa** - entra o sale del modo de pantalla completa.
- **Touch / Mouse** : puedes cambiar entre los modos de mouse (Touch directo y el puntero del Mouse).

### Uso directo gestos táctiles y modos de mouse en una sesión remota

Dos modos de mouse están disponibles para interactuar con la sesión.
- **Pulsación directa**: pasa todos los contactos táctiles a la sesión se interpreta de forma remota.
  - Se usa en la misma forma que usa Windows con una pantalla táctil.
- **Puntero del mouse**: transforma la pantalla táctil local en un panel táctil grande que permite mover un puntero del mouse en la sesión.
  - Se usa en la misma manera que usarías configura con un panel táctil.

> [!NOTE]
> Interactuar con Windows 8 o posterior se admiten los gestos táctiles nativo en el modo táctil directo. 

| Modo de mouse    | Operación del mouse      | Gesto                                                               |
|---------------|----------------------|-----------------------------------------------------------------------|
| Pulsación directa  | Haga clic           | pulsación de 1 dedos                                                          |
| Pulsación directa  | Hacer clic con el botón derecho          | 1 pulsación de dedos y sostener                                                 |
| Puntero del mouse | Haga clic           | pulsación de 1 dedos                                                          |
| Puntero del mouse | Hacer clic con el botón primario y arrastrar  | pulsa dos veces 1 dedo y sostener, a continuación, arrastra                               |
| Puntero del mouse | Hacer clic con el botón derecho          | pulsación de dedos 2                                                          |
| Puntero del mouse | Hacer clic con el botón derecho y arrastrar | 2 pulsa dos veces dedo y sostener, a continuación, arrastra                               |
| Puntero del mouse | Rueda del mouse          | 2 dedo pulsa y sostén después arrastre hacia arriba o hacia abajo                           |
| Puntero del mouse | Zoom                 | Usar los 2 dedos y alejar para ampliar o mover los dedos para alejar. |

> [!TIP]
> Preguntas y comentarios siempre son inicio de sesión. Sin embargo, no envíe una solicitud de ayuda para solucionar problemas mediante el uso de la característica de comentarios al final de este artículo. En su lugar, ve al [foro de cliente de escritorio remoto](https://social.technet.microsoft.com/forums/windowsserver/en-us/home?forum=winrdc) e iniciar un nuevo subproceso. ¿Tiene una sugerencia de la característica? Nos indicará con el [Centro de opiniones](feedback-hub://?tabid=2&contextid=605).
