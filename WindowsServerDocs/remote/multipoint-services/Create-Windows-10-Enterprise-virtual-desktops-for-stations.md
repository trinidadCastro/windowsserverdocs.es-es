---
title: Crear escritorios virtuales de Windows 10 Enterprise para las estaciones
description: Aprenda a crear escritorios de Windows Server 2016 para la estación
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
ms.openlocfilehash: e68412808e037b788d5b25c1c2c7b14253e40ea6
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871732"
---
# <a name="create-windows-10-enterprise-virtual-desktops-for-stations"></a>Crear escritorios virtuales de Windows 10 Enterprise para las estaciones
Esta configuración opcional en Multipoint Services está destinada principalmente a situaciones en las que una aplicación esencial requiere su propia instancia de un sistema operativo cliente para cada usuario. Algunos ejemplos son las aplicaciones que no se pueden instalar en Windows Server y las aplicaciones que no ejecutarán varias instancias en el mismo equipo host.  
  
> [!NOTE]  
> Estos escritorios virtuales, también conocidos como VDI, son mucho más intensivo de recursos que las sesiones de escritorio de Multipoint Services predeterminadas, por lo que se recomienda usar sesiones de Multipoint Services predeterminadas siempre que sea posible.  
  
## <a name="prerequisites"></a>Requisitos previos  
Para preparar la creación de escritorios virtuales de estación, asegúrese de que el sistema Multipoint Services cumple los requisitos siguientes:      
  
|Hardware|Requisitos|         |
|------------|----------------|----------------| 
|CPU (multimedia)|1 núcleo o subproceso por máquina virtual|  
|Unidad de estado sólido (SSD)|Capacidad > = 20 GB por estación + 40 GB para el sistema operativo host de Multipoint Services<br /><br />IOPS de\/escritura de lectura aleatoria > = 3K por estación|  
|RAM|2 GB por estación + 2 GB para el sistema operativo host de Windows MultiPoint Server|  
|Gráficos|DX11|  
|BIOS|Configuración de CPU de BIOS configurada para habilitar la virtualización: traducción de direcciones de segundo nivel (SLAT)|  
  
-   **Estaciones** : Configure las estaciones del sistema Multipoint Services. Para más información, consulte [Asociación de estaciones adicionales a multipoint Services](Attach-additional-stations-to-your-MultiPoint-services-computer.md).  
  
-   **Dominio** : en un entorno de dominio, el equipo con Windows MultiPoint Server se ha agregado al dominio y se ha agregado un usuario de dominio al grupo de administradores locales en el sistema operativo host de Multipoint Services.  
  
## <a name="procedures"></a>Procedimientos  
Use los siguientes procedimientos para:  
  
