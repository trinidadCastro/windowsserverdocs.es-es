---
ms.assetid: e7f9e518-2d5d-4a0d-9147-34e1304f42ac
title: 'Lista de comprobación: configuración de AD FS para consumir notificaciones de AD FS 1. x'
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 1c952abc0bca5eadbfc14f3eda54d05826d50294
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408486"
---
# <a name="checklist-configuring-ad-fs--to-consume-claims-from-ad-fs-1x"></a>Lista de comprobación: configuración de AD FS para consumir notificaciones de AD FS 1. x

  
## <a name="checklist-configuring-ad-fs-to-consume-claims-from-adfs1x"></a>Lista de comprobación: configuración de AD FS para consumir notificaciones de AD FS 1. x  
Esta lista de comprobación incluye las tareas necesarias para configurar el Servicios de federación de Active Directory (AD FS) \(AD FS\) Servicio de federación en Windows Server 2012 para consumir notificaciones enviadas por un AD FS 1. *x* servicio de Federación.  
  
> [!NOTE]  
> Complete las tareas de esta lista de comprobación en orden. Cuando un vínculo de referencia le dirija a un procedimiento, vuelva a este tema tras completar los pasos de este procedimiento, de tal forma que pueda continuar con las tareas restantes de la lista de comprobación.  
  
![consumir notificaciones de AD FS](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**lista de comprobación: configuración de AD FS para consumir notificaciones de AD FS 1. x**  
  
||Tarea|Referencia|  
|-|--------|-------------|  
|![usar notificaciones de AD FS](media/icon_checkboxo.gif)|Planee la interoperabilidad entre AD FS en Windows Server 2012 y versiones anteriores de AD FS y obtenga más información sobre el tipo de notificaciones de identificador de nombre.|![consumir notificaciones de AD FS](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[planeación de la interoperabilidad con AD FS 1. x](https://technet.microsoft.com/library/ff678040.aspx)|  
|![usar notificaciones de AD FS](media/icon_checkboxo.gif)|Antes de poder interoperar con una versión anterior de AD FS, primero debe crear una confianza de proveedor de notificaciones en el Servicio de federación de AD FS. **Nota:** No se puede crear una relación de confianza con un AD FS 1. *x* servicio de Federación mediante el uso de metadatos de Federación.<br /><br />Cuando configure la confianza mediante el procedimiento en el vínculo de la derecha, debe hacer lo siguiente en el Asistente para agregar confianza de proveedor de notificaciones para configurar esta confianza para interoperar con un AD FS 1. *x* servicio de Federación:<br /><br />1. en la página **Seleccionar origen de datos** , seleccione **escribir manualmente los datos sobre la relación de confianza para usuario autenticado**.<br />2. en la página **Elegir perfil** , seleccione **AD FS perfil 1,0 y 1,1**.<br />3. en la página **configurar dirección URL** , en **WS\-Federación pasiva URL**, escriba la **dirección URL del punto de conexión de servicio de Federación** como se define en el AD FS 1. *x* servicio de Federación del socio comercial.<br />4. en la página **configurar identificadores** , en **identificador de confianza de proveedor de notificaciones**, escriba el **URI de servicio de Federación** tal y como se define en el AD FS 1. *x* servicio de Federación del socio comercial.|![usar notificaciones de AD FS](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[crear una confianza de proveedor de notificaciones manualmente](../../ad-fs/operations/Create-a-Claims-Provider-Trust.md)|  
|![usar notificaciones de AD FS](media/icon_checkboxo.gif)|En la relación de confianza para proveedor de notificaciones que ha creado anteriormente, debe crear una regla de notificación que tomará las notificaciones entrantes del AD FS 1. x Servicio de federación y pasarlas, filtrarlas o transformarlas en un tipo de notificación de ID. de nombre.<br /><br />Cuando el tipo de notificaciones de ID. de nombre se ha pasado, filtrado o transformado, se puede usar como entrada para otra regla o reglas, de modo que la AD FS Servicio de federación en Windows Server 2012 pueda entenderla y utilizarla.|![usar notificaciones de AD FS](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[crear una regla para enviar una notificación compatible con AD FS 1. x](../../ad-fs/operations/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md)|  
|![usar notificaciones de AD FS](media/icon_checkboxo.gif)|Póngase en contacto con el administrador del AD FS 1. *x* servicio de Federación y tener el administrador del AD FS 1. *x* servicio de Federación configurar una nueva confianza de asociado de recurso. Además, proporcione a ese administrador el Servicio de federación URI \(en el\)de propiedades Servicio de federación, la dirección URL del punto de conexión Servicio de federación y un token exportado\-el archivo de certificado de firma \(solo con la clave pública\). El administrador necesitará estos elementos para configurar la confianza.|N\/A|  
  

