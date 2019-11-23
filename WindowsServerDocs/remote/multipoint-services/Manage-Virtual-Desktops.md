---
title: Administrar escritorios virtuales
description: Aprenda a administrar escritorios virtuales (VDI) en Multipoint Services
ms.custom: na
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fa9ac0ed-47cb-4811-91ff-4fcb62d7858b
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: 45bb3e98779bc27913c7e675a9c9db7e575d9d72
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71389589"
---
# <a name="manage-virtual-desktops"></a>Administrar escritorios virtuales
VDI de un solo equipo permite configurar cada estación de Multipoint Services *local* para que se conecte a un sistema operativo invitado de Windows 10 Enterprise que se ejecute en una máquina virtual (VM) de Hyper-V en el mismo equipo de Multipoint Services que la estación. Estas estaciones de escritorio virtual se pueden personalizar con aplicaciones que no se puede instalar en una versión de Windows Server.  
  
## <a name="enable-the-virtual-desktop-feature"></a>Habilitar la característica de escritorio virtual  
  
1.  Abra MultiPoint Manager y, después, haga clic en la pestaña **Escritorios virtuales**.  
  
2.  En **VDI Tasks** (Tareas de VDI), haga clic en **Create virtual desktop** (Crear escritorio virtual) y, después, vaya al VHD o .iso de Windows 10 Enterprise.  
  
El sistema se reinicia y este proceso puede tardar varios minutos.  
  
## <a name="create-a-virtual-desktop-template"></a>Crear una plantilla de escritorio virtual  
  
1.  Abra MultiPoint Manager y, después, haga clic en la pestaña **Escritorios virtuales**.  
  
2.  En **VDI Tasks** (Tareas de VDI), haga clic en **Create virtual desktop** (Crear escritorio virtual) y, después, vaya al VHD o .iso de Windows 10 Enterprise.  
  
    Si usa el DVD, el programa encontrará automáticamente el archivo .wim de Windows 10 Enterprise. En caso contrario, haga clic en **Examinar** y vaya hasta el VHD o .iso de Windows 10 Enterprise.  
  
    Modifique el prefijo si quiere. El valor predeterminado será el nombre del equipo host.  
  
    > [!NOTE]  
    > El prefijo se usa para dar nombre a la plantilla y a las estaciones de escritorios virtuales. El nombre de la plantilla es prefix \-t. Las estaciones de escritorios virtuales se denominarán prefix \-*n*, donde *n* es el identificador de la estación.  
  
4.  Escriba un nombre y una contraseña para la cuenta de administrador local, que se usará para iniciar sesión en todos los escritorios virtuales de estación que se creen a partir de esta plantilla y luego haga clic en **Aceptar**.  
  
    La creación de la plantilla tarda varios minutos en completarse.  
      
    Luego, aprenda a personalizar la plantilla de escritorio virtual.  
      
    > [!NOTE]  
    > Si el servidor MultiPoint está unido a un dominio, el cuadro de diálogo rellena un campo adicional que le permite establecer si las máquinas virtuales que se crearon a partir de la plantilla deben estar unidas a un dominio.   
  
## <a name="import-a-virtual-desktop-template"></a>Importar una plantilla de escritorio virtual  
En caso de que haya creado una plantilla de escritorio virtual en otro servidor MultiPoint, puede importar esa plantilla haciendo lo descrito aquí.  

1.  Abra MultiPoint Manager y, después, haga clic en la pestaña **Escritorios virtuales**.  
  
2.  En VDI Tasks (Tareas de VDI), haga clic en **Import virtual desktop template** (Importar plantilla de escritorio virtual).  
  
3.  Busque la plantilla y defina la ruta de acceso y el prefijo de la plantilla importada.  
  
## <a name="customize-the-virtual-desktop-template"></a>Personalizar la plantilla de escritorio virtual  
Tras crear la plantilla de escritorio virtual, puede personalizarla con aplicaciones y actualizaciones de software y configurar el sistema.   

1. Abra MultiPoint Manager y, después, haga clic en la pestaña **Escritorios virtuales**.  
2. Elija la plantilla de escritorio virtual y haga clic en **Customize virtual desktop template** (Personalizar plantilla de escritorio virtual).  
La plantilla se abre en una ventana aparte y se presentan más instrucciones donde se destacan los pasos más importantes para personalizar la plantilla de virtual. Revise estas instrucciones detenidamente.  
  
## <a name="create-virtual-desktop-stations"></a>Crear estaciones de escritorios virtuales  
  
1.  Abra MultiPoint Manager en modo de estación y, después, haga clic en la pestaña **Escritorios virtuales**.  
  
    > [!NOTE]  
    > Si el sistema MultiPoint Services no se está ejecutando en modo de estación, reinícielo antes de completar este procedimiento.  
  
2.  Seleccione la plantilla de escritorio virtual en el panel de la izquierda\-. El nombre es <prefijo – t>.  
  
3.  En las tareas de plantilla, haga clic en **Create virtual desktop stations** (Crear estaciones de escritorios virtuales) y, luego, haga clic en **Aceptar**.  
  
    El proceso de creación de una estación de escritorios virtuales tarda varios minutos.  
  
    > [!NOTE]  
    > Si alguna de las estaciones locales está conectada actualmente a una sesión\-escritorio virtual basado en, debe cerrar la sesión de esas estaciones para que se conecten a una de las estaciones de escritorios virtuales recién creadas.  
  
### <a name="validate-the-newly-created-customized-virtual-station-desktops"></a>Validar los escritorios virtuales personalizados recién creados en las estaciones  
  
Para validar los escritorios de la estación virtual personalizada, inicie sesión en una o varias de las estaciones de escritorios virtuales mediante una cuenta de administrador local o una cuenta de dominio y, a continuación, compruebe que la nueva máquina virtual\-escritorios virtuales basados en funcionan correctamente.  
  
## <a name="disable-virtual-desktops"></a>Deshabilitar escritorios virtuales  
  
Cuando los escritorios virtuales se deshabilitan, se desactivará la característica Hyper-V. Se cerrará la sesión de todos los usuarios y el sistema se reiniciará. Todas las estaciones virtuales se asignan a sesiones locales de MultiPoint después de reiniciar el sistema.  

1. Abra MultiPoint Manager en modo de estación y, después, haga clic en la pestaña **Escritorios virtuales**.  
  
2. En VDI Tasks (Tareas de VDI), haga clic en **Disable virtual desktops** (Deshabilitar escritorios virtuales). 