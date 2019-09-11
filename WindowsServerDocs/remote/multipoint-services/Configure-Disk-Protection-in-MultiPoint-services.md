---
title: Configurar la protección de disco en MultiPoint Services
description: Obtenga información sobre cómo configurar la protección de disco para Multipoint Services
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: bd9bf5b9-e481-499b-9c15-7ee5a4f470c4
author: evaseydl
manager: scottman
ms.author: evas
ms.date: 08/04/2016
ms.openlocfilehash: 867848b65b02b6a7436fc5c86ba796a1b42aec42
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871738"
---
# <a name="configure-disk-protection"></a>Configurar la protección de disco
Puede usar la protección de disco en Multipoint Services para proteger el volumen del sistema de las actualizaciones imprevistas, programar las actualizaciones de Windows que se van a conservar mientras la protección de disco está activa, deshabilitar temporalmente la protección de disco y desinstalar la protección de disco.  
  
Al habilitar la protección de disco en Multipoint Services, puede proteger el volumen del sistema (la unidad donde está instalado Windows, normalmente C:) de cambios no deseados. Cuando está habilitada la protección de disco, los cambios realizados en el volumen del sistema se almacenan en una ubicación temporal, de modo que el reinicio del equipo lo descartará y automáticamente devolverá el sistema al estado correcto conocido anterior.  
  
El administrador puede instalar software fácilmente o realizar cambios en la configuración deshabilitando temporalmente la protección de disco. Con el fin de mantener el sistema al día con las actualizaciones de Windows y las definiciones de antimalware, protección de disco programa una ventana de mantenimiento para descargar e instalar actualizaciones. Además, el administrador puede proporcionar un script personalizado que se ejecutará durante la ventana de mantenimiento para adaptarse a las necesidades de mantenimiento más allá de Windows Update.  
  
## <a name="enable-disk-protection"></a>Habilitar protección de disco  
Antes de habilitar la protección de disco, asegúrese de que todas las aplicaciones y controladores estén instalados y actualizados, y mueva los perfiles de usuario a un volumen que no se protegerá. Si necesita hacer actualizaciones manuales después de habilitar la protección de disco, puede deshabilitar temporalmente la protección de disco. Sin embargo, es más fácil poner el sistema en un estado ideal antes de activar la protección de disco.  
  
 
1.  Inicie sesión en el servidor que ejecuta Multipoint Services como administrador.  
  
2.  Antes de habilitar la protección de disco:  
  
    -   Asegúrese de que el sistema Multipoint Services esté exactamente en el estado en el que desea que permanezca. Por ejemplo, asegúrese de que el software instalado, la configuración del sistema y las actualizaciones son correctas.  
  
    -   Mueva los perfiles de usuario a un volumen que no esté protegido o configure una ubicación de archivos compartidos fuera del volumen del sistema, tal como se describe en [habilitación del uso compartido de archivos en Multipoint Services](Enable-file-sharing-in-MultiPoint-services.md).  
  
3.  En la pantalla **Inicio** , Abra **Multipoint Manager**.  
  
4.  Haga clic en la pestaña **Inicio** , haga clic en **Habilitar protección de disco**y, a continuación, haga clic en **Aceptar**.  
  
Cuando se habilita la protección de disco por primera vez, el sistema se prepara instalando un controlador y creando un archivo caché en el volumen del sistema. El archivo de caché almacenará temporalmente cualquier cambio realizado en el volumen del sistema mientras la protección de disco esté activa. Dado que las actualizaciones del sistema se almacenan en el archivo caché, no modifican el contenido protegido del volumen fuera del archivo caché. Cada vez que se inicia el sistema, se restablece el archivo de caché, lo que descarta los cambios almacenados desde el inicio del sistema anterior. Por lo tanto, el sistema siempre se inicia en el mismo estado que cuando se habilitó la protección de disco.  
  
