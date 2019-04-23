---
ms.date: 01/07/2019
contributor: damaerteMSFT
author: maertendMSFT
keywords: OpenSSH, SSH, Win32-OpenSSH
title: OpenSSH en Windows
ms.product: w10
ms.openlocfilehash: 68ced1ff133495d7658e486e7e72321e18857b21
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843366"
---
# <a name="openssh-in-windows"></a>OpenSSH en Windows

OpenSSH es la versión de código abierto de las herramientas de Secure Shell (SSH) utilizadas por los administradores de Linux y otras que no son de Windows para la administración entre plataformas de sistemas remotos. Se ha agregado a Windows a partir del otoño 2018 OpenSSH y se incluye en Windows 10 y Windows Server 2019. 

SSH se basa en una arquitectura cliente / servidor donde el sistema que está trabajando el usuario es el cliente y el sistema remoto que está administrando es el servidor. OpenSSH incluye una gama de componentes y herramientas diseñados para proporcionar un enfoque sencillo y seguro para la administración remota del sistema, incluidos:

* sshd.exe, que es el componente de servidor SSH que debe ejecutarse en el sistema que se administran de forma remota 
* SSH.exe, que es el componente de cliente SSH que se ejecuta en el sistema del usuario local
* SSH keygen.exe genera, administra y convierte las claves de autenticación de SSH 
* SSH agent.exe almacena claves privadas utilizadas para la autenticación de clave pública
* SSH add.exe claves privadas agrega a la lista permitida por el servidor
* SSH keyscan.exe ayuda a recopilar las claves de host públicas SSH de un número de hosts
* SFTP.exe es el servicio que proporciona el protocolo seguro de transferencia de archivos y se ejecuta a través de SSH
* SCP.exe es una utilidad de copia de archivo que se ejecuta en SSH

Documentación de esta sección se centra en cómo se usa OpenSSH en Windows, incluida una instalación y casos de uso y la configuración de Windows específicos. Estos son los temas:
* Instalación y desinstalación de OpenSSH para Windows Server 2019 y Windows 10

Más documentación detallada sobre las características comunes de OpenSSH está disponible en línea en [OpenSSH.com](https://www.openssh.com/manual.html). 

El patrón [proyecto de código abierto de OpenSSH](https://www.openssh.com) está administrado por los desarrolladores en el proyecto OpenBSD. La bifurcación de Microsoft de este proyecto está en [Github](https://github.com/PowerShell/openssh-portable).
Comentarios sobre Windows OpenSSH es esperada y puede proporcionarse mediante la creación de problemas de Github en nuestro [repositorio de OpenSSH Github](https://github.com/PowerShell/openssh-portable). 
