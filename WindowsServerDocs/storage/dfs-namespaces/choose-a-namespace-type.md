---
title: Choose a Namespace Type
description: En este artículo se describe cómo elegir un tipo de espacio de nombres.
ms.date: 6/5/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: b4addd18629bd54cd9d5fc2df5c660c621e735de
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2020
ms.locfileid: "85473792"
---
# <a name="choose-a-namespace-type"></a>Choose a namespace type

> Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 y Windows Server 2008

Al crear un espacio de nombres, debe elegir uno de los dos tipos de espacio de nombres: un espacio de nombres independiente o un espacio de nombres basado en dominio. Además, si elige un espacio de nombres basado en dominio, debe elegir un modo de espacio de nombres: modo de servidor de Windows 2000 o modo de Windows Server 2008.

## <a name="choosing-a-namespace-type"></a>Selección de un tipo de espacio de nombres

Elija un espacio de nombres independiente si se da alguna de las siguientes condiciones en el entorno:

-   La organización no usa Active Directory Domain Services (AD DS).
-   Desea aumentar la disponibilidad del espacio de nombres mediante un clúster de conmutación por error.
-   Debe crear un único espacio de nombres con más de 5.000 carpetas DFS en un dominio que no cumpla los requisitos de un espacio de nombres basado en dominio (modo Windows Server 2008) tal y como se describe más adelante en este tema.

> [!NOTE]
> Para comprobar el tamaño de un espacio de nombres, haga clic con el botón secundario en el espacio de nombres en el árbol de la consola de administración de DFS, haga clic en **propiedades**y vea el tamaño del espacio de nombres en el cuadro de diálogo **propiedades del espacio de nombres** . Para obtener más información acerca de la escalabilidad del espacio de nombres DFS, consulte los [servicios de archivo](https://technet.microsoft.com/library/cc771548.aspx)del sitio web de Microsoft.

Elija un espacio de nombres basado en el dominio si se da alguna de las siguientes condiciones en el entorno:

-   Desea garantizar la disponibilidad del espacio de nombres utilizando varios servidores de espacio de nombres.
-   Desea ocultar el nombre del servidor de espacio de nombres a los usuarios. Esto facilita la sustitución del servidor de espacio de nombres o la migración del espacio de nombres a otro servidor.

## <a name="choosing-a-domain-based-namespace-mode"></a>Selección de un espacio de nombres basado en el dominio

Si elige un espacio de nombres basado en dominio, debe elegir si desea usar el modo de servidor de Windows 2000 o el modo de Windows Server 2008. El modo Windows Server 2008 incluye compatibilidad con la enumeración basada en el acceso y una mayor escalabilidad. El espacio de nombres basado en el dominio incluido con Windows 2000 Server se llama ahora "espacio de nombres basado en el dominio (modo Windows 2000 Server)".

Para usar el modo de Windows Server 2008, el dominio y el espacio de nombres deben cumplir los siguientes requisitos mínimos:

-   El bosque utiliza el nivel funcional de bosque de Windows Server 2003 o superior.
-   El dominio usa el nivel funcional de dominio de Windows Server 2008 o superior.
-   Todos los servidores de espacio de nombres ejecutan Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008.

Si su entorno lo admite, elija el modo de Windows Server 2008 al crear nuevos espacios de nombres basados en dominio. Este modo proporciona características adicionales y escalabilidad además de eliminar la posible necesidad de migrar el espacio de nombres desde el modo Windows 2000 Server.

Para obtener información sobre cómo migrar un espacio de nombres al modo Windows Server 2008, vea [migrar un espacio de nombres basado en dominio al modo Windows server 2008](migrate-a-domain-based-namespace-to-windows-server-2008-mode.md).

Si su entorno no admite espacios de nombres basados en dominio en modo Windows Server 2008, use el modo de servidor Windows 2000 existente para el espacio de nombres.

## <a name="comparing-namespace-types-and-modes"></a>Comparación de los tipos y modos de espacios de nombres

En la tabla siguiente se describen las características de cada tipo y modo de espacio de nombres.

|Característica|Espacio de nombres independiente|Espacio de nombres basado en el dominio (modo Windows 2000 Server) |Espacio de nombres basado en dominio (modo Windows Server 2008) |
|---|---|---|---|
|Ruta de acceso de espacio de nombres|\\\ *ServerName\RootName* |\\\ *NetBIOSDomainName\RootName* <br />\\\ *DNSDomainName\RootName*|\\\ *NetBIOSDomainName\RootName* <br /> \\\ *DNSDomainName\RootName*|
|Ubicación de almacenamiento de la información del espacio de nombres|En el Registro y en una caché de memoria del servidor de espacio de nombres|En AD DS y en una caché de memoria en cada servidor de espacio de nombres|En AD DS y en una caché de memoria en cada servidor de espacio de nombres|
|Recomendaciones de tamaño del espacio de nombres|El espacio de nombres puede contener más de 5.000 carpetas con destinos. el límite recomendado es 50.000 carpetas con destinos|El tamaño del objeto de espacio de nombres en AD DS debe ser inferior a 5 megabytes (MB) para mantener la compatibilidad con los controladores de dominio que no ejecutan Windows Server 2008. Esto significa que no se admiten más de 5.000 carpetas con destinos aproximadamente.|El espacio de nombres puede contener más de 5.000 carpetas con destinos. el límite recomendado es 50.000 carpetas con destinos |
|Nivel funcional del bosque mínimo AD DS|AD DS no es necesario|Windows 2000|Windows Server 2003|
|Nivel funcional del dominio mínimo AD DS|AD DS no es necesario|Windows 2000 mixto|Windows Server 2008|
|Servidores de espacio de nombres admitidos mínimos|Windows 2000 Server|Windows 2000 Server|Windows Server 2008|
|Compatibilidad con la enumeración basada en el acceso (si está habilitada)|Sí, requiere el servidor de espacio de nombres de Windows Server 2008|No|Sí|
|Métodos admitidos para garantizar la disponibilidad de un espacio de nombres|Se crea un espacio de nombres independiente en un clúster de conmutación por error.|Use varios servidores de espacio de nombres para hospedar el espacio de nombres. (Los servidores de espacio de nombres deben encontrarse en el mismo dominio.)|Use varios servidores de espacio de nombres para hospedar el espacio de nombres. (Los servidores de espacio de nombres deben encontrarse en el mismo dominio.)|
|Compatibilidad con el uso de la replicación DFS para replicar destinos de carpeta|Se admite cuando se une a un dominio de AD DS|Compatible|Compatible|

## <a name="additional-references"></a>Referencias adicionales

-   [Implementar espacios de nombres DFS](deploying-dfs-namespaces.md)
-   [Migrar un espacio de nombres basado en dominio al modo Windows Server 2008](migrate-a-domain-based-namespace-to-windows-server-2008-mode.md)


