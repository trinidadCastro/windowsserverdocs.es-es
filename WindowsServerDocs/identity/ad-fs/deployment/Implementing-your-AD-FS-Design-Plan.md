---
ms.assetid: d04dd17e-a843-46fd-8711-0039918f92d9
title: Implementación del plan de diseño de AD FS
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 31c2e048c3c125d0ea60610b049501151d7aa823
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59830786"
---
# <a name="implementing-your-ad-fs-design-plan"></a>Implementación del plan de diseño de AD FS

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Las siguientes condiciones ambientales y los requisitos son factores importantes en la implementación de los servicios de federación de Active Directory \(AD FS\) plan de diseño:  
  
-   **Socios compatibles:** Normalmente, usar AD FS para que funcione con organizaciones asociadas. Para establecer la federación de identidades, determinar las organizaciones con el que quiera formar una asociación. Después de una implementación de línea base AD FS en su lugar, trabajar con socios implica agregar a socios comerciales, eliminando a los asociados y actualizar la información de partner. Los cambios realizados en las asociaciones pueden producirse por diversas razones. Por ejemplo, la implementación de AD FS puede requerir actualizaciones de asociación si su compañero cambia de forma significativa su negocio, su organización se convierte en parte de una organización más grande o de una federación de las organizaciones o su organización se adquiere por otro empresa. En cualquier escenario en el que federar las identidades desde varios dominios, necesitará saber los dominios \(asociados\) que admiten actualmente y todos los dominios adicionales que representan posibles asociados.  
  
-   **Aplicaciones admitidas y tipos de servicio:** Algunas aplicaciones y servicios requieren acceso a recursos del sistema operativo, mientras que otros son "notificaciones." Es importante entender los tipos de aplicaciones y servicios compatibles con AD FS para que puede formular los requisitos de administración.  
  
-   **Diagramas de arquitectura lógicos y físicos o topología de implementación:** Necesitará saber:  
  
    -   Si los servidores de federación funcionará en un conjunto de servidores de granja o en un único servidor.  
  
    -   La red donde implementa firewalls y servidores proxy.  
  
    -   La ubicación de los recursos y si los usuarios tienen acceso a recursos dentro de su organización, fuera de la organización, o ambos.  
  
## <a name="how-to-implement-your-ad-fs-design-using-this-guide"></a>Cómo implementar el diseño de AD FS mediante esta guía  
El siguiente paso para implementar el diseño es determinar en qué orden se debe realizar cada tarea de implementación. En esta guía se usan listas de comprobación que le guiarán a través de las distintas tareas de implementación de servidores y aplicaciones que son necesarias para implementar el plan de diseño. Listas de comprobación primarios y secundarios se utilizan según sea necesario para representar el orden en que las tareas de un AD FS específico se debe procesar el diseño.  
  
Use las listas de comprobación primaria de esta sección de la guía para familiarizarse con las tareas de implementación para implementar el diseño de AD FS preferido de su organización:  
  
-   [Lista de comprobación: Implementar un diseño SSO Web](Checklist--Implementing-a-Web-SSO-Design.md)  
  
-   [Lista de comprobación: Implementar un diseño de SSO Web federado](Checklist--Implementing-a-Federated-Web-SSO-Design.md)  
