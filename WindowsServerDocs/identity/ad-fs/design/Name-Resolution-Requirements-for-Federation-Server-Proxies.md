---
ms.assetid: c28c60ff-693d-49ee-a75b-58f24866217b
title: Requisitos de resolución de nombres para servidores proxy de federación
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: a94e4de181cd8794d479bbd6695a94658aba0f86
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855026"
---
# <a name="name-resolution-requirements-for-federation-server-proxies"></a>Requisitos de resolución de nombres para servidores proxy de federación

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cuando los equipos cliente de Internet intentan acceder a una aplicación que está protegida por Active Directory Federation Services \(AD FS\), primero deben autenticarse en el servidor de federación. En la mayoría de los casos, el servidor de federación normalmente no es directamente accesible desde Internet. Por lo tanto, los equipos cliente de Internet se deben redirigir al servidor proxy de federación en su lugar. Puede lograr el redireccionamiento correctamente agregando el sistema de nombres de dominio adecuado \(DNS\) registros a la zona DNS o zonas con conexión a Internet.  
  
El método que se utiliza para redirigir a los clientes de Internet para el servidor proxy de federación depende de cómo configurar la zona DNS en la red perimetral o cómo configurar una zona DNS que usted controla en Internet. Los servidores proxy de federación están previstos para usarse en una red perimetral. Redirigen las solicitudes de cliente de Internet a servidores de federación correctamente solo cuando DNS se ha configurado correctamente en todo Internet\-orientado a las zonas que usted controla. Por lo tanto, la configuración de la Internet\-orientado a las zonas, si tiene una zona DNS que sirve a solo la red perimetral o en una zona DNS que sirve a tanto la red perimetral y los clientes de Internet, es importante.  
  
En este tema se describe los pasos que puede seguir para configurar la resolución de nombres cuando se coloca un servidor proxy de federación en la red perimetral. Para determinar los pasos que se deben seguir, determine primero cuál de los siguientes escenarios de DNS coincide más con la infraestructura DNS de la red perimetral de su organización. A continuación, siga los pasos para ese escenario.  
  
## <a name="dns-zone-serving-only-the-perimeter-network"></a>Zona DNS que sirve solamente a la red perimetral  
En este escenario, la organización tiene una o dos zonas DNS en la red perimetral y su organización no controla ninguna zona DNS en Internet. Resolución de nombres correcta para un servidor proxy de federación en la zona DNS que sirve solo el escenario de red perimetral depende de las condiciones siguientes:  
  
-   El servidor proxy de federación debe tener una configuración en el archivo de hosts para resolver el nombre de dominio completo \(FQDN\) de la URL del punto de conexión de servidor de federación a una dirección IP de un servidor de federación o un clúster de servidores de federación.  
  
-   DNS en la red perimetral del asociado de cuenta debe estar configurado para que el FQDN de la URL del punto de conexión de servidor de federación se resuelve en la dirección IP del servidor proxy de federación.  
  
En la ilustración siguiente y los pasos correspondientes se muestra cómo se cumple cada una de estas condiciones se consigue para un ejemplo determinado. En esta ilustración, equilibrio de carga de red de Microsoft \(NLB\) tecnología proporciona un único, el FQDN de clúster y una única dirección IP del clúster para una granja de servidores de federación existente.  
  
![requisitos de nombre](media/adfs2_deploy_single_fs.gif)  
  
