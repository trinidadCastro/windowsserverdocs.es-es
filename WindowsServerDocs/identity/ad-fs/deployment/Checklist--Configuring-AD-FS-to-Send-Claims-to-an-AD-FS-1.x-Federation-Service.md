---
ms.assetid: 4b81ac66-3f34-4a39-a8bf-5411131a69c2
title: 'Lista de comprobación: configuración de AD FS para consumir notificaciones de AD FS 1.x'
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: bdfd06a28f8ccaa04014024a235cd19512adb181
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887036"
---
# <a name="checklist-configuring-ad-fs-to-send-claims-to-an-ad-fs-1x-federation-service"></a>Lista de comprobación: Configuración de AD FS para enviar notificaciones a un servicio de federación de AD FS 1.x

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012
  
## <a name="checklist-configuring-ad-fs-to-send-claims-to-an-adfs1x-federation-service"></a>Lista de comprobación: Configuración de AD FS para enviar notificaciones a AD FS 1.x Federation Service  
Esta lista de comprobación incluye las tareas que son necesarias para configurar los servicios de federación de Active Directory \(AD FS\) servicio de federación de Windows Server 2012 para enviar notificaciones a los que se pueden entender mediante AD FS 1. *x* servicio de federación.  
  
> [!NOTE]  
> Complete las tareas de esta lista de comprobación en orden. Cuando un vínculo de referencia le dirija a un procedimiento, vuelva a este tema tras completar los pasos de este procedimiento, de tal forma que pueda continuar con las tareas restantes de la lista de comprobación.  
  
![configurar AD FS para enviar notificaciones](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**lista de comprobación: Configuración de AD FS para enviar notificaciones a AD FS 1.x Federation Service**  
  
||Tarea|Referencia|  
|-|--------|-------------|  
|![configurar AD FS para enviar notificaciones](media/icon_checkboxo.gif)|Planear la interoperabilidad entre AD FS en Windows Server 2012 y versiones anteriores de AD FS y aprenda que más sobre el identificador de nombre de tipo de notificación.|![configurar AD FS para enviar notificaciones](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[planeamiento de la interoperabilidad con AD FS 1.x](https://technet.microsoft.com/library/ff678040.aspx)|  
|![configurar AD FS para enviar notificaciones](media/icon_checkboxo.gif)|Antes de que se puede lograr la interoperabilidad con una versión anterior de AD FS, primero debe crear una entidad de confianza en el servicio de federación de AD FS para AD FS 1. *x* servicio de federación. **Nota:** No se puede crear una relación de confianza con AD FS 1. *x* servicio de federación mediante el uso de los metadatos de federación.<br /><br />Al establecer la relación de confianza mediante el procedimiento en el vínculo a la derecha, debe hacer lo siguiente en el usuario de confianza entidad Asistente para agregar confianza para establecer esta relación de confianza para interoperar con AD FS 1. *x* servicio de federación:<br /><br />1.  En el **Seleccionar origen de datos** , seleccione **introducir datos sobre el usuario de confianza de terceros de confianza manualmente**.<br />2.  En el **Elegir perfil** página, seleccione **perfil de AD FS 1.0 y 1.1**.<br />3.  En el **configurar dirección URL de** página, en **WS\-URL Federation Passive**, escriba el **dirección URL del extremo de servicio de federación** tal como se define en AD FS 1. *x* servicio de federación del asociado.<br />4.  En el **configurar identificadores** página, en **identificador de confianza para usuario autenticado partes**, tipo de la **URI del servicio de federación** tal como se define en AD FS 1. *x* servicio de federación del asociado.|![configurar AD FS para enviar notificaciones](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[crear un Relying Party Trust Manually](../../ad-fs/operations/Create-a-Relying-Party-Trust.md)|  
|![configurar AD FS para enviar notificaciones](media/icon_checkboxo.gif)|En la confianza que creó anteriormente, debe crear reglas que tomar las notificaciones entrantes que se han extraído de un almacén de atributos y pasar a través, filtrar o transformarlos en un identificador de nombre de notificación de tipo que se puede entender y consume el AD de notificación FS 1. *x* servicio de federación. **Nota:** Antes de crear esta regla, asegúrese de que el conjunto de reglas de notificación que va a crear esta regla tiene una regla que viene antes que se extrae en primer lugar un Lightweight Directory Access Protocol \(LDAP\) atributo notificación desde un almacén de atributos. Esta notificación se usará como entrada para la regla que ha creado para enviar AD FS 1. *x*\-notificación compatible con. Para obtener más información sobre cómo crear una regla para extraer un atributo LDAP, consulte [crear una regla para enviar atributos LDAP como notificaciones](../../ad-fs/operations/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims.md).|![configurar AD FS para enviar notificaciones](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[crear una regla para enviar AD FS 1.x notificación compatible con](../../ad-fs/operations/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md)|  
|![configurar AD FS para enviar notificaciones](media/icon_checkboxo.gif)|Póngase en contacto con el Administrador de AD FS 1. *x* servicio de federación y que el Administrador de AD FS 1. *x* establecer una confianza de asociado de cuenta nuevo servicio de federación. Asimismo, proporciona ese administrador con el URI del servicio de federación \(en las propiedades del servicio de federación\), WS\-dirección URL del extremo pasivo de federación \(la dirección URL de punto de conexión de servicio de federación\), y un token de exportado\-archivo de certificado de firma \(con la clave pública solo\). Ese administrador necesitará estos elementos para establecer la relación de confianza.|N\/A|  
  

