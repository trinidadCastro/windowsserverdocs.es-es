---
title: Solución de problemas del compilador de registros de Windows Server Essentials
description: Describe cómo usar Windows Server Essentials
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
ms.openlocfilehash: 3e6d6fbd003b20214f9c8194be7b1584de097984
ms.sourcegitcommit: 39244de670f712857a5fdd56630e95d57b7001a5
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/06/2019
ms.locfileid: "74897661"
---
# <a name="troubleshoot-windows-server-essentials-log-collector-errors"></a>Solución de problemas del compilador de registros de Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Al ejecutar el Compilador de registros, puede encontrar uno de los siguientes errores. Para resolver un problema, siga las instrucciones indicadas para el error en cuestión.  
  
> [!NOTE]
> En este documento, los equipos de la red que no sean el servidor, se denominan equipos de red.
  
###  <a name="BKMK_TheDestinationFolderIsNotValid"></a>La carpeta de destino no es válida  
 **Causa:** la carpeta donde está intentando copiar los archivos de registro no existe o no tiene suficiente espacio para almacenar los archivos.  
  
 **Solución:** compruebe que exista la carpeta seleccionada y que haya suficiente espacio libre disponible en la unidad para los archivos. También debe asegurarse de que haya suficiente espacio libre restante en la carpeta temporal de la unidad.  
  
###  <a name="BKMK_ANetworkErrorHasOccurred"></a>Se ha producido un error de red  
 **Causa:** puede haber un problema relacionado con la red en el equipo de red o el servidor.  
  
 **Solución:** asegúrese de que todos los equipos y dispositivos de red estén encendidos y conectados a la red. Si no puede resolver el problema, póngase en contacto con la persona que mantiene la red para obtener ayuda.  
  
###  <a name="BKMK_CannotCollectLogFiles"></a>No se pueden recopilar archivos de registro para el equipo  
 **Causa:** es posible que el Compilador de registros no pueda instalarse en el equipo porque el equipo no se conectó correctamente al servidor mediante el asistente de conexión del equipo al servidor.  
  
 **Solución:** Para obtener información sobre cómo resolver problemas relacionados con las conexiones con el servidor, consulte [solucionar problemas de conexión de equipos al servidor](https://go.microsoft.com/fwlink/p/?LinkID=241492).  
  
 Si aún no puede conectar el equipo al servidor, puede copiar los archivos del registro manualmente en una unidad flash USB de la siguiente manera:  
  
-   Para los equipos cliente que ejecutan Windows 7, Windows 8 o Windows Multipoint Server, puede copiar la carpeta **Logs** ubicada en **%sysdir%\programdata\Microsoft\Windows Server**.  
  
###  <a name="BKMK_YouDoNotHavePermission"></a>No tiene permiso para guardar los archivos de registro en la carpeta seleccionada  
 **Causa:** es posible que no tenga permisos de escritura en la carpeta que ha seleccionado para guardar los archivos de registro.  
  
 **Solución:** Si usa la ruta de acceso predeterminada para guardar los archivos de registro, asegúrese de que tiene permiso de escritura para la carpeta compartida **\\\\< ServerName\>\Logs**. Si va a almacenar los registros en un equipo de red, asegúrese de tener permiso de escritura para la carpeta que ha seleccionado para guardar los archivos de registro.  
  
###  <a name="BKMK_TheComputerIsNotConfiguredProperly"></a>El equipo no está configurado correctamente para recopilar los archivos de registro  
 **Causa:** el equipo no se ha configurado correctamente para el Compilador de registros.  
  
 **Solución:** reinstale el Compilador de registros. Consulte [Reinstalling the Log Collector](Install-the-Windows-Server-Essentials-Log-Collector.md#BKMK_Reinstall).  
  
###  <a name="BKMK_AnUnknownErrorOccurred"></a>Se produjo un error desconocido  
 **Causa:** desconocida.  
  
 **Solución 1:** vuelva a ejecutar el Compilador de registros. Si el error se produce de nuevo, asegúrese de que no haya ningún problema de conectividad. También puede intentar reinstalar el Compilador de registros. Consulte [Reinstalling the Log Collector](Install-the-Windows-Server-Essentials-Log-Collector.md#BKMK_Reinstall). Si no puede resolver el problema, póngase en contacto con la persona que mantiene la red para obtener ayuda.  
  
 **Solución 2:** en primer lugar, intente abrir la carpeta donde se guardarán los archivos de registro. Si ya se genera el archivo zip con el nombre del equipo, omita este error y use los archivos de registro en su lugar. Si no se ha generado ningún archivo de registro, vuelva a ejecutar el Compilador de registros. Si el error se produce de nuevo, asegúrese de que no haya ningún problema de conectividad. También puede intentar reinstalar el Compilador de registros. Consulte [Reinstalling the Log Collector](Install-the-Windows-Server-Essentials-Log-Collector.md#BKMK_Reinstall). Si no puede resolver el problema, póngase en contacto con la persona que mantiene la red para obtener ayuda.