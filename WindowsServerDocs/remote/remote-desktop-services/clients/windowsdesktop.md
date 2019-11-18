---
title: Introducción al cliente de escritorio de Windows
description: Información básica sobre el cliente del escritorio de Windows.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
author: heidilohr
manager: daveba
ms.author: helohr
ms.date: 11/12/2019
ms.localizationpriority: medium
ms.openlocfilehash: 2f786a1db0854ae89c1ceb23942793deb7f608e1
ms.sourcegitcommit: 315f015102c42c6fa7694e76adecdfb448390391
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/13/2019
ms.locfileid: "74019602"
---
# <a name="get-started-with-the-windows-desktop-client"></a>Introducción al cliente de escritorio de Windows

>Se aplica a: Windows 10 y Windows 7

El cliente de Escritorio remoto para el escritorio de Windows se puede usar para acceder a aplicaciones y escritorios de Windows de forma remota desde otro dispositivo Windows.

> [!NOTE]
> - Esta documentación no se aplica al cliente de Conexión a Escritorio remoto (MSTSC) que se distribuye con Windows. Se aplica al nuevo cliente de Escritorio remoto (MSRDC).
> - Actualmente, este cliente solo admite el acceso a aplicaciones y escritorios remotos desde el [Escritorio virtual de Windows](https://aka.ms/wvd).
> - ¿Quieres más información acerca de las nuevas versiones del cliente del escritorio de Windows? Consulta las [Novedades del cliente de escritorio de Windows](windowsdesktop-whatsnew.md)

## <a name="install-the-client"></a>Instalar el cliente

Elige el cliente que coincida con tu versión de Windows:

- [Windows de 64 bits](https://go.microsoft.com/fwlink/?linkid=2068602)
- [Windows de 32 bits](https://go.microsoft.com/fwlink/?linkid=2098960)
- [Windows ARM64](https://go.microsoft.com/fwlink/?linkid=2098961)

Puedes instalar el cliente para el usuario actual, lo que no requiere derechos de administrador, o bien el administrador puede instalar y configurar el cliente para que todos los usuarios del dispositivo puedan acceder a él.

Una vez instalado, el cliente se puede iniciar desde el menú Inicio al buscar **Escritorio remoto.**

## <a name="update-the-client"></a>Actualizar el cliente

Se te enviará una notificación cuando haya una nueva versión del cliente disponible siempre y cuando el administrador no haya deshabilitado las notificaciones. La notificación aparecerá en Connection Center (Centro de conexión) o en el centro de actividades de Windows. Para actualizar el cliente, solo tienes que seleccionar la notificación.

También puedes buscar manualmente nuevas actualizaciones para el cliente:

1. En el centro de conexiones, pulsa en el menú de desbordamiento ( **...** ) en la barra de comandos de la parte superior del cliente.
2. Selecciona **Acerca de** en el menú desplegable.
3. Pulsa en **Buscar actualizaciones**.
4. Si hay una actualización disponible, pulsa en **Instalar actualización** para actualizar el cliente.

## <a name="feeds"></a>Fuentes

Obtén la lista de recursos administrados a los que puedes tener acceso, como aplicaciones y equipos de escritorio, al suscribirte a la fuente que te proporcionó el administrador. Al suscribirte, los recursos están disponibles en el equipo local. Actualmente, el cliente del escritorio de Windows admite los recursos publicados desde el escritorio virtual de Windows.

### <a name="subscribe-to-a-feed"></a>Suscribirse a una fuente

1. Desde la página principal del cliente, también conocida como Centro de conexiones, pulsa en **Suscribirse**.
2. Inicia sesión con tu cuenta de usuario cuando se te solicite.
3. Los recursos aparecerán en el centro de conexiones agrupados por área de trabajo.

Puedes iniciar los recursos con uno de los métodos siguientes:

- Ve al centro de conexiones y haz doble clic en un recurso para iniciarlo.
- También puedes ir al menú Inicio y buscar una carpeta con el nombre del área de trabajo o escribir el nombre del recurso en la barra de búsqueda.

### <a name="workspace-details"></a>Detalles del área de trabajo

Después de suscribirte, puedes ver información adicional sobre un área de trabajo en el panel de detalles:

- Nombre del área de trabajo
- URL y nombre de usuario usados para suscribirse
- Número de aplicaciones y escritorios
- Fecha y hora de la última actualización
- Estado de la última actualización

Acceso al panel de detalles:

1. En el centro de conexión, pulsa el menú de desbordamiento ( **...** ) que hay al lado del área de trabajo.
2. Selecciona **Detalles** en el menú desplegable.
3. El panel de detalles aparece en el lado derecho del cliente.

Después de suscribirte, el área de trabajo se actualizará automáticamente de forma periódica. Se pueden agregar, cambiar o quitar recursos en función de los cambios que realiza el administrador.

También puedes buscar manualmente las actualizaciones de los recursos cuando sea necesario. Para ello, selecciona **Actualizar ahora** en el panel de detalles.

### <a name="unsubscribe-from-a-feed"></a>Cancelar la suscripción a una fuente

En esta sección se indica el proceso para cancelar tu suscripción a una fuente. Puedes cancelar la suscripción para suscribirte de nuevo con una cuenta diferente o quitar los recursos del sistema.

1. En el centro de conexión, pulsa el menú de desbordamiento ( **...** ) que hay al lado del área de trabajo.
2. Selecciona **Cancelar suscripción** en el menú desplegable.
3. Revisa el cuadro de diálogo y selecciona **Continuar**.

## <a name="managed-desktops"></a>Escritorios administrados

Las áreas de trabajo pueden contener varios recursos administrados, incluidos los escritorios. Al acceder a un escritorio administrado, tienes acceso a todas las aplicaciones que instaló el administrador.

### <a name="desktop-settings"></a>Configuración del escritorio

Puedes configurar algunas de las opciones de configuración de los recursos de escritorio para asegurarte de que la experiencia satisface tus necesidades. Para acceder a la lista de opciones de configuración disponibles:

1. En Connection Center (Centro de conexión), haz clic con el botón derecho en un recurso de escritorio.
2. Selecciona **Detalles** en el menú desplegable.
3. El panel Configuración aparece en el lado derecho del cliente que muestra el nombre del escritorio.

El cliente usará las opciones que configuró el administrador, a menos que se desactive la opción **Usar configuración predeterminada**. Esto te permite configurar las siguientes opciones:

- La opción **Use all monitors** (Usar todos los monitores) cambia la sesión de escritorio entre el uso de todos los monitores locales disponibles y un solo monitor.
- La opción **Start in full screen** (Iniciar en pantalla completa) determina si la sesión se iniciará en modo de pantalla completa o de ventana. Esta configuración se habilita automáticamente cuando se utilizan todos los monitores.
- La opción **Update the resolution on resize** (Actualizar la resolución al cambiar de tamaño) cambia el comportamiento al cambiar el tamaño de la sesión en modo de ventana. Si está habilitada, la resolución del escritorio remoto se actualizará para que coincida con el tamaño de la ventana local. Si está deshabilitada, la sesión conservará la resolución especificada en **Resolución** durante toda su duración. Esta configuración se habilita automáticamente cuando se utilizan todos los monitores.
- La opción **Resolución** te permite especificar la resolución del escritorio remoto. La sesión conservará esta resolución durante toda la duración. Esta configuración se deshabilita automáticamente si la resolución está establecida para actualizarse al cambiar el tamaño.
- La opción **Change the size of the text and apps** (Cambiar el tamaño del texto y las aplicaciones) especifica el tamaño del contenido de la sesión. Esta configuración solo se aplica al conectarse con Windows 8.1 y versiones posteriores o Windows Server 2012 R2 y versiones posteriores. Esta configuración se deshabilita automáticamente si la resolución está establecida para actualizarse al cambiar el tamaño.
- La opción **Fit session to window** (Ajustar sesión a la ventana) determina cómo se muestra la sesión cuando la resolución del escritorio remoto difiere del tamaño de la ventana local. Cuando se habilite, se cambiará el tamaño del contenido de la sesión para ajustarse dentro de la ventana, a la vez que se conservará la relación de aspecto de la sesión. Cuando se deshabilite, las barras de desplazamiento o las áreas negras se mostrarán cuando la resolución y el tamaño de la ventana no coincidan.

## <a name="provide-feedback"></a>Enviar comentarios

¿Tienes algunas alguna sugerencia de característica o quieres informar de un problema? Para indicárnoslo, utiliza el [Centro de opiniones](feedback-hub://?tabid=2&contextid=883). También puedes acceder al Centro de opiniones a través de tu cliente:

1. En el centro de conexiones, pulsa en el menú de desbordamiento ( **...** ) en la barra de comandos de la parte superior del cliente.
2. Selecciona **Comentarios** en el menú desplegable para abrir el Centro de opiniones.
