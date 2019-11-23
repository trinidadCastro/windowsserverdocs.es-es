---
ms.assetid: 551c1a0d-8d30-41b4-9c4a-35a3337dd3bc
title: Implementación de servidores de federación
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 689bd33bc95c2b142dfbe6d0448a604b2971979e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359982"
---
# <a name="checklist-configuring-ad-fs-to-send-claims-to-an-ad-fs-1x-claims-aware-web-agent"></a>Lista de comprobación: configurar AD FS para enviar notificaciones a un agente web para notificaciones de AD FS 1.x

  
## <a name="checklist-configuring-ad-fs-to-send-claims-to-an-adfs1x-claims-aware-web-agent"></a>Lista de comprobación: configuración de AD FS para enviar notificaciones a un agente Web compatible con\-notificaciones de AD FS 1. x  
Esta lista de comprobación incluye las tareas necesarias para configurar el Servicios de federación de Active Directory (AD FS) \(AD FS\) Servicio de federación en Windows Server 2012 para enviar notificaciones que puede entender una aplicación hospedada por un servidor Web que ejecuta AD FS 1. el agente Web compatible con\-de notificaciones *x* .  
  
> [!NOTE]  
> Complete las tareas de esta lista de comprobación en orden. Cuando un vínculo de referencia le dirija a un procedimiento, vuelva a este tema tras completar los pasos de este procedimiento, de tal forma que pueda continuar con las tareas restantes de la lista de comprobación.  
  
![configurar AD FS para enviar una lista de comprobación de notificaciones](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**: configuración de AD FS para enviar notificaciones a un agente Web compatible con\-de notificaciones AD FS 1. x**  
  
||Tarea|Referencia|  
|-|--------|-------------|  
|![configurar AD FS para enviar notificaciones](media/icon_checkboxo.gif)|Planee la interoperabilidad entre AD FS en Windows Server 2012 y versiones anteriores de AD FS y obtenga más información sobre el tipo de notificaciones de identificador de nombre.|![configurar AD FS para enviar el](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[planeamiento de notificaciones para la interoperabilidad con AD FS 1. x](https://technet.microsoft.com/library/ff678040.aspx)|  
|![configurar AD FS para enviar notificaciones](media/icon_checkboxo.gif)|Si todavía no lo ha hecho, use el vínculo de la derecha para crear primero una relación de confianza para usuario autenticado entre el AD FS Servicio de federación en Windows Server 2012 y el AD FS 1. *x* servicio de Federación.|[Lista de comprobación: configuración de AD FS para enviar notificaciones a un AD FS 1. x Servicio de federación](Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Federation-Service.md)|  
|![configurar AD FS para enviar notificaciones](media/icon_checkboxo.gif)|Antes de poder lograr la interoperación con una aplicación hospedada en el AD FS 1. *x* claims\-agente Web compatible, primero debe crear una relación de confianza para usuario autenticado en el AD FS servicio de Federación de Windows Server 2012 al AD FS 1. el agente Web compatible con\-de notificaciones *x* . **Nota:** La creación de esta confianza en el Servicio de federación de AD FS es equivalente a agregar una nueva **aplicación** a la AD FS 1. x servicio de Federación \(servicio de Federación\\de la **Directiva de confianza\\mi organización\\aplicación**\). Esta relación de confianza para usuario autenticado es necesaria porque AD FS no tiene un nodo de **aplicación** equivalente en su propio ajuste\-en. Sin embargo, todavía debe tener un canal seguro a la aplicación.<br /><br />Cuando configure la confianza mediante el procedimiento en el vínculo de la derecha, debe hacer lo siguiente en el Asistente para agregar relación de confianza para usuario autenticado con el fin de configurar esta confianza para interoperar con un AD FS 1. agente Web compatible con\-de notificaciones *x* :<br /><br />1. en la página **Seleccionar origen de datos** , seleccione **escribir manualmente los datos sobre la relación de confianza para usuario autenticado**.<br />2. en la página **Elegir perfil** , seleccione **AD FS perfil 1,0 y 1,1**.<br />3. en la página **configurar dirección URL** , en **WS\-Federación pasiva URL**, escriba la **dirección URL** de la aplicación tal y como se define en el AD FS 1. *x* servicio de Federación del socio comercial.<br />4. en la página **configurar identificadores** , en **identificador de confianza de usuario de confianza**, escriba la **dirección URL** de la aplicación tal como se define en el AD FS 1. agente Web compatible con\-de notificaciones *x*|![configurar AD FS para enviar notificaciones](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[crear una relación de confianza para usuario autenticado manualmente](../../ad-fs/operations/Create-a-Relying-Party-Trust.md)|  
|![configurar AD FS para enviar notificaciones](media/icon_checkboxo.gif)|Póngase en contacto con el administrador del servidor Web que ejecuta el AD FS 1. el agente Web compatible con\-de notificaciones *x* y que el administrador edite el archivo Web. config que está asociado a la aplicación\-compatible con notificaciones \(en el sitio web predeterminado en Internet Information Services \(\)IIS \) para que el agente Web apunte al AD FS servicio de Federación.<br /><br />Por ejemplo, reemplace *myresourcefederationserver* en la etiqueta `<fs> https://myresourcefederationserver/adfs/fs/federationserverservice.asmx</fs>` del archivo Web. config por un nombre de servidor de Federación AD FS válido.<br /><br />Esto es necesario para que las notificaciones de aplicación y AD FS 1. x\-agente Web compatible puedan consumir las notificaciones que se le envían desde el AD FS Servicio de federación en Windows Server 2012.|N\/A|  
|![configurar AD FS para enviar notificaciones](media/icon_checkboxo.gif)|En la relación de confianza para usuario autenticado que creó anteriormente, tiene que crear reglas de notificaciones que tomarán las notificaciones entrantes que se extrajeron de un almacén de atributos y las pasarán, filtrarán o transformarán en un tipo de notificación de ID. de nombre que el AD FS 1 pueda comprender y usar. el agente Web compatible con\-de notificaciones *x* . **Nota:** Antes de crear esta regla, asegúrese de que el conjunto de reglas de notificaciones en el que va a crear esta regla tenga una regla antes de que extraiga primero un protocolo ligero de acceso a directorios \(la petición de atributo LDAP\) de un almacén de atributos. Esta demanda se usará como entrada para la regla que cree para enviar un AD FS 1. *x*\-Claim compatible. Para obtener más información sobre cómo crear una regla para extraer un atributo LDAP, vea [crear una regla para enviar atributos LDAP como notificaciones](../../ad-fs/operations/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims.md).|![configurar AD FS para enviar notificaciones](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[creación de una regla para enviar una notificación compatible con AD FS 1. x](../../ad-fs/operations/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md)|  
  

