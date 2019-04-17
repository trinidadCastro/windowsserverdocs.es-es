---
title: Requisitos de hardware de agrupación en clústeres de conmutación por error y opciones de almacenamiento
description: Requisitos de hardware y las opciones de almacenamiento para la creación de un clúster de conmutación por error.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 04/26/2018
ms.localizationpriority: medium
ms.openlocfilehash: 4706372b06d0554196b692c3ddcda145dee5bae5
ms.sourcegitcommit: e0479b0114eac7f232e8b1e45eeede96ccd72b26
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/22/2018
ms.locfileid: "2082675"
---
# <a name="failover-clustering-hardware-requirements-and-storage-options"></a>Requisitos de hardware de agrupación en clústeres de conmutación por error y opciones de almacenamiento

Se aplica a: Windows Server 2012 R2, Windows Server 2012, Windows Server 2016

Se necesita el siguiente hardware para crear un clúster de conmutación por error. Para ser compatible con Microsoft, deberá estar certificado todo el hardware para la versión de Windows Server que se ejecuta, y la solución de clúster de conmutación por error completa debe pasar todas las pruebas de la sección validar un asistente de configuración. Para obtener más información sobre la validación de un clúster de conmutación por error, vea [Validar el Hardware para un clúster de conmutación por error](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134244(v%3dws.11)>).

- **Servidores**: se recomienda usar un conjunto de equipos que contengan los mismos componentes o similares.
- **Adaptadores de red y cable (para la comunicación de red)**: si usa iSCSI, cada adaptador de red debe estar dedicado a la comunicación de red o iSCSI, no ambos.

    En la infraestructura de red que conecta los nodos del clúster, evitar tener puntos únicos de error. Por ejemplo, puede conectar los nodos del clúster por varias redes distintas. Como alternativa, puede conectar los nodos del clúster con una red que se construye con adaptadores de red combinados, conmutadores redundantes, enrutadores redundantes o hardware similar que quita puntos únicos de error.

    >[!NOTE]
    >Si se conectan los nodos del clúster con una sola red, la red pasará el requisito de redundancia en la sección validar un asistente de configuración. Sin embargo, el informe del asistente incluirá una advertencia de que la red no debe tener puntos únicos de error.

- **Controladores de dispositivo o adaptadores apropiados para el almacenamiento de información**:

  - **Serial Attached SCSI o canal de fibra**: si usa Serial Attached SCSI o canal de fibra, en todos los servidores agrupados, todos los elementos de la pila de almacenamiento deben ser idénticos. Es necesario que el software de E/S (MPIO) múltiples rutas ser idéntico y que el software de módulo específico de dispositivo (DSM) ser idénticos. Se recomienda que los controladores de dispositivo de almacenamiento masivo: el host bus adapter (HBA), los controladores HBA y firmware de HBA: que están conectados al almacenamiento de clúster ser idéntica. Si utiliza distinto HBA, debe comprobar con el proveedor de almacenamiento que siguen sus configuraciones recomendadas o compatibles.
  - **iSCSI**: si usa iSCSI, cada servidor agrupado debe tener uno o varios adaptadores de red o HBA que está dedicado al almacenamiento de clúster. La red que se use para iSCSI no debe usarse para la comunicación de red. En todos los servidores agrupados, los adaptadores de red que se use para conectarse al destino del almacenamiento iSCSI deben ser idénticos, y se recomienda que utilice Gigabit Ethernet o superior.
- **Almacenamiento**: debe usar [Almacenamiento espacios directa](../storage/storage-spaces/storage-spaces-direct-overview.md) o almacenamiento de información que es compatible con Windows Server 2012 R2 o Windows Server 2012 compartido. Puede usar almacenamiento compartido que se adjunta, y también puede usar recursos compartidos de archivos de SMB 3.0 como almacenamiento compartido para los servidores que ejecutan Hyper-V que están configurados en un clúster de conmutación por error. Para obtener más información, vea [Implementación de Hyper-V a través de SMB](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134187(v%3dws.11)>).

    En la mayoría de los casos, almacenamiento conectado debe contener varios, separe los discos (números de unidad lógica o LUN) a los que se configuran en el nivel de hardware. Algunos clústeres, de un disco funciona como el testigo de disco (que se describen al final de esta subsección). Otros discos contienen los archivos necesarios para las funciones de clúster (anteriormente denominadas servicios en clúster o aplicaciones). Requisitos de almacenamiento son los siguientes:

  - Para usar la compatibilidad nativa de disco incluida en el clúster de conmutación por error, use discos básicos, los discos no dinámicos.
  - Se recomienda que aplica formato a las particiones con NTFS. Si usa volúmenes compartidos de clúster (CSV), la partición para cada uno de ellos debe ser NTFS.

    >[!NOTE]
    >Si tiene un testigo de disco para la configuración de quórum, puede formatear el disco con NTFS o sistema de archivos resistente (ReFS).

  - Para el estilo de partición del disco, puede usar el registro de inicio maestro (MBR) o de tabla de particiones GUID (GPT).

    Un testigo de disco es un disco en el almacenamiento de clúster que ha designado para albergar una copia de la base de datos de configuración del clúster. Un clúster de conmutación por error tiene un testigo de disco sólo si se especifica como parte de la configuración de quórum. Para obtener más información, vea [Descripción de quórum en espacios de almacenamiento directo](../storage/storage-spaces/understand-quorum.md).

