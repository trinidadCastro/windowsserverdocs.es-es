---
title: Instalar la WEB1 de servidor web
description: Este tema forma parte de la Guía de implementación de servidores de certificados para las implementaciones inalámbricas y cableadas 802.1X
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: f51c9e38-98bb-49c1-9d39-427d07021499
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 15da16094a47a2492dc9054e0671c3709fe23362
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446456"
---
# <a name="install-the-web-server-web1"></a>Instalar la WEB1 de servidor web

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

El rol de servidor Web (IIS) en Windows Server 2016 proporciona una plataforma segura, fácil de administrar, modular y extensible para hospedar sitios Web, servicios y aplicaciones de forma confiable. Con IIS, puede compartir información con usuarios en Internet, una intranet o extranet. IIS es una plataforma web unificada que integra IIS, ASP.NET, servicios FTP, PHP y Windows Communication Foundation (WCF).  

Al implementar los certificados de servidor, el servidor Web proporciona una ubicación donde puede publicar la lista de revocación de certificados (CRL) para la entidad de certificación (CA). Después de la publicación, la CRL es accesible para todos los equipos de la red para que puede usar esta lista durante el proceso de autenticación para comprobar que no se revoquen los certificados presentados por otros equipos.   

Si un certificado se encuentra en la CRL como revocado, se produce un error en el esfuerzo de autenticación y el equipo está protegido de confiar en una entidad que tiene un certificado que ya no es válido.  

Antes de instalar el rol servidor Web (IIS), asegúrese de que ha configurado la dirección IP y el nombre del servidor y se han unido el equipo al dominio.  

## <a name="to-install-the-web-server-iis-server-role"></a>Para instalar el rol de servidor web (IIS)  
Para completar este procedimiento, debe pertenecer al grupo **Administradores**.  

>[!NOTE]  
>Para llevar a cabo este procedimiento con Windows PowerShell, abra PowerShell, escriba el siguiente comando y, a continuación, presione ENTRAR.  
`Install-WindowsFeature Web-Server -IncludeManagementTools`  

1.  En el Administrador del servidor, haga clic en **Administrar** y en **Agregar roles y características**. Se abre el Asistente para agregar roles y características.  
2.  En **Antes de comenzar**, haga clic en **Siguiente**.  

**Nota**   
El **antes de comenzar** no se muestra la página del Asistente de las características y agregar Roles si ya ha ejecutado el agregar Roles y características Asistente y seleccionó **omitir esta página predeterminada** en ese momento.  

3. En la página **Tipo de instalación**, haga clic en **Siguiente**.  
4. En el **selección de servidor** página, haga clic en **siguiente**.  
5. En el **roles de servidor** página, seleccione **servidor Web (IIS)** y, a continuación, haga clic en **siguiente**.  
6. Haga clic en **Siguiente** hasta haber aceptado todas las configuraciones predeterminadas del servidor web y, a continuación, haga clic en **Instalar**.  
7. Compruebe que todas las instalaciones se realizaron correctamente y, a continuación, haga clic en **Cerrar**.
