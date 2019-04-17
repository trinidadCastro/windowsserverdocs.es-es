---
title: Almacenamiento
description: ''
author: JasonGerend
manager: elizapo
layout: LandingPage
ms.prod: windows-server-threshold
ms.technology: storage
ms.assetid: 6b74bc7c-a58d-4915-af8e-2cc27f2c4726
ms.topic: landing-page
ms.author: jgerend
ms.localizationpriority: medium
ms.date: 03/08/2019
ms.openlocfilehash: d83d51ebf56d38f93c176d403ea5c6c14f625ee2
ms.sourcegitcommit: 419bf9d6d18e7a32cba2cbcd98587b81a79adc51
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/11/2019
ms.locfileid: "9152182"
---
# Almacenamiento

>Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server (canal semianual)

>[!TIP]
> ¿Buscas información sobre versiones anteriores de Windows Server? Echa un vistazo a nuestras otras [bibliotecas de Windows Server](/previous-versions/windows/) en docs.microsoft.com. También puedes [buscar en este sitio](https://docs.microsoft.com/search/index?search=Windows+Server&dataSource=previousVersions) para obtener información específica.

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
                                            <p>Descubrir las novedades de almacenamiento de Windows Server</p>
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
                        <p><h3><a href="storage-spaces/storage-spaces-direct-overview.md">Espacios de almacenamiento directo</a></h3> Conectado directamente el almacenamiento local, incluidos los dispositivos SATA y NVME, para optimizar el uso del disco después de agregar nuevos discos físicos y tiempos de reparación de disco virtual con mayor rapidez. Consulta también <a href="storage-spaces/overview.md">Espacios de almacenamiento</a> para obtener información sobre SAS compartida y espacios de almacenamiento independiente.</p>
<HR />
                        <p><h3><a href="storage-replica/storage-replica-overview.md">Réplica de almacenamiento</a></h3> Independiente del almacenamiento y a nivel de bloque replicación sincrónica, entre servidores o clústeres para la preparación ante desastres y recuperación, así como la extensión de un clúster de conmutación por error para alta disponibilidad entre sitios. La replicación sincrónica permite el reflejo de datos en sitios físicos con volúmenes coherentes frente a bloqueos para asegurar que no se produce absolutamente ninguna pérdida de datos en el nivel de sistema de archivos.</p>
<HR />
                        <p><h3><a href="storage-qos/storage-qos-overview.md">Calidad de servicio (QoS) del almacenamiento</a></h3> Supervisar y administrar el rendimiento del almacenamiento para máquinas virtuales con Hyper-V y los roles de servidor de archivos de escalabilidad horizontal automáticamente centralmente improveing equidad de recursos de almacenamiento entre varias máquinas virtuales que usan el mismo clúster de servidor de archivos.</p>
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
                        <p><h3><a href="storage-migration-service/overview.md">Servicio de migración de almacenamiento</a></h3>Migrar servidores a una versión más reciente de Windows Server mediante una herramienta gráfica que inventaría los datos en los servidores, transfiere los datos y la configuración a servidores más recientes y, a continuación, opcionalmente, pasa las identidades de los servidores antiguos a los nuevos servidores por lo tanto, que las aplicaciones y los usuarios no tienes que cambiar nada.</p>
<HR />
                        <p><h3><a href="work-folders/work-folders-overview.md">Carpetas de trabajo</a></h3> Almacenar y acceder a archivos de trabajo en equipos y dispositivos, suele denominar llevar tu propio dispositivo (BYOD), además de equipos corporativos. Los usuarios obtienen una ubicación adecuada para almacenar los archivos de trabajo y pueden tener acceso a ellos desde cualquier lugar. Las organizaciones mantienen el control sobre los datos corporativos almacenando los archivos en servidores de archivos administrados centralmente y, de manera opcional, especificando directivas de dispositivo de usuario como contraseñas de cifrado y de bloqueo de pantalla.</p>
<HR />
                        <p><h3><a href="folder-redirection/folder-redirection-rup-overview.md">Archivos sin conexión y redirección de carpetas</a></h3> Redireccionar la ruta de acceso de las carpetas locales (por ejemplo, la carpeta documentos) a una ubicación de red, mientras se almacenamiento en caché local el contenido para lograr una mayor velocidad y disponibilidad.</p>
<HR />
                        <p><h3><a href="folder-redirection/deploy-roaming-user-profiles.md">Perfiles de usuario móvil</a></h3> Redireccionar un perfil de usuario a una ubicación de red.</p>
<HR />
                        <p><h3><a href="dfs-namespaces/dfs-overview.md">Espacios de nombres DFS</a></h3> Agrupar las carpetas compartidas ubicadas en distintos servidores en uno o varios espacios de nombres estructurados lógicamente. Los usuarios ven cada espacio de nombres como una sola carpeta compartida con una serie de subcarpetas. Sin embargo, la estructura subyacente del espacio de nombres puede estar constituida por varios recursos compartidos de archivos ubicados en distintos servidores y en varios sitios.</p>
<HR />
                        <p><h3><a href="dfs-replication/dfsr-overview.md">Replicación de DFS</a></h3> Replicar carpetas (incluidos los que hace referencia una ruta de acceso del espacio de nombres DFS) en varios servidores y sitios. Replicación DFS usa un algoritmo de compresión denominado compresión diferencial remota (RDC). RDC detecta los cambios en los datos de un archivo y permite que Replicación DFS replique solo los bloques de archivo modificados en lugar del archivo completo.</p>
<HR />
                        <p><h3><a href="fsrm/fsrm-overview.md">Administrador de recursos del servidor de archivos</a></h3> Administrar y clasificar datos almacenados en los servidores de archivos.<p>
<HR />
                        <p><h3><a href="iscsi/iscsi-target-server.md">Servidor de destino iSCSI</a></h3> Proporciona el almacenamiento en bloque a otros servidores y aplicaciones de la red mediante el estándar Internet SCSI (iSCSI).</p>
<HR />
                       <p><h3><a href="iscsi/iscsi-boot-overview.md">Servidor de destino iSCSI</a></h3> Puede arrancar cientos de equipos desde una imagen del sistema operativo que se almacena en una ubicación centralizada. Esto mejora la eficacia, la facilidad de uso, la disponibilidad y la seguridad.</p>
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
                        <p><h3><a href="refs/refs-overview.md">ReFS</a></h3> Un sistema de archivos resistente que maximiza la disponibilidad de los datos, escala de manera eficiente a conjuntos de datos de gran tamaño en diversas cargas de trabajo y garantiza la integridad de los datos por medio de resistencia a los daños (independientemente de los errores de software o hardware).<p>
<HR />
                        <p><h3><a href="file-server/file-server-smb-overview.md">Protocolo de Server Message Block (SMB)</a></h3> Un archivo de red protocolo que permite a las aplicaciones de un equipo puedan leer y escribir archivos y solicitar servicios desde los programas de servidor en una red de equipos de uso compartido. El protocolo SMB puede usarse sobre el protocolo TCP/IP u otros protocolos de red. Con el uso de un protocolo SMB, una aplicación (o el usuario de una aplicación) puede acceder a los archivos u otros recursos de un servidor remoto. Esto permite que las aplicaciones puedan leer, crear y actualizar archivos en un servidor remoto. También puede comunicarse con cualquier programa del servidor que esté configurado para recibir la solicitud de un cliente SMB.<p>
<HR />
                        <p><h3><a href="storage-spaces/Storage-class-memory-health.md">Memoria de clase de almacenamiento</a></h3> Proporciona un rendimiento similar a la memoria del equipo (muy rápida), pero con la persistencia de datos de las unidades de almacenamiento normales. Windows trata la memoria de clase de almacenamiento de manera similar a las unidades normales (simplemente, más rápido), pero hay algunas diferencias en la manera en que se administra el estado del dispositivo.<p>
<HR />
                        <p><h3><a href="https://technet.microsoft.com/library/cc766295(v=ws.10).aspx">Cifrado de unidad BitLocker</a></h3> Almacena los datos en volúmenes en un formato cifrado, incluso si el equipo ha sido manipulado o cuando no se está ejecutando el sistema operativo. Esto ayuda a proteger de ataques sin conexión, que son aquellos que se realizan deshabilitando o evitando el sistema operativo instalado, o bien, quitando físicamente el disco duro para atacar los datos por separado.<p>
<HR />
                        <p><h3><a href="https://technet.microsoft.com/library/dn466522(v=ws.11).aspx">NTFS</a></h3> El sistema de archivos principal para las versiones recientes de Windows y Windows Server, proporciona un conjunto completo de características que incluyen descriptores de seguridad, cifrado, cuotas de disco y metadatos enriquecidos y puede usarse con volúmenes compartidos de clúster (CSV) para proporcionar continuamente volúmenes disponibles que pueden tener acceso simultáneamente desde varios nodos de un clúster de conmutación por error.<p>
<HR />
                        <p><h3><a href="https://technet.microsoft.com/library/jj592688(v=ws.11).aspx">Network File System (NFS)</a></h3> Proporciona una solución para las empresas que tienen entornos heterogéneos que constan de equipos Windows y equipos que no son de Windows de uso compartido de archivos.<p>
                    </div>
                </div>
            </div>
        </div>
    </li>
</ul>

---


## En Azure

* [Azure Storage](https://azure.microsoft.com/documentation/services/storage/)
* [Azure StorSimple](https://www.microsoft.com/en-us/cloud-platform/azure-storsimple)
