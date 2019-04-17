---
ms.assetid: d04dd17e-a843-46fd-8711-0039918f92d9
title: "Implementar tu AD FS diseñar Plan"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 31c2e048c3c125d0ea60610b049501151d7aa823
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="implementing-your-ad-fs-design-plan"></a>Implementar tu AD FS diseñar Plan

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Las siguientes condiciones ambientales y requisitos son factores importantes en la implementación de tu plan de diseño de los servicios de federación de Active Directory \(AD FS\):  
  
-   **Soporte de los asociados:** normalmente usas AD FS para trabajar con las organizaciones asociadas. Para establecer federación de identidades, determinar las organizaciones con la que quieres constituyen una sociedad. Una vez una implementación de referencia AD FS, operativo con socios implica agregar a asociados, partners, actualizar o eliminar información del partner. Cambios en asociaciones pueden producirse por diversos motivos. Por ejemplo, la implementación de AD FS puede requerir actualizaciones de asociación si su compañero cambia su negocio de forma significativa, la organización se convierte en parte de una organización más grande o federación de las organizaciones o la organización adquiere una compañía diferente. En cualquier escenario en el que federación identidades de varios dominios, tendrás que conocer el \(partners\) dominios que admiten actualmente y todos los dominios que representan a posibles asociados.  
  
-   **Admite la aplicación y los tipos de servicio:** algunas aplicaciones y servicios requieren acceso a recursos del sistema operativo, mientras que otros son "reclamaciones cuenta". Es importante comprender los tipos de aplicaciones y servicios que AD FS admite por lo que puede formular requisitos de administración.  
  
-   **Diagramas de arquitectura lógicos y físicos o topología de implementación:** necesitas saber:  
  
    -   Si los servidores de federación funcionarán en un conjunto de granja servidores o en un solo servidor.  
  
    -   En la red implementa los firewall y proxy.  
  
    -   La ubicación de los recursos y si los usuarios tienen acceso a recursos desde dentro de la organización, fuera de la organización, o ambos.  
  
## <a name="how-to-implement-your-ad-fs-design-using-this-guide"></a>Cómo implementar el diseño de AD FS mediante esta guía  
El siguiente paso para implementar el diseño es determinar el orden en que debe realizarse cada tarea de implementación. Esta guía usa listas de comprobación que te ayudarán a recorrer las tareas de implementación distintos de servidor y una aplicación que son necesarios para implementar el plan de diseño. Listas de comprobación primarios y secundarios se usan según sea necesario para representar el orden en que las tareas para un determinado AD FS se debe procesar el diseño.  
  
Usa las siguientes listas de comprobación de elemento primario en esta sección de la guía para estar familiarizado con las tareas de implementación para implementar el diseño de AD FS preferido de la organización:  
  
-   [Lista de comprobación: Implementar un diseño de inicio de sesión único Web](Checklist--Implementing-a-Web-SSO-Design.md)  
  
-   [Lista de comprobación: Implementar un diseño SSO Web federado](Checklist--Implementing-a-Federated-Web-SSO-Design.md)  