## <a name="hardware-requirements-for-hyper-v"></a>Requisitos de hardware para Hyper-V

Si va a crear un clúster de conmutación por error que incluye máquinas virtuales en clúster, los servidores de clúster deben admitir los requisitos de hardware para el rol de Hyper-V. Hyper-V requiere un procesador de 64 bits que incluye lo siguiente:

- Virtualización asistida por hardware. Esta opción está disponible en procesadores que incluyen una opción de virtualización — específicamente procesadores con tecnología de virtualización de Intel (Intel VT) o la tecnología de virtualización AMD (AMD-V).
- Prevención de ejecución de datos (DEP) forzada por hardware debe estar disponible y habilitada. En concreto, debe habilitar XD de Intel de bit (execute disable bit) o NX de AMD (bit no ejecutar).

Para obtener más información acerca de la función de Hyper-V, vea [Introducción a Hyper-V](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831531(v%3dws.11)>).

## <a name="deploying-storage-area-networks-with-failover-clusters"></a>Implementación de redes de área de almacenamiento de información con los clústeres de conmutación por error

Al implementar una red de área de almacenamiento (SAN) con un clúster de conmutación por error, siga estas instrucciones:

- **Confirme la compatibilidad del almacenamiento de información**: confirmar con fabricantes y proveedores que el almacenamiento, incluidos los controladores, el firmware y el software usado para el almacenamiento, son compatibles con clústeres de conmutación por error en la versión de Windows Server que esté ejecutando.
- **Aislar los dispositivos de almacenamiento, un clúster por dispositivo**: servidores de diferentes clústeres no deben poder tener acceso a los mismos dispositivos de almacenamiento. En la mayoría de los casos, un LUN utilizado para un conjunto de servidores de clúster debería estar aislado de todos los demás servidores a través de LUN masking o zoning.
- **Considere el uso de software de E/S de múltiples rutas o colaborado adaptadores de red**: en un fabric de almacenamiento altamente disponible, puede implementar los clústeres de conmutación por error con varios adaptadores de bus host mediante software de E/S de múltiples rutas o adaptadores (también denominado carga de red Equilibrio y conmutación por error o LBFO). Esto proporciona el mayor nivel de redundancia y disponibilidad. Para Windows Server 2012 R2 o Windows Server 2012, la solución de varias rutas debe estar basada en E/S de múltiples rutas de Microsoft (MPIO). Su proveedor de hardware normalmente proporcionará un módulo de específica de cada dispositivo (DSM) MPIO para el hardware, aunque Windows Server incluye uno o varios DSM como parte del sistema operativo.

    Para obtener más información sobre LBFO, consulte [Introducción a la formación de equipos NIC](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nic-teaming) en la biblioteca técnica de Windows Server.

    >[!IMPORTANT]
    >Adaptadores de bus host y el software de E/S de múltiples rutas pueden ser muy dependientes de la versión. Si va a implementar una solución de varias rutas para el clúster, trabajan estrechamente con su proveedor de hardware para elegir los adaptadores correctos, firmware y software para la versión de Windows Server que esté ejecutando.

- **Considere el uso de espacios de almacenamiento**: si planea implementar almacenamiento conectado en serie SCSI (SAS) agrupado que está configurado con espacios de almacenamiento, vea [Implementar agrupado espacios de almacenamiento](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj822937(v%3dws.11)>) para los requisitos.

## <a name="more-information"></a>Más información

- [Clúster de conmutación por error](failover-clustering.md)
- [Espacios de almacenamiento](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831739(v%3dws.11)>)
- [Usar clústeres invitados para alta disponibilidad](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn440540(v%3dws.11)>)