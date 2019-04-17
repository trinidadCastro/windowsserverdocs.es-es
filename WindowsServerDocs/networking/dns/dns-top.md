---
title: Sistema de nombres de dominio (DNS)
description: En este tema proporciona una descripción general de DNS en Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: 1324ba18-4e28-4b9d-bbe7-75707e6d30ab
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 23851d2d8015fc6ae9e0653e8a0843f8c4295162
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="domain-name-system-dns"></a>Sistema de nombres de dominio (DNS)

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Sistema de nombres de dominio (DNS) es uno del conjunto estándar de protocolos que componen TCP/IP y juntos DNS al cliente y servidor DNS proporcionan equipo nombre to-IP servicios de resolución de nombres de asignación de direcciones a equipos y usuarios.  
  
> [!NOTE]  
> Además de este tema, el siguiente contenido DNS está disponible.  
>   
> -   [Novedades en el cliente DNS](What-s-New-in-DNS-Client.md)  
> -   [Novedades en el servidor DNS](What-s-New-in-DNS-Server.md)  
> -   [Guía de escenario de directiva DNS](deploy/DNS-Policy-Scenario-Guide.md)  
> -   Vídeo: [Windows Server 2016: administración de IPAM DNS](https://channel9.msdn.com/Blogs/windowsserver/Windows-Server-2016-DNS-management-in-IPAM)  
  
En Windows Server 2016, DNS es un rol de servidor que se puede instalar mediante comandos de administrador del servidor o Windows PowerShell. Si vas a instalar un nuevo bosque de Active Directory y dominio, DNS se instala automáticamente con Active Directory como el servidor de catálogo Global para el bosque y dominio.  
  
Los servicios de dominio de Active Directory (AD DS) utiliza DNS como mecanismo de ubicación de controlador de dominio. Cuando se realiza cualquiera de las principales operaciones de Active Directory, como la autenticación, actualizar o buscar, los equipos usan DNS para buscar controladores de dominio de Active Directory. Además, los controladores de dominio usan DNS para encontrar entre sí.  
  
El servicio cliente DNS se incluye en todas las versiones de cliente y servidor de sistema operativo Windows y se ejecuta de forma predeterminada tras la instalación del sistema operativo. Cuando se configura una conexión de red TCP/IP con la dirección IP del servidor DNS, el cliente DNS consulta el servidor DNS para detectar controladores de dominio y para resolver los nombres de equipo a direcciones IP. Por ejemplo, cuando un usuario de la red con una cuenta de usuario de Active Directory inicia sesión un dominio de Active Directory, el servicio cliente DNS consulta el servidor DNS para buscar un controlador de dominio para el dominio de Active Directory. Cuando el servidor DNS responde a la consulta y proporciona la dirección IP del controlador de dominio al cliente, el cliente pone en contacto con el controlador de dominio y puede empezar el proceso de autenticación.  
  
Los servicios de servidor DNS de Windows Server 2016 y cliente DNS usan el protocolo DNS que se incluye en el conjunto de protocolos de TCP/IP. DNS es parte de la capa de la aplicación del modelo de referencia de TCP/IP, como se muestra en la siguiente ilustración.  
  
![DNS en TCP/IP](../media/Domain-Name-System--DNS-/dns_in_tcpip.jpg)  
  

