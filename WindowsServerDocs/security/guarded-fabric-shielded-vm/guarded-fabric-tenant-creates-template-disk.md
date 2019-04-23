---
title: 'Máquinas virtuales blindadas para inquilinos: creación de un disco de plantilla: opcional'
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: c1992f8b-6f88-4dbc-b4a5-08368bba2787
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 2709c84a16dadc2211af4a6f5b43c13266ede155
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874636"
---
# <a name="shielded-vms-for-tenants---creating-a-template-disk-optional"></a>Máquinas virtuales blindadas para inquilinos: creación de un disco de plantilla (opcional)

>Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016

Para crear una nueva máquina virtual blindada, deberá usar un disco de plantilla firmada, especialmente preparado. Los metadatos de los discos de plantilla firmada ayuda a garantizar que los discos no se modifican después de haber creado y permite como un inquilino para restringir qué discos se puede usar para crear las máquinas virtuales blindadas. Una manera de proporcionar este disco es para usted, el inquilino para crearlo, tal como se describe en este tema. 

> [!IMPORTANT]
> Si lo prefiere, puede usar en su lugar un disco de plantilla proporcionado por el proveedor de servicios de hospedaje. Si lo hace, es importante implementar una prueba de máquina virtual con ese disco de plantilla y ejecutar sus propias herramientas (antivirus, detectores de vulnerabilidades etc.) para validar el disco es, de hecho, en un estado de confianza.

## <a name="prepare-an-operating-system-vhdx"></a>Preparar un VHDX de sistema operativo

Para crear un disco de plantilla blindada, deberá preparar un disco del sistema operativo que se va a ejecutar el Asistente de disco de plantilla. Se usará este disco como disco del SO en máquinas virtuales blindadas. Puede usar las herramientas existentes para crear este disco, como Microsoft Desktop Image Service Manager (DISM), o configurar una máquina virtual con un VHDX en blanco e instale manualmente el sistema operativo en ese disco. Al configurar el disco, debe cumplir los requisitos siguientes que son específicas de generación 2 o máquinas virtuales blindadas: 

| Requisito de VHDX | Razón |
|-----------|----|
|Debe ser un disco de tabla de particiones GUID (GPT) | Es necesario para que las máquinas virtuales de generación 2 admitan UEFI|
|Tipo de disco debe ser **básica** en contraposición a **dinámica**. <br>Nota: Esto hace referencia al tipo de disco lógico, no la característica VHDX "expansión dinámica" compatible con Hyper-V. | BitLocker no admite discos dinámicos.|
|El disco tiene al menos dos particiones. Una partición debe incluir la unidad donde está instalado Windows. Esta es la unidad que va cifrar BitLocker. La otra partición es la partición activa, que contiene el cargador de arranque y permanece sin cifrar para que se puede iniciar el equipo.|Es necesario para BitLocker|
|Es de sistema de archivos NTFS | Es necesario para BitLocker|
|El sistema operativo instalado en el VHDX es uno de los siguientes:<br>-Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 <br>-Windows 10, Windows 8.1, Windows 8| Es necesario para admitir máquinas virtuales de generación 2 y la plantilla de arranque seguro de Microsoft|
|Sistema operativo debe ser generalizado (ejecute sysprep.exe) | El aprovisionamiento de la plantilla implica especializado máquinas virtuales para cargas de trabajo de un inquilino específico| 

> [!NOTE]
> No copie el disco de plantilla en la biblioteca de VMM en esta fase. 

### <a name="required-packages-to-create-a-nano-server-template-disk"></a>Los paquetes para crear un disco de plantilla de Nano Server necesarios

Si planea ejecutar Nano Server como el sistema operativo invitado en máquinas virtuales blindadas, debe asegurarse de que la imagen de Nano Server incluye los siguientes paquetes:

- Microsoft-NanoServer-Guest-Package
- Microsoft-NanoServer-SecureStartup-Package

## <a name="run-windows-update-on-the-template-operating-system"></a>Ejecute Windows Update en el sistema operativo de plantilla

En el disco de plantilla, compruebe que el sistema operativo tiene todas las actualizaciones más recientes de Windows instalado. Recientemente, las actualizaciones publicadas mejoran la confiabilidad del proceso de blindaje-to-end: un proceso que se producirán errores como completa si el sistema operativo de plantilla no está actualizado.

