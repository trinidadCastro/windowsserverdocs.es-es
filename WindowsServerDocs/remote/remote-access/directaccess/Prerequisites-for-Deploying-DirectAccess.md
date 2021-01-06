---
title: Requisitos previos para la implementación de DirectAccess
description: En este tema se proporcionan los requisitos previos para la implementación de DirectAccess en Windows Server 2016.
manager: brianlic
ms.topic: article
ms.assetid: 57c7e039-42ef-4909-867a-b5f669c9e74e
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 35dd601a1ca961d39352ca226142cc58c550273e
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97948271"
---
# <a name="prerequisites-for-deploying-directaccess"></a>Requisitos previos para la implementación de DirectAccess

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En la tabla siguiente se enumeran los requisitos previos necesarios para usar los asistentes de configuración de para implementar DirectAccess.

|Escenario|Requisitos previos|
|-|-|
|[Implementar un único servidor de DirectAccess con el Asistente para Introducción](../../remote-access/directaccess/single-server-wizard/Deploy-a-Single-DirectAccess-Server-Using-the-Getting-Started-Wizard.md)|-El Firewall de Windows debe estar habilitado en todos los perfiles<p>: Solo se admite para clientes que ejecutan &reg; Windows 10. <br />              Windows &reg; 8 y windows &reg; 8,1 Enterprise.<p>-No se requiere una infraestructura de clave pública.<p>-No se admite para implementar la autenticación en dos fases. Se necesitan credenciales de dominio para la autenticación.<p>-Implementa DirectAccess automáticamente en todos los equipos móviles del dominio actual.<p>-El tráfico a Internet no pasa por DirectAccess. La configuración de túnel forzado no es compatible.<p>-El servidor de DirectAccess es el servidor de ubicación de red.<p>-No se admite la protección de acceso a redes (NAP).<p>-No se admite el cambio de directivas con una característica distinta de la consola de administración de DirectAccess o de cmdlets de Windows PowerShell.<p>-En el caso de una configuración multisitio, ahora o en el futuro, en primer lugar, siga las instrucciones de [implementación de un único servidor de DirectAccess con la configuración avanzada](../../remote-access/directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md).|
|[Implementar un único servidor de DirectAccess con configuración avanzada](../../remote-access/directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md)|-Se debe implementar una infraestructura de clave pública.<br /> Para obtener más información, vea [la guía del laboratorio de pruebas: Basic PKI for Windows Server 2012](/answers/topics/windows-server-2012.html).<p>-El Firewall de Windows debe estar habilitado en todos los perfiles.<p>Los siguientes sistemas operativos de servidor son compatibles con DirectAccess.<p>-Puede implementar todas las versiones de Windows Server 2016 como un cliente de DirectAccess o un servidor de DirectAccess.<br />-Puede implementar todas las versiones de Windows Server 2012 R2 como un cliente de DirectAccess o un servidor de DirectAccess.<br />-Puede implementar todas las versiones de Windows Server 2012 como un cliente de DirectAccess o un servidor de DirectAccess.<br />-Puede implementar todas las versiones de Windows Server 2008 R2 como un cliente de DirectAccess o un servidor de DirectAccess.<p>Los siguientes sistemas operativos de cliente son compatibles con DirectAccess.<p>-Windows 10 &reg; Enterprise<br />-Windows 10 &reg; Enterprise 2015 rama de mantenimiento a largo plazo (LTSB)<br />-Windows &reg; 8 y 8,1 Enterprise<br />-Windows &reg; 7 Ultimate<br />-Windows &reg; 7 Enterprise<p>-La configuración de túnel forzado no es compatible con la autenticación de KerbProxy.<p>-No se admite el cambio de directivas con una característica distinta de la consola de administración de DirectAccess o de cmdlets de Windows PowerShell.<p>-No se admite la separación de los roles de servidor NAT64/DNS64 y IPHTTPS en otro servidor.|