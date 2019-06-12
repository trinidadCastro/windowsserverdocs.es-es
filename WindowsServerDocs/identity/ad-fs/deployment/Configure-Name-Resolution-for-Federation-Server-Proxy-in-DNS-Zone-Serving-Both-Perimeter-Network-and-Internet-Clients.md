---
ms.assetid: 1a6740e6-5b6d-41f8-9ec4-32cdbee3e1bb
title: Configurar la resolución de nombres para un servidor proxy de federación de una zona DNS que da servicio tanto a la red perimetral como a los clientes de Internet
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: cef87e725db7068ac4ed93524e09a25de95ec276
ms.sourcegitcommit: 9a4ab3a0d00b06ff16173aed616624c857589459
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/11/2019
ms.locfileid: "66828519"
---
# <a name="configure-name-resolution-for-a-federation-server-proxy-in-a-dns-zone-that-serves-both-the-perimeter-network-and-internet-clients"></a>Configurar la resolución de nombres para un servidor proxy de federación de una zona DNS que da servicio tanto a la red perimetral como a los clientes de Internet


Para que funcione correctamente la resolución de nombres para un servidor proxy de federación en un Active Directory Federation Services \(AD FS\) escenario en qué sistema de nombres de dominio de uno o varios \(DNS\) zonas atender tanto la red perimetral y los clientes de Internet, deben completar las tareas siguientes:  
  
-   DNS en la zona de Internet que usted controla debe estar configurado para resolver el que nombre para el servidor proxy de federación de host de todas las solicitudes de cliente de Internet para la instancia de AD FS. Para lograr esto, se agrega un host \(A\) registro de recursos a la zona DNS de Internet para el servidor proxy de federación.  
  
-   DNS en la red perimetral debe estar configurado para resolver el que nombre para el servidor de federación de host de todas las solicitudes de cliente entrantes para la instancia de AD FS. Para lograr esto, se agrega un host \(A\) registro de recursos a la zona DNS perimetral para el servidor proxy de federación.  
  
> [!NOTE]  
> Se supone que un host \(A\) registro de recursos para el servidor de federación ya se ha creado en la empresa de red DNS. Si este registro no existe todavía, cree este registro y, a continuación, realizar estos procedimientos. Para obtener más información sobre cómo crear un host \(A\) registro de recursos del servidor de federación, consulte [agrega un Host &#40;A&#41; registro de recurso al DNS corporativo para un servidor de federación](Add-a-Host--A--Resource-Record-to-Corporate-DNS-for-a-Federation-Server.md).  
  
## <a name="add-a-host-a-resource-record-to-the-internet-dns-zone-for-a-federation-server-proxy"></a>Agregar un host \(A\) registro de recursos a la zona DNS de Internet para un servidor proxy de federación  
Para que los equipos cliente en Internet pueden tener acceso correctamente a un servidor de federación a través de un servidor proxy de federación recién implementado, primero debe crear un host \(A\) registro de recursos en la zona DNS de Internet que usted controla. Este registro de recursos resuelve el nombre de host del servidor de federación de cuenta \(por ejemplo, fs.fabrikam.com\) a la dirección IP del servidor proxy de federación de cuenta \(por ejemplo, 131.107.27.68\) en el red perimetral.  
  
> [!NOTE]  
> Se supone que utiliza un servidor DNS con Windows 2000 Server, Windows Server 2003 o Windows Server 2008 con el servicio servidor DNS para controlar la zona DNS de Internet.  
  
Pertenencia a **administradores**, o equivalente, es lo mínimo necesario para completar este procedimiento.  Revise los detalles sobre el uso de las cuentas adecuadas y pertenencia a grupos en [dominio grupos predeterminados locales y](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
#### <a name="to-add-a-host-a-resource-record-to-the-internet-dns-zone-for-a-federation-server-proxy"></a>Para agregar un host \(A\) registro de recursos a la zona DNS de Internet para un servidor proxy de federación  
  
1.  En un servidor DNS para la zona DNS de Internet, abra el complemento DNS\-en.  
  
2.  En el árbol de consola, a la derecha\-haga clic en la zona de búsqueda directa correspondiente y, a continuación, haga clic en **nuevo Host \(A o AAAA\)** .  
  
3.  En **nombre**, escriba el nombre de equipo del servidor de federación. Por ejemplo, para el nombre de dominio completo \(FQDN\) fs.fabrikam.com, escriba **fs**.  
  
4.  En **dirección IP**, escriba la dirección IP para el nuevo servidor proxy de federación, por ejemplo, 131.107.27.68.  
  
5.  Haga clic en **Agregar host**.  
  
## <a name="add-a-host-a-resource-record-to-the-perimeter-dns-zone-for-a-federation-server-proxy"></a>Agregar un host \(A\) registro de recursos a la zona DNS perimetral para un servidor proxy de federación  
Para que las solicitudes de cliente de Internet pueden ser procesadas correctamente por el servidor proxy de federación y tener acceso al servidor de federación después de que se resuelven con la zona DNS de Internet, debe crear un host \(A\) registro de recursos en el perímetro Zona DNS. Este registro de recursos resuelve el nombre de host del servidor de federación de cuenta \(por ejemplo, fs. Fabrikam.com\) a la dirección IP del servidor de federación de cuenta \(por ejemplo, 192.168.1.4\) en la red corporativa.  
  
> [!NOTE]  
> Se supone que utiliza un servidor DNS con Windows 2000 Server, Windows Server 2003, Windows Server 2008 o Windows Server® 2012 con el servicio servidor DNS para controlar la zona DNS perimetral.  
  
Pertenencia a **administradores**, o equivalente, es lo mínimo necesario para completar este procedimiento.  Revise los detalles sobre el uso de las cuentas adecuadas y pertenencia a grupos en [dominio grupos predeterminados locales y](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
#### <a name="to-add-a-host-a-resource-record-to-the-perimeter-dns-zone-for-a-federation-server-proxy"></a>Para agregar un host \(A\) registro de recursos a la zona DNS perimetral para un servidor proxy de federación  
  
1.  En un servidor DNS de la red perimetral, abra el **ajuste el DNS\-en**.  
  
2.  En el árbol de consola, a la derecha\-haga clic en la zona de búsqueda directa correspondiente y, a continuación, haga clic en **nuevo Host \(A o AAAA\)** .  
  
3.  En **nombre**, escriba el nombre de equipo del servidor de federación. Por ejemplo, para el FQDN fs.fabrikam.com, escriba **fs**.  
  
4.  En el **dirección IP** cuadro de texto, escriba la dirección IP dirección para el servidor de federación en la red corporativa, por ejemplo, 192.168.1.4.  
  
5.  Haga clic en **Agregar host**.  
  
## <a name="additional-references"></a>Referencias adicionales  
[Lista de comprobación: configuración de un servidor proxy de federación](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  
[Requisitos de resolución de nombres para servidores proxy de federación](https://technet.microsoft.com/library/dd807055.aspx)  
  

