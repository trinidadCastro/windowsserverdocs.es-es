---
title: Instalar el WEB1 de servidor Web
description: Este tema es parte de la Guía de certificados de servidor de implementación para implementaciones de conexión inalámbrica y cableadas 802.1X
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: f51c9e38-98bb-49c1-9d39-427d07021499
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e818eac49719b394a2c73cc125a2e7ba9ea80c82
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="install-the-web-server-web1"></a>Instalar el WEB1 de servidor Web

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

El rol de servidor Web (IIS) en Windows Server 2016, proporciona una plataforma segura, fácil de administrar, modular y extensible para hospedar los sitios Web, servicios y aplicaciones de forma segura. Con IIS, puede compartir información con los usuarios en Internet, intranet o una extranet. IIS es una plataforma web unificado que se integra IIS, ASP.NET, servicios FTP, PHP y Windows Communication Foundation (WCF).  

Cuando implementas certificados de servidor, el servidor Web proporciona una ubicación donde puede publicar la lista de revocación de certificados (CRL) para la entidad de certificación (CA). Tras la publicación, la CRL es accesible para todos los equipos de la red para que puedan usar esta lista durante el proceso de autenticación para comprobar que no se revoquen certificados presentados por otros equipos.   

Si es un certificado en la CRL como revocado, el esfuerzo de autenticación se produce un error y el equipo queda protegido frente al confiar en una entidad que tiene un certificado que ya no es válido.  

Antes de instalar el rol de servidor Web (IIS), asegúrate de que has configurado el nombre del servidor y la dirección IP y hayan unido el equipo al dominio.  

## <a name="to-install-the-web-server-iis-server-role"></a>Para instalar el rol de servidor Web Server (IIS)  
Para completar este procedimiento, debe ser miembro de la **administradores** grupo.  

>[!NOTE]  
>Para realizar este procedimiento mediante Windows PowerShell, abre PowerShell, escribe el siguiente comando y, a continuación, presione ENTRAR.  
`Install-WindowsFeature Web-Server -IncludeManagementTools`  

1.  En el administrador del servidor, haz clic en **administrar**y, a continuación, haz clic en **agregar Roles y características**. Abre el agregar Roles and Features Wizard.  
2.  En **antes de comenzar**, haz clic en **siguiente**.  

**Ten en cuenta**   
La **antes de comenzar** no se muestra la página del agregar Roles y características de asistente si ejecutado previamente el agregar Roles y características de asistente y ha seleccionado **omitir esta página predeterminada** en ese momento.  

3.  En la **tipo de instalación** página, haz clic en **siguiente**.  
4.  En la **selección de servidor** página, haz clic en **siguiente**.  
5.  En la **roles de servidor** página, seleccione **servidor Web (IIS)**y, a continuación, haz clic en **siguiente**.  
6.  Haz clic en **siguiente** hasta que haya aceptado el valor todas las de configuración del servidor web y, a continuación, haz clic en **instalar**.  
7.  Comprueba que todas las instalaciones tuvieron éxito y, a continuación, haz clic en **cerrar**.
