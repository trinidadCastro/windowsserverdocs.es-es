---
ms.assetid: 41b56704-c6f9-4d29-af97-62123e300565
title: "Revisar los conceptos de diseño de la unidad organizativa"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 0e832d068a4d03316853d8b59e3f2ac4a6ebc816
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="reviewing-ou-design-concepts"></a>Revisar los conceptos de diseño de la unidad organizativa

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La estructura de unidad organizativa (OU) para un dominio incluye lo siguiente:  
  
-   Un diagrama de la jerarquía de la unidad organizativa  
  
-   Una lista de unidades organizativas  
  
-   Para cada unidad organizativa:  
  
    -   El propósito de la unidad organizativa  
  
    -   Una lista de usuarios o grupos que tienen control sobre la unidad organizativa o los objetos en la unidad organizativa  
  
    -   El tipo de control que los usuarios y grupos tienen sobre los objetos en la unidad organizativa  
  
La jerarquía de la unidad organizativa no es necesario reflejar la jerarquía de departamento de la organización o grupo. Unidades organizativas se crean para un propósito específico, como la delegación de administración, la aplicación de la directiva de grupo, o para limitar la visibilidad de objetos.  
  
Puedes diseñar la estructura de unidad organizativa para delegar la administración a los individuos o grupos dentro de la organización que requieren autonomía para administrar sus propios datos y recursos. Unidades organizativas representan los límites administrativos y le permiten controlar el ámbito de autoridad de los administradores de datos.  
  
Por ejemplo, puede crear una unidad organizativa denominada ResourceOU y usarla para almacenar todas las cuentas de equipo que pertenecen al archivo y administrados por un grupo de servidores de impresión. A continuación, puedes configurar la seguridad en la unidad organizativa para que solo los administradores de datos en el grupo tienen acceso a la unidad organizativa. Esto impide que los administradores de los datos de otros grupos altere las cuentas de servidor de impresión y el archivo.  
  
Puedes refinar aún más la estructura de unidad organizativa creando subárboles de unidades organizativas para fines específicos, como la aplicación de la directiva de grupo o para limitar la visibilidad de los objetos protegidos de modo que solo determinados usuarios puedan verlos. Por ejemplo, si es necesario aplicar la directiva de grupo para un grupo selecto de usuarios o recursos, puede agregar los usuarios o recursos a una unidad organizativa y, a continuación, aplicar directiva de grupo para dicha unidad organizativa. También puedes usar la jerarquía de la unidad organizativa habilitar aún más la delegación de control administrativo.  
  
Si bien no hay ningún límite técnica a la cantidad de niveles de la estructura de unidad organizativa, para facilitar la administración recomendamos limitar la estructura de unidad organizativa a una profundidad de no más de 10 niveles. No hay ningún límite técnica en el número de unidades organizativas en cada nivel. Ten en cuenta que Servicios de dominio de Active Directory (AD DS)-aplicaciones habilitadas pueden tener restricciones en el número de caracteres que se usen en el nombre distintivo (es decir, el protocolo ligero de acceso a directorios (LDAP) ruta de acceso completa al objeto en el directorio) o en la profundidad de la unidad organizativa dentro de la jerarquía.  
  
La estructura de unidad organizativa en AD DS no está destinada a ser visibles para los usuarios finales. La estructura de unidad organizativa es una herramienta administrativa para los administradores de servicio y para los administradores de datos, y es fácil cambiar. Continuar revisar y actualizar el diseño de la estructura de unidad organizativa para reflejar los cambios en la estructura administrativa y para admitir la administración basada en directivas.  
  


