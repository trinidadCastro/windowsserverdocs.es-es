---
description: 'Más información acerca de: Stand-Alone servidor de Federación con WID'
ms.assetid: 33b80a3f-67f3-4da7-ac4a-7fd2232fbd5d
title: Servidor de federación independiente mediante con WID
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 2bb6425d77da8e39f35a84df19d1fd19c12a55f9
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97049383"
---
# <a name="stand-alone-federation-server-using-wid"></a>Servidor de federación independiente mediante con WID

Un \- servidor de Federación independiente en Servicios de federación de Active Directory (AD FS) \( AD FS \) consta de un único servidor que hospeda un servicio de Federación configurado para usar el WID de Windows Internal Database \( \) . Esta topología AD FS es para laboratorios de pruebas. No se recomienda para entornos de producción porque tiene un límite de un solo servidor de Federación, y no se puede usar para escalar verticalmente a más servidores.

Si desea agregar más servidores de Federación al laboratorio de pruebas, debe volver a generar el Servicio de federación desde cero implementando cualquiera de las demás topologías que se mencionan más adelante en esta sección. Por lo tanto, se recomienda usar esta topología para un laboratorio de pruebas o un \- entorno de prueba de \- concepto en la red de pruebas privada en la que un único servidor de Federación sea adecuado, tal como se muestra en la siguiente ilustración.

![servidor que usa WID](media/FedServerWID.gif)

## <a name="test-lab-considerations"></a>Consideraciones sobre el laboratorio de pruebas
En esta sección se describen diversas consideraciones sobre la audiencia, las ventajas y las limitaciones que se asocian con esta topología para entornos de laboratorio de pruebas.

### <a name="who-should-use-this-topology"></a>¿Quién debe usar esta topología?

-   Tecnologías de la información que los \( \) profesionales de ti o arquitectos de ti quieren evaluar o desarrollar una prueba de concepto para esta tecnología

### <a name="what-are-the-benefits-of-using-this-topology"></a>¿Cuáles son las ventajas de usar esta topología?

-   Fácil de configurar en un entorno de laboratorio de pruebas

### <a name="what-are-the-limitations-of-using-this-topology"></a>¿Cuáles son las limitaciones del uso de esta topología?

-   Solo un servidor de Federación por Servicio de federación \( no es capaz de escalar verticalmente a una granja de servidores\)

-   No redundante \( solo existe una única instancia de la base de datos de configuración AD FS\)


## <a name="see-also"></a>Consulte también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
