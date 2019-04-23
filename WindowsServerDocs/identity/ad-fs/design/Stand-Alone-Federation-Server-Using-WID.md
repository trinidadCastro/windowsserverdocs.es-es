---
ms.assetid: 33b80a3f-67f3-4da7-ac4a-7fd2232fbd5d
title: Servidor de federación independiente mediante WID
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 9ec4150a7d3adfaac786219d253e1d0898c18204
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59876526"
---
# <a name="stand-alone-federation-server-using-wid"></a>Servidor de federación independiente mediante WID

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Un soporte\-servidor de federación independiente de servicios de federación de Active Directory \(AD FS\) consta de un único servidor que hospeda un servicio de federación configurado para usar Windows Internal Database \(WID\). Esta topología de AD FS es para los laboratorios de pruebas. No se recomienda para entornos de producción porque tiene un límite de un solo servidor de federación y no se puede usar para escalar a más servidores.  
  
Si desea agregar más servidores de federación para el laboratorio de pruebas, debe recompilar el servicio de federación desde cero mediante la implementación de cualquiera de las otras topologías que se menciona más adelante en esta sección. Por lo tanto, se recomienda que use esta topología para un laboratorio de pruebas o prueba\-de\-entorno concepto en su red privada de prueba en el que un servidor de federación único es adecuado, tal como se muestra en la siguiente ilustración.  
  
![servidor con WID](media/FedServerWID.gif)  
  
## <a name="test-lab-considerations"></a>Consideraciones de laboratorio de prueba  
Esta sección describen diversas consideraciones acerca de los destinatarios, ventajas y limitaciones asociadas con esta topología para entornos de laboratorio de prueba.  
  
### <a name="who-should-use-this-topology"></a>¿Quién debe usar esta topología?  
  
-   Tecnología de la información \(TI\) profesionales de TI o arquitectos de TI que desean evaluar o desarrollar una prueba de concepto para esta tecnología  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>¿Cuáles son las ventajas de usar esta topología?  
  
-   Fácil de configurar en un entorno de laboratorio de pruebas  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>¿Cuáles son las limitaciones del uso de esta topología?  
  
-   Solo un servidor de federación por cada servicio de federación \(ninguna posibilidad de escalar verticalmente a una granja de servidores\)  
  
-   No redundantes \(existe solo una instancia de la base de datos de configuración de AD FS\)  
  

## <a name="see-also"></a>Vea también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
