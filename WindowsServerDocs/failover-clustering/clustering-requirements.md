---
title: Requisitos de hardware de agrupación en clústeres de conmutación por error y opciones de almacenamiento
description: Requisitos de hardware y las opciones de almacenamiento para crear un clúster de conmutación por error.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 04/26/2018
ms.localizationpriority: medium
ms.openlocfilehash: e6eb6f2acd420ae657a5c1b698e9733751378552
ms.sourcegitcommit: ed27ddbe316d543b7865bc10590b238290a2a1ad
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/09/2019
ms.locfileid: "65476080"
---
# <a name="failover-clustering-hardware-requirements-and-storage-options"></a>Requisitos de hardware de agrupación en clústeres de conmutación por error y opciones de almacenamiento

Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Necesita el siguiente hardware para crear un clúster de conmutación por error: Para que sea compatible con Microsoft, todas las herramientas de hardware deben estar certificadas para la versión de Windows Server que utilices, y la solución de clúster de conmutación por error completa debe superar todas las pruebas del Asistente para validar una configuración. Para obtener más información sobre la validación de un clúster de conmutación por error, consulte [Validate Hardware for a Failover Cluster](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134244(v%3dws.11)>).

- **Servidores**: Se recomienda que use un conjunto de equipos parecidos que contengan los mismos componentes o componentes similares.
- **Adaptadores de red y cable (para la comunicación de red)** : si usas iSCSI, los adaptadores de red se deben dedicar a la comunicación de red o iSCSI, pero no a ambos.

    En la infraestructura de red que conecta los nodos de clúster, evite tener puntos de concentración de errores. Por ejemplo, puede conectar los nodos del clúster mediante varias redes distintas. Como alternativa, puede conectar los nodos del clúster con una red construida con adaptadores de red agrupados, conmutadores redundantes, enrutadores redundantes o hardware similar que elimine los puntos únicos de error.

    >[!NOTE]
    >Si conecta los nodos de clúster con una red única, la red pasará el requisito de redundancia al Asistente para validar una configuración. Sin embargo, el informe del asistente incluirá una advertencia acerca de que la red no debería tener puntos de concentración de errores.

- **Controladores de dispositivo o adaptadores adecuados para el almacenamiento**:

  - **SCSI conectado en serie o Canal de fibra**: si usas canal de fibra o SCSI conectado en serie en todos los servidores de clúster, todos los elementos de la pila de almacenamiento deben ser idénticos. Es obligatorio que el software multipath de E/S (MPIO) sea idéntico y que el software del módulo específico de dispositivo (DSM) sean idénticos. Se recomienda que los controladores de dispositivo de almacenamiento masivo, el bus host (HBA) del adaptador, controladores HBA y firmware de HBA, que se adjuntan al almacenamiento de clúster sean idénticos. Si utiliza unos HBA distintos, debería comprobar con el proveedor de almacenamiento que está siguiendo sus configuraciones compatibles o recomendadas.
  - **iSCSI**: si usas iSCSI, cada servidor en clúster debería tener uno o varios adaptadores de red o adaptadores de bus host dedicados al almacenamiento de clúster. La red que se use para iSCSI no se debe usar para la comunicación de red. En todos los servidores en clúster, los adaptadores de red que usa para conectar con el destino de almacenamiento de iSCSI deben ser idénticos y se recomienda utilizar Gigabit Ethernet o superior.
- **Almacenamiento**: Debe usar [espacios de almacenamiento directo](../storage/storage-spaces/storage-spaces-direct-overview.md) o el almacenamiento compartido que sea compatible con Windows Server 2012 R2 o Windows Server 2012. Puede usar almacenamiento compartido que está conectado, y también puede usar recursos compartidos de archivos SMB 3.0 como almacenamiento compartido para servidores que ejecutan Hyper-V que están configurados en un clúster de conmutación por error. Para obtener más información, consulte [Implementación de Hyper-V en SMB](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134187(v%3dws.11)>).

    En la mayoría de los casos, el almacenamiento adjunto debe contener varios discos independientes (números de unidad lógica o LUN) configurados en el nivel de hardware. En algunos clústeres, un disco funciona como el testigo de disco (que se describe al final de esta subsección). Otros discos contienen los archivos requeridos para los roles en clúster (anteriormente llamados aplicaciones o servicios en clúster). Los requisitos de almacenamiento incluyen lo siguiente:

  - Para usar el soporte de disco nativo incluido en el clúster de conmutación por error, usa discos básicos, no discos dinámicos.
  - Se recomienda formatear las particiones con NTFS. Si usas volúmenes compartidos de clúster (CSV), la partición para cada uno de ellos debe ser NTFS.

    >[!NOTE]
    >Si tienes un testigo de disco para la configuración de quórum, puedes formatear el disco con NTFS o con el Sistema de archivos resistente (ReFS).

  - Para el estilo de partición del disco, puede usar el registro de arranque maestro (MBR) o la tabla de particiones GUID (GPT).

    Un testigo de disco es un disco del almacenamiento de clúster designado para mantener una copia de la base de datos de configuración de clúster. Un clúster de conmutación por error solamente tiene un testigo de disco si se especifica como parte de la configuración de cuórum. Para obtener más información, consulte [descripción quórum en espacios de almacenamiento directo](../storage/storage-spaces/understand-quorum.md).

