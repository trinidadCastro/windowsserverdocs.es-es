---
title: Requisitos de hardware y opciones de almacenamiento de clústeres de conmutación por error
description: Requisitos de hardware y opciones de almacenamiento para crear un clúster de conmutación por error.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
manager: lizross
ms.technology: storage-failover-clustering
ms.date: 04/26/2018
ms.localizationpriority: medium
ms.openlocfilehash: dec242282b0d7a07b8f244a1044134bd8af03f6f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80827858"
---
# <a name="failover-clustering-hardware-requirements-and-storage-options"></a>Requisitos de hardware y opciones de almacenamiento de clústeres de conmutación por error

Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Necesita el siguiente hardware para crear un clúster de conmutación por error: Para que sea compatible con Microsoft, todas las herramientas de hardware deben estar certificadas para la versión de Windows Server que utilices, y la solución de clúster de conmutación por error completa debe superar todas las pruebas del Asistente para validar una configuración. Para obtener más información acerca de la validación de un clúster de conmutación por error, consulta [Validar hardware para un clúster de conmutación por error](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134244(v%3dws.11)>).

- **Servidores**: se recomienda utilizar un conjunto de equipos parecidos que contengan los mismos componentes o componentes similares.
- **Adaptadores de red y cable (para la comunicación de red)** : si utiliza iSCSI, cada adaptador de red debe estar dedicado a la comunicación de red o a iSCSI, no a ambos.

    En la infraestructura de red que conecta los nodos del clúster, evite los puntos de error únicos. Por ejemplo, puede conectar los nodos del clúster mediante varias redes distintas. También puede conectar los nodos del clúster con una red construida con adaptadores de red en equipo, conmutadores redundantes, enrutadores redundantes o hardware similar que elimine los puntos de error únicos.

    >[!NOTE]
    >Si conecta los nodos del clúster con una sola red, ésta pasará el requisito de redundancia en el Asistente para validar una configuración. Sin embargo, el informe del asistente incluirá una advertencia acerca de que la red no debe tener puntos de error únicos.

- **Controladores de dispositivo o adaptadores adecuados para el almacenamiento**:

  - **SCSI conectado en serie o Canal de fibra**: si usas canal de fibra o SCSI conectado en serie en todos los servidores de clúster, todos los elementos de la pila de almacenamiento deben ser idénticos. Es necesario que el software de e/s de múltiples rutas (MPIO) sea idéntico y que el software del módulo específico del dispositivo (DSM) sea idéntico. Se recomienda que los controladores de dispositivo de almacenamiento masivo, el adaptador de bus host (HBA), los controladores HBA y el firmware de HBA, que están conectados al almacenamiento de clúster sean idénticos. Si utiliza unos HBA distintos, debería comprobar con el proveedor de almacenamiento que está siguiendo sus configuraciones compatibles o recomendadas.
  - **iSCSI**: si está usando iSCSI, cada servidor en clúster debería tener uno o varios adaptadores de red o adaptadores de bus host dedicados al almacenamiento de clúster. La red que se use para iSCSI no se debe usar para la comunicación de red. En todos los servidores agrupados, los adaptadores de red que se usen para conectar al destino del almacenamiento iSCSI deben ser idénticos y se recomienda usar Gigabit Ethernet o superior.
- **Almacenamiento**: debe usar [espacios de almacenamiento directo](../storage/storage-spaces/storage-spaces-direct-overview.md) o el almacenamiento compartido que sea compatible con Windows Server 2012 R2 o Windows Server 2012. Puede usar el almacenamiento compartido adjunto y también puede usar recursos compartidos de archivos SMB 3,0 como almacenamiento compartido para servidores que ejecutan Hyper-V y que están configurados en un clúster de conmutación por error. Para obtener más información, consulte [Implementación de Hyper-V en SMB](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134187(v%3dws.11)>).

    En la mayoría de los casos, el almacenamiento adjunto debe contener varios discos independientes (números de unidad lógica o LUN) configurados en el nivel de hardware. En algunos clústeres, un disco funciona como el testigo de disco (que se describe al final de esta subsección). Otros discos contienen los archivos requeridos para los roles en clúster (anteriormente llamados aplicaciones o servicios en clúster). Los requisitos de almacenamiento incluyen lo siguiente:

  - Para usar el soporte de disco nativo incluido en el clúster de conmutación por error, usa discos básicos, no discos dinámicos.
  - Se recomienda formatear las particiones con NTFS. Si usas volúmenes compartidos de clúster (CSV), la partición para cada uno de ellos debe ser NTFS.

    >[!NOTE]
    >Si tienes un testigo de disco para la configuración de quórum, puedes formatear el disco con NTFS o con el Sistema de archivos resistente (ReFS).

  - Para el estilo de partición del disco, puede usar el registro de arranque maestro (MBR) o la tabla de particiones GUID (GPT).

    Un testigo de disco es un disco del almacenamiento en clúster designado para contener una copia de la base de datos de configuración del clúster. Un clúster de conmutación por error solamente tiene un testigo de disco si se especifica como parte de la configuración de cuórum. Para obtener más información, consulte [Descripción del cuórum en espacios de almacenamiento directo](../storage/storage-spaces/understand-quorum.md).

