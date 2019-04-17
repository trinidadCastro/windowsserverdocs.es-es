---
title: Introducción a Escritorio remoto en Android
description: Basic configura pasos para el cliente de escritorio remoto para Android.
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
ms.date: 07/24/2018
ms.localizationpriority: medium
ms.openlocfilehash: 42b4b4ffb73bd9d5d1397d32bd36c41d7e404dd7
ms.sourcegitcommit: d3f73936160505a40633ad8dd5931ac5fe3eccdb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/12/2019
ms.locfileid: "9297434"
---
# Introducción a Escritorio remoto en Android

>Se aplica a: Windows 10, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2

Puedes usar al cliente de escritorio remoto para Android para trabajar con los equipos de escritorio y las aplicaciones de Windows directamente desde tu dispositivo Android.

Usa la siguiente información para empezar a trabajar. Asegúrate de echar un vistazo a las [preguntas más frecuentes sobre](remote-desktop-client-faq.md) si tienes alguna pregunta.

> [!NOTE]
> - ¿Curiosidad acerca de las nuevas versiones del cliente Android? Echa un vistazo a [Novedades para escritorio remoto en Android?](android-whatsnew.md)
> Puedes ejecutar al cliente Android 4.1 y dispositivos más recientes, así como en Chromebooks con 53 ChromeOS instalado. Más información acerca de las aplicaciones de Android en Chrome [aquí](https://sites.google.com/a/chromium.org/dev/chromium-os/chrome-os-systems-supporting-android-apps).

## Obtener al cliente de escritorio remoto y empezar a usar

Sigue estos pasos para empezar a trabajar con Escritorio remoto en tu dispositivo Android:

1. Descargar al cliente de escritorio remoto de [Google Play](https://play.google.com/store/apps/details?id=com.microsoft.rdc.android). 
2. [Configurar el equipo para que acepte las conexiones remotas](remote-desktop-allow-access.md).
3. Agregar una conexión a Escritorio remoto o un recurso remoto. Usar una conexión para conectarse directamente a un equipo con Windows y un recurso remoto para usar un programa RemoteApp, basada en la sesión de escritorio o escritorio virtual publicado de forma local. 
4. Crear un widget para que pueda empezar rápidamente a Escritorio remoto.

> [!NOTE]
> Si quieres piloto nuevas características de versiones anteriores se recomienda descargar nuestra aplicación [Beta de escritorio remoto de Microsoft](https://play.google.com/store/apps/details?id=com.microsoft.rdc.android.beta) desde la tienda de Google Play. 

### Agregar una conexión a Escritorio remoto

Para crear una conexión a Escritorio remoto:

1. En la pulsación del centro de conexión **+** y, a continuación, pulsa el **escritorio**.
2. Escribe la siguiente información para el equipo que desea conectarse para:
  - **Nombre de equipo** : el nombre del equipo. Esto puede ser un nombre de equipo de Windows, un nombre de dominio de Internet o una dirección IP. También puede agregar información de puerto en el nombre de equipo (por ejemplo, **MyDesktop:3389** o **10.0.0.1:3389**).
  - **Nombre de usuario** : el nombre de usuario para tener acceso al equipo remoto. Puedes usar los siguientes formatos: *nombre de usuario*, *DOMINIO\nombre_usuario*o *user_name@domain.com*. También puedes especificar si se va a petición de un nombre de usuario y una contraseña.
3. También puedes establecer las siguientes opciones adicionales:
  - **Nombre descriptivo** : un nombre fácil de recordar para el equipo se conecta a. Puedes usar cualquier cadena, pero si no se especifica un nombre descriptivo, se muestra el nombre de equipo.
  - **Puerta de enlace de** puerta de enlace de escritorio remoto el que quieres usar para conectarse a los escritorios virtuales, los programas de RemoteApp y escritorios basados en sesión en una red corporativa interna. Obtener la información acerca de la puerta de enlace desde el administrador del sistema.
    ¿Se necesita configurar una puerta de enlace de escritorio remoto?
  - **Sonido** : selecciona el dispositivo que se usará para audio durante la sesión remota. Puedes elegir reproducir el sonido en los dispositivos locales, el dispositivo remoto, o no en absoluto.
  - **Personalizar la resolución de pantalla** : establecer una resolución personalizada para una conexión al habilitar esta configuración. Cuando se aplica desactivar la resolución que se hayan definido en la configuración global de la aplicación.
  - **Botones del mouse intercambio** : usa esta opción para intercambio de funciones de botón izquierdo del mouse para el botón derecho del ratón. (Esto es especialmente útil si el equipo remoto está configurado para un usuario a la izquierda, pero usan un mouse a la derecha).
  - **Conectarse a la sesión del administrador** : usa esta opción para conectarse a una sesión de consola para administrar un servidor de Windows.
  - **Redirigir a un almacenamiento local** : monta el almacenamiento local como un sistema de archivos remoto en el equipo remoto.
4. Pulsa en **Guardar**.

¿Es necesario modificar estas opciones de configuración? Pulsa el menú de desbordamiento (**…**) junto al nombre del escritorio y, a continuación, pulsa **Editar**.

¿Quieres eliminar la conexión? De nuevo, pulsa el menú de desbordamiento (**…**) y, a continuación, pulsa **Quitar**.

>[!TIP]
> Si recibes un error 0xf07 una contraseña incorrecta ("no pudimos conectamos al equipo remoto porque ha expirado la contraseña asociada con la cuenta de usuario"), cambiar la contraseña y vuelve a intentarlo.

### Agregar un recurso remoto
Recursos remotos son los programas de RemoteApp, escritorios basados en la sesión y escritorios virtuales publicados con RemoteApp y escritorio.

Para agregar un recurso remoto:

1. En la pantalla del centro de conexión, pulsa **+** y, a continuación, pulsa **La fuente de recursos remotos**. 
2. Escribe la información de los recursos remotos:
   - **Correo electrónico o la dirección URL** : la dirección URL del servidor de acceso Web de escritorio remoto. También puedes escribir tu cuenta de correo electrónico corporativo en este campo: Esto indica que el cliente para buscar el servidor de acceso Web de RD asociado con la dirección de correo electrónico.
   - **Nombre de usuario** : el nombre de usuario que se usará para el servidor de acceso Web de RD a que te conectas.
   - **Contraseña** : la contraseña que se usará para el servidor de acceso Web de RD a que te conectas.
3. Pulsa en **Guardar**.

Los recursos remotos se mostrará en el centro de conexión.


Para eliminar recursos remotos:

1. En el centro de conexión, pulsa el menú de desbordamiento (**…**) junto a los recursos remotos.
2. Pulsa **Quitar**.
3. Confirmar la eliminación.

### Widgets: un equipo de escritorio guardada en la pantalla principal de Pin

Las aplicaciones de escritorio remoto admiten las conexiones de anclaje a la pantalla principal mediante la función widget Android. La manera de agregar un widget depende del tipo de dispositivo Android que usas y su sistema operativo. Esta es la manera más común para agregar un widget: 

1. Pulsa en **las aplicaciones** para iniciar el menú de las aplicaciones.
2. Pulsa **widgets**.
3. Deslizar rápidamente a través de los widgets y busca el icono de escritorio remoto con la descripción, "Pin de escritorio remoto".
4. Pulsa mantenga esa widget de escritorio remoto y moverlo a la pantalla principal.
5. Cuando se suelta el icono, podrás ver los escritorios remotos guardados. Elegir la conexión que desea guardar en la pantalla principal.

Ahora la conexión a Escritorio remoto para iniciar directamente desde la pantalla principal, si se presionara.

> [!NOTE]
> Si cambia el nombre de la conexión a escritorio de la aplicación de escritorio remoto, la etiqueta de este anclado escritorio remoto no se actualizará.

## Conectarse a una puerta de enlace de escritorio remoto para acceder a recursos internos

Una puerta de enlace de escritorio remoto (puerta de enlace de escritorio remoto) te permite conectarte a un equipo remoto en una red corporativa desde cualquier lugar en Internet. Puedes crear y administrar las puertas de enlace con el cliente de escritorio remoto.

Para configurar una puerta de enlace nuevo:

1. En el centro de conexión, pulsa **configuración > puertas de enlace**. Pulsa **+** para agregar una puerta de enlace nuevo.
2. Escribe la siguiente información:
  - **Nombre del servidor** : el nombre del equipo que quieras usar como una puerta de enlace. Esto puede ser un nombre de equipo de Windows, un nombre de dominio de Internet o una dirección IP. También puedes agregar información de puerto para el nombre del servidor (por ejemplo: **RDGateway:443** o **10.0.0.1:443**).
  - **Nombre de usuario** : el nombre de usuario y contraseña que se usará para la puerta de enlace de escritorio remoto se conecta a. También puedes seleccionar **usar la cuenta de usuario de escritorio** para usar las mismas credenciales como los que se usan para la conexión a Escritorio remoto.

## Administrar las cuentas de usuario

Cuando se conecta a los recursos de un escritorio o remoto, puedes guardar las cuentas de usuario para seleccionar de nuevo. También puedes definir las cuentas de usuario en el cliente, en lugar de guardar los datos de usuario cuando se conecta a un equipo de escritorio.

Para crear una nueva cuenta de usuario:

1. En el centro de conexión, pulsa **configuración**y, a continuación, pulsa **cuentas de usuario**.
2. Pulsa **+** para agregar una nueva cuenta de usuario.
3. Escribe la siguiente información:
   - **Nombre de usuario** : el nombre del usuario para guardar para su uso con una conexión remota. Puedes escribir el nombre de usuario en cualquiera de los siguientes formatos: nombre de usuario, DOMINIO\nombre_usuario, o user_name@domain.com.
   - **Contraseña** : la contraseña para el usuario especificado. Cada cuenta de usuario que quieres guardar para usar para las conexiones remotas debe tener una contraseña asociada con ella.
4. Pulsa en **Guardar**.

Para eliminar una cuenta de usuario:

1. En el centro de conexión, pulsa en **las cuentas de usuario de configuración >**.
2. Pulsa y contener una cuenta de usuario de la lista para seleccionarlo. Puedes seleccionar varios usuarios.
3. Pulsa la Papelera para eliminar el usuario seleccionado.

## Navegar por la sesión de escritorio remoto
Cuando se inicia una conexión a Escritorio remoto, hay herramientas disponibles que puedes usar para navegar a la sesión.

### Iniciar una conexión a Escritorio remoto

1. Pulsa la conexión a Escritorio remoto para iniciar la sesión. 
2. Si se le pide que comprueba el certificado para el escritorio remoto, pulsa **Connect**. También puedes seleccionar **No preguntar de nuevo para las conexiones a este equipo** siempre acepta el certificado.

### Administrar la configuración de la aplicación global

Puedes establecer la siguiente configuración global del cliente Android:

- **Mostrar vistas previas de escritorio** : te permite ver una vista previa de un equipo de escritorio en el centro de la conexión antes de que conectarse a ella. De manera predeterminada, se establece en **activado**.
- **Ejemplo de zoom** - te permite usar gestos de reducir para acercar. Si la aplicación que estás usando a través de escritorio remoto admite multitoque (introducida en Windows 8), activar esta opción **desactivado**.
- **Ayudar a mejorar el escritorio remoto** : envía datos anónima a Microsoft. Usamos estos datos para mejorar al cliente. Puedes obtener más información sobre cómo tratamos esta anónimos datos privados, consulta la [Declaración de privacidad de cliente de escritorio remoto](https://www.microsoft.com/privacystatement/RemoteApp/Default.aspx). De manera predeterminada, esta opción está **activada**.
- **Mostrar** - hay dos opciones de configuración globales de la pantalla:
   - **Orientación** : establece la orientación preferida (horizontal o vertical) para la sesión. 
   >[!NOTE]
   > Si se conecta a un equipo que ejecute Windows 8 o una versión anterior de Windows, la sesión no se escale correctamente. Lo mejor es desconecte el equipo y, a continuación, volver a conectar con la orientación que quieras usar. Una opción aún mejor es actualizar el equipo para al menos Windows 8.1.

   - **Resolución** : establece la resolución que quieras usar para las conexiones a escritorio globalmente. Si ya has configurado una resolución personalizada para una aplicación individual o la conexión, esta configuración no cambiará.
   >[!NOTE]
   >Cuando se cambia una de las opciones de configuración de pantalla, sólo se aplican a nuevas conexiones desde ese punto en. Para ver el cambio en una sesión que ya estés conectado para desconectar y, a continuación, vuelve a conectar.

### Barra de conexión

La conexión barra da como resultado que acceso a los controles de navegación adicionales. De manera predeterminada, la barra de conexión se coloca en el medio en la parte superior de la pantalla. Pulsa dos veces y arrastrar la barra a la izquierda o derecha para moverlo.

- **Control de movimiento panorámico**: el control de movimiento panorámico permite a se amplió y se muevan alrededor de la pantalla. Ten en cuenta que el control de movimiento panorámico solo está disponible con la entrada táctil directo.
   - Habilitar o deshabilitar el control de movimiento panorámico: pulsa el icono de movimiento panorámico en la barra de conexión para mostrar el control de movimiento panorámico y zoom de la pantalla. Pulsa en el icono de movimiento panorámico en la barra de conexión para ocultar el control y devolver la pantalla a su resolución original.
   - Usar el control de movimiento panorámico: pulsar y sostener el movimiento panorámico de control y, a continuación, arrastrar en la dirección que quieras mover la pantalla.
   - Mover el control de movimiento panorámico: pulsa dos veces y mantenga el control de movimiento panorámico para mover el control en la pantalla.
- **Las opciones adicionales**: pulsa el icono de las opciones adicionales para mostrar la selección de sesión comandos y la barra de la barra (consulta más adelante).
- **Teclado**: pulsa el icono de teclado para mostrar u ocultar el teclado. El control de movimiento panorámico se muestra automáticamente cuando se muestre el teclado.
- **Mover la barra de conexión**: pulsa y de suspensión de la barra de conexión y, a continuación, arrastrar y colocar a una nueva ubicación en la parte superior de la pantalla.


### Barra de comandos

Pulsa en la barra de conexión para mostrar la barra de comandos en el lado derecho de la pantalla. Puedes cambiar entre los modos de mouse (Touch directo y el puntero del Mouse). Usa el botón de inicio para volver al centro de conexión de la barra de comandos. También puedes usar el botón Atrás para la misma acción. No se desconectará la sesión activa. 


### Uso directo gestos táctiles y modos de mouse en una sesión remota

El cliente usa gestos táctiles estándar. También puedes usar gestos táctiles para replicar acciones de mouse en el escritorio remoto. Los modos de mouse disponibles se definen en la siguiente tabla.

> [!NOTE]
> Interactuar con Windows 8 o posterior se admiten los gestos táctiles nativo en el modo táctil directo. 

| Modo de mouse    | Operación del mouse      | Gesto                                                               |
|---------------|----------------------|-----------------------------------------------------------------------|
| Pulsación directa  | Haga clic           | pulsación de 1 dedos                                                          |
| Pulsación directa  | Hacer clic con el botón derecho          | 1 pulsación de dedos y sostener                                                 |
| Puntero del mouse | Zoom                 | Usar los 2 dedos y alejar para ampliar o mover los dedos para alejar. |
| Puntero del mouse | Haga clic           | pulsación de 1 dedos                                                          |
| Puntero del mouse | Hacer clic con el botón primario y arrastrar  | pulsa dos veces 1 dedo y sostener, a continuación, arrastra                               |
| Puntero del mouse | Hacer clic con el botón derecho          | pulsación de dedos 2                                                          |
| Puntero del mouse | Hacer clic con el botón derecho y arrastrar | 2 pulsa dos veces dedo y sostener, a continuación, arrastra                               |
| Puntero del mouse | Rueda del mouse          | 2 dedo pulsa y sostén después arrastre hacia arriba o hacia abajo                           |

> [!TIP]
> Preguntas y comentarios siempre son inicio de sesión. Sin embargo, no envíe una solicitud de ayuda para solucionar problemas mediante el uso de la característica de comentarios al final de este artículo. En su lugar, ve al [foro de cliente de escritorio remoto](https://social.technet.microsoft.com/forums/windowsserver/en-us/home?forum=winrdc) e iniciar un nuevo subproceso. ¿Tiene una sugerencia de la característica? Nos indicará en el [foro de voz del usuario de cliente](https://remotedesktop.uservoice.com/forums/272085-remote-desktop-for-android).
