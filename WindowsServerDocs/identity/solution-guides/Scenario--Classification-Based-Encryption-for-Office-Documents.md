---
ms.assetid: 73542e1c-53ef-4ddb-89b1-bc563b2bfb49
title: Cifrado basado en la clasificación de escenarios para documentos de Office
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: ab1b277b83369cf2e4ef4be5aa467dea8b2d2f84
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357440"
---
# <a name="scenario-classification-based-encryption-for-office-documents"></a>Escenario: Cifrado de documentos de Office basado en la clasificación

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La protección de la información confidencial se centra principalmente en mitigar el riesgo para la organización. Diversas normas de cumplimiento, como la HIPAA (Health Insurance Portability and Accountability Act) o el estándar de seguridad de datos para la industria de tarjetas de pago (PCI-DSS), dictan el cifrado de la información y hay numerosos motivos empresariales para cifrar la información empresarial confidencial. Sin embargo, cifrar la información resulta costoso y podría afectar a la productividad empresarial. Por lo tanto, las organizaciones tienden a adoptar distintos enfoques y prioridades para cifrar su información.  
  
## <a name="BKMK_OVER"></a>Descripción del escenario  
 Windows Server 2012 proporciona la capacidad de cifrar automáticamente los archivos de Microsoft Office confidenciales, en función de su clasificación. Esto se realiza a través de tareas de administración de archivos que invocan la protección mediante Active Directory Rights Management Services (AD RMS) de los documentos confidenciales unos pocos segundos después de haber identificado el archivo como confidencial en el servidor de archivos. Esto se realiza mediante las tareas continuas de administración de archivos en el servidor de archivos.  
  
El cifrado de AD RMS proporciona otra capa de protección para los archivos. Aunque una persona con acceso a un archivo confidencial lo envíe sin querer por correo electrónico, el archivo está protegido por el cifrado de AD RMS. Los usuarios que quieran acceder al archivo deben autenticarse primero en un servidor de AD RMS para recibir la clave de descifrado. En la ilustración siguiente se muestra este proceso.  
  
![guías de soluciones](media/Scenario--Classification-Based-Encryption-for-Office-Documents/DynamicAccessControl_RevGuide_6.JPG)  
  
**Figura 6** Protección de RMS basada en la clasificación  
  
La compatibilidad con formatos de archivo que no son de Microsoft está disponible a través de terceros. Una vez protegido un archivo con el cifrado de AD RMS, las características de administración de datos como la clasificación basada en búsquedas o en contenido ya no están disponibles para ese archivo.  
  
## <a name="in-this-scenario"></a>En este escenario  
La siguiente es información que está disponible para este escenario:  
  
-   [Consideraciones de planeación para el cifrado de documentos de Office](assetId:///14714ba6-d6a2-45e4-aae5-d3318817e52a)  
  
-   [Pasos de demostración para implementar &#40;el cifrado de archivos de Office&#41;](Deploy-Encryption-of-Office-Files--Demonstration-Steps-.md)  
  
-   [Control de acceso dinámico: Información general sobre el escenario](Dynamic-Access-Control--Scenario-Overview.md)  
  
## <a name="BKMK_NEW"></a>Roles y características incluidos en este escenario  
En la tabla siguiente, se enumeran los roles y las características que forman parte de este escenario y se describe la manera en que son compatibles con él.  
  
|Rol/característica|Compatibilidad con este escenario|  
|-----------------|---------------------------------|  
|Rol Servicio de dominio de Active Directory (AD DS)|AD DS proporciona una base de datos distribuida que almacena y administra información acerca de los recursos de red y datos específicos de las aplicaciones habilitadas para el uso de directorios. En este escenario, AD DS en Windows Server 2012 presenta una plataforma de autorización basada en notificaciones que permite la creación de notificaciones de usuario y notificaciones de dispositivo, identidad compuesta (notificaciones de usuario más dispositivo), un nuevo modelo de directivas de acceso central y el uso de información de clasificación de archivos en decisiones de autorización.|  
|Rol de servicios de archivos y almacenamiento<br /><br />Administrador de recursos del servidor de archivos|Servicios de archivos y almacenamiento incluye tecnologías que permiten configurar y administrar uno o más servidores de archivos que proporcionan ubicaciones centrales en la red para almacenar archivos y compartirlos con los usuarios. Si los usuarios de la red necesitan tener acceso a los mismos archivos y aplicaciones, o si la administración centralizada de archivos y copias de seguridad es importante en su organización, deberá configurar uno o más equipos como servidor de archivos. Para ello, debe agregar a los equipos el rol Servicios de archivos y almacenamiento y los servicios de rol pertinentes. En este escenario, los administradores de servidores de archivo pueden configurar las tareas de administración de archivos que invocan la protección mediante AD RMS de los documentos confidenciales unos pocos segundos después de haber identificado el archivo como confidencial en el servidor de archivos (tareas continuas de administración de archivos en el servidor de archivos).|  
|Rol Active Directory Rights Management Services (AD RMS)|AD RMS permite que personas y administradores especifiquen permisos de acceso a documentos, libros y presentaciones a través de las directivas de Information Rights Management (IRM). Esto ayuda a impedir que personas no autorizadas impriman, reenvíen o copien la información confidencial. Después de que ha restringido el permiso para un archivo mediante IRM, se aplican las restricciones de acceso y uso sin importar dónde se encuentre la información, ya que el permiso para un archivo se almacena en el archivo de documento mismo. En este escenario, el cifrado de AD RMS proporciona otra capa de protección para los archivos. Aunque una persona con acceso a un archivo confidencial lo envíe sin querer por correo electrónico, el archivo está protegido por el cifrado de AD RMS. Los usuarios que quieran acceder al archivo deben autenticarse primero en un servidor de AD RMS para recibir la clave de descifrado.|  
  


