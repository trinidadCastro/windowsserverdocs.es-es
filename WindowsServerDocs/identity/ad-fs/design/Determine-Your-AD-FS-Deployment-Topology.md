---
ms.assetid: f67b0bc9-e5af-4891-9da0-d9be539af42d
title: "Determinar la topología de implementación de AD FS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3300c16be6d516d7ec0bf4d0c3a025e59e6126b6
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="determine-your-ad-fs-deployment-topology"></a>Determinar la topología de implementación de AD FS

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

El primer paso para planear una implementación de servicios de federación de Active Directory \(AD FS\) es determinar la topología de implementación adecuada para satisfacer la única \(SSO\) sign\ en necesidades de la organización. Los temas de esta sección describen las diversas topologías de implementación que se pueden usar con AD FS. También se describen las ventajas y limitaciones asociadas con cada topología de implementación para que pueda seleccionar la topología más adecuada para tus necesidades empresariales específicos.  
  
Antes de leer este tema de la topología de implementación, te recomendamos que primero realices las tareas en el orden en que se muestra en la siguiente tabla.  
  
|Tarea recomendada|Descripción|Referencia|  
|--------------------|---------------|-------------|  
|Revisar cómo se almacenan los datos de AD FS y cómo se replica en otros servidores de federación de una granja de servidores de federación.|Comprender el propósito de y los métodos de replicación que pueden usarse para los datos subyacentes que se almacenan en la base de datos de configuración de AD FS. Este tema presenta los conceptos de la base de datos de configuración y se describen los tipos de base de dos datos: \(WID\) base de datos interna de Windows y Microsoft SQL Server.|[La función de la base de datos de configuración de AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)|  
|Selecciona el tipo de base de datos de configuración de AD FS que implementas en la organización.|Revisa las distintas ventajas y limitaciones que están asociadas con el uso de la base de datos de configuración de AD FS, junto con los distintos escenarios de aplicaciones que admiten WID o SQL Server.|[Consideraciones de la topología de implementación de AD FS](AD-FS-Deployment-Topology-Considerations.md)|  
  
> [!NOTE]  
> Para implementar la redundancia básica, equilibrio de carga y la opción para escalar el \(if required\) servicios de federación, te recomendamos que implementes al menos dos servidores de federación por granja de servidores de federación de todos los entornos de producción, independientemente del tipo de base de datos que se usará.  
  
Cuando haya revisado el contenido en la tabla anterior, continuar con los siguientes temas de esta sección:  
  
-   [Servidor de federación independiente mediante WID](Stand-Alone-Federation-Server-Using-WID.md)  
  
-   [Federación granja de servidores con WID](Federation-Server-Farm-Using-WID-2012.md)  
  
-   [Federación granja de servidores con WID y servidores proxy](Federation-Server-Farm-Using-WID-and-Proxies-2012.md)  
  
-   [Federación granja de servidores con SQL Server](Federation-Server-Farm-Using-SQL-Server-2012.md)  
  
Cuando termine de seleccionar la topología de implementación de AD FS, te recomendamos que revises el tema [planear la capacidad de servidor de AD FS](Planning-for-AD-FS-Server-Capacity.md) para determinar el número de servidores que tendrás que implementar para admitir esta topología recomendadas.  
  
## <a name="see-also"></a>Consulta también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)

