---
ms.assetid: 41b56704-c6f9-4d29-af97-62123e300565
title: Revisar los conceptos de diseño de unidad organizativa
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: f05104466c1cedcfbc8d94060ffa8fbfd9d18033
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832176"
---
# <a name="reviewing-ou-design-concepts"></a>Revisar los conceptos de diseño de unidad organizativa

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La estructura de unidad organizativa (OU) para un dominio incluye lo siguiente:  
  
-   Un diagrama de la jerarquía de unidades Organizativas  
  
-   Una lista de unidades organizativas  
  
-   Para cada unidad organizativa:  
  
    -   El propósito de la unidad organizativa  
  
    -   Una lista de usuarios o grupos que tienen control sobre la unidad organizativa o los objetos de la unidad organizativa  
  
    -   El tipo de control que los usuarios y grupos tienen sobre los objetos en la unidad organizativa  
  
La jerarquía de unidad organizativa no es necesario reflejar la jerarquía de la organización o grupo de departamento. Las unidades organizativas se crean para un propósito específico, como la delegación de administración, la aplicación de directiva de grupo, o para limitar la visibilidad de objetos.  
  
Puede diseñar la estructura de unidades Organizativas para delegar la administración a individuos o grupos dentro de su organización que requieren la autonomía para administrar sus propios recursos y los datos. Las unidades organizativas representan los límites administrativos y le permiten controlar el ámbito de autoridad de los administradores de datos.  
  
Por ejemplo, puede crear una unidad organizativa denominada ResourceOU y usarla para almacenar todas las cuentas de equipo que pertenecen al archivo y administrados por un grupo de servidores de impresión. A continuación, puede configurar la seguridad en la unidad organizativa para que solo los administradores de datos del grupo tengan acceso a la unidad organizativa. Esto impide que los administradores de datos de otros grupos de alteración con las cuentas de servidor de impresión y archivo.  
  
Puede refinar su estructura de OU mediante la creación de los subárboles de unidades organizativas para fines específicos, como la aplicación de directiva de grupo o para limitar la visibilidad de objetos protegidos por lo que sólo determinados usuarios puedan verlos. Por ejemplo, si tiene que aplicar la directiva de grupo a un grupo selecto de usuarios o los recursos, puede agregar esos usuarios o los recursos a una unidad organizativa y, a continuación, aplicar directiva de grupo a esa unidad organizativa. También puede usar la jerarquía de unidades Organizativas para habilitar la delegación de control administrativo aún más.  
  
Aunque no hay ningún límite técnico para el número de niveles en la estructura de unidades Organizativas, facilidad de administración se recomienda que limite su estructura de OU hasta una profundidad de no más de 10 niveles. No hay ningún límite técnico para el número de unidades organizativas en cada nivel. Tenga en cuenta que Active Directory Domain Services (AD DS): las aplicaciones habilitadas podrían tener restricciones en el número de caracteres utilizado en el nombre distintivo (es decir, el protocolo ligero de acceso a directorios (LDAP) ruta de acceso completa al objeto en el directorio) o en el Profundidad de la unidad organizativa dentro de la jerarquía.  
  
La estructura de OU en AD DS no está pensada para ser visibles para los usuarios finales. La estructura de OU es una herramienta administrativa para los administradores de servicio y los administradores de datos, y resulta fácil cambiar. Continuar revisar y actualizar el diseño de la estructura OU para reflejar los cambios en la estructura administrativa y para admitir la administración basada en directivas.  
  


