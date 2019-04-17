---
title: Introducción a Escritorio remoto en iOS
description: Obtén información sobre cómo configurar el cliente de escritorio remoto para iOS
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 03ec5a3d-d3f2-4afd-9405-ae58b6ecc91c
author: lizap
manager: dongill
ms.author: elizapo
date: 01/13/2017
ms.localizationpriority: medium
ms.openlocfilehash: aab84dde3ded2a3d3f17f4bb318302321c444606
ms.sourcegitcommit: d3f73936160505a40633ad8dd5931ac5fe3eccdb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/12/2019
ms.locfileid: "9297234"
---
# Introducción a Escritorio remoto en iOS

>Se aplica a: Windows 10, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2

Puedes usar al cliente de escritorio remoto para iOS para trabajar con aplicaciones, los recursos y los equipos de escritorio de Windows desde tu dispositivo iOS (iPhone y iPads).

Usa la siguiente información para empezar a trabajar. Asegúrate de echar un vistazo a las [preguntas más frecuentes sobre](remote-desktop-client-faq.md) si tienes alguna pregunta.

> [!NOTE]
> - ¿Curiosidad acerca de las nuevas versiones del cliente de iOS? Echa un vistazo a [Novedades para escritorio remoto en iOS?](ios-whatsnew.md)
> - El cliente de iOS admite dispositivos que ejecutan iOS 6.x y versiones posteriores.

## Obtener al cliente de escritorio remoto y empezar a usar

### Descargar al cliente de escritorio remoto de la tienda de iOS
Sigue estos pasos para empezar a trabajar con Escritorio remoto en tu dispositivo iOS:

