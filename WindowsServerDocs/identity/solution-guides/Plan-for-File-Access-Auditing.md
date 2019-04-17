---
ms.assetid: 3ea48a72-20a2-4da4-84e4-26b5728513ce
title: "Planear el archivo de auditoría de acceso"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 65e941f6741298da54186be10bd9c0add115ea14
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="plan-for-file-access-auditing"></a>Planear el archivo de auditoría de acceso

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En este tema se explica la seguridad de configuración que se debe considerar al implementar el Control de acceso dinámico en tu empresa de auditoría de las mejoras de auditoría que se introducen en Windows Server 2012 y el nuevo. La configuración de directiva de auditoría real que implementas dependerá de los objetivos, que pueden incluir el cumplimiento normativo, supervisión, análisis detallado y solución de problemas.  
  
> [!NOTE]  
> Más información acerca de cómo planear e implementar una estrategia de auditoría para la empresa se explica en global de seguridad [planear e implementar directivas de auditoría de seguridad avanzadas](https://go.microsoft.com/fwlink/?LinkID=191139). Para obtener más información sobre cómo configurar e implementar una directiva de auditoría de seguridad, consulta el [directiva de auditoría de seguridad avanzada Step-by-Step guía](https://go.microsoft.com/fwlink/?LinkID=191141).  
  
Las siguientes funcionalidades de auditoría de seguridad en Windows Server 2012 pueden usarse con el Control de acceso dinámico para ampliar tu estrategia de auditoría de seguridad general.  
  
-   **Las directivas de auditoría basadas en expresión**. Control de acceso dinámico te permite crear directivas de auditoría de destino mediante expresiones en función de notificaciones de usuario, el equipo y el recurso. Por ejemplo, podrías crear una directiva de auditoría para realizar un seguimiento de lectura y escritura todas las operaciones en archivos clasificados como impacto empresarial de alto por los empleados que no tienen una distancia de alta seguridad. Las directivas de auditoría basadas en expresión se pueden crear directamente para un archivo o carpeta o de forma centralizada mediante Directiva de grupo. Para obtener más información, consulta [con la auditoría de acceso a objetos Global de directiva de grupo](https://go.microsoft.com/fwlink/?LinkId=241498).  
  
-   **Información adicional de auditoría de acceso a objeto**. Auditoría de acceso de archivo no es nueva en Windows Server 2012. Con la directiva de auditoría adecuado en su lugar, los sistemas operativos Windows y Windows Server generan un evento de auditoría cada vez que un usuario accede a un archivo. Los eventos de acceso a los archivos existentes (4656, 4663) contienen información sobre los atributos del archivo que se tuvo acceso. Esta información puede usarse por herramientas de filtrado de registro de eventos que te ayudarán a identificar los eventos de auditoría más relevantes. Para obtener más información, consulta [manipulación de identificadores de auditoría](https://technet.microsoft.com//library/dd772626(WS.10).aspx) y [Administrador de cuentas de seguridad de auditoría](https://go.microsoft.com/fwlink/?LinkId=241501).  
  
-   **Para obtener más información de los eventos de inicio de sesión de usuario**. Con la directiva de auditoría adecuado en su lugar, sistemas operativos Windows genera un evento de auditoría cada vez que un usuario inicia sesión en un equipo local o remotamente. En Windows Server 2012 o Windows 8, también puede supervisar reclamaciones de usuarios y dispositivos asociados con el token de seguridad de un usuario. Algunos ejemplos pueden incluir autorizaciones departamento, la empresa, proyecto y seguridad. Evento 4626 contiene información sobre estas notificaciones de usuario y reclamaciones de dispositivo, que se pueden aprovechar mediante las herramientas de administración de registro de auditoría para correlacionar los eventos de inicio de sesión de usuario con eventos de acceso de objetos para habilitar el filtrado de eventos basado en atributos de archivo y atributos de usuario. Para obtener más información acerca de la auditoría de inicio de sesión de usuario, consulta [auditar inicio de sesión](https://go.microsoft.com/fwlink/?LinkId=241502).  
  
-   **Seguimiento de cambios para nuevos tipos de objetos protegibles**. Seguimiento de cambios en objetos protegibles puede ser importante en los siguientes escenarios:  
  
    -   **Seguimiento de cambios para las directivas de acceso central y reglas de acceso central**. Directivas de acceso central y las reglas de acceso central definan la directiva central que se puede usar para controlar el acceso a los recursos críticos. Cualquier cambio en estos puede afectar directamente a los permisos de acceso de archivo que se conceden a los usuarios en varios equipos. Por lo tanto, el seguimiento de cambios en las directivas de acceso central y reglas de acceso central puede ser importante para la organización. Debido a las directivas de acceso central y reglas de acceso central se almacenan en los servicios de dominio de Active Directory (AD DS), se pueden auditar los intentos de modificarla, como cambios de auditoría en cualquier otro objeto protegible en AD DS. Para obtener más información, consulta [auditar el acceso de servicio de directorio](https://technet.microsoft.com/library/dd941618(WS.10).aspx).  
  
    -   **Seguimiento de cambios para las definiciones en el diccionario de Reclamación**. Las definiciones de Reclamación incluyen el nombre de la notificación, la descripción y valores posibles. Cualquier cambio en la definición de la notificación puede afectar a los permisos de acceso en los recursos fundamentales. Por lo tanto, seguimiento de cambios de las definiciones de notificación puede ser importante para la organización. Como las directivas de acceso central y reglas de acceso central, definiciones de notificación se almacenan en AD DS; por lo tanto, se pueden auditar como cualquier otro objeto protegible en AD DS. Para obtener más información, consulta [auditar el acceso de servicio de directorio](https://technet.microsoft.com/library/dd941618(WS.10).aspx).  
  
    -   **Seguimiento de cambios para atributos de archivo**. Atributos de archivo determinan qué central aplica reglas de acceso al archivo. Un cambio en los atributos de archivo puede afectar potencialmente las restricciones de acceso en el archivo. Por lo tanto, es importante realizar un seguimiento de cambios en atributos de archivo. Puede controlar los cambios en atributos de archivo en cualquier equipo mediante la configuración de la directiva de auditoría de cambio de directiva de autorización. Para obtener más información, consulta [auditoría de cambio de directiva de autorización](https://go.microsoft.com/fwlink/?LinkId=241504) y [auditoría de acceso a objetos de sistemas de archivos](https://go.microsoft.com/fwlink/?LinkId=241505). En Windows Server 2012, el evento 4911 diferencia cambios de directiva de atributo de archivo de otros eventos de cambio de directiva de autorización.  
  
    -   **Cambio de la directiva de acceso central asociada con un archivo de seguimiento.** Evento 4913 muestra los identificadores de seguridad (SID) de las directivas de acceso central antiguas y nuevas. Cada directiva de acceso central también tiene un nombre descriptivo del usuario que se puede ver mediante este identificador de seguridad. Para obtener más información, consulta [auditoría de cambio de directiva de autorización](https://go.microsoft.com/fwlink/?LinkId=241504).  
  
    -   **Seguimiento de cambios para atributos de usuario y del equipo**. Como archivos, objetos de usuario y del equipo pueden tener atributos y cambios a estos atributos pueden afectar la capacidad del usuario para acceder a archivos. Por lo tanto, puede ser muy valiosa para realizar un seguimiento de cambios en atributos de usuario o equipo. Los objetos de usuario y del equipo se almacenan en AD DS; por lo tanto, se pueden auditar cambios en sus atributos. Para obtener más información, consulta [acceso DS](https://go.microsoft.com/fwlink/?LinkId=241508).  
  
-   **Cambiar la directiva de almacenamiento provisional**. Cambios en las directivas de acceso central pueden afectar a las decisiones de control de acceso en todos los equipos donde se aplican las directivas. Una directiva dinámica puede conceder más acceso del deseado y una directiva demasiado restrictiva podría generar una cantidad excesiva de llamadas de servicio de asistencia. Como resultado, puede ser muy valiosa para comprobar los cambios en una directiva de acceso central antes de aplicar el cambio. Para ese propósito, Windows Server 2012 presenta el concepto de "prueba". Almacenamiento provisional permite que los usuarios puedan comprobar los cambios de directiva propuesta antes de aplicarlas. Para usar el almacenamiento provisional de directiva, se implementan directivas propuestas con las directivas aplicadas, pero provisional directivas no son realmente conceder o denegar permisos. En su lugar, Windows Server 2012 registra un evento de auditoría (4818) cuando el resultado de la comprobación de acceso que usa la directiva provisional es diferente del resultado de una comprobación de acceso que usa la directiva aplicada.  
  


