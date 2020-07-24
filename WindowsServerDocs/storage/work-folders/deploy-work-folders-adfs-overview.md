---
title: Información general sobre la implementación de carpetas de trabajo con AD FS y proxy de aplicación Web
ms.prod: windows-server
ms.technology: storage-work-folders
ms.topic: article
ms.assetid: ea19f0f0-6cc0-4322-b387-c0873f7795ad
manager: klaasl
ms.author: jeffpatt
author: JeffPatt24
ms.date: 4/5/2017
ms.openlocfilehash: 5eff8eab3f6e45b515d71e5043f0cd60be28a384
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "86965817"
---
# <a name="deploy-work-folders-with-ad-fs-and-web-application-proxy-overview"></a>Implementar carpetas de trabajo con AD FS y proxy de aplicación web: información general

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En los temas de esta sección se proporcionan instrucciones para implementar carpetas de trabajo con Servicios de federación de Active Directory (AD FS) (AD FS) y proxy de aplicación Web. Las instrucciones están diseñadas para ayudarle a crear una configuración completa de carpetas de trabajo en funcionamiento con equipos cliente que están preparados para empezar a usar carpetas de trabajo de forma local o a través de Internet.  
  
Carpetas de trabajo es un componente incluido en Windows Server 2012 R2 que permite a los trabajadores de la información sincronizar archivos de trabajo entre sus dispositivos. Para más información sobre Carpetas de trabajo, vea [Introducción a Carpetas de trabajo](Work-Folders-Overview.md).  
  
Para habilitar a los usuarios para que sincronicen sus Carpetas de trabajo por Internet, debe publicar Carpetas de trabajo a través de un proxy inverso, lo que hace que Carpetas de trabajo esté disponible externamente en Internet. El proxy de aplicación Web, que se incluye en AD FS, es una opción que puede usar para proporcionar funcionalidad de proxy inverso. El proxy de aplicación web autentica previamente el acceso a la aplicación web carpetas de trabajo mediante AD FS, de modo que los usuarios de cualquier dispositivo puedan tener acceso a carpetas de trabajo desde fuera de la red corporativa. 

> [!NOTE]
>   Las instrucciones que se describen en esta sección son para un entorno de Windows Server 2016. Si usa Windows Server 2012 R2, siga las instrucciones de [Windows server 2012 R2](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn747208(v=ws.11)).
  
En estos temas se proporciona lo siguiente:  
  
-   Instrucciones paso a paso para configurar e implementar carpetas de trabajo con AD FS y proxy de aplicación web a través de la interfaz de usuario de Windows Server. Las instrucciones describen cómo configurar un entorno de prueba simple con certificados autofirmados. Después puede usar el ejemplo de prueba como guía para ayudarle a crear un entorno de producción que utiliza certificados de confianza pública.  
  
## <a name="prerequisites"></a>Requisitos previos  
Para seguir los procedimientos y ejemplos de estos temas, debe tener preparados los siguientes componentes:  
  
-   Un Active Directory® bosque de servicios de dominio con extensiones de esquema en Windows Server 2012 R2 para admitir la referencia automática de equipos y dispositivos al servidor de archivos adecuado cuando se usan varios servidores de archivos. Es preferible que DNS esté habilitado en el bosque, pero esto no es necesario.  
  
-   Un controlador de dominio: un servidor que tiene habilitado el rol AD DS y que está configurado con un dominio (para el ejemplo de prueba, contoso.com).  
  
    Se necesita un controlador de dominio que ejecute Windows Server 2012 R2 como mínimo para admitir el registro de dispositivos para Workplace Join. Si no desea usar Workplace Join, puede ejecutar Windows Server 2012 en el controlador de dominio.  
  
-   Dos servidores que se unen al dominio (por ejemplo, contoso.com) y que ejecutan Windows Server 2016. Un servidor se usará para AD FS y el otro se usará para carpetas de trabajo.  
  
-   Un servidor que no está unido a un dominio y que ejecuta Windows Server 2016. Este servidor ejecutará el proxy de aplicación web y debe tener una tarjeta de red para el dominio de red (por ejemplo, contoso.com) y otra tarjeta de red para la red externa.  
  
-   Un equipo cliente unido a un dominio que ejecuta Windows 7 o posterior.  
  
-   Un equipo cliente que no está unido a un dominio que ejecuta Windows 7 o posterior.  
  
En el entorno de prueba que se trata en esta guía, debe tener la topología que se muestra en el diagrama siguiente. Los equipos pueden ser equipos físicos o máquinas virtuales (VM). 
  
![Diagrama que muestra los segmentos de red de Internet, DMZ y contoso. En el segmento de Internet: cliente2; en la red perimetral: un servidor WAP; en el segmento contoso: servidor de carpetas de trabajo, un controlador de dominio, un servidor de AD FS y Client1](media/deploy-work-folders-adfs/WF_ADFS_WAP_Diagram.png)

## <a name="deployment-overview"></a>Introducción a la implementación  
En este grupo de temas, le guiará a través de un ejemplo paso a paso de la configuración de AD FS, el proxy de aplicación web y las carpetas de trabajo en un entorno de prueba. Los componentes se configurarán en este orden:  
  
1.  AD FS  
  
2.  Carpetas de trabajo  
  
3.  Proxy de aplicación web  
  
4.  La estación de trabajo unida a un dominio y una estación de trabajo no unida a un dominio  
  
También usará un script de Windows PowerShell para crear certificados autofirmados.  
  
## <a name="deployment-steps"></a>Pasos de implementación  
Para realizar la implementación mediante la interfaz de usuario de Windows Server, siga los pasos de estos temas:  
  
-   [Implementar carpetas de trabajo con AD FS y proxy de aplicación web: paso 1, configurar AD FS](deploy-work-folders-adfs-step1.md)  
  
-   [Implementar carpetas de trabajo con AD FS y proxy de aplicación web: paso 2 AD FS trabajo posterior a la configuración](deploy-work-folders-adfs-step2.md)  
  
-   [Implementar carpetas de trabajo con AD FS y proxy de aplicación web: paso 3, configurar carpetas de trabajo](deploy-work-folders-adfs-step3.md)  
  
-   [Implementar carpetas de trabajo con AD FS y proxy de aplicación web: paso 4: configurar el proxy de aplicación Web](deploy-work-folders-adfs-step4.md)  
  
-   [Implementar carpetas de trabajo con AD FS y proxy de aplicación web: paso 5, configurar clientes](deploy-work-folders-adfs-step5.md)  

## <a name="see-also"></a>Consulte también  
[Introducción a Carpetas de trabajo](Work-Folders-Overview.md)  
[Diseñar una implementación de Carpetas de trabajo](Plan-Work-Folders.md)  
[Implementar Carpetas de trabajo](Deploy-Work-Folders.md)  
  
