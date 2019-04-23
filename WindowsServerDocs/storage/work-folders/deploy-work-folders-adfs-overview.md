---
title: Información general para implementar Carpetas de trabajo con AD FS y el Proxy de aplicación web (WAP)
ms.prod: windows-server-threshold
ms.technology: storage-work-folders
ms.topic: article
ms.assetid: ea19f0f0-6cc0-4322-b387-c0873f7795ad
manager: klaasl
ms.author: jeffpatt
author: JeffPatt24
ms.date: 4/5/2017
ms.openlocfilehash: 48c7d771c7ec75a4bc340608a96410ea388418e9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874626"
---
# <a name="deploy-work-folders-with-ad-fs-and-web-application-proxy-overview"></a>Implementar carpetas de trabajo con AD FS y Proxy de aplicación Web: Información general

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Los temas de esta sección proporcionan instrucciones para implementar Carpetas de trabajo mediante el componente Servicios de federación de Active Directory (AD FS) y el Proxy de aplicación web (WAP). Estas instrucciones están diseñadas para ayudarte a crear una instalación completa y funcional del componente Carpetas de trabajo, mediante los equipos cliente que estén listos para empezar a usar Carpetas de trabajo de forma local o a través de Internet.  
  
Carpetas de trabajo es un componente que se introdujo en Windows Server 2012 R2 y que permite a aquellos profesionales de tecnologías de la información sincronizar archivos de trabajo entre sus dispositivos. Para obtener más información acerca de Carpetas de trabajo, consulta [Introducción a Carpetas de trabajo](Work-Folders-Overview.md).  
  
Para habilitar a los usuarios para que sincronicen sus Carpetas de trabajo por Internet, debe publicar Carpetas de trabajo a través de un proxy inverso, lo que hace que Carpetas de trabajo esté disponible externamente en Internet. El Proxy de aplicación web que se incluye en AD FS, es una opción que puedes usar para proporcionar la funcionalidad de proxy inverso. El Proxy de aplicación web autentica previamente el acceso a la aplicación web Carpetas de trabajo mediante AD FS, para que los usuarios puedan acceder a Carpetas de trabajo desde fuera de la red corporativa y desde cualquier dispositivo. 

> [!NOTE]
>   Las instrucciones que se tratan en esta sección toman como referencia un entorno de Windows Server 2016. Si estás usando Windows Server 2012 R2, lee el artículo en el que se detallan las [instrucciones para Windows Server 2012 R2](https://technet.microsoft.com/library/dn747208(v=ws.11).aspx).
  
En estos temas se incluye lo siguiente:  
  
-   Instrucciones paso a paso para configurar e implementar Carpetas de trabajo con AD FS y el Proxy de aplicación web, a través de la interfaz de usuario de Windows Server. En estas instrucciones se describe cómo configurar un entorno de prueba simple con certificados autofirmados. A continuación, puedes usar el ejemplo de prueba como guía para crear un entorno de producción que use certificados de confianza pública.  
  
## <a name="prerequisites"></a>Requisitos previos  
Para seguir los procedimientos y ejemplos que se detallan estos temas, debes tener los siguientes componentes listos:  
  
-   Un bosque de Active Directory® Domain Services con extensiones de esquema en Windows Server 2012 R2 para poder remitir automáticamente los equipos y los dispositivos al servidor de archivos adecuado cuando se usen varios servidores de archivos. Es preferible que el DNS esté habilitado en el bosque, pero esto no es necesario.  
  
-   Un controlador de dominio: Un servidor que tiene habilitado el rol de AD DS y se configura con un dominio (por ejemplo, prueba, contoso.com).  
  
    Un controlador de dominio que ejecute al menos Windows Server 2012 R2; esto es necesario para poder registrar el dispositivo en Workplace Join. Si no quieres usar Workplace Join, puedes ejecutar Windows Server 2012 en el controlador de dominio.  
  
-   Dos servidores que estén unidos al dominio (por ejemplo, contoso.com) y que ejecuten Windows Server 2016. Un servidor se usará para en AD FS y el otro se usará en Carpetas de trabajo.  
  
-   Un servidor que no esté unido a un dominio y que ejecute Windows Server 2016. Este servidor ejecutará el Proxy de aplicación web y debe tener una tarjeta de red para el dominio de red (por ejemplo, contoso.com) y otra tarjeta de red para la red externa.  
  
-   Un equipo cliente unido a un dominio que ejecute Windows 7 o posterior.  
  
-   Un equipo cliente que no esté unido a un dominio que ejecute Windows 7 o posterior.  
  
En cuanto al entorno de prueba que estamos explicando en esta guía, debes tener la topología que se muestra en el siguiente diagrama. Estos equipos pueden ser máquinas físicas o virtuales (VM). 
  
![El diagrama muestra los segmentos de red de Internet, DMZ y Contoso. En el segmento de Internet: Client2; en la red Perimetral: un servidor WAP. en el segmento de Contoso: Trabajar en carpetas de servidor, un controlador de dominio, un servidor de AD FS y Client1](media/deploy-work-folders-adfs/WF_ADFS_WAP_Diagram.png)

## <a name="deployment-overview"></a>Información general sobre la implementación  
En este grupo de temas, te guiaremos paso a paso mediante un ejemplo que muestra la manera de configurar AD FS, el Proxy de aplicación web y las Carpetas de trabajo en un entorno de prueba. Los componentes se configurarán en este orden:  
  
1.  AD FS  
  
2.  Carpetas de trabajo  
  
3.  Proxy de aplicación web  
  
4.  Estación de trabajo unida y no unida a un dominio  
  
Asimismo, usarás un script de Windows PowerShell para crear certificados autofirmados.  
  
## <a name="deployment-steps"></a>Pasos de implementación  
Para realizar la implementación mediante la interfaz de usuario de Windows Server, sigue los pasos descritos en estos temas:  
  
-   [Implementar carpetas de trabajo con AD FS y Proxy de aplicación Web: Paso 1: configuración de AD FS](deploy-work-folders-adfs-step1.md)  
  
-   [Implementar carpetas de trabajo con AD FS y Proxy de aplicación Web: Paso 2, trabajo posterior a la configuración de AD FS](deploy-work-folders-adfs-step2.md)  
  
-   [Implementar carpetas de trabajo con AD FS y Proxy de aplicación Web: Paso 3, configurar carpetas de trabajo](deploy-work-folders-adfs-step3.md)  
  
-   [Implementar carpetas de trabajo con AD FS y Proxy de aplicación Web: Paso 4: configurar el Proxy de aplicación Web](deploy-work-folders-adfs-step4.md)  
  
-   [Implementar carpetas de trabajo con AD FS y Proxy de aplicación Web: El paso 5, configure los clientes](deploy-work-folders-adfs-step5.md)  

## <a name="see-also"></a>Vea también  
[Introducción a las carpetas de trabajo](Work-Folders-Overview.md)  
[Diseñar una implementación de carpetas de trabajo](Plan-Work-Folders.md)  
[Implementar carpetas de trabajo](Deploy-Work-Folders.md)  
  

