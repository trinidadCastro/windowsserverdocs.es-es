---
title: Creación de un disco de plantilla de máquina virtual blindada Linux
ms.topic: article
ms.assetid: d0e1d4fb-97fc-4389-9421-c869ba532944
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 08/29/2018
ms.openlocfilehash: fca3faca236a2fc5162d7a50ef02acad9b508226
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87996310"
---
# <a name="create-a-linux-shielded-vm-template-disk"></a>Creación de un disco de plantilla de máquina virtual blindada Linux

> Se aplica a: Windows Server 2019, Windows Server (canal semianual),

En este tema se explica cómo preparar un disco de plantilla para máquinas virtuales blindadas de Linux que se pueden usar para crear instancias de una o varias máquinas virtuales de inquilino.

## <a name="prerequisites"></a>Requisitos previos

Para preparar y probar una máquina virtual blindada Linux, necesitará los siguientes recursos disponibles:

- Un servidor con virtualización capababilities que ejecute Windows Server, versión 1709 o posterior
- Un segundo equipo (Windows 10 o Windows Server 2016) capaz de ejecutar el administrador de Hyper-V para conectarse a la consola de la máquina virtual en ejecución
- Una imagen ISO para uno de los sistemas operativos de máquinas virtuales blindadas compatibles con Linux:
    - Ubuntu 16,04 LTS con el kernel 4,4
    - Red Hat Enterprise Linux 7.3
    - SUSE Linux Enterprise Server 12 Service Pack 2
- Acceso a Internet para descargar el paquete lsvmtools y las actualizaciones del sistema operativo

> [!IMPORTANT]
> Las versiones más recientes de los sistemas operativos Linux anteriores pueden incluir un error conocido del controlador TPM que impedirá el aprovisionamiento correcto como máquinas virtuales blindadas.
> No se recomienda actualizar las plantillas o las máquinas virtuales blindadas a una versión más reciente hasta que esté disponible una corrección.
> La lista de sistemas operativos compatibles anteriores se actualizará cuando las actualizaciones se realicen de forma pública.

## <a name="prepare-a-linux-vm"></a>Preparación de una máquina virtual Linux

Las máquinas virtuales blindadas se crean a partir de discos de plantilla seguros.
Los discos de plantilla contienen el sistema operativo de la máquina virtual y los metadatos, incluida una firma digital de las particiones/boot y/root, para asegurarse de que los componentes principales del sistema operativo no se modifican antes de la implementación.

Para crear un disco de plantilla, primero debe crear una máquina virtual normal (sin protección) que se va a preparar como la imagen base para futuras máquinas virtuales blindadas.
El software que instale y los cambios de configuración que realice en esta máquina virtual se aplicarán a todas las máquinas virtuales blindadas creadas desde este disco de plantilla.
Estos pasos le guiarán a través de los requisitos mínimos para preparar una máquina virtual Linux para templatization.

> [!NOTE]
> El cifrado de disco de Linux se configura cuando el disco está particionado.
> Esto significa que debe crear una nueva máquina virtual que esté cifrada previamente mediante dm-crypt para crear un disco de plantilla de máquina virtual blindada Linux.


1.  En el servidor de virtualización, asegúrese de que Hyper-V y las características de compatibilidad de Hyper-V de protección del host se instalan mediante la ejecución de los siguientes comandos en una consola de PowerShell con privilegios elevados:

    ```powershell
    Install-WindowsFeature Hyper-V, HostGuardian -IncludeManagementTools -Restart
    ```

2.  Descargue la imagen ISO de una fuente de confianza y almacénela en el servidor de virtualización o en un recurso compartido de archivos accesible para el servidor de virtualización.

3.  En el equipo de administración que ejecuta Windows Server versión 1709, ejecute el siguiente comando para instalar la máquina virtual blindada Herramientas de administración remota del servidor:

    ```powershell
    Install-WindowsFeature RSAT-Shielded-VM-Tools
    ```

4.  Abra el **Administrador de Hyper-V** en el equipo de administración y conéctese al servidor de virtualización.
    Para ello, haga clic en "conectar con el servidor..." en el panel acciones o haciendo clic con el botón derecho en el administrador de Hyper-V y eligiendo "conectar con el servidor..." Proporcione el nombre DNS del servidor de Hyper-V y, si es necesario, las credenciales necesarias para conectarse a él.

5.  Mediante el administrador de Hyper-V, [Configure un conmutador externo](../../virtualization/hyper-v/get-started/create-a-virtual-switch-for-hyper-v-virtual-machines.md) en el servidor de virtualización para que la máquina virtual Linux pueda acceder a Internet para obtener actualizaciones.

