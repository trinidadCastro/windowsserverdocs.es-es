---
description: 'Más información sobre: máquinas virtuales blindadas para inquilinos: creación de un disco de plantilla (opcional)'
title: 'Máquinas virtuales blindadas para inquilinos: creación de un disco de plantilla: opcional'
ms.topic: article
ms.assetid: c1992f8b-6f88-4dbc-b4a5-08368bba2787
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 08/29/2018
ms.openlocfilehash: 610b0a8d91564249ca03f57e8957bbe85770b74d
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97043795"
---
# <a name="shielded-vms-for-tenants---creating-a-template-disk-optional"></a>Máquinas virtuales blindadas para inquilinos: creación de un disco de plantilla (opcional)

>Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016

Para crear una nueva máquina virtual blindada, deberá usar un disco de plantilla firmado y preparado especialmente. Los metadatos de los discos de plantilla firmada ayudan a garantizar que los discos no se modifiquen después de que se hayan creado y le permitan como inquilino restringir qué discos se pueden usar para crear las máquinas virtuales blindadas. Una manera de proporcionar este disco es para usted, el inquilino, para crearlo, tal como se describe en este tema.

> [!IMPORTANT]
> Si lo prefiere, puede usar un disco de plantilla proporcionado por el proveedor de servicios de hosting. Si lo hace, es importante implementar una máquina virtual de prueba con ese disco de plantilla y ejecutar sus propias herramientas (antivirus, detectores de vulnerabilidades, etc.) para validar el disco, de hecho, en un estado en el que confíe.

## <a name="prepare-an-operating-system-vhdx"></a>Preparar un VHDX de sistema operativo

Para crear un disco de plantilla blindada, primero debe preparar un disco del sistema operativo que se ejecutará a través del Asistente para crear un disco de plantilla. Este disco se usará como disco del sistema operativo en las máquinas virtuales blindadas. Puede usar cualquier herramienta existente para crear este disco, como Microsoft Desktop Image Service Manager (DISM), o bien configurar manualmente una máquina virtual con un VHDX en blanco e instalar el sistema operativo en ese disco. Al configurar el disco, debe cumplir los siguientes requisitos específicos de la generación 2 o las máquinas virtuales blindadas:

| Requisito para VHDX | Motivo |
|-----------|----|
|Debe ser un disco de tabla de particiones GUID (GPT) | Necesario para que las máquinas virtuales de generación 2 admitan UEFI|
|El tipo de disco debe ser **básico** en lugar de **dinámico**. <br>Nota: Esto hace referencia al tipo de disco lógico, no a la característica VHDX de "expansión dinámica" compatible con Hyper-V. | BitLocker no admite discos dinámicos.|
|El disco tiene al menos dos particiones. Una partición debe incluir la unidad en la que está instalado Windows. Esta unidad será cifrada por BitLocker. La otra partición es la partición activa, que contiene el cargador de inicio y permanece sin cifrar para poder iniciar el equipo.|Necesario para BitLocker|
|El sistema de archivos es NTFS | Necesario para BitLocker|
|El sistema operativo instalado en VHDX es uno de los siguientes:<br>-Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 <br>-Windows 10, Windows 8.1, Windows 8| Necesario para admitir máquinas virtuales de generación 2 y la plantilla de arranque seguro de Microsoft|
|El sistema operativo debe estar generalizado (ejecutar sysprep.exe) | El aprovisionamiento de plantillas implica máquinas virtuales especializadas para la carga de trabajo de un inquilino específico|

> [!NOTE]
> No copie el disco de plantilla en la biblioteca VMM en esta fase.

### <a name="required-packages-to-create-a-nano-server-template-disk"></a>Paquetes necesarios para crear un disco de plantilla de nano Server

Si tiene previsto ejecutar nano Server como sistema operativo invitado en máquinas virtuales blindadas, debe asegurarse de que la imagen de nano Server incluye los siguientes paquetes:

- Microsoft-NanoServer-Guest-Package
- Microsoft-NanoServer-SecureStartup-Package

## <a name="run-windows-update-on-the-template-operating-system"></a>Ejecutar Windows Update en el sistema operativo de la plantilla

En el disco de plantilla, compruebe que el sistema operativo tiene instaladas todas las actualizaciones de Windows más recientes. Las actualizaciones publicadas recientemente mejoran la confiabilidad del proceso de blindaje de un extremo a otro: un proceso que puede no completarse si el sistema operativo de la plantilla no está actualizado.

