---
title: Creación de un disco de plantilla de máquina virtual blindada con Windows
ms.prod: windows-server
ms.topic: article
ms.assetid: 9c8b84e8-1f5a-47a1-83ca-b1dbd801cb0b
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 01/29/2019
ms.openlocfilehash: 766ea9688b7f08914ca68a960cc21393963bd0e9
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856778"
---
# <a name="create-a-windows-shielded-vm-template-disk"></a>Creación de un disco de plantilla de máquina virtual blindada con Windows

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2019


Al igual que con las máquinas virtuales normales, puede crear una plantilla de máquina virtual (por ejemplo, una [plantilla de máquina virtual en Virtual Machine Manager (VMM)](https://technet.microsoft.com/system-center-docs/vmm/manage/manage-library-add-vm-templates)) para facilitar a los inquilinos y administradores la implementación de nuevas máquinas virtuales en el tejido mediante un disco de plantilla. Dado que las máquinas virtuales blindadas son recursos sensibles a la seguridad, existen pasos adicionales para crear una plantilla de máquina virtual que admita el blindaje. En este tema se describen los pasos para crear un disco de plantilla blindada y una plantilla de máquina virtual en VMM.

Para entender cómo se ajusta este tema en el proceso general de implementación de máquinas virtuales blindadas, consulte [los pasos de configuración de proveedor de servicio de hospedaje para hosts protegidos y máquinas virtuales blindadas](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md).

## <a name="prepare-an-operating-system-vhdx"></a>Preparar un VHDX de sistema operativo

En primer lugar, prepare un disco del sistema operativo que se ejecutará a través del Asistente para creación de discos de plantilla blindada. Este disco se usará como disco del sistema operativo en las máquinas virtuales del inquilino. Puede usar cualquier herramienta existente para crear este disco, como Microsoft Desktop Image Service Manager (DISM), o bien configurar manualmente una máquina virtual con un VHDX en blanco e instalar el sistema operativo en ese disco. Al configurar el disco, debe cumplir los siguientes requisitos específicos de la generación 2 o las máquinas virtuales blindadas: 

| Requisito para VHDX | Razón |
|-----------|----|
|Debe ser un disco de tabla de particiones GUID (GPT) | Necesario para que las máquinas virtuales de generación 2 admitan UEFI|
|El tipo de disco debe ser **básico** en lugar de **dinámico**. <br>Nota: Esto hace referencia al tipo de disco lógico, no a la característica VHDX de "expansión dinámica" compatible con Hyper-V. | BitLocker no admite discos dinámicos.|
|El disco tiene al menos dos particiones. Una partición debe incluir la unidad en la que está instalado Windows. Esta unidad será cifrada por BitLocker. La otra partición es la partición activa, que contiene el cargador de inicio y permanece sin cifrar para poder iniciar el equipo.|Necesario para BitLocker|
|El sistema de archivos es NTFS | Necesario para BitLocker|
|El sistema operativo instalado en VHDX es uno de los siguientes:<br>-Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 <br>-Windows 10, Windows 8.1, Windows 8| Necesario para admitir máquinas virtuales de generación 2 y la plantilla de arranque seguro de Microsoft|
|El sistema operativo debe estar generalizado (ejecute Sysprep. exe) | El aprovisionamiento de plantillas implica máquinas virtuales especializadas para la carga de trabajo de un inquilino específico| 

> [!NOTE]
> Si usa VMM, no copie el disco de plantilla en la biblioteca VMM en esta fase. 

## <a name="run-windows-update-on-the-template-operating-system"></a>Ejecutar Windows Update en el sistema operativo de la plantilla

En el disco de plantilla, compruebe que el sistema operativo tiene instaladas todas las actualizaciones de Windows más recientes. Las actualizaciones publicadas recientemente mejoran la confiabilidad del proceso de blindaje de un extremo a otro: un proceso que puede no completarse si el sistema operativo de la plantilla no está actualizado.

## <a name="prepare-and-protect-the-vhdx-with-the-template-disk-wizard"></a>Preparación y protección del VHDX con el Asistente para crear un disco de plantilla

Para usar un disco de plantilla con máquinas virtuales blindadas, el disco debe estar preparado y cifrarse con BitLocker mediante el Asistente para creación de discos de plantilla blindada. Este asistente generará un hash para el disco y lo agregará a un catálogo de firmas de volumen (VSC). El VSC se firma con un certificado que especifique y se usa durante el proceso de aprovisionamiento para asegurarse de que el disco que se está implementando para un inquilino no se ha modificado o reemplazado por un disco en el que el inquilino no confía. Por último, BitLocker se instala en el sistema operativo del disco (si aún no lo está) para preparar el disco para el cifrado durante el aprovisionamiento de la máquina virtual.

> [!NOTE]
> El Asistente para crear un disco de plantilla modificará el disco de plantilla que especifique en contexto. Es posible que desee realizar una copia del VHDX desprotegido antes de ejecutar el Asistente para realizar actualizaciones en el disco más adelante. No podrá modificar un disco protegido con el Asistente para crear disco de plantilla.

Siga los pasos que se describen a continuación en un equipo que ejecute Windows Server 2016, Windows 10 (con herramientas de administración remota del servidor, RSAT instaladas) o posterior (no es necesario que sea un host protegido o un servidor VMM):

1. Copie el VHDX generalizado creado en [preparar un vhdx de sistema operativo](#prepare-an-operating-system-vhdx) en el servidor, si aún no está allí.

2. Para administrar el servidor de forma local, instale la característica **herramientas de máquinas virtuales blindadas** desde **herramientas de administración remota del servidor** en el servidor.

        Install-WindowsFeature RSAT-Shielded-VM-Tools -Restart
        
    También puede administrar el servidor desde un equipo cliente en el que haya instalado el [herramientas de administración remota del servidor de Windows 10](https://www.microsoft.com/download/details.aspx?id=45520).

3. Obtenga o cree un certificado para firmar el VSC del VHDX que se convertirá en el disco de plantilla para nuevas máquinas virtuales blindadas. Los detalles sobre este certificado se mostrarán a los inquilinos cuando creen sus archivos de datos de blindaje y estén autorizando los discos en los que confían. Por lo tanto, es importante obtener este certificado de una entidad de certificación de confianza mutua para el usuario y los inquilinos. En escenarios empresariales en los que se encuentre el anfitrión y el inquilino, puede considerar la posibilidad de emitir este certificado desde su PKI.

    Si está configurando un entorno de prueba y simplemente desea utilizar un certificado autofirmado para preparar el disco de plantilla, ejecute un comando similar al siguiente:

        New-SelfSignedCertificate -DnsName publisher.fabrikam.com

4. Inicie el **Asistente para crear un disco de plantilla** desde la carpeta **herramientas administrativas** del menú Inicio o escriba **TemplateDiskWizard. exe** en un símbolo del sistema.

5. En la página **certificado** , haga clic en **examinar** para mostrar una lista de certificados. Seleccione el certificado con el que desea preparar la plantilla de disco. Haga clic en **Aceptar** y, a continuación, en **Siguiente**.

6. En la página disco virtual, haga clic en **examinar** para seleccionar el VHDX que ha preparado y, a continuación, haga clic en **siguiente**.

7. En la página Catálogo de firmas, proporcione un **nombre** y una **versión** de disco descriptivos. Estos campos están presentes para ayudarle a identificar el disco una vez preparado.

    Por ejemplo, para **el nombre del disco** podría escribir _WS2016_ y para la **versión**, _1.0.0.0_

8. Revise las selecciones en la página revisar configuración del asistente. Al hacer clic en **generar**, el asistente habilitará BitLocker en el disco de plantilla, calculará el hash del disco y creará el catálogo de firmas de volumen, que se almacena en los metadatos de VHDX.

    Espere hasta que finalice el proceso de preparación antes de intentar montar o trasladar el disco de plantilla. Este proceso puede tardar varios minutos en completarse, en función del tamaño del disco.

    > [!IMPORTANT]
    > Los discos de plantilla solo se pueden usar con el proceso de aprovisionamiento seguro de máquinas virtuales blindadas.
    > Si intenta arrancar una máquina virtual normal (sin blindar) con un disco de plantilla, es probable que se produzca un error de detención (pantalla azul) y no se admita.

9. En la página **Resumen** , se muestra información sobre la plantilla de disco, el certificado usado para firmar el VSC y el emisor de certificados. Haz clic en **Cerrar** para salir del asistente.

Si usa VMM, siga los pasos descritos en las secciones restantes de este tema para incorporar un disco de plantilla a una plantilla de máquina virtual blindada en VMM. 

## <a name="copy-the-template-disk-to-the-vmm-library"></a>Copiar el disco de plantilla en la biblioteca VMM

Si usa VMM, después de crear un disco de plantilla, debe copiarlo en un recurso compartido de biblioteca VMM para que los hosts puedan descargar y usar el disco al aprovisionar nuevas máquinas virtuales. Use el procedimiento siguiente para copiar el disco de plantilla en la biblioteca VMM y, a continuación, actualice la biblioteca.

1. Copie el archivo VHDX en la carpeta de recursos compartidos de biblioteca de VMM. Si usó la configuración predeterminada de VMM, copie el disco de plantilla en _\\<vmmserver>\MSSCVMMLibrary\VHDs_.

2. Actualice el servidor de biblioteca. Abra el área de trabajo **biblioteca** , expanda **servidores de biblioteca**, haga clic con el botón secundario en el servidor de biblioteca que desee actualizar y haga clic en **Actualizar**.

3. A continuación, proporcione a VMM información sobre el sistema operativo instalado en el disco de plantilla:

    a. Busque el disco de plantilla recién importado en el servidor de biblioteca en el área de trabajo **biblioteca** .

    b. Haga clic con el botón secundario en el disco y seleccione **propiedades**.

    c. En **sistema operativo**, expanda la lista y seleccione el sistema operativo instalado en el disco. Al seleccionar un sistema operativo se indica a VMM que el VHDX no está en blanco.

    d. Si ha actualizado las propiedades, haga clic en **Aceptar**.

El icono de escudo pequeño situado junto al nombre del disco indica el disco como un disco de plantilla preparado para las máquinas virtuales blindadas. También puede hacer clic con el botón derecho en los encabezados de columna y alternar la columna **blindada** para ver una representación textual que indica si un disco está diseñado para implementaciones de máquinas virtuales normales o blindadas.

![Disco de plantilla de VM blindada](../media/Guarded-Fabric-Shielded-VM/shielded-vm-template-disk.png)

## <a name="create-the-shielded-vm-template-in-vmm-using-the-prepared-template-disk"></a>Creación de la plantilla de máquina virtual blindada en VMM mediante el disco de plantilla preparado

Con un disco de plantilla preparado en la biblioteca VMM, está listo para crear una plantilla de máquina virtual para máquinas virtuales blindadas. Las plantillas de máquina virtual para máquinas virtuales blindadas difieren ligeramente de las plantillas de máquina virtual tradicionales en que se han corregido ciertos valores de configuración (máquina virtual de segunda generación, UEFI y arranque seguro habilitados, etc.) y otros no están disponibles (la personalización de inquilinos se limita a algunos, seleccione Propiedades de la máquina virtual). Para crear la plantilla de máquina virtual, siga estos pasos:

1. En el área de trabajo **biblioteca** , haga clic en **Crear plantilla de VM** en la pestaña Inicio en la parte superior.

2. En la página **Seleccionar origen**, haga clic en **Usar una plantilla de VM o un disco duro virtual existentes almacenados en la biblioteca** y, a continuación, haga clic en **Examinar**.

3. En la ventana que aparece, seleccione un disco de plantilla preparado de la biblioteca VMM. Para identificar más fácilmente los discos preparados, haga clic con el botón derecho en un encabezado de columna y habilite la columna **blindada** . Haga clic en **Aceptar** y **siguiente**.

4. Especifique un nombre de plantilla de máquina virtual y, opcionalmente, una descripción y, a continuación, haga clic en **siguiente**.

5. En la página **configurar hardware** , especifique las capacidades de las máquinas virtuales creadas a partir de esta plantilla. Asegúrese de que haya al menos una NIC disponible y configurada en la plantilla de máquina virtual. La única manera de que un inquilino se conecte a una máquina virtual blindada es a través de Conexión a Escritorio remoto, Administración remota de Windows u otras herramientas de administración remota preconfiguradas que funcionan a través de protocolos de red.

    Si opta por aprovechar los grupos de direcciones IP estáticas en VMM en lugar de ejecutar un servidor DHCP en la red de inquilinos, deberá avisar a los inquilinos de esta configuración. Cuando un inquilino suministra su archivo de datos de blindaje, que contiene el archivo de instalación desatendida para VMM, deberá proporcionar valores de marcador de posición especiales para la información del grupo de direcciones IP estáticas. Para obtener más información sobre los marcadores de posición de VMM en archivos de instalación desatendida de inquilinos, vea [crear un archivo de respuesta](guarded-fabric-tenant-creates-shielding-data.md#create-an-answer-file). 

6. En la página **configurar sistema operativo** , VMM solo mostrará algunas opciones para las máquinas virtuales blindadas, incluida la clave de producto, la zona horaria y el nombre del equipo. El inquilino especifica información segura, como la contraseña de administrador y el nombre de dominio, a través de un archivo de datos de blindaje (. Archivo PDK). 

    > [!NOTE]
    > Si decide especificar una clave de producto en esta página, asegúrese de que sea válida para el sistema operativo en el disco de plantilla. Si se usa una clave de producto incorrecta, se producirá un error en la creación de la máquina virtual.

Una vez creada la plantilla, los inquilinos pueden usarla para crear nuevas máquinas virtuales. Tendrá que comprobar que la plantilla de máquina virtual es uno de los recursos disponibles para el rol de usuario Administrador de inquilinos (en VMM, los roles de usuario se encuentran en el área de trabajo **configuración** ).

## <a name="prepare-and-protect-the-vhdx-using-powershell"></a>Preparación y protección del VHDX con PowerShell

Como alternativa a la ejecución del Asistente para crear un disco de plantilla, puede copiar el disco de plantilla y el certificado en un equipo que ejecute RSAT y ejecutar [Protect-TemplateDisk](https://docs.microsoft.com/powershell/module/shieldedvmtemplate/protect-templatedisk?view=win10-ps
) para iniciar el proceso de firma.
En el ejemplo siguiente se usa el nombre y la información de versión especificados por los parámetros _TemplateName_ y _version_ .
El VHDX que proporcione al parámetro `-Path` se sobrescribirá con el disco de plantilla actualizado, por lo que debe asegurarse de realizar una copia antes de ejecutar el comando.

```powershell
# Replace "THUMBPRINT" with the thumbprint of your template disk signing certificate in the line below
$certificate = Get-Item Cert:\LocalMachine\My\THUMBPRINT

Protect-TemplateDisk -Certificate $certificate -Path "WindowsServer2019-ShieldedTemplate.vhdx" -TemplateName "Windows Server 2019" -Version 1.0.0.0
```

El disco de plantilla ya está listo para su uso para aprovisionar máquinas virtuales blindadas.
Si usa System Center Virtual Machine Manager para implementar la máquina virtual, ahora puede copiar el VHDX en la biblioteca de VMM.

También puede que desee extraer el catálogo de firmas de volumen del VHDX.
Este archivo se usa para proporcionar información sobre el certificado de firma, el nombre del disco y la versión a los propietarios de máquinas virtuales que desean usar la plantilla.
Deben importar este archivo en el Asistente para archivos de datos de blindaje con el fin de autorizarle, el autor de la plantilla en posesión del certificado de firma, para crear este y futuros discos de plantilla para ellos.

Para extraer el catálogo de firmas de volumen, ejecute el siguiente comando en PowerShell:

```powershell
Save-VolumeSignatureCatalog -TemplateDiskPath 'C:\temp\MyLinuxTemplate.vhdx' -VolumeSignatureCatalogPath 'C:\temp\MyLinuxTemplate.vsc'
```


## <a name="next-step"></a>Paso siguiente

> [!div class="nextstepaction"]
> [Creación de un archivo de datos de blindaje](guarded-fabric-tenant-creates-shielding-data.md)

## <a name="see-also"></a>Vea también

- [Pasos de configuración del proveedor de servicios de hospedaje para hosts protegidos y máquinas virtuales blindadas](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [VM blindadas y tejido protegido](guarded-fabric-and-shielded-vms-top-node.md)
