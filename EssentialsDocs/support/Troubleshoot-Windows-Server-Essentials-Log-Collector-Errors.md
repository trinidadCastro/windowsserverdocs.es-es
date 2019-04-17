---
title: Solucionar errores de selector de registro de Windows Server Essentials
description: "Describe cómo usar Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fa2e1685-31c0-4d4f-a10a-6c8885dfc493
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: e92236c8e5d956b50f657ebcbe1a942b5841fded
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="troubleshoot-windows-server-essentials-log-collector-errors"></a>Solucionar errores de selector de registro de Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Cuando se ejecuta el selector de registro, puede encontrar uno de los siguientes errores. Para resolver un problema, sigue las instrucciones proporcionadas para el error asociado.  
  
> [!NOTE]
>  ¿En este documento, los equipos de la red, que no sean de tu servidor, se denominan equipos de la red.?  
  
###  <a name="BKMK_TheDestinationFolderIsNotValid"></a>La carpeta de destino no es válida  
 **Causa:** la carpeta donde está intentando copiar los archivos de registro no exista o que no tenga suficiente espacio para contener los archivos.  
  
 **Solución:** comprobar que la carpeta seleccionada existe y que hay suficiente espacio libre disponible en la unidad para los archivos. También debes asegurarte de que hay suficiente espacio disponible en la carpeta temporal en la unidad.  
  
###  <a name="BKMK_ANetworkErrorHasOccurred"></a>Se ha producido un error de red  
 **Causa:** puede haber un problema relacionado con la red en el equipo de la red o el servidor.  
  
 **Solución:** Asegúrese de que todos los equipos y dispositivos en red están encendidos, y que estén conectados a la red. Si no puede resolver el problema, ponte en contacto con la persona que mantiene la red para obtener ayuda.  
  
###  <a name="BKMK_CannotCollectLogFiles"></a>No se puede recopilar archivos de registro para el equipo  
 **Causa:** el recolector de registro no se instale en el equipo porque el equipo no se conectó correctamente al servidor con el equipo de conectarse al Asistente de servidor.  
  
 **Solución:** para obtener información sobre cómo resolver problemas relacionados con las conexiones a tu servidor, consulte [solucionar problemas de conexión de equipos en el servidor](https://go.microsoft.com/fwlink/p/?LinkID=241492)? (https://go.microsoft.com/fwlink/p/?LinkID=241492) en el sitio Web de Microsoft.  
  
 Si estás sigues sin poder conectarse al servidor en el equipo, a continuación, puedes copiar los archivos de registro manualmente a una unidad flash USB como sigue:  
  
-   Para los equipos cliente que ejecutan Windows 7, Windows 8 o Windows Multipoint Server, puedes copiar la **registros** carpeta ubicada en **%sysdir%\programdata\Microsoft\Windows Server**.  
  
###  <a name="BKMK_YouDoNotHavePermission"></a>No tienes permiso para guardar los archivos de registro en la carpeta seleccionada  
 **Causa:** puede que no tengas permiso de escritura a la carpeta seleccionada para guardar los archivos de registro.  
  
 **Solución:** si estás usando la ruta de acceso predeterminada para guardar archivos de registro, asegúrate de tener permisos de escritura para la carpeta compartida **\\\ < ServerName\ > \Logs**. Si va a almacenar registros en un equipo de red, asegúrate de que tienes permisos de escritura para la carpeta seleccionada para guardar los archivos de registro.  
  
###  <a name="BKMK_TheComputerIsNotConfiguredProperly"></a>El equipo no está configurado correctamente para recopilar los archivos de registro  
 **Causa:** el equipo no se ha configurado correctamente para el selector de registro.  
  
 **Solución:** volver a instalar el selector de registro. Consulta [reinstalar el recolector de registro](Install-the-Windows-Server-Essentials-Log-Collector.md#BKMK_Reinstall).  
  
###  <a name="BKMK_AnUnknownErrorOccurred"></a>Se ha producido un error desconocido  
 **Causa:** desconocido.  
  
 **Solución de 1:** volver a ejecutar el recolector de registro. Si el error se repite, asegúrate de que no hay ningún problema de conectividad. También puedes intentar volver a instalar al selector de registro. Consulta [reinstalar el recolector de registro](Install-the-Windows-Server-Essentials-Log-Collector.md#BKMK_Reinstall). Si no puede resolver el problema, ponte en contacto con la persona que mantiene la red para obtener ayuda.  
  
 **Solución de 2:** en primer lugar, intenta abrir la carpeta donde se guardan los archivos de registro. Si ya se genera el archivo zip con nombre de equipo, ignorar este error y usa los archivos de registro. Si no hay ningún archivo de registro generado, vuelve a ejecutar el recolector de registro. Si el error se repite, asegúrate de que no hay ningún problema de conectividad. También puedes intentar volver a instalar al selector de registro. Consulta [reinstalar el recolector de registro](Install-the-Windows-Server-Essentials-Log-Collector.md#BKMK_Reinstall). Si no puede resolver el problema, ponte en contacto con la persona que mantiene la red para obtener ayuda.