---
description: Más información acerca de cómo configurar la resolución de nombres para un servidor proxy de Federación en una zona DNS que solo sirve a la red perimetral
ms.assetid: b7109e46-b66e-4c5c-8b87-a6611d68415a
title: Configurar la resolución de nombres para un servidor proxy de federación de una zona DNS que da servicio solo a la red perimetral
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.author: billmath
ms.openlocfilehash: 186ee4df7f1ca8df3ed7aa91da5963e268e06097
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97050283"
---
# <a name="configure-name-resolution-for-a-federation-server-proxy-in-a-dns-zone-that-serves-only-the-perimeter-network"></a>Configurar la resolución de nombres para un servidor proxy de federación de una zona DNS que da servicio solo a la red perimetral


Para que la resolución de nombres funcione correctamente en un servidor de Federación en una Servicios de federación de Active Directory (AD FS) \( AD FS \) escenario en el que una o varias zonas DNS del sistema de nombres de dominio \( \) solo sirven a la red perimetral, se deben completar las siguientes tareas:

-   El archivo de hosts del servidor proxy de Federación debe actualizarse para agregar la dirección IP de un servidor de Federación.

-   DNS en la red perimetral debe estar configurado para resolver todas las solicitudes de cliente para el nombre de host de AD FS en el servidor proxy de Federación. Para ello, agregue un registro de recurso de host \( \) a al DNS perimetral para el servidor proxy de Federación.

> [!NOTE]
> En estos procedimientos se supone que \( \) ya se ha creado un registro de recursos de host para el servidor de Federación en el DNS de la red corporativa. Si este registro todavía no existe, cree este registro y, a continuación, realice estos procedimientos. Para obtener más información acerca de cómo crear el \( \) registro de recursos de host a para el servidor de Federación, consulte [agregar un host &#40;un registro de recursos de&#41; al DNS corporativo para un servidor de Federación](Add-a-Host--A--Resource-Record-to-Corporate-DNS-for-a-Federation-Server.md).

## <a name="add-the-ip-address-of-a-federation-server-to-the-hosts-file"></a>Agregar la dirección IP de un servidor de Federación al archivo de hosts
Para que un servidor proxy de Federación pueda funcionar según lo previsto en la red perimetral de un asociado de cuenta, debe agregar una entrada al archivo de hosts en ese servidor proxy de Federación que apunte al nombre de host DNS de un servidor de Federación, \( por ejemplo, FS.fabrikam.com \) y dirección IP \( , por ejemplo, 192.168.1.4 \) en la red corporativa del asociado de cuenta. Al agregar esta entrada al archivo de hosts, se impide que el servidor proxy de Federación se ponga en contacto con él mismo para resolver una \- llamada iniciada por el cliente a un servidor de Federación en el asociado de cuenta.

La pertenencia al grupo **Administradores** o equivalente en el equipo local es el requisito mínimo necesario para completar este procedimiento.  Revise los detalles sobre el uso de las cuentas y pertenencias a grupos adecuadas en [grupos predeterminados locales y de dominio](https://go.microsoft.com/fwlink/?LinkId=83477).

#### <a name="to-add-the-ip-address-of-a-federation-server-to-the-hosts-file"></a>Para agregar la dirección IP de un servidor de Federación al archivo de hosts

1.  Navegue hasta la carpeta% SystemRoot% \\ WinNT \\ system32 \\ drivers Directory y busque el archivo **hosts** .

2.  Inicia el Bloc de notas y abre el archivo **hosts**.

3.  Agregue la dirección IP y el nombre de host de un servidor de Federación del asociado de cuenta al archivo de **hosts** , tal como se muestra en el ejemplo siguiente:

    **192.168.1.4fs.fabrikam.com**

4.  Guarde y cierre el archivo.

## <a name="add-a-host-a-resource-record-to-perimeter-dns-for-a-federation-server-proxy"></a>Agregar un registro de recurso de host \( \) a al DNS perimetral para un servidor proxy de Federación
Para que los clientes de Internet puedan tener acceso correctamente a un servidor de Federación a través de un proxy de servidor de Federación recién implementado, primero debe crear un registro de recurso de host \( a \) en el DNS del perímetro. Este registro de recursos resuelve el nombre de host del servidor de Federación de la cuenta \( , por ejemplo, FS.fabrikam.com \) en la dirección IP del servidor proxy de Federación de la cuenta \( , por ejemplo, 131.107.27.68 \) en la red perimetral.

> [!NOTE]
> Se supone que está utilizando un servidor DNS que ejecuta Windows 2000 Server, Windows Server 2003 o Windows Server 2008 con el servicio servidor DNS, para controlar la zona DNS perimetral.

La pertenencia al grupo **administradores**, o equivalente, es lo mínimo necesario para completar este procedimiento.  Revise los detalles sobre el uso de las cuentas y pertenencias a grupos adecuadas en [grupos predeterminados locales y de dominio](https://go.microsoft.com/fwlink/?LinkId=83477).

#### <a name="to-add-a-host-a-resource-record-to-perimeter-dns-for-a-federation-server-proxy"></a>Para agregar un registro de recurso de host \( \) a al DNS perimetral para un servidor proxy de Federación

1.  En un servidor DNS para la red perimetral, abra el complemento DNS \- . Haga clic en **Inicio**, seleccione **herramientas administrativas** y, a continuación, haga clic en **DNS**.

2.  En el árbol de consola, \- haga clic con el botón secundario en la zona de búsqueda directa aplicable y, a continuación, haga clic en **nuevo host \( a o AAAA \)**.

3.  En **nombre**, escriba solo el nombre de equipo del servidor de Federación. Por ejemplo, para el nombre de dominio completo \( FQDN \) FS.fabrikam.com, escriba **FS**.

4.  En **dirección IP**, escriba la dirección IP del nuevo servidor proxy de Federación, por ejemplo, **131.107.27.68**.

5.  Haz clic en **Agregar host**.

## <a name="additional-references"></a>Referencias adicionales
[Lista de comprobación: configuración de un servidor proxy de federación](Checklist--Setting-Up-a-Federation-Server-Proxy.md)

[Requisitos de resolución de nombres para servidores proxy de federación](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dd807055(v=ws.11))