6.  A continuación, cree una nueva máquina virtual para instalar el sistema operativo Linux en.
    En el panel acciones, haga clic en **nueva**  >  **máquina virtual** para abrir el asistente.
    Proporcione un nombre descriptivo para la máquina virtual, como "hace plantilla Linux", y haga clic en **siguiente**.

7.  En la segunda página del asistente, seleccione **generación 2** para asegurarse de que la máquina virtual se aprovisiona con un perfil de firmware basado en UEFI.

8.  Complete el resto del asistente según sus preferencias.
    No use un disco de diferenciación para esta máquina virtual. los discos de plantilla de máquina virtual blindada no pueden usar discos de diferenciación.
    Por último, conecte la imagen ISO que descargó anteriormente a la unidad de DVD virtual para esta máquina virtual, de modo que pueda instalar el sistema operativo.

9.  En el administrador de Hyper-V, seleccione la máquina virtual recién creada y haga clic en **conectar...** en el panel acciones para asociarla a una consola virtual de la máquina virtual.
    En la ventana que aparece, haga clic en **iniciar** para encender la máquina virtual.

10. Continúe con el proceso de configuración de la distribución de Linux seleccionada.
    Aunque cada distribución de Linux usa un asistente de configuración diferente, deben cumplirse los siguientes requisitos para las máquinas virtuales que se convertirán en discos de plantilla de máquina virtual blindada Linux:

    - El disco se debe particionar con el diseño de la tabla partición de GUID (GPT).
    - La partición raíz debe cifrarse con DM-Crypt. La frase de contraseña debe establecerse en **frase de contraseña** (todo en minúsculas). Esta frase de contraseña se volverá aleatoria y la partición se volverá a cifrar cuando se aprovisione una máquina virtual blindada.
    - La partición de arranque debe usar el sistema de archivos **ext2**

11. Una vez que el sistema operativo Linux se ha iniciado completamente y ha iniciado sesión, se recomienda instalar el kernel virtual de Linux y los paquetes de servicios de integración de Hyper-V asociados.
    Además, querrá instalar un servidor SSH u otra herramienta de administración remota para tener acceso a la máquina virtual una vez blindada.

    En Ubuntu, ejecute el siguiente comando para instalar estos componentes:

    ```bash
    sudo apt-get install linux-virtual linux-tools-virtual linux-cloud-tools-virtual linux-image-extra-virtual openssh-server
    ```

    En RHEL, ejecute el siguiente comando en su lugar:

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

12. Configure el sistema operativo Linux según sea necesario.
    Cualquier software que instale, las cuentas de usuario que agregue y los cambios de configuración que realice en todo el sistema se aplicarán a todas las máquinas virtuales futuras creadas desde este disco de plantilla.
    Debe evitar guardar secretos o paquetes innecesarios en el disco.

