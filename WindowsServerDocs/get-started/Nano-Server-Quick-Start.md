---
title: Inicio rápido de Nano Server
description: Pasos para implementar rápidamente un Nano Server básico en máquinas virtuales o físicas
ms.prod: windows-server
manager: DonGill
ms.technology: server-nano
ms.date: 09/05/2017
ms.topic: get-started-article
author: jaimeo
ms.author: jaimeo
ms.localizationpriority: medium
ms.openlocfilehash: 7729b853f2e54c7f99d428fcb821a68d7a22aef0
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2020
ms.locfileid: "80826818"
---
# <a name="nano-server-quick-start"></a>Inicio rápido de Nano Server

>Se aplica a: Windows Server 2016

> [!IMPORTANT]
> A partir de Windows Server, versión 1709, Nano Server estará disponible solo como [imagen base del sistema operativo del contenedor](/virtualization/windowscontainers/quick-start/using-insider-container-images#install-base-container-image). Consulta [Cambios en Nano Server](nano-in-semi-annual-channel.md) para más información. 

Siga los pasos de esta sección para empezar a trabajar rápidamente con una implementación básica de Nano Server usando DHCP para obtener una dirección IP. Puede ejecutar un VHD de Nano Server en una máquina virtual o arrancarlo en un equipo físico; los pasos son ligeramente diferentes.

Una vez que ha intentado las tareas básicas con estos pasos de inicio rápido, puede encontrar detalles de la creación de sus propias imágenes personalizadas, la administración de paquetes con varios métodos, las operaciones de dominio y mucho más en [Implementación de Nano Server](Deploy-Nano-Server.md).
  
**Nano Server en una máquina virtual**  
  
Siga estos pasos para crear un VHD de Nano Server que se ejecutará en una máquina virtual.  
  
## <a name="to-quickly-deploy-nano-server-in-a-virtual-machine"></a>Para implementar rápidamente Nano Server en una máquina virtual  
  
1. Copie la carpeta *NanoServerImageGenerator* desde la carpeta \NanoServer de la imagen ISO de Windows Server 2016 en una carpeta del disco duro.  
  
2. Inicia Windows PowerShell como administrador, cambia el directorio a la carpeta donde has colocado la carpeta NanoServerImageGenerator y, luego, importa el módulo con `Import-Module .\NanoServerImageGenerator -Verbose`  
   >[!NOTE]  
   >Puede que tenga que ajustar la directiva de ejecución de Windows PowerShell. `Set-ExecutionPolicy RemoteSigned` debería funcionar correctamente.  
  
3. Cree un VHD para la edición Standard que establezca un nombre de equipo e incluya **controladores invitado** de Hyper-V mediante la ejecución del comando siguiente que le solicitará una contraseña de administrador para el nuevo VHD:  
  
   `New-NanoServerImage -Edition Standard -DeploymentType Guest -MediaPath <path to root of media> -BasePath .\Base -TargetPath .\NanoServerVM\NanoServerVM.vhd -ComputerName <computer name>`, donde  
  
   -   **-MediaPath <ruta de acceso a la raíz de medios\>** especifica una ruta de acceso a la raíz del contenido de la imagen ISO de Windows Server 2016. Por ejemplo, si ha copiado el contenido de la imagen ISO a d:\TP5ISO, usaría esa ruta de acceso.  
  
   -   **-BasePath** (opcional) especifica una carpeta que se creará para copiar los paquetes y archivos WIM de Nano Server.  
  
   -   **-TargetPath** especifica una ruta de acceso, incluidos el nombre de archivo y la extensión, donde se creará el VHD o VHDX resultante.  
  
   -   **Computer_name** especifica el nombre de equipo que tendrá la máquina virtual de Nano Server que va a crear.  
  
   **Ejemplo:** `New-NanoServerImage -Edition Standard -DeploymentType Guest -MediaPath f:\ -BasePath .\Base -TargetPath .\Nano1\Nano.vhd -ComputerName Nano1`  
  
   Este ejemplo crea un VHD de un archivo ISO montado como f:\\. Al crear el VHD, utiliza una carpeta denominada Base en el mismo directorio donde se ejecutó New-NanoServerImage; coloca el VHD (llamado Nano.vhd) en una carpeta denominada Nano1 en la carpeta desde donde se ejecuta el comando. El nombre de equipo será Nano1. El VHD resultante contendrá la edición Standard de Windows Server 2016 y será adecuado para la implementación de la máquina virtual de Hyper-V. Si desea una máquina virtual de generación 1, cree una imagen de VHD especificando una extensión **.vhd** para -TargetPath. Si desea una máquina virtual de generación 2, cree una imagen de VHDX especificando una extensión **.vhdx** para -TargetPath. También puede generar directamente un archivo WIM especificando una extensión **.wim** para -TargetPath.  
  
   > [!NOTE]  
   > New-NanoServerImage se admite en Windows 8.1, Windows 10, Windows Server 2012 R2 y Windows Server 2016.  
  
4. En el Administrador de Hyper-V, cree una nueva máquina virtual y utilice el VHD creado en el paso 3.  
  
5. Arranque la máquina virtual y conéctese a ella en el Administrador de Hyper-V.  
  
6. Inicia sesión en la Consola de recuperación (consulta la sección Consola de recuperación de Nano Server en esta guía), con el administrador y la contraseña indicados al ejecutar el script en el paso 3.  
   > [!NOTE]  
   > La Consola de recuperación solo admite funciones básicas de teclado. No se admiten las luces del teclado, las secciones de 10 teclas y el cambio de la distribución del teclado entre Bloq Mayús y Bloq num.
  
7. Obtenga la dirección IP de la máquina virtual de Nano Server y use la comunicación remota de Windows PowerShell u otra herramienta de administración remota para conectarse y administrar de forma remota la máquina virtual.  
  
**Nano Server en un equipo físico**  
  
También puede crear un VHD que ejecutará Nano Server en un equipo físico, con los controladores de dispositivo instalados previamente. Si el hardware necesita un controlador que no se ha proporcionado ya, con el fin de iniciar una red o conectarse a ella, sigue los pasos descritos en la sección Agregar más controladores de esta guía.  
  
## <a name="to-quickly-deploy-nano-server-on-a-physical-computer"></a>Para implementar Nano Server rápidamente en un equipo físico  
  
1.  Copie la carpeta *NanoServerImageGenerator* desde la carpeta \NanoServer de la imagen ISO de Windows Server 2016 en una carpeta del disco duro.  
  
2.  Inicia Windows PowerShell como administrador, cambia el directorio a la carpeta donde has colocado la carpeta NanoServerImageGenerator y, luego, importa el módulo con `Import-Module .\NanoServerImageGenerator -Verbose`  
  
>[!NOTE]  
>Puede que tenga que ajustar la directiva de ejecución de Windows PowerShell. `Set-ExecutionPolicy RemoteSigned` debería funcionar correctamente.  
  
3. Cree un VHD que establezca un nombre de equipo e incluya los controladores de OEM e Hyper-V mediante la ejecución del comando siguiente que le solicitará una contraseña de administrador para el nuevo VHD:  
  
   `New-NanoServerImage -Edition Standard -DeploymentType Host -MediaPath <path to root of media> -BasePath .\Base -TargetPath .\NanoServerPhysical\NanoServer.vhd -ComputerName <computer name> -OEMDrivers -Compute -Clustering`, donde  
  
   -   **-MediaPath <ruta de acceso a la raíz de medios\>** especifica una ruta de acceso a la raíz del contenido de la imagen ISO de Windows Server 2016. Por ejemplo, si ha copiado el contenido de la imagen ISO a d:\TP5ISO, usaría esa ruta de acceso.  
  
   -   **BasePath** especifica una carpeta que se creará para copiar los paquetes y archivos WIM de Nano Server. (Este parámetro es opcional).  
  
   -   **TargetPath** especifica una ruta de acceso, incluidos el nombre de archivo y la extensión, donde se creará el VHD o VHDX resultante.  
  
   -   **Computer_name** es el nombre de equipo para el servidor Nano Server que va a crear.  
  
   **Ejemplo:** `New-NanoServerImage -Edition Standard -DeploymentType Host -MediaPath F:\ -BasePath .\Base -TargetPath .\Nano1\NanoServer.vhd -ComputerName Nano-srv1 -OEMDrivers -Compute -Clustering`  
  
   Este ejemplo crea un VHD de un archivo ISO montado como F:\\. Al crear el VHD, utiliza una carpeta denominada Base en el mismo directorio donde se ejecutó New-NanoServerImage; coloca el VHD en una carpeta denominada Nano1 en la carpeta desde donde se ejecuta el comando. El nombre del equipo será Nano-srv1 y tendrá controladores de OEM instalados para la mayoría del hardware habitual y tiene el rol de Hyper-V y la característica de agrupación en clústeres habilitados. Se usa la edición Standard Nano.  
  
4. Inicie sesión como administrador en el servidor físico donde desea ejecutar el VHD de Nano Server.  
  
5. Copie el VHD que este script crea en el equipo físico y configúrelo para que arranque desde este nuevo VHD. Para ello, siga estos pasos:  
  
   1.  Monte el VHD generado. En este ejemplo, se monta en D:\\.  
  
   2.  Ejecute **bcdboot d:\windows**.  
  
   3.  Desmonte el VHD.  
  
6. Arranque el equipo físico en el VHD de Nano Server.  
  
7. Inicia sesión en la Consola de recuperación (consulta la sección Consola de recuperación de Nano Server en esta guía), con el administrador y la contraseña indicados al ejecutar el script en el paso 3.
   > [!NOTE]  
   > La Consola de recuperación solo admite funciones básicas de teclado. No se admiten las luces del teclado, las secciones de 10 teclas y el cambio de la distribución del teclado entre Bloq Mayús y Bloq num. 
  
8. Obtenga la dirección IP del equipo de Nano Server y use la comunicación remota de Windows PowerShell u otra herramienta de administración remota para conectarse y administrar de forma remota la máquina virtual.  
