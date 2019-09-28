---
ms.assetid: b7109e46-b66e-4c5c-8b87-a6611d68415a
title: Configurar la resolución de nombres para un servidor proxy de federación de una zona DNS que da servicio solo a la red perimetral
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: de4627f2e03e6432f4e678cd9ca932819cb483d5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408439"
---
# <a name="configure-name-resolution-for-a-federation-server-proxy-in-a-dns-zone-that-serves-only-the-perimeter-network"></a>Configurar la resolución de nombres para un servidor proxy de federación de una zona DNS que da servicio solo a la red perimetral


Para que la resolución de nombres pueda funcionar correctamente en un servidor de Federación en un escenario Servicios de federación de Active Directory (AD FS) \(AD FS @ no__t-1 en el que una o varias zonas del sistema de nombres de dominio \(DNS @ no__t-3 solo sirven para la red perimetral, lo siguiente las tareas deben completarse:  
  
-   El archivo de hosts del servidor proxy de Federación debe actualizarse para agregar la dirección IP de un servidor de Federación.  
  
-   DNS en la red perimetral debe estar configurado para resolver todas las solicitudes de cliente para el nombre de host de AD FS en el servidor proxy de Federación. Para ello, agregue un registro de recursos de host \(A @ no__t-1 al DNS perimetral para el servidor proxy de Federación.  
  
> [!NOTE]  
> En estos procedimientos se supone que ya se ha creado un registro de recursos de host \(A @ no__t-1 para el servidor de Federación en el DNS de la red corporativa. Si este registro todavía no existe, cree este registro y, a continuación, realice estos procedimientos. Para obtener más información sobre cómo crear el registro de recursos de host \(A @ no__t-1 para el servidor de Federación, consulte [ &#40;agregar&#41; un registro de recursos de host a a DNS corporativo para un servidor de Federación](Add-a-Host--A--Resource-Record-to-Corporate-DNS-for-a-Federation-Server.md).  
  
## <a name="add-the-ip-address-of-a-federation-server-to-the-hosts-file"></a>Agregar la dirección IP de un servidor de Federación al archivo de hosts  
Para que un servidor proxy de Federación pueda funcionar según lo esperado en la red perimetral de un asociado de cuenta, debe agregar una entrada al archivo de hosts en ese servidor proxy de Federación que apunte al nombre de host DNS de un servidor de Federación @no__t 0for ejemplo, FS. fabrikam. com @ no__t-1 y dirección IP \(For ejemplo, 192.168.1.4 @ no__t-3 en la red corporativa del asociado de cuenta. Al agregar esta entrada al archivo de hosts, se impide que el servidor proxy de Federación se ponga en contacto con él mismo para resolver una llamada de cliente @ no__t-0initiated a un servidor de Federación en el asociado de cuenta.  
  
El requisito mínimo para realizar este procedimiento es pertenecer al grupo **Administradores** o un grupo equivalente en el equipo local.  Revise los detalles sobre el uso de las cuentas y pertenencias a grupos adecuadas en [grupos predeterminados locales y de dominio](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
#### <a name="to-add-the-ip-address-of-a-federation-server-to-the-hosts-file"></a>Para agregar la dirección IP de un servidor de Federación al archivo de hosts  
  
1.  Navegue hasta la carpeta% SystemRoot% \\Winnt @ no__t-1System32 @ no__t-2Drivers y busque el archivo de **hosts** .  
  
2.  Inicie el Bloc de notas y, a continuación, abra el archivo de **hosts**.  
  
3.  Agregue la dirección IP y el nombre de host de un servidor de Federación del asociado de cuenta al archivo de **hosts** , tal como se muestra en el ejemplo siguiente:  
  
    **192.168.1.4fs.fabrikam.com**  
  
4.  Guarde y cierre el archivo.  
  
## <a name="add-a-host-a-resource-record-to-perimeter-dns-for-a-federation-server-proxy"></a>Agregar un registro de recursos de host \(A @ no__t-1 al DNS perimetral para un servidor proxy de Federación  
Para que los clientes de Internet puedan acceder correctamente a un servidor de Federación a través de un proxy de servidor de Federación recién implementado, primero debe crear un registro de recursos de host \(A @ no__t-1 en el DNS perimetral. Este registro de recursos resuelve el nombre de host del servidor de Federación de la cuenta \(for ejemplo, FS. fabrikam. com @ no__t-1 en la dirección IP del proxy de servidor de Federación de cuenta \(For ejemplo, 131.107.27.68 @ no__t-3 en la red perimetral.  
  
> [!NOTE]  
> Se supone que está utilizando un servidor DNS que ejecuta Windows 2000 Server, Windows Server 2003 o Windows Server 2008 con el servicio servidor DNS, para controlar la zona DNS perimetral.  
  
La pertenencia al grupo **administradores**, o equivalente, es lo mínimo necesario para completar este procedimiento.  Revise los detalles sobre el uso de las cuentas y pertenencias a grupos adecuadas en [grupos predeterminados locales y de dominio](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
#### <a name="to-add-a-host-a-resource-record-to-perimeter-dns-for-a-federation-server-proxy"></a>Para agregar un registro de recursos de host \(A @ no__t-1 al DNS perimetral para un servidor proxy de Federación  
  
1.  En un servidor DNS para la red perimetral, abra el DNS Snap @ no__t-0in. Haga clic en **Inicio**, seleccione **herramientas administrativas**y, a continuación, haga clic en **DNS**.  
  
2.  En el árbol de consola, haga clic en @ no__t-0click la zona de búsqueda directa aplicable y, a continuación, haga clic en **nuevo Host \(A o AAAA @ no__t-3**.  
  
3.  En **nombre**, escriba solo el nombre de equipo del servidor de Federación. Por ejemplo, para el nombre de dominio completo \(FQDN @ no__t-1 fs.fabrikam.com, escriba **FS**.  
  
4.  En **dirección IP**, escriba la dirección IP del nuevo servidor proxy de Federación, por ejemplo, **131.107.27.68**.  
  
5.  Haga clic en **Agregar host**.  
  
## <a name="additional-references"></a>Referencias adicionales  
[Lista de comprobación: configuración de un servidor proxy de federación](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  
[Requisitos de resolución de nombres para servidores proxy de federación](https://technet.microsoft.com/library/dd807055.aspx)  
  

