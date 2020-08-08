---
ms.assetid: c28c60ff-693d-49ee-a75b-58f24866217b
title: Requisitos de resolución de nombres para servidores proxy de federación
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 2122f2a6a3372650d321b456791d33147c4d1f85
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87945213"
---
# <a name="name-resolution-requirements-for-federation-server-proxies"></a>Requisitos de resolución de nombres para servidores proxy de federación

Cuando los equipos cliente en Internet intentan obtener acceso a una aplicación protegida por Servicios de federación de Active Directory (AD FS) \( AD FS \) , primero deben autenticarse en el servidor de Federación. En la mayoría de los casos, el servidor de Federación normalmente no es accesible directamente desde Internet. Por lo tanto, los equipos cliente de Internet se deben redirigir al servidor proxy de Federación en su lugar. Puede realizar el redireccionamiento correctamente agregando los registros DNS del sistema de nombres de dominio adecuados \( \) a la zona o zonas DNS que se encuentran en Internet.

El método que se usa para redirigir a los clientes de Internet al servidor proxy de Federación depende de cómo configure la zona DNS en la red perimetral o de cómo configure una zona DNS que usted controla en Internet. Los servidores proxy de federación están previstos para usarse en una red perimetral. Redirigen las solicitudes de cliente de Internet a los servidores de Federación correctamente solo cuando DNS se ha configurado correctamente en todas las \- zonas orientadas a Internet que usted controla. Por lo tanto, la configuración de las \- zonas orientadas a Internet (tanto si tiene una zona DNS que sirve únicamente a la red perimetral como si tiene una zona DNS que sirve a la red perimetral y a clientes de Internet) es importante.

En este tema se describen los pasos que puede seguir para configurar la resolución de nombres cuando se coloca un servidor proxy de Federación en la red perimetral. Para determinar los pasos que se deben seguir, determine primero cuál de los siguientes escenarios de DNS coincide más con la infraestructura DNS de la red perimetral de su organización. A continuación, siga los pasos para ese escenario.

## <a name="dns-zone-serving-only-the-perimeter-network"></a>Zona DNS que sirve solamente a la red perimetral
En este escenario, la organización tiene una o dos zonas DNS en la red perimetral y su organización no controla ninguna zona DNS en Internet. La resolución de nombres correcta para un servidor proxy de Federación en la zona DNS que solo sirve el escenario de red perimetral depende de las condiciones siguientes:

-   El servidor proxy de Federación debe tener una configuración en el archivo de hosts para resolver el FQDN del nombre de dominio completo \( \) de la dirección URL del punto de conexión del servidor de Federación en una dirección IP de un servidor de Federación o un clúster de servidores de Federación.

-   DNS en la red perimetral del asociado de cuenta debe configurarse para que el FQDN de la dirección URL del punto de conexión del servidor de Federación se resuelva en la dirección IP del servidor proxy de Federación.

En la ilustración siguiente y los pasos correspondientes se muestra cómo se cumple cada una de estas condiciones se consigue para un ejemplo determinado. En esta ilustración, la tecnología NLB de equilibrio de carga de red de Microsoft \( \) proporciona un FQDN de clúster único y una dirección IP de clúster única para una granja de servidores de Federación existente.

![requisitos de nombres](media/adfs2_deploy_single_fs.gif)

