---
title: Introducción al cliente de escritorio de Windows
description: Información básica sobre el cliente del escritorio de Windows.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
author: heidilohr
manager: daveba
ms.author: helohr
ms.date: 09/13/2019
ms.localizationpriority: medium
ms.openlocfilehash: c864ba0e51054a553bfd53f845bd4d1c9ff3c8ba
ms.sourcegitcommit: 61767c405da44507bd3433967543644e760b20aa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/14/2019
ms.locfileid: "70988243"
---
# <a name="get-started-with-the-windows-desktop-client"></a>Introducción al cliente de escritorio de Windows

>Se aplica a: Windows 10 y Windows 7

El cliente de Escritorio remoto para el escritorio de Windows se puede usar para acceder a aplicaciones y escritorios de Windows de forma remota desde otro dispositivo Windows.

> [!NOTE]
> - Esta documentación no se aplica al cliente de Conexión a Escritorio remoto (MSTSC) que se distribuye con Windows. Se aplica al nuevo cliente de Escritorio remoto (MSRDC).
> - Actualmente, este cliente solo admite el acceso a aplicaciones y escritorios remotos desde el [Escritorio virtual de Windows](https://aka.ms/wvd).
> - ¿Quieres más información acerca de las nuevas versiones del cliente del escritorio de Windows? Consulta las [Novedades del cliente de escritorio de Windows](windowsdesktop-whatsnew.md)

## <a name="install-the-client"></a>Instalar el cliente

Actualmente, puedes descargar el cliente para Windows de 64 bits. Esta lista se actualizará cuando el cliente esté disponible para más versiones de Windows.

- [Cliente de Windows de 64 bits](https://go.microsoft.com/fwlink/?linkid=2068602)

Puedes instalar el cliente para el usuario actual, lo que no requiere derechos de administrador, o bien el administrador puede instalar y configurar el cliente para que todos los usuarios del dispositivo puedan acceder a él.

Una vez instalado, el cliente se puede iniciar desde el menú Inicio al buscar **Escritorio remoto.**

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

## <a name="update-the-client"></a>Actualizar el cliente

A menos que lo deshabilite un administrador, se te notificará cuando haya disponible una nueva versión del cliente. Esta notificación se puede mostrar directamente en el centro de conexiones o en el centro de actividades de Windows. Selecciona la notificación para iniciar el proceso de actualización.

También puedes buscar manualmente nuevas actualizaciones para el cliente:

1. En el centro de conexiones, pulsa en el menú de desbordamiento ( **...** ) en la barra de comandos de la parte superior del cliente.
2. Selecciona **Acerca de** en el menú desplegable.
3. Pulsa en **Buscar actualizaciones**.
4. Si hay una actualización disponible, pulsa en **Instalar actualización** para actualizar el cliente.

## <a name="providing-feedback"></a>Comentarios

¿Tienes algunas alguna sugerencia de característica o quieres informar de un problema? Indícanoslo desde el [Centro de opiniones](feedback-hub://?tabid=2&contextid=883), al que también se puede acceder desde el cliente:

1. En el centro de conexiones, pulsa en el menú de desbordamiento ( **...** ) en la barra de comandos de la parte superior del cliente.
2. Selecciona **Comentarios** en el menú desplegable para abrir el Centro de opiniones.