## <a name="sign-and-protect-the-vhdx-with-the-template-disk-wizard"></a>Inicie sesión y proteger el VHDX con el Asistente de disco de plantilla

Para usar un disco de plantilla con las máquinas virtuales blindadas, el disco debe firmarse y cifrado con BitLocker. Para ello, usará al Asistente de creación blindadas disco de plantilla. Este asistente generará un valor hash para el disco y agregarlo a un catálogo de firmas de volumen (VSC). La VSC está firmado con un certificado que especifique y se usa durante el proceso de aprovisionamiento para garantizar el disco que se implementa para un inquilino no se ha modificado o reemplazado por un disco que no confía en el inquilino. Por último, BitLocker se instala en el sistema de operativo del disco (si aún no está allí) para preparar el disco para el cifrado durante el aprovisionamiento de máquinas virtuales.

> [!NOTE]
> El Asistente de disco de plantilla va a modificar el disco de plantilla que se especifique en contexto. Es posible que desee realizar una copia de VHDX no protegido antes de ejecutar el Asistente para realizar actualizaciones en el disco en un momento posterior. No podrá modificar un disco que se ha protegido con el Asistente de disco de plantilla.

En un equipo que ejecuta Windows Server 2016 (no es necesario ser un host protegido o el servidor VMM), realice los pasos siguientes:

1. Copie el VHDX generalizado que creó en [preparar un sistema operativo VHDX](#prepare-an-operating-system-vhdx) al servidor, si aún no está.

2. Instalar el **herramientas VM blindadas** característica **herramientas de administración remota del servidor** en el equipo.

        Install-WindowsFeature RSAT-Shielded-VM-Tools -Restart

3. Obtener o crear un certificado para firmar el VHDX que se convertirá en el disco de plantilla para nuevas máquinas virtuales blindadas. Detalles acerca de este certificado se incorporará a un archivo de datos de blindaje, que autoriza el disco como un disco de confianza. Por lo tanto, es importante obtener este certificado desde una entidad de certificación que y el hospedaje del servicio confianza de proveedor. En los escenarios empresariales donde está el proveedor de hospedaje y el inquilino, es posible que considere la posibilidad de emitir este certificado de la PKI.

    Si va a configurar un entorno de prueba y de querer usar un certificado autofirmado para firmar el disco de plantilla, ejecute un comando similar al siguiente en el equipo:

        New-SelfSignedCertificate -DnsName publisher.fabrikam.com

4. Iniciar el **Asistente de disco de plantilla** desde el **herramientas administrativas** carpeta en el menú Inicio o escribiendo **TemplateDiskWizard.exe** en un símbolo del sistema.

5. En el **certificado** página, haga clic en **examinar** para mostrar una lista de certificados. Seleccione el certificado para firmar la plantilla de disco. Haga clic en **Aceptar** y, a continuación, en **Siguiente**.

6. En la página de disco Virtual, haga clic en **examinar** para seleccionar el VHDX que haya preparado, a continuación, haga clic en **siguiente**.

7. En la página Catálogo de firmas, proporcionar descriptivo **nombre del disco** y **versión.** Estos campos están presentes para ayudarle a identificar el disco una vez que se ha firmado.

    Por ejemplo, para **nombre del disco** podría escribir _WS2016_ y **versión**, _1.0.0.0_

8. Revise las selecciones en la página Revisar la configuración del asistente. Al hacer clic en **generar**, el asistente se habilite BitLocker en el disco de plantilla, calcular el hash del disco y crear el catálogo de firmas de volumen, que se almacena en los metadatos VHDX.

    Espere a que finalice el proceso de firma antes de intentar montar o mover el disco de plantilla. Este proceso puede tardar un rato en completarse, según el tamaño del disco. 

9. En el **resumen** página, información sobre la plantilla de disco, el certificado usado para firmar la plantilla, y se muestra el emisor del certificado. Haga clic en **Cerrar** para salir del asistente.


Proporcionar la plantilla de disco blindado al proveedor de servicios de hospedaje, junto con un archivo de datos de blindaje que cree, como se describe en [crear datos para definir una máquina virtual blindada de blindaje](guarded-fabric-tenant-creates-shielding-data.md).

## <a name="see-also"></a>Vea también

- [Implementar máquinas virtuales blindadas](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [Las máquinas virtuales blindadas y tejido protegido](guarded-fabric-and-shielded-vms-top-node.md)
