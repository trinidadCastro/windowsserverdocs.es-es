---
title: Configurar la protección de disco en MultiPoint Services
description: Obtenga información sobre cómo configurar la protección de disco de MultiPoint Services
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
ms.openlocfilehash: dc0a46b57753f08cc7d79fd05de7a9e81469cc86
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820176"
---
# <a name="configure-disk-protection"></a>Configurar la protección de disco
Puede usar la protección de disco en Multipoint Services para proteger el volumen del sistema de actualizaciones no deseadas, programar actualizaciones de Windows que se conservarán mientras está activa, para deshabilitar temporalmente la protección de disco y desinstalar la protección de disco de la protección de disco.  
  
Habilitar la protección de disco en MultiPoint Services, puede proteger el volumen del sistema (la unidad donde está instalado Windows, que normalmente C:) de cambios no deseados. Cuando se habilita la protección de disco, los cambios realizados en el volumen del sistema se almacenan en una ubicación temporal para que simplemente reiniciando el equipo descarta y devuelve automáticamente el sistema al estado anterior funcione correctamente.  
  
El administrador puede instalar software o realizar cambios de configuración por deshabilitar temporalmente la protección de disco. Con el fin de mantener el sistema actual con las actualizaciones de Windows y las definiciones de antimalware, protección de disco programa una ventana de mantenimiento para descargar e instalar las actualizaciones. El administrador también puede proporcionar un script personalizado para ejecutarse durante el período de mantenimiento para acomodar las necesidades de mantenimiento más allá de Windows Update.  
  
## <a name="enable-disk-protection"></a>Habilitar la protección de disco  
Antes de habilitar la protección de disco, asegúrese de que todas las aplicaciones y los controladores están instalados y actualizados y mueva los perfiles de usuario a un volumen que no se protegerán. Si necesita realizar actualizaciones manuales después de habilitar la protección de disco, puede deshabilitar temporalmente la protección de disco. Sin embargo, resulta más fácil de obtener el sistema en un estado ideal antes de que se activa la protección de disco.  
  
 
1.  Inicie sesión en el servidor que ejecuta MultiPoint Services como administrador.  
  
2.  Antes de habilitar la protección de disco:  
  
    -   Asegúrese de que el sistema es exactamente el estado en el que desea que permanezca de MultiPoint Services. Por ejemplo, asegúrese de que el software instalado, configuración del sistema y las actualizaciones son correctas.  
  
    -   Mueva los perfiles de usuario a un volumen que no está protegido, o configurar una ubicación de archivo compartido desactivado el volumen del sistema, como se describe en [habilite compartir en MultiPoint Services archivos](Enable-file-sharing-in-MultiPoint-services.md).  
  
3.  Desde el **iniciar** pantalla, abra **MultiPoint Manager**.  
  
4.  Haga clic en el **inicio** , haga clic **habilitar la protección de disco**y, a continuación, haga clic en **Aceptar**.  
  
Cuando se habilita la protección de disco por primera vez, el sistema está preparado por instalar a un controlador y la creación de un archivo caché en el volumen del sistema. El archivo caché almacenará temporalmente los cambios realizados en el volumen del sistema mientras está activa la protección de disco. Dado que las actualizaciones del sistema se almacenan en el archivo de caché, no modifique el contenido protegido del volumen fuera del archivo de caché. Cada vez que se inicia el sistema, se restablece el archivo de caché, que descarta los cambios almacenados en ella desde el inicio del sistema anterior. Por lo tanto, el sistema siempre se inicia en el mismo estado que cuando se habilitó la protección de disco.  
  
Windows necesita actualizar algunos archivos del sistema: incluidos el archivo de paginación del sistema, ubicación del volcado de bloqueo y los registros de eventos. Esos archivos no se descartan cuando se habilita la protección de disco. Para lograr esto, se crea un nuevo volumen denominado DpReserved cuando está habilitada la protección de disco por primera vez, y esos archivos se mueven a ese volumen. La partición DpReserved no está protegida, por lo persisten escrituras a esos archivos a través de reinicios, incluso cuando está habilitada la protección de disco.  
  
## <a name="schedule-software-updates"></a>Programar actualizaciones de software  
Si Windows está configurado para instalar automáticamente las actualizaciones de Windows, protección de disco permite que estas actualizaciones en el tiempo configurado y descartar las actualizaciones. Por ejemplo, si las actualizaciones de Windows se programan para 3:00 a.m., protección de disco busca actualizaciones cada día a las 3:00 a.m. Si se encuentra alguna actualización, MultiPoint Services deshabilita temporalmente la protección de disco, aplica las actualizaciones y, a continuación, vuelve a habilitar la protección de disco.  
   
1.  En el Administrador de MultiPoint, mostrar el **inicio** pestaña y, a continuación, haga clic en **programar actualizaciones de software**.  
  
2.  En el cuadro de diálogo actualizaciones de Software de programación, haga clic en **actualizar en**y seleccione una hora para actualizaciones de: por ejemplo, **3:00 AM**.  
  
3.  Seleccione el **ejecutar Windows Update** casilla de verificación.  
  
4.  Si su organización ejecuta su propio script de actualización, seleccione el **ejecutar el siguiente programa** casilla y especifique la ubicación del script de actualización de su organización.  
  
5.  Seleccione un tiempo máximo para permitir las actualizaciones ejecutar.  
  
6.  En **cuando termine**, elija si desea que el sistema vuelva a su estado anterior de energía o cierra correctamente después de aplicar las actualizaciones.  
  
7.  Haga clic en **Aceptar**.  
  
## <a name="temporarily-disable-disk-protection"></a>Deshabilitar temporalmente la protección de disco  
Si un administrador necesita instalar software, cambiar la configuración del sistema o realizar otras tareas de mantenimiento que implican actualizaciones del sistema, puede deshabilitar temporalmente la protección de disco. Una vez realizados los cambios, vuelva a habilitar la protección de disco. Durante los reinicios del sistema, el sistema conserva su estado cuando se habilitó la protección de disco.  
    
1.  En el Administrador de MultiPoint, haga clic en el **inicio** ficha.  
  
2.  En la pestaña Inicio, haga clic en **deshabilitar protección de disco**y, a continuación, haga clic en **Aceptar**.  
  
> [!NOTE]  
> No olvide volver a habilitar la protección de disco una vez completado el mantenimiento. El sistema no estarán protegido de nuevo hasta que el administrador explícitamente vuelve a habilita protección de disco.  
  
## <a name="uninstall-disk-protection"></a>Desinstalar la protección de disco  
Protección de disco de desinstalación quita el controlador y el archivo caché, por lo que solo debe hacer esto si desea dejar de usar la protección de disco a largo plazo. Si simplemente desea realizar el mantenimiento o detener temporalmente la protección, use la tarea de protección de disco Disable en su lugar.  
  
Protección de disco, puede desinstalar si está habilitado o deshabilitado.  
   
1.  En el Administrador de MultiPoint, haga clic en el **inicio** ficha.  
  
2.  En la pestaña Inicio y haga clic en **desinstalar la protección de disco**y, a continuación, haga clic en **Aceptar**.  
  
    Tras hacer clic en **Aceptar**, el equipo se reinicia. El proceso de desinstalación requiere varios reinicios, durante el cual se quitan el controlador y el archivo de caché. La partición se conservará DpReserved y el archivo de paginación ubicación del volcado de bloqueo y registro de eventos de los archivos permanecen configurado para usar la partición DpReserved.