---
title: Crear escritorios virtuales de Windows 10 Enterprise para las estaciones
description: Aprenda a crear los equipos de escritorio de Windows Server 2016 para la estación
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 63f08b5b-c735-41f4-b6c8-411eff85a4ab
author: evaseydl
ms.author: evas
manager: scottman
ms.openlocfilehash: befd784f4a2179c121992057e298d4ea9068c11b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862086"
---
# <a name="create-windows-10-enterprise-virtual-desktops-for-stations"></a>Crear escritorios virtuales de Windows 10 Enterprise para las estaciones
Esta configuración opcional en MultiPoint Services sirve principalmente para situaciones donde una aplicación esencial requiere su propia instancia de un sistema operativo cliente para cada usuario. Algunos ejemplos son las aplicaciones que no se puede instalar en Windows Server y aplicaciones que no se ejecutarán varias instancias en el mismo equipo host.  
  
> [!NOTE]  
> Estos escritorios virtuales, también conocido como VDI, son mucho más recursos que las sesiones de escritorio de MultiPoint Services de forma predeterminada, por lo que le recomendamos que use las sesiones de MultiPoint Services de forma predeterminada cuando sea posible.  
  
## <a name="prerequisites"></a>Requisitos previos  
Para preparar la creación de estación de escritorios virtuales, asegúrese de que los servicios de MultiPoint sistema cumple los requisitos siguientes:      
  
|Hardware|Requisitos|         |
|------------|----------------|----------------| 
|CPU (multimedia)|1 núcleo o subproceso por cada máquina virtual|  
|Unidades de estado sólido (SSD)|Capacidad de > = 20GB cada estación + 40GB para sistema operativo de hosts de los servicios de MultiPoint<br /><br />Operaciones aleatorias de lectura\/IOPS de escritura > = 3 K cada estación|  
|RAM|2GB por estación + 2GB para el sistema operativo de host de Windows MultiPoint Server|  
|Gráficos|DX11|  
|BIOS|Configuración de CPU de BIOS para habilitar la virtualización, traducción de direcciones de segundo nivel (SLAT)|  
  
-   **Las estaciones** -configurar las estaciones para su sistema MultiPoint Services. Para obtener más información, consulte [adjuntar adicionales estaciones en MultiPoint Services](Attach-additional-stations-to-your-MultiPoint-services-computer.md).  
  
-   **Dominio** : en un entorno de dominio, el equipo de Windows MultiPoint Server se ha agregado al dominio y un usuario de dominio se agregó al grupo Administradores local en el sistema operativo de host de MultiPoint Services.  
  
## <a name="procedures"></a>Procedimientos  
Use los siguientes procedimientos para:  
  
