---
ms.assetid: b7109e46-b66e-4c5c-8b87-a6611d68415a
title: "Configurar la resolución de nombre de un servidor Proxy de servidor de federación en una zona de DNS que sirve solo la red perimetral"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 32b8e3cc133ce95872881115608bb8cfb17b2427
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="configure-name-resolution-for-a-federation-server-proxy-in-a-dns-zone-that-serves-only-the-perimeter-network"></a>Configurar la resolución de nombre de un servidor Proxy de servidor de federación en una zona de DNS que sirve solo la red perimetral

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Para que funcione correctamente la resolución de nombres de un servidor de federación en un escenario de \(AD FS\) de los servicios de federación de Active Directory en el que una o varias zonas del sistema de nombres de dominio \(DNS\) sirven solo la red perimetral, se deben completar las siguientes tareas:  
  
-   El archivo de hosts en el proxy de servidor de federación debe actualizarse para agregar la dirección IP de un servidor de federación.  
  
-   DNS en la red perimetral debe configurarse para resolver el que nombre para el proxy de servidor de federación de host de todas las solicitudes de cliente de la AD FS. Para ello, agrega un registro de host \(A\) recursos al perímetro DNS para que el proxy de servidor de federación.  
  
> [!NOTE]  
> Estos procedimientos suponen que ya ha creado un registro de host \(A\) recursos del servidor de federación de la red corporativa DNS. Si todavía no existe este registro, crea este registro y, a continuación, realizar estos procedimientos. Para obtener más información sobre cómo crear el registro de host \(A\) recursos del servidor de federación, consulta [agregar un Host & #40; Un & #41; Registro de recursos corporativos DNS para un servidor de federación](Add-a-Host--A--Resource-Record-to-Corporate-DNS-for-a-Federation-Server.md).  
  
## <a name="add-the-ip-address-of-a-federation-server-to-the-hosts-file"></a>Agrega la dirección IP de un servidor de federación al archivo de hosts  
Para que un proxy de servidor de federación funcionen según lo esperado en la red perimetral de un asociado de cuenta, debes agregar una entrada para el archivo de hosts en ese proxy del servidor de federación que apunta al nombre de host DNS de un servidor de federación \ (por ejemplo, fs.fabrikam.com\) y la dirección IP \ (por ejemplo, 192.168.1.4\) en la red corporativa de asociado de la cuenta. Agregar esta entrada en el archivo de hosts, impide que el proxy de servidor de federación ponerse en contacto con para resolver una llamada iniciadas por client\ en un servidor de federación de asociado de la cuenta.  
  
Pertenencia a **administradores**, o equivalente, en el equipo local es lo mínimo necesario para completar este procedimiento.  Revisar detalles sobre el uso de las cuentas adecuadas y agrupar pertenencias a [Local y dominio predeterminada grupos](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
#### <a name="to-add-the-ip-address-of-a-federation-server-to-the-hosts-file"></a>Para agregar la dirección IP de un servidor de federación para el archivo de hosts  
  
1.  Navega hasta la carpeta de directorio %systemroot%\\Winnt\\System32\\Drivers y busca el **hosts** archivo.  
  
2.  Inicia el Bloc de notas y, a continuación, abre el **hosts** archivo.  
  
3.  Agregar la dirección IP y el nombre de host de un servidor de federación de asociado de cuenta a la **hosts** del archivo, como se muestra en el siguiente ejemplo:  
  
    **192.168.1.4fs.fabrikam.com**  
  
4.  Guarda y cierra el archivo.  
  
## <a name="add-a-host-a-resource-record-to-perimeter-dns-for-a-federation-server-proxy"></a>Agregar un registro de host \(A\) recursos a perímetro DNS para un proxy de federación de servidor  
Para que los clientes en Internet pueden obtener acceso a un servidor de federación a través de un proxy de servidor de federación recientemente implementado, primero debes crear un registro de host \(A\) recursos en el perímetro de DNS. Este resuelve el nombre de host del servidor de federación de cuenta \ (por ejemplo, fs.fabrikam.com\) a la dirección IP de proxy del servidor de federación de cuenta \ (por ejemplo, 131.107.27.68\) en la red perimetral.  
  
> [!NOTE]  
> Se supone que estás usando un servidor DNS, ejecuta Windows 2000 Server, Windows Server 2003 o Windows Server 2008 con el servicio de servidor DNS, para controlar la zona DNS de perímetro.  
  
Pertenencia a **administradores**, o equivalente, es lo mínimo necesario para completar este procedimiento.  Revisar detalles sobre el uso de las cuentas adecuadas y agrupar pertenencias a [Local y dominio predeterminada grupos](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
#### <a name="to-add-a-host-a-resource-record-to-perimeter-dns-for-a-federation-server-proxy"></a>Para agregar un registro de host \(A\) recursos al perímetro DNS de un proxy de federación de servidor  
  
1.  En un servidor DNS para la red perimetral, abre el DNS en snap\. Haz clic en **inicio**, elija **herramientas administrativas**y, a continuación, haz clic en **DNS**.  
  
2.  En la consola de árbol, right\ y haga clic en la zona de búsqueda directa correspondiente y, a continuación, haz clic en **nuevo Host \(A or AAAA\)**.  
  
3.  En **nombre**, escribe el nombre de equipo del servidor de federación. Por ejemplo, para la fs.fabrikam.com \(FQDN\) del nombre de dominio completo, escriba **fs**.  
  
4.  En **dirección IP**, escribe la dirección IP para el proxy de servidor de federación nuevo, por ejemplo, **131.107.27.68**.  
  
5.  Haz clic en **agregar Host**.  
  
## <a name="additional-references"></a>Referencias adicionales  
[Lista de comprobación: Configurar un servidor Proxy Server federación](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  
[Requisitos de resolución de nombre de Proxies de servidor de federación](https://technet.microsoft.com/library/dd807055.aspx)  
  

