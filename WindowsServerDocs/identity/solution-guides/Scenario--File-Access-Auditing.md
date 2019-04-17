---
ms.assetid: 7be1f2cb-02d5-4209-ba79-edf496a88f47
title: "Escenario de auditoría de acceso del archivo"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 93d78bbefce38173198f991543fb3a06d145b373
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="scenario-file-access-auditing"></a>Escenario: Auditoría de acceso del archivo

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Auditoría de seguridad es una de las herramientas más eficaces para ayudar a mantener la seguridad de una empresa. Uno de los principales objetivos de auditoría de seguridad es el cumplimiento normativo. Los estándares del sector, como Sarbanes Oxley, seguros de salud portabilidad y responsabilidad (HIPAA) y del sector de la tarjeta de pago (PCI) requieren que las empresas sigue un estricto conjunto de reglas relacionadas con la privacidad y seguridad de los datos. Ayuda de auditorías de seguridad de establecer la presencia de estas directivas y comprobar el cumplimiento de estos estándares. Además, las auditorías de seguridad ayudan a detectar el comportamiento anómalo, identificar y mitigar lagunas en las directivas de seguridad y disuadir irresponsable comportamiento mediante la creación de una pista de actividad de usuario que puede usarse para realizar análisis detallado.  
  
Por lo general, los requisitos de directivas de auditoría se controlan en los siguientes niveles:  
  
-   **Seguridad de la información.** Registros de auditoría de acceso de archivo se suelen usar para la detección de intrusiones y análisis detallada. Poder recibir eventos dirigidos acerca del acceso a la información de gran valor vamos a las organizaciones mejoran considerablemente su precisión de investigación y de tiempo de respuesta.  
  
-   **Directiva de la organización.** Por ejemplo, las organizaciones reguladas por los estándares PCI podrían tener una directiva central para controlar el acceso a todos los archivos que están marcados como que contiene información de la tarjeta de crédito e información personalmente identificable (PII).  
  
-   **Directiva de departamento.** Por ejemplo, el departamento de finanzas puede requerir que la capacidad de modificar algunos documentos de finanzas (por ejemplo, un informe de ganancias trimestrales) restringirse al departamento de finanzas y, por consiguiente, el departamento de quiera supervisar todos los otros intentos para cambiar estos documentos.  
  
-   **Directiva de empresa.** Por ejemplo, los propietarios de negocios quiera supervisar todos los intentos no autorizados para ver los datos que pertenecen a sus proyectos.  
  
Además, el departamento de cumplimiento quiera supervisar los cambios en todas las directivas de autorización central y construcciones de directiva como usuario, el equipo y atributos de los recursos.  
  
Una de las principales consideraciones de auditoría de seguridad es el costo de recopilar, almacenar y analizar los eventos de auditoría. Si las directivas de auditoría son demasiado amplias, sube el volumen de eventos de auditoría que se recopila y aumenta los costos. Si las directivas de auditoría son demasiado estrechas, corre el riesgo de falta de eventos importantes.  
  
Con Windows Server 2012, puedes crear directivas de auditoría mediante el uso de notificaciones y las propiedades de recurso. Esto se lleva a las directivas de auditoría más enriquecida, más específico y más fácil de administrar. Es viable en escenarios que, hasta ahora, no era posible eran muy difíciles de realizar. Los siguientes son ejemplos de directivas de auditoría que los administradores pueden crear:  
  
-   Auditar todos los usuarios que no tiene una distancia de alta seguridad e intenta obtener acceso a un documento Repercusión. Por ejemplo, la auditoría | Todo el mundo | Todo acceso | Resource.BusinessImpact=HBI y User.SecurityClearance!=High.  
  
-   Todos los proveedores de auditoría cuando intentan acceder a los documentos que están relacionados con los proyectos que no están trabajando. Por ejemplo, la auditoría | Todo el mundo | Todo acceso | User.EmploymentStatus=Vendor y User.Project Not_AnyOf Resource.Project.  
  
Estas directivas ayudan a regular el volumen de eventos de auditoría y limitarlos a solo los datos más relevantes o los usuarios.  
  
Después de que los administradores crear y aplicar las directivas de auditoría, la siguiente consideración para ellos gleaning información significativa de los eventos de auditoría que recopilan. Eventos de auditoría basadas en expresión ayudan a reducir el volumen de auditorías. Sin embargo, los usuarios necesitan una forma de consultar estos eventos para información significativa y pregunta, como "que tiene acceso a Mis datos Repercusión?" O "¿Hubo un intento no autorizado para acceder a datos confidenciales?"  
  
 Windows Server 2012 mejora eventos de acceso a datos existentes a las reclamaciones de usuario, el equipo y el recurso. Estos eventos se generan en por servidor. Para proporcionar una vista completa de los eventos en toda la organización, Microsoft está trabajando con nuestros asociados para proporcionar la recopilación de eventos y las herramientas de análisis, como los servicios de recopilación de auditorías en System Center operación Manager.  
  
Figura 4 se muestra una descripción general de una directiva de auditoría central.  
  
![guías de solución](media/Scenario--File-Access-Auditing/DynamicAccessControl_RevGuide_4.JPG)  
  
**Figura 4** experiencias de auditoría Central  
  
Configurar y consumir auditorías de seguridad normalmente implican los siguientes pasos generales:  
  
1.  Identificar el conjunto correcto de los datos y los usuarios a supervisar  
  
2.  Crear y aplicar directivas de auditoría adecuado  
  
3.  Recopilar y analizar los eventos de auditoría  
  
4.  Administrar y supervisar las directivas que se crearon  
  
## <a name="in-this-scenario"></a>En este escenario  
Los temas siguientes proporcionan instrucciones adicionales para este escenario:  
  
-   [Planear el archivo de auditoría de acceso](Plan-for-File-Access-Auditing.md)  
  
-   [Implementar la auditoría de seguridad con directivas de auditoría Central & #40; pasos de demostración & #41;](Deploy-Security-Auditing-with-Central-Audit-Policies--Demonstration-Steps-.md)  
  
## <a name="BKMK_NEW"></a>Roles y características incluidas en este escenario  
La siguiente tabla enumera los roles y características que forman parte de este escenario y describe cómo apoyan.  
  
|Rol o característica|¿Cómo admite este escenario|  
|-----------------|---------------------------------|  
|Rol de servicios de dominio Directory activo|AD DS en Windows Server 2012 presenta una plataforma de notificaciones de autorización que permite la creación de notificaciones de usuario y reclamaciones de dispositivo, identidad compuesto, (usuario más notificaciones de dispositivo), nuevo modelo de acceso central (PAC) de directivas y el uso de información de clasificación de archivo en las decisiones de autorización.|  
|Rol de servicios de archivos y almacenamiento|Servidores de archivos en Windows Server 2012 proporcionan una interfaz de usuario donde los administradores pueden ver los permisos eficaces para los usuarios de un archivo o carpeta y solucionar problemas de acceso y conceder acceso según sea necesario.|  
  