13. Si tiene previsto usar System Center Virtual Machine Manager para implementar las máquinas virtuales, instale el agente invitado de VMM para permitir que VMM especializa su sistema operativo durante el aprovisionamiento de la máquina virtual.
    La especialización permite que cada máquina virtual se configure de forma segura con distintos usuarios y claves SSH, configuraciones de red y pasos de configuración personalizados.
    Obtenga información acerca de cómo [obtener e instalar el agente invitado de VMM](/system-center/vmm/vm-linux#install-the-vmm-guest-agent) en la documentación de VMM.

14. A continuación, [agregue el repositorio de software de Linux de Microsoft al administrador de paquetes](../../administration/linux-package-repository-for-microsoft-software.md).

15. Use el administrador de paquetes para instalar el paquete lsvmtools que contiene la corrección de compatibilidad del cargador de máquinas virtuales blindadas Linux, los componentes de aprovisionamiento y la herramienta de preparación de discos.

    ```bash
    # Ubuntu 16.04
    sudo apt-get install lsvmtools

    # SLES 12 SP2
    sudo zypper install lsvmtools

    # RHEL 7.3
    sudo yum install lsvmtools
    ```

14. Cuando haya terminado de personalizar el sistema operativo Linux, busque el programa de instalación de lsvmprep en el sistema y ejecútelo.

    ```bash
    # The path below may change based on the version of lsvmprep installed
    # Run "find /opt -name lsvmprep" to locate the lsvmprep executable
    sudo /opt/lsvmtools-1.0.0-x86-64/lsvmprep
    ```

15. Apague la máquina virtual.

16. Si tomó puntos de control de la máquina virtual (incluidos los puntos de comprobación automáticos creados por Hyper-V con Windows 10 Fall Creators Update), asegúrese de eliminarlos antes de continuar.
    Los puntos de control crean discos de diferenciación (. avhdx) que no son compatibles con el Asistente de disco de plantilla.

    Para eliminar puntos de control, abra el **Administrador de Hyper-V**, seleccione la máquina virtual, haga clic con el botón derecho en el punto de control superior del panel puntos de control y luego haga clic en **eliminar subárbol de punto de control**.

    ![Eliminar todos los puntos de control de la máquina virtual de plantilla en el administrador de Hyper-V](../media/Guarded-Fabric-Shielded-VM/delete-checkpoints-lsvm-template.png)

## <a name="protect-the-template-disk"></a>Proteger el disco de plantilla

La máquina virtual que preparó en la sección anterior está casi lista para usarse como un disco de plantilla de máquina virtual blindada Linux.
El último paso es ejecutar el disco a través del Asistente para crear un disco de plantilla, que generará un hash y firmará digitalmente el estado actual de las particiones raíz y de arranque.
El hash y la firma digital se comprueban cuando se aprovisiona una máquina virtual blindada para garantizar que no se realizaron cambios no autorizados en las dos particiones entre la creación y la implementación de plantillas.

### <a name="obtain-a-certificate-to-sign-the-disk"></a>Obtener un certificado para firmar el disco

Para firmar digitalmente las medidas de disco, necesitará obtener un certificado en el equipo en el que ejecutará el Asistente para crear un disco de plantilla.
El certificado debe cumplir los siguientes requisitos:

Propiedad de certificado | Valor requerido
---------------------|---------------
Algoritmo de clave | RSA
Tamaño mínimo de clave | 2048 bits
Algoritmo de firma | SHA256 (recomendado)
Uso de claves | Firma digital

Los detalles sobre este certificado se mostrarán a los inquilinos cuando creen sus archivos de datos de blindaje y estén autorizando los discos en los que confían.
Por lo tanto, es importante obtener este certificado de una entidad de certificación de confianza mutua para el usuario y los inquilinos.
En escenarios empresariales en los que se encuentre el anfitrión y el inquilino, puede considerar la posibilidad de emitir este certificado a su entidad de certificación de empresa.
Proteja este certificado con cuidado, ya que cualquier persona que posea este certificado puede crear nuevos discos de plantilla que sean de confianza para el disco auténtico.

En un entorno de laboratorio de pruebas, puede crear un certificado autofirmado con el siguiente comando de PowerShell:

```powershell
New-SelfSignedCertificate -Subject "CN=Linux Shielded VM Template Disk Signing Certificate"
```

### <a name="process-the-disk-with-the-template-disk-wizard-cmdlet"></a>Procesar el disco con el cmdlet del Asistente para crear un disco de plantilla

Copie el disco de plantilla y el certificado en un equipo que ejecute Windows Server, versión 1709 y, a continuación, ejecute los siguientes comandos para iniciar el proceso de firma.
El VHDX que proporcione al `-Path` parámetro se sobrescribirá con el disco de plantilla actualizado, por lo que debe asegurarse de realizar una copia antes de ejecutar el comando.

> [!IMPORTANT]
> Los Herramientas de administración remota del servidor disponibles en Windows Server 2016 o Windows 10 no se pueden usar para preparar un disco de plantilla de máquina virtual blindada Linux.
> Use el cmdlet [Protect-TemplateDisk](/powershell/module/shieldedvmtemplate/protect-templatedisk?view=win10-ps) disponible en Windows Server, versión 1709 o el herramientas de administración remota del servidor disponible en windows Server 2019 para preparar un disco de plantilla de máquina virtual blindada Linux.

```powershell
# Replace "THUMBPRINT" with the thumbprint of your template disk signing certificate in the line below
$certificate = Get-Item Cert:\LocalMachine\My\THUMBPRINT

Protect-TemplateDisk -Path 'C:\temp\MyLinuxTemplate.vhdx' -TemplateName 'Ubuntu 16.04' -Version 1.0.0.0 -Certificate $certificate -ProtectedTemplateTargetDiskType PreprocessedLinux
```

El disco de plantilla ya está listo para usarse para aprovisionar máquinas virtuales blindadas Linux.
Si usa System Center Virtual Machine Manager para implementar la máquina virtual, ahora puede copiar el VHDX en la biblioteca de VMM.

También puede que desee extraer el catálogo de firmas de volumen del VHDX.
Este archivo se usa para proporcionar información sobre el certificado de firma, el nombre del disco y la versión a los propietarios de máquinas virtuales que desean usar la plantilla.
Deben importar este archivo en el Asistente para archivos de datos de blindaje con el fin de autorizarle, el autor de la plantilla en posesión del certificado de firma, para crear este y futuros discos de plantilla para ellos.

Para extraer el catálogo de firmas de volumen, ejecute el siguiente comando en PowerShell:

```powershell
Save-VolumeSignatureCatalog -TemplateDiskPath 'C:\temp\MyLinuxTemplate.vhdx' -VolumeSignatureCatalogPath 'C:\temp\MyLinuxTemplate.vsc'
```