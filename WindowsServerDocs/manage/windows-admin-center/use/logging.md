---
title: Registro de eventos
description: Registro de eventos desde el centro de administración de Windows (Honolulu Project)
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: d91b92cb3bba99ae4aa96a96650a251a6df4cea5
ms.sourcegitcommit: e0479b0114eac7f232e8b1e45eeede96ccd72b26
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/22/2018
ms.locfileid: "2074346"
---
# <a name="use-event-logging-in-windows-admin-center-to-gain-insight-into-management-activities-and-track-gateway-usage"></a>Usar el registro de sucesos en el centro de administración de Windows para comprender mejor las actividades de administración y el uso de la puerta de enlace de seguimiento

>Se aplica a: Centro de administración de Windows, vista previa del centro de administración de Windows

Centro de administración de Windows escribe los registros de eventos le permiten ver las actividades de administración se lleva a cabo en los servidores en el entorno, así como para ayudar a solucionar los problemas del centro de administración de Windows.

## <a name="gain-insight-into-management-activities-in-your-environment-through-user-action-logging"></a>Obtenga información sobre las actividades de administración en su entorno a través del registro de la acción de usuario

Centro de administración de Windows proporciona información acerca de las actividades de administración realizadas en los servidores en su entorno por las acciones de registro para el canal de eventos de **Microsoft-ServerManagementExperience** en el registro de eventos del servidor administrado, con suceso 4000 y SMEGateway de origen. Centro de administración de Windows sólo los registros de acciones en el servidor administrado, por lo que no podrá ver los eventos registrados si un usuario obtiene acceso a un servidor con fines de sólo lectura.

Los eventos registrados incluyen la siguiente información:

| Clave           | Valor                                                                                              |
|---------------|----------------------------------------------------------------------------------------------------|
| PowerShell    | Nombre de la secuencia de comandos de PowerShell que se haya ejecutado en el servidor, si la acción ejecutó un script de PowerShell |
| CIM           | Llamada CIM que se ejecutó en el servidor, si la acción ejecutó una llamada CIM                        |
| Módulo        | Herramienta (o módulo) que se ejecutó la acción                                                     |
| Puerta de enlace       | Nombre de la máquina de puerta de enlace de centro de administración de Windows que se ejecutó la acción                     |
| UserOnGateway | Nombre de usuario utilizado para tener acceso a la puerta de enlace de centro de administración de Windows y ejecutar la acción                    |
| UserOnTarget  | Nombre de usuario utilizado para acceder al servidor administrado de destino, si es diferente de la userOnGateway (es decir, el usuario tener acceso a mediante el servidor con credenciales "Administrar como") |
| Delegación    | Valor booleano: si el destino administrados servidor confía en la puerta de enlace y se delegan las credenciales desde el equipo del usuario cliente             |
| VUELTAS          | Valor booleano: si el usuario tuvo acceso al servidor con credenciales de [VUELTAS](https://technet.microsoft.com/mt227395.aspx)                          |
| File          | nombre del archivo cargado, si la acción fue una carga de archivos                                |

## <a name="learn-about-windows-admin-center-activity-with-event-logging"></a>Obtenga información acerca de la actividad del centro de administración de Windows con el registro de eventos

Centro de administración de Windows registra la actividad de puerta de enlace para el canal de eventos en el equipo de puerta de enlace que le ayudarán a solucionar los problemas y ver las métricas en uso. Estos eventos se registran en el canal de eventos de **Microsoft ServerManagementExperience** .

[Obtenga más información sobre cómo solucionar problemas de centro de administración de Windows.](troubleshooting.md)
