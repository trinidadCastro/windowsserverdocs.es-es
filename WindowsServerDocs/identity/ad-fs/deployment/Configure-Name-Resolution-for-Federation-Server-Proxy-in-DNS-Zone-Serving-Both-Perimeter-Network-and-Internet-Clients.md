---
ms.assetid: 1a6740e6-5b6d-41f8-9ec4-32cdbee3e1bb
title: Configurar la resolución de nombres para un servidor proxy de federación de una zona DNS que da servicio tanto a la red perimetral como a los clientes de Internet
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 487ba9d90043ada0d401d7e5a9d02872e1872b7e
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854938"
---
# <a name="configure-name-resolution-for-a-federation-server-proxy-in-a-dns-zone-that-serves-both-the-perimeter-network-and-internet-clients"></a>Configurar la resolución de nombres para un servidor proxy de federación de una zona DNS que da servicio tanto a la red perimetral como a los clientes de Internet


Para que la resolución de nombres funcione correctamente para un servidor proxy de Federación en una Servicios de federación de Active Directory (AD FS) \(AD FS escenario de\) en el que uno o varios sistemas de nombres de dominio \(zonas\) DNS atienden a la red perimetral y a los clientes de Internet, se deben completar las siguientes tareas:  
  
-   El DNS de la zona de Internet que controla debe estar configurado para resolver todas las solicitudes de cliente de Internet para el nombre de host de AD FS en el servidor proxy de Federación. Para ello, agregue un host \(un registro de recursos\) a la zona DNS de Internet para el servidor proxy de Federación.  
  
-   DNS en la red perimetral debe estar configurado para resolver todas las solicitudes de cliente entrantes para el nombre de host de AD FS en el servidor de Federación. Para ello, agregue un host \(un registro de recursos\) a la zona DNS perimetral para el servidor proxy de Federación.  
  
> [!NOTE]  
> Se supone que ya se ha creado un host \(un registro de recursos\) para el servidor de Federación en el DNS de la red corporativa. Si este registro todavía no existe, cree este registro y, a continuación, realice estos procedimientos. Para obtener más información acerca de cómo crear un host \(un registro de recursos\) para el servidor de Federación, consulte [ &#40;agregar&#41; un registro de recursos de host a a DNS corporativo para un servidor de Federación](Add-a-Host--A--Resource-Record-to-Corporate-DNS-for-a-Federation-Server.md).  
  
## <a name="add-a-host-a-resource-record-to-the-internet-dns-zone-for-a-federation-server-proxy"></a>Agregar un host \(un registro de recursos de\) a la zona DNS de Internet para un servidor proxy de Federación  
Para que los equipos cliente de Internet puedan tener acceso correctamente a un servidor de Federación a través de un proxy de servidor de Federación recién implementado, primero debe crear un host \(un\) registro de recursos en la zona DNS de Internet que usted controla. Este registro de recursos resuelve el nombre de host del servidor de Federación de la cuenta \(por ejemplo, fs.fabrikam.com\) a la dirección IP del servidor proxy de Federación de la cuenta \(por ejemplo, 131.107.27.68\) en la red perimetral.  
  
> [!NOTE]  
> Se supone que usa un servidor DNS que ejecuta Windows 2000 Server, Windows Server 2003 o Windows Server 2008 con el servicio servidor DNS para controlar la zona DNS de Internet.  
  
La pertenencia al grupo **administradores**, o equivalente, es lo mínimo necesario para completar este procedimiento.  Revise los detalles sobre el uso de las cuentas y pertenencias a grupos adecuadas en [grupos predeterminados locales y de dominio](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
#### <a name="to-add-a-host-a-resource-record-to-the-internet-dns-zone-for-a-federation-server-proxy"></a>Para agregar un host \(un registro de recursos de\) a la zona DNS de Internet para un servidor proxy de Federación  
  
1.  En un servidor DNS para la zona DNS de Internet, abra el\-de complemento DNS en.  
  
2.  En el árbol de consola, haga clic con el botón secundario\-en la zona de búsqueda directa aplicable y, a continuación, haga clic en **nuevo Host \(a o AAAA\)** .  
  
3.  En **nombre**, escriba solo el nombre de equipo del servidor de Federación. Por ejemplo, para el nombre de dominio completo \(FQDN\) fs.fabrikam.com, escriba **FS**.  
  
4.  En **dirección IP**, escriba la dirección IP del nuevo servidor proxy de Federación, por ejemplo, 131.107.27.68.  
  
5.  Haga clic en **Agregar host**.  
  
## <a name="add-a-host-a-resource-record-to-the-perimeter-dns-zone-for-a-federation-server-proxy"></a>Agregar un host \(un registro de recursos de\) a la zona DNS perimetral para un servidor proxy de Federación  
Para que el servidor proxy de Federación pueda procesar correctamente las solicitudes de cliente de Internet y llegar al servidor de Federación después de que las resuelva la zona DNS de Internet, debe crear un host \(un\) registro de recursos en la zona DNS perimetral. Este registro de recursos resuelve el nombre de host del servidor de Federación de la cuenta \(por ejemplo, FS. fabrikam.com\) a la dirección IP del servidor de Federación de cuenta \(por ejemplo,\) 192.168.1.4 en la red corporativa.  
  
> [!NOTE]  
> Se supone que usa un servidor DNS que ejecuta Windows 2000 Server, Windows Server 2003, Windows Server 2008 o Windows Server&reg; 2012 con el servicio servidor DNS para controlar la zona DNS perimetral.  
  
La pertenencia al grupo **administradores**, o equivalente, es lo mínimo necesario para completar este procedimiento.  Revise los detalles sobre el uso de las cuentas y pertenencias a grupos adecuadas en [grupos predeterminados locales y de dominio](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
#### <a name="to-add-a-host-a-resource-record-to-the-perimeter-dns-zone-for-a-federation-server-proxy"></a>Para agregar un host \(un registro de recursos\) a la zona DNS perimetral para un servidor proxy de Federación  
  
1.  En un servidor DNS para la red perimetral, abra el **\-de complemento DNS en**.  
  
2.  En el árbol de consola, haga clic con el botón secundario\-en la zona de búsqueda directa aplicable y, a continuación, haga clic en **nuevo Host \(a o AAAA\)** .  
  
3.  En **nombre**, escriba solo el nombre de equipo del servidor de Federación. Por ejemplo, para el FQDN fs.fabrikam.com, escribe **fs**.  
  
4.  En el cuadro de texto **dirección IP** , escriba la dirección IP del servidor de Federación en la red corporativa, por ejemplo, 192.168.1.4.  
  
5.  Haga clic en **Agregar host**.  
  
## <a name="additional-references"></a>Referencias adicionales  
[Lista de comprobación: configuración de un servidor proxy de Federación](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  
[Requisitos de resolución de nombres para servidores proxy de federación](https://technet.microsoft.com/library/dd807055.aspx)  
  

