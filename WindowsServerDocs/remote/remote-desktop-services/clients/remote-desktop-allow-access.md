---
title: 'Escritorio remoto: permitir el acceso a tu PC'
description: Obtén información sobre las opciones de acceso remoto a tu PC
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
ms.openlocfilehash: d3c85c1c765583eb26cef60ecd245708e0e02772
ms.sourcegitcommit: d3f73936160505a40633ad8dd5931ac5fe3eccdb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/12/2019
ms.locfileid: "9297224"
---
# Escritorio remoto: permitir el acceso a tu PC

>Se aplica a: Windows 10, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2

Puedes usar Escritorio remoto para conectarse y controlar el equipo de un dispositivo remoto mediante el uso de un [cliente de escritorio remoto de Microsoft](remote-desktop-clients.md) (disponible en Windows, iOS, macOS y Android). Al permitir conexiones remotas a tu equipo, puedes usar otro dispositivo para conectarte a tu PC y tener acceso a todas las aplicaciones, archivos y los recursos de red como si estuvieras sentado en tu servicio de asistencia.  

> [!NOTE]
> Puedes usar Escritorio remoto para conectarse a Windows 10 Pro y Enterprise, Windows 8.1 y 8 Enterprise y Pro, Windows 7 Professional, Enterprise y Ultimate y Windows Server las versiones más recientes que Windows Server 2008. No puedes conectarte a los equipos que ejecutan la edición de inicio (por ejemplo, Windows 10 Home). 

Para conectarse a un equipo remoto, que debe estar activado el equipo, debe tener una conexión de red, escritorio remoto debe estar habilitado, debe tener acceso a la red al equipo remoto (Esto puede ser a través de Internet), y debe tener permiso para conectarse. Para que obtener permiso para conectarse, debe ser en la lista de usuarios. Antes de empezar a una conexión, es una buena idea para buscar el nombre del equipo que va a conectar y para asegurarse de que se permiten las conexiones a Escritorio remoto a través de su firewall.

## Cómo habilitar Escritorio remoto

La forma más sencilla de permitir el acceso a tu PC desde un dispositivo remoto está usando las opciones de escritorio remoto en configuración. Dado que esta funcionalidad se ha agregado en la Windows 10 Fall Creators update (1709), independiente descargable también está disponible la aplicación que proporciona una funcionalidad similar para las versiones anteriores de Windows. También puedes usar el modo heredado de habilitar Escritorio remoto, sin embargo, este método proporciona menos funcionalidad y la validación.

### Windows 10 Fall Creators Update (1709) o posterior

Puedes configurar tu PC para el acceso remoto con unos sencillos pasos.
1. En el dispositivo al que desea conectarse para seleccionar el **Inicio** y la haga clic en el icono de **configuración** de la izquierda.
2. Selecciona el grupo de **sistema** seguido por el elemento de [**Escritorio remoto**](ms-settings:remotedesktop) .
3. Usar el control deslizante para habilitar Escritorio remoto.
4. También se recomienda mantener el equipo activo e intuitivo para facilitar las conexiones. Haz clic en **Mostrar la configuración** para habilitar.
5. Según sea necesario, agregar usuarios que se pueden conectar de forma remota haciendo clic en **Seleccionar usuarios que pueden tener acceso remoto a este equipo**.
   1. Los miembros del grupo de administradores automáticamente tienen acceso.
6. Anota el nombre del equipo en **cómo conectarse a este equipo**. Tendrás que esta opción para configurar a los clientes.

### Windows 7 y una versión anterior de Windows 10

Para configurar el equipo para el acceso remoto, descarga y ejecuta el [Asistente de escritorio remoto de Microsoft](https://www.microsoft.com/download/details.aspx?id=50042). Este asistente se actualiza la configuración del sistema para habilitar el acceso remoto, se garantiza que el equipo está activo para las conexiones y comprueba que el firewall permite las conexiones a Escritorio remoto. 

### Todas las versiones de Windows (método heredado)

Para habilitar Escritorio remoto mediante las propiedades del sistema heredado, sigue las instrucciones para [conectarse a otro equipo mediante la conexión a Escritorio remoto](https://windows.microsoft.com/windows/remote-desktop-connection-faq).

## ¿Puedo habilitar Escritorio remoto?

Si solo quieres tener acceso a tu PC cuando físicamente está sentado frente a él, no necesitas habilitar Escritorio remoto. Habilitar Escritorio remoto, abre un puerto en el equipo que está visible para la red local. Solo debe habilitar Escritorio remoto en redes de confianza, como su hogar. Además, no quieres habilitar Escritorio remoto en cualquier equipo donde está estrictamente controlado acceso.

Ten en cuenta que cuando se habilita el acceso a Escritorio remoto, que concede a cualquier persona del grupo de administradores, así como los usuarios adicionales selecciones, la capacidad de acceso remoto a sus cuentas en el equipo.

Debes asegurarte de que cada cuenta que tenga acceso a tu PC está configurado con una contraseña segura.

## ¿Por qué permitir conexiones solo con autenticación a nivel de red? 
 
Si quieres limitar quién puede tener acceso a tu PC, elegir permitir el acceso solo con nivel de autenticación red (NLA). Al habilitar esta opción, los usuarios deben autenticarse en la red antes de que se pueden conectar a tu PC. Permitir que solo las conexiones desde equipos que ejecuten Escritorio remoto con NLA es un método de autenticación más seguro que puede ayudar a proteger tu equipo de software y los usuarios malintencionados. Para obtener más información sobre NLA y escritorio remoto, echa un vistazo a [Configurar NLA para las conexiones de RDS](https://technet.microsoft.com/library/cc732713(v=ws.11).aspx). 

Si te conectas remotamente en un equipo de la red doméstica desde fuera de esa red, no selecciones esta opción.
