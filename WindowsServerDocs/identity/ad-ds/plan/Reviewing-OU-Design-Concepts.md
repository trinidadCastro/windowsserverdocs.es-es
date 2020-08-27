---
ms.assetid: 41b56704-c6f9-4d29-af97-62123e300565
title: Revisar los conceptos de diseño de unidad organizativa
author: iainfoulds
ms.author: iainfou
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: adb770769d9d79ca5f5d9a9a753ecb90914e2309
ms.sourcegitcommit: 1dc35d221eff7f079d9209d92f14fb630f955bca
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/26/2020
ms.locfileid: "88938555"
---
# <a name="reviewing-ou-design-concepts"></a>Revisar los conceptos de diseño de unidad organizativa

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La estructura de la unidad organizativa (OU) de un dominio incluye lo siguiente:

-   Diagrama de la jerarquía de la unidad organizativa

-   Una lista de unidades organizativas

-   Para cada unidad organizativa:

    -   El propósito de la unidad organizativa

    -   Una lista de usuarios o grupos que tienen control sobre la unidad organizativa o los objetos de la unidad organizativa

    -   El tipo de control que los usuarios y grupos tienen sobre los objetos de la unidad organizativa

No es necesario que la jerarquía de la unidad organizativa refleje la jerarquía departamental de la organización o el grupo. Las unidades organizativas se crean para un fin específico, como la delegación de la administración, la aplicación de directiva de grupo o para limitar la visibilidad de los objetos.

Puede diseñar la estructura de la unidad organizativa para delegar la administración a individuos o grupos de la organización que requieran la autonomía de administrar sus propios recursos y datos. Las unidades organizativas representan los límites administrativos y permiten controlar el ámbito de autoridad de los administradores de datos.

Por ejemplo, puede crear una unidad organizativa denominada ResourceOU y usarla para almacenar todas las cuentas de equipo que pertenezcan a los servidores de archivos e impresión administrados por un grupo. Después, puede configurar la seguridad en la unidad organizativa para que solo los administradores de datos del grupo tengan acceso a la unidad organizativa. Esto impide que los administradores de datos de otros grupos manipulen las cuentas del servidor de archivos y de impresión.

Puede refinar la estructura de la unidad organizativa mediante la creación de subárboles de unidades organizativas para fines específicos, como la aplicación de directiva de grupo o para limitar la visibilidad de los objetos protegidos para que solo determinados usuarios puedan verlos. Por ejemplo, si necesita aplicar directiva de grupo a un grupo selecto de usuarios o recursos, puede agregar esos usuarios o recursos a una unidad organizativa y, a continuación, aplicar directiva de grupo a esa unidad organizativa. También puede usar la jerarquía de unidades organizativas para habilitar la delegación adicional del control administrativo.

Aunque no hay ningún límite técnico en cuanto al número de niveles de la estructura de la unidad organizativa, se recomienda limitar la estructura de la unidad organizativa a una profundidad de no más de 10 niveles. No hay ningún límite técnico para el número de unidades organizativas en cada nivel. Tenga en cuenta que las aplicaciones habilitadas para Active Directory Domain Services (AD DS) pueden tener restricciones en el número de caracteres usados en el nombre distintivo (es decir, la ruta de acceso del Protocolo ligero de acceso a directorios (LDAP) al objeto en el directorio) o en la profundidad de la unidad organizativa dentro de la jerarquía.

La estructura de la unidad organizativa de AD DS no está pensada para ser visible para los usuarios finales. La estructura de la unidad organizativa es una herramienta administrativa para los administradores de servicios y para los administradores de datos, y es fácil de cambiar. Continúe revisando y actualizando el diseño de la estructura de la unidad organizativa para reflejar los cambios en la estructura administrativa y para admitir la administración basada en directivas.



