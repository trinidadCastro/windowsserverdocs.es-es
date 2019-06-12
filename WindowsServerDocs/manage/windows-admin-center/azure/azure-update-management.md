---
title: Use Windows Admin Center para administrar las actualizaciones de sistema operativo con Update Management de Azure
description: Use Windows Admin Center (proyecto Honolulu) para configurar la administración de actualizaciones de Azure para administrar el sistema operativo se actualiza.
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 07/17/2018
ms.localizationpriority: low
ms.prod: windows-server-threshold
ms.openlocfilehash: 79b18e9963fba0993a7f34b1409edba6abfd48f0
ms.sourcegitcommit: 48bb3e5c179dc520fa879b16c9afe09e07c87629
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66452540"
---
# <a name="use-windows-admin-center-to-manage-operating-system-updates-with-azure-update-management"></a>Use Windows Admin Center para administrar las actualizaciones de sistema operativo con Update Management de Azure

[Más información sobre la integración de Azure con Windows Admin Center.](../plan/azure-integration-options.md)

Administración de actualizaciones de Azure es una solución en Azure Automation que le permite administrar las actualizaciones y revisiones para varios equipos desde un único lugar, en lugar de en cada servidor. Con Update Management de Azure, rápidamente puede evaluar el estado de las actualizaciones disponibles, programar la instalación de las actualizaciones necesarias y revisar los resultados de implementación para comprobar si las actualizaciones que se aplican correctamente. Esto es posible si las máquinas son máquinas virtuales de Azure, hospedado por otros proveedores de nube o local. [Más información sobre la administración de actualizaciones de Azure.](https://docs.microsoft.com/azure/automation/automation-update-management)

Con Windows Admin Center, puede configurar fácilmente y usar Administración de actualizaciones de Azure para mantener actualizados los servidores administrados. Si aún no tiene un área de trabajo de Log Analytics en su suscripción de Azure, Windows Admin Center automáticamente configurará su servidor y crear los recursos de Azure necesarios en la suscripción y ubicación que especifique. Si tiene un área de trabajo de Log Analytics existente, Windows Admin Center puede configurar automáticamente el servidor para consumir las actualizaciones de administración de actualizaciones de Azure.  

Para empezar, ir a la herramienta de actualizaciones en una conexión de servidor y seleccione "Configurar ahora" y proporcionar sus preferencias para los recursos de Azure relacionados. 

Una vez que haya configurado el servidor que se administrarán mediante la administración de actualizaciones de Azure, puede tener acceso a administración de actualizaciones de Azure mediante el hipervínculo disponibles en la herramienta de actualizaciones. 

[Obtenga información sobre cómo dejar de usar Update Management de Azure para actualizar el servidor.](azure-monitor.md#disabling-monitoring)

Tenga en cuenta que debe [registrar la puerta de enlace de Windows Admin Center con Azure](../configure/azure-integration.md) antes de configurar la administración de actualizaciones de Azure.

