---
title: Crear una Linux blindada disco de plantilla de máquina virtual
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: d0e1d4fb-97fc-4389-9421-c869ba532944
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: e791d18e027e5e3e1c5b9c52659641ff588a96a2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858676"
---
# <a name="create-a-linux-shielded-vm-template-disk"></a>Crear una Linux blindada disco de plantilla de máquina virtual

> Se aplica a: Windows Server, Windows Server (canal semianual), de 2019 

En este tema se explica cómo preparar un disco de plantilla para máquinas virtuales que se pueden usar para crear una instancia de inquilino de uno o más máquinas virtuales blindadas Linux.

## <a name="prerequisites"></a>Requisitos previos

Para preparar y probar una Linux máquina virtual blindada, necesitará los siguientes recursos disponibles:

- Un servidor con capababilities de virtualización que ejecuta Windows Server, versión 1709 o posterior
- Un segundo equipo (Windows 10 o Windows Server 2016) capaz de ejecutar el Administrador de Hyper-V para conectarse a la consola de la máquina virtual en ejecución
- Una imagen ISO para uno de Linux compatible blindadas sistemas operativos de la máquina virtual:
    - Ubuntu 16.04 LTS con el kernel 4.4
    - Red Hat Enterprise Linux 7.3
    - SUSE Linux Enterprise Server 12 Service Pack 2
- Acceso a Internet para descargar el paquete lsvmtools y actualizaciones del sistema operativo

> [!IMPORTANT]
> Las versiones más recientes de los anteriores que Linux OSes puede incluir un problema conocido de controlador TPM que impedirá que el aprovisionamiento correctamente como máquinas virtuales blindadas.
> No se recomienda que actualice las plantillas o máquinas virtuales blindadas a una versión más reciente hasta que esté disponible una corrección.
> La lista de sistemas operativos compatibles anteriores se actualizará cuando las actualizaciones sean públicas.

## <a name="prepare-a-linux-vm"></a>Preparar una máquina virtual Linux

Se crean las máquinas virtuales blindadas desde discos de plantilla seguras.
Discos de plantilla contienen el sistema operativo para que la máquina virtual y los metadatos, incluidos una firma digital de las particiones/Boot y/root, para asegurarse de que no se modifican los componentes principales de sistema operativo antes de la implementación.

Para crear un disco de plantilla, primero debe crear una VM normal (sin protección) que se preparará como imagen base para futuras máquinas virtuales blindadas.
El software que se instale y cambios de configuración realizados en esta máquina virtual se aplicarán a todas las máquinas virtuales blindadas creadas a partir de este disco de plantilla.
Estos pasos le guiará a través de los requisitos mínimos para preparar una VM de Linux para templatization.

> [!NOTE]
> Cifrado de disco de Linux se configura cuando se crean particiones en el disco.
> Esto significa que debe crear una nueva máquina virtual previamente cifrada mediante dm-crypt para crear una Linux blindada disco de plantilla de máquina virtual.


1.  En el servidor de virtualización, asegúrese de que Hyper-V y las características de compatibilidad de Hyper-V de guardián de Host se instalan mediante la ejecución de los siguientes comandos en una consola de PowerShell con privilegios elevados:

    ```powershell
    Install-WindowsFeature Hyper-V, HostGuardian -IncludeManagementTools -Restart
    ```

2.  Descargar la imagen ISO de una fuente de confianza y almacenarlo en el servidor de virtualización o en un recurso compartido de archivos accesible para el servidor de virtualización.

3.  En el equipo de administración que ejecuta Windows Server versión 1709, instale el blindadas VM herramientas de administración remota, ejecute el comando siguiente:

    ```powershell
    Install-WindowsFeature RSAT-Shielded-VM-Tools
    ```

4.  Abra **Administrador de Hyper-V** en el equipo de administración y conectarse al servidor de virtualización.
    Para ello, haga clic en "Conectar a servidor …" en el panel de acciones o con el botón derecho al hacer clic en el Administrador de Hyper-V y elija "conectan al servidor..." Proporcione el nombre DNS para el servidor de Hyper-V y, si es necesario, las credenciales necesarias para conectarse a él.

