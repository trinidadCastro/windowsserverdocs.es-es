---
ms.assetid: d04dd17e-a843-46fd-8711-0039918f92d9
title: Implementación del plan de diseño de AD FS
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 6306b87dd06774bfde5ffc3ff98818d47d0c858f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408371"
---
# <a name="implementing-your-ad-fs-design-plan"></a>Implementación del plan de diseño de AD FS

Las siguientes condiciones y requisitos del entorno son factores importantes en la implementación de su plan de diseño Servicios de federación de Active Directory (AD FS) \(AD FS @ no__t-1:  
  
-   **Asociados admitidos:** Normalmente, se usa AD FS para trabajar con organizaciones asociadas. Para establecer la Federación de identidades, determine las organizaciones con las que desea formar una asociación. Una vez que se implementa una base de referencia AD FS, el funcionamiento de los asociados implica agregar asociados, eliminar asociados y actualizar la información de los asociados. Pueden producirse cambios en las asociaciones por diversos motivos. Por ejemplo, la implementación de AD FS podría requerir actualizaciones de la asociación si el socio cambia su negocio de manera significativa, la organización se convierte en parte de una organización mayor o en una Federación de organizaciones, o bien si la organización se adquiere por otro recíproca. En cualquier escenario en el que se federan identidades de varios dominios, deberá conocer los dominios \(partners @ no__t-1 que actualmente admite y todos los dominios adicionales que representan posibles asociados.  
  
-   **Tipos de aplicación y servicio admitidos:** Algunas aplicaciones y servicios requieren acceso a los recursos del sistema operativo, mientras que otros son "compatibles con notificaciones". Es importante comprender los tipos de aplicaciones y servicios que AD FS admite para que pueda formular los requisitos de administración.  
  
-   **Diagramas de arquitectura lógicos y físicos o topología de implementación:** Necesitará saber:  
  
    -   Si los servidores de Federación funcionarán en un conjunto de servidores de la granja o en un solo servidor.  
  
    -   Donde la red implementa firewalls y servidores proxy.  
  
    -   La ubicación de los recursos y si los usuarios obtienen acceso a los recursos desde dentro de la organización, fuera de la organización o ambos.  
  
## <a name="how-to-implement-your-ad-fs-design-using-this-guide"></a>Cómo implementar el diseño de AD FS mediante esta guía  
El paso siguiente para implementar el diseño consiste en determinar en qué orden se debe realizar cada tarea de implementación. En esta guía se usan listas de comprobación que le guiarán a través de las distintas tareas de implementación de servidores y aplicaciones que son necesarias para implementar el plan de diseño. Las listas de comprobación principales y secundarias se usan según sea necesario para representar el orden en el que se deben procesar las tareas de un diseño de AD FS específico.  
  
Use las siguientes listas de comprobación principales en esta sección de la guía para familiarizarse con las tareas de implementación para implementar el diseño de AD FS preferido de su organización:  
  
-   [Lista de comprobación: Implementar un diseño de SSO web](Checklist--Implementing-a-Web-SSO-Design.md)  
  
-   [Lista de comprobación: Implementar un diseño de SSO web federado](Checklist--Implementing-a-Federated-Web-SSO-Design.md)  
