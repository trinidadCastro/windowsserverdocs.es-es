---
ms.assetid: 7be1f2cb-02d5-4209-ba79-edf496a88f47
title: Auditoría de acceso a archivos de escenario
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 37a3b17360112d958b59a7e9c3f64aed5e6f6a5b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406998"
---
# <a name="scenario-file-access-auditing"></a>Escenario: auditoría de acceso a archivos

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La auditoría de seguridad es una de las herramientas más eficaces para mantener la seguridad de una empresa. Uno de los principales objetivos de las auditorías de seguridad es el cumplimiento de las normas. Estándares de la industria como Sarbanes Oxley, Health Insurance Portability and Accountability Act (HIPPA) y Payment Card Industry (PCI) exigen que las empresas sigan un estricto conjunto de reglas relacionadas con la seguridad y la privacidad de los datos. Las auditorías de seguridad ayudan a establecer la presencia de dichas directivas y demuestran el cumplimiento de esos estándares. Además, las auditorías de seguridad ayudan a detectar comportamientos anómalos, a identificar y mitigar brechas en las directivas de seguridad y a impedir el comportamiento irresponsable mediante la creación de un registro de la actividad del usuario que puede utilizarse para un análisis forense.  
  
Los requisitos de directiva de auditoría generalmente se impulsan en los siguientes niveles:  
  
-   **Seguridad de la información.** Los seguimientos de auditoría de acceso a archivos a menudo se utilizan para análisis forenses y detección de intrusión. Poder obtener eventos concretos sobre el acceso a información de alto nivel permite a las organizaciones mejorar notablemente su tiempo de respuesta y la exactitud de la investigación.  
  
-   **Directiva organizativa.** Por ejemplo, las organizaciones reguladas por los estándares PCI podrían contar con una directiva central para supervisar el acceso a todos los archivos que están marcados como archivos que contienen información de tarjeta de crédito e información personal identificable (PII).  
  
-   **Directiva departamental.** Por ejemplo, es posible que el departamento de finanzas exija que la posibilidad de modificar ciertos documentos de finanzas (como informes de ganancias trimestrales) se restrinja al departamento de finanzas y, por consiguiente, el departamento desearía supervisar todos los demás intentos de modificar estos documentos.  
  
-   **Directiva empresarial.** Por ejemplo, es posible que los propietarios de empresas deseen supervisar todos los intentos no autorizados de ver datos que pertenecen a sus proyectos.  
  
Además, quizás el departamento de cumplimiento desee supervisar todos los cambios realizados en las directivas de autorización central y los constructos de la directiva como atributos de usuario, equipo y recurso.  
  
Una de las principales consideraciones de las auditorías de seguridad es el costo de recopilar, almacenar y analizar los eventos de auditoría. Si las directivas de auditoría son demasiado amplias, aumenta el volumen de los eventos de auditoría recopilados y esto aumenta los costos. Si las directivas de auditoría son demasiado minuciosas, se corre el riesgo de pasar por alto eventos importantes.  
  
Con Windows Server 2012, puede crear directivas de auditoría mediante el uso de notificaciones y propiedades de recursos. Esto genera directivas de auditoría más avanzadas, más concretas y más fáciles de administrar. Habilita escenarios que, hasta el momento, eran imposibles o demasiado difíciles de llevar a cabo. A continuación se muestran ejemplos de directivas de auditoría que los administradores pueden crear:  
  
-   Auditar a todos los que no tienen permiso de alta seguridad e intentan obtener acceso a un documento HBI. Por ejemplo, Audit | Everyone | All-Access | Resource.BusinessImpact=HBI AND User.SecurityClearance!=High.  
  
-   Auditar a todos los proveedores cuando intenten obtener acceso a documentos que están relacionados con proyectos en los que están trabajando. Por ejemplo, Audit | Everyone | All-Access | User.EmploymentStatus=Vendor AND User.Project Not_AnyOf Resource.Project.  
  
Estas directivas ayudan a regular el volumen de los eventos de auditoría y los limitan a la mayoría de los datos o usuarios relevantes.  
  
Una vez que los administradores crearon y aplicaron las directivas de auditoría, deben tener en cuenta la recolección de información importante de los eventos de auditoría que recopilan. Las eventos de auditoría basados en expresiones pueden ayudar a reducir el volumen de auditorías. Sin embargo, los usuarios necesitan una forma de consultar estos eventos para obtener información significativa y formular preguntas como "¿quién tiene acceso a mis datos de HBI?". o "¿hubo algún intento no autorizado de acceder a datos confidenciales?"  
  
 Windows Server 2012 mejora los eventos de acceso a datos existentes con notificaciones de usuario, equipo y recurso. Estos eventos se generan por servidor. Para proporcionar una vista completa de los eventos de la organización, Microsoft está trabajando con asociados que proporcionan herramientas de recopilación y análisis de eventos, como los Servicios de recopilación de auditorías de System Center Operations Manager.  
  
La figura 4 muestra información general de una directiva de auditoría central.  
  
![guías de soluciones](media/Scenario--File-Access-Auditing/DynamicAccessControl_RevGuide_4.JPG)  
  
**Figura 4** Experiencias de auditoría central  
  
Configurar y utilizar auditorías de seguridad suele implicar los siguientes pasos generales:  
  
1.  Identificar el conjunto correcto de datos y usuarios que se deben supervisar  
  
2.  Crear y aplicar las directivas de auditoría apropiadas  
  
3.  Recopilar y analizar los eventos de auditoría  
  
4.  Administrar y supervisar las directivas que se crearon  
  
## <a name="in-this-scenario"></a>En este escenario  
Los siguientes temas proporcionan más información para este escenario:  
  
-   [Planear la auditoría de acceso a archivos](Plan-for-File-Access-Auditing.md)  
  
-   [Pasos de la demostración de implementación de auditoría &#40;de seguridad con directivas de auditoría central&#41;](Deploy-Security-Auditing-with-Central-Audit-Policies--Demonstration-Steps-.md)  
  
## <a name="BKMK_NEW"></a>Roles y características incluidos en este escenario  
En la tabla siguiente, se enumeran los roles y las características que forman parte de este escenario y se describe la manera en que son compatibles con él.  
  
|Rol/característica|Compatibilidad con este escenario|  
|-----------------|---------------------------------|  
|Rol de Servicios de dominio de Active Directory|AD DS en Windows Server 2012 presenta una plataforma de autorización basada en notificaciones que permite crear notificaciones de usuario y notificaciones de dispositivo, identidad compuesta (notificaciones de usuario más dispositivo), el nuevo modelo de directivas de acceso central (CAP) y el uso de la clasificación de archivos información en decisiones de autorización.|  
|Rol de servicios de archivos y almacenamiento|Los servidores de archivos de Windows Server 2012 proporcionan una interfaz de usuario donde los administradores pueden ver los permisos vigentes para los usuarios de un archivo o carpeta, solucionar problemas de acceso y conceder acceso según sea necesario.|  
  


