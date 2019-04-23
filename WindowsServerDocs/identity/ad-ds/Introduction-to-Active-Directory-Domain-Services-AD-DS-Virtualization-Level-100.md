---
ms.assetid: 7a3114c8-bda8-49bb-83a8-4e04340ab221
title: Introducción a la virtualización de los Servicios de dominio de Active Directory (AD DS) (nivel 100)
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: b818ba5a58db38bdb3c0f630a8d9d2daa1494403
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878096"
---
# <a name="introduction-to-active-directory-domain-services-ad-ds-virtualization-level-100"></a>Introducción a la virtualización de los Servicios de dominio de Active Directory (AD DS) (nivel 100)

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

El entorno de virtualización de los Servicios de dominio de Active Directory (AD DS) lleva ya unos cuantos años en activo. A partir de Windows Server 2012, AD DS proporciona mayor compatibilidad con virtualización de controladores de dominio mediante la introducción de las capacidades de virtualización segura.

## <a name="safe-virtualization-of-domain-controllers"></a>Virtualización segura de controladores de dominio

Los entornos virtuales presentan desafíos únicos para las cargas de trabajo distribuidas que dependen de un esquema de replicación basado en reloj lógico. Una replicación de AD DS, por ejemplo, utiliza un valor que aumenta de forma continua (denominado número de secuencias actualizadas o USN) y que se asigna a las transacciones en cada controlador de dominio. Instancia de base de datos de cada controlador de dominio también se proporciona una identidad, denominada InvocationID. El InvocationID, o identificador de invocación, de un controlador de dominio junto con su USN forman un identificador exclusivo que se asocia a cada transacción de escritura que se ejecuta en cada controlador de dominio y debe ser único en el bosque.

La replicación de AD DS utiliza el InvocationID y USN en cada controlador de dominio para determinar los cambios que es necesario replicar en otros controladores de dominio. Si un controlador de dominio se revierte en el tiempo sin conocimiento del controlador de dominio y se reutiliza un USN para una transacción completamente distinta, la replicación no convergerá debido a que otros controladores de dominio detectarán que ya han recibido las actualizaciones asociado al USN reutilizado en el contexto de ese InvocationID.

Por ejemplo, en la siguiente ilustración se muestra la secuencia de eventos que tiene lugar en Windows Server 2008 R2 y en sistemas operativos anteriores cuando se detecta una reversión de USN en VDC2, el controlador de dominio de destino que se ejecuta en una máquina virtual. En esta ilustración, se produce la detección de la reversión de USN en VDC2 cuando un asociado de replicación detecta que VDC2 envió un valor de actualización USN que había visto por el asociado de replicación, lo que indica que base de datos de VDC2 se revirtió en el tiempo de forma incorrecta.

![Introducción a AD DS](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/ADDS_Exampleofhowreplicationcanbecomeinconsistent.png)

