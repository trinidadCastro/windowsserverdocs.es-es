---
ms.assetid: 3ea48a72-20a2-4da4-84e4-26b5728513ce
title: Planear la auditoría de acceso a archivos
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: ffe8843f9ace604bc0904ba2d1eaef78d2872b99
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861168"
---
# <a name="plan-for-file-access-auditing"></a>Planear la auditoría de acceso a archivos

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La información de este tema explica las mejoras de auditoría de seguridad que se introdujeron en Windows Server 2012 y las nuevas configuraciones de auditoría que debe tener en cuenta al implementar Access Control dinámicos en su empresa. La configuración de la directiva de auditoría que implemente dependerá de sus objetivos, que pueden incluir el cumplimiento de las normas, la supervisión, el análisis forense y la resolución de problemas.  
  
> [!NOTE]  
> En [planeación e implementación de directivas de auditoría de seguridad avanzada](https://go.microsoft.com/fwlink/?LinkID=191139)se explica información detallada acerca de cómo planear e implementar una estrategia de auditoría de seguridad general para su empresa. Para obtener más información sobre la configuración e implementación de una directiva de auditoría de seguridad, consulte la [Guía paso a paso de la Directiva de auditoría de seguridad avanzada](https://go.microsoft.com/fwlink/?LinkID=191141).  
  
Las siguientes capacidades de auditoría de seguridad de Windows Server 2012 se pueden usar con el Access Control dinámico para ampliar la estrategia general de auditoría de seguridad.  
  
-   **Directivas de auditoría basadas en expresiones**. Control de acceso dinámico le permite crear directivas de auditoría concretas mediante el uso de expresiones basadas en notificaciones de usuario, equipo y recursos. Por ejemplo, podría crear una directiva de auditoría para realizar un seguimiento de las operaciones de lectura y escritura en archivos clasificados como de alto impacto empresarial por los empleados que no tienen permisos de alta seguridad. Las directivas de auditoría basadas en expresiones se pueden crear directamente para un archivo o una carpeta, o centralmente a través de la Directiva de grupo. Para obtener más información, vea [Directiva de grupo usar la auditoría de acceso a objetos global](https://go.microsoft.com/fwlink/?LinkId=241498).  
  
-   **Información adicional de auditoría de acceso a objetos**. La auditoría de acceso a archivos no es nueva en Windows Server 2012. Si la directiva de auditoría correcta está vigente, los sistemas operativos Windows y Windows Server generan un evento de auditoría cada vez que un usuario obtiene acceso a un archivo. Los eventos de acceso a archivos existentes (4656, 4663) contienen información sobre los atributos del archivo al que se obtuvo acceso. Las herramientas de filtrado del registro de eventos pueden usar esta información para ayudarlo a identificar los eventos de auditoría más relevantes. Para obtener más información, consulte [auditar la manipulación de control de auditoría](https://technet.microsoft.com//library/dd772626(WS.10).aspx) y [auditar el administrador de cuentas de seguridad](https://go.microsoft.com/fwlink/?LinkId=241501).  
  
-   **Obtenga más información de los eventos de inicio de sesión de usuario**. Si la directiva de auditoría correcta está vigente, los sistemas operativos Windows generarán un evento de auditoría cada vez que un usuario inicie sesión en un equipo, ya sea de forma local o remota. En Windows Server 2012 o Windows 8, también puede supervisar las notificaciones de usuario y de dispositivo asociadas al token de seguridad de un usuario. Algunos ejemplos son autorizaciones de departamento, compañía, proyecto y seguridad. El evento 4626 contiene información sobre estas notificaciones de dispositivo y usuario, que se pueden utilizar con las herramientas de administración de registro de auditoría para establecer una correlación entre los eventos de inicio de sesión de usuario y los eventos de acceso a objetos con el fin de habilitar el filtrado de eventos en función de atributos de archivo y de usuario. Para obtener más información acerca de la auditoría de inicio de sesión de usuario, consulte [Audit Logon](https://go.microsoft.com/fwlink/?LinkId=241502).  
  
-   **Seguimiento de cambios en nuevos tipos de objetos protegibles**. El seguimiento de cambios en objetos protegibles puede ser importante en los siguientes escenarios:  
  
    -   **Seguimiento de cambios en directivas de acceso central y reglas de acceso central**. Las directivas y las reglas de acceso central definen la directiva central que puede utilizarse para controlar el acceso a recursos críticos. Cualquier cambio que se efectúe en ellas puede afectar directamente a los permisos de acceso a archivos que se conceden a los usuarios en varios equipos. Por lo tanto, el seguimiento de cambios en directivas y reglas de acceso central puede ser importante para su organización. Dado que las directivas de acceso central y las reglas de acceso central se almacenan en Active Directory Domain Services (AD DS), puede auditar los intentos de modificarlos, como la auditoría de cambios en cualquier otro objeto protegible en AD DS. Para obtener más información, consulte [auditar el acceso del servicio de directorio](https://technet.microsoft.com/library/dd941618(WS.10).aspx).  
  
    -   **Seguimiento de cambios en definiciones del diccionario de notificaciones**. Las definiciones de notificaciones incluyen el nombre, la descripción y los posibles valores de la notificación. Cualquier cambio en la definición de la notificación puede afectar a los permisos de acceso a recursos críticos. Por lo tanto, el seguimiento de cambios en las definiciones de notificaciones puede ser importante para su organización. Al igual que las directivas de acceso central y las reglas de acceso central, las definiciones de notificaciones se almacenan en AD DS; por lo tanto, se pueden auditar como cualquier otro objeto protegible en AD DS. Para obtener más información, consulte [auditar el acceso del servicio de directorio](https://technet.microsoft.com/library/dd941618(WS.10).aspx).  
  
    -   **Seguimiento de cambios en atributos de archivo**. Los atributos de archivo determinan qué regla de acceso central se aplica al archivo. Un cambio en los atributos de archivo podría afectar a las restricciones de acceso del archivo. Por lo tanto, puede ser importante realizar un seguimiento de los cambios en los atributos de archivo. Puede realizar un seguimiento de los cambios en los atributos de archivo en cualquier equipo configurando la directiva de auditoría de cambio de directiva de autorización. Para obtener más información, vea auditoría de [cambio de directiva de autorización](https://go.microsoft.com/fwlink/?LinkId=241504) y [Auditoría de acceso a objetos para sistemas de archivos](https://go.microsoft.com/fwlink/?LinkId=241505). En Windows Server 2012, el evento 4911 diferencia los cambios de directiva de atributos de archivo de otros eventos de cambio de directiva de autorización.  
  
    -   **Seguimiento de cambios para la Directiva de acceso central asociada a un archivo.** El Evento 4913 muestra los identificadores de seguridad (SID) de las directivas de acceso central nuevas y anteriores. Cada directiva de acceso central también cuenta con un nombre descriptivo del usuario que se puede buscar con el identificador de seguridad. Para obtener más información, consulte [Auditoría de cambio de directiva de autorización](https://go.microsoft.com/fwlink/?LinkId=241504).  
  
    -   **Seguimiento de cambios en atributos de usuario y de equipo**. Al igual que los archivos, los objetos de usuario y de equipo pueden tener atributos, y los cambios en estos atributos pueden afectar a la capacidad del usuario para obtener acceso a los archivos. Por lo tanto, realizar un seguimiento de los cambios realizados en los atributos de usuario o de equipo puede ser muy valioso. Los objetos de usuario y equipo se almacenan en AD DS; por lo tanto, se pueden auditar los cambios en los atributos. Para obtener más información, vea [acceso a DS](https://go.microsoft.com/fwlink/?LinkId=241508).  
  
-   **Almacenamiento provisional de cambios de directiva**. Los cambios realizados en las directivas de acceso central pueden afectar a las decisiones de control de acceso de todos los equipos en los que se aplican las directivas. Una directiva poco estricta podría conceder más acceso del deseado y una directiva demasiado restrictiva podría generar una cantidad excesiva de llamadas al departamento de soporte técnico. Por lo tanto, es muy importante verificar los cambios realizados en una directiva de acceso central antes de aplicarlos. Para ello, Windows Server 2012 presenta el concepto de "almacenamiento provisional". El almacenamiento provisional permite a los usuarios verificar los cambios de directiva propuestos antes de aplicarlos. Para usar el almacenamiento provisional de directivas, se implementan las directivas propuestas con las directivas exigidas, pero las directivas almacenadas provisionalmente en realidad no conceden ni deniegan permisos. En su lugar, Windows Server 2012 registra un evento de auditoría (4818) cada vez que el resultado de la comprobación de acceso que usa la Directiva almacenada provisionalmente es diferente del resultado de una comprobación de acceso que usa la Directiva exigida.  
  


