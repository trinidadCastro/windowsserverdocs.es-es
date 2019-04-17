---
ms.assetid: 7a3114c8-bda8-49bb-83a8-4e04340ab221
title: "Introducción a la virtualización de (AD DS) de servicios de dominio de Active Directory (nivel 100)"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 3c7fdc2e20bc4a27f2555af54f396a15f664d6c7
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="introduction-to-active-directory-domain-services-ad-ds-virtualization-level-100"></a>Introducción a la virtualización de (AD DS) de servicios de dominio de Active Directory (nivel 100)

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Virtualización de los entornos de los servicios de dominio de Active Directory (AD DS) ha sido en curso para un número de años. A partir de Windows Server 2012, AD DS proporciona mayor compatibilidad con controladores de dominio de virtualización Introducción a las funcionalidades de seguridad de virtualización y habilitando la implementación rápida de controladores de dominio virtual a través de clonación. Estas nuevas características de virtualización proporcionan mayor compatibilidad con nubes públicas y privadas, entornos híbridos donde existen de partes de AD DS local y en la nube e infraestructuras de AD DS que residen completamente locales.

**En este documento**

-   [Virtualización de prueba de errores de controladores de dominio](../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#safe_virt_dc)

-   [Clonación del controlador de dominio virtualizada](../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#virtualized_dc_cloning)

-   [Pasos para implementar un controlador de dominio virtualizada clone](../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#steps_deploy_vdc)

-   [Solución de problemas](../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#troubleshooting)

## <a name="safe_virt_dc"></a>Virtualización de prueba de errores de controladores de dominio
Entornos virtuales presentan desafíos para las cargas de trabajo distribuidos que dependen de un esquema de replicación lógica basada en el reloj. Replicación de AD DS, por ejemplo, usa un valor progresión (conocido como un número de secuencia de actualización o USN) asignado a las transacciones en cada controlador de dominio. Instancia de base de datos de cada controlador de dominio también dispone de una identidad, conocida como un Id. de invocación. Id. de invocación de un controlador de dominio y su USN juntos servir como un identificador único asociado a cada transacción de escritura se realiza en cada controlador de dominio y debe ser único en el bosque.

Replicación de AD DS usa Id. de invocación y USN en cada controlador de dominio para determinar qué cambios deben replicarse en otros controladores de dominio. Si un controlador de dominio se revierte en hora fuera del reconocimiento del controlador de dominio y se vuelve a utilizar un USN de una transacción totalmente diferente, no se converge replicación porque otros controladores de dominio se creen que han recibido ya las actualizaciones asociadas con el USN vuelva a usar en el contexto de ese Id. de invocación.

Por ejemplo, en la siguiente ilustración se muestra la secuencia de eventos que se produce en Windows Server 2008 R2 y versiones anteriores del sistema operativo cuando se detecta la reversión de USN en VDC2, el controlador de dominio de destino que se ejecuta en una máquina virtual. En esta ilustración, la detección de reversión de USN se produce en VDC2 cuando un duplicador detecta que VDC2 ha enviado un valor de USN grado de actualización que se ha visto anteriormente el Partner de replicación, que indica que base de datos de VDC2 ha revierten en tiempo de manera incorrecta.

![Introducción a AD DS](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/ADDS_Exampleofhowreplicationcanbecomeinconsistent.png)

Una máquina virtual (VM) facilita el proceso para los administradores de hipervisor revertir un dominio USN del controlador (su reloj lógico), por ejemplo, una instantánea fuera de reconocimiento del controlador de dominio. Para obtener más información acerca de USN y USN reversión, incluida otra ilustración para demostrar sin detectar instancias de reversión de USN, consulta [USN y reversión de USN](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv(WS.10).aspx#usn_and_usn_rollback).

A partir de Windows Server 2012, los controladores de dominio virtual de AD DS hospedados en plataformas de hipervisor que exponen un identificador denominado identificador VM-Generation pueden detectar y emplear medidas de seguridad necesarias para proteger el entorno de AD DS, si la máquina virtual se revierten en tiempo de la aplicación de una instantánea de la máquina virtual. El diseño de VM-GenerationID utiliza un mecanismo independiente del proveedor de hipervisor para exponer este identificador en el espacio de direcciones de la máquina virtual de invitado, por lo que la experiencia de seguridad de virtualización es constante disponibilidad de cualquier hipervisor que admite VM-GenerationID. Este identificador se pueden muestrear servicios y aplicaciones que se ejecuta dentro de la máquina virtual para detectar si una máquina virtual se ha deshecho en el tiempo.

### <a name="BKMK_HowSafeguardsWork"></a>¿Cómo funcionan estas medidas de seguridad de virtualización?
Durante la instalación del controlador de dominio, AD DS inicialmente almacena el identificador de la máquina virtual GenerationID como parte del atributo msDS GenerationID en el objeto de equipo del controlador de dominio en su base de datos (a menudo denominado el árbol de la información de directorio o DIT). Se realiza un seguimiento por separado del GenerationID de la máquina virtual con un controlador de Windows en la máquina virtual.

Cuando un administrador restablece la máquina virtual desde una instantánea anterior, el valor actual de la GenerationID de VM desde el controlador de máquina virtual se compara con un valor en el DIT.

Si los dos valores son diferentes, se restablece el Id. de invocación y el conjunto de RID descarta USN fin de evitar volver a usar. Si los valores son los mismos, la transacción se ejecuta de forma normal.

AD DS también compara el valor actual de la GenerationID de máquina virtual de la máquina virtual con un valor de DIT cada vez que el controlador de dominio se reinicia y, si es diferente, que se restablece el Id. de invocación, descarta el conjunto de RID y actualiza el DIT con el nuevo valor. También no autorizada se sincroniza la carpeta SYSVOL para completar la restauración segura. Esto permite que las medidas de seguridad ampliar la aplicación de instantáneas en máquinas virtuales que estaban apagado. Estas medidas de seguridad que se introdujeron en Windows Server 2012 permiten a los administradores de AD DS aprovechar las ventajas únicas de implementar y administrar los controladores de dominio en un entorno virtualizado.

La siguiente ilustración muestra cómo se aplican las medidas de seguridad de virtualización cuando se detecta la reversión de USN mismo en un controlador de dominio virtualizado que se ejecuta Windows Server 2012 en un hipervisor que admite VM-GenerationID.

![Introducción a AD DS](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/ADDS_VDC_Exampleofhowsafeguardswork.gif)

En este caso, cuando el hipervisor detecta un cambio en el valor de VM-GenerationID, se desencadenan medidas de seguridad de virtualización, incluido el restablecimiento de Id. de invocación para el controlador de dominio virtualizado (de A B en el ejemplo anterior) y actualizar el valor de VM-GenerationID guardados en la máquina virtual para que coincida con el nuevo valor (G2) almacenado por el hipervisor. Las medidas de seguridad Asegúrate de que la replicación converge los controladores de dominio.

Con Windows Server 2012, AD DS, emplea medidas de seguridad en controladores de dominio virtuales alojados en VM-GenerationID cuenta hipervisores y garantiza que la aplicación accidental de instantáneas u otros tales habilitado hipervisor mecanismos que podrían reversión estado de la máquina virtual no interrumpa el entorno de AD DS (por evitar problemas de replicación como una burbuja de USN u objetos persistentes). Sin embargo, no se recomienda restaurar un controlador de dominio mediante la aplicación de una instantánea de la máquina virtual como un mecanismo alternativo para la copia de seguridad de un controlador de dominio. Se recomienda que sigues usando las copias de seguridad de Windows Server o de otras soluciones de copia de seguridad en función de writer VSS.

> [!CAUTION]
> Si un controlador de dominio en un entorno de producción accidentalmente se revierte a una instantánea, es recomendable que consulte a los proveedores de las aplicaciones y restauración de servicios de máquina virtual, para obtener instrucciones sobre cómo comprobar el estado de estos programas después de la instantánea.

Para obtener más información, consulta [virtualizados arquitectura de restauración segura del controlador de dominio](../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_SafeRestoreArch).

## <a name="virtualized_dc_cloning"></a>Clonación del controlador de dominio virtualizada
A partir de Windows Server 2012, los administradores de forma sencilla y segura implementar controladores de dominio de réplica al copiar un controlador de dominio virtual existente. En un entorno virtual, los administradores ya no tengan que implementar una imagen de servidor prepara mediante sysprep.exe repetidamente, promover al servidor a un controlador de dominio y, a continuación, finalizar requisitos de configuración adicionales para la implementación de cada controlador de dominio de réplica.

> [!NOTE]
> Los administradores deben seguir los procesos existentes para implementar el primer controlador de dominio en un dominio, como el uso de un sysprep.exe para preparar un disco de duro virtual (VHD) de servidor, promover al servidor a un controlador de dominio y, a continuación, completa los requisitos de configuración adicionales. En un escenario de recuperación ante desastres, usa la última copia de seguridad del servidor para restaurar el primer controlador de dominio en un dominio.

### <a name="scenarios-that-benefit-from-virtual-domain-controller-cloning"></a>Escenarios pueden beneficiarse de clonación del controlador de dominio virtual

-   Implementación rápida de controladores de dominio adicionales en un dominio nuevo

-   Restaurar rápidamente la continuidad del negocio durante la recuperación ante desastres restaurando la capacidad de AD DS mediante la implementación rápida de controladores de dominio mediante la clonación

-   Optimizar las implementaciones de nube privada aprovechando elástico aprovisionamiento de controladores de dominio para dar cabida a una mayor escala requisitos

-   Aprovisionamiento rápido de entornos de prueba, lo que permite la implementación y las pruebas de nuevas características y funcionalidades antes de la fase de producción

-   Necesidades rápidamente cumplen con mayor capacidad en sucursales duplicando los controladores de dominio existentes en sucursales

Al implementar rápidamente un gran número de controladores de dominio, continúe con los procedimientos para validar el estado de cada controlador de dominio cuando finalice la instalación existentes. Implementar controladores de dominio en lotes razonablemente tamaños para que se puede validar su estado cuando se complete cada lote de instalaciones. El tamaño de lote recomendado es 10. Para obtener más información, consulta [pasos para implementar un controlador de dominio virtualizada clone](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#steps_deploy_vdc).

### <a name="clear-separation-of-responsibilities"></a>Separación clara de responsabilidades
La autorización para clonar controladores de dominio virtualizada está bajo el control del Administrador de AD DS. En el orden de los administradores de hipervisor implementar los controladores de dominio adicionales mediante la copia de los controladores de dominio virtual, el Administrador de AD DS tiene que seleccionar y autorizar a un controlador de dominio y, a continuación, ejecuta los pasos preparatorios para habilitarlo como un origen de clonación.

Con la máquina virtual de aprovisionamiento normalmente en la purview del Administrador de hipervisor, los administradores de hipervisor pueden aprovisionar máquinas de controlador de dominio de réplica copiando controladores de dominio virtualizados que están autorizados y preparados para clonar por el Administrador de AD DS.

> [!WARNING]
> Cualquier persona que pueden administrar el hipervisor que hospeda un controlador de dominio virtual debe ser muy segura y auditada en el entorno.

### <a name="how-does-virtual-domain-controller-cloning-work"></a>¿Cómo funciona el controlador de dominio virtual clonación trabajo?
El proceso de clonación implica realizar una copia de VHD de un controlador de dominio virtual existente (o, para las configuraciones más complejas, el controlador de dominio máquina virtual), autorizarles clonación en AD DS y creación de un archivo de configuración clone. Esto reduce el número de pasos y tiempo implicados en la implementación de una réplica de controlador de dominio virtual mediante la eliminación de lo contrario, las tareas de implementación repetitivo.

El controlador de dominio clone usa los siguientes criterios para detectar que es una copia de otro controlador de dominio:

1.  El valor del identificador VM-Generation proporcionado por la máquina virtual es diferente del valor del identificador VM-Generation almacenado en el DIT.

    > [!NOTE]
    > La plataforma de hipervisor debe admitir VM-Generation identificador (ID VM-Generation compatible con Windows Server 2012 Hyper-V).

2.  Presencia de un archivo llamado DCCloneConfig.xml en una de las siguientes ubicaciones:

    -   El directorio donde reside el DIT

    -   %windir%\Ntds

    -   La raíz de una unidad extraíble

Cuando se cumplan los criterios, pasa por el proceso de clonación aprovisionar como una réplica de controlador de dominio.

El controlador de dominio clone usa el contexto de seguridad del controlador de dominio de origen (el controlador de dominio cuya copia representa) ponerse en contacto con el titular de rol de maestro de operaciones de emulador de controlador de dominio principal (PDC) de Windows Server 2012 (también conocido como operaciones de maestro único flexibles o FSMO). El emulador PDC debe ejecutar Windows Server 2012, pero no tiene que estar ejecutándose en un hipervisor.

> [!NOTE]
> Si tienes un esquema de extensión con atributos que hacen referencia el controlador de dominio de origen y el atributo se encuentra en uno de los objetos copiados (objeto de equipo, objeto de configuración NTDS) para crear la copia, este atributo no se copian o actualizado para hacer referencia el controlador de dominio clone.

Después de comprobar que el controlador de dominio solicitante está autorizado para clonar, el emulador PDC creará una identidad de máquina nueva, incluida la nueva cuenta, SID, nombre y contraseña que identifica este equipo como un controlador de dominio de réplica y enviar esta información a la copia. El controlador de dominio clone, a continuación, se prepara los archivos de base de datos de AD DS para que actúe como una réplica y también se limpiar el estado del equipo.

Para obtener más información, consulta [controlador de dominio virtualizados clonación arquitectura](../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_CloneArch).

### <a name="cloning-components"></a>Componentes de clonación
Los componentes clonación incluyen nuevos cmdlets en el módulo de Active Directory para Windows PowerShell y los archivos XML asociados:

-   **Nueva ADDCCloneConfigFile** "este cmdlet crea y coloca DCCloneConfig.xml en la ubicación correcta para asegurarte de que está disponible para desencadenar la clonación. También realiza comprobaciones de requisitos previos para garantizar la clonación correcta. Se incluye en el módulo de Active Directory para Windows PowerShell. Puede ejecutar localmente en un controlador de dominio virtualizada se prepara para clonar o puedes ejecutar remotamente mediante la - opción sin conexión. Puedes especificar opciones de configuración para el controlador de dominio clone, como su nombre, el sitio y la dirección IP.

    Las comprobaciones de requisitos previos que lleva a cabo son:

    > [!NOTE]
    > El requisito previo comprueba que no son cuando realiza la "se usa la opción sin conexión. Para obtener más información, consulta [New-ADDCCloneConfigFile de ejecución en modo sin conexión](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#BKMK_OfflineMode).

    -   El controlador de dominio se preparó está autorizado para clonar (es un miembro de la **clonación controladores de dominio** grupo)

    -   El emulador ejecuta Windows Server 2012.

    -   Todos los programas o servicios que se enumeran de ejecución **Get ADDCCloningExcludedApplicationList** se incluyen en CustomDCCloneAllowList.xml (se explica más detalladamente al final de esta lista de componentes de clonación).

-   **DCCloneConfig.xml** "para clonar correctamente un controlador de dominio virtualizado, este archivo debe estar presente en el directorio donde se encuentra el DIT, *%windir%\NTDS*, o la raíz de una unidad extraíble. Además se usa como uno de los desencadenadores para detectar e iniciar la clonación, también proporciona un medio para especificar las opciones de configuración para el controlador de dominio clone.

    El esquema y un archivo de muestra para el archivo DCCloneConfig.xml se almacenan en todos los equipos de Windows Server 2012 en:

    -   %windir%\system32\DCCloneConfigSchema.xsd

    -   %windir%\system32\SampleDCCloneConfig.Xml

    Se recomienda que uses el cmdlet New-ADDCCloneConfigFile para crear el archivo DCCloneConfig.xml. Aunque también podría usar el archivo de esquema con un editor XML cuenta para crear este archivo, edita manualmente el archivo aumenta la probabilidad de errores. Si se edita el archivo, debe realizarse mediante el uso de XML editores, como Visual Studio, [el Bloc de notas de XML](https://www.microsoft.com/download/details.aspx?displaylang=en&id=7973), o aplicaciones de terceros (no usar el Bloc de notas).

-   **Get-ADDCCloningExcludedApplicationList** "este cmdlet se ejecuta en el controlador de dominio de origen antes de comenzar el proceso de clonación para determinar los programas instalados o servicios que no estén en la lista predeterminada compatible, DefaultDCCloneAllowList.xml, o una inclusión definidos por el usuario archivo CustomDCCloneAllowList.xml con nombre de lista y lo que no han sido evaluados para clonar impacto.

    Este cmdlet busca el controlador de dominio de origen para los servicios en el Administrador de Control de servicios y programas instalados, en **HKLM\Software\Microsoft\Windows\CurrentVersion\Uninstall** que no se especifican en la (DefaultDCCloneAllowList.xml) lista predeterminada o, si un solo se proporciona, la lista de inclusión definidos por el usuario (CustomDCCloneAllowList.xml file). La lista de aplicaciones y servicios que se devuelve al ejecutar el cmdlet es la diferencia entre lo que se ha proporcionado en el DefaultDCCloneAllowList.xml o el archivo CustomDCCloneAllowList.xml y la lista que se construye en tiempo de ejecución en función de lo que está instalado en el DC de origen. El resultado de servicios y programas de Get-ADDCCloningExcludedApplicationList puede agregarse al archivo CustomDCCloneAllowList.xml si determinas que los servicios y programas pueden segura clonar. Para determinar si un programa de servicio o bien se instalaba puede clonar forma segura, evaluar las siguientes condiciones:

    -   ¿Es el servicio o programa instalado afectadas por la identidad del equipo, como nombre, SID, contraseña y así sucesivamente?

    -   ¿Es el servicio o instalado tienda programa cualquier estado localmente en el equipo que podría afectar a su funcionalidad en la copia de?

    Debe trabajar con el proveedor de software de la aplicación para determinar si el servicio o programa puede segura clonar.

    > [!NOTE]
    > Antes de aprovisionar los programas en el archivo CustomDCCloneAllowList.xml o servicios adicionales, comprueba si tienes la licencia necesarias para copiar dicho software contenido en esa máquina virtual.

    Si las aplicaciones no son clonación, quitarlos desde el controlador de dominio de origen antes de crear el medio de copia. Si una aplicación aparece en la salida del cmdlet, pero no se incluye en el archivo CustomDCCloneAllowList.xml, clonación se producirá un error. Para clonar para tener éxito, la salida del cmdlet no debe enumerar todos los servicios o programas. En otras palabras, una aplicación debe estar incluida en el archivo CustomDCCloneAllowList.xml o quitará el controlador de dominio de origen.

    La siguiente tabla explican las opciones para ejecutar Get-ADDCCloningExcludedApplicationList.

    |||
    |-|-|
    |Argumento|Explicación|
    |*<no argument specified>*|Muestra una lista de programas o servicios en la consola que no se hayan contabilizado para clonar. Si ya hay un CustomDCCloneAllowList.XML en cualquiera de las ubicaciones aceptable, utiliza ese archivo a las pantallas el resto de servicios y programas (que puede ser nada si coincide con las listas).|
    |-GenerateXml|Crea el archivo CustomDCCloneAllowList.XML rellenado con los servicios y programas que aparecen en la consola.|
    |-Force|Sobrescribe un archivo CustomDCCloneAllowList.XML existente.|
    |-Path|Ruta de acceso de carpeta para crear el CustomDCCloneAllowList.XML.|

-   **DefaultDCCloneAllowList.xml** "este archivo está presente de forma predeterminada en cada Windows Server 2012 dominio controlador en el *%windir%\system32*. Enumera los servicios y programas instalados que se pueden clonar de forma segura de manera predeterminada. No debes cambiar la ubicación o el contenido de este archivo o clonación se producirá un error.

-   **CustomDCCloneAllowList.xml** "Si tienes servicios o programas instalados que residen en el controlador de dominio de origen que están fuera de los indicados en el archivo DefaultDCCloneAllowList.xml, aquellos servicios y programas deben incluirse en este archivo. Para encontrar los servicios o programas instalados que no aparecen en el en el archivo DefaultDCCloneAllowList.xml, ejecuta el **Get ADDCCloningExcludedApplicationList** cmdlet. Debes usar la **"GenerateXml** argumento para generar el archivo XML.

    El proceso de clonación comprueba las siguientes ubicaciones en orden para este archivo y utiliza el primer archivo XML que se encuentra, independientemente del contenido de la otra carpeta:

    1.  La siguiente clave del registro:

        ```
        HKey_Local_Machine\System\CurrentControlSet\Services\NTDS\Parameters
        AllowListFolder (REG_SZ)
        ```

    2.  Directorio de trabajo de DSA

    3.  %systemroot%\NTDS

    4.  Medios extraíbles de lectura y escritura, por orden de la letra de unidad, en la raíz de la unidad

### <a name="deployment-scenarios"></a>Escenarios de implementación
Se admiten los siguientes escenarios de implementación para la clonación del controlador de dominio virtual:

-   Implementar un controlador de dominio clone al hacer una copia del archivo de disco duro virtual (vhd) de un controlador de dominio de origen.

-   Implementar un controlador de dominio clone copiando la máquina virtual de un controlador de dominio de origen con la semántica de exportación e importación expuesta por el hipervisor.

> [!NOTE]
> Los pasos en la sección [pasos para implementar un controlador de dominio virtualizada clone](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#steps_deploy_vdc) demostrar copiar una máquina virtual con la característica de exportación e importación de Windows Server 2012 Hyper-V.

## <a name="steps_deploy_vdc"></a>Pasos para implementar un controlador de dominio virtualizada clone

-   [Requisitos previos](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#prerequisites)

-   [Paso 1: Conceder el controlador de dominio virtualizada de origen el permiso para clonar](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#bkmk4_grant_source)

-   [Paso 2: Ejecutar Get-ADDCCloningExcludedApplicationList cmdlet](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#bkmk6_run_get-addccloningexcludedapplicationlist_cmdlet)

-   [Paso 3: Ejecutar New-ADDCCloneConfigFile](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#bkmk5_create_insert_dccloneconfig)

-   [Paso 4: Exportar e importar la máquina virtual del controlador de dominio de origen](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#bkmk7_export_import_vm_sourcedc)

### <a name="prerequisites"></a>Requisitos previos

-   Para completar los pasos descritos en los siguientes procedimientos, debe ser miembro del grupo Domain Admins o tener los permisos equivalentes asignados.

-   Los comandos de Windows PowerShell que se usa en esta guía se deben ejecutar desde un símbolo del sistema con privilegios elevados. Para ello, haz clic en el **Windows PowerShell** icono y, a continuación, haz clic en **ejecutar como administrador**.

-   Un servidor de Windows Server 2012 con el rol de servidor de Hyper-V instalado (**HyperV1**).

-   Un segundo servidor de Windows Server 2012 con el rol de servidor de Hyper-V instalado (**HyperV2**).

    > [!NOTE]
    > -   Si usas otro hipervisor, debe ponerse en contacto con el proveedor de ese hipervisor para comprobar si el hipervisor admite Id. de VM-Generation Si el hipervisor no es compatible con el identificador de VM-Generation y ha proporcionado una DCCloneConfig.xml, la nueva máquina virtual arrancará en modo de restauración de servicios de directorio (DSRM).
    > -   Para aumentar la disponibilidad del servicio de AD DS, esta guía se recomienda y proporciona instrucciones a través de dos hosts de Hyper-V diferentes, lo que ayuda a evitar que un potencialmente único punto de error. Sin embargo, no es necesario dos hosts de Hyper-V para realizar la clonación del controlador de dominio virtual.
    > -   Debes ser miembro del grupo de administradores locales en cada servidor Hyper-V (**HyperV1** y **HyperV2**).
    > -   Para importar y exportar un archivo de disco duro virtual con Hyper-V correctamente, los conmutadores de red virtual en los hosts de Hyper-V deben tener el mismo nombre. Por ejemplo, si tienes una red virtual encender **HyperV1** denominado VNet, a continuación, debe haber una red virtual se enciende **HyperV2** denominado VNet.
    > -   Si los dos hosts de Hyper-V (**HyperV1** y **HyperV2**) tienen diferentes procesadores, cierre la máquina virtual (**VirtualDC1**) que vas a exportar, en la máquina virtual, haz clic **configuración**, haz clic en **procesador**y, en **compatibilidad del procesador** selecciona **migrar a un equipo físico con una versión de procesador diferente** y haz clic en **Aceptar**.

-   Implementado Windows Server 2012 controlador de dominio (virtualizado o físico) que hospeda el rol de emulador PDC (**DC1**). Para comprobar si el rol de emulador PDC está alojado en un controlador de dominio de Windows Server 2012, ejecuta el siguiente comando de Windows PowerShell:

    ```
    Get-ADComputer (Get-ADDomainController "Discover "Service "PrimaryDC").name "Property operatingsystemversion | fl
    ```

    El valor de OperatingSystemVersion debe devolver como una versión 6.2. Si es necesario, puedes transferir el rol de emulador PDC a un controlador de dominio que ejecute Windows Server 2012. Para obtener más información, consulta [usar Ntdsutil.exe para asumir funciones FSMO a un controlador de dominio o transferir](https://support.microsoft.com/kb/255504).

-   Un controlador de dominio virtualizada de invitado de Windows Server 2012 implementado (**VirtualDC1**) que se encuentra en el mismo dominio que aloja el rol de emulador PDC el controlador de dominio de Windows Server 2012 (**DC1**). Esto hará que el controlador de dominio de origen utilizado para clonar. El controlador de dominio virtual de invitado se hospedará en un servidor de Windows Server 2012 Hyper-V (**HyperV1**).

    > [!NOTE]
    > -   Para clonar para tener éxito, no puede ser el controlador de dominio de origen que se usa para crear la copia de un controlador de dominio que ha sido degradado desde que se creó el origen multimedia VHD.
    > -   Apagar el controlador de dominio de origen antes de copiar la máquina virtual o en su disco duro virtual.
    > -   No deben duplicar un disco duro virtual o restaurar una instantánea que es más antiguo que el valor de ciclo de vida de desecho (o el valor de duración del objeto eliminado si está habilitada la Papelera de reciclaje de Active Directory). Si va a copiar un disco duro virtual de un controlador de dominio, asegúrate de que el archivo VHD no es más antiguo que la duración de desecho valor (de manera predeterminada, 60 días). No debe copiar un disco duro virtual de un controlador de dominio de ejecución para crear medios de clone.

    Expulsar cualquier unidad de disco virtual (VFD) el origen de controlador de dominio puede tener. Esto puede causar un problema de uso compartido cuando se intenta importar la nueva máquina virtual.

    Solo Windows Server 2012 controladores de dominio hospedados en un hipervisor VM-GenerationID pueden usarse como origen para clonar. El controlador de dominio de Windows Server 2012 de origen usado para clonar debe estar en buen estado. Para determinar el estado del controlador de dominio de origen ejecuta [dcdiag](https://technet.microsoft.com/library/cc731968(WS.10).aspx). ¿Para comprender mejor de los resultados devueltos por dcdiag, consulta [lo que hace realmente DCDIAG... hacer?](http://blogs.technet.com/b/askds/archive/2011/03/22/what-does-dcdiag-actually-do.aspx).

    Si el controlador de dominio de origen es un servidor DNS, el controlador de dominio clonados también estará un servidor DNS. Debes seleccionar a un servidor DNS que las zonas solo integradas en Active Directory de hosts.

    Configuración del cliente DNS no se clona pero en su lugar, se especifica en el archivo DCCloneConfig.xml. Si no se especifican, el controlador de dominio clonados apuntará a sí mismo como servidor DNS preferido de manera predeterminada. El controlador de dominio clonados no tendrá una delegación DNS. El Administrador de la zona DNS principal debe actualizar la delegación de DNS para el controlador de dominio clonados según sea necesario.

    > [!WARNING]
    > Las medidas de seguridad de virtualización no se extiende a Active Directory Lightweight Directory Services (AD LDS). Por lo tanto no deben intentar clonar un controlador de dominio de AD DS que hospeda una instancia de AD LDS agregando esta instancia de AD LDS a la CustomDCCloneAllowList.xml. Como AD LDS no es VM-Generation Id. de cuenta, clonación un controlador de dominio con AD LDS puede causar divergencia induce reversión de USN en ese AD LDS conjunto de configuración.

    No se admiten los siguientes roles de servidor para clonar:

    -   Protocolo de configuración dinámica de Host (DHCP)

    -   Servicios de certificados de Active Directory (AD CS)

    -   Active Directory Rights Management Services (AD LDS)

### <a name="bkmk4_grant_source"></a>Paso 1: Conceder el controlador de dominio virtualizada de origen el permiso para clonar
En este procedimiento, conceder el controlador de dominio de origen el permiso para ser clonados utilizando **centro de administración de Active Directory** para agregar el controlador de dominio de origen a la **clonación controladores de dominio** grupo.

##### <a name="to-grant-the-source-virtualized-domain-controller-the-permission-to-be-cloned"></a>Para conceder el origen virtualizar el permiso clonación del controlador de dominio

1.  En cualquier controlador de dominio en el mismo dominio que el controlador de dominio se preparó para clonar (**VirtualDC1**), abre **centro de administración de Active Directory** (ADAC), busque el objeto de controlador de dominio virtualizados (controladores de dominio se ubican en el **controladores de dominio** contenedor en ADAC), derecho haz clic en él, elige **agregar al grupo** y, en **escribe el nombre de objeto para seleccionar** tipo **clonación controladores de dominio** y, a continuación, haz clic en **Aceptar**.

    La actualización de pertenencia a grupo realizada en este paso debe replicar en emulador PDC antes de realizar la clonación. Si la **clonación controladores de dominio** grupo no se encuentra, el rol de emulador PDC no es posible que se hospeden en un controlador de dominio que ejecute Windows Server 2012.

    > [!NOTE]
    > Para abrir ADAC en un controlador de dominio de Windows Server 2012, abre Windows PowerShell y escribe **dsac.exe**.

![Introducción a AD DS](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos ***

El siguiente cmdlet de Windows PowerShell realiza la misma función que el procedimiento anterior:


    Add-ADGroupMember "Identity "CN=Cloneable Domain Controllers,CN=Users, DC=Fabrikam,DC=Com" "Member "CN=VirtualDC1,OU=Domain Controllers,DC=Fabrikam,DC=com"


### <a name="bkmk6_run_get-addccloningexcludedapplicationlist_cmdlet"></a>Paso 2: Ejecutar Get-ADDCCloningExcludedApplicationList cmdlet
En este procedimiento, ejecuta el `Get-ADDCCloningExcludedApplicationList`cmdlet en el controlador de dominio virtualizada de origen para identificar los programas o servicios que no se evalúan para clonar. Debes ejecutar el cmdlet Get-ADDCCloningExcludedApplicationList antes el cmdlet New-ADDCCloneConfigFile porque si el cmdlet New-ADDCCloneConfigFile detecta una aplicación excluida, no se creará un archivo DCCloneConfig.xml.

##### <a name="to-identify-applications-or-services-that-run-on-a-source-domain-controller-which-have-not-been-evaluated-for-cloning"></a>Para identificar las aplicaciones o servicios que se ejecutan en un controlador de dominio de origen que no han sido evaluados para clonar

1.  En el controlador de dominio de origen (**VirtualDC1**), haz clic en **administrador del servidor**, haz clic en **herramientas**, haz clic en **módulo Active Directory para Windows PowerShell** y, a continuación, escribe el siguiente comando:


    Get-ADDCCloningExcludedApplicationList


2.  Investigar la lista de los servicios devueltos y los programas instalados con el proveedor de software para determinar si se pueden clonar forma segura. Si las aplicaciones o servicios en la lista no se puede clonar forma segura, debes quitar desde el controlador de dominio de origen o clonación se producirá un error.

3.  El conjunto de servicios y programas instalados que se determinaron clonación de forma segura, ejecuta el comando nuevo con la **"GenerateXML** conmutador para aprovisionar estos servicios y programas en el **CustomDCCloneAllowList.xml** archivo.


    Get-ADDCCloningExcludedApplicationList - GenerateXml


### <a name="bkmk5_create_insert_dccloneconfig"></a>Paso 3: Ejecutar New-ADDCCloneConfigFile
Ejecutar New-ADDCCloneConfigFile en el controlador de dominio de origen y, opcionalmente, especificar opciones de configuración para el controlador de dominio clone, como el nombre, la dirección IP y la resolución DNS.

Por ejemplo, para crear un controlador de dominio clone denominado VirtualDC2 con una dirección IPv4 estática, escribe:


    New-ADDCCloneConfigFile "Static -IPv4Address "10.0.0.2" -IPv4DNSResolver "10.0.0.1" -IPv4SubnetMask "255.255.255.0" -CloneComputerName "VirtualDC2" -IPv4DefaultGateway "10.0.0.3" -SiteName "REDMOND"

> [!NOTE]
> El controlador de dominio clone estará en el mismo sitio que el controlador de dominio de origen, a menos que un sitio distinto se especifica en el archivo DCCloneConfig.xml. Se recomienda que especificar un sitio apropiado en el archivo DCCloneConfig.xml para el controlador de dominio clone según su dirección IP.

El nombre del equipo es opcional. Si no especificas uno, se generará un nombre único basado en el siguiente algoritmo:

-   El prefijo consiste en los primeros 8 caracteres del nombre de equipo de controlador de dominio de origen. Por ejemplo, un nombre de equipo de origen de SourceComputer se trunca en una cadena de prefijo de SourceCo.

-   El sufijo de nomenclatura único del formato "" CL*nnnn*"se anexa a la cadena de prefijo donde *nnnn*es el siguiente valor disponible desde 0001-9999 que el PDC determina que no está actualmente en uso. Por ejemplo, si 0047 es el siguiente número disponible en el intervalo permitido, usando el ejemplo anterior del prefijo de nombre de equipo SourceCo, el nombre que se usará para el equipo clone derivado se establecerá como SourceCo CL0047.

> [!NOTE]
> Un servidor de catálogo global (GC) se requiere para el cmdlet New-ADDCCloneConfigFile funcione correctamente. La pertenencia del controlador de dominio de origen a la **clonación controladores de dominio** grupo debe reflejarse en el catálogo global. El catálogo global no tiene que ser el mismo controlador de dominio como el emulador PDC, pero preferiblemente debería estar en el mismo sitio. Si un catálogo global no está disponible, el comando produce el error "el servidor no está operativo." Para obtener más información, consulta [virtualizada de solución de problemas de controlador de dominio](../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md).

Para crear un controlador de dominio clone denominado Clone1 con una configuración estática IPv4 y especificar preferidos y alternativos servidores WINS, escribe:


    New-ADDCCloneConfigFile "CloneComputerName "Clone1" "Static -IPv4Address "10.0.0.5" "IPv4DNSResolver "10.0.0.1" "IPv4SubnetMask "255.255.0.0" "PreferredWinsServer "10.0.0.1" "AlternateWinsServer "10.0.0.2"


> [!NOTE]
> Si especificas servidores WINS, debes especificar tanto **"PreferredWINSServer** y **" AlternateWINSServer**. Si especificas solo de los argumentos, clonación produce un error con código 0x80041005 de error que aparece en dcpromo.log.

Para crear un controlador de dominio clone denominado Clone2 con valores dinámicos de IPv4, escribe:


    New-ADDCCloneConfigFile -CloneComputerName "Clone2" -IPv4DNSResolver "10.0.0.1" 


> [!NOTE]
> En este caso, debe haber un servidor DHCP en el entorno de que la copia puede alcanzar y obtener la dirección IP y otras opciones de configuración de red correspondiente.

Para crear un controlador de dominio clone denominado Clone2 con valores dinámicos de IPv4 y especificar preferidos y alternativos servidores WINS, escribe:


    New-ADDCCloneConfigFile -CloneComputerName "Clone2" -IPv4DNSResolver "10.0.0.1" -SiteName "REDMOND" "PreferredWinsServer "10.0.0.1" "AlternateWinsServer "10.0.0.2"


Para crear un controlador de dominio clone con valores dinámicos de IPv6, escribe:


    New-ADDCCloneConfigFile -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc"


Para crear un controlador de dominio clone con una configuración estática IPv6, escribe:


    New-ADDCCloneConfigFile "Static -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc"


> [!NOTE]
> Al especificar la configuración de IPv6, la única diferencia entre los valores estáticos y dinámicos es la inclusión de **-estático** cambiar. La inclusión de la **-estático** conmutador hace obligatorio para especificar al menos uno **IPv6DNSResolver**. Se espera que la dirección IPv6 estática se configuren a través de configuración automática de direcciones sin estado (SLAAC) con prefijos de enrutador asignado. Con IPv6 dinámico, las resoluciones DNS son opcionales, pero se espera que la copia puede llegar a un servidor DHCP IPv6 habilitado en la subred para obtener información de configuración de DNS y dirección IPv6.

#### <a name="BKMK_OfflineMode"></a>Ejecuta New-ADDCCloneConfigFile en modo sin conexión
Si tienes varias copias de los medios de controlador de dominio de origen que se han preparado para clonar (lo que significa que el controlador de dominio de origen está autorizado para clonar, ha sido el cmdlet Get-ADDCCloningExcludedApplicationList ejecutar etc.) y para especificar una configuración diferente para cada copia del medio, puede ejecutar New-ADDCCloneConfigFile en modo sin conexión. Esto puede ser más eficiente que preparar individualmente cada máquina virtual, por ejemplo, mediante la importación de cada copia.

En este caso, los administradores de dominio pueden montar el disco sin conexión y usar herramientas de administración remota de servidor (RSAT) para ejecutar el cmdlet New-ADDCCloneConfigFile con-argumento sin conexión con el fin de agregar los archivos XML, que permite la automatización de la fábrica similar con nuevas opciones de Windows PowerShell que se incluye en Windows Server 2012. Para obtener más información sobre cómo montar el disco sin conexión para poder ejecutar el cmdlet New-ADDCCloneConfigFile en modo sin conexión, consulta [agregar XML para el disco del sistema sin conexión](../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Deployment-and-Configuration.md#BKMK_Offline).

Primero debe ejecutar el cmdlet localmente en los medios de origen para garantizar que pasan de comprobaciones de ese requisito previo. No se realizan las comprobaciones de requisitos previos en modo sin conexión porque se pueden ejecutar el cmdlet desde un equipo que no estén desde el mismo dominio o desde un equipo unido al dominio. Después de ejecutar el cmdlet localmente, se creará un archivo DCCloneConfig.xml. Puedes eliminar DCCloneConfig.xml que se crea localmente si vas a usar el modo sin conexión posteriormente.

Para crear un controlador de dominio clone denominado CloneDC1 en modo sin conexión, en un sitio de REDMOND"dirección IPv4 estática, tipo:


    New-ADDCCloneConfigFile -Offline -CloneComputerName CloneDC1 -SiteName REDMOND -IPv4Address "10.0.0.2" -IPv4DNSResolver "10.0.0.1" -IPv4SubnetMask "255.255.0.0" -IPv4DefaultGateway "10.0.0.1" -Static -Path F:\Windows\NTDS


Para crear un controlador de dominio clone denominado Clone2 en modo sin conexión con estática IPv4 y IPv6 una configuración estática, escribe:


    New-ADDCCloneConfigFile -Offline -IPv4Address "10.0.0.2" -IPv4DNSResolver "10.0.0.1" -IPv4SubnetMask "255.255.0.0" -Static -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -CloneComputerName "Clone2" -PreferredWINSServer "10.0.0.1" -AlternateWINSServer "10.0.0.3" -Path F:\Windows\NTDS


Para crear un controlador de dominio clone en modo sin conexión con estática IPv4 y IPv6 dinámico y especificar varios servidores DNS para la configuración de resolución DNS, escribe:


    New-ADDCCloneConfigFile -Offline -IPv4Address "10.0.0.10" -IPv4SubnetMask "255.255.0.0" -IPv4DefaultGateway "10.0.0.1" -IPv4DNSResolver @( "10.0.0.1","10.0.0.2" ) -Static -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -Path F:\Windows\NTDS 


Para crear un controlador de dominio clone denominado Clone1 en modo sin conexión con dinámico IPv4 e IPv6 una configuración estática, escribe:


    New-ADDCCloneConfigFile -Offline -Static -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -CloneComputerName "Clone1" -PreferredWINSServer "10.0.0.1" -AlternateWINSServer "10.0.0.3" -SiteName "REDMOND" -Path F:\Windows\NTDS


Para crear un controlador de dominio clone en modo sin conexión con dinámico IPv4 y IPv6 dinámico, escribe:


    New-ADDCCloneConfigFile -Offline -IPv4DNSResolver "10.0.0.1" -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -Path F:\Windows\NTDS


### <a name="bkmk7_export_import_vm_sourcedc"></a>Paso 4: Exportar e importar la máquina virtual del controlador de dominio de origen
En este procedimiento, exporta la máquina virtual del controlador de dominio virtualizada de origen y, a continuación, importar la máquina virtual. Esta acción crea un controlador de dominio virtualizada clone en tu dominio.

Debes ser miembro del grupo de administradores locales en cada host de Hyper-V. Si usas credenciales diferentes para cada servidor, ejecutar los cmdlets de Windows PowerShell para exportar e importar la máquina virtual en las diferentes sesiones de Windows PowerShell.

Si hay instantáneas en el controlador de dominio de origen, debe eliminarse antes de exporta el controlador de dominio de la fuente porque la máquina virtual no se importará si una instantánea tiene opciones de configuración del procesador que no son compatibles con el host de hyper-v de destino. Si la configuración de procesador es compatible entre los hosts de origen y destino hyper-v, puede exportar y copia el origen sin eliminar instantáneas de antemano. Después de la importación, sin embargo, las instantáneas deben eliminarse de la máquina virtual clone antes de que empiece.

##### <a name="to-copy-a-virtual-domain-controller-by-exporting-and-then-importing-the-virtualized-source-domain-controller"></a>Para copiar un controlador de dominio virtual exportar e importar, a continuación, el controlador de dominio de origen virtualizada

1.  En **HyperV1**, apagado, el controlador de dominio de origen (**VirtualDC1**).

    ![Introducción a AD DS](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos ***

    Stop-VM-VirtualDC1 - ComputerName HyperV1 el nombre


2.  En **HyperV1**, eliminar instantáneas y luego exportar el controlador de dominio de origen (VirtualDC1) en el directorio c:\CloneDCs.

> [!NOTE]
> Deberías eliminar todas las instantáneas asociadas porque cada vez que se toma una instantánea, nuevo AVHD se crea un archivo que actúa como discos de diferenciación. Esto crea un efecto en cadena. Si se han tomado instantáneas e inserta el archivo DCCLoneConfig.xml en el disco duro virtual, puede terminar la creación de un clone desde una versión anterior de DIT o insertar el archivo de configuración en el archivo VHD incorrecto. Eliminar la instantánea combina todas estas AVHDs en el VHD base.

![Introducción a AD DS](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos ***


    Get-VMSnapshot VirtualDC1 | Remove-VMSnapshot -IncludeAllChildSnapshots
    Export-VM -Name VirtualDC1 -ComputerName HyperV1 -Path c:\CloneDCs\VirtualDC1


3.  Copia la carpeta **virtualdc1** en el directorio c:\Import de **HyperV2**.

4.  En **HyperV2**, con **Administrador de Hyper-V**, importar la máquina virtual (con la **importar Virtual Machine** asistente en **Administrador de Hyper-V**) desde la carpeta **c:\Import\virtualdc1** y eliminar todos los asociados **instantáneas**.

Usa el **copiar la máquina virtual (crear nuevo Id. único)** opción al importar la máquina virtual.

![Introducción a AD DS](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos ***

    $path = Get-ChildItem "C:\CloneDCs\VirtualDC1\VirtualDC1\Virtual Machines"
    $vm = Import-VM -Path $path.fullname -Copy -GenerateNewId
    Rename-VM $vm VirtualDC2


Para crear copia de varios controladores de dominio desde el mismo controlador de dominio de origen:

  -   Interfaz de usuario: en el **Importar máquina Virtual** asistente, especificar nuevas ubicaciones para **carpeta de configuración de la máquina Virtual**, **tienda instantánea**, **carpeta paginación inteligente**y otra **ubicación** para los discos duros virtuales para la máquina virtual.

  -   Windows PowerShell: especificar nuevas ubicaciones para la máquina virtual con los siguientes parámetros para el `Import-VM`cmdlet:

        $path = Get-ChildItem "C:\CloneDCs\VirtualDC1\VirtualDC1\Virtual máquinas" Import-VM-ruta de acceso $path.fullname-copia - GenerateNewId - ComputerName HyperV2 - VhdDestinationPath "path" - SnapshotFilePath "path" - SmartPagingFilePath "path" - VirtualMachinePath "path"


> [!NOTE]
> El tamaño de lote recomendado para crear clone varios controladores de dominio al mismo tiempo es 10. El número máximo está restringido por el número máximo de conexiones de replicación salientes, cuyo valor predeterminado es 16 replicación del sistema de archivos distribuido (DFSR) y 10 para el servicio de replicación de archivos (FRS). No deben implementarse más que el número de controladores de dominio clone recomendadas simultáneamente a menos que has probado exhaustivamente ese número para su entorno.

5.  En **HyperV1**, reinicie el controlador de dominio de origen (**(VirtualDC1**) para ponerlo en línea.

![Introducción a AD DS](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos ***

    Start-VM -Name VirtualDC1 -ComputerName HyperV1


6.  En **HyperV2**, inicia la máquina virtual (**VirtualDC2**) para que aparezca en pantalla como un controlador de dominio clone en el dominio.

![Introducción a AD DS](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos ***


    Start-VM -Name VirtualDC2 -ComputerName HyperV2

> [!NOTE]
> Debe tener instalado el emulador PDC para clonar para tener éxito. Si estaba apagado, asegúrate de que se ha iniciado y realiza la sincronización inicial para que sea consciente es decir contiene el rol de emulador PDC. Para obtener más información, consulta el tema Microsoft [artículo de Knowledge Base 305476](https://support.microsoft.com/kb/305476).

Una vez completada la clonación, comprueba el nombre del equipo para garantizar la operación de clonación clone se realizó correctamente. Comprueba que la máquina virtual no se inició en el modo de restauración de servicios de directorio (DSRM). Si intentas iniciar sesión y aparece un error que indica que ningún servidor está disponibles, intenta iniciar sesión DSRM. Si el controlador de dominio no se ha clonar correctamente y se inicia en DSRM, compruebe los registros del Visor de eventos y registros de dcpromo en la carpeta %systemroot%/debug.

El controlador de dominio clonados será un miembro de la **clonación controladores de dominio** grupo porque la pertenencia a copia desde el controlador de dominio de origen. Como procedimiento recomendado, debes dejar el **clonación controladores de dominio** vacío de grupo hasta que esté listo para realizar operaciones de clonación, y deberás quitar miembros una vez finalizadas las operaciones de clonación.

Si el controlador de dominio de origen almacena un medio de copia de seguridad, el controlador de dominio clonados también almacenará el medio de copia de seguridad. Puedes ejecutar `wbadmin get versions`para mostrar el medio de copia de seguridad en el controlador de dominio clonados. Un miembro del grupo Administradores de dominio debe eliminar el medio de copia de seguridad en el controlador de dominio clonados para evitar que accidentalmente se restaure. Para obtener más información sobre cómo eliminar una copia de seguridad con wbadmin.exe, consulta [Wbadmin eliminar systemstatebackup](https://technet.microsoft.com/library/cc742081(v=WS.10).aspx).

## <a name="troubleshooting"></a>Solución de problemas
Si el controlador de dominio clone (**VirtualDC2**) se inicie en modo de restauración de servicios de directorio (DSRM), no devuelve a un modo normal en su propio en el siguiente reinicio. Para iniciar sesión en un controlador de dominio que se inicia en DSRM, usar **. \Administrator** y especificar la contraseña DSRM.

Corregir la causa de errores de clonación y comprueba que la dcpromo.log no indica que no se volverá a intentar clonación. Si no se volverá a intentar la clonación, descartar con seguridad los medios. Si clonación puede volver a intentar, debes quitar la marca de arranque del modo de restauración de DS con el fin de intentar clonación nuevo.

1.  Abre Windows Server 2012 con un comando con privilegios elevados (derecho haz clic en Windows Server 2012 y elige ejecutar como administrador) y, a continuación, escribe **msconfig**.

2.  En la **arranque** ficha **opciones de arranque**, desactiva **arranque seguro** (ya está seleccionada con la opción **reparación de Active Directory habilitada**).

3.  Haz clic en **Aceptar** y reiniciar cuando se te solicite.

Para obtener más información sobre solución de problemas acerca de los controladores de dominio virtualizado, consulta [virtualizada de solución de problemas de controlador de dominio](../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md).


