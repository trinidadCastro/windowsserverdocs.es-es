---
title: Instalar la WEB1 de servidor web
description: Este tema forma parte de la guía de implementación de certificados de servidor para las implementaciones cableadas e inalámbricas de 802.1 X
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: f51c9e38-98bb-49c1-9d39-427d07021499
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6ad8106c9c8330dd1b8632b3672d6413c1a1faaf
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356217"
---
# <a name="install-the-web-server-web1"></a>Instalar la WEB1 de servidor web

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

El rol de servidor Web (IIS) en Windows Server 2016 proporciona una plataforma segura, fácil de administrar, modular y extensible para hospedar sitios web, servicios y aplicaciones de forma confiable. Con IIS, puede compartir información con usuarios en Internet, en una intranet o en una extranet. IIS es una plataforma web unificada que integra IIS, ASP.NET, servicios FTP, PHP y Windows Communication Foundation (WCF).  

Al implementar certificados de servidor, el servidor web proporciona una ubicación en la que puede publicar la lista de revocación de certificados (CRL) de la entidad de certificación (CA). Después de la publicación, la CRL es accesible para todos los equipos de la red, de modo que puedan usar esta lista durante el proceso de autenticación para comprobar que no se revocan los certificados presentados por otros equipos.   

Si un certificado se encuentra en la CRL como revocado, se produce un error en el trabajo de autenticación y el equipo está protegido contra la confianza de una entidad que tiene un certificado que ya no es válido.  

Antes de instalar el rol de servidor Web (IIS), asegúrese de que ha configurado el nombre del servidor y la dirección IP, y de que ha unido el equipo al dominio.  

## <a name="to-install-the-web-server-iis-server-role"></a>Para instalar el rol de servidor web (IIS)  
Para completar este procedimiento, debe pertenecer al grupo **Administradores**.  

>[!NOTE]  
>Para llevar a cabo este procedimiento con Windows PowerShell, abra PowerShell, escriba el siguiente comando y, a continuación, presione Entrar.  
`Install-WindowsFeature Web-Server -IncludeManagementTools`  

1.  En el Administrador del servidor, haga clic en **Administrar** y en **Agregar roles y características**. Se abre el Asistente para agregar roles y características.  
2.  En **Antes de comenzar**, haga clic en **Siguiente**.  

**Nota**   
La página **antes de comenzar** del Asistente para agregar roles y características no se muestra si previamente ejecutó el Asistente para agregar roles y características y seleccionó **omitir esta página de forma predeterminada** en ese momento.  

3. En la página **Tipo de instalación**, haga clic en **Siguiente**.  
4. En la página **selección de servidor** , haga clic en **siguiente**.  
5. En la página **roles de servidor** , seleccione **servidor Web (IIS)** y, a continuación, haga clic en **siguiente**.  
6. Haga clic en **Siguiente** hasta haber aceptado todas las configuraciones predeterminadas del servidor web y, a continuación, haga clic en **Instalar**.  
7. Compruebe que todas las instalaciones se realizaron correctamente y, a continuación, haga clic en **Cerrar**.
