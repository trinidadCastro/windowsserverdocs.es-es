---
ms.assetid: 551c1a0d-8d30-41b4-9c4a-35a3337dd3bc
title: Implementación de servidores de federación
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 32675aa7fb8c7b928bcf80a4d1072fe5eab7cd61
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59864086"
---
# <a name="checklist-configuring-ad-fs-to-send-claims-to-an-ad-fs-1x-claims-aware-web-agent"></a>Lista de comprobación: Configuración de AD FS para enviar notificaciones a un agente Web para notificaciones de AD FS 1.x

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012
  
## <a name="checklist-configuring-ad-fs-to-send-claims-to-an-adfs1x-claims-aware-web-agent"></a>Lista de comprobación: Configuración de AD FS para enviar notificaciones a un notificaciones de AD FS 1.x\-agente Web compatible con  
Esta lista de comprobación incluye las tareas que son necesarias para configurar los servicios de federación de Active Directory \(AD FS\) servicio de federación de Windows Server 2012 para enviar notificaciones a los que se pueden entender por una aplicación que está hospedada en un Servidor Web que ejecuta AD FS 1. *x* notificaciones\-agente Web compatible con.  
  
> [!NOTE]  
> Complete las tareas de esta lista de comprobación en orden. Cuando un vínculo de referencia le dirija a un procedimiento, vuelva a este tema tras completar los pasos de este procedimiento, de tal forma que pueda continuar con las tareas restantes de la lista de comprobación.  
  
![configurar AD FS para enviar notificaciones](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**lista de comprobación: Configuración de AD FS para enviar notificaciones a un notificaciones de AD FS 1.x\-agente Web compatible con**  
  
||Tarea|Referencia|  
|-|--------|-------------|  
|![configurar AD FS para enviar notificaciones](media/icon_checkboxo.gif)|Planear la interoperabilidad entre AD FS en Windows Server 2012 y versiones anteriores de AD FS y aprenda que más sobre el identificador de nombre de tipo de notificación.|![configurar AD FS para enviar notificaciones](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[planeamiento de la interoperabilidad con AD FS 1.x](https://technet.microsoft.com/library/ff678040.aspx)|  
|![configurar AD FS para enviar notificaciones](media/icon_checkboxo.gif)|Si aún no lo ha hecho, utilice el vínculo a la derecha para crear una confianza entre el servicio de federación de AD FS en Windows Server 2012 y AD FS 1. *x* servicio de federación.|[Lista de comprobación: Configuración de AD FS para enviar notificaciones a un servicio de federación de AD FS 1.x](Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Federation-Service.md)|  
|![configurar AD FS para enviar notificaciones](media/icon_checkboxo.gif)|Antes de que se puede conseguir la interoperación con una aplicación que está hospedada por AD FS 1. *x* notificaciones\-agente Web compatible con, primero debe crear una entidad de confianza en el servicio de federación de AD FS en Windows Server 2012 a AD FS 1. *x* notificaciones\-agente Web compatible con. **Nota:** Creación de esta relación de confianza en el servicio de federación de AD FS es equivalente a agregar un nuevo **aplicación** para AD FS 1.x Federation Service \( **servicio de federación\\directiva de confianza\\ Mi organización\\aplicación**\). Esta relación de confianza es necesaria porque AD FS no tiene un equivalente **aplicación** nodo en su propio complemento\-en. Sin embargo, todavía debe tener un canal seguro para la aplicación.<br /><br />Al establecer la relación de confianza mediante el procedimiento en el vínculo a la derecha, debe hacer lo siguiente en el usuario de confianza entidad Asistente para agregar confianza para establecer esta relación de confianza para interoperar con AD FS 1. *x* notificaciones\-agente Web compatible con:<br /><br />1.  En el **Seleccionar origen de datos** , seleccione **introducir datos sobre el usuario de confianza de terceros de confianza manualmente**.<br />2.  En el **Elegir perfil** página, seleccione **perfil de AD FS 1.0 y 1.1**.<br />3.  En el **configurar dirección URL de** página, en **WS\-URL Federation Passive**, tipo de la **dirección URL de la aplicación** tal como se define en AD FS 1. *x* servicio de federación del asociado.<br />4.  En el **configurar identificadores** página, en **identificador de confianza para usuario autenticado partes**, tipo de la **dirección URL de la aplicación** tal como se define en AD FS 1. *x* notificaciones\-agente Web compatible con|![configurar AD FS para enviar notificaciones](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[crear un Relying Party Trust Manually](../../ad-fs/operations/Create-a-Relying-Party-Trust.md)|  
|![configurar AD FS para enviar notificaciones](media/icon_checkboxo.gif)|Póngase en contacto con el administrador del servidor Web de AD FS 1. *x* notificaciones\-consciente del agente de Web y hacer que ese administrador edite el archivo web.config que está asociado con las notificaciones\-aplicación compatible con \(en el sitio Web predeterminado en Internet Information Servicios \(IIS\) \) para señalar el agente Web de en el servicio de federación de AD FS.<br /><br />Por ejemplo, reemplace *myresourcefederationserver* en la etiqueta `<fs> https://myresourcefederationserver/adfs/fs/federationserverservice.asmx</fs>` del archivo web.config con un nombre de servidor de federación de AD FS válido.<br /><br />Esto es necesario para la aplicación y notificaciones de AD FS 1.x\-agente Web para que pueda consumir las notificaciones que se envían a él desde el servicio de federación de AD FS en Windows Server 2012.|N\/A|  
|![configurar AD FS para enviar notificaciones](media/icon_checkboxo.gif)|En la confianza que creó anteriormente, tiene que crear reglas de notificación que tomar las notificaciones entrantes que se han extraído de un almacén de atributos y pasan a través, filtrar o transformar en un tipo de notificación de Id. de nombre que se puede entender y consumirlos el AD FS 1. *x* notificaciones\-agente Web compatible con. **Nota:** Antes de crear esta regla, asegúrese de que el conjunto de reglas de notificación que va a crear esta regla tiene una regla que viene antes que se extrae en primer lugar un Lightweight Directory Access Protocol \(LDAP\) atributo notificación desde un almacén de atributos. Esta notificación se usará como entrada para la regla que ha creado para enviar AD FS 1. *x*\-notificación compatible con. Para obtener más información sobre cómo crear una regla para extraer un atributo LDAP, consulte [crear una regla para enviar atributos LDAP como notificaciones](../../ad-fs/operations/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims.md).|![configurar AD FS para enviar notificaciones](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[crear una regla para enviar AD FS 1.x notificación compatible con](../../ad-fs/operations/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md)|  
  

