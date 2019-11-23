---
title: Elegir un tipo de espacio de nombres
description: En este artículo se describe cómo elegir un tipo de espacio de nombres.
ms.date: 6/5/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: becaab1b4c35492200ad3d75d5f31829d85e10f8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402197"
---
# <a name="choose-a-namespace-type"></a>Elegir un tipo de espacio de nombres

> Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 y Windows Server 2008

Al crear un espacio de nombres, debes elegir uno de los dos tipos de espacio de nombres: un espacio de nombres independiente o un espacio de nombres basado en dominio. Además, si elige un espacio de nombres basado en dominio, debe elegir un modo de espacio de nombres: modo de servidor de Windows 2000 o modo de Windows Server 2008.

## <a name="choosing-a-namespace-type"></a>Elegir un tipo de espacio de nombres

Elige un espacio de nombres independiente si cualquiera de las siguientes condiciones se aplican a tu entorno:

-   Su organización no usa Active Directory Domain Services (AD DS).
-   Deseas aumentar la disponibilidad del espacio de nombres mediante un clúster de conmutación.
-   Debe crear un único espacio de nombres con más de 5.000 carpetas DFS en un dominio que no cumpla los requisitos de un espacio de nombres basado en dominio (modo Windows Server 2008) tal y como se describe más adelante en este tema.

> [!NOTE]
> Para comprobar el tamaño de un espacio de nombres, haz clic con el botón derecho en el espacio de nombres en el árbol de consola de Administración DFS, haz clic en **Propiedades** y luego visualiza el tamaño del espacio de nombres en el cuadro de diálogo **Propiedades de espacio de nombres**. Para obtener más información sobre la escalabilidad de los espacios de nombres DFS, consulta el sitio web de Microsoft [Servicios de archivo](https://technet.microsoft.com/library/cc771548.aspx).

Elige un espacio de nombres basado en dominio si cualquiera de las siguientes condiciones se aplican a tu entorno:

-   Deseas garantizar la disponibilidad del espacio de nombres mediante varios servidores de espacios de nombres.
-   Deseas ocultar el nombre del servidor de espacio de nombres de los usuarios. Esto facilita la sustitución del servidor del espacio de nombres o migrar al espacio de nombres a otro servidor.

## <a name="choosing-a-domain-based-namespace-mode"></a>Elegir un modo de espacio de nombres basado en dominio

Si elige un espacio de nombres basado en dominio, debe elegir si desea usar el modo de servidor de Windows 2000 o el modo de Windows Server 2008. El modo Windows Server 2008 incluye compatibilidad con la enumeración basada en el acceso y una mayor escalabilidad. El espacio de nombres basado en dominio introducido en el servidor de Windows 2000 ahora se conoce como "espacio de nombres basado en dominio (modo de servidor Windows 2000)".

Para usar el modo de Windows Server 2008, el dominio y el espacio de nombres deben cumplir los siguientes requisitos mínimos:

-   El bosque usa Windows Server 2003 o un nivel funcional de bosque superior.
-   El dominio usa el nivel funcional de dominio de Windows Server 2008 o superior.
-   Todos los servidores de espacio de nombres ejecutan Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008.

Si su entorno lo admite, elija el modo de Windows Server 2008 al crear nuevos espacios de nombres basados en dominio. Este modo proporciona características adicionales y escalabilidad, y también elimina la necesidad de migrar un espacio de nombres del modo de servidor de Windows 2000.

Para obtener información sobre cómo migrar un espacio de nombres al modo Windows Server 2008, vea [migrar un espacio de nombres basado en dominio al modo Windows server 2008](migrate-a-domain-based-namespace-to-windows-server-2008-mode.md).

Si su entorno no admite espacios de nombres basados en dominio en modo Windows Server 2008, use el modo de servidor Windows 2000 existente para el espacio de nombres.

## <a name="comparing-namespace-types-and-modes"></a>Comparar tipos de espacios de nombres y modos

En la siguiente tabla, se describen las características de cada tipo de espacio de nombres y modo.

|Característica|Espacio de nombres independiente|Espacio de nombres basado en dominio (modo Windows 2000 Server) |Espacio de nombres basado en dominio (modo Windows Server 2008) | 
|---|---|---|---|
|Ruta de acceso al espacio de nombres|\\\ *ServerName\RootName* |\\\ *NetBIOSDomainName\RootName* <br />\\\ *DNSDomainName\RootName*|\\\ *NetBIOSDomainName\RootName* <br /> \\\ *DNSDomainName\RootName*|
|Ubicación de almacenamiento de información de espacios de nombres|En el registro y en una caché en memoria en el servidor de espacio de nombres|En AD DS y en una caché en memoria en cada servidor de espacio de nombres|En AD DS y en una caché en memoria en cada servidor de espacio de nombres|
|Recomendaciones de tamaño de espacio de nombres|El espacio de nombres puede incluir más de 5000 carpetas con destinos; el límite recomendado es 50 000 carpetas con destinos|El tamaño del objeto de espacio de nombres en AD DS debe ser inferior a 5 megabytes (MB) para mantener la compatibilidad con controladores de dominio que no ejecutan Windows Server 2008. Esto significa que no más de aproximadamente 5000 carpetas con destinos.|El espacio de nombres puede incluir más de 5000 carpetas con destinos; el límite recomendado es 50 000 carpetas con destinos |
|Nivel funcional del bosque de AD DS mínimo|AD DS no es obligatorio|Windows 2000|Windows Server 2003|
|Nivel funcional del dominio de AD DS mínimo|AD DS no es obligatorio|Windows 2000 mixto|Windows Server 2008|
|Servidores de espacio de nombres mínimos compatibles|Windows 2000 Server|Windows 2000 Server|Windows Server 2008|
|Compatibilidad con la enumeración basada en el acceso (si estuviera habilitada)|Sí, requiere el servidor de espacio de nombres Windows Server 2008|No|Sí|
|Métodos compatibles para garantizar la disponibilidad de espacios de nombres|Cree un espacio de nombres independiente en un clúster de conmutación por error.|Use varios servidores de espacios de nombres para hospedar el espacio de nombres. (Los servidores de espacios de nombres deben estar en el mismo dominio).|Use varios servidores de espacios de nombres para hospedar el espacio de nombres. (Los servidores de espacios de nombres deben estar en el mismo dominio).|
|Compatibilidad con el uso de la replicación DFS para replicar destinos de carpeta|Compatible cuando une a un dominio de AD DS|Se admite|Se admite|

## <a name="see-also"></a>Consulte también

-   [Implementar espacios de nombres DFS](deploying-dfs-namespaces.md)
-   [Migrar un espacio de nombres basado en dominio al modo Windows Server 2008](migrate-a-domain-based-namespace-to-windows-server-2008-mode.md)


