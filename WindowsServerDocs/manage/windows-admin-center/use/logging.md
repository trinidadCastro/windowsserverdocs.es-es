---
title: Registro de eventos
description: Registro de eventos del centro de administración de Windows (proyecto Honolulu)
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 012c2229fb29aa711d9887f28859e09bcf71c14a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356870"
---
# <a name="use-event-logging-in-windows-admin-center-to-gain-insight-into-management-activities-and-track-gateway-usage"></a>Usar el registro de eventos en el centro de administración de Windows para obtener información sobre las actividades de administración y realizar un seguimiento del uso de la puerta

>Se aplica a: Windows Admin Center, versión preliminar de Windows Admin Center

El centro de administración de Windows escribe registros de eventos para que pueda ver las actividades de administración que se realizan en los servidores de su entorno, así como para ayudarle a solucionar los problemas del centro de administración de Windows.

## <a name="gain-insight-into-management-activities-in-your-environment-through-user-action-logging"></a>Obtener información sobre las actividades de administración en su entorno mediante el registro de acciones de usuario

El centro de administración de Windows proporciona una visión general de las actividades de administración realizadas en los servidores de su entorno mediante el registro de acciones en el canal de eventos **Microsoft-ServerManagementExperience** en el registro de eventos del servidor administrado, con EventID 4000 y SMEGateway de origen. El centro de administración de Windows solo registra las acciones en el servidor administrado, por lo que no verá los eventos registrados si un usuario tiene acceso a un servidor con fines de solo lectura.

Los eventos registrados incluyen la siguiente información:

| Key           | Valor                                                                                              |
|---------------|----------------------------------------------------------------------------------------------------|
| PowerShell    | Nombre del script de PowerShell que se ejecutó en el servidor, si la acción ejecutó un script de PowerShell |
| ESTUDIO           | Llamada CIM que se ejecutó en el servidor, si la acción ejecutó una llamada CIM                        |
| Módulo        | Herramienta (o módulo) en la que se ejecutó la acción                                                     |
| Puerta de enlace       | Nombre del equipo de puerta de enlace del centro de administración de Windows en el que se ejecutó la acción                     |
| UserOnGateway | Nombre de usuario usado para tener acceso a la puerta de enlace del centro de administración de Windows y ejecutar la acción                    |
| UserOnTarget  | Nombre de usuario que se usa para tener acceso al servidor administrado de destino, si es diferente del userOnGateway (es decir, el usuario al que se mediante el servidor con las credenciales de "administrar como") |
| Delegación    | Booleano: Si el servidor administrado de destino confía en la puerta de enlace y las credenciales se delegan desde el equipo cliente del usuario             |
| EXTINGUI          | Booleano: Si el usuario tuvo acceso al servidor mediante credenciales de [laps](https://technet.microsoft.com/mt227395.aspx)                          |
| Archivo          | nombre del archivo cargado, si la acción era una carga de archivos                                |

## <a name="learn-about-windows-admin-center-activity-with-event-logging"></a>Obtener información acerca de la actividad del centro de administración de Windows con registro de eventos

El centro de administración de Windows registra la actividad de puerta de enlace en el canal de eventos del equipo de puerta de enlace para ayudarle a solucionar problemas y ver las métricas de uso. Estos eventos se registran en el canal **de eventos Microsoft-ServerManagementExperience** .

[Más información sobre la solución de problemas del centro de administración de Windows.](troubleshooting.md)
