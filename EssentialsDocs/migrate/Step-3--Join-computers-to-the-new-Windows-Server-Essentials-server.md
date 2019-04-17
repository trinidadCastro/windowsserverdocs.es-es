---
title: 'Paso 3: Unirte a equipos en el nuevo servidor de Windows Server Essentials'
description: "Describe cómo usar Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a0e07d1a-8409-429b-87d7-0f4a7e14d668
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: f71ac280e2de0b7d945f2d979fe52d173f7c3323
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="step-3-join-computers-to-the-new-windows-server-essentials-server"></a>Paso 3: Unirte a equipos en el nuevo servidor de Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

El siguiente paso del proceso de migración es conectar equipos cliente al servidor nuevo que ejecuta Windows Server Essentials.  
  
> [!NOTE]
>  Puedes omitir este paso para equipos que ejecutan los sistemas de operativos de Windows Vista o Windows XP. El software del conector de servidor de Windows no es compatible con equipos que ejecutan Windows XP o Windows Vista.  
  
 Antes de que puedes unirte a un equipo cliente al servidor de Windows Server Essentials de nuevo, debe desconectar desde el servidor de origen al desinstalar el software del conector de servidor de Windows en el equipo cliente.  
  
### <a name="to-uninstall-windows-server-connector-on-a-client-computer"></a>Para desinstalar el conector de Windows Server en un equipo cliente  
  
1.  Desde un equipo cliente, abre el Panel de Control y, a continuación, abre **programas y características**.  
  
2.  En la lista de programas, haz clic en la aplicación de la conexión que se ejecuta en el equipo.  
  
    > [!NOTE]
    >  La aplicación de conexión puede ser **conector de Windows Small Business Server 2011 Essentials**, o **conector de Windows Server Essentials**, dependiendo de qué versión de Windows Server Essentials se ha conectado el equipo cliente.  
  
3.  Haz clic en **desinstalar**.  
  
### <a name="to-reconnect-a-client-computer-to-the-server"></a>Para volver a conectar un equipo cliente al servidor  
  
1.  Inicia sesión en el equipo que desea conectarse al servidor.  
  
    > [!NOTE]
    >  Si este equipo tiene varias cuentas de usuario, inicia sesión con la cuenta de usuario que tiene documentos, imágenes y las preferencias del usuario que quieras conservar después de conectar el equipo al servidor.  
  
2.  Abre un explorador de Internet, como Internet Explorer.  
  
3.  En la barra de direcciones, escriba **http://<servername\>/Connect**, y, a continuación, presione ENTRAR.  
  
4.  Sigue las instrucciones en pantalla para unir el equipo cliente al servidor de Windows Server Essentials de nuevo.  
  
## <a name="next-steps"></a>Pasos siguientes  
 Se han unido a los equipos cliente al servidor nuevo que ejecuta Windows Server Essentials. Ahora ve a [paso 4: mover la configuración y los datos a la migración de servidor de destino para Windows Server Essentials](Step-4--Move-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md).  
  

Para ver todos los pasos, consulta [migrar a Windows Server Essentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md).

