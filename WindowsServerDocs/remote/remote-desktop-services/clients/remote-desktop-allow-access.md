---
title: 'Escritorio remoto: permitir el acceso al equipo'
description: Obtén información acerca de las opciones de acceso remoto a tu equipo.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.topic: article
ms.assetid: 0f1557ed-53f7-4333-b023-c8e0f4b58bf4
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 06/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: 0596dbd847ae8e64e1f4c780f142f28e34656ae8
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2020
ms.locfileid: "80855908"
---
# <a name="remote-desktop---allow-access-to-your-pc"></a>Escritorio remoto: permitir el acceso al equipo

>Se aplica a: Windows 10, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2

Puedes usar Escritorio remoto para conectarte y controlar el equipo desde un dispositivo remoto mediante un [cliente de Escritorio remoto de Microsoft ](remote-desktop-clients.md) (disponible para Windows, iOS, macOS y Android). Cuando permites las conexiones remotas a tu equipo, puedes usar otro dispositivo para conectar tu equipo y tener acceso a todas las aplicaciones, archivos y recursos de red como si estuvieras sentado delante de su escritorio.  

> [!NOTE]
> Puedes usar Escritorio remoto para conectarte a Windows 10 Pro y Enterprise, Windows 8.1 y 8 Enterprise y Pro, Windows 7 Professional, Enterprise y Ultimate, y a las versiones de Windows Server posteriores a Windows Server 2008. No puedes conectarte a equipos que ejecutan una edición Home (como Windows 10 Home). 

Para conectarse a un equipo remoto, ese equipo debe estar activado, debe tener una conexión de red, Escritorio remoto debe estar habilitado, debes tener acceso de red al equipo remoto (puede ser a través de Internet) y debes tener permiso para conectarte. Para tener permiso para conectarte, debes encontrarte en la lista de usuarios. Antes de iniciar una conexión, es una buena idea buscar el nombre del equipo al que te estás conectando y asegurarte de que las conexiones de Escritorio remoto estén permitidas a través de su firewall.

## <a name="how-to-enable-remote-desktop"></a>Habilitación del Escritorio remoto

La forma más sencilla de permitir el acceso a tu equipo desde un dispositivo remoto es utilizar las opciones de Escritorio remoto en Configuración. Dado que esta funcionalidad se agregó en la actualización de Windows 10 Fall Creators (1709), también está disponible una aplicación descargable independiente que proporciona una funcionalidad similar para versiones anteriores de Windows. También puedes utilizar la forma heredada de habilitar Escritorio remoto; sin embargo, este método proporciona menos funcionalidad y validación.

### <a name="windows-10-fall-creator-update-1709-or-later"></a>Windows 10 Fall Creator Update (1709) o posterior

Puedes configurar tu equipo para el acceso remoto con unos pocos pasos sencillos.
1. En el dispositivo al que quieres conectarte, selecciona **Inicio** y haz clic en el icono de **Configuración** situado a la izquierda.
2. Selecciona el grupo **Sistema** seguido por el elemento [**Escritorio remoto**](ms-settings:remotedesktop).
3. Utiliza el control deslizante para habilitar Escritorio remoto.
4. También se recomienda mantener el equipo activo y reconocible para facilitar las conexiones. Haz clic en **Mostrar configuración** para habilitarla.
5. Según sea necesario, agrega los usuarios que pueden conectarse de forma remota; para ello, haz clic en **Select users that can remotely access this PC** (Seleccionar los usuarios que pueden obtener acceso remoto a este equipo).
   1. Los miembros del grupo de administradores tienen acceso de forma automática.
6. Toma nota del nombre de este equipo en **How to connect to this PC** (Cómo conectarse a este equipo). Lo necesitarás para configurar a los clientes.

### <a name="windows-7-and-early-version-of-windows-10"></a>Windows 7 y una versión anterior de Windows 10

Para configurar el equipo para el acceso remoto, descarga y ejecuta el [Asistente para el Escritorio remoto de Microsoft](https://www.microsoft.com/download/details.aspx?id=50042). Este asistente actualiza la configuración del sistema para habilitar el acceso remoto, garantiza que el equipo esté activo para las conexiones y comprueba que el firewall permita las conexiones al Escritorio remoto. 

### <a name="all-versions-of-windows-legacy-method"></a>Todas las versiones de Windows (método heredado)

Para habilitar Escritorio remoto mediante las propiedades del sistema heredado, sigue las instrucciones para [conectarte a otro equipo mediante la Conexión a Escritorio remoto](https://windows.microsoft.com/windows/remote-desktop-connection-faq).

## <a name="should-i-enable-remote-desktop"></a>¿Cómo se habilita el Escritorio remoto?

Si solo quieres acceder a tu equipo para usarlo físicamente, no necesitas habilitar el Escritorio remoto. Al habilitar el Escritorio remoto, se abre un puerto en el equipo que esté visible para la red local. Solo debes habilitar el Escritorio remoto en redes de confianza, como las de tu casa. Tampoco quieres habilitar el Escritorio remoto en ningún equipo en el que el acceso esté estrictamente controlado.

Ten en cuenta que, cuando habilitas el acceso al Escritorio remoto, estás otorgando a cualquier usuario del grupo Administradores, así como a cualquier usuario adicional que selecciones, la capacidad de acceder de forma remota a tus cuentas en el equipo.

Debes asegurarte de que todas las cuentas que tienen acceso a tu equipo estén configuradas con una contraseña segura.

## <a name="why-allow-connections-only-with-network-level-authentication"></a>¿Por qué permitir conexiones solo con Autenticación a nivel de red? 

Si quieres restringir quién puede acceder a tu equipo, elige permitir el acceso solo con Autenticación a nivel de red (NLA). Cuando se habilita esta opción, los usuarios deben autenticarse en la red antes de conectarse a tu equipo. Permitir conexiones solo desde equipos que ejecutan Escritorio remoto con NLA es un método de autenticación más seguro que puede ayudar a proteger el equipo de usuarios y software malintencionados. Para más información acerca de NLA y el Escritorio remoto, consulta [Configuración de las conexiones NLA para RDS](https://technet.microsoft.com/library/cc732713(v=ws.11).aspx).

Si te conectas de forma remota a un equipo en la red doméstica desde fuera de esa red, no selecciones esta opción.
