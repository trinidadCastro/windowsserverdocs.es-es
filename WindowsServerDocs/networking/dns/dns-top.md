---
title: Sistema de nombres de dominio (DNS)
description: Este tema proporciona información general de DNS en Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: 1324ba18-4e28-4b9d-bbe7-75707e6d30ab
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3d4ec63e904dd899a3ddc53a59274ad607136edd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870826"
---
# <a name="domain-name-system-dns"></a>Sistema de nombres de dominio (DNS)

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Sistema de nombres de dominio (DNS) es uno del conjunto de protocolos que incluyen TCP/IP estándar del sector, y juntos el cliente DNS y servidor DNS proporcionan servicios de resolución de nombres equipo una dirección IP del nombre de la asignación a equipos y usuarios.  
  
> [!NOTE]  
> Además de este tema, el siguiente contenido DNS está disponible.  
>   
> -   [Novedades en el cliente DNS](What-s-New-in-DNS-Client.md)  
> -   [Novedades en el servidor DNS](What-s-New-in-DNS-Server.md)  
> -   [Guía del escenario de directiva DNS](deploy/DNS-Policy-Scenario-Guide.md)  
> -   Video: [Windows Server 2016: Administración de DNS en IPAM](https://channel9.msdn.com/Blogs/windowsserver/Windows-Server-2016-DNS-management-in-IPAM)  
  
En Windows Server 2016, el DNS es un rol de servidor que se puede instalar mediante los comandos del administrador del servidor o Windows PowerShell. Si va a instalar un nuevo bosque de Active Directory y dominio, DNS se instala automáticamente con Active Directory que el servidor de catálogo Global para el bosque y dominio.  
  
Los servicios de dominio de Active Directory (AD DS) utiliza DNS como mecanismo de ubicación de controlador de dominio. Cuando se realiza cualquiera de las principales operaciones de Active Directory, como la autenticación, actualización o la búsqueda, los equipos utilizan DNS para buscar controladores de dominio de Active Directory. Además, los controladores de dominio use DNS se localicen entre sí.  
  
El servicio cliente DNS se incluye en todas las versiones de cliente y servidor del sistema operativo Windows y se ejecuta de forma predeterminada tras la instalación del sistema operativo. Al configurar una conexión de red TCP/IP con la dirección IP de un servidor DNS, el cliente DNS consulta el servidor DNS para detectar los controladores de dominio y para resolver los nombres de equipo a direcciones IP. Por ejemplo, cuando un usuario de red con una cuenta de usuario de Active Directory inicia sesión en un dominio de Active Directory, el servicio cliente DNS consulta el servidor DNS para localizar un controlador de dominio para el dominio de Active Directory. Cuando el servidor DNS responde a la consulta y proporciona la dirección IP del controlador de dominio para el cliente, el cliente contacta con el controlador de dominio y puede comenzar el proceso de autenticación.  
  
Los servicios de servidor DNS de Windows Server 2016 y el cliente DNS usan el protocolo DNS que se incluye en el conjunto de protocolos TCP/IP. DNS es parte de la capa de aplicación del modelo de referencia de TCP/IP, tal como se muestra en la siguiente ilustración.  
  
![DNS en TCP/IP](../media/Domain-Name-System--DNS-/dns_in_tcpip.jpg)  
  