5.  Con el Administrador de Hyper-V, [configurar un conmutador externo](https://docs.microsoft.com/windows-server/virtualization/hyper-v/get-started/create-a-virtual-switch-for-hyper-v-virtual-machines) en el servidor de virtualización para la VM de Linux puede tener acceso a Internet para obtener actualizaciones.

6.  A continuación, cree una nueva máquina virtual para instalar el sistema operativo Linux en.
    En el panel Acciones, haga clic en **New** > **Máquina Virtual** para que aparezca el asistente.
    Proporcione un nombre descriptivo para la máquina virtual, como "Pre-templatized Linux" y haga clic en **siguiente**.

7.  En la segunda página del asistente, seleccione **generación 2** para asegurarse de que la máquina virtual se aprovisiona con un perfil basado en UEFI firmware.

8.  Complete el resto del asistente según sus preferencias.
    No use un disco de diferenciación para esta máquina virtual; discos de plantilla de máquina virtual blindados no pueden utilizar discos de diferenciación.
    Por último, conecte la imagen ISO que descargó anteriormente a la unidad de DVD virtual para esta máquina virtual para que pueda instalar el sistema operativo.

9.  En el Administrador de Hyper-V, seleccione la máquina virtual recién creada y haga clic en **conectar...**  en el panel de acciones para adjuntar a una consola de virtual de la máquina virtual.
    En la ventana que aparece, haga clic en **iniciar** a encender la máquina virtual.

10. Continúe con el proceso de instalación para la distribución de Linux seleccionado.
    Mientras que cada distribución de Linux usa a un Asistente para instalación diferente, deben cumplirse los siguientes requisitos para máquinas virtuales que se convertirá en Linux habían blindada discos de plantilla de máquina virtual:

    - El disco debe estar particionado con el diseño de tabla de partición GUID (GPT)
    - La partición raíz debe estar cifrada con dm-crypt. La frase de contraseña se debe establecer en **frase de contraseña** (todo en minúsculas). Esta frase de contraseña se aleatorio y la partición vuelven a cifrar cuando se aprovisiona una máquina virtual blindada.
    - La partición de arranque debe usar el **ext2** sistema de archivos

11. Una vez que se haya iniciado totalmente del sistema operativo Linux y que haya iniciado sesión, se recomienda que instale el kernel de linux virtual y los paquetes de servicios de integración de Hyper-V asociados.
    Además, desea instalar un servidor SSH u otra herramienta de administración remota para tener acceso a la máquina virtual una vez que está blindada.

    En Ubuntu, ejecute el siguiente comando para instalar estos componentes:

    ```bash
    sudo apt-get install linux-virtual linux-tools-virtual linux-cloud-tools-virtual linux-image-extra-virtual openssh-server
    ```

    En RHEL, ejecute el siguiente comando:

    ```bash
    sudo yum install hyperv-daemons openssh-server
    sudo service sshd start
    ```

    Y en SLES, ejecute el siguiente comando:

    ```bash
    sudo zypper install hyper-v
    sudo chkconfig hv_kvp_daemon on
    sudo systemctl enable sshd
    ```

12. Configuración del sistema operativo Linux según sea necesario.
    Cualquier software instalar, las cuentas de usuario que agregue, y asegúrese de que los cambios de configuración de todo el sistema se aplicarán a todas las máquinas virtuales futuras creadas a partir de este disco de plantilla.
    Debe evitar guardar los paquetes innecesarios ni secretos en el disco.

13. Si va a usar System Center Virtual Machine Manager para implementar las máquinas virtuales, instale el agente de invitado VMM para permitir que VMM especializar el sistema operativo durante el aprovisionamiento de máquinas virtuales.
    La especialización permite que cada máquina virtual debe establecerse forma segura con distintos usuarios y claves SSH, configuraciones de red y los pasos de instalación personalizada.
    Obtenga información sobre cómo [obtener e instalar el agente de invitado VMM](https://docs.microsoft.com/system-center/vmm/vm-linux#install-the-vmm-guest-agent) en la documentación de VMM.

14. A continuación, [agregar el repositorio de Software de Linux de Microsoft a su administrador de paquetes](../../administration/linux-package-repository-for-microsoft-software.md).

15. Mediante el Administrador de paquetes, instale el paquete de lsvmtools que contiene el sistema operativo Linux había blindada correcciones de compatibilidad de cargador de arranque de máquina virtual, los componentes y la herramienta de preparación del disco de aprovisionamiento.

    ```bash
    # Ubuntu 16.04
    sudo apt-get install lsvmtools

    # SLES 12 SP2
    sudo zypper install lsvmtools

    # RHEL 7.3
    sudo yum install lsvmtools
    ```

14. Cuando haya terminado personalizar el sistema operativo Linux, busque el programa de instalación lsvmprep en el sistema y ejecutarlo.
    
    ```bash
    # The path below may change based on the version of lsvmprep installed
    # Run "find /opt -name lsvmprep" to locate the lsvmprep executable
    sudo /opt/lsvmtools-1.0.0-x86-64/lsvmprep
    ```

15. Apague la máquina virtual.

16. Si ha realizado cualquier punto de comprobación de la máquina virtual (incluidos los puntos de comprobación creados por Hyper-V con la de Windows 10 Fall Creators Update), debe eliminarlos antes de continuar.
    Los puntos de control creación discos de diferenciación (.avhdx) que no son compatibles con el Asistente de disco de plantilla.
    
    Para eliminar los puntos de control, abra **Administrador de Hyper-V**, seleccione la máquina virtual, haga clic con el botón secundario del mouse en el punto de control de nivel superior en el panel de puntos de control y luego haga clic en **Eliminar subárbol de punto de comprobación**.

    ![Eliminar todos los puntos de control para la plantilla de máquina virtual en el Administrador de Hyper-V](../media/Guarded-Fabric-Shielded-VM/delete-checkpoints-lsvm-template.png)

## <a name="protect-the-template-disk"></a>Proteger el disco de plantilla

La máquina virtual que se preparó en la sección anterior está casi lista para usarse como una Linux blindada disco de plantilla de máquina virtual.
El último paso es ejecutar el Asistente de disco de plantilla, que hash y firmar digitalmente el estado actual de las particiones raíz y arranque el disco.
El hash y firma digital se comprueban cuando se aprovisiona una máquina virtual blindada para asegurarse de que no hay cambios no autorizados realizados en las dos particiones entre la creación de una plantilla y la implementación.

### <a name="obtain-a-certificate-to-sign-the-disk"></a>Obtener un certificado para firmar el disco

Para firmar digitalmente las mediciones de disco, deberá obtener un certificado en el equipo donde se ejecutará el Asistente de disco de plantilla.
El certificado debe cumplir los siguientes requisitos:

Propiedad de certificado | Valor requerido
---------------------|---------------
Algoritmo de clave | RSA
Tamaño mínimo de clave | 2048 bits
Algoritmo de firma | SHA256 (recomendado)
Uso de la clave | Firma digital

Detalles acerca de este certificado se mostrará a los inquilinos cuando crean sus archivos de datos de blindaje y se autoriza que confían los discos.
Por lo tanto, es importante obtener dicho certificado de una entidad de certificación confianza mutua entre usted y sus inquilinos.
En los escenarios empresariales donde está el proveedor de hospedaje y el inquilino, es posible que considere la posibilidad de emitir este certificado de la entidad de certificación empresarial.
Proteger este certificado con cuidado, como cualquier usuario en posesión de este certificado puede crear nuevos discos de plantilla que son de confianza igual que el disco auténtico.

En un entorno de laboratorio de pruebas, puede crear un certificado autofirmado con el siguiente comando de PowerShell:

```powershell
New-SelfSignedCertificate -Subject "CN=Linux Shielded VM Template Disk Signing Certificate"
```

### <a name="process-the-disk-with-the-template-disk-wizard-cmdlet"></a>El disco con el cmdlet de Asistente de disco de plantilla de proceso

Copie el disco de plantilla y el certificado en un equipo que ejecuta Windows Server, versión 1709, a continuación, ejecute los siguientes comandos para iniciar el proceso de firma.
El VHDX se proporciona a los `-Path` parámetro se sobrescribirá con el disco de plantilla actualizada, así que asegúrese de realizar una copia antes de ejecutar el comando.

> [!IMPORTANT]
> No se puede usar las herramientas de administración de servidor remoto disponible en Windows Server 2016 o Windows 10 para preparar una Linux blindada disco de plantilla de máquina virtual.
> Usar sólo la [proteger TemplateDisk](https://docs.microsoft.com/powershell/module/shieldedvmtemplate/protect-templatedisk?view=win10-ps) cmdlet disponible en Windows Server, versión 1709 o las herramientas de administración de servidor remoto disponible en Windows Server 2019 para preparar una Linux blindada disco de plantilla de máquina virtual.

```powershell
# Replace "THUMBPRINT" with the thumbprint of your template disk signing certificate in the line below
$certificate = Get-Item Cert:\LocalMachine\My\THUMBPRINT

Protect-TemplateDisk -Path 'C:\temp\MyLinuxTemplate.vhdx' -TemplateName 'Ubuntu 16.04' -Version 1.0.0.0 -Certificate $certificate -ProtectedTemplateTargetDiskType PreprocessedLinux
```

El disco de plantilla ahora está listo para usarse para aprovisionar máquinas virtuales blindadas Linux.
Si usa System Center Virtual Machine Manager para implementar la máquina virtual, ahora puede copiar el VHDX a la biblioteca VMM.

También puede extraer el catálogo de firmas de volumen de VHDX.
Este archivo se usa para proporcionar información sobre el certificado de firma, el nombre del disco y la versión a los propietarios de la máquina virtual que deseen usar la plantilla.
Debe importar este archivo en el Asistente de archivo de datos de blindaje para autorizar a usted, el autor de la plantilla en posesión del certificado de firmado, para crear este y discos de plantilla futuras para ellos.

Para extraer el catálogo de firmas de volumen, ejecute el siguiente comando en PowerShell:

```powershell
Save-VolumeSignatureCatalog -TemplateDiskPath 'C:\temp\MyLinuxTemplate.vhdx' -VolumeSignatureCatalogPath 'C:\temp\MyLinuxTemplate.vsc'
```