Windows necesita actualizar algunos archivos del sistema, incluidos el archivo de paginación del sistema, la ubicación de los volcados de memoria y los registros de eventos. Estos archivos no se descartan cuando está habilitada la protección de disco. Para ello, se crea un nuevo volumen denominado DpReserved cuando la protección de disco se habilita por primera vez, y esos archivos se mueven a ese volumen. La partición DpReserved no está protegida, por lo que las escrituras en esos archivos se conservan a través de los reinicios, incluso cuando está habilitada la protección de disco.  
  
## <a name="schedule-software-updates"></a>Programar actualizaciones de software  
Si Windows está configurado para instalar automáticamente actualizaciones de Windows, la protección de disco permite estas actualizaciones en el momento configurado y no las descarta. Por ejemplo, si las actualizaciones de Windows están programadas para las 3:00 a.m., protección de disco busca actualizaciones cada día a las 3:00 a.m. Si se encuentran actualizaciones, Multipoint Services deshabilita temporalmente la protección de disco, aplica las actualizaciones y, a continuación, vuelve a habilitar la protección de disco.  
   
1.  En Multipoint Manager, abra la pestaña **Inicio** y haga clic en **programar actualizaciones de software**.  
  
2.  En el cuadro de diálogo programar actualizaciones de software, haga clic en **actualizar en**y seleccione una hora para las actualizaciones; por ejemplo, **3:00 AM**.  
  
3.  Active la casilla **ejecutar Windows Update** .  
  
4.  Si su organización ejecuta su propio script de actualización, active la casilla **ejecutar el siguiente programa** y especifique la ubicación del script de actualización de su organización.  
  
5.  Seleccione un tiempo máximo para permitir que se ejecuten las actualizaciones.  
  
6.  En **cuando termine**, elija si el sistema volverá a su estado de energía anterior o se apagará después de aplicar las actualizaciones.  
  
7.  Haga clic en **Aceptar**.  
  
## <a name="temporarily-disable-disk-protection"></a>Deshabilitar temporalmente la protección de disco  
Si un administrador necesita instalar software, cambiar la configuración del sistema o realizar otras tareas de mantenimiento que impliquen actualizaciones del sistema, pueden deshabilitar temporalmente la protección de disco. Una vez realizados los cambios, vuelva a habilitar la protección de disco. Durante el reinicio del sistema, el sistema conservará su estado cuando se haya habilitado la protección de disco.  
    
1.  En Multipoint Manager, haga clic en la pestaña **Inicio** .  
  
2.  En la pestaña Inicio, haga clic en **deshabilitar protección de disco**y, a continuación, haga clic en **Aceptar**.  
  
> [!NOTE]  
> Recuerde volver a habilitar la protección de disco una vez completado el mantenimiento. El sistema no se protegerá de nuevo hasta que el administrador vuelva a habilitar explícitamente la protección de disco.  
  
## <a name="uninstall-disk-protection"></a>Desinstalar protección de disco  
Al desinstalar la protección de disco, se quita el controlador y el archivo de caché, por lo que solo debe hacerlo si desea dejar de usar la protección de disco a largo plazo. Si simplemente desea realizar el mantenimiento o detener temporalmente la protección, utilice en su lugar la tarea deshabilitar protección de disco.  
  
Puede desinstalar la protección de disco si está habilitada o deshabilitada.  
   
1.  En Multipoint Manager, haga clic en la pestaña **Inicio** .  
  
2.  En la pestaña Inicio, haga clic en **desinstalar protección de disco**y, a continuación, haga clic en **Aceptar**.  
  
    Después de hacer clic en **Aceptar**, el equipo se reinicia. El proceso de desinstalación requiere varios reinicios, durante los cuales se quitan el controlador y el archivo de caché. La partición DpReserved permanece y el archivo de paginación, la ubicación de volcado de memoria y los archivos de registro de eventos permanecen configurados para usar la partición DpReserved.