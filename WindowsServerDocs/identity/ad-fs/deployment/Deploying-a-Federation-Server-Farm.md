---
ms.assetid: bbb5b68f-00ad-4715-8176-0c2769b706c4
title: Guía de implementación de AD FS en Windows Server 2012 R2
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: bb4d13d13771d76a306a32988c0faa03dd01db49
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855468"
---
# <a name="deploying-a-federation-server-farm"></a>Implementar una granja de servidores de federación


Para implementar una granja de servidores de federación, completa las tareas de esta lista de comprobación en orden. Cuando el vínculo de una referencia te lleve a un tema conceptual, vuelve a esta lista de comprobación antes de revisar el tema conceptual para que puedas seguir con las tareas pendientes de esta lista de comprobación.  
  
![implementación](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**de la granja de servidores federados: implementación de una granja de servidores de Federación**  
  
||Tarea|Referencia|  
|-|--------|-------------|  
|![implementando granja de servidores federados](media/icon_checkboxo.gif)|Revise los conceptos y las consideraciones importantes a la preparativos para implementar Servicios de federación de Active Directory (AD FS) \(AD FS\). **Nota:**|![implementación de la granja de servidores federadas](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[AD FS guía de diseño en Windows server 2012 R2](../../ad-fs/design/AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)<p>![implementar la granja de servidores federados Descripción de los](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[conceptos de AD FS clave](../../ad-fs/technical-reference/Understanding-Key-AD-FS-Concepts.md)|  
||Si decides usar Microsoft SQL Server para el almacén de configuración de AD FS, asegúrate de implementar una instancia práctica de SQL Server.|[SQL Server](https://technet.microsoft.com/sqlserver) **ADVERTENCIA:** en Windows Server 2012 R2, si desea crear una granja de AD FS y usar SQL Server para almacenar los datos de configuración, puede usar SQL Server 2008 y versiones más recientes, incluido SQL Server 2012.|  
|![implementando granja de servidores federados](media/icon_checkboxo.gif)|Conecta tu equipo a un dominio de Active Directory|![implementación de una granja de servidores federados](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[unir un equipo a un dominio](Join-a-Computer-to-a-Domain.md)|  
|![implementando granja de servidores federados](media/icon_checkboxo.gif)|Inscriba una capa de sockets seguros \(certificado\) SSL para AD FS.|![la implementación](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[de una granja de servidores federados inscriba un certificado SSL para AD FS](Enroll-an-SSL-Certificate-for-AD-FS.md)|  
|![implementando granja de servidores federados](media/icon_checkboxo.gif)|Instala el servicio de rol de AD FS.|![](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[la implementación de la granja de servidores federados instalar el servicio de rol AD FS](Install-the-AD-FS-Role-Service.md)|  
|![implementando granja de servidores federados](media/icon_checkboxo.gif)|Configura el servidor de federación.|![implementación](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[de una granja de servidores federados configurar un servidor de Federación](Configure-a-Federation-Server.md)|  
|![implementando granja de servidores federados](media/icon_checkboxo.gif)|Paso opcional: configurar un servidor de Federación con el servicio de registro de dispositivos \(DRS\).|![implementación](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[de una granja de servidores federados configurar un servidor de Federación con el servicio de registro de dispositivos](Configure-a-federation-server-with-Device-Registration-Service.md)|  
|![implementando granja de servidores federados](media/icon_checkboxo.gif)|Agregue un host \(un\) y alias \(registro de recursos de\) CNAME en el sistema de nombres de dominio corporativo \(DNS\) para el servicio de Federación y DRS.|![la implementación de una granja](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[de servidores federados configurar el DNS corporativo para el servicio de Federación y el DRS](Configure-Corporate-DNS-for-the-Federation-Service-and-DRS.md)|  
|![implementando granja de servidores federados](media/icon_checkboxo.gif)|Comprueba que esté operativo un servidor de federación.|![implementación](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[de una granja de servidores federados comprobar que un servidor de Federación está operativo](Verify-That-a-Federation-Server-Is-Operational.md)|  
  

## <a name="see-also"></a>Consulta también  
[Implementación de AD FS](../../ad-fs/AD-FS-Deployment.md)  

[Guía de implementación de AD FS de Windows Server 2012 R2](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
  

