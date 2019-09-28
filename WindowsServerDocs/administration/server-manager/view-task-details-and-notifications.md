---
title: Visualización de tareas de detalles y notificaciones
description: Administrador de servidores
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: a3dcbac95e60fce75316f8a4427aef54bdfad15b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383022"
---
# <a name="view-task-details-and-notifications"></a>Visualización de tareas de detalles y notificaciones

>Se aplica a: Windows Server 2016

En Administrador del servidor en Windows Server 2012 R2 o Windows Server 2012, al realizar tareas de administración como agregar roles y características, iniciar servicios, actualizar datos mostrados en la consola de Administrador del servidor o crear un grupo de servidores personalizado, la notificación se muestra en el área **notificaciones** del encabezado de la consola de administrador del servidor. Las notificaciones y el cuadro de diálogo **detalles de tarea** que puede abrir desde el menú **notificaciones** haciendo clic en el icono de marca, muestran el estado de las tareas o solicitudes de usuario, muestran si se produjo un error en una tarea y le ayudan a solucionar los problemas. mensajes de error detallados sobre los errores de tareas.

## <a name="the-notifications-area"></a>Área Notificaciones
El área **notificaciones** en la barra de menús administrador del servidor, marcada con un icono de marca, muestra los resultados de las tareas que se inician en Administrador del servidor. Las notificaciones le informan de si las tareas iniciadas en Administrador del servidor fueron correctas o no. Cuando hay notificaciones disponibles, su número se muestra junto al icono de la marca. Si una tarea ha experimentado un error, si solo se ha podido completar parcialmente (por ejemplo, si no se ha podido completar en todos los servidores remotos en los que deseaba realizarla) o si se ha completado con advertencias, la marca **Notificaciones** aparece en color rojo. Entre otras, se muestran notificaciones para las siguientes tareas.

-   Actualización manual de los datos que se muestran en Administrador del servidor (las notificaciones se muestran para las actualizaciones automáticas solo si se produce un error en las actualizaciones)

-   iniciar o detener servicios

-   Instalación o desinstalación de roles, servicios de rol y características

-   Inicio de un análisis de Analizador de procedimientos recomendados (BPA)

-   agregar servidores remotos para administrarlos (se muestran notificaciones para los errores de contacto o actualización de los datos que se muestran para los servidores remotos)

Los elementos del menú **Notificaciones** muestran una barra de progreso, una breve descripción de la tarea, el nombre del servidor de destino de la tarea (o uno de los servidores de destino, si hay varios seleccionados), un vínculo a un control o cuadro de diálogo relacionado si procede y un menú **Tareas** . El menú **Tareas** muestra los comandos que se aplican a la notificación activa (la notificación sobre la que mantiene el cursor del mouse). Por ejemplo, si detiene un servicio, puede hacer clic en **Reiniciar** en el menú **Tareas** de la notificación para reiniciarlo.

Las notificaciones son especialmente útiles para instalar o desinstalar roles, servicios de función y características. Por ejemplo, si inicia una instalación de características en un servidor remoto, puede cerrar el Asistente para agregar roles y características mientras la instalación todavía está en curso, pero la tarea activa permanece en la lista de **notificaciones** . El elemento **notificaciones** muestra una barra de progreso para la instalación y le permite volver a abrir el Asistente para agregar roles y características, si es necesario, haciendo clic en **Asistente para agregar roles y características**. Los elementos de esta lista le notifican si se ha producido un error en la instalación o si es necesario seguir pasos de configuración adicionales para finalizar la implementación de la característica.

Las notificaciones también desempeñan un papel importante en la solución de problemas con tareas o procesos en Administrador del servidor. Para obtener más información sobre cómo usar los mensajes en el área **notificaciones** y en el cuadro de diálogo **detalles de tarea** para solucionar problemas de tareas o procesos incorrectos, consulte la [Guía de solución de problemas de administrador del servidor](https://social.technet.microsoft.com/wiki/contents/articles/13443.windows-server-2012-server-manager-troubleshooting-guide-part-i-overview.aspx).

Para eliminar una notificación que ya no desee ver en la lista de **notificaciones** , mantenga el cursor del mouse sobre la notificación y, a continuación, haga clic en **quitar tarea** (**X**).

## <a name="viewing-and-troubleshooting-tasks-by-using-task-details"></a>Ver y solucionar problemas de tareas mediante detalles de tarea
El comando **detalles de tarea** de la parte inferior del menú **notificaciones** abre el cuadro de diálogo **detalles de tarea** , que proporciona descripciones completas de los eventos de tarea (Inicio, detención, advertencias, aciertos o errores). Al igual que los demás controles de lista de Administrador del servidor, como **los eventos, los** **servicios**y los iconos de **analizador de procedimientos recomendados** , puede filtrar y crear consultas para que se ejecuten en las tareas que se muestran en el cuadro de diálogo **detalles de tarea** . (para obtener más información sobre cómo filtrar y crear consultas en controles de lista, vea [filtrar, ordenar y consultar datos en Administrador del servidor mosaicos](filter-sort-and-query-data-in-server-manager-tiles.md)). En el panel superior puede revisar las notificaciones que se han mostrado en el menú **Notificaciones** y ver cuántas de ellas se han generado sobre la misma tarea. al seleccionar una notificación en el panel superior se muestran los detalles completos sobre la notificación en el panel inferior.

El panel inferior es especialmente útil para solucionar los errores de las tareas. Si Administrador del servidor no se puede conectar a un servidor que es miembro del grupo de servidores ni obtener datos de este, las entradas de este panel contienen a menudo mensajes detallados, incluido el texto completo de los problemas subyacentes de administración remota de Windows (WinRM), redes o seguridad que impedir que Administrador del servidor se comunique con un servidor de destino.

## <a name="see-also"></a>Vea también
[Filtrar, ordenar y consultar datos en Administrador del servidor mosaicos](filter-sort-and-query-data-in-server-manager-tiles.md)
[Administrador del servidor guía de solución de problemas](https://social.technet.microsoft.com/wiki/contents/articles/13443.windows-server-2012-server-manager-troubleshooting-guide-part-i-overview.aspx)
