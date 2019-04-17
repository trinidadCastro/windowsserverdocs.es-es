---
ms.assetid: 73542e1c-53ef-4ddb-89b1-bc563b2bfb49
title: "Escenario clasificación cifrado para documentos de Office"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 38e058f36522ba6a2c81694cb883d0946b04adda
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="scenario-classification-based-encryption-for-office-documents"></a>Escenario: El cifrado basado en la clasificación para documentos de Office

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Protección de información confidencial es principalmente acerca de cómo mitigar el riesgo de la organización. Las normativas de cumplimiento distintos, por ejemplo, la portabilidad seguros de salud y responsabilidad (HIPAA) y pago tarjeta Industry Data Security Standard (PCI DSS), dictan cifrado de información, y existen muchos motivos comerciales para cifrar la información confidencial de la empresa. Sin embargo, cifrado de la información es muy costoso y perjudique de productividad empresarial. Por lo tanto, las organizaciones suelen tener diferentes enfoques y prioridades para cifrar su información.  
  
## <a name="BKMK_OVER"></a>Descripción del escenario  
 Windows Server 2012 proporciona la capacidad de cifrar archivos con información confidencial de Microsoft Office, en función de su clasificación de forma automática. Esto se logra con tareas de administración de archivos que invocar la protección de Active Directory Rights Management Services (AD RMS) para los documentos con información confidencial unos pocos segundos después de que el archivo se identifica como un archivo con información confidencial en el servidor de archivos. Esto se facilita las tareas de administración de archivos continua en el servidor de archivos.  
  
Cifrado de AD RMS proporciona otra capa de protección de archivos. Incluso si una persona con acceso a un archivo con información confidencial involuntariamente envía ese archivo a través de correo electrónico, el archivo está protegido mediante el cifrado de AD RMS. Los usuarios que quieran tener acceso al archivo deben autenticarse primero a un servidor de AD RMS para recibir la clave de descifrado. La figura siguiente muestra este proceso.  
  
![guías de solución](media/Scenario--Classification-Based-Encryption-for-Office-Documents/DynamicAccessControl_RevGuide_6.JPG)  
  
**Figura 6** protección basada en la clasificación de RMS  
  
Compatibilidad con formatos de archivo no son de Microsoft está disponible a través de los proveedores no son de Microsoft. Después de un archivo se ha protegido mediante el cifrado de AD RMS, funciones de administración de datos como la clasificación en función de búsqueda o contenido ya no están disponibles para ese archivo.  
  
## <a name="in-this-scenario"></a>En este escenario  
El siguiente es la orientación que está disponible para este escenario:  
  
-   [Consideraciones de diseño para el cifrado de documentos de Office](assetId:///14714ba6-d6a2-45e4-aae5-d3318817e52a)  
  
-   [Implementar el cifrado de archivos de Office & #40; pasos de demostración & #41;](Deploy-Encryption-of-Office-Files--Demonstration-Steps-.md)  
  
-   [Control de acceso dinámico: Información general del escenario](Dynamic-Access-Control--Scenario-Overview.md)  
  
## <a name="BKMK_NEW"></a>Roles y características incluidas en este escenario  
La siguiente tabla enumera los roles y características que forman parte de este escenario y describe cómo apoyan.  
  
|Rol o característica|¿Cómo admite este escenario|  
|-----------------|---------------------------------|  
|Rol de Active Directory los servicios de dominio (AD DS)|AD DS proporciona una base de datos distribuida que almacena y administra la información sobre recursos de red y datos específicos de la aplicación desde aplicaciones habilitadas para directorio. En este escenario, AD DS en Windows Server 2012 presenta una plataforma de notificaciones de autorización que permite la creación de notificaciones de usuario y reclamaciones de dispositivo, identidad compuesto (usuario más notificaciones de dispositivo), un nuevo modelo de directivas de acceso central y el uso de información de clasificación de archivos en las decisiones de autorización.|  
|Rol de servicios de archivos y almacenamiento<br /><br />Administrador de recursos del servidor de archivos|Servicios de archivos y almacenamiento proporciona tecnologías para ayudar a configurar y administrar uno o varios servidores de archivos que proporcionan ubicaciones centrales de la red donde puedes almacenar archivos y compartirlos con los usuarios. Si los usuarios de red necesitan acceso a los mismos archivos y las aplicaciones, o si centralizada copia de seguridad y administración de archivos es importante para la organización, debe configurar uno o más equipos como un servidor de archivos agregando la función File and Storage Services y los servicios de rol adecuado a los equipos. En este escenario, los administradores de servidor de archivos pueden configurar las tareas de administración de archivos que invocar la protección de AD RMS para los documentos con información confidencial unos pocos segundos después de que el archivo se identifica como un archivo con información confidencial en el servidor de archivos (tareas de administración de archivos continua en el servidor de archivos).|  
|Rol de Active Directory Rights Management Services (AD RMS)|AD RMS permite a usuarios y administradores (a través de directivas de Information Rights Management (IRM)) para especificar los permisos de acceso a documentos, libros y presentaciones. Esto ayuda a evitar que información confidencial impriman, reenviar o copien personas no autorizadas. Después de que se ha restringido permiso para un archivo mediante el uso de IRM, se aplican las restricciones de acceso y el uso sea cual sea la información, porque el permiso a un archivo se almacena en el propio archivo de documento. En este escenario, el cifrado de AD RMS proporciona otra capa de protección de archivos. Incluso si una persona con acceso a un archivo con información confidencial involuntariamente envía ese archivo a través de correo electrónico, el archivo está protegido mediante el cifrado de AD RMS. Los usuarios que quieran tener acceso al archivo deben autenticarse primero a un servidor de AD RMS para recibir la clave de descifrado.|  
  


