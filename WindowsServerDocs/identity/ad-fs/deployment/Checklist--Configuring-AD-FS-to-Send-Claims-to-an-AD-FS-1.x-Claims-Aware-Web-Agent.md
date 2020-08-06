---
ms.assetid: 551c1a0d-8d30-41b4-9c4a-35a3337dd3bc
title: configurar AD FS para enviar notificaciones a un agente web para notificaciones de AD FS 1.x
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 61a2fb57e893615b0112592ce575a55ca3d6d71a
ms.sourcegitcommit: de8fea497201d8f3d995e733dfec1d13a16cb8fa
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87864149"
---
# <a name="checklist-configuring-ad-fs-to-send-claims-to-an-ad-fs-1x-claims-aware-web-agent"></a>Lista de comprobación: configurar AD FS para enviar notificaciones a un agente web para notificaciones de AD FS 1.x


## <a name="checklist-configuring-ad-fs-to-send-claims-to-an-adfs1x-claims-aware-web-agent"></a>Lista de comprobación: configuración de AD FS para enviar notificaciones a un \- agente Web compatible con notificaciones de AD FS 1. x
Esta lista de comprobación incluye las tareas necesarias para configurar el Servicios de federación de Active Directory (AD FS) \( AD FS \) servicio de Federación en Windows Server 2012 para enviar notificaciones que una aplicación hospedada en un servidor Web que ejecuta el AD FS 1 puede entender.* x* \- agente Web compatible con notificaciones.

> [!NOTE]
> Complete las tareas de esta lista de comprobación en orden. Cuando un vínculo de referencia le dirija a un procedimiento, vuelva a este tema tras completar los pasos de este procedimiento, de tal forma que pueda continuar con las tareas restantes de la lista de comprobación.

![configurar AD FS para enviar una lista de comprobación de notificaciones](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**: configuración de AD FS para enviar notificaciones a un \- agente Web compatible con notificaciones de AD FS 1. x**

|Tarea|Referencia|
|--------|-------------|
|Planee la interoperabilidad entre AD FS en Windows Server 2012 y versiones anteriores de AD FS y obtenga más información sobre el tipo de notificaciones de identificador de nombre.|![Configure AD FS para enviar el](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[planeamiento de notificaciones de interoperabilidad con AD FS 1. x](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/ff678040(v=ws.11))|
|Si todavía no lo ha hecho, use el vínculo de la derecha para crear primero una relación de confianza para usuario autenticado entre el AD FS Servicio de federación en Windows Server 2012 y el AD FS 1. *x* servicio de Federación.|[Lista de comprobación: configurar AD FS para enviar notificaciones a un servicio de federación de AD FS 1.x](Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Federation-Service.md)|
|Antes de poder lograr la interoperación con una aplicación hospedada en el AD FS 1. *x* \- agente Web compatible con notificaciones, primero debe crear una relación de confianza para usuario autenticado en el servicio de Federación de AD FS de Windows Server 2012 al AD FS 1. *x* \- agente Web compatible con notificaciones. **Nota:** La creación de esta confianza en el Servicio de federación AD FS es equivalente a agregar una nueva **aplicación** a la AD FS 1. x servicio de Federación \( **servicio de Federación \\ Directiva de confianza \\ mi \\ aplicación**de la organización \) . Esta relación de confianza para usuario autenticado es necesaria porque AD FS no tiene un nodo de **aplicación** equivalente en su propio complemento \- . Sin embargo, todavía debe tener un canal seguro a la aplicación.<p>Cuando configure la confianza mediante el procedimiento en el vínculo de la derecha, debe hacer lo siguiente en el Asistente para agregar relación de confianza para usuario autenticado con el fin de configurar esta confianza para interoperar con un AD FS 1. *x* \- agente Web compatible con notificaciones:<p>1. en la página **Seleccionar origen de datos** , seleccione **escribir manualmente los datos sobre la relación de confianza para usuario autenticado**.<br />2. en la página **Elegir perfil** , seleccione **AD FS perfil 1,0 y 1,1**.<br />3. en la página **configurar dirección URL** , **en \- dirección URL de WS Federation Passive**, escriba la **dirección URL** de la aplicación tal como se define en el AD FS 1.* x* servicio de Federación del socio comercial.<br />4. en la página **configurar identificadores** , en **identificador de confianza de usuario de confianza**, escriba la **dirección URL** de la aplicación tal como se define en el AD FS 1. agente Web compatible con notificaciones *x* \-|![configurar AD FS para enviar notificaciones](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[crear una relación de confianza para usuario autenticado manualmente](../../ad-fs/operations/Create-a-Relying-Party-Trust.md)|
|Póngase en contacto con el administrador del servidor Web que ejecuta el AD FS 1. *x* \- agente Web compatible con notificaciones y haga que el administrador edite el archivo web.config asociado a la \- aplicación para notificaciones \( en el sitio web predeterminado en Internet Information Services \( IIS \) \) para que el agente Web apunte al AD FS servicio de Federación.<p>Por ejemplo, reemplace *myresourcefederationserver* en la etiqueta `<fs>https://myresourcefederationserver/adfs/fs/federationserverservice.asmx</fs>` del archivo de web.config por un nombre de servidor de Federación AD FS válido.<p>Esto es necesario para que la aplicación y el agente Web compatible con notificaciones de AD FS 1. x pueda \- consumir las notificaciones que se le envían desde el servicio de Federación de AD FS en Windows Server 2012.|N\/D|
|En la relación de confianza para usuario autenticado que creó anteriormente, tiene que crear reglas de notificaciones que tomarán las notificaciones entrantes que se extrajeron de un almacén de atributos y las pasarán, filtrarán o transformarán en un tipo de notificación de ID. de nombre que el AD FS 1 pueda comprender y usar. *x* \- agente Web compatible con notificaciones. **Nota:** Antes de crear esta regla, asegúrese de que el conjunto de reglas de notificaciones en el que va a crear esta regla tenga una regla antes de que extraiga primero una \( petición de atributo LDAP de Protocolo ligero de acceso a directorios \) de un almacén de atributos. Esta demanda se usará como entrada para la regla que cree para enviar un AD FS 1. *x* \- notificaciones compatibles. Para obtener más información sobre cómo crear una regla para extraer un atributo LDAP, vea [crear una regla para enviar atributos LDAP como notificaciones](../../ad-fs/operations/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims.md).|![configurar AD FS para enviar notificaciones](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[creación de una regla para enviar una notificación compatible con AD FS 1. x](../../ad-fs/operations/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md)|

