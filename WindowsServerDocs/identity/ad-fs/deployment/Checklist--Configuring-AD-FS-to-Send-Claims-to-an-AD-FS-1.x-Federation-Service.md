---
ms.assetid: 4b81ac66-3f34-4a39-a8bf-5411131a69c2
title: "Lista de comprobación: configurar AD FS para consumir reclamaciones de AD FS 1.x"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: bdfd06a28f8ccaa04014024a235cd19512adb181
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="checklist-configuring-ad-fs-to-send-claims-to-an-ad-fs-1x-federation-service"></a>Lista de comprobación: Configurar AD FS para enviar notificaciones a un servicio de federación de AD FS 1.x

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012
  
## <a name="checklist-configuring-ad-fs-to-send-claims-to-an-ad-fs-1x-federation-service"></a>Lista de comprobación: Configurar AD FS para enviar notificaciones a un AD FS 1.x servicios de federación de  
Esta lista de comprobación incluye las tareas que son necesarias para configurar los servicios de federación de los servicios de federación de Active Directory \(AD FS\) en Windows Server 2012 para enviar notificaciones que te ayudarán a comprender una AD FS 1. *x* servicios de federación.  
  
> [!NOTE]  
> Completar las tareas en esta lista de comprobación en orden. Cuando un vínculo de referencia tiene un procedimiento, vuelve a este tema después de completar los pasos de este procedimiento para que se puede continuar con las tareas restantes en esta lista de comprobación.  
  
![configurar AD FS para enviar notificaciones](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**lista de comprobación: configurar AD FS para enviar notificaciones a un AD FS 1.x servicios de federación de**  
  
||Tarea|Referencia|  
|-|--------|-------------|  
|![configurar AD FS para enviar notificaciones](media/icon_checkboxo.gif)|Planear para la interoperabilidad entre AD FS en Windows Server 2012 y versiones anteriores de AD FS y obtener que más información sobre el identificador de nombre de tipo de notificación.|![configurar AD FS para enviar notificaciones](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[planeación para la interoperabilidad con AD FS 1.x](https://technet.microsoft.com/library/ff678040.aspx)|  
|![configurar AD FS para enviar notificaciones](media/icon_checkboxo.gif)|Antes de que se puede lograr la interoperabilidad con una versión anterior de AD FS, primero debes crear una confianza de terceros confiar en el servicio de federación de AD FS para el 1 de AD FS. *x* servicios de federación. **Nota:** no se puede crear una relación de confianza con una 1 de AD FS. *x* servicios de federación de mediante el uso de metadatos de federación.<br /><br />Cuando configuras la confianza con el procedimiento en el vínculo a la derecha, debes hacer lo siguiente en el Asistente para agregar confiar terceros confianza para configurar esta confianza para interoperar con un 1 de AD FS. *x* servicios de federación de:<br /><br />1. en la **Seleccionar origen de datos** , seleccione **introducir datos acerca de los usuarios de confianza de terceros confianza manualmente**.<br />2. en la **Elegir perfil** página, seleccione **perfil de AD FS 1.0 y 1.1**.<br />3. en la **configurar URL** página, en **WS\ federación pasivo URL**, tipo el **dirección URL de extremo de servicios de federación** según se define en el 1 de AD FS. *x* servicios de federación del asociado.<br />4. en la **configurar identificadores** página, debajo **identificador de confianza parte Relying**, tipo el **federación servicio URI** según se define en el 1 de AD FS. *x* servicios de federación del asociado.|![configurar AD FS para enviar notificaciones](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[crear un confiar terceros de confianza de forma manual](../../ad-fs/operations/Create-a-Relying-Party-Trust.md)|  
|![configurar AD FS para enviar notificaciones](media/icon_checkboxo.gif)|En la confianza de terceros confianza que creaste anteriormente, debes crear reclamación tipo que se puede entender y consumida por el anuncio de la notificación de reglas que tomar notificaciones entrantes extraídas de un almacén de atributo y pasar, filtrar o transformarlos en un identificador de nombre FS 1. *x* servicios de federación. **Nota:** antes de crear esta regla, asegúrate de que el conjunto de reglas de notificación de que estás creando esta regla tiene una regla que viene antes de que primero se extrae una reclamación de atributo de protocolo ligero de acceso a directorios \(LDAP\) de un almacén de atributo. Esta notificación se usará como entrada a la regla que creas para enviar un 1 de AD FS. *x*\-compatible reclamación. Para obtener más información sobre cómo crear una regla para extraer un atributo LDAP, consulta [crear una regla para enviar atributos LDAP como notificaciones](../../ad-fs/operations/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims.md).|![configurar AD FS para enviar notificaciones](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[crear una regla para enviar un AD FS 1.x reclamación Compatible](../../ad-fs/operations/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md)|  
|![configurar AD FS para enviar notificaciones](media/icon_checkboxo.gif)|Ponte en contacto con el administrador del 1 de AD FS. *x* servicios de federación y el administrador del 1 de AD FS. *x* servicios de federación de configurar una nueva confianza partner de cuenta. Además, proporciona el Administrador con el URI de servicios de federación \ (en los servicios de federación properties\), la dirección URL de extremo WS\ federación pasivo \ (el servicio de federación extremo URL\), y una exportados token\ firma el archivo de certificado \ (con only\ de clave pública). El administrador deberá estos elementos para configurar la relación de confianza.|N\/A|  
  

