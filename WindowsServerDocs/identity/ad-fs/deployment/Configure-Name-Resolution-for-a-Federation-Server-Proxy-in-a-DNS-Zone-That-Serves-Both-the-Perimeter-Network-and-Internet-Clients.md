---
ms.assetid: 1a6740e6-5b6d-41f8-9ec4-32cdbee3e1bb
title: "Configurar la resolución de nombres para la zona un Proxy de servidor de federación de DNS que sirven tanto el perímetro de la red y los clientes de Internet"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 3b154459163b2142ff1d3aba424a86305d093de4
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="configure-name-resolution-for-a-federation-server-proxy-in-a-dns-zone-that-serves-both-the-perimeter-network-and-internet-clients"></a>Configurar la resolución de nombres para la zona un Proxy de servidor de federación de DNS que sirven tanto el perímetro de la red y los clientes de Internet

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Por lo que la resolución de nombres puede funciona correctamente con un proxy de federación de servidor en un escenario de \(AD FS\) de los servicios de federación de Active Directory en el que una o varias zonas del sistema de nombres de dominio \(DNS\) servir tanto la red perimetral y los clientes de Internet, se deben completar las siguientes tareas:  
  
-   DNS en la zona de Internet que tú controlas debe configurarse para resolver el que nombre para el proxy de servidor de federación de host de todas las solicitudes de cliente de Internet de la AD FS. Para ello, agrega un registro de host \(A\) recursos a la zona de Internet DNS para que el proxy de servidor de federación.  
  
-   DNS en la red perimetral debe configurarse para resolver el que nombre al servidor de federación de host de todas las solicitudes entrantes de cliente de la AD FS. Para ello, agrega un registro de host \(A\) recursos a la zona DNS perímetro para que el proxy de servidor de federación.  
  
> [!NOTE]  
> Se supone que ya ha creado un registro de host \(A\) recursos del servidor de federación de la red corporativa DNS. Si todavía no existe este registro, crea este registro y, a continuación, realizar estos procedimientos. Para obtener más información sobre cómo crear un registro de host \(A\) recursos del servidor de federación, consulta [agregar un Host & #40; Un & #41; Registro de recursos corporativos DNS para un servidor de federación](Add-a-Host--A--Resource-Record-to-Corporate-DNS-for-a-Federation-Server.md).  
  
## <a name="add-a-host-a-resource-record-to-the-internet-dns-zone-for-a-federation-server-proxy"></a>Agregar un registro de host \(A\) recursos a la zona de Internet DNS para un proxy de servidor de federación  
Para que los equipos cliente en Internet pueden obtener acceso a un servidor de federación a través de un proxy de servidor de federación recientemente implementado, primero debes crear un registro de host \(A\) recursos en la zona de Internet DNS que tú controlas. Este resuelve el nombre de host del servidor de federación de cuenta \ (por ejemplo, fs.fabrikam.com\) a la dirección IP de proxy del servidor de federación de cuenta \ (por ejemplo, 131.107.27.68\) en la red perimetral.  
  
> [!NOTE]  
> Se supone que estás usando un servidor DNS que ejecuta Windows 2000 Server, Windows Server 2003 o Windows Server 2008 con el servicio de servidor DNS para controlar la zona de Internet DNS.  
  
Pertenencia a **administradores**, o equivalente, es lo mínimo necesario para completar este procedimiento.  Revisar detalles sobre el uso de las cuentas adecuadas y agrupar pertenencias a [Local y dominio predeterminada grupos](http://go.microsoft.com/fwlink/?LinkId=83477).   
  
#### <a name="to-add-a-host-a-resource-record-to-the-internet-dns-zone-for-a-federation-server-proxy"></a>Para agregar un registro de host \(A\) recursos a la zona de Internet DNS para un proxy de federación de servidor  
  
1.  En un servidor DNS para la zona Internet DNS, abre el DNS en snap\.  
  
2.  En la consola de árbol, right\ y haga clic en la zona de búsqueda directa correspondiente y, a continuación, haz clic en **nuevo Host \(A or AAAA\)**.  
  
3.  En **nombre**, escribe el nombre de equipo del servidor de federación. Por ejemplo, para la fs.fabrikam.com \(FQDN\) del nombre de dominio completo, escriba **fs**.  
  
4.  En **dirección IP**, escribe la dirección IP para el proxy de servidor de federación nuevo, por ejemplo, 131.107.27.68.  
  
5.  Haz clic en **agregar Host**.  
  
## <a name="add-a-host-a-resource-record-to-the-perimeter-dns-zone-for-a-federation-server-proxy"></a>Agregar un registro de host \(A\) recursos a la zona DNS perímetro para un proxy de federación de servidor  
Para que las solicitudes de cliente de Internet pueden ser procesadas correctamente por el proxy de servidor de federación y alcanzar el servidor de federación después de que se resuelven por la zona de Internet DNS, debes crear un registro de host \(A\) recursos en la zona DNS de perímetro. Este resuelve el nombre de host del servidor de federación de cuenta \ (por ejemplo, fs. Fabrikam.com\) a la dirección IP del servidor de federación de cuenta \ (por ejemplo, 192.168.1.4\) en la red corporativa.  
  
> [!NOTE]  
> Se supone que estás usando un servidor DNS que ejecuta Windows 2000 Server, Windows Server 2003, Windows Server 2008 o Windows Server® 2012 con el servicio de servidor DNS para controlar la zona DNS de perímetro.  
  
Pertenencia a **administradores**, o equivalente, es lo mínimo necesario para completar este procedimiento.  Revisar detalles sobre el uso de las cuentas adecuadas y agrupar pertenencias a [Local y dominio predeterminada grupos](http://go.microsoft.com/fwlink/?LinkId=83477).   
  
#### <a name="to-add-a-host-a-resource-record-to-the-perimeter-dns-zone-for-a-federation-server-proxy"></a>Para agregar un registro de host \(A\) recursos a la zona DNS perímetro para un proxy de federación de servidor  
  
1.  En un servidor DNS para la red perimetral, abre el **DNS en snap\**.  
  
2.  En la consola de árbol, right\ y haga clic en la zona de búsqueda directa correspondiente y, a continuación, haz clic en **nuevo Host \(A or AAAA\)**.  
  
3.  En **nombre**, escribe el nombre de equipo del servidor de federación. Por ejemplo, para el FQDN fs.fabrikam.com, escribe **fs**.  
  
4.  En la **dirección IP** cuadro de texto, escribe la dirección IP la dirección del servidor de federación de la red corporativa, por ejemplo, 192.168.1.4.  
  
5.  Haz clic en **agregar Host**.  
  
## <a name="additional-references"></a>Referencias adicionales  
[Lista de comprobación: Configurar un servidor Proxy Server federación](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  
[Requisitos de resolución de nombre de Proxies de servidor de federación](https://technet.microsoft.com/library/dd807055.aspx)  
  

