---
ms.assetid: 826974ea-3635-40df-aa37-77dd12a363c8
title: "Lista de comprobación: configuración de la organización de Partner de cuenta"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: df8057bf8afb51cbd9ca2ec704144b5863bdf064
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="checklist-configuring-the-account-partner-organization"></a>Lista de comprobación: Configuración de la organización de Partner de cuenta

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La organización de partner cuenta contiene los usuarios que tendrán acceso a aplicaciones basadas en Web\ en el partner de recursos. Los administradores de esta organización deben utilizar la administración de AD FS snap\ en crear confianza confianzas de terceros para representar sus relaciones de confianza con las organizaciones asociadas de recursos. A su vez, el Administrador de partner de recursos debe crear confianzas de proveedor de notificaciones para cada organización de partner de la cuenta que quieran confiar.  
  
Esta lista de comprobación incluye tareas para la implementación de los servicios de federación de Active Directory \(AD FS\) en la organización de partner de la cuenta. También incluye tareas para configurar los componentes que son necesarios para establecer la mitad one\ una asociación de federación.  
  
Si estás implementando un [Web SSO diseño](https://technet.microsoft.com/library/dd807033.aspx), no tienes que seguir esta lista de comprobación. Sin embargo, es necesario que completar las tareas en esta lista de comprobación para implementar correctamente una [federados Web SSO diseño](https://technet.microsoft.com/library/dd807050.aspx).  
  
> [!IMPORTANT]  
> Asegúrate de que el Administrador de la organización de partner de recurso sigue la directriz de [lista de comprobación: configuración de la organización de Partner de recurso](Checklist--Configuring-the-Resource-Partner-Organization.md) para garantizar que se hayan completado todas las tareas de implementación necesarios para crear la segunda mitad de la asociación de federación correctamente.  
  
> [!NOTE]  
> Completar las tareas en esta lista de comprobación en orden. Cuando un vínculo de referencia tiene un procedimiento, vuelve a este tema después de completar los pasos de este procedimiento para que se puede continuar con las tareas restantes en esta lista de comprobación.  
  
![Configurar cuenta partner org](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**lista de comprobación: configuración de la organización de partner de cuenta**  
  
||Tarea|Referencia|  
|-|--------|-------------|  
|![Configurar cuenta partner org](media/icon_checkboxo.gif)|Si tienes una implementación de AD FS 1.0 o 1.1 existente en el entorno de producción hoy en día, consulta el vínculo a la derecha para obtener información acerca de cómo migrar la configuración de los servicios de federación de actual a un nuevo servicio de federación de AD FS. Si vas a implementar AD FS por primera vez en la organización con AD FS, puedes omitir este paso y continuar con la siguiente tarea en esta lista de comprobación para obtener información sobre cómo configurar una organización de partner de cuenta nueva.|![Configurar cuenta partner org](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[planear una migración a AD FS 2.0](https://technet.microsoft.com/library/ff678044.aspx)|  
|![Configurar cuenta partner org](media/icon_checkboxo.gif)|En función de los objetivos de implementación, revisa la información sobre los componentes necesarios para proporcionar a los usuarios con acceso a las aplicaciones federados.|![Configurar cuenta partner org](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[proporcionar tu Active Directory a los usuarios acceso a tus aplicaciones para notificaciones y servicios](https://technet.microsoft.com/library/dd807071.aspx)<br /><br />![Configurar cuenta partner org](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[proporcionar tu Active Directory a los usuarios acceso a las aplicaciones y servicios de otras organizaciones](https://technet.microsoft.com/library/dd807123.aspx)<br /><br />![Configurar cuenta partner org](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[proporcionar a los usuarios acceso a la otra organización a tus aplicaciones para notificaciones y servicios](https://technet.microsoft.com/library/dd807099.aspx)|  
|![Configurar cuenta partner org](media/icon_checkboxo.gif)|Determinar qué diseño de AD FS esta organización de partner de cuenta se asociará con.|![Configurar cuenta partner org](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Web SSO diseño](https://technet.microsoft.com/library/dd807033.aspx)<br /><br />![Configurar cuenta partner org](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[federados Web SSO diseño](https://technet.microsoft.com/library/dd807050.aspx)|  
|![Configurar cuenta partner org](media/icon_checkboxo.gif)|Antes de comenzar a implementar los servidores de AD FS, revisa la; 1. \) sus ventajas y desventajas de elegir Windows Internal Database \(WID\) o SQL Server para almacenar la configuración de AD FS base de datos 2. \) tipos de topología de implementación de AD FS y sus recomendaciones de diseño de red y la ubicación de servidor asociado.|![Configurar cuenta partner org](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[determinar la topología de implementación de AD FS](https://technet.microsoft.com/library/gg982491.aspx)<br /><br />![Configurar cuenta partner org](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[consideraciones de la topología de implementación de AD FS](https://technet.microsoft.com/library/gg982489.aspx)|  
|![Configurar cuenta partner org](media/icon_checkboxo.gif)|Revisa la capacidad de AD FS guías de planeación para determinar el número apropiado de servidor de federación y servidores proxy federación servidor que usas en tu entorno de producción.|![Configurar cuenta partner org](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[planear la capacidad de servidor de AD FS](https://technet.microsoft.com/library/gg749899.aspx)|  
|![Configurar cuenta partner org](media/icon_checkboxo.gif)|Para planear e implementar la topología física para la implementación de partner cuenta eficazmente, determinar si el diseño de AD FS requiere uno o varios servidores de federación o servidores proxy de servidor de federación.|![Configurar cuenta partner org](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[lista de comprobación: configurar un servidor de federación](Checklist--Setting-Up-a-Federation-Server.md)<br /><br />![Configurar cuenta partner org](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[lista de comprobación: configuración de un Proxy de servidor de federación](Checklist--Setting-Up-a-Federation-Server-Proxy.md)|  
|![Configurar cuenta partner org](media/icon_checkboxo.gif)|Determinar el tipo de almacén de atributo que quieras agregar a AD FS. A continuación, se agrega mediante la administración de AD FS snap\ en el almacén de atributo.|![Configurar cuenta partner org](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[las tiendas rol de atributo](../../ad-fs/technical-reference/The-Role-of-Attribute-Stores.md)<br /><br />![Configurar cuenta partner org](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[agregar un almacén de atributo](../../ad-fs/operations/Add-an-Attribute-Store.md)|  
|![Configurar cuenta partner org](media/icon_checkboxo.gif)|Si necesitas enviar notificaciones o consumir notificaciones de un socio de recurso que está usando ya sea un servicio de federación de AD FS 1.0 o 1.1, consulta el vínculo a la derecha para obtener información sobre cómo configurar AD FS para interoperar con versiones anteriores de AD FS. Si la organización de partner de recurso también está usando AD FS para enviar o consumir notificaciones en la organización, puedes omitir este paso y continuar con la siguiente tarea de esta lista de comprobación.|![Configurar cuenta partner org](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[planeación para la interoperabilidad con AD FS 1.x](https://technet.microsoft.com/library/ff678040.aspx)|  
|![Configurar cuenta partner org](media/icon_checkboxo.gif)|Una vez implementado el primer servidor de federación de la organización de partner de cuenta, crea una relación de confianza de terceros confianza con la administración de AD FS en snap\. Puedes crear una confianza de terceros de confianza escribiendo datos acerca de un asociado de recurso manualmente o mediante una dirección URL de metadatos de federación que le proporciona el Administrador de la organización de partner de recursos. Puedes usar los metadatos de federación para recuperar automáticamente los datos para el asociado de recurso. **Nota:** si el asociado de recurso publica sus metadatos federación o proporciona una copia del archivo de la misma para su uso, te recomendamos que recuperar automáticamente los datos porque puede ahorrar tiempo.|![Configurar cuenta partner org](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[crear un confiar terceros de confianza de forma manual](../../ad-fs/operations/Create-a-Relying-Party-Trust.md)<br /><br />![Configurar cuenta partner org](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[crear un confiar terceros de confianza usando federación metadatos](../../ad-fs/operations/Create-a-Relying-Party-Trust.md)|  
|![Configurar cuenta partner org](media/icon_checkboxo.gif)|Según las necesidades de la organización, crear uno o más conjuntos de reglas de notificación para cada usuario de confianza confianza de terceros que se especifica en la administración de AD FS en snap\ para que las notificaciones se emitirá correctamente.|![Configurar cuenta partner org](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[lista de comprobación: crear reglas de notificación para una confianza de terceros confiar](Checklist--Creating-Claim-Rules-for-a-Relying-Party-Trust.md)|  
|![Configurar cuenta partner org](media/icon_checkboxo.gif)|Debe crearse una descripción de la reclamación si no existe uno ya que cumplen las necesidades de la organización. Incluye un conjunto predeterminado de las descripciones de notificación que se exponen en la administración de AD FS snap\ en AD FS.|![Configurar cuenta partner org](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[agregar una descripción de la notificación](../../ad-fs/operations/Add-a-Claim-Description.md)|  
|![Configurar cuenta partner org](media/icon_checkboxo.gif)|Determinar si la organización, deberás utilizar la delegación de identidad para autorizar o restringir una cuenta especificada para "actuar como" o suplanten a otros usuarios. Este es un requisito a menudo cuando las aplicaciones Web front\ final deben interactuar con servicios Web back\.|![Configurar cuenta partner org](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[cuándo delegación de identidad de uso](https://technet.microsoft.com/library/dd807122.aspx)|  
|![Configurar cuenta partner org](media/icon_checkboxo.gif)|Preparar los equipos cliente federación por:<br /><br />-Agregar la dirección URL para el servidor de federación de asociado de cuenta a la lista de sitios de confianza para el explorador del cliente.<br />-Usando directivas de grupo para insertar los certificados de capa de Sockets seguros \(SSL\) apropiados en los equipos cliente.|![Configurar cuenta partner org](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[preparar los equipos cliente en la cuenta de Partner](https://technet.microsoft.com/library/dd807114(v=ws.11).aspx)<br /><br />![Configurar cuenta partner org](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[configurar equipos cliente para confiar en el servidor de federación de cuenta](Configure-Client-Computers-to-Trust-the-Account-Federation-Server.md)<br /><br />![Configurar cuenta partner org](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[distribuir certificados en los equipos cliente mediante la directiva de grupo](Distribute-Certificates-to-Client-Computers-by-Using-Group-Policy.md)| 