-   [Crear una plantilla para escritorios virtuales](#create-a-template-for-virtual-desktops)  
  
-   [Crear escritorios virtuales a partir de la plantilla](#create-virtual-machine-desktops-from-the-template)  
  
-   [Copia de una plantilla de escritorio virtual existente](#copy-an-existing-virtual-desktop-template)  
  
### <a name="create-a-template-for-virtual-desktops"></a>Crear una plantilla para escritorios virtuales  
Antes de poder crear una plantilla para los escritorios virtuales, debe habilitar la característica de escritorio virtual en MultiPoint Server.  
  
##### <a name="to-enable-the-virtual-desktop-feature"></a>Para habilitar la característica de escritorio virtual  
  
1.  Inicie sesión en el sistema operativo host de servidor multipoint con una cuenta de administrador local o, en un dominio, con una cuenta de dominio que sea miembro del grupo local de administradores.  
  
2.  En la pantalla **Inicio** , abra Multipoint Manager.  
  
3.  Haga clic en la pestaña **escritorios** virtuales, haga clic en **Habilitar escritorios virtuales**y, a continuación, haga clic en **Aceptar**y espere a que se reinicie el sistema.  
  
El siguiente paso consiste en crear una plantilla de escritorio virtual. Está creando literalmente un archivo de disco duro virtual (VHD) que puede usar como plantilla para crear escritorios virtuales de estación para Multipoint Manager. Puede usar los medios de instalación física para Windows o un. Archivo de imagen ISO como origen de la plantilla. También puede utilizar un. VHD de la instalación de Windows. Tenga en cuenta que para utilizar un disco de instalación física, debe insertar el disco antes de iniciar el asistente.  
  
##### <a name="to-create-a-virtual-desktop-template"></a>Para crear una plantilla de escritorio virtual  
  
1.  Inicie sesión en el sistema operativo host de servidor multipoint con una cuenta de administrador local o, en el dominio, una cuenta de dominio que sea miembro del grupo de administradores locales.  
  
2.  En la pantalla **Inicio** , abra Multipoint Manager.  
  
3.  Haga clic en la pestaña **escritorios virtuales** .  
  
4.   Copie un archivo. ISO de Windows 10 Enterprise en el SSD local.  
  
5.  En la pestaña escritorios virtuales, haga clic en **Crear plantilla de escritorio virtual.**   
  
6.  En **prefijo**, escriba el prefijo que se usará para identificar la plantilla y los escritorios virtuales creados con la plantilla. El prefijo predeterminado es el nombre del equipo host.  
  
    El prefijo se usa para dar nombre a la plantilla y a las estaciones de escritorios virtuales. La plantilla será <*prefijo*>-t. Las estaciones de escritorios virtuales se denominarán <*prefijo*>-*n*, donde *n* es el identificador de la estación.  
  
7.  Escriba un nombre de usuario y una contraseña que se usarán para la cuenta de administrador local para la plantilla. En un dominio, escriba las credenciales de una cuenta de dominio que se agregará al grupo de administradores locales. Esta cuenta puede usarse para iniciar sesión en la plantilla y en todas las estaciones de escritorios virtuales creadas a partir de la plantilla.  
  
8. Haga clic en **Aceptar**y espere a que se complete la creación de la plantilla.  
  
9. La nueva plantilla se mostrará en la pestaña **escritorios virtuales** . La plantilla se desactivará.  
  
El siguiente paso consiste en configurar la plantilla con el software y la configuración que desee en los escritorios virtuales. Debe hacerlo antes de crear los escritorios virtuales a partir de la plantilla.  
  
##### <a name="to-customize-a-virtual-desktop-template"></a>Para personalizar una plantilla de escritorio virtual  
  
1.  Inicie sesión en el sistema operativo host de servidor multipoint con una cuenta de administrador local o, en un dominio, con una cuenta de dominio en el grupo de administradores locales.  
  
2.  En la pantalla **Inicio** , abra Multipoint Manager.  
  
3.  Haga clic en la pestaña **escritorios virtuales** .  
  
4.  Seleccione la plantilla que desea personalizar, haga clic en **personalizar plantilla**y, a continuación, haga clic en **Aceptar**.  
  
    > [!NOTE]  
    > Solo están disponibles las plantillas que no se han usado para crear estaciones de escritorios virtuales. Si desea actualizar una plantilla que ya está en uso, debe hacer una copia de la plantilla mediante la tarea **Importar plantilla** , que se describe más adelante, en [copiar una plantilla de escritorio virtual existente](#copy-an-existing-virtual-desktop-template).  
  
    La plantilla se abre en una ventana de **conexión de máquina virtual** de Hyper-V y el inicio de sesión automático se realiza mediante la cuenta de administrador integrada.  
  
5.  En este momento puede instalar aplicaciones y actualizaciones de software, cambiar la configuración y actualizar el perfil de administrador. Todos los cambios realizados en el perfil de administrador integrado de la plantilla se copiarán en el perfil de usuario predeterminado en las estaciones de escritorios virtuales que se crean a partir de la plantilla.  
  
    Si va a conectar sus estaciones a través de un dominio, se recomienda crear una cuenta de usuario local y agregarla al grupo de administradores locales durante la personalización.  
  
    > [!NOTE]  
    > Si el sistema se reinicia mientras se personaliza una plantilla, puede producirse un error en el inicio de sesión automático con la cuenta de administrador integrada después de reiniciar el sistema. Para solucionar este problema, inicie sesión manualmente con la cuenta de administrador local que creó, cambie la contraseña de la cuenta de administrador integrada, cierre la sesión y vuelva a iniciarla con la cuenta predefinida Administrador y la nueva contraseña. (Tendrá que eliminar el perfil que se creó al iniciar sesión con la cuenta de administrador local).  
  
6.  Cuando termine de configurar el sistema, haga doble clic en el acceso directo **CompleteCustomization** en el escritorio del administrador para ejecutar Sysprep y, a continuación, cierre la plantilla. Durante la personalización, la herramienta Sysprep quita toda la información única del sistema para preparar la instalación de Windows para la imagen.  
  
### <a name="create-virtual-machine-desktops-from-the-template"></a>Crear escritorios de máquinas virtuales a partir de la plantilla  
Con la plantilla de escritorio virtual configurada de la forma en que desea que los escritorios estén, está listo para empezar a crear escritorios virtuales. Se creará un escritorio virtual para cada estación que esté conectada al equipo de MultiPoint Server. La próxima vez que un usuario inicie sesión en una estación, verá el escritorio virtual en lugar del escritorio basado en sesión que se mostró antes.  
  
> [!NOTE]  
> Este procedimiento solo funciona cuando MultiPoint Server está en *modo de estación*. Si el sistema está en *modo de consola*, puede cambiar al modo de estación de Multipoint Manager. Si usa la configuración de Multipoint predeterminada, también puede iniciar el modo de estación reiniciando el equipo. De forma predeterminada, el equipo de MultiPoint Server siempre se inicia en modo de estación  
  
##### <a name="to-create-virtual-desktops-for-your-stations"></a>Para crear escritorios virtuales para las estaciones  
  
1.  Inicie sesión en Windows MultiPoint Server desde una estación remota (por ejemplo, desde un equipo Windows con Conexión a Escritorio remoto) mediante una cuenta de administrador local o, en un dominio, una cuenta de dominio en el grupo de administradores locales.  
  
    > [!NOTE]  
    > Como alternativa, puede iniciar sesión en el servidor con una estación local. Sin embargo, cuando cree un escritorio virtual de estación, tendrá que cerrar la estación que usó para crear el escritorio virtual con el fin de conectar la otra estación al nuevo escritorio virtual.  
  
2.  En la pantalla **Inicio** , abra Multipoint Manager.  
  
3.  Si el equipo está en modo de consola, cambie al modo de estación:  
  
    1.  En la pestaña **Inicio** , haga clic en **cambiar al modo de estación**.  
  
    2.  Cuando se reinicie el equipo, inicie sesión como administrador.  
  
4.  Haga clic en la pestaña **escritorios virtuales** .  
  
5.  Seleccione la plantilla de escritorio virtual que desea usar con las estaciones, haga clic en **crear estaciones de escritorios virtuales**y, a continuación, haga clic en **Aceptar**.  
  
Cuando se complete la tarea, cada estación local se conectará a un escritorio virtual basado en máquina virtual.  
  
> [!NOTE]  
> Si una cuenta de usuario ha iniciado sesión en cualquiera de las estaciones locales, deberá cerrar sesión en la sesión para que la estación se conecte a uno de los escritorios virtuales de la estación recién creados.  
  
### <a name="copy-an-existing-virtual-desktop-template"></a>Copia de una plantilla de escritorio virtual existente  
Use el procedimiento siguiente para crear una copia de una plantilla de escritorio virtual existente que puede personalizar y usar. Esto puede ser útil en las situaciones siguientes:  
  
-   Para copiar una plantilla maestra desde un recurso compartido de red en un equipo host de MultiPoint Server para que se puedan crear estaciones de escritorios virtuales a partir de la plantilla maestra.  
  
-   Para crear una copia de una plantilla que se está usando actualmente para poder crear personalizaciones adicionales.  
  
##### <a name="to-import-a-virtual-desktop-template"></a>Para importar una plantilla de escritorio virtual  
  
1.  Inicie sesión en el servidor Multipoint como administrador.  
  
2.  En la pantalla **Inicio** , abra Multipoint Manager.  
  
3.  Haga clic en la pestaña **escritorios virtuales** .  
  
4.  Haga clic en **Importar plantilla de escritorio virtual**y use **examinar** para seleccionar el archivo. vhd (plantilla) que desea importar. Al importar una plantilla, se realiza una copia del archivo. vhd original. De forma predeterminada, Multipoint Services almacena archivos. vhd en la\\carpeta\\C\\: users\\Public\\Hard Disks\\ de Hyper\-V.  
  
5.  Escriba un prefijo para la nueva plantilla y, a continuación, haga clic en **Aceptar**.  
  
6.  Si va a realizar más personalizaciones en una plantilla local, puede cambiar el nombre del prefijo incrementando un número de versión al final del prefijo. O bien, si va a importar una plantilla maestra, puede que desee agregar la versión de la plantilla maestra al final del nombre del prefijo predeterminado.  
  
7.  Cuando la tarea se completa, puede personalizar la plantilla o usarla como si se creara.  
