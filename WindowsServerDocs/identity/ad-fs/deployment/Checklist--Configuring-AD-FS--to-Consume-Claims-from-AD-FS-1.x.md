---
ms.assetid: e7f9e518-2d5d-4a0d-9147-34e1304f42ac
title: 'Lista de comprobación: configuración de AD FS para consumir notificaciones de AD FS 1.x'
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 41a71ff49d211d294768c0e4a55692ced3f2d844
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192447"
---
# <a name="checklist-configuring-ad-fs--to-consume-claims-from-ad-fs-1x"></a>Lista de comprobación: Configuración de AD FS para consumir notificaciones de AD FS 1.x

  
## <a name="checklist-configuring-ad-fs-to-consume-claims-from-adfs1x"></a>Lista de comprobación: Configuración de AD FS para consumir notificaciones de AD FS 1.x  
Esta lista de comprobación incluye las tareas que son necesarias para configurar los servicios de federación de Active Directory \(AD FS\) servicio de federación de Windows Server 2012 consuma las notificaciones que se envían por AD FS 1. *x* servicio de federación.  
  
> [!NOTE]  
> Complete las tareas de esta lista de comprobación en orden. Cuando un vínculo de referencia le dirija a un procedimiento, vuelva a este tema tras completar los pasos de este procedimiento, de tal forma que pueda continuar con las tareas restantes de la lista de comprobación.  
  
![consumir notificaciones de AD FS](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**lista de comprobación: Configuración de AD FS para consumir notificaciones de AD FS 1.x**  
  
||Tarea|Referencia|  
|-|--------|-------------|  
|![consumir notificaciones de AD FS](media/icon_checkboxo.gif)|Planear la interoperabilidad entre AD FS en Windows Server 2012 y versiones anteriores de AD FS y aprenda que más sobre el identificador de nombre de tipo de notificación.|![consumir notificaciones de AD FS](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[planeamiento de la interoperabilidad con AD FS 1.x](https://technet.microsoft.com/library/ff678040.aspx)|  
|![consumir notificaciones de AD FS](media/icon_checkboxo.gif)|Antes de que puede interactuar con una versión anterior de AD FS, primero debe crear una confianza de proveedor de notificaciones en el servicio de federación de AD FS. **Nota:** No se puede crear una relación de confianza con AD FS 1. *x* servicio de federación mediante el uso de los metadatos de federación.<br /><br />Al establecer la relación de confianza mediante el procedimiento en el vínculo a la derecha, debe hacer lo siguiente en el proveedor de confianza Asistente para agregar notificaciones para establecer esta relación de confianza para interoperar con AD FS 1. *x* servicio de federación:<br /><br />1.  En el **Seleccionar origen de datos** , seleccione **introducir datos sobre el usuario de confianza de terceros de confianza manualmente**.<br />2.  En el **Elegir perfil** página, seleccione **perfil de AD FS 1.0 y 1.1**.<br />3.  En el **configurar dirección URL de** página, en **WS\-URL Federation Passive**, escriba el **dirección URL del extremo de servicio de federación** tal como se define en AD FS 1. *x* servicio de federación del asociado.<br />4.  En el **configurar identificadores** página, en **identificador de confianza de proveedor de notificaciones**, escriba el **URI del servicio de federación** tal como se define en AD FS 1. *x* servicio de federación del asociado.|![consumir notificaciones de AD FS](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[crear un Claims Provider Trust Manually](../../ad-fs/operations/Create-a-Claims-Provider-Trust.md)|  
|![consumir notificaciones de AD FS](media/icon_checkboxo.gif)|En la confianza de proveedor de notificaciones que creó anteriormente, debe crear una regla de notificación que tardará notificaciones entrantes de AD FS 1.x Federation Service y paso a través, filtrar o transformarlos en un tipo de notificación de Id. de nombre.<br /><br />El tipo de notificación de Id. de nombre se ha pasado a través, filtrar o transformar, puede usar como entrada para otra regla o las reglas para que se pueda entender y consumido por el servicio de federación de AD FS en Windows Server 2012.|![consumir notificaciones de AD FS](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[crear una regla para enviar AD FS 1.x notificación compatible con](../../ad-fs/operations/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md)|  
|![consumir notificaciones de AD FS](media/icon_checkboxo.gif)|Póngase en contacto con el Administrador de AD FS 1. *x* servicio de federación y que el Administrador de AD FS 1. *x* establecer una confianza de asociado de recurso nuevo servicio de federación. Asimismo, proporciona ese administrador con el URI del servicio de federación \(en las propiedades del servicio de federación\), la dirección URL de punto de conexión de servicio de federación y un token de exportado\-archivo de certificado de firma \(con clave pública solo\). El administrador necesitará estos elementos para establecer la relación de confianza.|N\/A|  
  

