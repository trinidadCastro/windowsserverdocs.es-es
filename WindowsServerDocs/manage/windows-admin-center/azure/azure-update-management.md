---
title: Usa Windows Admin Center para administrar las actualizaciones de sistema operativo con administración de actualizaciones de Azure
description: Usa Windows Admin Center (proyecto Honolulu) para configurar la administración de actualizaciones de Azure para administrar el sistema operativo se actualiza.
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 07/17/2018
ms.localizationpriority: low
ms.prod: windows-server-threshold
ms.openlocfilehash: 5ba81968f8baa81176ad646fb2a97961ddc49fda
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/12/2019
ms.locfileid: "9297098"
---
# Usa Windows Admin Center para administrar las actualizaciones de sistema operativo con administración de actualizaciones de Azure

[Obtén más información acerca de la integración de Azure con Windows Admin Center.](../plan/azure-integration-options.md)

Administración de actualizaciones de Azure es una solución en la automatización de Azure que te permite administrar las actualizaciones y revisiones de seguridad para varios equipos desde un único lugar, en lugar de en cada servidor. Con la administración de actualizaciones de Azure, rápidamente puede evaluar el estado de las actualizaciones disponibles, programar la instalación de las actualizaciones necesarias y revisar los resultados de la implementación para comprobar las actualizaciones que correspondan correctamente. Esto es posible si las máquinas están las máquinas virtuales de Azure, hospedado por otros proveedores de nube o local. [Más información sobre la administración de actualizaciones de Azure.](https://docs.microsoft.com/azure/automation/automation-update-management)

Con Windows Admin Center, puede configurar fácilmente y usar Administración de actualizaciones de Azure para mantener los servidores administrados actualizado. Si aún no tienes un área de trabajo de Log Analytics en tu suscripción de Azure, Windows Admin Center se configura el servidor automáticamente y crear los recursos de Azure necesarios en la suscripción y la ubicación especificada. Si tienes un área de trabajo de Log Analytics existente, Windows Admin Center puede configurar automáticamente el servidor para que consumen las actualizaciones de administración de actualizaciones de Azure.  

Para empezar, ve a la herramienta de actualizaciones de una conexión de servidor y proporcionar tus preferencias para los recursos de Azure relacionados "Configurar ahora". 

Una vez que has configurado el servidor para administrarse mediante la administración de actualizaciones de Azure, puedes acceder a la administración de actualizaciones de Azure utilizando el hipervínculo proporcionado en la herramienta de actualizaciones. 

[Obtén información sobre cómo dejar de usar Azure Update Management para actualizar el servidor.](azure-monitor.md#disabling-monitoring)

Ten en cuenta que debes [registrar tu puerta de enlace de Windows Admin Center con Azure](..\configure\azure-integration.md) antes de configurar la administración de actualizaciones de Azure.