Para obtener más información sobre cómo configurar una dirección IP del clúster o un FQDN de clúster con NLB, vea [especificando los parámetros de clúster](https://go.microsoft.com/fwlink/?LinkId=75282).  
  
### <a name="1-configure-the-hosts-file-on-the-federation-server-proxy"></a>1. Configurar l archivo de hosts en el servidor proxy de federación.  
Dado que DNS en la red perimetral está configurado para resolver todas las solicitudes para fs.fabrikam.com en el servidor proxy de federación de cuenta, el proxy de servidor de federación de asociado de cuenta tiene una entrada en su archivo de hosts local para resolver fs.fabrikam.com a la dirección IP de la servidor de federación de cuenta real \(o nombre DNS del clúster para la granja de servidores de federación\) que está conectado a la red corporativa. Esto hace posible para el servidor proxy de federación de cuenta resolver el nombre de host fs.fabrikam.com en el servidor de federación de cuenta en lugar de a sí mismo, como ocurriría si intentase buscar fs.fabrikam.com usando un DNS perimetral, por lo que la federación proxy de servidor puede comunicarse con el servidor de federación.  
  
### <a name="2-configure-perimeter-dns"></a>2. Configurar DNS perimetral  
Dado que hay solo un único host nombre de AD FS que los equipos cliente se dirigen a, si se encuentran en una intranet o en Internet, los equipos cliente en Internet que utilizan el servidor DNS perimetral deben resolver el FQDN del servidor de federación de cuenta \(fs.fabrikam.com\) a la dirección IP del proxy de servidor de federación de cuentas en la red perimetral. Para que pueden reenviar a los clientes del servidor proxy de federación de cuenta cuando intentan resolver fs.fabrikam.com, el DNS perimetral contiene una zona DNS corp.fabrikam.com limitada con un único host \(A\) registro de recursos de fs \(fs.fabrikam.com\) y la dirección IP del proxy de servidor de federación de cuentas en la red perimetral.  
  
Para obtener más información acerca de cómo modificar el archivo de hosts del servidor proxy de federación y configurar DNS en la red perimetral, consulte [configurar la resolución de nombres para un servidor Proxy de federación en una zona DNS que sirve de la red de perímetro](../../ad-fs/deployment/Configure-Name-Resolution-for-a-Federation-Server-Proxy-in-a-DNS-Zone-That-Serves-Only-the-Perimeter-Network.md).  
  
## <a name="dns-zone-serving-both-the-perimeter-network-and-internet-clients"></a>Zona DNS que sirve a la red perimetral y a clientes de Internet  
En este escenario, su organización controla la zona DNS en la red perimetral y al menos una zona DNS en Internet. Resolución de nombres correcta para un servidor proxy de federación en este escenario depende de las condiciones siguientes:  
  
-   DNS en la zona de Internet del asociado de cuenta debe estar configurado para que el FQDN del nombre de host del servidor de federación se resuelve en la dirección IP del servidor proxy de federación en la red perimetral.  
  
-   DNS en la red perimetral del asociado de cuenta debe estar configurado para que el FQDN del nombre de host del servidor de federación se resuelve en la dirección IP del servidor de federación de la red corporativa.  
  
En la ilustración siguiente y los pasos correspondientes se muestra cómo se cumple cada una de estas condiciones se consigue para un ejemplo determinado.  
  
![requisitos de nombre](media/adfs2_deploy_fsp_3DNS.gif)  
  
### <a name="1-configure-perimeter-dns"></a>1. Configurar DNS perimetral  
En este escenario, porque se supone que configurará la zona DNS de Internet que controla para resolver las solicitudes que se realizan para una dirección URL de punto de conexión concreto \(es decir, fs.fabrikam.com\) para el servidor proxy de federación en el red perimetral, también debe configurar la zona en el DNS perimetral para enviar estas solicitudes al servidor de federación en la red corporativa.  
  
Para que los clientes pueden reenviarse al servidor de federación de cuenta cuando intentan resolver fs.fabrikam.com, el DNS perimetral está configurado con un único host \(A\) registro de recursos de fs \(fs.fabrikam.com\) y la dirección IP del servidor de federación de cuenta en la red corporativa. Esto hace posible para el servidor proxy de federación de cuenta resolver el nombre de host fs.fabrikam.com en el servidor de federación de cuenta en lugar de a sí mismo, como ocurriría si intentase buscar fs.fabrikam.com usando DNS de Internet, para que el servidor de federación proxy puede comunicarse con el servidor de federación.  
  
### <a name="2-configure-internet-dns"></a>2. Configurar DNS en Internet  
Para que la resolución de nombres sea correcta en este escenario, todas las solicitudes de los equipos cliente en Internet a fs.fabrikam.com las debe resolver la zona DNS de Internet que usted controla. Por lo tanto, debe configurar la zona DNS de Internet para que reenvíe las solicitudes de cliente para fs.fabrikam.com a la dirección IP del proxy de servidor de federación de cuentas en la red perimetral.  
  
Para obtener más información sobre cómo modificar la red perimetral y las zonas DNS de Internet, consulte [configurar la resolución de nombres para un servidor Proxy de federación en DNS zona que sirve tanto la red perimetral y los clientes de Internet](../../ad-fs/deployment/Configure-Name-Resolution-for-a-Federation-Server-Proxy-in-a-DNS-Zone-That-Serves-Both-the-Perimeter-Network-and-Internet-Clients.md).  
  
## <a name="see-also"></a>Vea también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
