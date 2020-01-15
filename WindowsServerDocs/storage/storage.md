---
title: Almacenamiento
description: ''
author: JasonGerend
manager: elizapo
layout: LandingPage
ms.prod: windows-server
ms.technology: storage
ms.assetid: 6b74bc7c-a58d-4915-af8e-2cc27f2c4726
ms.topic: landing-page
ms.author: jgerend
ms.localizationpriority: medium
ms.date: 03/08/2019
ms.openlocfilehash: 7e7fbd6ce3fcef6b0f8da88927d83f28d3fff0a8
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/14/2020
ms.locfileid: "75950227"
---
# <a name="storage"></a>Almacenamiento

>Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server (canal semianual)

>[!TIP]
> ¿Busca información sobre versiones anteriores de Windows Server? Eche un vistazo a nuestras otras [bibliotecas de Windows Server](/previous-versions/windows/) en docs.microsoft.com. También puede [buscar en este sitio](https://docs.microsoft.com/search/index?search=Windows+Server&dataSource=previousVersions) para obtener información específica.

<hr />
El almacenamiento en Windows Server proporciona funciones nuevas y mejoradas para los clientes de centros de datos definidos por software (SDDC) que se centran en las cargas de trabajo virtualizadas. Windows Server también proporciona una extensa compatibilidad para clientes empresariales que usan servidores de archivos con cargas de trabajo existentes.

<hr />
<ul class="cardsF panelContent">
<li>
 <a href="whats-new-in-storage.md">
                            <div class="cardSize">
                                <div class="cardPadding">
                                    <div class="card">
                                        <div class="cardImageOuter">
                                            <div class="cardImage">
                                                <img src="../media/i-whats-new.svg" alt="" />
                                            </div>
                                        </div>
                                        <div class="cardText">
                                            <h2>Novedades</h2>
                                            <p>Descubra las novedades de almacenamiento de Windows Server</p>
                                        </div>
                                    </div>
                                </div>
                            </div>
                          </a>
                        </li>
</ul>
<hr />
<ul class="cardsF panelContent">
<li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-store.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3>Almacenamiento definido por software para cargas de trabajo virtualizadas</h3>
<HR />
                        <p><h3><a href="storage-spaces/storage-spaces-direct-overview.md">Espacios de almacenamiento directo</a></h3> Almacenamiento local conectado directamente, incluidos los dispositivos SATA y NVME, para optimizar el uso del disco después de agregar nuevos discos físicos y para tiempos de reparación del disco virtual más rápidos. Consulta también <a href="storage-spaces/overview.md">Espacios de almacenamiento</a> para obtener información sobre SAS compartida y espacios de almacenamiento independiente.</p>
<HR />
                        <p><h3><a href="storage-replica/storage-replica-overview.md">Réplica de almacenamiento</a></h3> Replicación sincrónica independiente del almacenamiento y a nivel de bloque entre clústeres o servidores para la preparación y recuperación ante desastres, así como la ampliación de un clúster de conmutación por error entre sitios para lograr una alta disponibilidad. La replicación sincrónica permite el reflejo de datos en sitios físicos con volúmenes coherentes frente a bloqueos para asegurar que no se produce absolutamente ninguna pérdida de datos en el nivel de sistema de archivos.</p>
<HR />
                        <p><h3><a href="storage-qos/storage-qos-overview.md">Calidad de servicio (QoS) de almacenamiento</a></h3> Supervise y administre de forma centralizada el rendimiento del almacenamiento para máquinas virtuales con Hyper-V y los roles de Servidor de archivos de escalabilidad horizontal mejoran automáticamente la equidad de recursos de almacenamiento entre varias máquinas virtuales con el mismo clúster de servidores de archivos.</p>
<HR />
                        <p><h3><a href="data-deduplication/overview.md">Desduplicación de datos</a></h3> Optimiza el espacio libre en un volumen mediante el examen de los datos en el volumen para la duplicación. Una vez identificadas, las partes duplicadas del conjunto de datos del volumen se almacenan una vez, opcionalmente, se comprimen para un ahorro adicional. Desduplicación de datos optimiza las redundancias sin poner en peligro la integridad o fidelidad de los datos.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
<li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-store.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3>Servidores de archivos de uso general</h3>
<HR />
                        <p><h3><a href="storage-migration-service/overview.md">Servicio de migración de almacenamiento</a></h3>Migrar los servidores a una versión más reciente de Windows Server mediante una herramienta gráfica que informan sobre los datos en los servidores, transfiere los datos y la configuración a los servidores más recientes y, de forma opcional, mueve las identidades de los servidores antiguos a los nuevos servidores para que las aplicaciones y los usuarios no es necesario cambiar nada.</p>
<HR />
                        <p><h3><a href="work-folders/work-folders-overview.md">Carpetas de trabajo</a></h3> Almacene y acceda a los archivos de trabajo en equipos y dispositivos personales, a menudo denominados "traiga su propio dispositivo" (BYOD), además de los equipos corporativos. Los usuarios obtienen una ubicación adecuada para almacenar los archivos de trabajo y pueden tener acceso a ellos desde cualquier lugar. Las organizaciones mantienen el control sobre los datos corporativos almacenando los archivos en servidores de archivos administrados centralmente y, de manera opcional, especificando directivas de dispositivo de usuario como contraseñas de cifrado y de bloqueo de pantalla.</p>
<HR />
                        <p><h3><a href="folder-redirection/folder-redirection-rup-overview.md">Redirección de archivos y carpetas sin conexión</a></h3> Redirija la ruta de acceso de las carpetas locales (como la carpeta documentos) a una ubicación de red, mientras se almacena en caché el contenido localmente para aumentar la velocidad y la disponibilidad.</p>
<HR />
                        <p><h3><a href="folder-redirection/deploy-roaming-user-profiles.md">Perfiles de usuario móviles</a></h3> Redirigir un perfil de usuario a una ubicación de red.</p>
<HR />
                        <p><h3><a href="dfs-namespaces/dfs-overview.md">Espacios de nombres DFS</a></h3> Agrupe las carpetas compartidas que se encuentran en distintos servidores en uno o más espacios de nombres estructurados lógicamente. Los usuarios ven cada espacio de nombres como una sola carpeta compartida con una serie de subcarpetas. Sin embargo, la estructura subyacente del espacio de nombres puede estar constituida por varios recursos compartidos de archivos ubicados en distintos servidores y en varios sitios.</p>
<HR />
                        <p><h3><a href="dfs-replication/dfsr-overview.md">Replicación DFS</a></h3> Replique las carpetas (incluidas aquellas a las que hace referencia la ruta de acceso de un espacio de nombres DFS) en varios servidores y sitios. Replicación DFS usa un algoritmo de compresión denominado compresión diferencial remota (RDC). RDC detecta los cambios en los datos de un archivo y permite que Replicación DFS replique solo los bloques de archivo modificados en lugar del archivo completo.</p>
<HR />
                        <p><h3><a href="fsrm/fsrm-overview.md">Administrador de recursos del servidor de archivos</a></h3> Administrar y clasificar los datos almacenados en los servidores de archivos.<p>
<HR />
                        <p><h3><a href="iscsi/iscsi-target-server.md">Servidor de destino iSCSI</a></h3> Proporciona almacenamiento en bloque a otros servidores y aplicaciones de la red mediante el estándar Internet SCSI (iSCSI).</p>
<HR />
                       <p><h3><a href="iscsi/iscsi-boot-overview.md">Servidor de destino iSCSI</a></h3> Puede arrancar cientos de equipos desde una sola imagen de sistema operativo que se almacena en una ubicación centralizada. Esto mejora la eficacia, la facilidad de uso, la disponibilidad y la seguridad.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
<li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-store.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3>Sistemas de archivos, protocolos, etc.</h3>
<HR />
                        <p><h3><a href="refs/refs-overview.md">ReFS</a></h3> Un sistema de archivos resistente que maximiza la disponibilidad de los datos, se escala de forma eficiente a conjuntos de datos de gran tamaño en diversas cargas de trabajo y proporciona integridad de los datos por medio de la resistencia a los daños (independientemente de los errores de software o hardware).<p>
<HR />
                        <p><h3><a href="file-server/file-server-smb-overview.md">Protocolo Bloque de mensajes del servidor (SMB)</a></h3> Protocolo de uso compartido de archivos de red que permite a las aplicaciones de un equipo leer y escribir en archivos y solicitar servicios de programas de servidor en una red de equipos. El protocolo SMB puede usarse sobre el protocolo TCP/IP u otros protocolos de red. Con el uso de un protocolo SMB, una aplicación (o el usuario de una aplicación) puede acceder a los archivos u otros recursos de un servidor remoto. Esto permite que las aplicaciones puedan leer, crear y actualizar archivos en un servidor remoto. También puedes comunicarse con cualquier programa del servidor que esté configurado para recibir la solicitud de un cliente SMB.<p>
<HR />
                        <p><h3><a href="storage-spaces/Storage-class-memory-health.md">Memoria de clase de almacenamiento</a></h3> Proporciona un rendimiento similar a la memoria del equipo (muy rápido), pero con la persistencia de datos de las unidades de almacenamiento normales. Windows trata la memoria de clase de almacenamiento de manera similar a las unidades normales (simplemente, más rápido), pero hay algunas diferencias en la manera en que se administra el estado del dispositivo.<p>
<HR />
                        <p><h3><a href="https://technet.microsoft.com/library/cc766295(v=ws.10).aspx">Cifrado de unidad BitLocker</a></h3> Almacena los datos en los volúmenes en un formato cifrado, incluso si el equipo se altera o cuando el sistema operativo no se está ejecutando. Esto ayuda a proteger de ataques sin conexión, que son aquellos que se realizan deshabilitando o evitando el sistema operativo instalado, o bien, quitando físicamente el disco duro para atacar los datos por separado.<p>
<HR />
                        <p><h3><a href="https://technet.microsoft.com/library/dn466522(v=ws.11).aspx">NTFS</a></h3> El sistema de archivos principal para las versiones recientes de Windows y Windows Server: proporciona un conjunto completo de características, como descriptores de seguridad, cifrado, cuotas de disco y metadatos enriquecidos, y se puede usar con volúmenes compartidos de clúster (CSV) para proporcionar de forma continua volúmenes disponibles a los que se puede tener acceso simultáneamente desde varios nodos de un clúster de conmutación por error.<p>
<HR />
                        <p><h3><a href="https://technet.microsoft.com/library/jj592688(v=ws.11).aspx">Network File System (NFS)</a></h3> Proporciona una solución de uso compartido de archivos para empresas que tienen entornos heterogéneos que se componen de equipos Windows y que no son de Windows.<p>
                    </div>
                </div>
            </div>
        </div>
    </li>
</ul>

---


## <a name="in-azure"></a>En Azure

* [Azure Storage](https://azure.microsoft.com/documentation/services/storage/)
* [StorSimple de Azure](https://www.microsoft.com/cloud-platform/azure-storsimple)
