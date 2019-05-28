---
ms.assetid: b7109e46-b66e-4c5c-8b87-a6611d68415a
title: Configurar la resolución de nombres para un servidor proxy de federación de una zona DNS que da servicio solo a la red perimetral
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 7d046c720c5c6250b6efa03e068aa66e2a6bbe3d
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192298"
---
# <a name="configure-name-resolution-for-a-federation-server-proxy-in-a-dns-zone-that-serves-only-the-perimeter-network"></a>Configurar la resolución de nombres para un servidor proxy de federación de una zona DNS que da servicio solo a la red perimetral


Para que funcione correctamente la resolución de nombres para un servidor de federación en un Active Directory Federation Services \(AD FS\) escenario en qué sistema de nombres de dominio de uno o varios \(DNS\) zonas actúan sólo en el perímetro de red, los siguientes tareas deben completarse:  
  
-   El archivo de hosts en el servidor proxy de federación debe actualizarse para agregar la dirección IP de un servidor de federación.  
  
-   DNS en la red perimetral debe estar configurado para resolver el que nombre para el servidor proxy de federación de host de todas las solicitudes de cliente de AD FS. Para ello, se agrega un host \(A\) registro de recursos DNS perimetral para el servidor proxy de federación.  
  
> [!NOTE]  
> Estos procedimientos supone que un host \(A\) registro de recursos para el servidor de federación ya se ha creado en la empresa de red DNS. Si este registro no existe todavía, cree este registro y, a continuación, realizar estos procedimientos. Para obtener más información sobre cómo crear el host \(A\) registro de recursos del servidor de federación, consulte [agrega un Host &#40;A&#41; registro de recurso al DNS corporativo para un servidor de federación](Add-a-Host--A--Resource-Record-to-Corporate-DNS-for-a-Federation-Server.md).  
  
## <a name="add-the-ip-address-of-a-federation-server-to-the-hosts-file"></a>Agregue la dirección IP de un servidor de federación para el archivo de hosts  
Para que un servidor proxy de federación funcione según lo previsto en la red perimetral de un asociado de cuenta, debe agregar una entrada al archivo hosts en ese servidor proxy de federación que señala al nombre de host DNS de un servidor de federación \(por ejemplo, fs.fabrikam.com \) y la dirección IP \(por ejemplo, 192.168.1.4\) en la red corporativa del asociado de cuenta. Al agregar esta entrada al archivo hosts impide que el servidor proxy de federación de ponerse en contacto con sí mismo para resolver un cliente\-inició la llamada a un servidor de federación del asociado de cuenta.  
  
El requisito mínimo para realizar este procedimiento es pertenecer al grupo **Administradores** o un grupo equivalente en el equipo local.  Revise los detalles sobre el uso de las cuentas adecuadas y pertenencia a grupos en [dominio grupos predeterminados locales y](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
#### <a name="to-add-the-ip-address-of-a-federation-server-to-the-hosts-file"></a>Para agregar la dirección IP de un servidor de federación para el archivo de hosts  
  
1.  Vaya a % systemroot %\\Winnt\\System32\\carpeta del directorio de controladores y busque el **hosts** archivo.  
  
2.  Inicie el Bloc de notas y, a continuación, abra el archivo de **hosts**.  
  
3.  Agregue la dirección IP y el nombre de host de un servidor de federación del asociado de cuenta para el **hosts** de archivos, como se muestra en el ejemplo siguiente:  
  
    **192.168.1.4fs.fabrikam.com**  
  
4.  Guarde y cierre el archivo.  
  
## <a name="add-a-host-a-resource-record-to-perimeter-dns-for-a-federation-server-proxy"></a>Agregar un host \(A\) registro de recursos DNS perimetral para un servidor proxy de federación  
Para que los clientes en Internet pueden tener acceso correctamente a un servidor de federación a través de un servidor proxy de federación recién implementado, primero debe crear un host \(A\) registro de recursos en el DNS perimetral. Este registro de recursos resuelve el nombre de host del servidor de federación de cuenta \(por ejemplo, fs.fabrikam.com\) a la dirección IP del servidor proxy de federación de cuenta \(por ejemplo, 131.107.27.68\) en el red perimetral.  
  
> [!NOTE]  
> Se supone que utiliza un servidor DNS, ejecute Windows 2000 Server, Windows Server 2003 o Windows Server 2008 con el servicio servidor DNS, para controlar la zona DNS perimetral.  
  
Pertenencia a **administradores**, o equivalente, es lo mínimo necesario para completar este procedimiento.  Revise los detalles sobre el uso de las cuentas adecuadas y pertenencia a grupos en [dominio grupos predeterminados locales y](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
#### <a name="to-add-a-host-a-resource-record-to-perimeter-dns-for-a-federation-server-proxy"></a>Para agregar un host \(A\) registro de recursos DNS perimetral para un servidor proxy de federación  
  
1.  En un servidor DNS de la red perimetral, abra el complemento DNS\-en. Haga clic en **iniciar**, apunte a **herramientas administrativas**y, a continuación, haga clic en **DNS**.  
  
2.  En el árbol de consola, a la derecha\-haga clic en la zona de búsqueda directa correspondiente y, a continuación, haga clic en **nuevo Host \(A o AAAA\)** .  
  
3.  En **nombre**, escriba el nombre de equipo del servidor de federación. Por ejemplo, para el nombre de dominio completo \(FQDN\) fs.fabrikam.com, escriba **fs**.  
  
4.  En **dirección IP**, escriba la dirección IP para el nuevo servidor proxy de federación, por ejemplo, **131.107.27.68**.  
  
5.  Haga clic en **Agregar host**.  
  
## <a name="additional-references"></a>Referencias adicionales  
[Lista de comprobación: configuración de un servidor proxy de federación](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  
[Requisitos de resolución de nombres para servidores proxy de federación](https://technet.microsoft.com/library/dd807055.aspx)  
  