Una máquina virtual (VM) facilita para los administradores del hipervisor revertir un dominio USN del controlador (su reloj lógico), por ejemplo, aplicar una instantánea sin conocimiento del controlador de dominio. Para obtener más información acerca de USN y la reversión de USN, incluida otra ilustración donde se muestran instancias no detectadas de la reversión de USN, consulte [USN y reversión de USN](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv(WS.10).aspx#usn_and_usn_rollback).

A partir de Windows Server 2012, los controladores de dominio virtual de AD DS hospedados en plataformas de hipervisor que expongan un identificador denominado identificador de generación de máquina virtual pueden detectar y emplear medidas de seguridad necesarias para proteger el entorno de AD DS si se revierte la máquina virtual en tiempo de la aplicación de una instantánea de máquina virtual. El diseño de VM-GenerationID utiliza un mecanismo independiente del proveedor del hipervisor para exponer este identificador en el espacio de dirección de la máquina virtual invitada, de modo que cualquier hipervisor que admita VM-GenerationID puede proporcionar una experiencia de virtualización segura. Los servicios y las aplicaciones que se ejecutan en la máquina virtual pueden muestrear este identificador para detectar si una máquina virtual se ha revertido en el tiempo.

### <a name="BKMK_HowSafeguardsWork"></a>¿Cómo funcionan estas medidas de seguridad de virtualización?
Durante la instalación del controlador de dominio, AD DS almacena inicialmente el identificador VM GenerationID como parte del atributo msDS-GenerationID en el objeto de equipo del controlador de dominio en su base de datos (a menudo denominado el árbol de información de directorios o DIT). El identificador VM GenerationID es objeto de un seguimiento independiente por parte de un controlador de Windows en la máquina virtual.

Cuando un administrador restaura la máquina virtual de una instantánea anterior, el valor actual de VM GenerationID del controlador de la máquina virtual se compara con un valor en el DIT.

Si los dos valores son distintos, se restablece el invocationID y se descarta el grupo RID para evitar la reutilización del USN. Si los valores coinciden, la transacción se confirma del modo habitual.

AD DS también compara el valor actual del VM GenerationID de la máquina virtual con el valor del DIT cada vez que se reinicia el controlador de dominio y, si es distinto, restablece el invocationID, descarta el grupo RID y actualiza el DIT con este nuevo valor. También sincroniza de forma no autoritativa la carpeta SYSVOL para completar la restauración segura. Esto permite ampliar las medidas de seguridad a la aplicación de instantáneas en las máquinas virtuales que se apagaron. Estas medidas de seguridad introducidos en Windows Server 2012 permiten a los administradores de AD DS para beneficiarse de las ventajas únicas derivadas de la implementación y administración de controladores de dominio en un entorno virtualizado.

La siguiente ilustración muestra cómo se aplican las medidas de seguridad de virtualización cuando se detecta la misma reversión de USN en un controlador de dominio virtualizado que ejecuta Windows Server 2012 en un hipervisor que admita VM-GenerationID.

![Introducción a AD DS](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/ADDS_VDC_Exampleofhowsafeguardswork.gif)

En este caso, cuando el hipervisor detecta un cambio en el valor de VM-GenerationID, se desencadenan las medidas de seguridad de virtualización, tales como el restablecimiento del InvocationID en el controlador de dominio virtualizado (de A a B en el ejemplo anterior) y la actualización del valor de VM-GenerationID guardado en la máquina virtual para que coincida con el nuevo valor (G2) almacenado por el hipervisor. Las medidas de seguridad garantizan la convergencia de la replicación en ambos controladores de dominio.

Con Windows Server 2012, AD DS emplea medidas de seguridad en controladores de dominio virtuales hospedados en hipervisores compatibles de VM-GenerationID y garantiza que la aplicación accidental de instantáneas o de otros compatibles con el hipervisor mecanismos que podrían revertir virtual estado de la máquina no deteriore el entorno de AD DS (mediante la prevención de problemas de replicación tales como una burbuja de USN u objetos persistentes). Sin embargo, restaurar un controlador de dominio mediante la aplicación de una instantánea de máquina virtual no es un mecanismo alternativo recomendable para realizar una copia de seguridad de un controlador de dominio. Se recomienda seguir usando Copias de seguridad de Windows Server u otras soluciones de copia de seguridad basadas en VSS Writer.

> [!CAUTION]
> Si un controlador de dominio en un entorno de producción se revierte accidentalmente a una instantánea, se recomienda consultar a los proveedores para las aplicaciones y servicios hospedados en esa máquina virtual, para obtener instrucciones sobre cómo comprobar el estado de estos programas después de restauración de instantáneas.

Para obtener más información, consulte [Virtualized domain controller safe restore architecture](../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_SafeRestoreArch).

## <a name="virtualized_dc_cloning"></a>Clonación del controlador de dominio virtualizados
A partir de Windows Server 2012, los administradores pueden sencilla y segura implementar controladores de dominio de réplica mediante la copia de un controlador de dominio virtual existente. En un entorno virtual, los administradores ya no tienen que implementar varias veces una imagen de servidor preparada mediante sysprep.exe, promover el servidor a un controlador de dominio y, a continuación, completar requisitos de configuración adicionales para implementar cada controlador de dominio de réplica.

> [!NOTE]
> Los administradores deben seguir los procesos existentes para implementar el primer controlador de dominio en un dominio; por ejemplo, utilizar sysprep.exe para preparar un disco duro virtual (VHD) de servidor, promover el servidor a un controlador de dominio y, a continuación, completar los requisitos de configuración adicionales. En un escenario de recuperación ante desastres, utilice la copia de seguridad de servidor más reciente para restaurar el primer controlador de dominio en un dominio.

### <a name="scenarios-that-benefit-from-virtual-domain-controller-cloning"></a>Escenarios que aprovechan la clonación de controladores de dominio virtuales

-   Implementación rápida de controladores de dominio adicionales en un nuevo dominio

-   Restauración rápida de la continuidad empresarial durante la recuperación ante desastres mediante la restauración de la capacidad de AD DS por medio de la implementación rápida de controladores de dominio a través de la clonación

-   Optimización de implementaciones de nubes privadas mediante el aprovechamiento del aprovisionamiento elástico de controladores de dominio para acomodar los requisitos de una escala mayor

-   Aprovisionamiento rápido de entornos de prueba que permitan implementar y probar las nuevas características y capacidades antes del lanzamiento de producción

-   Satisfacción rápida de las necesidades de una mayor capacidad en las sucursales mediante la clonación de controladores de dominio existentes en las sucursales

Cuando deba implementar rápidamente un gran número de controladores de dominio, siga los procedimientos existentes para validar el mantenimiento de cada controlador de dominio una vez finalizada la instalación. Implemente los controladores de dominio en lotes de un tamaño razonable para poder validar su mantenimiento después de haber completado cada uno de los lotes de instalaciones. El tamaño de lote recomendado es 10. Para obtener más información, consulte [Pasos para implementar un controlador de dominio virtualizado clonado](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#steps_deploy_vdc).

### <a name="clear-separation-of-responsibilities"></a>Separación clara de responsabilidades
La autorización para clonar controladores de dominio virtualizados depende del administrador de AD DS. Para que los administradores del hipervisor puedan implementar controladores de dominio adicionales mediante la copia de controladores de dominio virtuales, el administrador de AD DS debe seleccionar y autorizar un controlador de dominio y posteriormente debe ejecutar pasos previos para habilitarlo como un origen de clonación.

Como el aprovisionamiento de las máquinas virtuales suele ser responsabilidad del administrador del hipervisor, este puede aprovisionar máquinas virtuales de controlador de dominio de réplica mediante la copia de controladores de dominio virtualizados que han sido autorizados y preparados para la clonación por el administrador de AD DS.

> [!WARNING]
> Cualquiera que tenga autorización para administrar el hipervisor que hospeda un controlador de dominio virtual debe haberse auditado y considerarse de plena confianza en el entorno.

### <a name="how-does-virtual-domain-controller-cloning-work"></a>¿Cómo funciona la clonación de controladores de dominio virtuales?
El proceso de clonación implica realizar una copia del VHD de un controlador de dominio virtual existente (o, para configuraciones más complejas, la máquina virtual del controlador de dominio), autorizar su clonación en AD DS y la creación de un archivo de configuración de clonación. Esto reduce el número de pasos y el tiempo empleados en la implementación de un controlador de dominio virtual de réplica, ya que se eliminan las tareas de implementación que serían repetitivas.

El controlador de dominio clonado utiliza los siguientes criterios para detectar que es una copia de otro controlador de dominio:

1.  El valor del identificador de generación de VM suministrado por la máquina virtual no coincide con el valor del identificador de generación de VM almacenado en el DIT.

    > [!NOTE]
    > La plataforma del hipervisor debe admitir identificadores de generación de VM (Windows Server 2012 Hyper-V es compatible con Id. de generación de VM).

2.  La presencia de un archivo denominado DCCloneConfig.xml en una de las siguientes ubicaciones:

    -   El directorio donde reside el DIT

    -   %windir%\NTDS

    -   La raíz de una unidad de medios extraíbles

Cuando se cumplen los criterios, se lleva a cabo el proceso de clonación para aprovisionarse como un controlador de dominio de réplica.

El controlador de dominio clonado utiliza el contexto de seguridad del controlador de dominio de origen (el controlador de dominio cuya copia representa) para ponerse en contacto con el contenedor de rol de maestro de operaciones del emulador de controlador de dominio principal (PDC) de Windows Server 2012 (también conocida como operaciones de maestro únicas flexible o FSMO). El emulador de PDC debe ejecutar Windows Server 2012, pero no tiene que se esté ejecutando en un hipervisor.

> [!NOTE]
> Si tiene una extensión de esquema con atributos que hacen referencia al controlador de dominio de origen y el atributo se encuentra en uno de los objetos copiados (objeto de equipo, objeto de configuración NTDS) para crear el clon, ese atributo no se copiará ni actualizará para hacer referencia al controlador de dominio clonado.

Después de comprobar que el controlador de dominio solicitante está autorizado para la clonación, el emulador de PDC creará una nueva identidad de máquina con una nueva cuenta, SID, nombre y contraseña que identifique esta máquina como un controlador de dominio de réplica y enviará esta información de vuelta al clon. A continuación, el controlador de dominio clonado preparará los archivos de base de datos de AD DS para que funcionen como una réplica y también limpiará el estado de la máquina.

Para obtener más información, consulte el tema sobre la [arquitectura de clonación de controladores de dominio virtualizados](../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_CloneArch).

### <a name="cloning-components"></a>Componentes de clonación
Los componentes de clonación incluyen nuevos cmdlets del módulo de Active Directory para Windows PowerShell y archivos XML asociados:

-   **New-ADDCCloneConfigFile** "este cmdlet crea y coloca el archivo DCCloneConfig.xml en la ubicación correcta para asegurarse de está disponible para desencadenar la clonación. También realiza comprobaciones de requisitos previos para garantizar una clonación correcta. Se incluye en el módulo de Active Directory para Windows PowerShell. Puede ejecutarlo localmente en un controlador de dominio virtualizado que se esté preparando para la clonación o puede ejecutarlo de manera remota mediante la opción -offline. Puede especificar la configuración del controlador de dominio, como su nombre, sitio y dirección IP.

    Se realizan las siguientes comprobaciones de requisitos previos:

    > [!NOTE]
    > Los requisitos previos comprueba que no son realizadas cuando el "se usa la opción offline. Para obtener más información, consulte [Ejecución de New-ADDCCloneConfigFile en el modo sin conexión](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#BKMK_OfflineMode).

    -   El controlador de dominio que se está preparando se autoriza para la clonación (es un miembro del grupo **Controladores de dominio clonables**).

    -   El emulador de PDC ejecuta Windows Server 2012.

    -   Los programas o servicios excluidos de la ejecución **Get-ADDCCloningExcludedApplicationList** se incluyen en CustomDCCloneAllowList.xml (se explica con más detalle al final de esta lista de componentes de clonación).

-   **DCCloneConfig.xml** "para clonar correctamente un controlador de dominio virtualizado, este archivo debe estar presente en el directorio donde reside el DIT, *%windir%\NTDS*, o la raíz de una unidad de medios extraíbles. Además de usarse como uno de los desencadenadores para detectar e iniciar la clonación, también proporciona un medio para especificar la configuración del controlador de dominio clonado.

    El esquema y un archivo de ejemplo para el archivo DCCloneConfig.xml se almacenan en todos los equipos de Windows Server 2012 en:

    -   %windir%\system32\DCCloneConfigSchema.xsd

    -   %windir%\system32\SampleDCCloneConfig.xml

    Se recomienda usar el cmdlet New-ADDCCloneConfigFile para crear el archivo DCCloneConfig.xml. Aunque también podría usar el archivo de esquema con un editor compatible con XML para crear este archivo, la edición manual de este archivo aumenta la probabilidad de errores. Si edita el archivo, debe hacerlo con editores compatibles con XML, como Visual Studio, [XML Notepad](https://www.microsoft.com/download/details.aspx?displaylang=en&id=7973)o aplicaciones de terceros (no utilice el Bloc de notas).

-   **Get-ADDCCloningExcludedApplicationList** "este cmdlet se ejecuta en el controlador de dominio de origen antes de comenzar el proceso de clonación para determinar qué servicios o programas instalados no están en la lista admitida predeterminada DefaultDCCloneAllowList.xml o una inclusión definida por el usuario archivo CustomDCCloneAllowList.xml con nombre de la lista y, por tanto, no se han evaluado para la clonación de impacto.

    Este cmdlet busca en el controlador de dominio de origen los servicios del administrador de control de servicios y los programas instalados que aparecen en **HKLM\Software\Microsoft\Windows\CurrentVersion\Uninstall** y que no están especificados en la lista predeterminada (DefaultDCCloneAllowList.xml) o en la lista de inclusión definida por el usuario (CustomDCCloneAllowList.xml file), si se ha proporcionado una. La lista de aplicaciones y servicios que se devuelve al ejecutar el cmdlet representa la diferencia entre lo que ya se ha proporcionado en el archivo DefaultDCCloneAllowList.xml o en el archivo CustomDCCloneAllowList.xml y la lista que se crea en tiempo de ejecución, en función de lo que hay instalado en el controlador de dominio de origen. Los servicios y programas obtenidos de Get-ADDCCloningExcludedApplicationList pueden agregarse al archivo CustomDCCloneAllowList.xml si se determina que los servicios y programas pueden clonarse de forma segura. Para determinar si un servicio o programa instalado puede clonarse de forma segura, evalúe las siguientes condiciones:

    -   ¿Se ve afectado el servicio o programa instalado por la identidad de la máquina, como el nombre, el SID, la contraseña, etc.?

    -   ¿Almacena localmente el servicio o programa instalado algún estado en el equipo que podría afectar a su funcionalidad en el clon?

    Debe trabajar con el proveedor del software de la aplicación para determinar si el servicio o programa puede clonarse de forma segura.

    > [!NOTE]
    > Antes de aprovisionar servicios o programas adicionales en el archivo CustomDCCloneAllowList.xml, compruebe si dispone de la licencia necesaria para copiar el software contenido en esa máquina virtual.

    Si las aplicaciones no pueden clonarse, quítelas del controlador de dominio de origen antes de crear los medios clonados. Si una aplicación aparece en la salida del cmdlet pero no está incluida en el archivo CustomDCCloneAllowList.xml, la clonación no se ejecutará correctamente. Para que la clonación sea correcta, la salida del cmdlet no debería incluir ningún servicio ni programa. En otras palabras, una aplicación debería estar incluida en el archivo CustomDCCloneAllowList.xml o bien quitarse del controlador de dominio de origen.

    En la siguiente tabla se explican las opciones de ejecución de Get-ADDCCloningExcludedApplicationList.

    |||
    |-|-|
    |Argumento|Explicación|
    |*<no argument specified>*|Muestra una lista de los servicios o programas en la consola que no se han tenido en cuenta para la clonación. Si ya hay un archivo CustomDCCloneAllowList.XML en cualquiera de las ubicaciones permitidas, utiliza ese archivo para mostrar los servicios y programas restantes (que podría no ser ninguno si las listas coinciden).|
    |-GenerateXml|Crea el archivo CustomDCCloneAllowList.XML con los servicios y programas que aparecen en la consola.|
    |-Force|Sobrescribe un archivo CustomDCCloneAllowList.XML existente.|
    |-Path|Ruta de acceso a la carpeta para crear el archivo CustomDCCloneAllowList.XML.|

-   **DefaultDCCloneAllowList.xml** "este archivo está presente de forma predeterminada en cada Windows Server 2012 domain controller in las *%windir%\system32*. Incluye los servicios y programas instalados que pueden clonarse de forma segura de manera predeterminada. No debe cambiar la ubicación ni el contenido de este archivo o, de lo contrario, la clonación no se ejecutará correctamente.

-   **CustomDCCloneAllowList.xml** "Si tiene servicios o programas instalados que residen en el controlador de dominio de origen que se encuentran fuera de los que aparecen en el archivo DefaultDCCloneAllowList.xml, dichos servicios y programas deben incluirse en este archivo. Para buscar los servicios o programas instalados que no aparecen en el archivo DefaultDCCloneAllowList.xml, ejecute el cmdlet **Get-ADDCCloningExcludedApplicationList** . Debe usar el **"GenerateXml** argumento para generar el archivo XML.

    El proceso de clonación comprueba las siguientes ubicaciones en orden, en busca de este archivo, y utiliza el primer archivo XML que encuentra, independientemente del contenido de las demás carpetas:

    1.  La siguiente clave del Registro:

        ```
        HKey_Local_Machine\System\CurrentControlSet\Services\NTDS\Parameters
        AllowListFolder (REG_SZ)
        ```

    2.  Directorio de trabajo de DSA

    3.  %systemroot%\NTDS

    4.  Medios extraíbles de lectura/escritura según el orden de la letra de unidad, en la raíz de la unidad

### <a name="deployment-scenarios"></a>Escenarios de implementación
La clonación de controladores de dominio virtuales admite los siguientes escenarios de implementación:

-   Implementar un controlador de dominio clonado mediante la realización de una copia del archivo de disco duro virtual (vhd) del controlador de dominio de origen.

-   Implementar un controlador de dominio clonado mediante la copia de la máquina virtual de un controlador de dominio de origen con la semántica de exportación/importación expuesta por el hipervisor.

> [!NOTE]
> Los pasos descritos en la sección [pasos para implementar un controlador de dominio virtualizado clonado](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#steps_deploy_vdc) demostrar copiar una máquina virtual mediante la característica de exportación e importación de Windows Server 2012 Hyper-V.

## <a name="steps_deploy_vdc"></a>Pasos para implementar un controlador de dominio virtualizado clonado

-   [Requisitos previos](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#prerequisites)

-   [Paso 1: Conceder el permiso para clonarse de controlador de dominio virtualizado de origen](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#bkmk4_grant_source)

-   [Paso 2: Ejecute el cmdlet Get-ADDCCloningExcludedApplicationList](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#bkmk6_run_get-addccloningexcludedapplicationlist_cmdlet)

-   [Paso 3: Run New-ADDCCloneConfigFile](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#bkmk5_create_insert_dccloneconfig)

-   [Paso 4: Exportar e importar, a continuación, la máquina virtual del controlador de dominio de origen](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#bkmk7_export_import_vm_sourcedc)

### <a name="prerequisites"></a>Requisitos previos

-   Para completar los pasos de los procedimientos siguientes debe ser miembro del grupo Admins. del dominio o tener asignados permisos equivalentes.

-   Los comandos de Windows PowerShell usados en esta guía se deben ejecutar desde un símbolo del sistema con privilegios elevados. Para ello, haga clic en el **Windows PowerShell** icono y, a continuación, haga clic en **ejecutar como administrador**.

-   Un servidor de Windows Server 2012 con el rol de servidor de Hyper-V instalado (**HyperV1**).

-   Un segundo servidor de Windows Server 2012 con el rol de servidor de Hyper-V instalado (**HyperV2**).

    > [!NOTE]
    > -   Si está usando otro hipervisor, debería ponerse en contacto con el proveedor de ese hipervisor para comprobar si el hipervisor admite identificadores de generación de VM. Si el hipervisor no admite identificadores de generación de VM y ha proporcionado un archivo DCCloneConfig.xml, la nueva máquina virtual arrancará en el Modo de restauración de servicios de directorio (DSRM).
    > -   Para aumentar la disponibilidad del servicio AD DS, esta guía recomienda y explica el uso de dos hosts de Hyper-V distintos, lo que ayuda a evitar un posible único punto de error. Sin embargo, no se necesitan dos hosts de Hyper-V para realizar la clonación de controladores de dominio virtuales.
    > -   Necesita ser miembro del grupo Administradores local en cada servidor de Hyper-V (**HyperV1** y **HyperV2**).
    > -   Para importar y exportar correctamente un archivo VHD mediante Hyper-V, el conmutador de red virtual en ambos hosts de Hyper-V debería tener el mismo nombre. Por ejemplo, si tiene un conmutador de red virtual en **HyperV1** denominado VNet, es necesario que haya un conmutador de red virtual en **HyperV2** denominado VNet.
    > -   Si los dos hosts de Hyper-V (**HyperV1** y **HyperV2**) tienen distintos procesadores, apague la máquina virtual (**VirtualDC1**) que tiene previsto exportar, haga clic con el botón secundario en la máquina virtual, haga clic en **Configuración**, después en **Procesador** y, en **Compatibilidad de procesador**, seleccione **Migrar a un equipo físico con una versión de procesador distinta** y haga clic en **Aceptar**.

-   Implementado Windows Server 2012 controlador de dominio (virtualizado o físico) que hospeda el rol de emulador PDC (**DC1**). Para comprobar si el rol de emulador PDC está hospedado en un controlador de dominio de Windows Server 2012, ejecute el siguiente comando de Windows PowerShell:

    ```
    Get-ADComputer (Get-ADDomainController "Discover "Service "PrimaryDC").name "Property operatingsystemversion | fl
    ```

    El valor de OperatingSystemVersion devuelto debería ser la versión 6.2. Si es necesario, puede transferir el rol de emulador PDC a un controlador de dominio que ejecuta Windows Server 2012. Para obtener más información, consulte [Utilizar Ntdsutil.exe para asumir o transferir las funciones de FSMO a un controlador de dominio](https://support.microsoft.com/kb/255504).

-   Un controlador de dominio virtualizado de invitado de Windows Server 2012 implementado (**VirtualDC1**) que se encuentra en el mismo dominio que el controlador de dominio de Windows Server 2012 hospeda el rol de emulador PDC (**DC1**). Será el controlador de dominio de origen usado en la clonación. El controlador de dominio virtual invitado se hospedará en un servidor de Windows Server 2012 Hyper-V (**HyperV1**).

    > [!NOTE]
    > -   Para que la clonación sea correcta, el controlador de dominio de origen que se usa para crear el clon no puede corresponder a un controlador de dominio que haya sido disminuido de nivel desde que se crearon los medios del VHD de origen.
    > -   Apague el controlador de dominio de origen antes de copiar la máquina virtual o su VHD.
    > -   No debería clonar un VHD ni restaurar una instantánea que sea anterior al valor de vigencia del marcador de exclusión (o al valor de vigencia del objeto eliminado si se ha habilitado la papelera de reciclaje de Active Directory). Si está copiando un VHD de un controlador de dominio existente, asegúrese de que el archivo VHD no sea anterior al valor de vigencia del marcador de exclusión (de forma predeterminada, 60 días). No debería copiar un VHD de un controlador de dominio en ejecución para crear medios clonados.

    Expulse cualquier unidad de disquete virtual (VFD) que pueda tener el controlador de dominio de origen. Esto puede provocar un problema de uso compartido al intentar importar la nueva máquina virtual.

    Solo Windows Server 2012 controladores de dominio hospedados en un hipervisor en el VM-GenerationID pueden usarse como origen de clonación. Utilizado para clonar el controlador de dominio de origen Windows Server 2012 debe estar en un estado correcto. Para determinar el estado del controlador de dominio de origen, ejecute [dcdiag](https://technet.microsoft.com/library/cc731968(WS.10).aspx). Para obtener una mejor comprensión de la salida devuelta por dcdiag, consulte [lo que hace realmente DCDIAG... ¿hacer? ](http://blogs.technet.com/b/askds/archive/2011/03/22/what-does-dcdiag-actually-do.aspx).

    Si el controlador de dominio de origen es un servidor DNS, el controlador de dominio clonado también será un servidor DNS. Debería elegir un servidor DNS que hospede solamente zonas integradas en Active Directory.

    La configuración del cliente DNS no se clona sino que se especifica en el archivo DCCloneConfig.xml. Si no se especifica, el controlador de dominio clonado se señalará a sí mismo como el servidor DNS preferido de forma predeterminada. El controlador de dominio clonado no tendrá una delegación DNS. El administrador de la zona DNS principal debería actualizar la delegación DNS para el controlador de dominio clonado según sea necesario.

    > [!WARNING]
    > Las medidas de seguridad de virtualización no se extienden a Active Directory Lightweight Directory Services (AD LDS). Por lo tanto, no debería intentar clonar un controlador de dominio de AD DS que hospede una instancia de AD LDS agregando esta instancia de AD LDS a CustomDCCloneAllowList.xml. Dado que AD LDS no es compatible con identificadores de generación de VM, la clonación de un controlador de dominio con AD LDS puede provocar una divergencia inducida por la reversión de USN en ese conjunto de configuración de AD LDS.

    Los siguientes roles de servidor no se admiten en la clonación:

    -   Protocolo de configuración dinámica de host (DHCP)

    -   Active Directory Certificate Services (AD CS)

    -   Active Directory Lightweight Directory Services (AD LDS)

### <a name="bkmk4_grant_source"></a>Paso 1: Conceder al controlador de dominio virtualizado de origen el permiso para clonarse
En este procedimiento, se concede al controlador de dominio de origen el permiso para clonarse mediante el uso del **Centro de administración de Active Directory** para agregar el controlador de dominio de origen al grupo **Controladores de dominio clonables**.

##### <a name="to-grant-the-source-virtualized-domain-controller-the-permission-to-be-cloned"></a>Para conceder al controlador de dominio virtualizado de origen el permiso para clonarse

1.  En cualquier controlado de dominio que se encuentre en el mismo dominio que el controlador de dominio que se está preparando para clonarse (**VirtualDC1**), abra el **Centro de administración de Active Directory** (ADAC), ubique el objeto de controlador de dominio virtualizado (los controladores de dominio suelen ubicarse en el contenedor **Controladores de dominio** en ADAC), haga clic con el botón secundario en él, elija **Agregar a grupo** y, en **Escriba el nombre de objeto para seleccionar** , escriba **Cloneable Domain Controllers** y haga clic en **Aceptar**.

    La actualización de pertenencia a grupos llevada a cabo en este paso debe replicarse en el emulador de PDC para poder realizar la clonación. Si el **controladores de dominio clonables** no se encuentra el grupo, puede que el rol de emulador PDC no esté hospedado en un controlador de dominio que ejecuta Windows Server 2012.

    > [!NOTE]
    > Para abrir ADAC en un controlador de dominio de Windows Server 2012, abra Windows PowerShell y escriba **dsac.exe**.

![Introducción a AD DS](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos ***

El siguiente cmdlet de Windows PowerShell realiza la misma función que el procedimiento anterior:


    Add-ADGroupMember "Identity "CN=Cloneable Domain Controllers,CN=Users, DC=Fabrikam,DC=Com" "Member "CN=VirtualDC1,OU=Domain Controllers,DC=Fabrikam,DC=com"


### <a name="bkmk6_run_get-addccloningexcludedapplicationlist_cmdlet"></a>Paso 2: Ejecutar el cmdlet Get-ADDCCloningExcludedApplicationList
En este procedimiento, ejecute el cmdlet `Get-ADDCCloningExcludedApplicationList` en el controlador de dominio virtualizado de origen para identificar los programas o servicios que no se han evaluado para la clonación. Es necesario ejecutar el cmdlet Get-ADDCCloningExcludedApplicationList antes del cmdlet New-ADDCCloneConfigFile porque si el cmdlet New-ADDCCloneConfigFile detecta una aplicación excluida, no creará un archivo DCCloneConfig.xml.

##### <a name="to-identify-applications-or-services-that-run-on-a-source-domain-controller-which-have-not-been-evaluated-for-cloning"></a>Para identificar las aplicaciones o servicios que se ejecutan en un controlador de dominio de origen y que no se han evaluado para la clonación

1.  En el controlador de dominio de origen (**VirtualDC1**), haga clic en **Administrador del servidor**, haga clic en **Herramientas**, después en **Módulo de Active Directory para Windows PowerShell** y, a continuación, escriba el siguiente comando:


    Get-ADDCCloningExcludedApplicationList


2.  Examine junto con el proveedor del software la lista de servicios y programas instalados devueltos para determinar si pueden clonarse de forma segura. Si las aplicaciones o los servicios de la lista no pueden clonarse de forma segura, debe quitarlos del controlador de dominio de origen o, de lo contrario, la clonación no se ejecutará correctamente.

3.  Para el conjunto de servicios y programas instalados que se considera que pueden clonarse de forma segura, ejecute el comando nuevo con el **"GenerateXML** conmutador para aprovisionar estos servicios y programas en el **CustomDCCloneAllowList.xml**  archivo.


    Get-ADDCCloningExcludedApplicationList -GenerateXml


### <a name="bkmk5_create_insert_dccloneconfig"></a>Paso 3: Ejecutar New-ADDCCloneConfigFile
Ejecute New-ADDCCloneConfigFile en el controlador de dominio de origen y, opcionalmente, especifique la configuración del controlador de dominio clonado, como el nombre, la dirección IP y la resolución DNS.

Por ejemplo, para crear un controlador de dominio clonado denominado VirtualDC2 con una dirección IPv4 estática, escriba:


    New-ADDCCloneConfigFile "Static -IPv4Address "10.0.0.2" -IPv4DNSResolver "10.0.0.1" -IPv4SubnetMask "255.255.255.0" -CloneComputerName "VirtualDC2" -IPv4DefaultGateway "10.0.0.3" -SiteName "REDMOND"

> [!NOTE]
> El controlador de dominio clonado se ubicará en el mismo sitio que el controlador de dominio de origen, a menos que se especifique un sitio distinto en el archivo DCCloneConfig.xml. Se recomienda especificar un sitio adecuado en el archivo DCCloneConfig.xml para el controlador de dominio clonado en función de su dirección IP.

El nombre de equipo es opcional. Si no especifica ninguno, se generará un nombre único basado en el siguiente algoritmo:

-   El prefijo se forma con los 8 primeros caracteres del nombre de equipo del controlador de dominio de origen. Por ejemplo, si el nombre de equipo de origen es SourceComputer, se trunca formando la cadena de prefijo SourceCo.

-   Un sufijo nombre único del formato de "" CL*nnnn*"se anexa a la cadena de prefijo donde *nnnn* es el siguiente valor disponible entre 0001 y 9999 que el PDC determina que no está actualmente en uso. Por ejemplo, si 0047 es el siguiente número disponible en el intervalo permitido y se usa el ejemplo anterior de prefijo de nombre de equipo SourceCo, el nombre derivado que se usará en el equipo clonado será SourceCo-CL0047.

> [!NOTE]
> Se requiere un servidor de catálogo global (GC) para que el cmdlet New-ADDCCloneConfigFile funcione correctamente. Pertenencia del controlador de dominio de origen en el **controladores de dominio clonables** grupo debe reflejarse en el catálogo global. No es necesario que el GC sea el mismo controlador de dominio que el emulador de PDC, pero es preferible que se encuentre en el mismo sitio. Si un GC no está disponible, el comando genera el error "el servidor no es funcional". Para obtener más información, consulte [Virtualized Domain Controller Troubleshooting](../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md).

Para crear un controlador de dominio clonado denominado Clone1 con una configuración de IPv4 estática y especificar el servidor WINS preferido y alternativo, escriba:


    New-ADDCCloneConfigFile "CloneComputerName "Clone1" "Static -IPv4Address "10.0.0.5" "IPv4DNSResolver "10.0.0.1" "IPv4SubnetMask "255.255.0.0" "PreferredWinsServer "10.0.0.1" "AlternateWinsServer "10.0.0.2"


> [!NOTE]
> Si especifica servidores WINS, debe especificar ambos **"PreferredWINSServer** y **" AlternateWINSServer**. Si solo especifica uno de esos argumentos, la clonación no se ejecuta correctamente y aparece el código de error 0x80041005 en dcpromo.log.

Para crear un controlador de dominio clonado denominado Clone2 con una configuración de IPv4 dinámica, escriba:


    New-ADDCCloneConfigFile -CloneComputerName "Clone2" -IPv4DNSResolver "10.0.0.1" 


> [!NOTE]
> En este caso, debería haber un servidor DHCP en el entorno que estuviese al alcance del clon, para que este pudiese obtener la dirección IP y otra configuración de red relevante.

Para crear un controlador de dominio clonado denominado Clone2 con una configuración de IPv4 dinámica y especificar el servidor WINS preferido y alternativo, escriba:


    New-ADDCCloneConfigFile -CloneComputerName "Clone2" -IPv4DNSResolver "10.0.0.1" -SiteName "REDMOND" "PreferredWinsServer "10.0.0.1" "AlternateWinsServer "10.0.0.2"


Para crear un controlador de dominio clonado con una configuración de IPv6 dinámica, escriba:


    New-ADDCCloneConfigFile -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc"


Para crear un controlador de dominio clonado con una configuración de IPv6 estática, escriba:


    New-ADDCCloneConfigFile "Static -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc"


> [!NOTE]
> Al especificar la configuración de IPv6, la única diferencia entre la configuración estática y dinámica es la inclusión de **-estático** cambie. La inclusión de la **-estático** conmutador hace que sea obligatorio especificar al menos una **IPv6DNSResolver**. Se espera la dirección IPv6 estática se configure a través de la configuración automática de direcciones sin estado (SLAAC) con prefijos asignado de enrutador. IPv6 dinámica, las resoluciones DNS son opcionales, pero se espera que el clon pueda alcanzar un servidor DHCP habilitado para IPv6 en la subred para obtener información de configuración de DNS y dirección IPv6.

#### <a name="BKMK_OfflineMode"></a>Ejecute New-ADDCCloneConfigFile en modo sin conexión
Si tiene varias copias de medios de controladores de dominio de origen que hayan sido preparados para la clonación (lo que significa que el controlador de dominio de origen está autorizado para clonarse, se ha ejecutado el cmdlet Get-ADDCCloningExcludedApplicationList, etc.) y desea especificar una configuración distinta en cada copia de los medios, puede ejecutar New-ADDCCloneConfigFile en el modo sin conexión. Esto puede ser más eficaz que preparar cada máquina virtual de manera individual, por ejemplo, importando cada copia.

En este caso, los administradores de dominio pueden montar el disco sin conexión y use herramientas de administración de servidor remoto (RSAT) para ejecutar el cmdlet New-ADDCCloneConfigFile con el argumento - offline para agregar los archivos XML, que permite la automatización similar a factory con nuevas Opciones de Windows PowerShell incluidas en Windows Server 2012. Para obtener más información acerca de cómo montar el disco sin conexión para ejecutar el cmdlet New-ADDCCloneConfigFile en el modo sin conexión, consulte el tema sobre cómo [agregar XML al disco del sistema sin conexión](../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Deployment-and-Configuration.md#BKMK_Offline).

Primero debería ejecutar el cmdlet localmente en los medios de origen para asegurarse de que las comprobaciones de requisitos previos se ejecutan sin errores. Las comprobaciones de requisitos previos no se realizan en el modo sin conexión porque el cmdlet podría ejecutarse desde una máquina que no perteneciese al mismo dominio o desde un equipo unido al dominio. Después de ejecutar el cmdlet localmente, creará un archivo DCCloneConfig.xml. Puede eliminar el DCCloneConfig.xml que se crea localmente si tiene previsto usar el modo sin conexión posteriormente.

Para crear un controlador de dominio clonado denominado CloneDC1 en el modo sin conexión en un sitio de REDMOND"con la dirección IPv4 estática, escriba:


    New-ADDCCloneConfigFile -Offline -CloneComputerName CloneDC1 -SiteName REDMOND -IPv4Address "10.0.0.2" -IPv4DNSResolver "10.0.0.1" -IPv4SubnetMask "255.255.0.0" -IPv4DefaultGateway "10.0.0.1" -Static -Path F:\Windows\NTDS


Para crear un controlador de dominio clonado denominado Clone2 en el modo sin conexión con una configuración de IPv4 estática e IPv6 estática, escriba:


    New-ADDCCloneConfigFile -Offline -IPv4Address "10.0.0.2" -IPv4DNSResolver "10.0.0.1" -IPv4SubnetMask "255.255.0.0" -Static -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -CloneComputerName "Clone2" -PreferredWINSServer "10.0.0.1" -AlternateWINSServer "10.0.0.3" -Path F:\Windows\NTDS


Para crear un controlador de dominio clonado en el modo sin conexión con una configuración de IPv4 estática e IPv6 dinámica, y especificar varios servidores DNS en la configuración de resolución DNS, escriba:


    New-ADDCCloneConfigFile -Offline -IPv4Address "10.0.0.10" -IPv4SubnetMask "255.255.0.0" -IPv4DefaultGateway "10.0.0.1" -IPv4DNSResolver @( "10.0.0.1","10.0.0.2" ) -Static -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -Path F:\Windows\NTDS 


Para crear un controlador de dominio clonado denominado Clone1 en el modo sin conexión con una configuración de IPv4 dinámica e IPv6 estática, escriba:


    New-ADDCCloneConfigFile -Offline -Static -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -CloneComputerName "Clone1" -PreferredWINSServer "10.0.0.1" -AlternateWINSServer "10.0.0.3" -SiteName "REDMOND" -Path F:\Windows\NTDS


Para crear un controlador de dominio clonado en el modo sin conexión con una configuración de IPv4 dinámica e IPv6 dinámica, escriba:


    New-ADDCCloneConfigFile -Offline -IPv4DNSResolver "10.0.0.1" -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -Path F:\Windows\NTDS


### <a name="bkmk7_export_import_vm_sourcedc"></a>Paso 4: Exportar y, a continuación, importar la máquina virtual del controlador de dominio de origen
En este procedimiento, exporte la máquina virtual del controlador de dominio virtualizado de origen y, a continuación, importe la máquina virtual. Esta acción crea un controlador de dominio virtualizado clonado en el dominio.

Debe ser miembro del grupo Administradores local en cada host de Hyper-V. Si usa credenciales distintas en cada servidor, ejecute los cmdlets de Windows PowerShell para exportar e importar la máquina virtual en distintas sesiones de Windows PowerShell.

Si hay instantáneas en el controlador de dominio de origen, deberían eliminarse antes de exportar el controlador de dominio de origen, ya que la máquina virtual no se importará si una instantánea tiene una configuración de procesador que no es compatible con el host de Hyper-V de destino. Si la configuración de procesador de los hosts de Hyper-V de origen y destino es compatible, puede exportar y copiar el origen sin necesidad de eliminar instantáneas de antemano. Sin embargo, después de la importación, las instantáneas deben eliminarse de la máquina virtual clonada antes de que se inicie.

##### <a name="to-copy-a-virtual-domain-controller-by-exporting-and-then-importing-the-virtualized-source-domain-controller"></a>Para copiar un controlador de dominio virtual mediante la exportación e importación del controlador de dominio de origen virtualizado

1.  En **HyperV1**, apague el controlador de dominio de origen (**VirtualDC1**).

    ![Introducción a AD DS](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos ***

    Stop-VM-nombre VirtualDC1 - ComputerName HyperV1


2.  En **HyperV1**, elimine instantáneas y, a continuación, exporte el controlador de dominio de origen (VirtualDC1) al directorio c:\CloneDCs.

> [!NOTE]
> Debería eliminar todas las instantáneas asociadas porque cada vez que se toma una instantánea se crea un nuevo archivo AVHD que actúa como un disco de diferenciación. Esto crea un efecto en cadena. Si ha tomado instantáneas e inserta el archivo DCCLoneConfig.xml en el VHD, puede ser que cree el clon a partir de una versión anterior del DIT o que inserte el archivo de configuración en el archivo VHD incorrecto. La eliminación de la instantánea combina todos estos AVHD en un VHD de base.

![Introducción a AD DS](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos ***


    Get-VMSnapshot VirtualDC1 | Remove-VMSnapshot -IncludeAllChildSnapshots
    Export-VM -Name VirtualDC1 -ComputerName HyperV1 -Path c:\CloneDCs\VirtualDC1


3.  Copie la carpeta **virtualdc1** al directorio c:\Import de **HyperV2**.

4.  En **HyperV2**, utilice el **Administrador de Hyper-V**para importar la máquina virtual (mediante el asistente **Importar máquina virtual** del **Administrador de Hyper-V**) desde la carpeta **c:\Import\virtualdc1** y elimine todas las **Instantáneas**asociadas.

Use la opción **Copiar la máquina virtual (crear un identificador exclusivo nuevo)** al importar la máquina virtual.

![Introducción a AD DS](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos ***

    $path = Get-ChildItem "C:\CloneDCs\VirtualDC1\VirtualDC1\Virtual Machines"
    $vm = Import-VM -Path $path.fullname -Copy -GenerateNewId
    Rename-VM $vm VirtualDC2


Para crear varios controladores de dominio clonados a partir de un mismo controlador de dominio de origen:

  -   UI: en el asistente **Importar máquina virtual** , especifique nuevas ubicaciones para **Carpeta de configuración de la máquina virtual**, **Almacén de instantáneas**, **Carpeta de paginación inteligente**y otra **Ubicación** para los discos duros virtuales de la máquina virtual.

  -   Windows PowerShell: especifique nuevas ubicaciones para la máquina virtual mediante el uso de los siguientes parámetros para el `Import-VM` cmdlet:

        $path = get-ChildItem "C:\CloneDCs\VirtualDC1\VirtualDC1\Virtual máquinas" Import-VM-ruta de acceso $path.fullname - copia - GenerateNewId - VhdDestinationPath - ComputerName HyperV2 "path" - SnapshotFilePath "path" - SmartPagingFilePath "path": VirtualMachinePath "path"


> [!NOTE]
> El tamaño de lote recomendado para crear varios controladores de dominio clonados simultáneamente es 10. El número máximo queda restringido por el número máximo de conexiones de replicación de salida, cuyo valor predeterminado es 16 en la replicación del sistema de archivos distribuido (DFSR) y 10 en el servicio de replicación de archivos (FRS). No debería implementar simultáneamente más del número recomendado de controladores de dominio clonados, a menos que haya probado exhaustivamente ese número en su entorno.

5.  En **HyperV1**, reinicie el controlador de dominio de origen (**(VirtualDC1**) para volver a ponerlo en línea.

![Introducción a AD DS](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos ***

    Start-VM -Name VirtualDC1 -ComputerName HyperV1


6.  En **HyperV2**, inicie la máquina virtual (**VirtualDC2**) para ponerla en línea como un controlador de dominio clonado en el dominio.

![Introducción a AD DS](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos ***


    Start-VM -Name VirtualDC2 -ComputerName HyperV2

> [!NOTE]
> El emulador de PDC debe estar en ejecución para que la clonación se realice correctamente. Si se apagó, asegúrese de que se ha iniciado y ha realizado la sincronización inicial de modo que se reconoce como rol de emulador de PDC. Para obtener más información, consulte el [artículo 305476 de Microsoft Knowledge Base](https://support.microsoft.com/kb/305476).

Una vez finalizada la clonación, compruebe el nombre del equipo clonado para asegurarse de que la clonación se ha ejecutado correctamente. Compruebe que la máquina virtual no se inició en el Modo de restauración de servicios de directorio (DSRM). Si intenta iniciar sesión y recibe un error que indica que no hay servidores de inicio de sesión disponibles, intente iniciar sesión en el modo DSRM. Si el controlador de dominio no se clonó correctamente y arranca en el modo DSRM, compruebe los registros del Visor de eventos y los registros de dcpromo en la carpeta %systemroot%/debug.

El controlador de dominio clonado será un miembro del grupo **Controladores de dominio clonables** porque copia la pertenencia del controlador de dominio de origen. El procedimiento recomendado es dejar el grupo **Controladores de dominio clonables** vacío hasta estar preparado para realizar las operaciones de clonación y quitar los miembros después de que hayan finalizado las operaciones de clonación.

Si el controlador de dominio de origen almacena un medio de copia de seguridad, el controlador de dominio clonado también almacenará el medio de copia de seguridad. Puede ejecutar `wbadmin get versions` para mostrar el medio de copia de seguridad en el controlador de dominio clonado. Un miembro del grupo Admins. del dominio debería eliminar el medio de copia de seguridad en el controlador de dominio clonado para evitar que se restaure de forma accidental. Para obtener más información acerca de cómo eliminar una copia de seguridad de estado del sistema con wbadmin.exe, consulte [Wbadmin delete systemstatebackup](https://technet.microsoft.com/library/cc742081(v=WS.10).aspx).

## <a name="troubleshooting"></a>Solución de problemas
Si el controlador de dominio clonado (**VirtualDC2**) se inicia en el Modo de restauración de servicios de directorio (DSRM), no vuelve al modo normal por sí mismo en el siguiente reinicio. Para iniciar sesión en un controlador de dominio iniciado en el modo DSRM, utilice **.\Administrator** y especifique la contraseña de DSRM.

Corrija la causa del error de clonación y compruebe que dcpromo.log no indica que no se puede volver a intentar la clonación. Si no es posible volver a intentar la clonación, descarte el medio de forma segura. Si es posible volver a intentar la clonación, debe quitar la etiqueta de arranque de DSRM para volver a intentar la clonación.

1.  Abra Windows Server 2012 con un comando con privilegios elevados (derecha haga clic en Windows Server 2012 y elija Ejecutar como administrador) y, a continuación, escriba **msconfig**.

2.  En la pestaña **Arranque** , en **Opciones de arranque**, desactive **Arranque a prueba de errores** (ya está activada con la opción **Reparar Active Directory**habilitada).

3.  Haga clic en **Aceptar** y reinicie cuando se le solicite que lo haga.

Para obtener más información acerca de la solución de problemas de controladores de dominio virtualizados, consulte el tema relativo a la [solución de problemas de controladores de dominio virtualizados](../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md).