## <a name="hardware-requirements-for-hyper-v"></a>Requisitos de hardware para Hyper-V

Si estás creando un clúster de conmutación por error que incluye máquinas virtuales en clúster, los servidores de clúster deben admitir los requisitos de hardware del rol Hyper-V. Hyper-V requiere un procesador de 64 bits que incluya lo siguiente:

- Virtualización asistida por hardware. Está disponible en procesadores que incluyen una opción de virtualización, concretamente, procesadores con tecnología Intel Virtualization Technology (Intel VT) o AMD Virtualization (AMD-V).
- La Prevención de ejecución de datos (DEP) implementada por hardware debe estar disponible y habilitada. Concretamente, debe habilitar el bit XD de Intel (bit ejecutar deshabilitado) o el bit NX de AMD (bit no ejecutar).

Para obtener más información acerca de la función de Hyper-V, vea [Introducción a Hyper-V](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831531(v%3dws.11)>).

## <a name="deploying-storage-area-networks-with-failover-clusters"></a>Implementación de redes de área de almacenamiento con clústeres de conmutación por error

Al implementar una red de área de almacenamiento (SAN) con un clúster de conmutación por error, siga estas directrices:

- **Confirme la compatibilidad del almacenamiento**: confirma con los fabricantes y proveedores que el almacenamiento (incluidos los controladores, firmware y software utilizados para el mismo) es compatible con los clústeres de conmutación por error en la versión de Windows Server que estés ejecutando.
- **Aislar dispositivos de almacenamiento, un clúster por dispositivo**: los servidores de distintos clústeres no podrán tener acceso a los mismos dispositivos de almacenamiento. En la mayoría de los casos, el LUN usado para un conjunto de servidores de clúster debe aislarse de los demás servidores mediante la creación de máscaras o zonas LUN.
- **Considera la posibilidad de usar software de E/S de múltiples rutas o adaptadores de red agrupados**: en tejidos de almacenamiento de alta disponibilidad, puede implementar clústeres de conmutación por error con varios adaptadores de bus host mediante el uso de software de E/S de múltiples rutas o la formación de equipos de adaptadores de red (también denominada equilibrio de carga y conmutación por error, o LBFO). Esto proporciona el nivel máximo de redundancia y disponibilidad. En Windows Server 2012 R2 o Windows Server 2012, la solución de múltiples rutas debe estar basada en e/s de múltiples rutas (MPIO) de Microsoft. El fabricante de hardware normalmente proporcionará un módulo específico del dispositivo (DSM) MPIO para el hardware, aunque Windows Server incluye uno o más DSM como parte del sistema operativo.

    Para obtener más información sobre LBFO, consulte [Introducción a la formación de equipos NIC](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nic-teaming) en la biblioteca técnica de Windows Server.

    >[!IMPORTANT]
    >Los adaptadores de bus host y el software de E/S de varias rutas pueden ser muy dependientes de la versión. Si estás implementando una solución de múltiples rutas para el clúster, colabora estrechamente con el proveedor de hardware para elegir los adaptadores, firmware y software correctos para la versión de Windows Server que estás utilizando.

- **Considere la posibilidad de usar espacios de almacenamiento**: Si planea implementar el almacenamiento en clúster SCSI conectado en serie (SAS) que se configura con espacios de almacenamiento, consulte [implementación de espacios de almacenamiento en clúster](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj822937(v%3dws.11)>) para los requisitos.

## <a name="more-information"></a>Más información

- [Clúster de conmutación por error](failover-clustering.md)
- [Espacios de almacenamiento](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831739(v%3dws.11)>)
- [Uso de clústeres invitados para alta disponibilidad](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn440540(v%3dws.11)>)