-   [Crear una plantilla para escritorios virtuales](#a-namebkmkcreateatemplateacreate-a-template-for-virtual-desktops)  
  
-   [Creación de escritorios virtuales de la plantilla](#BKMK_CreateVirtualDesktopsfromTemplate)  
  
-   [Copiar una plantilla de escritorio virtual existente](#BKMK_CopyExiistingVirtualDesktopTemplate)  
  
### <a name="BKMK_CreateaTemplate"></a>Crear una plantilla para escritorios virtuales  
Para poder crear una plantilla para los escritorios virtuales, debe habilitar la característica de Escritorio Virtual en MultiPoint Server.  
  
##### <a name="to-enable-the-virtual-desktop-feature"></a>Para habilitar la característica de Escritorio Virtual  
  
1.  Inicie sesión en el sistema de operativo del host de MultiPoint Server con una cuenta de administrador local o, en un dominio, con una cuenta de dominio que sea miembro del grupo Administradores local.  
  
2.  Desde el **iniciar** pantalla, abra MultiPoint Manager.  
  
3.  Haga clic en el **escritorios virtuales** , haga clic **Habilitar escritorios virtuales**y, a continuación, haga clic en **Aceptar**y espere a que el reinicio del sistema.  
  
El siguiente paso es crear una plantilla de Escritorio Virtual. Literalmente, va a crear un archivo de disco duro virtual (VHD) que puede usar como plantilla para crear escritorios virtuales de estación de MultiPoint Manager. Puede usar los medios de instalación física de Windows o un. Archivo de imagen ISO a como origen para la plantilla. También puede usar una. Disco duro virtual de la instalación de Windows. Tenga en cuenta que para utilizar un disco de instalación física, debe insertar el disco antes de iniciar al asistente.  
  
##### <a name="to-create-a-virtual-desktop-template"></a>Para crear una plantilla de Escritorio Virtual  
  
1.  Inicie sesión en el sistema de operativo del host de MultiPoint Server con una cuenta de administrador local o, en el dominio, una cuenta de dominio que sea miembro del grupo Administradores local.  
  
2.  Desde el **iniciar** pantalla, abra MultiPoint Manager.  
  
3.  Haga clic en el **escritorios virtuales** ficha.  
  
4.   Copie un archivo .iso de Windows 10 Enterprise en el SSD local.  
  
5.  En la pestaña escritorios virtuales, haga clic en **crear plantilla de escritorio virtual.**   
  
6.  En **prefijo**, escriba un prefijo que se utiliza para identificar la plantilla y los escritorios virtuales creados con la plantilla. El prefijo predeterminado es el nombre del equipo host.  
  
    El prefijo se usa para dar nombre a la plantilla y a las estaciones de escritorios virtuales. La plantilla estará <*prefijo*>-t. Las estaciones de escritorios virtuales se denominarán <*prefijo*>-*n*, donde *n* es el identificador de estación.  
  
7.  Escriba un nombre de usuario y contraseña para la cuenta de administrador local para la plantilla. En un dominio, escriba las credenciales para una cuenta de dominio que se agregarán al grupo de administradores local. Esta cuenta puede usarse para iniciar sesión en la plantilla y todas las estaciones de escritorios virtuales crean a partir de la plantilla.  
  
8. Haga clic en **Aceptar**y espere a que finalice la creación de plantilla.  
  
9. La nueva plantilla se mostrará en el **escritorios virtuales** ficha. La plantilla se desactivará.  
  
El siguiente paso es configurar la plantilla con el software y la configuración que desee en los escritorios virtuales. Debe hacerlo antes de crear los escritorios virtuales desde la plantilla.  
  
##### <a name="to-customize-a-virtual-desktop-template"></a>Para personalizar una plantilla de escritorio virtual  
  
1.  Inicie sesión en el sistema de operativo del host de MultiPoint server con una cuenta de administrador local o, en un dominio, con una cuenta de dominio en el grupo de administradores local.  
  
2.  Desde el **iniciar** pantalla, abra MultiPoint Manager.  
  
3.  Haga clic en el **escritorios virtuales** ficha.  
  
4.  Seleccione la plantilla que desee personalizar, haga clic en **Personalizar plantilla**y, a continuación, haga clic en **Aceptar**.  
  
    > [!NOTE]  
    > Solo las plantillas que no se usaron para crear estaciones de escritorios virtuales están disponibles. Si desea actualizar una plantilla que ya está en uso, debe hacer una copia de la plantilla mediante el uso de la **Importar plantilla** tarea, se describe más adelante, en [copiar una plantilla existente de escritorio virtual](#BKMK_CopyExiistingVirtualDesktopTemplate).  
  
    La plantilla se abre en un Hyper-V **conectar VM** ventana e inicio de sesión automático se realiza mediante la cuenta Administrador integrado.  
  
5.  En este momento puede instalar aplicaciones y actualizaciones de software, cambiar la configuración y actualizar el perfil de administrador. Todos los cambios realizados en perfil de administrador integrada la plantilla se copiarán en el perfil de usuario de forma predeterminada en las estaciones de escritorios virtuales que se crean a partir de la plantilla.  
  
    Si va a conectar sus emisoras a través de un dominio, se recomienda que cree una cuenta de usuario local y agregar al grupo de administradores local durante la personalización.  
  
    > [!NOTE]  
    > Si se reinicia el sistema mientras que se va a personalizar una plantilla, puede producir un error en Inicio de sesión automático con la cuenta de administrador integrada tras reiniciar el sistema. Para solucionar este problema, manualmente el registro sobre el uso de la cuenta de administrador local que ha creado, cambie la contraseña de la cuenta predefinida Administrador, cierre la sesión y, a continuación, volver a iniciar sesión con la cuenta predefinida Administrador y la contraseña nueva. (Debe eliminar el perfil que se creó cuando inicie sesión con la cuenta de administrador local.)  
  
6.  Cuando termine de configurar el sistema, haga doble clic en el **CompleteCustomization** acceso directo en el escritorio del administrador para ejecutar Sysprep y, a continuación, cierre la plantilla. Durante la personalización, la herramienta Sysprep elimina toda la información única del sistema para preparar la instalación de Windows para crear la imagen.  
  
### <a name="BKMK_CreateVirtualDesktopsfromTemplate"></a>Crear equipos de escritorio de máquina virtual de la plantilla  
Con la plantilla de escritorio virtual configurada como desea que sus equipos de escritorio sea, está listo para empezar a crear escritorios virtuales. Para cada estación que está conectado al equipo de MultiPoint Server, se creará un escritorio virtual. La próxima vez que un usuario inicia sesión en una estación, verá el escritorio virtual en lugar del escritorio basado en sesión que se mostró antes.  
  
> [!NOTE]  
> Este procedimiento solo funciona cuando servidor MultiPoint está en *modo de estación*. Si el sistema está en *modo de consola*, puede cambiar al modo de estación de MultiPoint Manager. Si usa la configuración predeterminada de MultiPoint, también puede iniciar el modo de estación, reinicie el equipo. De forma predeterminada, el equipo de MultiPoint Server siempre se inicia en modo de estación  
  
##### <a name="to-create-virtual-desktops-for-your-stations"></a>Para crear escritorios virtuales para sus estaciones  
  
1.  Inicie sesión en el servidor de Windows MultiPoint desde una estación remota (por ejemplo, desde un equipo Windows con conexión a Escritorio remoto) mediante un administrador local de la cuenta o, en un dominio, un dominio de la cuenta del grupo Administradores local.  
  
    > [!NOTE]  
    > Como alternativa, puede iniciar sesión en el servidor mediante una estación local. Sin embargo, cuando se crea un escritorio virtual de la estación, tendrá que cerrar la estación que usó para crear el escritorio virtual con el fin de conectarse al nuevo escritorio virtual en otra estación.  
  
2.  Desde el **iniciar** pantalla, abra MultiPoint Manager.  
  
3.  Si el equipo está en modo de consola, cambie al modo de estación:  
  
    1.  En el **inicio** , haga clic **cambiar al modo de estación**.  
  
    2.  Cuando se reinicia el equipo, inicie sesión como administrador.  
  
4.  Haga clic en el **escritorios virtuales** ficha.  
  
5.  Seleccione la plantilla de escritorio virtual que desea usar con las estaciones, haga clic en **crear estaciones de escritorios virtuales**y, a continuación, haga clic en **Aceptar**.  
  
Cuando se complete la tarea, cada estación local se conectará a un escritorio virtual basado en la máquina virtual.  
  
> [!NOTE]  
> Si una cuenta de usuario ha iniciado sesión en cualquiera de las estaciones locales, deberá cerrar la sesión para obtener la estación para conectarse a uno de los escritorios virtuales estación recién creado.  
  
### <a name="BKMK_CopyExiistingVirtualDesktopTemplate"></a>Copiar una plantilla de escritorio virtual existente  
Use el procedimiento siguiente para crear una copia de una plantilla de escritorio virtual existente que puede personalizar y usar. Esto puede ser útil en las situaciones siguientes:  
  
-   Para copiar una plantilla maestra desde un recurso compartido de red en un equipo de MultiPoint Server host para que las estaciones de escritorios virtuales se pueden crear a partir de la plantilla principal.  
  
-   Para crear una copia de una plantilla que está actualmente en uso, por lo que puede realizar personalizaciones adicionales.  
  
##### <a name="to-import-a-virtual-desktop-template"></a>Para importar una plantilla de escritorio virtual  
  
1.  Inicie sesión como administrador el servidor MultiPoint.  
  
2.  Desde el **iniciar** pantalla, abra MultiPoint Manager.  
  
3.  Haga clic en el **escritorios virtuales** ficha.  
  
4.  Haga clic en **Importar plantilla de escritorio virtual**y usar **examinar** para seleccionar el archivo .vhd (plantilla) que se va a importar. Al importar una plantilla, se realiza una copia de VHD original. De forma predeterminada, MultiPoint Services almacena archivos .vhd en la unidad C:\\usuarios\\pública\\documentos\\Hyper\-V\\discos duros virtuales\\ carpeta.  
  
5.  Escriba un prefijo para la nueva plantilla y, a continuación, haga clic en **Aceptar**.  
  
6.  Si va a realizar más personalizaciones en una plantilla local, puede cambiar el nombre del prefijo al incrementar un número de versión al final del prefijo. O bien, si va a importar una plantilla principal, es posible que desea agregar la versión de la plantilla principal hasta el final del nombre de prefijo predeterminado.  
  
7.  Cuando se complete la tarea, puede personalizar la plantilla o usarlo y sirve crear estaciones.  
