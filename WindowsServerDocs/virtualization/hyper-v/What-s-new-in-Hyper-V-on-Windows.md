---
title: Novedades de Hyper-V en Windows Server 2016
description: Proporciona un resumen de las nuevas características de Hyper-V.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.topic: article
ms.assetid: 1a65a98e-54b6-4c41-9732-1e3d32fe3a5f
author: kbdazure
ms.author: kathydav
ms.date: 09/21/2017
ms.openlocfilehash: 5fc82d8eea78ad5605dceb6a21e8d543f9d9c88e
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857968"
---
# <a name="whats-new-in-hyper-v-on-windows-server"></a>Novedades de Hyper-V en Windows Server

>Se aplica a: Windows Server 2019, Microsoft Hyper-V Server 2016, Windows Server 2016
  
En este artículo se explica la funcionalidad nueva y modificada de Hyper-V en Windows Server 2019, Windows Server 2016 y Microsoft Hyper-V Server 2016. Para usar nuevas características en máquinas virtuales creadas con Windows Server 2012 R2 y que se han migrado o importado a un servidor que ejecuta Hyper-V en Windows Server 2019 o Windows Server 2016, deberá actualizar manualmente la versión de configuración de la máquina virtual. Para obtener instrucciones, consulte Actualización de la versión de la [máquina virtual](deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md).  
  
Este es el tema que se incluye en este artículo y si la funcionalidad es nueva o actualizada.  

## <a name="windows-server-version-1903"></a>Windows Server, versión 1903

### <a name="add-hyper-v-manager-to-server-core-installations-updated"></a>Agregar el administrador de Hyper-V a las instalaciones Server Core (actualizado)

Como ya sabes, se recomienda usar la opción de instalación básica cuando se usa el canal semianual en producción de Windows Server. Sin embargo, de manera predeterminada, la instalación básica omite algunas herramientas de administración útiles. Para agregar muchas de las herramientas que se usan con más frecuencia, puedes instalar la característica App Compatibility, pero aún así faltan algunas herramientas.

Por lo tanto, en función de los comentarios de los clientes, agregamos una más herramientas a la característica de compatibilidad de aplicaciones en esta versión: el administrador de Hyper-V (virtmgmt. msc).

Para más información, consulta [Característica App Compatibility de Server Core](../../get-started-19/install-fod-19.md).

## <a name="windows-server-2019"></a>Windows Server 2019

### <a name="security-shielded-virtual-machines-improvements-new"></a>Seguridad: mejoras en Virtual Machines blindadas (novedad)

