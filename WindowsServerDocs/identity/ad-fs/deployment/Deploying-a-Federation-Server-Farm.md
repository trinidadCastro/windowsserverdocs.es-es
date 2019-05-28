---
ms.assetid: bbb5b68f-00ad-4715-8176-0c2769b706c4
title: Guía de implementación de AD FS en Windows Server 2012 R2
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: c0f8dca425f644952c36a289ec72651f6383e846
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192189"
---
# <a name="deploying-a-federation-server-farm"></a>Implementar una granja de servidores de federación


Para implementar una granja de servidores de federación, completa las tareas de esta lista de comprobación en orden. Cuando el vínculo de una referencia te lleve a un tema conceptual, vuelve a esta lista de comprobación antes de revisar el tema conceptual para que puedas seguir con las tareas pendientes de esta lista de comprobación.  
  
![implementación de granja de servidores federados](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**lista de comprobación: Implementar una granja de servidores de federación**  
  
||Tarea|Referencia|  
|-|--------|-------------|  
|![implementación de granja de servidores federados](media/icon_checkboxo.gif)|Revisar conceptos importantes y las consideraciones mientras se prepara para implementar servicios de federación de Active Directory \(AD FS\). **Nota:**|![implementación de granja de servidores federados](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Guía de diseño de AD FS en Windows Server 2012 R2](../../ad-fs/design/AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)<br /><br />![implementación de granja de servidores federados](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Understanding Key AD FS Concepts](../../ad-fs/technical-reference/Understanding-Key-AD-FS-Concepts.md)|  
||Si decides usar Microsoft SQL Server para el almacén de configuración de AD FS, asegúrate de implementar una instancia práctica de SQL Server.|[SQL Server](https://technet.microsoft.com/sqlserver) **advertencia:** Si quieres crear una granja de AD FS en Windows Server 2012 R2 y usar SQL Server para almacenar los datos de configuración, puedes usar SQL Server 2008 y versiones posteriores, incluyendo SQL Server 2012.|  
|![implementación de granja de servidores federados](media/icon_checkboxo.gif)|Conecta tu equipo a un dominio de Active Directory|![implementación de granja de servidores federados](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[unir un equipo a un dominio](Join-a-Computer-to-a-Domain.md)|  
|![implementación de granja de servidores federados](media/icon_checkboxo.gif)|Inscribir una capa de sockets seguros \(SSL\) certificado para AD FS.|![implementación de granja de servidores federados](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[inscribir un certificado SSL para AD FS](Enroll-an-SSL-Certificate-for-AD-FS.md)|  
|![implementación de granja de servidores federados](media/icon_checkboxo.gif)|Instala el servicio de rol de AD FS.|![implementación de granja de servidores federados](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[instalar el servicio de rol de AD FS](Install-the-AD-FS-Role-Service.md)|  
|![implementación de granja de servidores federados](media/icon_checkboxo.gif)|Configura el servidor de federación.|![implementación de granja de servidores federados](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[configurar un servidor de federación](Configure-a-Federation-Server.md)|  
|![implementación de granja de servidores federados](media/icon_checkboxo.gif)|Paso opcional: Configurar un servidor de federación con Device Registration Service \(DRS\).|![implementación de granja de servidores federados](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[configurar un servidor de federación con el servicio de registro de dispositivos](Configure-a-federation-server-with-Device-Registration-Service.md)|  
|![implementación de granja de servidores federados](media/icon_checkboxo.gif)|Agregar un host \(A\) y alias \(CNAME\) el registro de recursos de sistema de nombres de dominio corporativo \(DNS\) para el servicio de federación y DRS.|![implementación de granja de servidores federados](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[configurar DNS corporativo para el servicio de federación y DRS](Configure-Corporate-DNS-for-the-Federation-Service-and-DRS.md)|  
|![implementación de granja de servidores federados](media/icon_checkboxo.gif)|Comprueba que esté operativo un servidor de federación.|![implementación de granja de servidores federados](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Compruebe que una federación servidor está operativo](Verify-That-a-Federation-Server-Is-Operational.md)|  
  

## <a name="see-also"></a>Vea también  
[Implementación de AD FS](../../ad-fs/AD-FS-Deployment.md)  

[Guía de implementación de Windows Server 2012 R2 AD FS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
  

