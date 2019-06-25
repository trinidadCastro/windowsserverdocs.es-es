---
ms.assetid: 7b22adfa-298d-413c-88d0-1231825b7d4d
title: Introducción a los escenarios de control de acceso dinámico
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: f70bd138a16b23f1fbf7bfabc98ee184e3b9ab37
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825576"
---
# <a name="dynamic-access-control-scenario-overview"></a>Control de acceso dinámico: Información general sobre el escenario

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En Windows Server 2012, puede aplicar la regulación de datos entre los servidores de archivos para controlar quién puede acceder a información y auditar quién ha accedido. El control de acceso dinámico permite:  
  
-   Identificar los datos mediante la clasificación automática y manual de archivos. Por ejemplo, podría etiquetar datos en servidores de archivos en toda la organización.  
  
-   Controlar el acceso a los archivos mediante la aplicación de directivas de red de seguridad que utilicen directivas de acceso central. Por ejemplo, podría definir quién puede acceder a la información de mantenimiento en la organización.  
  
-   Auditar el acceso a los archivos por medio de directivas de auditoría centrales de informes de cumplimiento y análisis forenses. Por ejemplo, podría identificar quién ha obtenido acceso a información muy confidencial.  
  
-   Aplicar la protección de Rights Management Services (RMS) mediante cifrado RMS automático para documentos de Microsoft Office confidenciales. Por ejemplo, podría configurar RMS para cifrar todos los documentos que contengan información de la Ley de transferencia y responsabilidad de seguros de salud (HIPAA).  
  
El conjunto de características Control de acceso dinámico se basa en inversiones en infraestructura que pueden usar también asociados y aplicaciones de línea de negocio, y las características pueden proporcionar un gran valor a las organizaciones que usan Active Directory. Esta infraestructura incluye:  
  
-   Un nuevo motor de autorización y auditoría para Windows que puede procesar expresiones condicionales y directivas centrales.  
  
-   Compatibilidad con la autenticación Kerberos para notificaciones de usuario y notificaciones de dispositivo.  
  
-   Mejoras en la infraestructura de clasificación de archivos (FCI).  
  
-   Compatibilidad con la extensibilidad RMS para que los asociados puedan proporcionar soluciones para cifrar archivos que no sean de Microsoft.  
  
## <a name="in-this-scenario"></a>En este escenario  
Este conjunto de contenido incluye los siguientes escenarios y orientación:  
  
## <a name="BKMK_APP"></a>Guía básica de contenido de Control de acceso dinámico  
  
