---
description: Más información acerca de cómo determinar la topología de implementación de AD FS
ms.assetid: f67b0bc9-e5af-4891-9da0-d9be539af42d
title: Determinar la topología de la implementación de AD FS
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 1c5bd925318a6f18513fa7267c2dfe2ab5381281
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97047703"
---
# <a name="determine-your-ad-fs-deployment-topology"></a>Determinar la topología de la implementación de AD FS

El primer paso en la planeación de una implementación de Servicios de federación de Active Directory (AD FS) \( AD FS \) es determinar la topología de implementación correcta para satisfacer las necesidades de inicio de sesión único \- \( \) de su organización. En los temas de esta sección se describen las distintas topologías de implementación que puede utilizar con AD FS. También se describen las ventajas y limitaciones de cada una de ellas para que puedas seleccionar la más adecuada para tus necesidades empresariales específicas.

Antes de leer este tema sobre las topologías de implementación, te recomendamos que realices las tareas en el orden con que se muestran en la siguiente tabla.

|Tarea recomendada|Descripción|Referencia|
|--------------------|---------------|-------------|
|Revise el modo en que los datos de AD FS se almacenan y replican en otros servidores de Federación de una granja de servidores de Federación.|Comprende cuáles son y qué finalidad tienen los métodos de replicación que puedes usar para los datos subyacentes que se almacenan en la base de datos de configuración de AD FS. En este tema se presentan los conceptos de la base de datos de configuración y se describen los dos tipos de bases de datos: Windows Internal Database \( WID \) y Microsoft SQL Server.|[El papel de la base de datos de configuración de AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)|
|Selecciona el tipo de base de datos de configuración de AD FS que implementarás en tu organización.|Consulta las diferentes ventajas y limitaciones de usar WID o SQL Server como base de datos de configuración de AD FS, además de los diferentes escenarios de aplicaciones que admiten.|[Consideraciones sobre la topología de implementación de AD FS](AD-FS-Deployment-Topology-Considerations.md)|

> [!NOTE]
> Para implementar la redundancia básica, el equilibrio de carga y la opción para escalar el Servicio de federación \( , si es necesario \) , se recomienda que implemente al menos dos servidores de Federación por granja de servidores de Federación para todos los entornos de producción, independientemente del tipo de base de datos que vaya a utilizar.

Cuando hayas revisado el contenido de la tabla anterior, continúa con los siguientes temas de esta sección:

-   [Servidor de federación independiente mediante WID](Stand-Alone-Federation-Server-Using-WID.md)

-   [Granja de servidores de federación con WID](Federation-Server-Farm-Using-WID-2012.md)

-   [Granja de servidores de federación con WID y servidores proxy](Federation-Server-Farm-Using-WID-and-Proxies-2012.md)

-   [Granja de servidores de federación con SQL Server](Federation-Server-Farm-Using-SQL-Server-2012.md)

Una vez que haya terminado de seleccionar la topología de implementación de AD FS, le recomendamos que revise el tema [Planning for AD FS Capacity Server](Planning-for-AD-FS-Server-Capacity.md) para determinar el número recomendado de servidores que debe implementar para admitir esta topología.

## <a name="see-also"></a>Consulte también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)