## <a name="sign-and-protect-the-vhdx-with-the-template-disk-wizard"></a>Firmar y proteger el VHDX con el Asistente para crear un disco de plantilla

Para usar un disco de plantilla con máquinas virtuales blindadas, el disco debe estar firmado y cifrado con BitLocker. Para ello, usará el Asistente para creación de discos de plantilla blindada. Este asistente generará un hash para el disco y lo agregará a un catálogo de firmas de volumen (VSC). El VSC se firma con un certificado que especifique y se usa durante el proceso de aprovisionamiento para asegurarse de que el disco que se está implementando para un inquilino no se ha modificado o reemplazado por un disco en el que el inquilino no confía. Por último, BitLocker se instala en el sistema operativo del disco (si aún no lo está) para preparar el disco para el cifrado durante el aprovisionamiento de la máquina virtual.

> [!NOTE]
> El Asistente para crear un disco de plantilla modificará el disco de plantilla que especifique en contexto. Es posible que desee realizar una copia del VHDX desprotegido antes de ejecutar el Asistente para realizar actualizaciones en el disco más adelante. No podrá modificar un disco protegido con el Asistente para crear disco de plantilla.

Realice los pasos siguientes en un equipo con Windows Server 2016 (no es necesario que sea un host protegido o el servidor VMM):

1. Copie el VHDX generalizado creado en [preparar un vhdx de sistema operativo](#prepare-an-operating-system-vhdx) en el servidor, si aún no está allí.

2. Instale la característica **herramientas de máquinas virtuales blindadas** desde **herramientas de administración remota del servidor** en el equipo.

    ```
    Install-WindowsFeature RSAT-Shielded-VM-Tools -Restart
    ```

3. Obtenga o cree un certificado para firmar el VHDX que se convertirá en el disco de plantilla para nuevas máquinas virtuales blindadas. Los detalles sobre este certificado se incorporarán en un archivo de datos de blindaje, que autoriza el disco como un disco de confianza. Por lo tanto, es importante obtener este certificado de una entidad de certificación que usted y su proveedor de servicios de hosting confían. En escenarios empresariales en los que se encuentre el anfitrión y el inquilino, puede considerar la posibilidad de emitir este certificado desde su PKI.

    Si está configurando un entorno de prueba y solo quiere usar un certificado autofirmado para firmar el disco de plantilla, ejecute un comando similar al siguiente en el equipo:

    ```
    New-SelfSignedCertificate -DnsName publisher.fabrikam.com
    ```

4. Inicie el **Asistente para crear un disco de plantilla** desde la carpeta **herramientas administrativas** del menú Inicio o escriba **TemplateDiskWizard.exe** en el símbolo del sistema.

5. En la página **certificado** , haga clic en **examinar** para mostrar una lista de certificados. Seleccione el certificado con el que desea firmar la plantilla de disco. Haga clic en **Aceptar** y luego en **Siguiente**.

6. En la página disco virtual, haga clic en **examinar** para seleccionar el VHDX que ha preparado y, a continuación, haga clic en **siguiente**.

7. En la página Catálogo de firmas, proporcione un **nombre** y una **versión** de disco descriptivos. Estos campos están presentes para ayudarle a identificar el disco una vez que se ha firmado.

    Por ejemplo, para **el nombre del disco** podría escribir _WS2016_ y para la **versión**, _1.0.0.0_

8. Revise las selecciones en la página revisar configuración del asistente. Al hacer clic en **generar**, el asistente habilitará BitLocker en el disco de plantilla, calculará el hash del disco y creará el catálogo de firmas de volumen, que se almacena en los metadatos de VHDX.

    Espere hasta que finalice el proceso de firma antes de intentar montar o trasladar el disco de plantilla. Este proceso puede tardar varios minutos en completarse, en función del tamaño del disco.

9. En la página **Resumen** , se muestra información sobre la plantilla de disco, el certificado usado para firmar la plantilla y el emisor de certificados. Haga clic en **Cerrar** para salir del asistente.


Proporcione la plantilla de disco blindada al proveedor de servicios de hospedaje, junto con un archivo de datos de blindaje que cree, tal como se describe en [creación de datos de blindaje para definir una máquina virtual blindada](guarded-fabric-tenant-creates-shielding-data.md).

## <a name="additional-references"></a>Referencias adicionales

- [Implementar máquinas virtuales blindadas](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [VM blindadas y tejido protegido](guarded-fabric-and-shielded-vms-top-node.md)
