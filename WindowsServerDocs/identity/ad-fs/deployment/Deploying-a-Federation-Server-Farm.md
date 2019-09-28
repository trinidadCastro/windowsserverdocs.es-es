---
ms.assetid: bbb5b68f-00ad-4715-8176-0c2769b706c4
title: Guía de implementación de AD FS en Windows Server 2012 R2
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: e5507cd567114d17c6500655ee210b70bd9ea1ec
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408414"
---
# <a name="deploying-a-federation-server-farm"></a>Implementar una granja de servidores de federación


Para implementar una granja de servidores de federación, completa las tareas de esta lista de comprobación en orden. Cuando el vínculo de una referencia te lleve a un tema conceptual, vuelve a esta lista de comprobación antes de revisar el tema conceptual para que puedas seguir con las tareas pendientes de esta lista de comprobación.  
  
![deploying granja de servidores federados @ no__t-1Checklist: Implementar una granja de servidores de Federación @ no__t-0  
  
||Tarea|Referencia|  
|-|--------|-------------|  
|![implementando granja de servidores federados](media/icon_checkboxo.gif)|Revise los conceptos y las consideraciones importantes a medida que prepara para implementar Servicios de federación de Active Directory (AD FS) \(AD FS @ no__t-1. **Nota:**|Guía de diseño de la granja de servidores federados de @no__t 0deploying](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[AD FS en Windows server 2012 R2](../../ad-fs/design/AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)<br /><br />@no__t: 0deploying la granja de servidores federados Descripción de los](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[conceptos de AD FS clave](../../ad-fs/technical-reference/Understanding-Key-AD-FS-Concepts.md)|  
||Si decides usar Microsoft SQL Server para el almacén de configuración de AD FS, asegúrate de implementar una instancia práctica de SQL Server.|[SQL Server](https://technet.microsoft.com/sqlserver) **ADVERTENCIA:** Si quieres crear una granja de AD FS en Windows Server 2012 R2 y usar SQL Server para almacenar los datos de configuración, puedes usar SQL Server 2008 y versiones posteriores, incluyendo SQL Server 2012.|  
|![implementando granja de servidores federados](media/icon_checkboxo.gif)|Conecta tu equipo a un dominio de Active Directory|![deploying granja de servidores federados](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[unir un equipo a un dominio](Join-a-Computer-to-a-Domain.md)|  
|![implementando granja de servidores federados](media/icon_checkboxo.gif)|Inscriba un certificado de capa de sockets seguros \(SSL @ no__t-1 para AD FS.|![deploying granja de servidores federados](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[inscriba un certificado SSL para AD FS](Enroll-an-SSL-Certificate-for-AD-FS.md)|  
|![implementando granja de servidores federados](media/icon_checkboxo.gif)|Instala el servicio de rol de AD FS.|![deploying granja de servidores federados](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[instalar el servicio de rol AD FS](Install-the-AD-FS-Role-Service.md)|  
|![implementando granja de servidores federados](media/icon_checkboxo.gif)|Configura el servidor de federación.|![deploying granja de servidores federados](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[configurar un servidor de Federación](Configure-a-Federation-Server.md)|  
|![implementando granja de servidores federados](media/icon_checkboxo.gif)|Paso opcional: Configure un servidor de Federación con el servicio de registro de dispositivos \(DRS @ no__t-1.|![deploying granja de servidores federados](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[configurar un servidor de Federación con el servicio de registro de dispositivos](Configure-a-federation-server-with-Device-Registration-Service.md)|  
|![implementando granja de servidores federados](media/icon_checkboxo.gif)|Agregue un host \(A @ no__t-1 y el alias \(CNAME @ no__t-3 registro de recursos al sistema de nombres de dominio corporativo \(DNS @ no__t-5 para el servicio de Federación y DRS.|![deploying granja de servidores federados](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[configurar el DNS corporativo para el servicio de Federación y DRS](Configure-Corporate-DNS-for-the-Federation-Service-and-DRS.md)|  
|![implementando granja de servidores federados](media/icon_checkboxo.gif)|Comprueba que esté operativo un servidor de federación.|![deploying granja de servidores federados](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[comprobar que un servidor de Federación está operativo](Verify-That-a-Federation-Server-Is-Operational.md)|  
  

## <a name="see-also"></a>Vea también  
[Implementación de AD FS](../../ad-fs/AD-FS-Deployment.md)  

[Guía de implementación de AD FS de Windows Server 2012 R2](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
  

