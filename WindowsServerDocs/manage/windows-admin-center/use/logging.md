---
title: Registro de eventos
description: Registro de eventos de Windows Admin Center (proyecto Honolulu)
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: d91b92cb3bba99ae4aa96a96650a251a6df4cea5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849306"
---
# <a name="use-event-logging-in-windows-admin-center-to-gain-insight-into-management-activities-and-track-gateway-usage"></a>Usar registro de eventos de Windows Admin Center para obtener información sobre las actividades de administración y seguimiento del uso de puerta de enlace

>Se aplica a: Windows Admin Center, vista previa de Windows Admin Center

Windows Admin Center escribe los registros de eventos le permiten ver las actividades de administración que se realizan en los servidores en su entorno, así como para ayudarle a solucionar los problemas de Windows Admin Center.

## <a name="gain-insight-into-management-activities-in-your-environment-through-user-action-logging"></a>Obtener una visión de las actividades de administración en su entorno a través del registro de acción del usuario

Windows Admin Center proporciona una visión de las actividades de administración realizadas en los servidores en su entorno mediante las acciones de registro para el **Microsoft ServerManagementExperience** canal de eventos en el registro de eventos de los recursos administrados servidor con EventID 4000 y SMEGateway de origen. Windows Admin Center solo registra las acciones en el servidor administrado, por lo que no verá los eventos registrados si un usuario tiene acceso a un servidor con fines de solo lectura.

Los eventos registrados incluyen la siguiente información:

| Key           | Valor                                                                                              |
|---------------|----------------------------------------------------------------------------------------------------|
| PowerShell    | Nombre del script de PowerShell que se ejecutó en el servidor, si la acción se ha ejecutado un script de PowerShell |
| CIM           | Llamada CIM que se ejecutó en el servidor, si la acción se ha ejecutado una llamada CIM                        |
| Módulo        | Herramienta (o módulo) donde se ejecutó la acción                                                     |
| Puerta de enlace       | Nombre de la máquina de puerta de enlace de Windows Admin Center donde se ejecutó la acción                     |
| UserOnGateway | Nombre de usuario utilizado para tener acceso a la puerta de enlace de Windows Admin Center y ejecutar la acción                    |
| UserOnTarget  | Nombre de usuario utilizado para tener acceso al servidor administrado de destino, si difiere el userOnGateway (es decir, el usuario tener acceso mediante el servidor mediante credenciales "Administrar como") |
| Delegación    | Booleano: si el destino se administra el servidor confía en la puerta de enlace y se delegan las credenciales de la máquina del usuario cliente             |
| LAPS          | Booleano: si el usuario tuvo acceso el servidor mediante [LAPS](https://technet.microsoft.com/mt227395.aspx) credenciales                          |
| Archivo          | nombre del archivo cargado, si la acción es una carga de archivos                                |

## <a name="learn-about-windows-admin-center-activity-with-event-logging"></a>Obtenga información acerca de la actividad de Windows Admin Center con el registro de eventos

Windows Admin Center registra la actividad de puerta de enlace para el canal de eventos en el equipo de puerta de enlace que le ayudarán a solucionar problemas y ver las métricas de uso. Estos eventos se registran en el **Microsoft ServerManagementExperience** canal de eventos.

[Más información sobre cómo solucionar problemas de Windows Admin Center.](troubleshooting.md)
