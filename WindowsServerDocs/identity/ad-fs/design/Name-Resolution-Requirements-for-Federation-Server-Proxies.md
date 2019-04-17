---
ms.assetid: c28c60ff-693d-49ee-a75b-58f24866217b
title: "Requisitos de resolución de nombre de Proxies de servidor de federación"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: a94e4de181cd8794d479bbd6695a94658aba0f86
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="name-resolution-requirements-for-federation-server-proxies"></a>Requisitos de resolución de nombre de Proxies de servidor de federación

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cuando los equipos cliente en Internet intentan acceder a una aplicación que está protegida por los servicios de federación de Active Directory \(AD FS\), primero deben autenticarse en el servidor de federación. En la mayoría de los casos, el servidor de federación normalmente no es accesible directamente desde Internet. Por lo tanto, los equipos cliente Internet deben ser redirigidos al proxy de servidor de federación en su lugar. Agregar los registros de sistema de nombres de dominio \(DNS\) adecuados para tu zona DNS o zonas que se enfrentan a Internet, puedes realizar redirección correcta.  
  
El método que se utiliza para redirigir a los clientes de Internet para el proxy de servidor de federación depende de cómo configurar la zona DNS en la red perimetral o cómo configurar una zona DNS que controlan en Internet. Proxies de servidor de federación están diseñados para usarse en una red de perímetro. Redirección las solicitudes de cliente de Internet a los servidores de federación correctamente solo cuando DNS se ha configurado correctamente en todas las zonas de Internet\ frontal Tú controlas. Por lo tanto, la configuración de las zonas Internet\ frontal: Si tienes una zona DNS Servers solo la red perimetral o una zona DNS para la red de perímetro y los clientes de Internet, es importante.  
  
Este tema describe los pasos que puede seguir para configurar la resolución de nombres al colocar un proxy de servidor de federación de la red perimetral. Para determinar los pasos que debes seguir, primero determinar cuál de los siguientes escenarios DNS coincide más estrechamente con la infraestructura DNS en la red de perímetro de la organización. A continuación, sigue los pasos de este escenario.  
  
## <a name="dns-zone-serving-only-the-perimeter-network"></a>Zona DNS Servers solo la red perimetral  
En este escenario, la organización tiene uno o dos zonas DNS en la red perimetral y la organización no controla las zonas DNS en Internet. Resolución de nombres correcto para un proxy de federación de servidor en la zona DNS que sirve solo el escenario de red perimetral depende de las siguientes condiciones:  
  
-   El proxy de servidor de federación debe tener una configuración en el archivo de hosts para resolver el nombre de dominio completo \(FQDN\) de la URL de extremo del servidor de federación en una dirección IP de un servidor de federación o un clúster de servidor de federación de nombre.  
  
-   DNS en la red perimetral de asociado de la cuenta debe configurarse para que el nombre completo de la URL de extremo del servidor de federación resuelve a la dirección IP del servidor proxy de servidor de federación.  
  
La siguiente ilustración y los pasos correspondientes muestran cómo cada una de estas condiciones se consigue para ver un ejemplo concreto. En esta ilustración, tecnología \(NLB\) equilibrio de carga de red de Microsoft proporciona un único clúster FQDN y un único, dirección IP para un conjunto de servidor de federación existente.  
  
![Requisitos de nombre](media/adfs2_deploy_single_fs.gif)  
  
