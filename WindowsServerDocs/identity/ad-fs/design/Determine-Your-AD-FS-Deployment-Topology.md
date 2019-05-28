---
ms.assetid: f67b0bc9-e5af-4891-9da0-d9be539af42d
title: Determinar la topología de la implementación de AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 06cc4bd37905f6bb7afbc513ffce216104654aba
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191482"
---
# <a name="determine-your-ad-fs-deployment-topology"></a>Determinar la topología de la implementación de AD FS

El primer paso para planear una implementación de Active Directory Federation Services \(AD FS\) consiste en determinar la topología de implementación adecuada para satisfacer el inicio de sesión único\-en \(SSO\) necesidades de su organización. Los temas de esta sección describen las distintas topologías de implementación que puede usar con AD FS. También se describen las ventajas y limitaciones de cada una de ellas para que puedas seleccionar la más adecuada para tus necesidades empresariales específicas.  
  
Antes de leer este tema sobre las topologías de implementación, te recomendamos que realices las tareas en el orden con que se muestran en la siguiente tabla.  
  
|Tarea recomendada|Descripción|Referencia|  
|--------------------|---------------|-------------|  
|Revise cómo se almacenan los datos de AD FS y cómo se replican en otros servidores de federación en una granja de servidores de federación.|Comprende cuáles son y qué finalidad tienen los métodos de replicación que puedes usar para los datos subyacentes que se almacenan en la base de datos de configuración de AD FS. En este tema se presentan los conceptos de la base de datos de configuración y se describen los dos tipos de base de datos: Windows Internal Database \(WID\) y Microsoft SQL Server.|[El papel de la base de datos de configuración de AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)|  
|Selecciona el tipo de base de datos de configuración de AD FS que implementarás en tu organización.|Consulta las diferentes ventajas y limitaciones de usar WID o SQL Server como base de datos de configuración de AD FS, además de los diferentes escenarios de aplicaciones que admiten.|[Consideraciones sobre la topología de implementación de AD FS](AD-FS-Deployment-Topology-Considerations.md)|  
  
> [!NOTE]  
> Para implementar redundancia básica, equilibrio de carga y la opción para escalar el servicio de federación \(si es necesario\), le recomendamos que implemente al menos dos servidores de federación por granja de servidores de federación para todos los entornos de producción, independientemente del tipo de base de datos que va a utilizar.  
  
Cuando hayas revisado el contenido de la tabla anterior, continúa con los siguientes temas de esta sección:  
  
-   [Servidor de federación independiente mediante WID](Stand-Alone-Federation-Server-Using-WID.md)  
  
-   [Granja de servidores de federación con WID](Federation-Server-Farm-Using-WID-2012.md)  
  
-   [Granja de servidores de federación con WID y servidores proxy](Federation-Server-Farm-Using-WID-and-Proxies-2012.md)  
  
-   [Granja de servidores de federación con SQL Server](Federation-Server-Farm-Using-SQL-Server-2012.md)  
  
Cuando termine de seleccionar la topología de implementación de AD FS, se recomienda que revise el tema [planear la capacidad de servidor de AD FS](Planning-for-AD-FS-Server-Capacity.md) para determinar el número recomendado de servidores que necesite implementar para admitir esta topología.  
  
## <a name="see-also"></a>Vea también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)

