---
ms.assetid: 33b80a3f-67f3-4da7-ac4a-7fd2232fbd5d
title: "Servidor de federación independiente mediante WID"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 9ec4150a7d3adfaac786219d253e1d0898c18204
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="stand-alone-federation-server-using-wid"></a>Servidor de federación independiente mediante WID

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Un servidor de federación stand\ solo en los servicios de federación de Active Directory \(AD FS\) consta de un solo servidor que hospeda un servicio de federación configurada para usar la \(WID\) base de datos interna de Windows. Esta topología de AD FS es para laboratorios de prueba. No se recomienda para entornos de producción porque tiene un límite de servidor de federación de un único y no puede usarse para ajustar la escala a más servidores.  
  
Si quieres agregar servidores de federación adicionales en el laboratorio de prueba, debes volver a compilar el servicio de federación desde cero mediante la implementación de cualquiera de las otras topologías mencionadas más adelante en esta sección. Por lo tanto, te recomendamos que uses esta topología para un laboratorio de pruebas o un entorno de concepto de descripción\ proof\ en la red privada de prueba en el que un servidor de federación solo es adecuado, tal como se muestra en la ilustración siguiente.  
  
![servidor mediante WID](media/FedServerWID.gif)  
  
## <a name="test-lab-considerations"></a>Consideraciones de laboratorio de prueba  
Esta sección describe diversas consideraciones acerca de la audiencia de destino, ventajas y limitaciones que están asociadas con esta topología para entornos de laboratorio de prueba.  
  
### <a name="who-should-use-this-topology"></a>¿Quién debe usar esta topología?  
  
-   \(IT\) profesionales de tecnología de información o arquitectos de TI que quieran evaluar o desarrollar una prueba de concepto para esta tecnología  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>¿Cuáles son las ventajas de usar esta topología?  
  
-   Fácil de configurar en un entorno de laboratorio de prueba  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>¿Cuáles son las limitaciones del uso de esta topología?  
  
-   Servidor de solo federación por servicios de federación de \ (ninguna funcionalidad para ajustar la escala a un farm\)  
  
-   No redundantes \ (una sola instancia de la exists\ de base de datos de configuración de AD FS)  
  

## <a name="see-also"></a>Consulta también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
