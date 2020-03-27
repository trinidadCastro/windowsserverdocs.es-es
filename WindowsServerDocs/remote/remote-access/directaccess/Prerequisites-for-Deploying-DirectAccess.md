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
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 14da51a2617764f2ee11eedcbc7a4e10b3b0da15
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "80309312"
---
# <a name="prerequisites-for-deploying-directaccess"></a>Requisitos previos para la implementación de DirectAccess

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En la tabla siguiente se enumeran los requisitos previos necesarios para usar los asistentes de configuración de para implementar DirectAccess.  
  
|||  
|-|-|  
|Escenario|Requisitos previos|  
|[Implementar un único servidor de DirectAccess con el Asistente para Introducción](../../remote-access/directaccess/single-server-wizard/Deploy-a-Single-DirectAccess-Server-Using-the-Getting-Started-Wizard.md)|-El Firewall de Windows debe estar habilitado en todos los perfiles<br /><br />: Solo se admite para clientes que ejecutan Windows 10&reg;, <br />              Windows&reg; 8 y Windows&reg; 8,1 Enterprise.<br /><br />-No se requiere una infraestructura de clave pública.<br /><br />-No se admite para implementar la autenticación en dos fases. Se necesitan credenciales de dominio para la autenticación.<br /><br />-Implementa DirectAccess automáticamente en todos los equipos móviles del dominio actual.<br /><br />-El tráfico a Internet no pasa por DirectAccess. La configuración de túnel forzado no es compatible.<br /><br />-El servidor de DirectAccess es el servidor de ubicación de red.<br /><br />-No se admite la protección de acceso a redes (NAP).<br /><br />-No se admite el cambio de directivas con una característica distinta de la consola de administración de DirectAccess o de cmdlets de Windows PowerShell.<br /><br />-En el caso de una configuración multisitio, ahora o en el futuro, en primer lugar, siga las instrucciones de [implementación de un único servidor de DirectAccess con la configuración avanzada](../../remote-access/directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md).|  
|[Implementar un único servidor de DirectAccess con configuración avanzada](../../remote-access/directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md)|-Se debe implementar una infraestructura de clave pública.<br />    Para obtener más información, vea [la guía del laboratorio de pruebas: Basic PKI for Windows Server 2012](https://social.technet.microsoft.com/wiki/contents/articles/7862.test-lab-guide-mini-module-basic-pki-for-windows-server-2012.aspx).<br /><br />-El Firewall de Windows debe estar habilitado en todos los perfiles.<br /><br />Los siguientes sistemas operativos de servidor son compatibles con DirectAccess.<br /><br />-Puede implementar todas las versiones de Windows Server 2016 como un cliente de DirectAccess o un servidor de DirectAccess.<br />-Puede implementar todas las versiones de Windows Server 2012 R2 como un cliente de DirectAccess o un servidor de DirectAccess.<br />-Puede implementar todas las versiones de Windows Server 2012 como un cliente de DirectAccess o un servidor de DirectAccess.<br />-Puede implementar todas las versiones de Windows Server 2008 R2 como un cliente de DirectAccess o un servidor de DirectAccess.<br /><br />Los siguientes sistemas operativos de cliente son compatibles con DirectAccess.<br /><br />-Windows 10&reg; Enterprise<br />-Windows 10&reg; Enterprise 2015 Rama de mantenimiento a largo plazo (LTSB)<br />-Windows&reg; 8 y 8,1 Enterprise<br />-Windows&reg; 7 Ultimate<br />-Windows&reg; 7 Enterprise<br /><br />-La configuración de túnel forzado no es compatible con la autenticación de KerbProxy.<br /><br />-No se admite el cambio de directivas con una característica distinta de la consola de administración de DirectAccess o de cmdlets de Windows PowerShell.<br /><br />-No se admite la separación de los roles de servidor NAT64/DNS64 y IPHTTPS en otro servidor.|  
  


