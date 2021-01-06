---
title: Usar el centro de administración de Windows para administrar las actualizaciones del sistema operativo con Azure Update Management
description: Use el centro de administración de Windows (Project Honolulu) para configurar Azure Update Management para administrar las actualizaciones del sistema operativo.
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 07/17/2018
ms.localizationpriority: low
ms.openlocfilehash: 74530e4a07dd3cc05d752e15337938ed62aff559
ms.sourcegitcommit: 38664a484b62a5c6342bd5105b814f55ee4b5604
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97917674"
---
# <a name="use-windows-admin-center-to-manage-operating-system-updates-with-azure-update-management"></a>Usar el centro de administración de Windows para administrar las actualizaciones del sistema operativo con Azure Update Management

[Más información acerca de la integración de Azure con el centro de administración de Windows.](./index.md)

Azure Update Management es una solución en Azure Automation que le permite administrar las actualizaciones y revisiones de varias máquinas desde un único lugar, en lugar de hacerlo por servidor. Con Azure Update Management, puede evaluar rápidamente el estado de las actualizaciones disponibles, programar la instalación de las actualizaciones necesarias y revisar los resultados de las implementaciones, con el fin de comprobar si las actualizaciones se aplican correctamente. Esto es posible si las máquinas son máquinas virtuales de Azure, hospedadas por otros proveedores en la nube o de forma local. [Más información sobre Azure Update Management](/azure/automation/update-management/overview).

Con el centro de administración de Windows, puede configurar y usar fácilmente Azure Update Management para mantener actualizados los servidores administrados. Si aún no tiene un área de trabajo de Log Analytics en su suscripción de Azure, el centro de administración de Windows configurará automáticamente el servidor y creará los recursos necesarios de Azure en la suscripción y la ubicación que especifique. Si ya dispone de un área de trabajo Log Analytics, el centro de administración de Windows puede configurar automáticamente el servidor para consumir actualizaciones de Azure Update Management.

Para empezar, vaya a la herramienta actualizaciones en una conexión de servidor y seleccione "configurar ahora" y proporcione sus preferencias para los recursos de Azure relacionados.

Una vez que haya configurado el servidor para que lo administre Azure Update Management, puede acceder a Azure Update Management mediante el hipervínculo proporcionado en la herramienta de actualizaciones.

[Aprenda a dejar de usar Azure Update Management para actualizar el servidor.](azure-monitor.md#disabling-monitoring)

Tenga en cuenta que debe [registrar la puerta de enlace del centro de administración de Windows con Azure](./azure-integration.md) antes de configurar Azure Update Management.