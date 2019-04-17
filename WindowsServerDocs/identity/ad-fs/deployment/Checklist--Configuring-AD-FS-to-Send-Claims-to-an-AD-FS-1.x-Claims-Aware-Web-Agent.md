---
ms.assetid: 551c1a0d-8d30-41b4-9c4a-35a3337dd3bc
title: "Implementación de los servidores de federación"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 32675aa7fb8c7b928bcf80a4d1072fe5eab7cd61
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="checklist-configuring-ad-fs-to-send-claims-to-an-ad-fs-1x-claims-aware-web-agent"></a>Lista de comprobación: Configurar AD FS para enviar notificaciones a un agente de Web para notificaciones de AD FS 1.x

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012
  
## <a name="checklist-configuring-ad-fs-to-send-claims-to-an-ad-fs-1x-claims-aware-web-agent"></a>Lista de comprobación: Configurar AD FS para enviar notificaciones a un agente de Web claims\ cuenta de AD FS 1.x  
Esta lista de comprobación incluye las tareas que son necesarias para configurar los servicios de federación de los servicios de federación de Active Directory \(AD FS\) en Windows Server 2012 para enviar notificaciones que te ayudarán a comprender una aplicación que está hospedada en un servidor Web que se ejecuta el 1 de AD FS. *x* claims\ cuenta agente de Web.  
  
> [!NOTE]  
> Completar las tareas en esta lista de comprobación en orden. Cuando un vínculo de referencia tiene un procedimiento, vuelve a este tema después de completar los pasos de este procedimiento para que se puede continuar con las tareas restantes en esta lista de comprobación.  
  
![configurar AD FS para enviar notificaciones](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**lista de comprobación: configurar AD FS para enviar notificaciones a un agente de Web claims\ cuenta de AD FS 1.x**  
  
||Tarea|Referencia|  
|-|--------|-------------|  
|![configurar AD FS para enviar notificaciones](media/icon_checkboxo.gif)|Planear para la interoperabilidad entre AD FS en Windows Server 2012 y versiones anteriores de AD FS y obtener que más información sobre el identificador de nombre de tipo de notificación.|![configurar AD FS para enviar notificaciones](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[planeación para la interoperabilidad con AD FS 1.x](https://technet.microsoft.com/library/ff678040.aspx)|  
|![configurar AD FS para enviar notificaciones](media/icon_checkboxo.gif)|Si aún no lo has hecho, usar el vínculo a la derecha para crear una confianza de terceros confianza entre el servicio de federación de AD FS en Windows Server 2012 y el 1 de AD FS. *x* servicios de federación.|[Lista de comprobación: Configurar AD FS para enviar notificaciones a un servicio de federación de AD FS 1.x](Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Federation-Service.md)|  
|![configurar AD FS para enviar notificaciones](media/icon_checkboxo.gif)|Puedes lograr antes interoperación con una aplicación que hospeda el 1 de AD FS. *x* agente de Web con reconocimiento de claims\, primero debes crear una confianza de terceros confiar en el servicio de federación de AD FS en Windows Server 2012 para el 1 de AD FS. *x* claims\ cuenta agente de Web. **Nota:** crear esta confianza en el servicio de federación de AD FS es el equivalente de agregar un nuevo **aplicación** a la de AD FS 1.x servicios de federación de \ (**federación Service\\Trust Policy\\My Organization\\Application**\). Esta confianza de terceros de confianza es necesaria porque AD FS no tiene un equivalente **aplicación** nodo en su propio snap\. Sin embargo, todavía debe tener un canal seguro a la aplicación.<br /><br />Cuando configuras la confianza con el procedimiento en el vínculo a la derecha, debes hacer lo siguiente en el Asistente para agregar confiar terceros confianza para configurar esta confianza para interoperar con un 1 de AD FS. *x* claims\ cuenta agente de Web:<br /><br />1. en la **Seleccionar origen de datos** , seleccione **introducir datos acerca de los usuarios de confianza de terceros confianza manualmente**.<br />2. en la **Elegir perfil** página, seleccione **perfil de AD FS 1.0 y 1.1**.<br />3. en la **configurar URL** página, en **WS\ federación pasivo URL**, tipo el **dirección URL de la aplicación** según se define en el 1 de AD FS. *x* servicios de federación del asociado.<br />4. en la **configurar identificadores** página, debajo **identificador de confianza parte Relying**, tipo el **dirección URL de la aplicación** según se define en el 1 de AD FS. *x* claims\ cuenta agente de Web|![configurar AD FS para enviar notificaciones](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[crear un confiar terceros de confianza de forma manual](../../ad-fs/operations/Create-a-Relying-Party-Trust.md)|  
|![configurar AD FS para enviar notificaciones](media/icon_checkboxo.gif)|Ponte en contacto con el administrador del servidor Web ejecuta el 1 de AD FS. *x* claims\ cuenta Web agente y hacer que ese administrador editar el archivo de web.config que está asociado con la aplicación con reconocimiento de claims\ \ (en el sitio Web predeterminado en Internet Information Services \(IIS\)\) para señalar el agente de Web en el servicio de federación de AD FS.<br /><br />Por ejemplo, reemplaza *myresourcefederationserver* en la etiqueta `<fs>https://myresourcefederationserver/adfs/fs/federationserverservice.asmx</fs>` del archivo web.config con un nombre de servidor de federación de AD FS válido.<br /><br />Esto es necesario para la aplicación y el agente de Web de AD FS 1.x claims\ para poder consumir las notificaciones que se envían a ella desde el servicio de federación de AD FS en Windows Server 2012.|N\/A|  
|![configurar AD FS para enviar notificaciones](media/icon_checkboxo.gif)|En la confianza de terceros confianza que creaste anteriormente, tienes que crear el tipo que se puede entender y consumida por el anuncio de la notificación de reglas que tomar notificaciones entrantes extraídas de un almacén de atributo y pasar, filtrar o transformarlos en un identificador de nombre de Reclamación FS 1. *x* claims\ cuenta agente de Web. **Nota:** antes de crear esta regla, asegúrate de que el conjunto de reglas de notificación de que estás creando esta regla tiene una regla que viene antes de que primero se extrae una reclamación de atributo de protocolo ligero de acceso a directorios \(LDAP\) de un almacén de atributo. Esta notificación se usará como entrada a la regla que creas para enviar un 1 de AD FS. *x*\-compatible reclamación. Para obtener más información sobre cómo crear una regla para extraer un atributo LDAP, consulta [crear una regla para enviar atributos LDAP como notificaciones](../../ad-fs/operations/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims.md).|![configurar AD FS para enviar notificaciones](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[crear una regla para enviar un AD FS 1.x reclamación Compatible](../../ad-fs/operations/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md)|  
  

