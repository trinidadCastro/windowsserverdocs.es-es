---
title: 'Escritorio remoto: permitir el acceso a su PC'
description: Obtenga información acerca de las opciones de acceso remoto a su PC
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0f1557ed-53f7-4333-b023-c8e0f4b58bf4
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 06/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: d03dcd307696aea55ab6a1569ab907635994772a
ms.sourcegitcommit: d888e35f71801c1935620f38699dda11db7f7aad
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/07/2019
ms.locfileid: "66804990"
---
# <a name="remote-desktop---allow-access-to-your-pc"></a>Escritorio remoto: permitir el acceso a su PC

>Se aplica a: Windows 10, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2

Puede usar Escritorio remoto para conectarse y controlar su equipo desde un dispositivo remoto mediante el uso de un [cliente de escritorio remoto de Microsoft](remote-desktop-clients.md) (disponible para Windows, iOS, macOS y Android). Al permitir conexiones remotas a su equipo, puede usar otro dispositivo para conectarse a su equipo y tener acceso a todas sus aplicaciones, archivos y recursos de red como si estuviera sentado delante de su escritorio.  

> [!NOTE]
> Puede usar Escritorio remoto para conectarse a Windows 10 Pro y Enterprise, Windows 8.1 y 8 Enterprise y Pro, Windows 7 Professional, Enterprise y Ultimate y Windows Server las versiones más recientes que Windows Server 2008. No se puede conectar a equipos que ejecutan la edición de la página principal (por ejemplo, Windows 10 Home). 

Para conectarse a un equipo remoto, que debe estar activado el equipo, debe tener una conexión de red, debe estar habilitado Escritorio remoto, debe tener acceso a la red al equipo remoto (Esto puede ser a través de Internet), y debe tener permiso para conectarse. Para que obtener permiso para conectarse, debe ser en la lista de usuarios. Antes de iniciar una conexión, es una buena idea para buscar el nombre del equipo a que se conecta y para asegurarse de que se permiten las conexiones de escritorio remoto a través de su firewall.

## <a name="how-to-enable-remote-desktop"></a>Cómo habilitar Escritorio remoto

La manera más sencilla para permitir el acceso a su equipo desde un dispositivo remoto está usando las opciones de escritorio remoto en la configuración. Puesto que esta funcionalidad se ha agregado de Windows 10 Fall Creators update (1709), otro descargable también está disponible la aplicación proporciona una funcionalidad similar para las versiones anteriores de Windows. También puede usar el modo heredado de la habilitación de escritorio remoto, sin embargo, este método proporciona menos funcionalidad y la validación.

### <a name="windows-10-fall-creator-update-1709-or-later"></a>Windows 10 Fall Creator Update (1709) o posterior

Puede configurar su equipo para el acceso remoto con unos pocos pasos sencillos.
1. En el dispositivo que desea conectarse, seleccione **iniciar** y haga clic en el **configuración** situado a la izquierda.
2. Seleccione el **sistema** grupo seguido por el [ **escritorio remoto** ](ms-settings:remotedesktop) elemento.
3. Use el control deslizante para habilitar Escritorio remoto.
4. También se recomienda mantener el equipo activo y reconocible para facilitar las conexiones. Haga clic en **Mostrar configuración** a habilitar.
5. Según sea necesario, agregue los usuarios que pueden conectarse de forma remota, haga clic en **seleccione los usuarios que pueden obtener acceso remoto a este equipo**.
   1. Los miembros del grupo de administradores automáticamente tienen acceso.
6. Tome nota del nombre de este equipo bajo **cómo conectarse a este equipo**. Las necesitará para configurar a los clientes.

### <a name="windows-7-and-early-version-of-windows-10"></a>Windows 7 y una versión anterior de Windows 10

Para configurar el equipo para el acceso remoto, descargue y ejecute el [Ayudante de escritorio remoto de Microsoft](https://www.microsoft.com/download/details.aspx?id=50042). Este asistente actualiza la configuración del sistema para habilitar el acceso remoto, se garantiza que el equipo está activo para las conexiones y comprueba que el firewall permita las conexiones a Escritorio remoto. 

### <a name="all-versions-of-windows-legacy-method"></a>Todas las versiones de Windows (método heredado)

Para habilitar Escritorio remoto mediante las propiedades del sistema heredado, siga las instrucciones para [conectar a otro equipo mediante conexión a Escritorio remoto](https://windows.microsoft.com/windows/remote-desktop-connection-faq).

## <a name="should-i-enable-remote-desktop"></a>¿Se debe habilitar Escritorio remoto?

Si solo desea tener acceso a su equipo al que está físicamente sentado delante de él, no es necesario habilitar Escritorio remoto. Habilitar el escritorio remoto, abre un puerto en el equipo que está visible para la red local. Sólo debe habilitar Escritorio remoto en redes de confianza, como su casa. Que no desea habilitar Escritorio remoto en cualquier equipo donde el acceso se controla estrictamente.

Tenga en cuenta que al habilitar el acceso a Escritorio remoto, se concede a cualquier persona en el grupo de administradores, así como todos los usuarios adicionales se selecciona, la capacidad de tener acceso remoto a sus cuentas en el equipo.

Debe asegurarse de que cada cuenta que tenga acceso a su equipo está configurado con una contraseña segura.

## <a name="why-allow-connections-only-with-network-level-authentication"></a>¿Por qué permitir conexiones sólo con autenticación a nivel de red? 

Si desea restringir quién puede tener acceso a su equipo, elija Permitir el acceso sólo con nivel de autenticación red (NLA). Cuando se habilita esta opción, los usuarios deben autenticarse en la red antes de poder conectarse a su equipo. Permitir conexiones sólo desde equipos que ejecuten Escritorio remoto con NLA es un método de autenticación más seguro que puede ayudar a proteger el equipo de software y los usuarios malintencionados. Para obtener más información acerca de NLA y escritorio remoto, consulte [configurar NLA para conexiones a RDS](https://technet.microsoft.com/library/cc732713(v=ws.11).aspx).

Si se conectará remotamente a un equipo en la red doméstica desde fuera de esa red, no seleccione esta opción.