|Escenario|Evaluar|Planear|Implementar|Operar|  
|------------|------------|--------|----------|-----------|  
|**Escenario: Directiva de acceso central**<br /><br />La creación de directivas de acceso central para los archivos permite a las organizaciones implementar y administrar de manera centralizada las directivas de autorización que incluyen expresiones condicionales mediante notificaciones de usuario, notificaciones de dispositivo y propiedades de recursos. Estas directivas se basan en requisitos de regulación empresarial y de cumplimiento. Estas directivas se crean y se hospedan en Active Directory, lo que facilita su administración e implementación.<br /><br />**Implementación de notificaciones entre bosques**<br /><br />En Windows Server 2012, AD DS mantiene un "diccionario de notificaciones" en cada bosque y todos los tipos que se usan en el bosque se definen en el nivel de bosque de Active Directory de notificaciones. Hay numerosos escenarios en los que es necesario que una entidad de seguridad atraviese un límite de confianza. En este escenario se describe el modo en que una notificación atraviesa un límite de confianza.|[Control de acceso dinámico: Información general del escenario](Dynamic-Access-Control--Scenario-Overview.md)<br /><br />[Implementar notificaciones en bosques](Deploy-Claims-Across-Forests.md)|[Plan: Una implementación de directiva de acceso Central](assetId:///0311a76d-d66c-4ddb-ade6-af586a2ad82f)<br /><br />-   [Proceso para asignar una solicitud empresarial a una directiva de acceso central](assetId:///0311a76d-d66c-4ddb-ade6-af586a2ad82f#BKMK_1)<br />-   [Delegación de administración para el Control de acceso dinámico](assetId:///0311a76d-d66c-4ddb-ade6-af586a2ad82f#BKMK_3.1)<br />-   [Mecanismos de excepción para la planeación de directivas de acceso Central](assetId:///0311a76d-d66c-4ddb-ade6-af586a2ad82f#BKMK_3.2)<br /><br />Procedimientos recomendados para usar notificaciones de usuario<br /><br />-   [Elegir la configuración correcta para habilitar las notificaciones en el dominio de usuario](assetId:///0311a76d-d66c-4ddb-ade6-af586a2ad82f#BKMK_DC_OP3)<br />-   [Operaciones para habilitar las notificaciones de usuario](assetId:///0311a76d-d66c-4ddb-ade6-af586a2ad82f#BKMK_3.4.2)<br />-   [Consideraciones sobre el uso de notificaciones de usuario en el servidor de archivos ACL discrecionales sin usar directivas de acceso Central](assetId:///0311a76d-d66c-4ddb-ade6-af586a2ad82f#BKMK_5)<br /><br />[Uso de notificaciones de dispositivo y grupos de seguridad de dispositivos](assetId:///0311a76d-d66c-4ddb-ade6-af586a2ad82f#BKMK_DeviceClaims)<br /><br />-   [Consideraciones sobre el uso de notificaciones de dispositivo estáticas](assetId:///0311a76d-d66c-4ddb-ade6-af586a2ad82f#BKMK_4.1)<br />-   [Operaciones para habilitar las notificaciones de dispositivo](assetId:///0311a76d-d66c-4ddb-ade6-af586a2ad82f#BKMK_4.3)<br /><br />Herramientas de implementación<br /><br />-   [Kit de herramientas de clasificación de datos](https://go.microsoft.com/fwlink/?LinkId=%20244300)|[Implementar una directiva de acceso Central &#40;pasos de demostración&#41;](Deploy-a-Central-Access-Policy--Demonstration-Steps-.md)<br /><br />[Implementar notificaciones en bosques &#40;pasos de demostración&#41;](Deploy-Claims-Across-Forests--Demonstration-Steps-.md)|-Modelado una directiva de acceso central|  
|**Escenario: Auditoría de acceso a archivos**<br /><br />La auditoría de seguridad es una de las herramientas más eficaces para mantener la seguridad de una empresa. Uno de los principales objetivos de las auditorías de seguridad es el cumplimiento de las normas. Por ejemplo, los estándares de la industria como Sarbanes Oxley, HIPPA y Payment Card Industry (PCI) exigen que las empresas sigan un estricto conjunto de reglas relacionadas con la seguridad y la privacidad de los datos. Las auditorías de seguridad ayudan a establecer la presencia o la ausencia de dichas directivas y prueban el cumplimiento o incumplimiento de estos estándares. Además, las auditorías de seguridad ayudan a detectar comportamientos anómalos, a identificar y mitigar brechas en la directiva de seguridad y a impedir el comportamiento irresponsable al crear un registro de la actividad del usuario que puede utilizarse para un análisis forense.|[Escenario: Auditoría de acceso a archivos](Scenario--File-Access-Auditing.md)|[Plan para el archivo de auditoría de acceso](Plan-for-File-Access-Auditing.md)|[Implementar la auditoría de seguridad con directivas de auditoría Central &#40;pasos de demostración&#41;](Deploy-Security-Auditing-with-Central-Audit-Policies--Demonstration-Steps-.md)|-   [Supervisar las directivas de acceso Central aplicables en un servidor de archivos](https://technet.microsoft.com/library/jj574188.aspx)<br />-   [Supervisar las directivas de acceso Central asociadas con los archivos y carpetas](https://technet.microsoft.com/library/jj574198.aspx)<br />-   [Supervisar los atributos de los recursos en archivos y carpetas](https://technet.microsoft.com/library/jj574208.aspx)<br />-   [Tipos de notificación de Monitor](https://technet.microsoft.com/library/jj574086.aspx)<br />-   [Supervisar las notificaciones de usuario y dispositivo durante el inicio de sesión](https://technet.microsoft.com/library/jj574082.aspx)<br />-   [Directiva de acceso Central de supervisión y las definiciones de reglas](https://technet.microsoft.com/library/jj574115.aspx)<br />-   [Definiciones de atributos de recurso de Monitor](https://technet.microsoft.com/library/jj574155.aspx)<br />-   [Supervisar el uso de dispositivos de almacenamiento extraíbles](https://technet.microsoft.com/library/jj574128.aspx).|  
|**Escenario: Asistencia para acceso denegado**<br /><br />En la actualidad, cuando los usuarios intentan acceder a un archivo remoto en el servidor de archivos, la única indicación que obtendrán es que tienen el acceso denegado. Como consecuencia, se generan solicitudes al departamento de soporte técnico o a los administradores de TI para que solucionen el problema y, a menudo, los administradores tienen dificultades para obtener el contexto adecuado de los usuarios, lo que complica incluso más la solución del problema. <br />En Windows Server 2012, el objetivo es intentar ayudar al trabajador de información y propietario empresarial de los datos para tratar con el acceso denegado problema antes de TI implicados y cuando este interviene, proporcionar toda la información correcta para una rápida resolución. Uno de los desafíos para lograr este objetivo es que no hay ningún modo centralizado para tratar con acceso denegado y cada aplicación lo aborda diferente y, por tanto, en Windows Server 2012, uno de los objetivos es mejorar la experiencia de acceso denegado para el Explorador de Windows.|[Escenario: Asistencia para acceso denegado](Scenario--Access-Denied-Assistance.md)|[Plan para la asistencia para acceso denegado](assetId:///b169f0a4-8b97-4da8-ae4a-c8f1986d19e1)<br /><br />-   [Determine el modelo de asistencia para acceso denegado](assetId:///b169f0a4-8b97-4da8-ae4a-c8f1986d19e1#BKMK_1.1)<br />-   [Determinar quién debe controlar las solicitudes de acceso](assetId:///b169f0a4-8b97-4da8-ae4a-c8f1986d19e1#BKMK_12)<br />-   [Personalizar el mensaje de asistencia para acceso denegado](assetId:///b169f0a4-8b97-4da8-ae4a-c8f1986d19e1#BKMK_13)<br />-   [Planear excepciones](assetId:///b169f0a4-8b97-4da8-ae4a-c8f1986d19e1#BKMK_1.4)<br />-   [Determinar la asistencia para acceso denegado cómo se implementa](assetId:///b169f0a4-8b97-4da8-ae4a-c8f1986d19e1#BKMK_1.5)|[Implementar la asistencia para acceso denegado &#40;pasos de demostración&#41;](Deploy-Access-Denied-Assistance--Demonstration-Steps-.md)||  
|**Escenario: Cifrado de documentos de Office basado en la clasificación**<br /><br />La protección de la información confidencial se centra principalmente en mitigar el riesgo para la organización. Diversas normas de cumplimiento, como la HIPAA o el estándar de seguridad de datos para la industria de tarjetas de pago (PCI-DSS), dictan el cifrado de la información y hay numerosos motivos empresariales para cifrar la información empresarial confidencial. Sin embargo, cifrar la información resulta costoso y podría afectar a la productividad empresarial. Por lo tanto, las organizaciones tienden a adoptar distintos enfoques y prioridades para cifrar su información. <br />Para admitir este escenario, Windows Server 2012 proporciona la capacidad de cifrar automáticamente los archivos de Windows Office confidenciales según su clasificación. Esto se realiza a través de tareas de administración de archivos que invocan la protección del Servidor de Active Directory Rights Management Services (AD RMS) de los documentos confidenciales unos pocos segundos después de haber identificado el archivo como confidencial en el servidor de archivos.|[Escenario: Cifrado de documentos de Office basado en la clasificación](Scenario--Classification-Based-Encryption-for-Office-Documents.md)|[Planear la implementación para el cifrado basado en la clasificación de documentos](assetId:///14714ba6-d6a2-45e4-aae5-d3318817e52a)|[Implementar el cifrado de archivos de Office &#40;pasos de demostración&#41;](Deploy-Encryption-of-Office-Files--Demonstration-Steps-.md)||  
|**Escenario: Obtenga información sobre los datos mediante el uso de clasificación**<br /><br />En la mayoría de las organizaciones ha ido aumentando la dependencia de los datos y los recursos de almacenamiento. Los administradores de TI se enfrentan al creciente desafío que supone la supervisión de infraestructuras de almacenamiento cada vez más grandes y más complejas, al mismo tiempo que se les asigna la responsabilidad de garantizar que el costo total de propiedad se mantenga en un nivel razonable. La administración de recursos de almacenamiento ya no se limita al volumen o la disponibilidad de datos, sino que implica además cumplir las directivas de la compañía y saber cómo se consume el almacenamiento para permitir un uso y un cumplimiento eficaces que mitiguen el riesgo. La infraestructura de clasificación de archivos permite obtener una idea clara de los datos mediante la automatización de los procesos de clasificación con el fin de poder administrar los datos de manera más eficaz. La infraestructura de clasificación de archivos proporciona los siguientes métodos de clasificación: manual, mediante programación y automática. Este escenario se centra en el método de clasificación automática de archivos.|[Escenario: Obtenga información sobre los datos mediante el uso de clasificación](Scenario--Get-Insight-into-Your-Data-by-Using-Classification.md)|[Planificar la clasificación automática de archivos](assetId:///e3c3bb4b-3034-42b7-b391-8ef5f5851955)|[Implementar la clasificación automática de archivos &#40;pasos de demostración&#41;](Deploy-Automatic-File-Classification--Demonstration-Steps-.md)||  
|**Escenario: Implementar la retención de información en servidores de archivos**<br /><br />Un período de retención es la cantidad de tiempo que un documento debería conservarse antes de que expire. El período de retención puede ser distinto en función de la organización. Puede clasificar archivos en una carpeta según tengan un período de retención a corto, medio o largo plazo y, a continuación, asignar el período de tiempo para cada tipo. Quizás desee conservar un archivo indefinidamente mediante una retención legal. <br />La infraestructura de clasificación de archivos y el Administrador de recursos del servidor de archivos utilizan tareas de administración de archivos y la clasificación de archivos para aplicar períodos de retención para un conjunto de archivos. Puede asignar un período de retención en una carpeta y, a continuación, usar una tarea de administración de archivos para configurar la duración de un período de retención asignado. Cuando los archivos de la carpeta están a punto de expirar, el propietario del archivo recibe un correo electrónico de notificación. También puede clasificar un archivo con el estado de retención legal para que la tarea de administración de archivos no haga expirar el archivo.|[Escenario: Implementar la retención de información en servidores de archivos](Scenario--Implement-Retention-of-Information-on-File-Servers.md)|[Planificar la retención de información en servidores de archivos](assetId:///edf13190-7077-455a-ac01-f534064a9e0c)|[Implementación de la retención de información en servidores de archivos &#40;pasos de demostración&#41;](Deploy-Implementing-Retention-of-Information-on-File-Servers--Demonstration-Steps-.md)||  
  
> [!NOTE]  
> No se admite el control de acceso dinámico en ReFS (Sistema de archivos resistente).  
  
## <a name="BKMK_LINKS"></a>Vea también  
  
|Tipo de contenido|Referencias|  
|----------------|--------------|  
|**Evaluación del producto**|-   [Guía de los revisores de Control de acceso dinámico](https://go.microsoft.com/fwlink/?LinkId=244309)<br />-   [Instrucciones para desarrolladores de Control de acceso dinámico](https://go.microsoft.com/fwlink/?LinkId=245870)|  
|**Planeamiento**|-   [Planear una implementación de directiva de acceso Central](assetId:///0311a76d-d66c-4ddb-ade6-af586a2ad82f)<br />-   [Plan para el archivo de auditoría de acceso](Plan-for-File-Access-Auditing.md)|  
|**Implementación**|-   [Implementación de Active Directory](https://go.microsoft.com/fwlink/p/?LinkID=238318)<br />-   [Archivo y la implementación de servicios de almacenamiento](https://go.microsoft.com/fwlink/?LinkID=24430)|  
|**Operaciones**|[Referencia de PowerShell de Control de acceso dinámico](https://go.microsoft.com/fwlink/?LinkId=243150)|  
|**Herramientas y configuración**|[Kit de herramientas de clasificación de datos](https://go.microsoft.com/fwlink/?LinkID=244300)|  
|**Recursos de la comunidad**|[Foro de servicios de directorio](https://social.technet.microsoft.com/Forums/winserverDS/threads)|  
  