1. Descargar al cliente de escritorio remoto de Microsoft de [iTunes](https://itunes.apple.com/us/app/microsoft-remote-desktop/id714464092?mt=8).
2. [Configurar el equipo para que acepte las conexiones remotas](remote-desktop-client-faq.md#how-do-i-set-up-a-pc-for-remote-desktop).
3. Agregar una [conexión a Escritorio remoto](#add-a-remote-desktop-connection) o un [recurso remoto](#add-a-remote-resource). Usar una conexión para conectarse a un directamente a un equipo con Windows y un recurso remoto para usar un programa RemoteApp, basada en la sesión de escritorio o un equipo de escritorio virtual publicado local con RemoteApp y escritorio. Esta característica es suele estar disponible en entornos corporativos.

### Descargar al cliente de escritorio remoto iOS Beta
En tu dispositivo iOS, sigue [estas instrucciones](https://aka.ms/rdiosbeta) para descargar al cliente de escritorio remoto iOS Beta.

### Agregar una conexión a Escritorio remoto

Para crear una conexión a Escritorio remoto: 
1. En la pulsación del centro de conexión **+** y, a continuación, pulsa **Agregar equipo o el servidor**.
2. Escribe la siguiente información para la conexión a Escritorio remoto:
  - **Nombre de equipo** : el nombre del equipo. Esto puede ser un nombre de equipo de Windows, un nombre de dominio de Internet o una dirección IP. También puede agregar información de puerto en el nombre de equipo (por ejemplo, **MyDesktop:3389** o **10.0.0.1:3389**).
  - **Nombre de usuario** : el nombre de usuario para tener acceso al equipo remoto. Puedes usar los siguientes formatos: *nombre de usuario*, *DOMINIO\nombre_usuario*o *user_name@domain.com*. También puedes especificar si se va a petición de un nombre de usuario y una contraseña.
3. También puedes establecer las siguientes opciones adicionales:
  - **Nombre descriptivo (opcional)** : un nombre fácil de recordar para el equipo se conecta a. Puedes usar cualquier cadena, pero si no se especifica un nombre descriptivo, se muestra el nombre de equipo.
  - **Puerta de enlace (opcional)** : puerta de enlace de escritorio remoto el que quieres usar para conectarse a los escritorios virtuales, los programas de RemoteApp y escritorios basados en sesión en una red corporativa interna. Obtener la información acerca de la puerta de enlace desde el administrador del sistema.
  - **Sonido** : selecciona el dispositivo que se usará para audio durante la sesión remota. Puedes elegir reproducir el sonido en los dispositivos locales, el dispositivo remoto, o no en absoluto.
  - **Botones del mouse de intercambio** , siempre que un movimiento de mouse podría enviar un comando con el botón izquierdo del mouse, envía el mismo comando con el botón derecho del ratón en su lugar. Esto es necesario si el equipo remoto está configurado para el modo de mouse a la izquierda.
  - **Modo de administrador** : conectarse a una sesión de administración en un servidor que ejecuta Windows Server 2003 o posterior.
4. Pulsa en **Guardar**.

¿Es necesario modificar estas opciones de configuración? Presiona y mantenga el escritorio que quieras editar y, a continuación, pulsa el icono de configuración. 

### Agregar un recurso remoto
Recursos remotos son los programas de RemoteApp, escritorios basados en la sesión y escritorios virtuales publicados con RemoteApp y escritorio.

- La dirección URL muestra el vínculo en el servidor de acceso Web de RD que proporciona acceso a RemoteApp y escritorio.
- Se enumeran las conexiones de escritorio y RemoteApp configurado.

Para agregar un recurso remoto:

1. En la pantalla del centro de conexión, pulsa **+** y, a continuación, pulsa **Agregar recursos remotos**. 
2. Escribe la información de los recursos remotos:
   - **Dirección URL de fuente** - la dirección URL del servidor de acceso Web de escritorio remoto. También puedes escribir tu cuenta de correo electrónico corporativo en este campo: Esto indica que el cliente para buscar el servidor de acceso Web de RD asociado con la dirección de correo electrónico.
   - **Nombre de usuario** : el nombre de usuario que se usará para el servidor de acceso Web de RD a que te conectas.
   - **Contraseña** : la contraseña que se usará para el servidor de acceso Web de RD a que te conectas.
3. Pulsa en **Guardar**.

Los recursos remotos se mostrará en el centro de conexión.


## Conectarse a una puerta de enlace de escritorio remoto para acceder a recursos internos

Una puerta de enlace de escritorio remoto (puerta de enlace de escritorio remoto) te permite conectarte a un equipo remoto en una red corporativa desde cualquier lugar en Internet. Puedes crear y administrar las puertas de enlace con el cliente de escritorio remoto.

Para configurar una puerta de enlace nuevo:

1. En el centro de conexión, pulsa **configuración > puertas de enlace**. 
2. Pulsa la **puerta de enlace de escritorio remoto de agregar**.
3. Escribe la siguiente información:
  - **Nombre del servidor** : el nombre del equipo que quieras usar como una puerta de enlace. Esto puede ser un nombre de equipo de Windows, un nombre de dominio de Internet o una dirección IP. También puedes agregar información de puerto para el nombre del servidor (por ejemplo: **RDGateway:443** o **10.0.0.1:443**).
  - **Nombre de usuario** : el nombre de usuario y contraseña que se usará para la puerta de enlace de escritorio remoto a que se conecta. También puedes seleccionar **usar las credenciales de conexión** para usar el mismo nombre de usuario y la contraseña como los que se usan para la conexión a Escritorio remoto.


## Administrar las cuentas de usuario 

Cuando se conecta a los recursos de un escritorio o remoto, puedes guardar las cuentas de usuario para seleccionar de nuevo. Puedes administrar las cuentas de usuario mediante el cliente de escritorio remoto.

Para crear una nueva cuenta de usuario:

1. En el centro de conexión, pulsa la **configuración**y, a continuación, pulsa **Nombres de usuario**.
2. Pulsa **Agregar cuenta de usuario**.
3. Escribe la siguiente información:
   - **Nombre de usuario** : el nombre del usuario para guardar para su uso con una conexión remota. Puedes escribir el nombre de usuario en cualquiera de los siguientes formatos: nombre de usuario, DOMINIO\nombre_usuario, o user_name@domain.com.
   - **Contraseña** : la contraseña para el usuario especificado. Cada cuenta de usuario que quieres guardar para usar para las conexiones remotas debe tener una contraseña asociada con ella.
4. Pulsa **Guardar**y, a continuación, pulsa **configuración**.
5. Pulsa en **Listo** para guardar la nueva configuración.

Para eliminar una cuenta de usuario:

1. En el centro de conexión, pulsa **> nombres de usuario de configuración**.
2. Deslizar el dedo la fila de derecha a izquierda para seleccionar el usuario.
3. Pulsa **Eliminar**.



## Navegar por la sesión de escritorio remoto
Cuando se inicia una sesión de escritorio remoto, hay herramientas disponibles que puedes usar para navegar a la sesión.

### Iniciar una conexión a escritorio remota

1. Pulsa la conexión a Escritorio remoto para iniciar la sesión de escritorio remota. 
2. Si se le pide que comprueba el certificado para el escritorio remoto, pulsa **Aceptar**. También puedes aceptar siempre deslizando el botón de alternancia **No preguntar de nuevo para las conexiones a este equipo** **on**. 

### Barra de conexión

La conexión barra da como resultado que acceso a los controles de navegación adicionales. 

- **Control de movimiento panorámico**: el control de movimiento panorámico permite a se amplió y se muevan alrededor de la pantalla. Ten en cuenta que el control de movimiento panorámico solo está disponible con la entrada táctil directo.
   - Habilitar o deshabilitar el control de movimiento panorámico: pulsa el icono de movimiento panorámico en la barra de conexión para mostrar el control de movimiento panorámico y zoom de la pantalla. Pulsa en el icono de movimiento panorámico en la barra de conexión para ocultar el control y devolver la pantalla a su resolución original.
   - Usar el control de movimiento panorámico: pulsar y sostener el movimiento panorámico de control y, a continuación, arrastrar en la dirección que quieras mover la pantalla.
   - Mover el control de movimiento panorámico: pulsa dos veces y mantenga el control de movimiento panorámico para mover el control en la pantalla.
- **Nombre de la conexión**: se muestra el nombre de la conexión actual. Pulsa en el nombre de conexión para mostrar la barra de selección de la sesión.
- **Teclado**: pulsa el icono de teclado para mostrar u ocultar el teclado. El control de movimiento panorámico se muestra automáticamente cuando se muestre el teclado.
- **Mover la barra de conexión**: pulsa y de suspensión de la barra de conexión y, a continuación, arrastrar y colocar a una nueva ubicación en la parte superior de la pantalla.

### Selección de sesión
Puede tener varias conexiones abrirse en equipos diferentes al mismo tiempo. Pulsa en la barra de conexión para mostrar la barra de selección de sesión en el lado izquierdo de la pantalla. La barra de selección de sesión te permite ver las conexiones de abierto y cambiar entre ellos. 

- Cambiar entre aplicaciones en una sesión de open recursos remotos.

    Cuando se conectan a recursos remotos, puedes cambiar entre aplicaciones abiertas dentro de esa sesión pulsando el menú de expansión y eligiendo en la lista de elementos disponibles.
- Iniciar una nueva sesión

  Puedes iniciar aplicaciones nuevas o sesiones de escritorio desde dentro de la conexión actual: pulsa **Empezar de nuevo**y, a continuación, elige de la lista de los elementos disponibles.

- Desconexión de una sesión

  Para desconectar una pulsación de sesión X en el lado izquierdo de la ventana de la sesión.

### Barra de comandos

La barra de comandos reemplaza la utilidad de la barra a partir de la versión 8.0.1. Puedes cambiar entre los modos de mouse y volver al centro de conexión de la barra de comandos.

## Usar gestos táctiles y los modos de mouse en una sesión remota

El cliente usa gestos táctiles estándar. También puedes usar gestos táctiles para replicar acciones de mouse en el escritorio remoto. Los modos de mouse disponibles se definen en la siguiente tabla.

> [!NOTE]
> Interactuar con Windows 8 o posterior se admiten los gestos táctiles nativo en el modo táctil directo. Para obtener más información sobre Windows 8 consulta gestos [Touch: Deslizar el dedo, pulsa y más allá](https://windows.microsoft.com/en-US/windows-8/touch-swipe-tap-beyond).

| Modo de mouse    | Operación del mouse      | Gesto                                                    |
|---------------|----------------------|------------------------------------------------------------|
| Pulsación directa  | Haga clic           | pulsación de 1 dedos                                               |
| Pulsación directa  | Hacer clic con el botón derecho          | 1 pulsación de dedos y sostener                                      |
| Puntero del mouse | Haga clic           | pulsación de 1 dedos                                               |
| Puntero del mouse | Hacer clic con el botón primario y arrastrar  | pulsa dos veces 1 dedo y sostener, a continuación, arrastra                    |
| Puntero del mouse | Hacer clic con el botón derecho          | pulsación de dedos 2                                               |
| Puntero del mouse | Hacer clic con el botón derecho y arrastrar | 2 pulsa dos veces dedo y sostener, a continuación, arrastra                    |
| Puntero del mouse | Rueda del mouse          | 2 dedo pulsa y sostén después arrastre hacia arriba o hacia abajo                |
| Puntero del mouse | Zoom                 | Ejemplo 2 dedos para acercar o propagarse 2 dedos para alejar |

## Dispositivos de entrada admitidos

El [cliente de escritorio remoto iOS beta](https://aka.ms/rdiosbeta) es compatible con el mouse físico Swiftpoint GT y ProPoint. Swiftpoint ofrece un [descuento exclusivo](https://www.swiftpoint.com/microsoft/) en el GT para los usuarios del cliente de iOS beta.

El cliente de iOS actualmente solo admite Swiftpoint mouse. Consulta la página de [Novedades de cliente de iOS](ios-whatsnew.md) y la [Tienda de la aplicación de iOS](https://aka.ms/rdios) para noticias sobre la compatibilidad con otros dispositivos en el futuro.

## Usar un teclado en una sesión remota

Puedes usar una pantalla o teclado físico en la sesión remota.

En la pantalla teclados, usa el botón en el borde derecho de la barra sobre el teclado para cambiar entre el teclado estándar y otros.

Si es compatible con Bluetooth para tu dispositivo iOS, el cliente detecta automáticamente el teclado Bluetooth.

Ten en cuenta que, debido a limitaciones en el sistema operativo, las teclas especiales, como Ctrl, la opción y la función no funcionará según lo esperado con un teclado de Bluetooth. Las siguientes claves funcionan:

- Teclas alfanuméricas
- Teclas de dirección
- Pestaña: Funciona de la pestaña, pero MAYÚS+TAB no funciona
- Home o Pos1: ALT+izquierda = Home
- Final: Alt+flecha derecha = final
- RE PÁG: Alt+flecha arriba = RE PÁG
- AV PÁG: Alt+flecha abajo = AV PÁG
- Seleccionar todo: Comando + = CTRL+a (Seleccionar todo en la mayoría de los programas)
- Cortar: Comando + X = CTRL+x (Cortar en la mayoría de los programas)
- Copia: Comando + C = Ctrl + C (copia en la mayoría de los programas)
- Pegar: Comando + V = CTRL+v (pegar en la mayoría de los programas)
- Símbolos: Alt + alfanumérico claves producirá símbolos diferentes según el idioma configurado

> [!TIP]
> Preguntas y comentarios siempre son inicio de sesión. Sin embargo, no envíe una solicitud de ayuda para solucionar problemas mediante el uso de la característica de comentarios al final de este artículo. En su lugar, ve al [foro de cliente de escritorio remoto](https://social.technet.microsoft.com/forums/windowsserver/en-us/home?forum=winrdc) e iniciar un nuevo subproceso. ¿Tiene una sugerencia de la característica? Nos indicará en el [foro de voz del usuario de cliente](https://remotedesktop.uservoice.com/forums/272085-remote-desktop-for-android).

