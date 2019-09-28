---
ms.assetid: 4b81ac66-3f34-4a39-a8bf-5411131a69c2
title: 'Lista de comprobación: configuración de AD FS para consumir notificaciones de AD FS 1. x'
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: f91944333da9ce4c1d78bbbf7b3652f1118e1f08
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408498"
---
# <a name="checklist-configuring-ad-fs-to-send-claims-to-an-ad-fs-1x-federation-service"></a>Lista de comprobación: Configuración de AD FS para enviar notificaciones a una Servicio de federación de AD FS 1. x

  
## <a name="checklist-configuring-ad-fs-to-send-claims-to-an-adfs1x-federation-service"></a>Lista de comprobación: Configuración de AD FS para enviar notificaciones a una Servicio de federación de AD FS 1. x  
Esta lista de comprobación incluye las tareas necesarias para configurar el Servicios de federación de Active Directory (AD FS) \(AD FS @ no__t-1 Servicio de federación en Windows Server 2012 para enviar notificaciones que un AD FS 1 puede entender. *x* servicio de Federación.  
  
> [!NOTE]  
> Complete las tareas de esta lista de comprobación en orden. Cuando un vínculo de referencia le dirija a un procedimiento, vuelva a este tema tras completar los pasos de este procedimiento, de tal forma que pueda continuar con las tareas restantes de la lista de comprobación.  
  
![configure AD FS para enviar notificaciones @ no__t-1Checklist: Configuración de AD FS para enviar notificaciones a un AD FS 1. x Servicio de federación @ no__t-0  
  
||Tarea|Referencia|  
|-|--------|-------------|  
|![configurar AD FS para enviar notificaciones](media/icon_checkboxo.gif)|Planee la interoperabilidad entre AD FS en Windows Server 2012 y versiones anteriores de AD FS y obtenga más información sobre el tipo de notificaciones de identificador de nombre.|![configure AD FS para enviar el](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[planeamiento de notificaciones para la interoperabilidad con AD FS 1. x](https://technet.microsoft.com/library/ff678040.aspx)|  
|![configurar AD FS para enviar notificaciones](media/icon_checkboxo.gif)|Antes de que pueda conseguir la interoperabilidad con una versión anterior de AD FS, primero debe crear una relación de confianza para usuario autenticado en el AD FS Servicio de federación en el AD FS 1. *x* servicio de Federación. **Nota:** No se puede crear una relación de confianza con un AD FS 1. *x* servicio de Federación mediante el uso de metadatos de Federación.<br /><br />Cuando configure la confianza mediante el procedimiento en el vínculo de la derecha, debe hacer lo siguiente en el Asistente para agregar relación de confianza para usuario autenticado con el fin de configurar esta confianza para interoperar con un AD FS 1. *x* servicio de Federación:<br /><br />1.  En la página **Seleccionar origen de datos** , seleccione **escribir manualmente los datos sobre la relación de confianza para usuario autenticado**.<br />2.  En la página **Elegir perfil** , seleccione **AD FS perfil 1,0 y 1,1**.<br />3.  En la página **configurar dirección URL** , en **WS @ No__t-2Federation Passive URL**, escriba la **dirección url del punto de conexión servicio de Federación** tal y como se define en el AD FS 1. *x* servicio de Federación del socio comercial.<br />4.  En la página **configurar identificadores** , en **identificador de confianza de usuario de confianza**, escriba el URI de **servicio de Federación** tal y como se define en el AD FS 1. *x* servicio de Federación del socio comercial.|![configure AD FS para enviar notificaciones](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[crear una relación de confianza para usuario autenticado manualmente](../../ad-fs/operations/Create-a-Relying-Party-Trust.md)|  
|![configurar AD FS para enviar notificaciones](media/icon_checkboxo.gif)|En la relación de confianza para usuario autenticado que creó anteriormente, debe crear reglas de notificaciones que tomarán las notificaciones entrantes que se extrajeron de un almacén de atributos y las pasarán, filtrarán o transformarán en un tipo de notificación de ID. de nombre que el AD pueda comprender y usar. FS 1. *x* servicio de Federación. **Nota:** Antes de crear esta regla, asegúrese de que el conjunto de reglas de notificaciones en el que va a crear esta regla tenga una regla antes de que extraiga primero una petición de atributo de Protocolo ligero de acceso a directorios \(LDAP @ no__t-1 de un almacén de atributos. Esta demanda se usará como entrada para la regla que cree para enviar un AD FS 1. 1Compatible *x*@no__t. Para obtener más información sobre cómo crear una regla para extraer un atributo LDAP, vea [crear una regla para enviar atributos LDAP como notificaciones](../../ad-fs/operations/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims.md).|![configure AD FS para enviar notificaciones](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[creación de una regla para enviar una notificación compatible con AD FS 1. x](../../ad-fs/operations/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md)|  
|![configurar AD FS para enviar notificaciones](media/icon_checkboxo.gif)|Póngase en contacto con el administrador del AD FS 1. *x* servicio de Federación y tener el administrador del AD FS 1. *x* servicio de Federación configurar una nueva confianza de asociado de cuenta. Además, proporcione a ese administrador el URI Servicio de federación \(in las propiedades de Servicio de federación @ no__t-1, la dirección URL de extremo pasivo de WS @ no__t-2Federation \(The Servicio de federación dirección URL del punto de conexión @ no__t-4 y un token exportado @ No__ el archivo de certificado de t-5signing \(With solo clave pública @ no__t-7. Ese administrador necesitará estos elementos para configurar la confianza.|N\/A|  
  

