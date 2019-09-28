---
ms.assetid: 1a6740e6-5b6d-41f8-9ec4-32cdbee3e1bb
title: Configurar la resolución de nombres para un servidor proxy de federación de una zona DNS que da servicio tanto a la red perimetral como a los clientes de Internet
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 118c03ada32d3cd5b198ecd238078984a38df0db
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359834"
---
# <a name="configure-name-resolution-for-a-federation-server-proxy-in-a-dns-zone-that-serves-both-the-perimeter-network-and-internet-clients"></a>Configurar la resolución de nombres para un servidor proxy de federación de una zona DNS que da servicio tanto a la red perimetral como a los clientes de Internet


Para que la resolución de nombres funcione correctamente para un servidor proxy de Federación en un escenario Servicios de federación de Active Directory (AD FS) \(AD FS @ no__t-1 en el que una o varias zonas del sistema de nombres de dominio \(DNS @ no__t-3 sirven a la red perimetral e Internet clientes, se deben completar las siguientes tareas:  
  
-   El DNS de la zona de Internet que controla debe estar configurado para resolver todas las solicitudes de cliente de Internet para el nombre de host de AD FS en el servidor proxy de Federación. Para ello, agregue un registro de recursos de host \(A @ no__t-1 a la zona DNS de Internet para el servidor proxy de Federación.  
  
-   DNS en la red perimetral debe estar configurado para resolver todas las solicitudes de cliente entrantes para el nombre de host de AD FS en el servidor de Federación. Para ello, agregue un registro de recursos host \(A @ no__t-1 a la zona DNS perimetral para el servidor proxy de Federación.  
  
> [!NOTE]  
> Se supone que ya se ha creado un registro de recursos de host \(A @ no__t-1 para el servidor de Federación en el DNS de la red corporativa. Si este registro todavía no existe, cree este registro y, a continuación, realice estos procedimientos. Para obtener más información acerca de cómo crear un registro de recursos de host \(A @ no__t-1 para el servidor de Federación, consulte [Agregar un registro de &#40;&#41; recursos de host a a DNS corporativo para un servidor de Federación](Add-a-Host--A--Resource-Record-to-Corporate-DNS-for-a-Federation-Server.md).  
  
## <a name="add-a-host-a-resource-record-to-the-internet-dns-zone-for-a-federation-server-proxy"></a>Agregar un registro de recursos de host \(A @ no__t-1 a la zona DNS de Internet para un servidor proxy de Federación  
Para que los equipos cliente de Internet puedan tener acceso correctamente a un servidor de Federación a través de un proxy de servidor de Federación recién implementado, primero debe crear un registro de recursos de host \(A @ no__t-1 en la zona DNS de Internet que usted controla. Este registro de recursos resuelve el nombre de host del servidor de Federación de la cuenta \(for ejemplo, FS. fabrikam. com @ no__t-1 en la dirección IP del proxy de servidor de Federación de cuenta \(For ejemplo, 131.107.27.68 @ no__t-3 en la red perimetral.  
  
> [!NOTE]  
> Se supone que usa un servidor DNS que ejecuta Windows 2000 Server, Windows Server 2003 o Windows Server 2008 con el servicio servidor DNS para controlar la zona DNS de Internet.  
  
La pertenencia al grupo **administradores**, o equivalente, es lo mínimo necesario para completar este procedimiento.  Revise los detalles sobre el uso de las cuentas y pertenencias a grupos adecuadas en [grupos predeterminados locales y de dominio](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
#### <a name="to-add-a-host-a-resource-record-to-the-internet-dns-zone-for-a-federation-server-proxy"></a>Para agregar un registro de recursos de host \(A @ no__t-1 a la zona DNS de Internet para un servidor proxy de Federación  
  
1.  En un servidor DNS para la zona DNS de Internet, abra el paso de DNS "no__t-0in".  
  
2.  En el árbol de consola, haga clic en @ no__t-0click la zona de búsqueda directa aplicable y, a continuación, haga clic en **nuevo Host \(A o AAAA @ no__t-3**.  
  
3.  En **nombre**, escriba solo el nombre de equipo del servidor de Federación. Por ejemplo, para el nombre de dominio completo \(FQDN @ no__t-1 fs.fabrikam.com, escriba **FS**.  
  
4.  En **dirección IP**, escriba la dirección IP del nuevo servidor proxy de Federación, por ejemplo, 131.107.27.68.  
  
5.  Haga clic en **Agregar host**.  
  
## <a name="add-a-host-a-resource-record-to-the-perimeter-dns-zone-for-a-federation-server-proxy"></a>Agregar un registro de recursos de host \(A @ no__t-1 a la zona DNS perimetral para un servidor proxy de Federación  
Para que el servidor proxy de Federación pueda procesar correctamente las solicitudes de cliente de Internet y llegar al servidor de Federación después de que se resuelvan mediante la zona DNS de Internet, debe crear un registro de recursos de host \(A @ no__t-1 en la zona DNS perimetral. Este registro de recursos resuelve el nombre de host del servidor de Federación de la cuenta \(for ejemplo, FS. fabrikam. com @ no__t-0 a la dirección IP del servidor de Federación de cuenta \(Para example, 192.168.1.4 @ no__t-2 en la red corporativa.  
  
> [!NOTE]  
> Se supone que usa un servidor DNS que ejecuta Windows 2000 Server, Windows Server 2003, Windows Server 2008 o Windows Server® 2012 con el servicio servidor DNS para controlar la zona DNS perimetral.  
  
La pertenencia al grupo **administradores**, o equivalente, es lo mínimo necesario para completar este procedimiento.  Revise los detalles sobre el uso de las cuentas y pertenencias a grupos adecuadas en [grupos predeterminados locales y de dominio](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
#### <a name="to-add-a-host-a-resource-record-to-the-perimeter-dns-zone-for-a-federation-server-proxy"></a>Para agregar un registro de recursos de host \(A @ no__t-1 a la zona DNS perimetral para un servidor proxy de Federación  
  
1.  En un servidor DNS para la red perimetral, abra el **DNS Snap @ no__t-1in**.  
  
2.  En el árbol de consola, haga clic en @ no__t-0click la zona de búsqueda directa aplicable y, a continuación, haga clic en **nuevo Host \(A o AAAA @ no__t-3**.  
  
3.  En **nombre**, escriba solo el nombre de equipo del servidor de Federación. Por ejemplo, para el FQDN fs.fabrikam.com, escriba **fs**.  
  
4.  En el cuadro de texto **dirección IP** , escriba la dirección IP del servidor de Federación en la red corporativa, por ejemplo, 192.168.1.4.  
  
5.  Haga clic en **Agregar host**.  
  
## <a name="additional-references"></a>Referencias adicionales  
[Lista de comprobación: configuración de un servidor proxy de federación](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  
[Requisitos de resolución de nombres para servidores proxy de federación](https://technet.microsoft.com/library/dd807055.aspx)  
  

