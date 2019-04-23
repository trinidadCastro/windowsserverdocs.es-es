---
title: Crear un disco de plantilla de máquina virtual blindado de Windows
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 9c8b84e8-1f5a-47a1-83ca-b1dbd801cb0b
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 01/29/2019
ms.openlocfilehash: e00322186ea34784048366bf17881af742cb4444
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853696"
---
# <a name="create-a-windows-shielded-vm-template-disk"></a>Crear un disco de plantilla de máquina virtual blindado de Windows

>Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016

Como con las máquinas virtuales normales, puede crear una plantilla de máquina virtual (por ejemplo, un [plantilla de máquina virtual en Virtual Machine Manager (VMM)](https://technet.microsoft.com/system-center-docs/vmm/manage/manage-library-add-vm-templates)) para que sea fácil para los inquilinos y los administradores para implementar nuevas máquinas virtuales en el tejido con un disco de plantilla. Dado que las máquinas virtuales blindadas están activos confidenciales, existen pasos adicionales para crear una plantilla de máquina virtual que admite el blindaje. En este tema se describe los pasos para crear un disco de plantilla blindado y una plantilla de máquina virtual en VMM.

Para entender cómo encaja este tema en el proceso general de la implementación de máquinas virtuales blindadas, consulte [hospeda los pasos de configuración del proveedor de servicio para hosts protegidos y máquinas virtuales blindadas](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md).

## <a name="prepare-an-operating-system-vhdx"></a>Preparar un VHDX de sistema operativo

En primer lugar, prepare un disco del sistema operativo que, a continuación, ejecutará a través del Asistente de creación de disco de plantilla blindada. Se usará este disco como disco del SO en máquinas virtuales del inquilino. Puede usar las herramientas existentes para crear este disco, como Microsoft Desktop Image Service Manager (DISM), o configurar una máquina virtual con un VHDX en blanco e instale manualmente el sistema operativo en ese disco. Al configurar el disco, debe cumplir los requisitos siguientes que son específicas de generación 2 o máquinas virtuales blindadas: 

| Requisito de VHDX | Razón |
|-----------|----|
|Debe ser un disco de tabla de particiones GUID (GPT) | Es necesario para que las máquinas virtuales de generación 2 admitan UEFI|
|Tipo de disco debe ser **básica** en contraposición a **dinámica**. <br>Nota: Esto hace referencia al tipo de disco lógico, no la característica VHDX "expansión dinámica" compatible con Hyper-V. | BitLocker no admite discos dinámicos.|
|El disco tiene al menos dos particiones. Una partición debe incluir la unidad donde está instalado Windows. Esta es la unidad que va cifrar BitLocker. La otra partición es la partición activa, que contiene el cargador de arranque y permanece sin cifrar para que se puede iniciar el equipo.|Es necesario para BitLocker|
|Es de sistema de archivos NTFS | Es necesario para BitLocker|
|El sistema operativo instalado en el VHDX es uno de los siguientes:<br>-Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 <br>-Windows 10, Windows 8.1, Windows 8| Es necesario para admitir máquinas virtuales de generación 2 y la plantilla de arranque seguro de Microsoft|
|Sistema operativo debe ser generalizado (ejecute sysprep.exe) | El aprovisionamiento de la plantilla implica especializado máquinas virtuales para cargas de trabajo de un inquilino específico| 

> [!NOTE]
> Si usa VMM, no copie el disco de plantilla en la biblioteca de VMM en esta fase. 

## <a name="run-windows-update-on-the-template-operating-system"></a>Ejecute Windows Update en el sistema operativo de plantilla

En el disco de plantilla, compruebe que el sistema operativo tiene todas las actualizaciones más recientes de Windows instalado. Recientemente, las actualizaciones publicadas mejoran la confiabilidad del proceso de blindaje-to-end: un proceso que se producirán errores como completa si el sistema operativo de plantilla no está actualizado.

## <a name="prepare-and-protect-the-vhdx-with-the-template-disk-wizard"></a>Preparar y proteger el VHDX con el Asistente de disco de plantilla

Para usar un disco de plantilla con las máquinas virtuales blindadas, debe preparar el disco y se cifrados con BitLocker por con el Asistente de creación de disco de plantilla blindada. Este asistente generará un valor hash para el disco y agregarlo a un catálogo de firmas de volumen (VSC). La VSC está firmado con un certificado que especifique y se usa durante el proceso de aprovisionamiento para garantizar el disco que se implementa para un inquilino no se ha modificado o reemplazado por un disco que no confía en el inquilino. Por último, BitLocker se instala en el sistema de operativo del disco (si aún no está allí) para preparar el disco para el cifrado durante el aprovisionamiento de máquinas virtuales.

> [!NOTE]
> El Asistente de disco de plantilla va a modificar el disco de plantilla que se especifique en contexto. Es posible que desee realizar una copia de VHDX no protegido antes de ejecutar el Asistente para realizar actualizaciones en el disco en un momento posterior. No podrá modificar un disco que se ha protegido con el Asistente de disco de plantilla.

En un equipo que ejecuta Windows Server 2016 (no es necesario ser un host protegido o un servidor VMM), realice los pasos siguientes:

1. Copie el VHDX generalizado que creó en [preparar un sistema operativo VHDX](#prepare-an-operating-system-vhdx) al servidor, si aún no está.

2. Para administrar el servidor local, instale el **herramientas VM blindadas** característica **herramientas de administración remota del servidor** en el servidor.

        Install-WindowsFeature RSAT-Shielded-VM-Tools -Restart
        
    También puede administrar el servidor desde un equipo cliente en el que ha instalado el [herramientas de administración de servidor remoto de Windows 10](https://www.microsoft.com/en-us/download/details.aspx?id=45520).

3. Obtener o crear un certificado para firmar la VSC para el VHDX que se convertirá en el disco de plantilla para nuevas máquinas virtuales blindadas. Detalles acerca de este certificado se mostrará a los inquilinos cuando crean sus archivos de datos de blindaje y se autoriza que confían los discos. Por lo tanto, es importante obtener dicho certificado de una entidad de certificación confianza mutua entre usted y sus inquilinos. En los escenarios empresariales donde está el proveedor de hospedaje y el inquilino, es posible que considere la posibilidad de emitir este certificado de la PKI.

    Si va a configurar un entorno de prueba y tan solo quiere usar un certificado autofirmado para preparar el disco de plantilla, ejecute un comando similar al siguiente:

        New-SelfSignedCertificate -DnsName publisher.fabrikam.com

4. Iniciar el **Asistente de disco de plantilla** desde el **herramientas administrativas** carpeta en el menú Inicio o escribiendo **TemplateDiskWizard.exe** en un símbolo del sistema.

5. En el **certificado** página, haga clic en **examinar** para mostrar una lista de certificados. Seleccione el certificado con la que se va a preparar la plantilla de disco. Haga clic en **Aceptar** y, a continuación, en **Siguiente**.

6. En la página de disco Virtual, haga clic en **examinar** para seleccionar el VHDX que haya preparado, a continuación, haga clic en **siguiente**.

7. En la página Catálogo de firmas, proporcionar descriptivo **nombre del disco** y **versión.** Estos campos están presentes para ayudarle a identificar el disco una vez que se ha preparado.

    Por ejemplo, para **nombre del disco** podría escribir _WS2016_ y **versión**, _1.0.0.0_

8. Revise las selecciones en la página Revisar la configuración del asistente. Al hacer clic en **generar**, el asistente se habilite BitLocker en el disco de plantilla, calcular el hash del disco y crear el catálogo de firmas de volumen, que se almacena en los metadatos VHDX.

    Espere a que finalice el proceso de preparación antes de intentar montar o mover el disco de plantilla. Este proceso puede tardar un rato en completarse, según el tamaño del disco.

    > [!IMPORTANT]
    > Discos de plantilla solo pueden usarse con la máquina virtual blindada segura el proceso de aprovisionamiento.
    > Intentando arrancar normal (sin protección) máquina virtual con un disco de plantilla se probablemente se producirá un error de detención (pantalla azul) y no se admite.

9. En el **resumen** página, información sobre la plantilla de disco, el certificado usado para firmar la VSC, y se muestra el emisor del certificado. Haga clic en **Cerrar** para salir del asistente.

Si usa VMM, siga los pasos descritos en las secciones restantes de este tema para incorporar un disco de plantilla en una plantilla de VM blindada en VMM. 

## <a name="copy-the-template-disk-to-the-vmm-library"></a>Copie el disco de plantilla en la biblioteca de VMM

Si usa VMM, después de crear un disco de plantilla, debe copiarlo en un recurso compartido de biblioteca VMM para que hosts pueden descargar y usar el disco durante el aprovisionamiento de nuevas máquinas virtuales. Utilice el procedimiento siguiente para copiar el disco de plantilla en la biblioteca de VMM y, a continuación, actualice la biblioteca.

1. Copie el archivo VHDX a la carpeta de recurso compartido de biblioteca VMM. Si usa la configuración de VMM de forma predeterminada, copie el disco de plantilla para  _\\ <vmmserver>\MSSCVMMLibrary\VHDs_.

2. Actualice al servidor de biblioteca. Abra el **biblioteca** área de trabajo, expanda **servidores de biblioteca**, haga doble clic en el servidor de biblioteca que desea actualizar y haga clic en **actualizar**.

3. A continuación, proporcione información sobre el sistema operativo instalado en el disco de plantilla VMM:

    a. Busque el disco de plantilla recién importadas en el servidor de biblioteca en el **biblioteca** área de trabajo.

    b. Haga clic en el disco y, a continuación, haga clic en **propiedades**.

    c. Para **del sistema operativo**, expanda la lista y seleccione el sistema operativo instalado en el disco. Al seleccionar un sistema operativo indica a VMM que VHDX no está en blanco.

    d. Si ha actualizado las propiedades, haga clic en **Aceptar**.

El icono de escudo pequeña situada junto al nombre del disco denota el disco como un disco de plantilla preparada para las máquinas virtuales blindadas. También puede hacer a la derecha clic en los encabezados de columna y alternar el **blindadas** columna para ver una representación de texto que indica si un disco está pensado para las implementaciones de máquina virtual blindadas o regulares.

![Disco de plantilla de máquina virtual blindada](../media/Guarded-Fabric-Shielded-VM/shielded-vm-template-disk.png)

## <a name="create-the-shielded-vm-template-in-vmm-using-the-prepared-template-disk"></a>Crear la plantilla de VM blindada en VMM mediante el disco de plantilla preparada

Con un disco de plantilla preparada en la biblioteca VMM, está listo para crear una plantilla de máquina virtual para máquinas virtuales blindadas. Plantillas de máquina virtual para las máquinas virtuales blindadas se diferencian ligeramente de plantillas de máquinas virtuales tradicionales en que ciertas opciones de configuración son fijos (generación 2 máquinas virtuales, UEFI y arranque seguro habilitan etc.) y otros no están disponibles (personalización de inquilino está limitado a unas, seleccione Propiedades de la máquina virtual) . Para crear la plantilla de máquina virtual, realice los pasos siguientes:

1. En el **biblioteca** área de trabajo, haga clic en **crear plantillas de VM** en la ficha Inicio en la parte superior.

2. En la página **Seleccionar origen**, haga clic en **Usar una plantilla de VM o un disco duro virtual existentes almacenados en la biblioteca** y, a continuación, haga clic en **Examinar**.

3. En la ventana que aparece, seleccione un disco de plantilla preparada en la biblioteca VMM. Para identificar más fácilmente qué discos están preparados, haga clic en un encabezado de columna y habilitar el **blindadas** columna. Haga clic en **Aceptar** , a continuación, **siguiente**.

4. Especifique un nombre de la plantilla de máquina virtual y, opcionalmente, una descripción y, a continuación, haga clic en **siguiente**.

5. En el **configurar Hardware** , especifique las capacidades de las máquinas virtuales creadas a partir de esta plantilla. Asegúrese de que al menos una NIC está disponible y configurado en la plantilla de máquina virtual. La única forma de un inquilino para conectarse a una máquina virtual blindada es a través de la conexión a Escritorio remoto, administración remota de Windows u otras herramientas de administración remota preconfigurada que funcionan a través de protocolos de red.

    Si elige aprovechar los grupos de IP estáticas en VMM en lugar de ejecutar un servidor DHCP en la red de inquilinos, necesita los inquilinos a esta configuración de alerta. Cuando un inquilino proporciona su archivo de datos de blindaje, que contiene el archivo de instalación desatendida para el servidor VMM, debe proporcionar los valores de marcador de posición especial para la información de grupo IP estática. Para obtener más información acerca de los marcadores de posición VMM en los archivos de instalación desatendida de inquilino, consulte [crear un archivo de respuesta](guarded-fabric-tenant-creates-shielding-data.md#create-an-answer-file). 

6. En el **configurar sistema operativo** página, VMM mostrará sólo unas cuantas opciones para las máquinas virtuales blindadas, incluida la clave de producto, la zona horaria y el nombre del equipo. Alguna información segura, como el nombre de dominio y contraseña de administrador, especificado por el inquilino a través de un archivo de datos de blindaje (. Archivo PDK). 

    > [!NOTE]
    > Si decide especificar una clave de producto en esta página, asegúrese de que sea válido para el sistema operativo en el disco de plantilla. Si se usa una clave de producto incorrecta, se producirá un error en la creación de máquinas virtuales.

Una vez creada la plantilla, los inquilinos pueden usar para crear nuevas máquinas virtuales. Deberá comprobar que la plantilla de máquina virtual es uno de los recursos disponibles para el rol de usuario de administrador de inquilinos (en VMM, los roles de usuario están en el **configuración** área de trabajo).

## <a name="prepare-and-protect-the-vhdx-using-powershell"></a>Preparar y proteger el VHDX mediante PowerShell

Como alternativa a ejecutar el Asistente de disco de plantilla, puede copiar el disco de plantilla y el certificado en un equipo que ejecuta RSAT y ejecutar [proteger TemplateDisk](https://docs.microsoft.com/powershell/module/shieldedvmtemplate/protect-templatedisk?view=win10-ps
) para iniciar el proceso de firma.
En el ejemplo siguiente se usa la información de nombre y la versión especificada por el _TemplateName_ y _versión_ parámetros.
El VHDX se proporciona a los `-Path` parámetro se sobrescribirá con el disco de plantilla actualizada, así que asegúrese de realizar una copia antes de ejecutar el comando.

```powershell
# Replace "THUMBPRINT" with the thumbprint of your template disk signing certificate in the line below
$certificate = Get-Item Cert:\LocalMachine\My\THUMBPRINT

Protect-TemplateDisk -Certificate $certificate -Path "WindowsServer2019-ShieldedTemplate.vhdx" -TemplateName "Windows Server 2019" -Version 1.0.0.0
```

El disco de plantilla ahora está listo para usarse para aprovisionar máquinas virtuales blindadas.
Si usa System Center Virtual Machine Manager para implementar la máquina virtual, ahora puede copiar el VHDX a la biblioteca VMM.

También puede extraer el catálogo de firmas de volumen de VHDX.
Este archivo se usa para proporcionar información sobre el certificado de firma, el nombre del disco y la versión a los propietarios de la máquina virtual que deseen usar la plantilla.
Debe importar este archivo en el Asistente de archivo de datos de blindaje para autorizar a usted, el autor de la plantilla en posesión del certificado de firmado, para crear este y discos de plantilla futuras para ellos.

Para extraer el catálogo de firmas de volumen, ejecute el siguiente comando en PowerShell:

```powershell
Save-VolumeSignatureCatalog -TemplateDiskPath 'C:\temp\MyLinuxTemplate.vhdx' -VolumeSignatureCatalogPath 'C:\temp\MyLinuxTemplate.vsc'
```


## <a name="next-step"></a>Paso siguiente

>[!div class="nextstepaction"]
[Crear un archivo de datos de blindaje](guarded-fabric-tenant-creates-shielding-data.md)

## <a name="see-also"></a>Vea también

- [Hospedaje de los pasos de configuración del proveedor de servicio para hosts protegidos y máquinas virtuales blindadas](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [Las máquinas virtuales blindadas y tejido protegido](guarded-fabric-and-shielded-vms-top-node.md)
