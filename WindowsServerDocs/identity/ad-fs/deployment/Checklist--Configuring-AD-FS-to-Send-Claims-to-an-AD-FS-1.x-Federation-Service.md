---
ms.assetid: 4b81ac66-3f34-4a39-a8bf-5411131a69c2
title: 'Lista de comprobación: configuración de AD FS para consumir notificaciones de AD FS 1. x'
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: d522dddf99d71d8921db511af6f00488e424736e
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854128"
---
# <a name="checklist-configuring-ad-fs-to-send-claims-to-an-ad-fs-1x-federation-service"></a>Lista de comprobación: configurar AD FS para enviar notificaciones a un servicio de federación de AD FS 1.x

  
## <a name="checklist-configuring-ad-fs-to-send-claims-to-an-adfs1x-federation-service"></a>Lista de comprobación: configuración de AD FS para enviar notificaciones a un AD FS 1. x Servicio de federación  
Esta lista de comprobación incluye las tareas necesarias para configurar la \(de Servicios de federación de Active Directory (AD FS) AD FS\) Servicio de federación en Windows Server 2012 para enviar notificaciones que puede comprender un AD FS 1. *x* servicio de Federación.  
  
> [!NOTE]  
> Complete las tareas de esta lista de comprobación en orden. Cuando un vínculo de referencia le dirija a un procedimiento, vuelva a este tema tras completar los pasos de este procedimiento, de tal forma que pueda continuar con las tareas restantes de la lista de comprobación.  
  
![configurar AD FS para enviar una lista de comprobación de notificaciones](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**: configuración de AD FS para enviar notificaciones a una Servicio de federación AD FS 1. x**  
  
||Tarea|Referencia|  
|-|--------|-------------|  
|![configurar AD FS para enviar notificaciones](media/icon_checkboxo.gif)|Planee la interoperabilidad entre AD FS en Windows Server 2012 y versiones anteriores de AD FS y obtenga más información sobre el tipo de notificaciones de identificador de nombre.|![configurar AD FS para enviar el](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[planeamiento de notificaciones para la interoperabilidad con AD FS 1. x](https://technet.microsoft.com/library/ff678040.aspx)|  
|![configurar AD FS para enviar notificaciones](media/icon_checkboxo.gif)|Antes de que pueda conseguir la interoperabilidad con una versión anterior de AD FS, primero debe crear una relación de confianza para usuario autenticado en el AD FS Servicio de federación en el AD FS 1. *x* servicio de Federación. **Nota:** No se puede crear una relación de confianza con un AD FS 1. *x* servicio de Federación mediante el uso de metadatos de Federación.<p>Cuando configure la confianza mediante el procedimiento en el vínculo de la derecha, debe hacer lo siguiente en el Asistente para agregar relación de confianza para usuario autenticado con el fin de configurar esta confianza para interoperar con un AD FS 1. *x* servicio de Federación:<p>1. en la página **Seleccionar origen de datos** , seleccione **escribir manualmente los datos sobre la relación de confianza para usuario autenticado**.<br />2. en la página **Elegir perfil** , seleccione **AD FS perfil 1,0 y 1,1**.<br />3. en la página **configurar dirección URL** , en **WS\-Federación pasiva URL**, escriba la **dirección URL del punto de conexión de servicio de Federación** como se define en el AD FS 1. *x* servicio de Federación del socio comercial.<br />4. en la página **configurar identificadores** , en **identificador de confianza de usuario de confianza**, escriba el URI de **servicio de Federación** tal y como se define en el AD FS 1. *x* servicio de Federación del socio comercial.|![configurar AD FS para enviar notificaciones](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[crear una relación de confianza para usuario autenticado manualmente](../../ad-fs/operations/Create-a-Relying-Party-Trust.md)|  
|![configurar AD FS para enviar notificaciones](media/icon_checkboxo.gif)|En la relación de confianza para usuario autenticado que creó anteriormente, debe crear reglas de notificaciones que tomarán las notificaciones entrantes que se extrajeron de un almacén de atributos y las pasarán, filtrarán o transformarán en un tipo de notificación de ID. de nombre que el AD FS 1 pueda comprender y usar. *x* servicio de Federación. **Nota:** Antes de crear esta regla, asegúrese de que el conjunto de reglas de notificaciones en el que va a crear esta regla tenga una regla antes de que extraiga primero un protocolo ligero de acceso a directorios \(la petición de atributo LDAP\) de un almacén de atributos. Esta demanda se usará como entrada para la regla que cree para enviar un AD FS 1. *x*\-Claim compatible. Para obtener más información sobre cómo crear una regla para extraer un atributo LDAP, vea [crear una regla para enviar atributos LDAP como notificaciones](../../ad-fs/operations/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims.md).|![configurar AD FS para enviar notificaciones](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[creación de una regla para enviar una notificación compatible con AD FS 1. x](../../ad-fs/operations/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md)|  
|![configurar AD FS para enviar notificaciones](media/icon_checkboxo.gif)|Póngase en contacto con el administrador del AD FS 1. *x* servicio de Federación y tener el administrador del AD FS 1. *x* servicio de Federación configurar una nueva confianza de asociado de cuenta. Además, proporcione a ese administrador el URI de Servicio de federación \(en el\)de propiedades de Servicio de federación, la dirección URL de extremo pasivo de Federación de WS\-\(la dirección URL del punto de conexión de Servicio de federación\)y un token exportado\-el archivo de certificado de firma \(solo con la clave pública\). Ese administrador necesitará estos elementos para configurar la confianza.|N\/A|  
  