Para obtener más información sobre la configuración de una dirección IP de clúster o un FQDN de clúster con NLB, vea [especificar los parámetros de clúster](https://go.microsoft.com/fwlink/?LinkId=75282).

### <a name="1-configure-the-hosts-file-on-the-federation-server-proxy"></a>1. Configurar l archivo de hosts en el servidor proxy de federación.
Como DNS en la red perimetral está configurado para resolver todas las solicitudes de fs.fabrikam.com en el servidor proxy de Federación de la cuenta, el servidor proxy de Federación del asociado de cuenta tiene una entrada en su archivo de hosts local para resolver fs.fabrikam.com en la dirección IP del servidor de Federación de cuenta real \( o el nombre DNS del clúster para la granja de servidores de Federación \) que está conectada a la red corporativa. Esto permite que el servidor proxy de Federación de la cuenta resuelva el nombre de host fs.fabrikam.com en el servidor de Federación de la cuenta en lugar de hacerlo en sí mismo, como sucedería si intentase buscar fs.fabrikam.com con el DNS perimetral, de modo que el servidor proxy de Federación pueda comunicarse con el servidor de Federación.

### <a name="2-configure-perimeter-dns"></a>2. Configurar DNS perimetral
Dado que solo hay un nombre de host AD FS a la que se dirigen los equipos cliente (tanto si están en una intranet como en Internet), los equipos cliente de Internet que usan el servidor DNS perimetral deben resolver el FQDN del servidor de Federación de cuenta \( FS.fabrikam.com \) con la dirección IP del proxy de servidor de Federación de la cuenta en la red perimetral. Para que pueda reenviar a los clientes el proxy de servidor de Federación de la cuenta cuando intenten resolver fs.fabrikam.com, el DNS perimetral contiene una zona DNS corp.fabrikam.com limitada con un solo host de \( un \) registro de recursos para FS \( FS.fabrikam.com \) y la dirección IP del proxy de servidor de Federación de la cuenta en la red perimetral.

Para obtener más información acerca de cómo modificar el archivo de hosts del servidor proxy de Federación y configurar DNS en la red perimetral, consulte [configuración de la resolución de nombres para un servidor proxy de Federación en una zona DNS que sirve solo a la red perimetral](../deployment/configure-name-resolution-for-federation-server-proxy-in-dns-zone-serving-only-perimeter-network.md).

## <a name="dns-zone-serving-both-the-perimeter-network-and-internet-clients"></a>Zona DNS que sirve a la red perimetral y a clientes de Internet
En este escenario, su organización controla la zona DNS en la red perimetral y al menos una zona DNS en Internet. Una resolución de nombres correcta para un servidor proxy de Federación en este escenario depende de las condiciones siguientes:

-   DNS en la zona de Internet del asociado de cuenta debe configurarse para que el FQDN del nombre de host del servidor de Federación se resuelva en la dirección IP del servidor proxy de Federación en la red perimetral.

-   DNS en la red perimetral del asociado de cuenta debe configurarse para que el FQDN del nombre de host del servidor de Federación se resuelva en la dirección IP del servidor de Federación de la red corporativa.

En la ilustración siguiente y los pasos correspondientes se muestra cómo se cumple cada una de estas condiciones se consigue para un ejemplo determinado.

![requisitos de nombres](media/adfs2_deploy_fsp_3DNS.gif)

### <a name="1-configure-perimeter-dns"></a>1. Configurar DNS perimetral
En este escenario, como se supone que configurará la zona DNS de Internet que controla para resolver las solicitudes que se realizan para una dirección URL de punto de conexión específica \( , es decir, FS.fabrikam.com \) al servidor proxy de Federación en la red perimetral, también debe configurar la zona en el DNS perimetral para enviar estas solicitudes al servidor de Federación en la red corporativa.

Para que los clientes puedan reenviarse al servidor de Federación de la cuenta cuando intentan resolver fs.fabrikam.com, el DNS perimetral está configurado con un solo host \( un \) registro de recursos para FS \( FS.fabrikam.com \) y la dirección IP del servidor de Federación de la cuenta en la red corporativa. Esto permite que el servidor proxy de Federación de la cuenta resuelva el nombre de host fs.fabrikam.com en el servidor de Federación de la cuenta en lugar de hacerlo en sí mismo, como sucedería si intentase buscar fs.fabrikam.com con DNS de Internet, de modo que el servidor proxy de Federación pueda comunicarse con el servidor de Federación.

### <a name="2-configure-internet-dns"></a>2. Configurar DNS en Internet
Para que la resolución de nombres sea correcta en este escenario, todas las solicitudes de los equipos cliente en Internet a fs.fabrikam.com las debe resolver la zona DNS de Internet que usted controla. Por lo tanto, debe configurar la zona DNS de Internet para que reenvíe las solicitudes de cliente para fs.fabrikam.com a la dirección IP del servidor proxy de Federación de la cuenta en la red perimetral.

Para obtener más información acerca de cómo modificar la red perimetral y las zonas DNS de Internet, consulte [configuración de la resolución de nombres para un servidor proxy de Federación en una zona DNS que sirve a la red perimetral y a los clientes de Internet](../deployment/configure-name-resolution-for-federation-server-proxy-in-dns-zone-serving-only-perimeter-network.md).

## <a name="see-also"></a>Consulte también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