## <a name="hardware-requirements-for-hyper-v"></a>Requisitos de hardware para Hyper-V

Si estás creando un clúster de conmutación por error que incluye máquinas virtuales en clúster, los servidores de clúster deben admitir los requisitos de hardware del rol Hyper-V. Hyper-V requiere un procesador de 64 bits que incluya lo siguiente:

- Virtualización asistida por hardware. Está disponible en procesadores que incluyen una opción de virtualización, concretamente, procesadores con tecnología Intel Virtualization Technology (Intel VT) o AMD Virtualization (AMD-V).
- La Prevención de ejecución de datos (DEP) implementada por hardware debe estar disponible y habilitada. Concretamente, debes habilitar el bit XD de Intel (bit ejecutar deshabilitado) o el bit NX de AMD (bit no ejecutar).

Para obtener más información acerca de la función de Hyper-V, vea [Introducción a Hyper-V](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831531(v%3dws.11)>).

## <a name="deploying-storage-area-networks-with-failover-clusters"></a>Implementación de redes de área de almacenamiento con clústeres de conmutación por error

Al implementar una red de área de almacenamiento (SAN) con un clúster de conmutación por error, siga estas instrucciones:

- **Confirma la compatibilidad del almacenamiento**: confirma con los fabricantes y proveedores que el almacenamiento (incluidos los controladores, firmware y software utilizados para el mismo) es compatible con los clústeres de conmutación por error en la versión de Windows Server que estés ejecutando.
- **Aísla los dispositivos de almacenamiento, un clúster por dispositivo**: los servidores de clústeres diferentes no deben poder tener acceso a los mismos dispositivos de almacenamiento. En la mayoría de los casos, el LUN usado para un conjunto de servidores de clúster debe aislarse de los demás servidores mediante la creación de máscaras o zonas LUN.
- **Considera la posibilidad de usar software de E/S de múltiples rutas o adaptadores de red agrupados**: en tejidos de almacenamiento de alta disponibilidad, puede implementar clústeres de conmutación por error con varios adaptadores de bus host mediante el uso de software de E/S de múltiples rutas o la formación de equipos de adaptadores de red (también denominada equilibrio de carga y conmutación por error, o LBFO). Esto proporciona el mayor nivel de redundancia y disponibilidad. Para Windows Server 2012 R2 o Windows Server 2012, su solución de múltiples rutas debe estar basada en E/S de múltiples rutas de Microsoft (MPIO). El fabricante de hardware normalmente proporcionará un módulo específico del dispositivo (DSM) MPIO para el hardware, aunque Windows Server incluye uno o más DSM como parte del sistema operativo.

    Para obtener más información sobre LBFO, consulte [Introducción a la formación de equipos NIC](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nic-teaming) en la biblioteca técnica de Windows Server.

    >[!IMPORTANT]
    >Los adaptadores de bus host y el software de E/S de múltiples rutas pueden ser muy sensibles a la versión. Si estás implementando una solución de múltiples rutas para el clúster, colabora estrechamente con el proveedor de hardware para elegir los adaptadores, firmware y software correctos para la versión de Windows Server que estás utilizando.

- **Considera la posibilidad de usar espacios de almacenamiento**: Si tiene previsto implementar almacenamiento SCSI (SAS) en clúster que se configura con espacios de almacenamiento conectado en serie, vea [Deploy Clustered Storage Spaces](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj822937(v%3dws.11)>) para conocer los requisitos.

## <a name="more-information"></a>Más información

- [Clúster de conmutación por error](failover-clustering.md)
- [Espacios de almacenamiento](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831739(v%3dws.11)>)
- [Usar clústeres invitados para alta disponibilidad](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn440540(v%3dws.11)>)