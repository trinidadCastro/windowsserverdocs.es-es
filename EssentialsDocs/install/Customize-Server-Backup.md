---
title: Personalización de la copia de seguridad del servidor
description: Obtenga información acerca de cómo personalizar la configuración de copia de seguridad del servidor para Windows Server Essentials.
ms.date: 10/03/2016
ms.topic: article
ms.assetid: 19b2559c-6090-45af-9a08-2eefc28473c8
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: 5558364d21374649cc57e28f9df1331a59428b7b
ms.sourcegitcommit: d2224cf55c5d4a653c18908da4becf94fb01819e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/21/2020
ms.locfileid: "97711090"
---
# <a name="customize-server-backup"></a>Personalización de la copia de seguridad del servidor

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

## <a name="turn-off-server-backup-by-default"></a>Desactivar Copias de seguridad de Windows Server de forma predeterminada.
 Puede optar por desactivar Copias de seguridad de Windows Server de forma predeterminada. Debe definir también el valor de **HKEY_LOCAL_MACHINE\Software\Microsoft\Windows Server\ServerBackup\ProviderDisabled** como 1 para habilitar esta opción.

 Cuando se establece esta clave, la interfaz de Usuario de copia de seguridad de Windows Server no se mostrará mediante el Panel o Launchpad. Esto le permite utilizar aplicaciones de terceros para Copias de seguridad de Windows Server.

#### <a name="to-add-serverbackupproviderdisabled-registry-key-and-set-the-value-to-1"></a>Para agregar ServerBackup\ProviderDisabled? clave del registro y establezca el valor en 1.

1.  En el servidor, haga clic en **Inicio**, **Ejecutar**, escriba **regedit** en el cuadro de texto **Abrir** y haga clic en **Aceptar**.

2.  En el panel de navegación, expanda **HKEY_LOCAL_MACHINE**, **SOFTWARE**, **Microsoft**, **Windows Server** y, finalmente, **ServerBackup**.

3.  Haga clic con el botón secundario en **ServerBackup**, haga clic en **Nuevo** y, a continuación, en **Valor DWWARD**.

4.  Para el nombre, escriba **ProviderDisabled**.

5.  Haga clic con el botón secundario sobre el nombre, seleccione **Modificar**, escriba **1** en el campo de datos de valor y después haga clic en **Aceptar**.

## <a name="turn-on-server-backup"></a>Activar Copias de seguridad de Windows Server
 Si Copias de seguridad de Windows Server estaba desactivada, puede activarla si crea la clave de registro **ProviderDisabled** (como se describe anteriormente en este documento).

 Debe eliminar la clave **HKEY_LOCAL_MACHINE\Software\Microsoft\Windows Server\ServerBackup\ProviderDisabled** para poder habilitar la copia de seguridad de servidor de forma predeterminada, cambiar el tipo de inicio del Servicio de copia de seguridad del servidor para Windows Server y reiniciar el servidor.

#### <a name="to-delete-serverbackupproviderdisabled-registry-key"></a>Para eliminar ServerBackup\ProviderDisabled? clave del registro

1.  En el servidor, mueva el ratón a la esquina superior derecha de la pantalla y haga clic en **Buscar**.

2.  En el cuadro de búsqueda, escriba **regedit** y después haga clic en la aplicación **Regedit**.

3.  En el panel de navegación, expanda **HKEY_LOCAL_MACHINE**, **SOFTWARE**, **Microsoft**, **Windows Server** y, finalmente, **ServerBackup**.

4.  Haga clic con el botón secundario en **ProviderDisabled** y, a continuación, haga clic en **Eliminar**.

#### <a name="change-the-start-type-of-windows-server-server-backup-service"></a>Cambie el tipo de inicio del Servicio de copia de seguridad del servidor para Windows Server

1.  En el servidor, mueva el ratón a la esquina superior derecha de la pantalla y haga clic en **Buscar**.

2.  En el cuadro de búsqueda, escriba **services.msc** y, a continuación, haga clic en la aplicación **Services**.

3.  En el panel de servicios, haga clic con el botón secundario en el **Servicio de copia de seguridad del servidor para Windows Server** y haga clic en **Propiedades**.

4.  En la ficha **General**, seleccione **Automático** para **Tipo de inicio**.

5.  Haga clic en **Aceptar** para cerrar el cuadro de diálogo.

#### <a name="restart-the-server"></a>Reinicio del servidor

1.  En el servidor, mueva el ratón a la esquina superior derecha de la pantalla, haga clic en **Configuración** y **Energía** y, a continuación, en Reiniciar.