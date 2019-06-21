---
title: Requisitos previos para la implementación de DirectAccess
description: Este tema proporcionan los requisitos previos para la implementación de DirectAccess en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 57c7e039-42ef-4909-867a-b5f669c9e74e
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f77df438ba2b282101a031b4ff4d145286cf3e94
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67283626"
---
# <a name="prerequisites-for-deploying-directaccess"></a>Requisitos previos para la implementación de DirectAccess

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En la tabla siguiente se enumera los requisitos previos necesarios para usar a los asistentes de configuración para implementar DirectAccess.  
  
|||  
|-|-|  
|Escenario|Requisitos previos|  
|[Implementar un único servidor de DirectAccess mediante el Asistente para introducción](../../remote-access/directaccess/single-server-wizard/Deploy-a-Single-DirectAccess-Server-Using-the-Getting-Started-Wizard.md)|-Firewall de Windows debe estar habilitado en todos los perfiles<br /><br />: Solo se admite para los clientes que ejecutan Windows 10&reg;, <br />              Windows&reg; 8 y Windows&reg; 8.1 Enterprise.<br /><br />-Una infraestructura de clave pública no es necesaria.<br /><br />-No se admite para la implementación de autenticación en dos fases. Se necesitan credenciales de dominio para la autenticación.<br /><br />-Implementa DirectAccess automáticamente a todos los equipos móviles en el dominio actual.<br /><br />-El tráfico a Internet no pasa a través de DirectAccess. La configuración de túnel forzado no es compatible.<br /><br />-Servidor de DirectAccess es el servidor de ubicación de red.<br /><br />-No se admite network Access Protection (NAP).<br /><br />-No se admite cambiar las directivas mediante la característica distinta de la consola de administración de DirectAccess o cmdlets de Windows PowerShell.<br /><br />-Para una configuración multisitio, ahora o en el futuro, primero siga las instrucciones de [implementar un único servidor de DirectAccess con configuración avanzada](../../remote-access/directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md).|  
|[Implementar un único servidor de DirectAccess con configuración avanzada](../../remote-access/directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md)|-Una infraestructura de clave pública debe implementarse.<br />    Para obtener más información, consulte [Minimódulo de guía de prueba de laboratorio: PKI básica para Windows Server 2012](https://social.technet.microsoft.com/wiki/contents/articles/7862.test-lab-guide-mini-module-basic-pki-for-windows-server-2012.aspx).<br /><br />-Firewall de Windows debe estar habilitado en todos los perfiles.<br /><br />Los siguientes sistemas operativos de servidor son compatibles con DirectAccess.<br /><br />-Puede implementar todas las versiones de Windows Server 2016 como un cliente de DirectAccess o un servidor de DirectAccess.<br />-Puede implementar todas las versiones de Windows Server 2012 R2 como un cliente de DirectAccess o un servidor de DirectAccess.<br />-Puede implementar todas las versiones de Windows Server 2012 como un cliente de DirectAccess o un servidor de DirectAccess.<br />-Puede implementar todas las versiones de Windows Server 2008 R2 como un cliente de DirectAccess o un servidor de DirectAccess.<br /><br />Los siguientes sistemas operativos de cliente son compatibles con DirectAccess.<br /><br />-Windows 10&reg; Enterprise<br />-Windows 10&reg; Enterprise 2015 largo plazo (LTSB) de la rama de mantenimiento<br />-Windows&reg; 8 y 8.1 Enterprise<br />-Windows&reg; 7 Ultimate<br />-Windows&reg; 7 Enterprise<br /><br />: Configuración de túnel forzado no se admite con la autenticación de KerbProxy.<br /><br />-No se admite cambiar las directivas mediante la característica distinta de la consola de administración de DirectAccess o cmdlets de Windows PowerShell.<br /><br />-No se admite separación NAT64/DNS64 y roles de servidor IPHTTPS en otro servidor.|  
  


