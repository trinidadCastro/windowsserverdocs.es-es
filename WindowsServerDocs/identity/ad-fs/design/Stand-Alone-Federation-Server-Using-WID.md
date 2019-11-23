---
ms.assetid: 33b80a3f-67f3-4da7-ac4a-7fd2232fbd5d
title: Servidor de federación independiente mediante WID
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 5fa89a4a57c618fd711234b8770a35750f3099bd
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358956"
---
# <a name="stand-alone-federation-server-using-wid"></a>Servidor de federación independiente mediante WID

Un servidor de Federación\-independiente en Servicios de federación de Active Directory (AD FS) \(AD FS\) consta de un único servidor que hospeda un Servicio de federación configurado para usar la \(WID de Windows Internal Database\). Esta topología AD FS es para laboratorios de pruebas. No se recomienda para entornos de producción porque tiene un límite de un solo servidor de Federación, y no se puede usar para escalar verticalmente a más servidores.  
  
Si desea agregar más servidores de Federación al laboratorio de pruebas, debe volver a generar el Servicio de federación desde cero implementando cualquiera de las demás topologías que se mencionan más adelante en esta sección. Por lo tanto, se recomienda usar esta topología para un laboratorio de pruebas o una\-de\-entorno de concepto en la red de pruebas privada en la que un único servidor de Federación es adecuado, tal como se muestra en la siguiente ilustración.  
  
![servidor que usa WID](media/FedServerWID.gif)  
  
## <a name="test-lab-considerations"></a>Consideraciones sobre el laboratorio de pruebas  
En esta sección se describen diversas consideraciones sobre la audiencia, las ventajas y las limitaciones que se asocian con esta topología para entornos de laboratorio de pruebas.  
  
### <a name="who-should-use-this-topology"></a>¿Quién debe usar esta topología?  
  
-   Tecnologías de la información \(ti\) profesionales o arquitectos de ti que desean evaluar o desarrollar una prueba de concepto para esta tecnología  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>¿Cuáles son las ventajas de usar esta topología?  
  
-   Fácil de configurar en un entorno de laboratorio de pruebas  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>¿Cuáles son las limitaciones del uso de esta topología?  
  
-   Solo un servidor de Federación por Servicio de federación no \(capacidad para escalar verticalmente a una granja\)  
  
-   No redundante \(solo existe una única instancia de la base de datos de configuración de AD FS\)  
  

## <a name="see-also"></a>Consulta también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
