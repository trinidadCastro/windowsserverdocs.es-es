---
ms.assetid: c9844427-27cf-4d76-b5bb-e06368b092f7
title: Clústeres de conmutación por error
ms.prod: windows-server-threshold
layout: LandingPage
ms.topic: landing-page
ms.manager: dongill
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 03/08/2019
ms.localizationpriority: high
ms.openlocfilehash: 445de065ff5b68b83481ee5bd83ebf18fdd180a7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848656"
---
# <a name="failover-clustering-in-windows-server"></a>Conmutación de clústeres por error en Windows Server

> Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server (canal semianual)

>[!TIP]
> ¿Buscas información sobre versiones anteriores de Windows Server? Echa un vistazo a nuestras otras [bibliotecas de Windows Server](/previous-versions/windows/) en docs.microsoft.com. También puedes [buscar en este sitio](https://docs.microsoft.com/search/index?search=Windows+Server&dataSource=previousVersions) para obtener información específica.

<hr />

Un clúster de conmutación por error es un grupo de equipos independientes que trabajan juntos para aumentar la disponibilidad y la escalabilidad de los roles en clúster (anteriormente denominados aplicaciones y servicios en clúster). Los servidores agrupados (denominados nodos) están conectados mediante cables físicos y mediante software. Si se produce un error en uno o más de los nodos del clúster, otro nodo comienza a dar servicio (proceso que se denomina conmutación por error). Además, los roles en clúster se supervisan proactivamente para comprobar que estén funcionando correctamente. Si no están funcionando, se reinician o se mueven a otro nodo.

Los clústeres de conmutación por error también proporcionan la funcionalidad Volúmenes compartidos de clúster (CSV), que ofrece un espacio de nombres distribuido y uniforme que los roles en clúster pueden usar para acceder al almacenamiento compartido de todos los nodos. Con la característica Clústeres de conmutación por error, los usuarios experimentan una cantidad mínima de interrupciones del servicio.

La Conmutación de clústeres por error tiene muchas aplicaciones prácticas, incluyendo:
* Almacenamiento de recursos compartidos de archivos disponible continuamente o altamente disponible para aplicaciones como Microsoft SQL Server y máquinas virtuales de Hyper-V.
* Roles en clúster de alta disponibilidad que se ejecutan en servidores físicos o en máquinas virtuales instaladas en servidores que ejecutan Hyper-V.

<hr />

<ul class="cardsF panelContent">
<li>
                         <div class="cardSize">
                                <div class="cardPadding">
                                    <div class="card">
                                        <div class="cardImageOuter">
                                            <div class="cardImage">
                                                <img src="../media/i-whats-new.svg" alt="" />
                                            </div>
                                        </div>
                                        <div class="cardText">
                                        <h2><a href="whats-new-in-failover-clustering.md">Novedades de agrupación en clústeres de conmutación por error</a></h2>
                                        </div>
                                    </div>
                                </div>
                             </div>
                          </a>
                        </li>
                     </ul>
<HR />

<ul class="cardsF panelContent">

<li>
                         <div class="cardSize">
                                <div class="cardPadding">
                                    <div class="card">
                                        <div class="cardImageOuter">
                                            <div class="cardImage">
                                                <img src="../media/i-cluster.svg" alt="" />
                                            </div>
                                        </div>
                                        <div class="cardText">
                                        <h3>Comprender</h3>
<HR />
                                        <p><a href="sofs-overview.md">Servidor de archivos de escalabilidad horizontal para datos de la aplicación</a></p>
<HR />
                                        <p><a href="../storage/storage-spaces/understand-quorum.md">Quórum de clúster y grupo</a></p>
<HR />
                                        <p><a href="fault-domains.md">Conocimiento de dominio de error</a></p>
<HR />
                                        <p><a href="smb-multichannel.md">Redes de clústeres de SMB multicanal y con varias NIC simplificadas</a></p>
<HR />
                                        <p><a href="vm-load-balancing-overview.md">Equilibrio de carga de máquina virtual</a></p>
<HR />
                                        <p><a href="../storage/storage-spaces/cluster-sets.md">Conjuntos de clústeres</a></p>
<HR />
                                        <p><a href="cluster-affinity.md">Afinidad de clúster</a></p>
                                        </div>
                                    </div>
                                </div>
                            </div>
                          </a>
                        </li>

<li>
                         <div class="cardSize">
                                <div class="cardPadding">
                                    <div class="card">
                                        <div class="cardImageOuter">
                                            <div class="cardImage">
                                                <img src="../media/i-cluster.svg" alt="" />
                                            </div>
                                        </div>
                                        <div class="cardText">
                                        <h3>Planificación</h3>
<HR />
                                        <p><a href="clustering-requirements.md">Requisitos de Hardware de clústeres de conmutación por error y opciones de almacenamiento</a></p>
<HR />
                                        <p><a href="failover-cluster-csvs.md">Clúster de uso compartido de volúmenes (CSV)</a></p>               
<HR />
                                        <p><a href="../storage/storage-spaces/storage-spaces-direct-in-vm.md">Usar clústeres invitados de máquina virtual con espacios de almacenamiento directo</a></p>
                                        </div>
                                    </div>
                                </div>
                            </div>
                          </a>
                        </li>
<li>
                         <div class="cardSize">
                                <div class="cardPadding">
                                    <div class="card">
                                        <div class="cardImageOuter">
                                            <div class="cardImage">
                                                <img src="../media/i-cluster.svg" alt="" />
                                            </div>
                                        </div>
                                        <div class="cardText">
                                        <h3>Implementación</a></h3> 
<HR />
                                        <p><a href="prestage-cluster-adds.md">Preconfiguración de objetos de equipo del clúster en servicios de dominio de Active Directory</a></p>
<HR />
                                        <p><a href="create-failover-cluster.md">Creación de un clúster de conmutación por error</a></p> 
<HR />
                                        <p><a href="deploy-two-node-clustered-file-server.md">Implementar un servidor de archivos de dos nodos</a></p> 
<HR />
                                        <p><a href="manage-cluster-quorum.md">Administrar el quórum y testigos</a></p> 
<HR />
                                        <p><a href="deploy-cloud-witness.md">Implementación de un testigo en la nube</a></p>
<HR />
                                        <p><a href="file-share-witness.md">Implementar un testigo del recurso compartido de archivos</a></p>
<HR />
                                        <p><a href="cluster-operating-system-rolling-upgrade.md">Actualizaciones graduales de sistema operativo del clúster</a></p> 
<HR />
                                        <p><a href="upgrade-option-same-hardware.md">Actualizar un clúster de conmutación por error en el mismo hardware</a></p>
<HR />
                                        <p><a href="https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn265970\(v%3dws.11\)">Implementar un clúster desasociado de Active Directory</a></p>
                                        </div>
                                    </div>
                                </div>
                            </div>
                          </a>
                        </li>
                     </ul>
<HR />
<ul class="cardsF panelContent">
<li>
                         <div class="cardSize">
                                <div class="cardPadding">
                                    <div class="card">
                                        <div class="cardImageOuter">
                                            <div class="cardImage">
                                                <img src="../media/i-cluster.svg" alt="" />
                                            </div>
                                        </div>
                                        <div class="cardText">
                                        <h3>Administrar</h3>
<HR />
                                        <p><a href="cluster-aware-updating.md">Actualización de clústeres</a></p> 
<HR />
                                        <p><a href="health-service-overview.md">Servicio de mantenimiento</a></p>
<HR />
                                        <p><a href="cluster-domain-migration.md">Migración del dominio del clúster</a></p>
<HR />
                                        <p><a href="troubleshooting-using-wer-reports.md">Solución de problemas con el informe de errores de Windows</a></p> 
                                        </div>
                                    </div>
                                </div>
                            </div>
                          </a>
                        </li>
<li>
                         <div class="cardSize">
                                <div class="cardPadding">
                                    <div class="card">
                                        <div class="cardImageOuter">
                                            <div class="cardImage">
                                                <img src="../media/i-cluster.svg" alt="" />
                                            </div>
                                        </div>
                                        <div class="cardText">
                                        <h3>Herramientas y configuración</a></h3>
<HR />
                                        <p><a href="https://docs.microsoft.com/powershell/module/failoverclusters/?view=win10-ps">Cmdlets de PowerShell de clústeres de conmutación por error</a></p> 
<HR />
                                        <p><a href="https://docs.microsoft.com/powershell/module/clusterawareupdating/?view=win10-ps">Tenga en cuenta los Cmdlets de PowerShell de actualización del clúster</a></p> 
                                        </div>
                                    </div>
                                </div>
                            </div>
                          </a>
                        </li>
<li>
                         <div class="cardSize">
                                <div class="cardPadding">
                                    <div class="card">
                                        <div class="cardImageOuter">
                                            <div class="cardImage">
                                                <img src="../media/i-cluster.svg" alt="" />
                                            </div>
                                        </div>
                                        <div class="cardText">
                                        <h3>Recursos de la comunidad</a></h3>
<HR />
                                        <p><a href="https://go.microsoft.com/fwlink/p/?LinkId=230641">Foro High Availability (Clustering)</a></p> 
<HR />
                                        <p><a href="http://blogs.msdn.com/b/clustering/">Blog del equipo de equilibrio de carga de agrupación en clústeres de conmutación por error y de red</a></p> 
                                        </div>
                                    </div>
                                </div>
                            </div>
                          </a>
                        </li>
</ul>
