---
ms.assetid: bbb5b68f-00ad-4715-8176-0c2769b706c4
title: "Guía de implementación de Windows Server 2012 R2 AD FS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 05f1ea6830237813e6fd2bd6a172f467e8d81065
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="deploying-a-federation-server-farm"></a>Implementar un conjunto de servidor de federación

>Se aplica a: Windows Server 2016, Windows Server 2012 R2

Para implementar una granja de servidores de federación, completa las tareas en esta lista de comprobación en orden. Cuando un vínculo de referencia le lleva a un tema conceptual, vuelve a esta lista de comprobación después de revisar el tema conceptual para que se puede continuar con las tareas restantes en esta lista de comprobación.  
  
![Implementación de granja de servidores federados](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**lista de comprobación: implementar un conjunto de servidor de federación**  
  
||Tarea|Referencia|  
|-|--------|-------------|  
|![Implementación de granja de servidores federados](media/icon_checkboxo.gif)|Revisar consideraciones y conceptos importantes como prepararse para implementar los servicios de federación de Active Directory \(AD FS\). **Nota:**|![Implementación de granja de servidores federados](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Guía de diseño de AD FS en Windows Server 2012 R2](../../ad-fs/design/AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)<br /><br />![Implementación de granja de servidores federados](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[comprensión clave AD FS conceptos](../../ad-fs/technical-reference/Understanding-Key-AD-FS-Concepts.md)|  
||Si decides usar Microsoft SQL Server para el almacén de configuración de AD FS, asegúrate de para implementar una instancia de SQL Server funcional.|[SQL Server](https://technet.microsoft.com/sqlserver)**advertencia:** en Windows Server 2012 R2, si quieres crear una granja de servidores de AD FS y usar SQL Server para almacenar los datos de configuración, puedes usar SQL Server 2008 y las versiones más recientes, incluido SQL Server 2012.|  
|![Implementación de granja de servidores federados](media/icon_checkboxo.gif)|Unir el equipo a un dominio de Active Directory.|![Implementación de granja de servidores federados](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[unir un equipo a un dominio](Join-a-Computer-to-a-Domain.md)|  
|![Implementación de granja de servidores federados](media/icon_checkboxo.gif)|Inscribir un certificado de capa de sockets seguros \(SSL\) de AD FS.|![Implementación de granja de servidores federados](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[inscribir un certificado SSL de AD FS](Enroll-an-SSL-Certificate-for-AD-FS.md)|  
|![Implementación de granja de servidores federados](media/icon_checkboxo.gif)|Instalar el servicio de rol de AD FS.|![Implementación de granja de servidores federados](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[instalar el servicio de rol de AD FS](Install-the-AD-FS-Role-Service.md)|  
|![Implementación de granja de servidores federados](media/icon_checkboxo.gif)|Configurar un servidor de federación.|![Implementación de granja de servidores federados](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[configurar un servidor de federación](Configure-a-Federation-Server.md)|  
|![Implementación de granja de servidores federados](media/icon_checkboxo.gif)|Paso opcional: configurar un servidor de federación con \(DRS\) servicio de registro del dispositivo.|![Implementación de granja de servidores federados](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[configurar un servidor de federación con el servicio de registro de dispositivo](Configure-a-federation-server-with-Device-Registration-Service.md)|  
|![Implementación de granja de servidores federados](media/icon_checkboxo.gif)|Agregar un registro de recursos host \(A\) y alias \(CNAME\) corporativa \(DNS\) sistema de nombres de dominio para el servicio de federación y DRS.|![Implementación de granja de servidores federados](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[configurar DNS corporativa para los servicios de federación y DRS](Configure-Corporate-DNS-for-the-Federation-Service-and-DRS.md)|  
|![Implementación de granja de servidores federados](media/icon_checkboxo.gif)|Comprueba que un servidor de federación está operativo.|![Implementación de granja de servidores federados](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[comprueba que una federación servidor está operativa](Verify-That-a-Federation-Server-Is-Operational.md)|  
  

## <a name="see-also"></a>Consulta también  
[Implementación de AD FS](../../ad-fs/AD-FS-Deployment.md)  

[Guía de implementación de Windows Server 2012 R2 AD FS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
  

