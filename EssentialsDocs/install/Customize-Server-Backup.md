---
title: Personalizar copias de seguridad del servidor
description: "Describe cómo usar Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 19b2559c-6090-45af-9a08-2eefc28473c8
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: d18dca276bccdf672664a5a3c2bd28e0221fff94
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="customize-server-backup"></a>Personalizar copias de seguridad del servidor

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

## <a name="turn-off-server-backup-by-default"></a>Desactivar la copia de seguridad del servidor de manera predeterminada  
 Puedes desactivar la copia de seguridad del servidor de manera predeterminada. Debes establecer el valor de **HKEY_LOCAL_MACHINE\Software\Microsoft\Windows Server\ServerBackup\ProviderDisabled** en 1 para habilitar esta opción.  
  
 Cuando se establece esta clave, la interfaz de usuario de copia de seguridad del servidor no se mostrará a través del panel o del Launchpad. Esto te permite usar aplicaciones de terceros para la copia de seguridad del servidor.  
  
#### <a name="to-add-serverbackupproviderdisabled-registry-key-and-set-the-value-to-1"></a>¿Para agregar ServerBackup\ProviderDisabled? clave del registro y establece el valor en 1  
  
1.  En el servidor, haz clic en **inicio**, haz clic en **ejecutar**, tipo **regedit** en la **abrir** textbox y, a continuación, haz clic en **Aceptar**.  
  
2.  En el panel de navegación, expande **HKEY_LOCAL_MACHINE**, expanda **SOFTWARE**, expanda **Microsoft**, expanda **Windows Server**y, a continuación, expanda **ServerBackup**.  
  
3.  Haz clic en **ServerBackup**, haz clic en **nueva**y, a continuación, haz clic en **valor DWARD**.  
  
4.  Como nombre, escribe **ProviderDisabled**.  
  
5.  Haz clic en el nombre, selecciona **modificar**, escribe **1** para la información del valor y, a continuación, haz clic en **Aceptar**.  
  
## <a name="turn-on-server-backup"></a>Activar la copia de seguridad del servidor  
 Puedes activar copia de seguridad de servidor si se ha desactivado creando **ProviderDisabled** clave de registro (como se describió anteriormente en este documento).  
  
 Elimina la clave **HKEY_LOCAL_MACHINE\Software\Microsoft\Windows Server\ServerBackup\ProviderDisabled** con el fin de habilitar la copia de seguridad de servidor de forma predeterminada, cambiar el tipo de inicio de servicio de servicio copia de seguridad de servidor de Windows Server y reiniciar el servidor.  
  
#### <a name="to-delete-serverbackupproviderdisabled-registry-key"></a>¿Para eliminar ServerBackup\ProviderDisabled? clave del registro  
  
1.  En el servidor, mueve el mouse a la esquina superior derecha de la pantalla y haz clic en **búsqueda**.  
  
2.  En el cuadro de búsqueda, escriba **regedit**y, a continuación, haz clic en el **Regedit** aplicación.  
  
3.  En el panel de navegación, expande **HKEY_LOCAL_MACHINE**, expanda **SOFTWARE**, expanda **Microsoft**, expanda **Windows Server**y, a continuación, expanda **ServerBackup**.  
  
4.  Haz clic en **ProviderDisabled**y, a continuación, haz clic en **eliminar**.  
  
#### <a name="change-the-start-type-of-windows-server-server-backup-service"></a>Cambiar el tipo de inicio del servicio de copia de seguridad de servidor de Windows Server  
  
1.  En el servidor, mueve el mouse a la esquina superior derecha de la pantalla y haz clic en **búsqueda**.  
  
2.  En el cuadro de búsqueda, escriba **services.msc**y, a continuación, haz clic en **servicios** aplicación.  
  
3.  En el panel de servicios, haz clic en el **servicio de copia de seguridad de servidor de Windows Server**y haz clic en **propiedades**.  
  
4.  En **General** , selecciona **automática** para **tipo de inicio**.  
  
5.  Haz clic en **Aceptar** para cerrar el cuadro de diálogo.  
  
#### <a name="restart-the-server"></a>Reiniciar el servidor  
  
1.  En el servidor, mueve el mouse a la esquina superior derecha de la pantalla, haz clic en **configuración**, haz clic en **energía**y, a continuación, haga clic en reiniciar.