Para obtener más información sobre cómo configurar una dirección IP del clúster o un clúster FQDN con NLB, consulta [especificar los parámetros de clúster](https://go.microsoft.com/fwlink/?LinkId=75282).  
  
### <a name="1-configure-the-hosts-file-on-the-federation-server-proxy"></a>1. Configurar el archivo de hosts en el proxy de servidor de federación  
Porque DNS en la red perimetral está configurado para resolver todas las solicitudes de fs.fabrikam.com para el proxy de servidor de federación de cuenta, el proxy de servidor de federación de partner de cuenta tiene una entrada en su archivo de hosts locales para resolver fs.fabrikam.com a la dirección IP del servidor de federación de cuenta real \ (o nombre de clúster DNS para la farm\ de servidor de federación) que esté conectado a la red corporativa. Esto hace posible para el proxy de servidor de federación de cuenta resolver el nombre de host fs.fabrikam.com al servidor de federación de cuenta, en lugar de a sí mismo, como se produciría si intentó buscar fs.fabrikam.com con perímetro DNS, para que el proxy de federación de servidor se puede comunicar con el servidor de federación.  
  
### <a name="2-configure-perimeter-dns"></a>2. Configurar perímetro DNS  
Ya que hay solo un único AD FS nombre de host que los equipos cliente se dirigen a: si están en una intranet o en Internet, los equipos cliente en Internet que usan el servidor DNS de perímetro deben resolver el nombre completo de la cuenta federación servidor \(fs.fabrikam.com\) a la dirección IP del proxy de servidor de federación de cuentas en la red de perímetro. Para que los clientes en el proxy de servidor de federación de cuenta, puedes reenviar cuando intenta resolver fs.fabrikam.com, perímetro DNS contiene una zona DNS corp.fabrikam.com limitado con un registro de recursos de un único host \(A\) para fs \(fs.fabrikam.com\) y la dirección IP del proxy de servidor de federación de cuentas en la red de perímetro.  
  
Para obtener más información acerca de cómo modificar el archivo de hosts de proxy del servidor de federación y configurar DNS en la red perimetral, consulta [configurar la resolución de nombres de un servidor Proxy de servidor de federación en una zona DNS que sirve solo la red perimetral](../../ad-fs/deployment/Configure-Name-Resolution-for-a-Federation-Server-Proxy-in-a-DNS-Zone-That-Serves-Only-the-Perimeter-Network.md).  
  
## <a name="dns-zone-serving-both-the-perimeter-network-and-internet-clients"></a>Zona DNS para la red de perímetro y los clientes de Internet  
En este escenario, tu organización controla la zona DNS en la red perimetral y al menos una zona DNS en Internet. Resolución de nombres correcto para un proxy de federación de servidor en este escenario depende de las siguientes condiciones:  
  
-   DNS en la zona de Internet de asociado de la cuenta debe configurarse para que el nombre completo del nombre de host del servidor de federación resuelve a la dirección IP del servidor proxy de servidor de federación en la red perimetral.  
  
-   DNS en la red perimetral de asociado de la cuenta debe configurarse para que el nombre completo del nombre de host del servidor de federación resuelve a la dirección IP del servidor de federación de la red corporativa.  
  
La siguiente ilustración y los pasos correspondientes muestran cómo cada una de estas condiciones se consigue para ver un ejemplo concreto.  
  
![Requisitos de nombre](media/adfs2_deploy_fsp_3DNS.gif)  
  
### <a name="1-configure-perimeter-dns"></a>1. Configurar perímetro DNS  
Para este escenario, ya que se supone que se configura la zona de Internet DNS control para resolver las solicitudes que están pensadas para una dirección URL de extremo específico \ (es decir, fs.fabrikam.com\) para el proxy de servidor de federación de la red perimetral, también debes configurar la zona en el perímetro de DNS para reenviar estas solicitudes al servidor de federación de la red corporativa.  
  
Para que los clientes pueden reenviarse al servidor de federación de cuenta cuando intenta resolver fs.fabrikam.com, perímetro DNS está configurado con un registro de recursos de un único host \(A\) para fs \(fs.fabrikam.com\) y la dirección IP del servidor de federación de cuenta en la red corporativa. Esto permite que el proxy de servidor de federación de cuenta resolver el nombre de host fs.fabrikam.com al servidor de federación de cuenta, en lugar de a sí mismo, como se produciría si intentó buscar fs.fabrikam.com mediante DNS de Internet, para que el proxy de federación de servidor se puede comunicar con el servidor de federación.  
  
### <a name="2-configure-internet-dns"></a>2. Configurar Internet DNS  
Resolución de nombres se realice correctamente en este escenario, todas las solicitudes de los equipos cliente en Internet para fs.fabrikam.com deben resolverse la zona de Internet DNS que tú controlas. Por lo tanto, debes configurar la zona de Internet DNS para reenviar las solicitudes de cliente para fs.fabrikam.com a la dirección IP del proxy de servidor de federación de cuentas en la red perimetral.  
  
Para obtener más información sobre cómo modificar de la red perimetral y las zonas Internet DNS, consulta [configurar la resolución de nombre de un servidor Proxy de servidor de federación de DNS zona que funciona tanto la red perimetral y los clientes de Internet](../../ad-fs/deployment/Configure-Name-Resolution-for-a-Federation-Server-Proxy-in-a-DNS-Zone-That-Serves-Both-the-Perimeter-Network-and-Internet-Clients.md).  
  
## <a name="see-also"></a>Consulta también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
