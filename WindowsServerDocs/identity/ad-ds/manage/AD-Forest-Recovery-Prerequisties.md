---
description: 'Más información acerca de: requisitos previos de la recuperación de Active Directory bosque'
title: Requisitos previos para la planeación de la recuperación de Active Directory bosque
ms.author: daveba
author: iainfoulds
manager: daveba
ms.date: 08/09/2018
ms.topic: article
ms.assetid: c49b40b2-598d-49aa-85b4-766bce960e0d
ms.openlocfilehash: 17efaa69d80f5be04c2536fe8c7dffc7afb09915
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97041763"
---
# <a name="active-directory-forest-recovery-prerequisites"></a>Requisitos previos de la recuperación de Active Directory bosque

> Se aplica a: Windows Server 2016, Windows Server 2012 y 2012 R2, Windows Server 2008 y 2008 R2

En el siguiente documento se describen los requisitos previos que debe conocer antes de diseñar un plan de recuperación de bosque o de intentar una recuperación.

## <a name="assumptions-for-using-this-guide"></a>Suposiciones sobre el uso de esta guía

1. Ha trabajado con un Soporte técnico de Microsoft Professional y:
   - Determinó la causa del error en todo el bosque. En esta guía no se sugiere una causa del error ni se recomienda ningún procedimiento para evitar el error.
   - Se han evaluado las posibles soluciones.
   - Concluido, en la consulta con Soporte técnico de Microsoft, que restaurar todo el bosque a su estado antes de que se produjera el error es la mejor manera de recuperarse del error. En muchos casos, la recuperación del bosque debe ser la última opción.

1. Ha seguido las recomendaciones de prácticas recomendadas de Microsoft para utilizar Active Directory sistema de nombres de dominio (DNS) integrado. En concreto, debe haber una zona DNS integrada en Active Directory para cada dominio Active Directory.
   - Si no es así, puede seguir usando los principios básicos de esta guía para realizar la recuperación del bosque. Sin embargo, tendrá que tomar medidas específicas para la recuperación de DNS en función de su propio entorno. Para obtener más información sobre el uso de DNS integrado Active Directory, consulte [creación de un diseño de infraestructura de DNS](../../ad-ds/plan/Creating-a-DNS-Infrastructure-Design.md).

1. Aunque esta guía está pensada como una guía genérica para la recuperación del bosque, no se incluyen todos los escenarios posibles. Por ejemplo, a partir de Windows Server 2008, hay una versión Server Core, que es una versión completa de Windows Server, pero sin una interfaz de usuario completa. Aunque es ciertamente posible recuperar un bosque que solo consta de controladores de servidor que ejecutan Server Core, en esta guía no se incluyen instrucciones detalladas. Sin embargo, en función de las instrucciones que se describen aquí podrá diseñar las acciones de línea de comandos necesarias.

> [!NOTE]
> Aunque los objetivos de esta guía son recuperar el bosque y mantener o restaurar la funcionalidad de DNS completa, la recuperación puede dar lugar a una configuración de DNS que se cambie de la configuración antes de que se produjera el error. Una vez recuperado el bosque, puede revertir a la configuración de DNS original. En las recomendaciones de esta guía no se describe cómo configurar los servidores DNS para realizar la resolución de nombres de otras partes del espacio de nombres corporativo en las que hay zonas DNS que no están almacenadas en AD DS.

## <a name="concepts-for-using-this-guide"></a>Conceptos para usar esta guía

Antes de empezar a planear la recuperación de un bosque Active Directory, debe estar familiarizado con lo siguiente:

- Conceptos fundamentales de Active Directory
- La importancia de los roles de maestro de operaciones (también conocidas como operaciones de maestro único flexible o FSMO). Estos roles son los siguientes:
  - Maestro de esquema
  - Maestro de nomenclatura de dominios
  - IDENTIFICADOR relativo (RID) maestro
  - Maestro emulador de controlador de dominio principal (PDC)
  - Maestro de infraestructura

Además, debe hacer una copia de seguridad y restaurar AD DS y SYSVOL en un entorno de laboratorio de forma periódica. Para obtener más información, vea [realizar copias de seguridad de los datos de estado del sistema](AD-Forest-Recovery-Procedures.md) y [realizar una restauración no autoritativa de Active Directory Domain Services](AD-Forest-Recovery-Procedures.md).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Recuperación del bosque de AD: requisitos previos](AD-Forest-Recovery-Prerequisties.md)

> [!div class="nextstepaction"]
> [Recuperación de bosque de AD: diseño de un plan de recuperación de bosque personalizado](AD-Forest-Recovery-Devising-a-Plan.md)

> [!div class="nextstepaction"]
> [Recuperación del bosque de AD: identificación del problema](AD-Forest-Recovery-Identify-the-Problem.md)

> [!div class="nextstepaction"]
> [Recuperación del bosque de AD: determinar cómo recuperar](AD-Forest-Recovery-Determine-how-to-Recover.md)

> [!div class="nextstepaction"]
> [Recuperación de bosque de AD: realizar la recuperación inicial](AD-Forest-Recovery-Perform-initial-recovery.md)

> [!div class="nextstepaction"]
> [Recuperación del bosque de AD: procedimientos](AD-Forest-Recovery-Procedures.md)

> [!div class="nextstepaction"]
> [Recuperación de bosque de AD: preguntas más frecuentes](AD-Forest-Recovery-FAQ.md)

> [!div class="nextstepaction"]
> [Recuperación de bosque de AD: recuperación de un solo dominio en un bosque de varios dominios](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)

> [!div class="nextstepaction"]
> [Recuperación de bosque de AD: recuperación de bosque con controladores de dominio de Windows Server 2003](AD-Forest-Recovery-Windows-Server-2003.md)
