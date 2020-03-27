---
title: Sistema de nombres de dominio (DNS)
description: En este tema se proporciona información general sobre DNS en Windows Server 2016
manager: brianlic
ms.prod: windows-server
ms.technology: networking-dns
ms.topic: article
ms.assetid: 1324ba18-4e28-4b9d-bbe7-75707e6d30ab
ms.author: lizross
author: eross-msft
ms.openlocfilehash: f23b6c1056ea29f583da055b303fb648539240ba
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "80317714"
---
# <a name="domain-name-system-dns"></a>Sistema de nombres de dominio (DNS)

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

El sistema de nombres de dominio (DNS) es uno de los conjuntos de protocolos estándar del sector que componen TCP/IP y, conjuntamente, el cliente DNS y el servidor DNS proporcionan servicios de resolución de nombres de nombre de equipo a dirección IP para equipos y usuarios.  
  
> [!NOTE]  
> Además de este tema, está disponible el siguiente contenido DNS.  
>   
> -   [Novedades del cliente DNS](What-s-New-in-DNS-Client.md)  
> -   [Novedades en el servidor DNS](What-s-New-in-DNS-Server.md)  
> -   [Guía del escenario de la Directiva DNS](deploy/DNS-Policy-Scenario-Guide.md)  
> -   Vídeo: [Windows Server 2016: administración de DNS en IPAM](https://channel9.msdn.com/Blogs/windowsserver/Windows-Server-2016-DNS-management-in-IPAM)  
  
En Windows Server 2016, DNS es un rol de servidor que puede instalar mediante Administrador del servidor o comandos de Windows PowerShell. Si va a instalar un nuevo bosque y dominio de Active Directory, DNS se instala automáticamente con Active Directory como el servidor de catálogo global para el bosque y el dominio.  
  
Active Directory Domain Services (AD DS) utiliza DNS como mecanismo de ubicación del controlador de dominio. Cuando se realiza alguna de las operaciones de Active Directory principal, como la autenticación, la actualización o la búsqueda, los equipos usan DNS para buscar Active Directory controladores de dominio. Además, los controladores de dominio usan DNS para localizarse entre sí.  
  
El servicio cliente DNS se incluye en todas las versiones de cliente y servidor del sistema operativo Windows y se ejecuta de forma predeterminada en la instalación del sistema operativo. Cuando se configura una conexión de red TCP/IP con la dirección IP de un servidor DNS, el cliente DNS consulta al servidor DNS para detectar controladores de dominio y resolver nombres de equipo en direcciones IP. Por ejemplo, cuando un usuario de red con una cuenta de usuario Active Directory inicia sesión en un dominio Active Directory, el servicio cliente DNS consulta al servidor DNS para encontrar un controlador de dominio para el dominio Active Directory. Cuando el servidor DNS responde a la consulta y proporciona la dirección IP del controlador de dominio al cliente, el cliente se pone en contacto con el controlador de dominio y puede comenzar el proceso de autenticación.  
  
Los servicios de cliente DNS y servidor DNS de Windows Server 2016 usan el protocolo DNS que se incluye en el conjunto de protocolos TCP/IP. DNS forma parte de la capa de aplicación del modelo de referencia de TCP/IP, tal y como se muestra en la siguiente ilustración.  
  
![DNS en TCP/IP](../media/Domain-Name-System--DNS-/dns_in_tcpip.jpg)  
  

