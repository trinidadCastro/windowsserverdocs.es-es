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
# <a name="checklist-configuring-ad-fs-to-send-claims-to-an-ad-fs-1x-claims-aware-web-agent"></a>Lista de comprobación: Configuración de AD FS para enviar notificaciones a un agente Web compatible con notificaciones de AD FS 1. x

  
## <a name="checklist-configuring-ad-fs-to-send-claims-to-an-adfs1x-claims-aware-web-agent"></a>Lista de comprobación: Configuración de AD FS para enviar notificaciones a un AD FS 1. x Claims @ no__t-0aware web Agent  
Esta lista de comprobación incluye las tareas necesarias para configurar el Servicios de federación de Active Directory (AD FS) \(AD FS @ no__t-1 Servicio de federación en Windows Server 2012 para enviar notificaciones que una aplicación hospedada en un servidor Web hospeda. ejecutar el AD FS 1. *x* Claims @ no__t-3aware web Agent.  
  
> [!NOTE]  
> Complete las tareas de esta lista de comprobación en orden. Cuando un vínculo de referencia le dirija a un procedimiento, vuelva a este tema tras completar los pasos de este procedimiento, de tal forma que pueda continuar con las tareas restantes de la lista de comprobación.  
  
![configure AD FS para enviar notificaciones @ no__t-1Checklist: Configuración de AD FS para enviar notificaciones a un AD FS 1. x notificaciones @ no__t-0aware web Agent @ no__t-1  
  
||Tarea|Referencia|  
|-|--------|-------------|  
|![configurar AD FS para enviar notificaciones](media/icon_checkboxo.gif)|Planee la interoperabilidad entre AD FS en Windows Server 2012 y versiones anteriores de AD FS y obtenga más información sobre el tipo de notificaciones de identificador de nombre.|![configure AD FS para enviar el](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[planeamiento de notificaciones para la interoperabilidad con AD FS 1. x](https://technet.microsoft.com/library/ff678040.aspx)|  
|![configurar AD FS para enviar notificaciones](media/icon_checkboxo.gif)|Si todavía no lo ha hecho, use el vínculo de la derecha para crear primero una relación de confianza para usuario autenticado entre el AD FS Servicio de federación en Windows Server 2012 y el AD FS 1. *x* servicio de Federación.|[Lista de comprobación: configurar AD FS para enviar notificaciones a un servicio de federación de AD FS 1.x](Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Federation-Service.md)|  
|![configurar AD FS para enviar notificaciones](media/icon_checkboxo.gif)|Antes de poder lograr la interoperación con una aplicación hospedada en el AD FS 1. *x* Claims @ no__t-1aware web Agent, primero debe crear una relación de confianza para usuario autenticado en el AD FS servicio de Federación de Windows Server 2012 al AD FS 1. *x* Claims @ no__t-1aware web Agent. **Nota:** La creación de esta confianza en el Servicio de federación AD FS es equivalente a agregar una nueva **aplicación** a la AD FS 1. x servicio de Federación \(**servicio de Federación @ No__t-3Trust Policy @ No__t-4MY Organization @ no__t-5Application**\). Esta relación de confianza para usuario autenticado es necesaria porque AD FS no tiene un nodo de **aplicación** equivalente en su propio ajuste @ no__t-1in. Sin embargo, todavía debe tener un canal seguro a la aplicación.<br /><br />Cuando configure la confianza mediante el procedimiento en el vínculo de la derecha, debe hacer lo siguiente en el Asistente para agregar relación de confianza para usuario autenticado con el fin de configurar esta confianza para interoperar con un AD FS 1. *x* Claims @ no__t-1aware web Agent:<br /><br />1.  En la página **Seleccionar origen de datos** , seleccione **escribir manualmente los datos sobre la relación de confianza para usuario autenticado**.<br />2.  En la página **Elegir perfil** , seleccione **AD FS perfil 1,0 y 1,1**.<br />3.  En la página **configurar dirección URL** , en **WS @ No__t-2Federation Passive URL**, escriba la **dirección URL** de la aplicación tal como se define en el AD FS 1. *x* servicio de Federación del socio comercial.<br />4.  En la página **configurar identificadores** , en **identificador de confianza de usuario de confianza**, escriba la **dirección URL** de la aplicación tal como se define en el AD FS 1. *x* Claims @ no__t-4aware web Agent|![configure AD FS para enviar notificaciones](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[crear una relación de confianza para usuario autenticado manualmente](../../ad-fs/operations/Create-a-Relying-Party-Trust.md)|  
|![configurar AD FS para enviar notificaciones](media/icon_checkboxo.gif)|Póngase en contacto con el administrador del servidor Web que ejecuta el AD FS 1. *x* Claims @ no__t-1aware web Agent y haga que el administrador edite el archivo Web. config que está asociado a la aplicación Claims @ no__t-2aware \(Under el sitio web predeterminado en Internet Information Services \(IIS @ no__t-5 @ no__t-6 a señale el agente Web en el Servicio de federación de AD FS.<br /><br />Por ejemplo, reemplace *myresourcefederationserver* en la etiqueta `<fs> https://myresourcefederationserver/adfs/fs/federationserverservice.asmx</fs>` del archivo Web. config por un nombre de servidor de Federación AD FS válido.<br /><br />Esto es necesario para la aplicación y AD FS 1. x Claims @ no__t-0aware web Agent puede consumir las notificaciones que se le envían desde el Servicio de federación de AD FS en Windows Server 2012.|N\/A|  
|![configurar AD FS para enviar notificaciones](media/icon_checkboxo.gif)|En la relación de confianza para usuario autenticado que creó anteriormente, tiene que crear reglas de notificaciones que tomarán las notificaciones entrantes que se extrajeron de un almacén de atributos y las pasarán, filtrarán o transformarán en un tipo de notificación de ID. de nombre que el AD FS 1. *x* Claims @ no__t-1aware web Agent. **Nota:** Antes de crear esta regla, asegúrese de que el conjunto de reglas de notificaciones en el que va a crear esta regla tenga una regla antes de que extraiga primero una petición de atributo de Protocolo ligero de acceso a directorios \(LDAP @ no__t-1 de un almacén de atributos. Esta demanda se usará como entrada para la regla que cree para enviar un AD FS 1. 1Compatible *x*@no__t. Para obtener más información sobre cómo crear una regla para extraer un atributo LDAP, vea [crear una regla para enviar atributos LDAP como notificaciones](../../ad-fs/operations/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims.md).|![configure AD FS para enviar notificaciones](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[creación de una regla para enviar una notificación compatible con AD FS 1. x](../../ad-fs/operations/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md)|  
  

