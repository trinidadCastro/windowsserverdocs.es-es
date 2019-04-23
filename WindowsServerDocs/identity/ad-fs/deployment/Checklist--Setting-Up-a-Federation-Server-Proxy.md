---
ms.assetid: 38c9bcd3-c6f8-4153-8e42-5fd31568c65a
title: 'Lista de comprobación: configurar un servidor Proxy de federación'
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 57951787b1ac7694cacca170b376087a60e34543
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59844056"
---
# <a name="checklist-setting-up-a-federation-server-proxy"></a>Lista de comprobación: Configuración de un servidor proxy de federación

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Esta lista de comprobación incluye las tareas de implementación para preparar un servidor que ejecuta Windows Server® 2012 para el rol de proxy del servidor de federación de Active Directory Federation Services \(AD FS\).  
  
> [!NOTE]  
> Complete las tareas de esta lista de comprobación en orden. Cuando un vínculo de referencia le dirija a un procedimiento, vuelva a este tema tras completar los pasos de este procedimiento, de tal forma que pueda continuar con las tareas restantes de la lista de comprobación.  
  
![configuración de un servidor proxy federado](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**lista de comprobación: Configurar un servidor proxy de federación**  
  
||Tarea|Referencia|  
|-|--------|-------------|  
|![Cómo configurar un servidor proxy federado](media/icon_checkboxo.gif)|Antes de empezar a implementar a servidores proxy de federación AD FS, revise los tipos de topología de implementación de AD FS y su colocación del servidor asociado y las recomendaciones de diseño de red.|![configuración de un servidor proxy federado](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[determinar la topología de implementación de AD FS](https://technet.microsoft.com/library/gg982491.aspx)<br /><br />![configuración de un servidor proxy federado](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[planear la ubicación del Proxy de servidor de federación](https://technet.microsoft.com/library/dd807130.aspx)<br /><br />![configuración de un servidor proxy federado](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[dónde colocar un servidor Proxy de federación](https://technet.microsoft.com/library/dd807048.aspx)|  
|![Cómo configurar un servidor proxy federado](media/icon_checkboxo.gif)|Revise la Guía de planeamiento de capacidad AD FS para determinar el número apropiado de servidores proxy de federación que debe usar en su entorno de producción.|![configuración de un servidor proxy federado](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[planear la capacidad de Proxy de servidor de federación](https://technet.microsoft.com/library/gg749898.aspx)|  
|![Cómo configurar un servidor proxy federado](media/icon_checkboxo.gif)|Determinar si un servidor proxy de federación único o una granja de proxy de servidor de federación es mejor para su implementación. **Nota:** Los servidores de federación también realizan responsabilidades de proxy de servidor de federación.|![configuración de un servidor proxy federado](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[cuándo se debe crear un servidor Proxy de federación](https://technet.microsoft.com/library/dd807032.aspx)<br /><br />![configuración de un servidor proxy federado](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[cuándo se debe crear una granja de Proxy de servidor de federación](https://technet.microsoft.com/library/dd807082.aspx)|  
|![Cómo configurar un servidor proxy federado](media/icon_checkboxo.gif)|Determinar si se creará este nuevo servidor proxy de federación en la red perimetral de la organización del asociado de cuenta o la organización del asociado de recurso.|![configuración de un servidor proxy federado](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[revisar el rol de servidor Proxy de federación del asociado de cuenta](https://technet.microsoft.com/library/dd807109.aspx)<br /><br />![configuración de un servidor proxy federado](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[revisar el rol de servidor Proxy de federación del asociado de recurso](https://technet.microsoft.com/library/dd807052.aspx)|  
|![Cómo configurar un servidor proxy federado](media/icon_checkboxo.gif)|Antes de instalar AD FS en un equipo que se convertirá en un servidor proxy de federación, lea sobre la importancia de obtención de un servidor de certificado de autenticación: para granjas de servidores proxy de federación: agregar o compartir certificados en todos los servidores en una granja de servidores.|![configuración de un servidor proxy federado](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[requisitos de certificados para servidores proxy de federación](https://technet.microsoft.com/library/dd807054.aspx)|  
|![Cómo configurar un servidor proxy federado](media/icon_checkboxo.gif)|Revise la información de la Guía de diseño de AD FS acerca de cómo actualizar el sistema de nombres de dominio \(DNS\) en la red perimetral, por lo que puede producir esa resolución de nombres correcta para los servidores de federación y servidores proxy de federación.|![configuración de un servidor proxy federado](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Name Resolution Requirements for Federation Server Proxies](https://technet.microsoft.com/library/dd807055.aspx)|  
|![Cómo configurar un servidor proxy federado](media/icon_checkboxo.gif)|Determinar si el servidor proxy de federación debe estar unido a un dominio. Aunque los servidores proxy de federación no debe estar unido a un dominio, son más fáciles de administrar con administración remota y las características de la directiva de grupo cuando están unidos a un dominio.|![configuración de un servidor proxy federado](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[unir un equipo a un dominio](Join-a-Computer-to-a-Domain.md)|  
|![Cómo configurar un servidor proxy federado](media/icon_checkboxo.gif)|Según cómo esté configurada la infraestructura DNS en la red perimetral, complete uno de los procedimientos en los temas de la derecha antes de implementar a un servidor proxy de federación de su organización. **Nota:** No realice ambos procedimientos. Lectura [Name Resolution Requirements for Federation Server Proxies](https://technet.microsoft.com/library/dd807055.aspx) para determinar qué procedimiento que mejor se adapte a los requisitos de su organización.|![configuración de un servidor proxy federado](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[configurar la resolución de nombres para un servidor Proxy de federación en una zona DNS que sirve solo la red perimetral](Configure-Name-Resolution-for-a-Federation-Server-Proxy-in-a-DNS-Zone-That-Serves-Only-the-Perimeter-Network.md)<br /><br />![configuración de un servidor proxy federado](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[configurar la resolución de nombres para un servidor Proxy de federación en DNS zona que sirve tanto la red perimetral y los clientes de Internet](Configure-Name-Resolution-for-a-Federation-Server-Proxy-in-a-DNS-Zone-That-Serves-Both-the-Perimeter-Network-and-Internet-Clients.md)|  
|![Cómo configurar un servidor proxy federado](media/icon_checkboxo.gif)|Después de obtener un servidor de certificados de autenticación, debe instalarlo en Internet Information Services \(IIS\) en el sitio Web predeterminado del servidor proxy de federación.|![configuración de un servidor proxy federado](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[importar un certificado de autenticación de servidor al sitio Web predeterminado](Import-a-Server-Authentication-Certificate-to-the-Default-Web-Site.md)|  
|![Cómo configurar un servidor proxy federado](media/icon_checkboxo.gif)|\(Opcional\) como una alternativa para obtener un servidor de certificados de autenticación de una entidad de certificación \(CA\), puede utilizar IIS para adquirir un certificado de ejemplo para el servidor proxy de federación.<br /><br />Dado que IIS genera un autoservicio\-certificado firmado que no proceden de un origen de confianza, usarlo para crear un\-firmó el certificado sólo en los escenarios siguientes:<br /><br />-Cuando se tiene que crear una capa de Sockets seguros \(SSL\) canal entre el servidor y un grupo limitado, conocido de usuarios<br />-Cuando tenga que solucionar en tercer lugar\-problemas de certificados de entidad **Precaución:** No es una práctica recomendada de seguridad para implementar un servidor proxy de federación en un entorno de producción mediante un autoservicio\-con signo, certificado de autenticación de servidor.|![configuración de un servidor proxy federado](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[IIS: Crear un\-firmó el certificado de servidor](https://go.microsoft.com/fwlink/?LinkID=108271)|  
|![Cómo configurar un servidor proxy federado](media/icon_checkboxo.gif)|Instalar el servicio de rol Proxy de servicio de federación en el equipo que se convertirá en el servidor proxy de federación.|![configuración de un servidor proxy federado](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[instalar el servicio de rol Proxy de servicio de federación](Install-the-Federation-Service-Proxy-Role-Service.md)|  
|![Cómo configurar un servidor proxy federado](media/icon_checkboxo.gif)|Configurar el software AD FS en el equipo para que actúe en el rol de servidor proxy de federación usando el Asistente para configuración de Proxy de AD FSFederation Server.|![configuración de un servidor proxy federado](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[configurar un equipo para el rol de servidor Proxy de federación](Configure-a-Computer-for-the-Federation-Server-Proxy-Role.md)|  
|![Cómo configurar un servidor proxy federado](media/icon_checkboxo.gif)|En el Visor de eventos, compruebe que el servicio de servidor proxy de federación se ha iniciado.|![configuración de un servidor proxy federado](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[Compruebe que un Federation Server Proxy Is Operational](Verify-That-a-Federation-Server-Proxy-Is-Operational.md)|  