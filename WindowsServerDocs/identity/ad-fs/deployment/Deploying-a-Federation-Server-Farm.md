---
ms.assetid: bbb5b68f-00ad-4715-8176-0c2769b706c4
title: Implementar una granja de servidores de Federación para Windows Server 2012 R2 AD FS
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 2f7602e99e550e993b916e603dad33724b3b0570
ms.sourcegitcommit: d08965d64f4a40ac20bc81b14f2d2ea89c48c5c8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2020
ms.locfileid: "96865044"
---
# <a name="deploying-a-federation-server-farm"></a>Implementación de una granja de servidores de federación

Para implementar una granja de servidores de federación, completa las tareas de esta lista de comprobación en orden. Cuando un vínculo de referencia lleve a un tema conceptual, vuelva a esta lista de comprobación después de revisar dicho tema para continuar con las tareas restantes.

![implementación](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**de la lista de comprobación de granja de servidores federados: implementación de una granja de servidores de Federación**

|Tarea|Referencia|
|--------|-------------|
|Revise los conceptos y las consideraciones importantes a medida que se preparan para implementar Servicios de federación de Active Directory (AD FS) \( AD FS \) . **Nota:**|![implementación](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[de la guía de diseño de AD FS de granja de servidores federados en Windows server 2012 R2](../../ad-fs/design/AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)<p>![implementar la granja de servidores federados Descripción de los conceptos de la](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[clave AD FS](../../ad-fs/technical-reference/Understanding-Key-AD-FS-Concepts.md)|
||Si decides usar Microsoft SQL Server para el almacén de configuración de AD FS, asegúrate de implementar una instancia práctica de SQL Server.|[SQL Server](/sql/sql-server/) **ADVERTENCIA:** en Windows Server 2012 R2, si desea crear una granja de AD FS y usar SQL Server para almacenar los datos de configuración, puede usar SQL Server 2008 y versiones más recientes, incluido SQL Server 2012.|
|Conecta tu equipo a un dominio de Active Directory|![implementar una granja de servidores federados](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[unir un equipo a un dominio](Join-a-Computer-to-a-Domain.md)|
|Inscriba un \( certificado SSL de capa de sockets seguros \) para AD FS.|![implementar una granja de servidores federados](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[inscribir un certificado SSL para AD FS](Enroll-an-SSL-Certificate-for-AD-FS.md)|
|Instala el servicio de rol de AD FS.|![implementación](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[de la granja de servidores federados instalar el servicio de rol de AD FS](Install-the-AD-FS-Role-Service.md)|
|Configura el servidor de federación.|![implementar una granja de servidores federados](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[configurar un servidor de Federación](Configure-a-Federation-Server.md)|
|Paso opcional: configurar un servidor de Federación con el servicio de registro de dispositivos \( DRS \) .|![implementación de una granja de servidores federados](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[configuración de un servidor de Federación con el servicio de registro de dispositivos](Configure-a-federation-server-with-Device-Registration-Service.md)|
|Agregue un \( registro de \) recursos CNAME y alias de host \( \) a DNS del sistema \( de nombres de dominio corporativo \) para el servicio de Federación y DRS.|![implementación de una granja de servidores federados](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[configurar el DNS corporativo para el servicio de Federación y el DRS](Configure-Corporate-DNS-for-the-Federation-Service-and-DRS.md)|
|Comprueba que esté operativo un servidor de federación.|![implementación](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[de una granja de servidores federados comprobación de que un servidor de Federación está operativo](Verify-That-a-Federation-Server-Is-Operational.md)|


## <a name="see-also"></a>Vea también
[Implementación de AD FS](../../ad-fs/AD-FS-Deployment.md)

[Guía de implementación de AD FS en Windows Server 2012 R2](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)

