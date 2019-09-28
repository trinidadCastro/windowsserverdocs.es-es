---
title: Requisitos previos para la implementación de DirectAccess
description: En este tema se proporcionan los requisitos previos para la implementación de DirectAccess en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 57c7e039-42ef-4909-867a-b5f669c9e74e
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6e04dbf1576277493ec849c8de82aeab51e97649
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71388809"
---
# <a name="prerequisites-for-deploying-directaccess"></a>Requisitos previos para la implementación de DirectAccess

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En la tabla siguiente se enumeran los requisitos previos necesarios para usar los asistentes de configuración de para implementar DirectAccess.  
  
|||  
|-|-|  
|Escenario|Requisitos previos|  
|[Implementar un único servidor de DirectAccess con el Asistente para Introducción](../../remote-access/directaccess/single-server-wizard/Deploy-a-Single-DirectAccess-Server-Using-the-Getting-Started-Wizard.md)|-El Firewall de Windows debe estar habilitado en todos los perfiles<br /><br />: Solo se admite para clientes que ejecutan Windows 10 @ no__t-0. <br />              Windows @ no__t-0 8 y Windows @ no__t-1 8,1 Enterprise.<br /><br />-No se requiere una infraestructura de clave pública.<br /><br />-No se admite para implementar la autenticación en dos fases. Se necesitan credenciales de dominio para la autenticación.<br /><br />-Implementa DirectAccess automáticamente en todos los equipos móviles del dominio actual.<br /><br />-El tráfico a Internet no pasa por DirectAccess. La configuración de túnel forzado no es compatible.<br /><br />-El servidor de DirectAccess es el servidor de ubicación de red.<br /><br />-No se admite la protección de acceso a redes (NAP).<br /><br />-No se admite el cambio de directivas con una característica distinta de la consola de administración de DirectAccess o de cmdlets de Windows PowerShell.<br /><br />-En el caso de una configuración multisitio, ahora o en el futuro, en primer lugar, siga las instrucciones de [implementación de un único servidor de DirectAccess con la configuración avanzada](../../remote-access/directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md).|  
|[Implementar un único servidor de DirectAccess con configuración avanzada](../../remote-access/directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md)|-Se debe implementar una infraestructura de clave pública.<br />    Para obtener más información, vea [Test Lab Guide mini-Module: PKI básica para Windows Server 2012 @ no__t-0.<br /><br />-El Firewall de Windows debe estar habilitado en todos los perfiles.<br /><br />Los siguientes sistemas operativos de servidor son compatibles con DirectAccess.<br /><br />-Puede implementar todas las versiones de Windows Server 2016 como un cliente de DirectAccess o un servidor de DirectAccess.<br />-Puede implementar todas las versiones de Windows Server 2012 R2 como un cliente de DirectAccess o un servidor de DirectAccess.<br />-Puede implementar todas las versiones de Windows Server 2012 como un cliente de DirectAccess o un servidor de DirectAccess.<br />-Puede implementar todas las versiones de Windows Server 2008 R2 como un cliente de DirectAccess o un servidor de DirectAccess.<br /><br />Los siguientes sistemas operativos de cliente son compatibles con DirectAccess.<br /><br />-Windows 10 @ no__t-0 Enterprise<br />-Windows 10 @ no__t-0 Enterprise 2015 Rama de mantenimiento a largo plazo (LTSB)<br />-Windows @ no__t-0 8 y 8,1 Enterprise<br />-Windows @ no__t-0 7 Ultimate<br />-Windows @ no__t-0 7 Enterprise<br /><br />-La configuración de túnel forzado no es compatible con la autenticación de KerbProxy.<br /><br />-No se admite el cambio de directivas con una característica distinta de la consola de administración de DirectAccess o de cmdlets de Windows PowerShell.<br /><br />-No se admite la separación de los roles de servidor NAT64/DNS64 y IPHTTPS en otro servidor.|  
  


