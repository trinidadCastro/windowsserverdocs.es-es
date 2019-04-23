---
title: Visualización de tareas de detalles y notificaciones
description: Administrador de servidores
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-server-manager
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 95117407-2dd3-4f9a-841f-4331be3544c3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 44fd23b917d08aad663c0d3fb8da4bebef47783e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831516"
---
# <a name="view-task-details-and-notifications"></a>Visualización de tareas de detalles y notificaciones

>Se aplica a: Windows Server 2016

En Administrador del servidor en Windows Server 2012 R2 o Windows Server 2012, cuando se realizan tareas de administración, como agregar roles y características, iniciar servicios, actualizar los datos que se muestran en la consola de administrador del servidor o crear un grupo personalizado de servidores, un se muestra una notificación en el **notificaciones** área del encabezado de consola de administrador del servidor. Las notificaciones y el **detalles de tareas** cuadro de diálogo que puede abrir desde el **notificaciones** menú, haga clic en el icono de marca, mostrar el estado de las tareas de usuario o solicitudes, se mostrará si no se pudo una tarea y le ayudan a Solucione los problemas remitiendo a mensajes de error detallados sobre los errores de tarea.

## <a name="the-notifications-area"></a>Área Notificaciones
El **notificaciones** área en la barra de menús Administrador del servidor, marcada mediante un icono de marca, muestra los resultados de las tareas iniciadas en el administrador del servidor. Las notificaciones le informan de si se obtuvieron las tareas que se inició en el administrador del servidor correcta o errónea. Cuando hay notificaciones disponibles, su número se muestra junto al icono de la marca. Si una tarea ha experimentado un error, si solo se ha podido completar parcialmente (por ejemplo, si no se ha podido completar en todos los servidores remotos en los que deseaba realizarla) o si se ha completado con advertencias, la marca **Notificaciones** aparece en color rojo. Entre otras, se muestran notificaciones para las siguientes tareas.

-   Actualizar manualmente los datos mostrados en el administrador del servidor (se muestran notificaciones para las actualizaciones automáticas solo si estas experimentan errores)

-   iniciar o detener servicios

-   Instalación o desinstalación de roles, servicios de rol y características

-   iniciar un examen de Best Practices Analyzer (BPA)

-   Agregar servidores remotos para administrarlos (se muestran notificaciones para que errores de contacto o actualizar los datos de muestra para los servidores remotos)

Los elementos del menú **Notificaciones** muestran una barra de progreso, una breve descripción de la tarea, el nombre del servidor de destino de la tarea (o uno de los servidores de destino, si hay varios seleccionados), un vínculo a un control o cuadro de diálogo relacionado si procede y un menú **Tareas** . El menú **Tareas** muestra los comandos que se aplican a la notificación activa (la notificación sobre la que mantiene el cursor del mouse). Por ejemplo, si detiene un servicio, puede hacer clic en **Reiniciar** en el menú **Tareas** de la notificación para reiniciarlo.

Las notificaciones son especialmente útiles para la instalación o desinstalación de roles, servicios de rol y características. Por ejemplo, si inicia la instalación de una característica en un servidor remoto, puede cerrar el agregar Roles y características Asistente mientras la instalación aún está en curso, pero la tarea activa permanece en el **notificaciones** lista. El **notificaciones** elemento muestra una barra de progreso para la instalación y le permite volver a abrir el agregar Roles y características Asistente, si es necesario, haciendo **agregar Roles y características Asistente**. Los elementos de esta lista le notifican si se ha producido un error en la instalación o si es necesario seguir pasos de configuración adicionales para finalizar la implementación de la característica.

Las notificaciones también desempeñan un papel importante en la solución de problemas con las tareas o procesos en el administrador del servidor. Para obtener más información acerca de cómo usar los mensajes en el **notificaciones** área y **detalles de tareas** cuadro de diálogo para solucionar problemas de tareas o procesos incorrectos, consulte la [administrador del servidor Guía de solución](https://social.technet.microsoft.com/wiki/contents/articles/13443.windows-server-2012-server-manager-troubleshooting-guide-part-i-overview.aspx).

Para eliminar una notificación que ya no desea ver en el **notificaciones** lista, mantenga el cursor del mouse encima de la notificación y, a continuación, haga clic en **quitar tarea** (**X**).

## <a name="viewing-and-troubleshooting-tasks-by-using-task-details"></a>Ver y solucionar problemas de tareas mediante detalles de tarea
El **detalles de tareas** comando en la parte inferior de la **notificaciones** se abre el menú el **detalles de tareas** cuadro de diálogo que proporciona descripciones completas de (a partir de eventos de tarea, detención, advertencias, aciertos o errores). Al igual que otros controles de lista en el administrador del servidor, como **eventos**, **servicios**, y **Best Practices Analyzer** iconos, puede filtrar y crear las consultas se ejecuten en las tareas que se muestran en el **detalles de tareas** cuadro de diálogo. (para obtener más información sobre el filtrado y crear consultas en los controles de lista, consulte [filtrar, ordenar y consultar los datos en iconos del administrador del servidor](filter-sort-and-query-data-in-server-manager-tiles.md).) En el panel superior puede revisar las notificaciones que se han mostrado en el menú **Notificaciones** y ver cuántas de ellas se han generado sobre la misma tarea. al seleccionar una notificación en el panel superior muestra los detalles completos acerca de la notificación en el panel inferior.

El panel inferior es especialmente útil para solucionar los errores de las tareas. Si el administrador del servidor no puede conectarse a u obtener datos para un servidor que sea miembro del grupo de servidores, las entradas de este panel contienen a menudo mensajes detallados, incluido el texto completo de subyacentes de administración remota de Windows (WinRM), redes o problemas de seguridad que impedir que el administrador del servidor se comunique con un servidor de destino.

## <a name="see-also"></a>Vea también
[Filtrar, ordenar y consultar datos en iconos del administrador del servidor](filter-sort-and-query-data-in-server-manager-tiles.md)
[Guía de solución de problemas del administrador del servidor](https://social.technet.microsoft.com/wiki/contents/articles/13443.windows-server-2012-server-manager-troubleshooting-guide-part-i-overview.aspx)