- **Mejoras de sucursal**

    Ahora puedes ejecutar máquinas virtuales blindadas en máquinas con conectividad intermitente para el Servicio de protección de host al aprovechar las nuevas características [HGS reserva](https://docs.microsoft.com/windows-server/security/guarded-fabric-shielded-vm/guarded-fabric-manage-branch-office#fallback-configuration) y [modo sin conexión](https://docs.microsoft.com/windows-server/security/guarded-fabric-shielded-vm/guarded-fabric-manage-branch-office#offline-mode). HGS de reserva te permite configurar un segundo conjunto de direcciones URL para Hyper-V para probar si no puede conectar con el servidor HGS principal.

    El modo sin conexión te permite continuar iniciando las máquinas virtuales blindadas, incluso si no se puede conectar a HGS, siempre y cuando la máquina virtual se haya iniciado correctamente una vez y no haya cambiado la configuración de seguridad del host.

- **Solución de problemas de las mejoras**

    También hemos facilitado la [solución de problemas de las máquinas virtuales blindadas](https://docs.microsoft.com/windows-server/security/guarded-fabric-shielded-vm/guarded-fabric-troubleshoot-shielded-vms) habilitando el soporte con el modo de sesión mejorada de VMConnect y PowerShell Direct. Estas herramientas son especialmente útiles si perdiste la conectividad de red con la VM y necesitas actualizar su configuración para restaurar el acceso.

    No es necesario configurar estas características y están disponibles automáticamente cuando se coloca una VM blindada en un host de Hyper-V con Windows Server, versión 1803 o posterior.

- **Compatibilidad con Linux**

    Si ejecutas entornos de sistemas operativos mixtos, Windows Server 2019 admite ahora la ejecución de Ubuntu, Red Hat Enterprise Linux y SUSE Linux Enterprise Server dentro de las máquinas virtuales blindadas.

## <a name="windows-server-2016"></a>Windows Server 2016

### <a name="compatible-with-connected-standby-new"></a>Compatible con el modo de espera conectado \(nuevo\)

Cuando se instala el rol de Hyper-V en un equipo que usa el modelo de energía siempre activado o conectado (AOAC), el estado de energía de **modo de espera conectado** ahora está disponible.  
  
### <a name="discrete-device-assignment-new"></a>Asignación de dispositivos discretos \(nueva\)

Esta característica permite conceder a una máquina virtual acceso directo y exclusivo a algunos dispositivos de hardware de PCIe. El uso de un dispositivo de esta manera omite la pila de virtualización de Hyper-V, lo que da lugar a un acceso más rápido. Para obtener más información sobre el hardware compatible, consulte la sección sobre la asignación discreta de dispositivos en [requisitos del sistema para Hyper-V en Windows Server 2016](System-requirements-for-Hyper-V-on-Windows.md). Para obtener más información, incluido el uso de esta característica y consideraciones, vea el apartado "[asignación de dispositivos discretos — Descripción y fondo](https://blogs.technet.microsoft.com/virtualization/2015/11/19/discrete-device-assignment-description-and-background/)" en el blog de virtualización.

### <a name="encryption-support-for-the-operating-system-disk-in-generation-1-virtual-machines-new"></a>Compatibilidad con el cifrado del disco del sistema operativo en máquinas virtuales de generación 1 \(nuevo)

Ahora puede proteger el disco del sistema operativo mediante el cifrado de unidad BitLocker en máquinas virtuales de generación 1. Una nueva característica, almacenamiento de claves, crea una pequeña unidad dedicada para almacenar la clave de BitLocker de la unidad del sistema. Esto se hace en lugar de usar un Módulo de plataforma segura virtual (TPM), que solo está disponible en las máquinas virtuales de generación 2. Para descifrar el disco e iniciar la máquina virtual, el host de Hyper-V debe formar parte de un tejido protegido autorizado o tener la clave privada de uno de los tutores de la máquina virtual. El almacenamiento de claves requiere una máquina virtual de la versión 8. Para obtener información sobre la versión de la máquina virtual, consulte [actualización de la versión de la máquina virtual en Hyper-V en Windows 10 o Windows Server 2016](./deploy/upgrade-virtual-machine-version-in-hyper-v-on-windows-or-windows-server.md).  
  
### <a name="host-resource-protection-new"></a>Protección de recursos de host \(nuevo\)

Esta característica ayuda a impedir que una máquina virtual use más de su recurso compartido de recursos del sistema al buscar niveles excesivos de actividad. Esto puede ayudar a evitar que una actividad excesiva de una máquina virtual reduzca el rendimiento del host o de otras máquinas virtuales. Cuando la supervisión detecta una máquina virtual con una actividad excesiva, la máquina virtual recibe menos recursos. Esta supervisión y cumplimiento está desactivado de forma predeterminada. Use Windows PowerShell para activarlo o desactivarlo. Para activarlo, ejecute este comando:  
  
```
Set-VMProcessor TestVM -EnableHostResourceProtection $true
```

Para obtener más información sobre este cmdlet, consulte [set-VMProcessor](https://docs.microsoft.com/powershell/module/hyper-v/set-vmprocessor).

### <a name="hot-add-and-remove-for-network-adapters-and-memory-new"></a>Adición y eliminación en caliente para los adaptadores de red y la memoria \(nuevo\)

Ahora puede Agregar o quitar un adaptador de red mientras la máquina virtual se está ejecutando, sin incurrir en tiempo de inactividad. Esto funciona para las máquinas virtuales de generación 2 que ejecutan sistemas operativos Windows o Linux.  
  
También puede ajustar la cantidad de memoria asignada a una máquina virtual mientras se está ejecutando, incluso si no ha habilitado Memoria dinámica. Esto funciona para las máquinas virtuales de generación 1 y generación 2, con Windows Server 2016 o Windows 10.  

### <a name="hyper-v-manager-improvements-updated"></a>Mejoras del administrador de Hyper-V \(actualizadas\) 
  
-   **Compatibilidad con credenciales alternativas** : ahora puede usar un conjunto diferente de credenciales en el administrador de Hyper-V cuando se conecte a otro host remoto de windows Server 2016 o Windows 10. También puede guardar estas credenciales para que sea más fácil volver a iniciar sesión.  
  
-   **Administrar versiones anteriores** : con el administrador de Hyper-v en windows Server 2019, windows Server 2016 y Windows 10, puede administrar equipos que ejecutan Hyper-v en windows Server 2012, Windows 8, windows Server 2012 R2 y Windows 8.1.  
  
-   **Protocolo de administración actualizado** : el administrador de Hyper-v ahora se comunica con los hosts remotos de Hyper-v mediante el protocolo WS-MAN, que permite la autenticación CredSSP, Kerberos o NTLM. Si usa CredSSP para conectarse a un host remoto de Hyper-V, puede realizar una migración en vivo sin habilitar la delegación restringida en Active Directory. La infraestructura basada en WS-MAN también facilita la habilitación de un host para la administración remota. WS-MAN se conecta a través del puerto 80, que está abierto de forma predeterminada.  
  
### <a name="integration-services-delivered-through-windows-update-updated"></a>Integration Services entregado a través de Windows Update \(actualizado\) 

Las actualizaciones de Integration Services para invitados de Windows se distribuyen a través de Windows Update. En el caso de los proveedores de servicios y los hosts de la nube privada, esto coloca el control de aplicar las actualizaciones a las manos de los inquilinos que poseen las máquinas virtuales. Los inquilinos ahora pueden actualizar sus máquinas virtuales Windows con todas las actualizaciones, incluidos los servicios de integración, con un único método. Para obtener más información acerca de Integration Services para invitados de Linux, consulte el [Virtual Machines de Linux y FreeBSD en Hyper-V](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md).  
  
> [!IMPORTANT]  
> El archivo de imagen vmguest. ISO ya no es necesario, por lo que no se incluye con Hyper-V en Windows Server 2016.  
  
### <a name="linux-secure-boot-new"></a>Arranque seguro de Linux \(nueva\) 

Los sistemas operativos Linux que se ejecutan en máquinas virtuales de generación 2 pueden arrancar ahora con la opción de arranque seguro habilitada. Ubuntu 14,04 y versiones posteriores, SUSE Linux Enterprise Server 12 y versiones posteriores, Red Hat Enterprise Linux 7,0 y versiones posteriores, y la versión 7,0 y posteriores están habilitadas para el arranque seguro en hosts que ejecutan Windows Server 2016. Antes de arrancar la máquina virtual por primera vez, debe configurar la máquina virtual para que use la entidad de certificación UEFI de Microsoft. Puede hacerlo desde el administrador de Hyper-V, Virtual Machine Manager o una sesión de Windows PowerShell con privilegios elevados. En Windows PowerShell, ejecute este comando:  
  
```  
Set-VMFirmware TestVM -SecureBootTemplate MicrosoftUEFICertificateAuthority  
```  
  
Para obtener más información sobre las máquinas virtuales Linux en Hyper-V, consulte el [Virtual Machines de Linux y FreeBSD en Hyper-v](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md). Para obtener más información sobre el cmdlet, consulte [set-VMFirmware](https://docs.microsoft.com/powershell/module/hyper-v/set-vmfirmware).

### <a name="more-memory-and-processors-for-generation-2-virtual-machines-and-hyper-v-hosts-updated"></a>Se \(actualizar la memoria y los procesadores de las máquinas virtuales de generación 2 y los hosts de Hyper-V\)

A partir de la versión 8, las máquinas virtuales de generación 2 pueden usar mucha más memoria y procesadores virtuales. Los hosts también se pueden configurar con mucha más memoria y procesadores virtuales de los admitidos anteriormente. Estos cambios admiten nuevos escenarios, como la ejecución de bases de datos de comercio electrónico grandes en memoria para el procesamiento de transacciones en línea (OLTP) y el almacenamiento de datos (DW). El blog de Windows Server publicó recientemente los resultados de rendimiento de una máquina virtual con 5,5 terabytes de memoria y 128 procesadores virtuales con una base de datos en memoria de 4 TB. El rendimiento era superior al 95% del rendimiento de un servidor físico. Para obtener más información, consulte [rendimiento de máquinas virtuales de gran escala de Hyper-V de Windows Server 2016 para el procesamiento de transacciones en memoria](https://blogs.technet.microsoft.com/windowsserver/2016/09/28/windows-server-2016-hyper-v-large-scale-vm-performance-for-in-memory-transaction-processing/). Para obtener más información acerca de las versiones de las máquinas virtuales, consulte [actualización de la versión de la máquina virtual en Hyper-V en Windows 10 o Windows Server 2016](./deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md). Para obtener la lista completa de las configuraciones máximas admitidas, consulte [planear la escalabilidad de Hyper-V en Windows Server 2016](./plan/plan-hyper-v-scalability-in-windows-server.md). 

### <a name="nested-virtualization-new"></a>Virtualización anidada \(nueva\)

Esta característica permite usar una máquina virtual como un host de Hyper-V y crear máquinas virtuales en ese host virtualizado. Esto puede ser especialmente útil para entornos de desarrollo y pruebas. Para usar la virtualización anidada, necesitará lo siguiente:  
  
-   Para ejecutar al menos Windows Server 2019, Windows Server 2016 o Windows 10 en el host de Hyper-V físico y en el host virtualizado.  
  
-   Un procesador con Intel VT-x (virtualización anidada solo está disponible para los procesadores Intel en este momento).  
  
Para obtener más información e instrucciones, consulte [ejecución de Hyper-V en una máquina virtual con virtualización anidada](https://docs.microsoft.com/virtualization/hyper-v-on-windows/user-guide/nested-virtualization).  
  
### <a name="networking-features-new"></a>Características de red \(nuevas\)

Las nuevas características de red incluyen:  
  
-   **Acceso directo a memoria remota (RDMA) y switch Embedded Teaming (Set)** . Puede configurar RDMA en los adaptadores de red enlazados a un conmutador virtual de Hyper-V, independientemente de si se usa también el conjunto. SET proporciona un conmutador virtual con algunas de las mismas funciones que la formación de equipos NIC. Para obtener más información, consulte [acceso directo a memoria remota (RDMA) y switch Embedded Teaming (Set)](https://docs.microsoft.com/windows-server/virtualization/hyper-v-virtual-switch/rdma-and-switch-embedded-teaming).  
  
-   **Varias colas de máquinas virtuales (VMMQ)** . Mejora el rendimiento de VMQ mediante la asignación de varias colas de hardware por máquina virtual.  La cola predeterminada se convierte en un conjunto de colas para una máquina virtual y el tráfico se propaga entre las colas.  
  
-   **Calidad de servicio (QoS) para redes definidas por software**. Administra la clase predeterminada de tráfico a través del conmutador virtual dentro del ancho de banda de clase predeterminado.  
  
Para obtener más información acerca de las nuevas características de red, consulte [novedades de las redes](../../networking/What-s-New-in-Networking.md).  
  
### <a name="production-checkpoints-new"></a>Puntos de control de producción \(nuevos\)

Los puntos de control de producción son imágenes en un momento dado de una máquina virtual. Estos proporcionan una manera de aplicar un punto de control que cumple con las directivas de soporte técnico cuando una máquina virtual ejecuta una carga de trabajo de producción. Los puntos de control de producción se basan en la tecnología de copia de seguridad dentro del invitado en lugar de en un estado guardado. En el caso de las máquinas virtuales Windows, se usa el servicio de instantáneas de volumen (VSS). En el caso de las máquinas virtuales Linux, los búferes del sistema de archivos se vacían para crear un punto de control coherente con el sistema de archivos. Si prefiere usar puntos de control basados en Estados guardados, en su lugar, elija puntos de control estándar. Para obtener más información, consulte [elegir entre los puntos de control estándar o de producción en Hyper-V](manage/Choose-between-standard-or-production-checkpoints-in-Hyper-V.md).  
  
> [!IMPORTANT]  
> Las nuevas máquinas virtuales usan puntos de control de producción como valor predeterminado.  
  
### <a name="rolling-hyper-v-cluster-upgrade-new"></a>Actualización de clústeres de Hyper-V \(nueva\)

Ahora puede Agregar un nodo que ejecute Windows Server 2019 o Windows Server 2016 a un clúster de Hyper-V con nodos que ejecutan Windows Server 2012 R2. Esto le permite actualizar el clúster sin tiempo de inactividad. El clúster se ejecuta en un nivel de característica de Windows Server 2012 R2 hasta que actualice todos los nodos del clúster y actualice el nivel funcional del clúster con el cmdlet de Windows PowerShell, [Update-ClusterFunctionalLevel](https://docs.microsoft.com/powershell/module/failoverclusters/Update-ClusterFunctionalLevel).  
  
> [!IMPORTANT]  
> Después de actualizar el nivel funcional del clúster, no puede volver a Windows Server 2012 R2.  
  
Para un clúster de Hyper-V con un nivel funcional de Windows Server 2012 R2 con nodos que ejecutan Windows Server 2012 R2, Windows Server 2019 y Windows Server 2016, tenga en cuenta lo siguiente:  
  
-   Administrar el clúster, Hyper-V y las máquinas virtuales desde un nodo que ejecuta Windows Server 2016 o Windows 10.  
  
-   Puede trasladar máquinas virtuales entre todos los nodos del clúster de Hyper-V.  
  
-   Para usar las nuevas características de Hyper-V, todos los nodos deben ejecutar Windows Server 2016 o y el nivel funcional del clúster debe estar actualizado.  
  
-   La versión de configuración de la máquina virtual para las máquinas virtuales existentes no está actualizada. Solo puede actualizar la versión de configuración después de actualizar el nivel funcional del clúster.  
  
-   Las máquinas virtuales que cree son compatibles con Windows Server 2012 R2, nivel de configuración de máquina virtual 5.  
  
Después de actualizar el nivel funcional del clúster:  
  
-   Puede habilitar las nuevas características de Hyper-V.  
  
-   Para que estén disponibles las nuevas características de máquina virtual, use el cmdlet Update-VmConfigurationVersion para actualizar manualmente el nivel de configuración de la máquina virtual. Para obtener instrucciones, consulte Actualización de la versión de la [máquina virtual](deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md).    
-   No se puede Agregar un nodo al clúster de Hyper-V que ejecuta Windows Server 2012 R2.  
  
> [!NOTE]  
> Hyper-V en Windows 10 no es compatible con los clústeres de conmutación por error.  
  
Para obtener más información e instrucciones, consulte la [actualización gradual del sistema operativo del clúster](https://technet.microsoft.com/library/dn850430.aspx).  

### <a name="shared-virtual-hard-disks-updated"></a>\(actualizados los discos duros virtuales compartidos\)
Ahora puede cambiar el tamaño de los discos duros virtuales compartidos (archivos. vhdx) que se usan para la agrupación en clústeres invitados, sin tiempo de inactividad. Los discos duros virtuales compartidos se pueden incrementar o reducir mientras la máquina virtual está en línea. Los clústeres invitados ahora también pueden proteger los discos duros virtuales compartidos mediante la réplica de Hyper-V para la recuperación ante desastres.

Habilite la replicación en la colección. La habilitación de la replicación en una colección **solo se expone a través de la interfaz WMI**. Consulte la documentación de [Msvm_CollectionReplicationService clase](https://msdn.microsoft.com/library/mt167787%28v=vs.85%29.aspx) para obtener más información. **No se puede administrar la replicación de una colección mediante el cmdlet de PowerShell o la interfaz de usuario.** Las máquinas virtuales deben estar en hosts que forman parte de un clúster de Hyper-V para tener acceso a las características específicas de una recopilación. Esto incluye los VHD compartidos de VHD compartidos en hosts independientes que no son compatibles con la réplica de Hyper-V.

Siga las instrucciones para VHD compartidos en [información general sobre el uso compartido de discos duros virtuales](https://technet.microsoft.com/library/dn281956.aspx)y asegúrese de que los VHD compartidos forman parte de un clúster invitado. 

Una colección con un VHD compartido, pero ningún clúster invitado asociado no puede crear puntos de referencia para la colección (independientemente de si el VHD compartido está incluido en la creación del punto de referencia o no). 

### <a name="virtual-machine-backupnew"></a>Copia de seguridad de máquina virtual\(nuevo\)

Si va a realizar una copia de seguridad de una sola máquina virtual (independientemente de si el host está agrupado o no), no debe usar un grupo de máquinas virtuales.  No debe utilizar una colección de instantáneas. Los grupos de máquinas virtuales y la recopilación de instantáneas están diseñados para usarse únicamente para realizar copias de seguridad de clústeres invitados que usan vhdx compartido. En su lugar, debe tomar una instantánea mediante el [proveedor WMI V2 de Hyper-V](https://msdn.microsoft.com/library/windows/desktop/hh850319(v=vs.85).aspx). Del mismo modo, no use el [proveedor WMI de clúster de conmutación por error](https://msdn.microsoft.com/library/windows/desktop/mt167750(v=vs.85).aspx).

### <a name="shielded-virtual-machines-new"></a>Máquinas virtuales blindadas \(nuevas\)

Las máquinas virtuales blindadas usan varias características para que sea más difícil para los administradores de Hyper-V y el malware en el host inspeccionar, manipular o robar datos del estado de una máquina virtual blindada. Los datos y el estado están cifrados, los administradores de Hyper-V no pueden ver los discos y la salida de vídeo, y las máquinas virtuales se pueden restringir para que se ejecuten solo en hosts conocidos y en buen estado, según lo determinado por un servidor de protección de host. Para más información, consulte [tejido protegido y máquinas virtuales blindadas](../../security/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms.md).
  
> [!NOTE]  
> Las máquinas virtuales blindadas son compatibles con la réplica de Hyper-V. Para replicar una máquina virtual blindada, el host en el que desea realizar la replicación debe estar autorizado para ejecutar esa máquina virtual blindada.  

### <a name="start-order-priority-for-clustered-virtual-machines-new"></a>Prioridad de orden de inicio de máquinas virtuales en clúster \(nueva\)

Esta característica le ofrece más control sobre las máquinas virtuales en clúster que se inician o se reinician en primer lugar. Esto facilita el inicio de máquinas virtuales que proporcionan servicios antes que las máquinas virtuales que usan esos servicios. Defina conjuntos, coloque las máquinas virtuales en conjuntos y especifique las dependencias. Use los cmdlets de Windows PowerShell para administrar los conjuntos, como [New-ClusterGroupSet](https://docs.microsoft.com/powershell/module/failoverclusters/new-clustergroupset), [Get-ClusterGroupSet](https://docs.microsoft.com/powershell/module/failoverclusters/get-clustergroupset)y [Add-ClusterGroupSetDependency](https://docs.microsoft.com/powershell/module/failoverclusters/add-clustergroupsetdependency).
.  
### <a name="storage-quality-of-service-qos-updated"></a>\(actualizada la calidad de servicio (QoS) de almacenamiento\)

Ahora puede crear directivas de Calidad de servicio de almacenamiento en un Servidor de archivos de escalabilidad horizontal y asignarlas a uno o varios discos virtuales en máquinas virtuales de Hyper-V. El rendimiento del almacenamiento se reajusta automáticamente para cumplir las directivas a medida que varía la carga de almacenamiento. Para obtener más información, consulte [calidad de servicio de almacenamiento](../../storage/storage-qos/storage-qos-overview.md).  
  
### <a name="virtual-machine-configuration-file-format-updated"></a>\(actualizó el formato del archivo de configuración de la máquina virtual\)

Los archivos de configuración de la máquina virtual usan un nuevo formato que facilita la lectura y escritura de datos de configuración. El formato también dificulta los daños en los datos si se produce un error de almacenamiento. Los archivos de datos de configuración de la máquina virtual usan una extensión de nombre de archivo. vmcx y los archivos de datos de estado en tiempo de ejecución usan una extensión de nombre de archivo. VMRS.  
  
> [!IMPORTANT]  
> La extensión de nombre de archivo. vmcx indica un archivo binario. No se admite la edición de archivos. vmcx o. VMRS.  
  
### <a name="virtual-machine-configuration-version-updated"></a>Versión de configuración de máquina virtual \(actualizada\)

La versión representa la compatibilidad de la configuración de la máquina virtual, el estado guardado y los archivos de instantáneas con la versión de Hyper-V. Las máquinas virtuales con la versión 5 son compatibles con Windows Server 2012 R2 y se pueden ejecutar en Windows Server 2012 R2 y Windows Server 2016. Las máquinas virtuales con versiones introducidas en Windows Server 2016 y Windows Server 2019 no se ejecutan en Hyper-V en Windows Server 2012 R2.   
  
Si mueve o importa una máquina virtual en un servidor que ejecuta Hyper-V en Windows Server 2016 o Windows Server 2019 desde Windows Server 2012 R2, la configuración de la máquina virtual no se actualiza automáticamente. Esto significa que puede devolver la máquina virtual a un servidor que ejecute Windows Server 2012 R2. Sin embargo, esto también significa que no puede usar las nuevas características de la máquina virtual hasta que actualice manualmente la versión de la configuración de la máquina virtual.  
  
Para obtener instrucciones sobre cómo comprobar y actualizar la versión, consulte Actualización de la versión de la [máquina virtual](deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md). En este artículo también se muestra la versión en la que se introdujeron algunas características.   
  
> [!IMPORTANT]  
> -   Después de actualizar la versión, no se puede migrar la máquina virtual a un servidor que ejecute Windows Server 2012 R2.  
> -   No se puede cambiar la configuración a una versión anterior.  
> -   El cmdlet [Update-VMVersion](https://docs.microsoft.com/powershell/module/hyper-v/update-vmversion) está bloqueado en un clúster de Hyper-V cuando el nivel funcional del clúster es Windows Server 2012 R2.  

### <a name="virtualization-based-security-for-generation-2-virtual-machines-new"></a>Seguridad basada en la virtualización para máquinas virtuales de generación 2 \(nuevo)

La seguridad basada en la virtualización impulsa características como Device Guard y Credential Guard, lo que ofrece una mayor protección del sistema operativo frente a las vulnerabilidades del malware. La seguridad basada en la virtualización está disponible en máquinas virtuales invitadas de generación 2 a partir de la versión 8. Para obtener información sobre la versión de la máquina virtual, consulte [actualización de la versión de la máquina virtual en Hyper-V en Windows 10 o Windows Server 2016](./deploy/upgrade-virtual-machine-version-in-hyper-v-on-windows-or-windows-server.md).

### <a name="windows-containers-new"></a>Contenedores de Windows \(nuevos\)

Los contenedores de Windows permiten que muchas aplicaciones aisladas se ejecuten en un equipo. Son rápidos de compilar y son muy escalables y portátiles. Hay dos tipos de tiempo de ejecución de contenedor disponibles, cada uno con un grado diferente de aislamiento de aplicaciones. Los contenedores de Windows Server usan el espacio de nombres y el aislamiento de procesos. Los contenedores de Hyper-V usan una máquina virtual ligera para cada contenedor.  
  
Las características principales incluyen:  
  
-   Compatibilidad con sitios web y aplicaciones que usan HTTPS  
  
-   Nano Server puede hospedar contenedores de Windows Server y Hyper-V  
  
-   Capacidad para administrar datos a través de carpetas compartidas de contenedor  
  
-   Capacidad de restringir los recursos de contenedor  
  
Para obtener más información, incluidas las guías de inicio rápido, consulte la [documentación sobre contenedores de Windows](https://docs.microsoft.com/virtualization/windowscontainers/index).  
  
### <a name="windows-powershell-direct-new"></a>Windows PowerShell Direct \(nuevo\)

Esto le ofrece una manera de ejecutar comandos de Windows PowerShell en una máquina virtual desde el host. Windows PowerShell Direct se ejecuta entre el host y la máquina virtual. Esto significa que no requiere requisitos de red ni de firewall, y funciona independientemente de la configuración de administración remota.  
  
Windows PowerShell Direct es una alternativa a las herramientas existentes que los administradores de Hyper-V usan para conectarse a una máquina virtual en un host de Hyper-V:  
  
-   Herramientas de administración remota como PowerShell o Escritorio remoto.  
  
-   Conexión de máquina virtual de Hyper-V (VMConnect)  
  
Esas herramientas funcionan bien, pero tienen ventajas e inconvenientes: VMConnect es confiable, pero puede ser difícil de automatizar. PowerShell remoto es eficaz, pero puede ser difícil de configurar y mantener. Estas ventajas pueden ser más importantes a medida que crece la implementación de Hyper-V. Windows PowerShell Direct aborda esto proporcionando una eficaz experiencia de scripting y automatización que es tan sencilla como usar VMConnect.
  
Para obtener información sobre los requisitos y las instrucciones, consulte [Administración de máquinas virtuales Windows con PowerShell Direct](manage/Manage-Windows-virtual-machines-with-PowerShell-Direct.md).  
