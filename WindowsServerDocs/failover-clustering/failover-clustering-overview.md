---
ms.assetid: c9844427-27cf-4d76-b5bb-e06368b092f7
title: Conmutación de clústeres por error
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
ms.sourcegitcommit: b0fece76b871da3fa9d6a996798a5008756f486b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2019
ms.locfileid: "9178606"
---
# Conmutación de clústeres por error en WindowsServer

> Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server (canal semianual)

>[!TIP]
> ¿Buscas información sobre versiones anteriores de Windows Server? Echa un vistazo a nuestras otras [bibliotecas de Windows Server](/previous-versions/windows/) en docs.microsoft.com. También puedes [buscar en este sitio](https://docs.microsoft.com/search/index?search=Windows+Server&dataSource=previousVersions) para obtener información específica.

<hr />

Un clúster de conmutación por error es un grupo de ordenadores independientes que trabajan juntos para aumentar la disponibilidad y la escalabilidad de los roles en clústeres (anteriormente denominados aplicaciones y servicios en clústeres). Los servidores en clústeres (denominados nodos) están conectados mediante cables físicos y mediante software. Si se produce un error en uno o más de los nodos de clústeres, otros nodos comienza a prestar servicio (proceso que se denomina conmutación por error). Además, los roles en clúster se supervisan proactivamente para comprobar que estén funcionando correctamente. Si no están funcionando, se reinician o se mueven a otro nodo.

Los clústeres de conmutación por error también proporcionan la funcionalidad Volúmenes compartidos de clústeres (CSV), que ofrece un espacio de nombres distribuido y uniforme que los roles en clúster pueden usar para acceder al almacenamiento compartido de todos los nodos. Con la función Conmutación de clústeres por error, los usuarios experimentan una cantidad mínima de interrupciones del servicio.

La Conmutación de clústeres por error tiene muchas aplicaciones prácticas, incluyendo:
* Almacenamiento de recursos compartidos de archivos disponibles continuamente o altamente disponibles para aplicaciones como Microsoft SQL Server y máquinas virtuales de Hyper-V.
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
                                        <h2><a href="whats-new-in-failover-clustering.md">Novedades de los clústeres de conmutación por error</a></h2>
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
                                        <p><a href="sofs-overview.md">Servidor de archivos de escalabilidad horizontal para datos de aplicaciones</a></p>
<HR />
                                        <p><a href="../storage/storage-spaces/understand-quorum.md">Cuórum de clúster y de grupo</a></p>
<HR />
                                        <p><a href="fault-domains.md">Reconocimiento de dominio de error</a></p>
<HR />
                                        <p><a href="smb-multichannel.md">SMB multicanal simplificada y redes de clústeres de varias NIC</a></p>
<HR />
                                        <p><a href="vm-load-balancing-overview.md">Equilibrio de carga de VM</a></p>
<HR />
                                        <p><a href="../storage/storage-spaces/cluster-sets.md">Conjuntos de clústeres</a></p>
<HR />
                                        <p><a href="cluster-affinity.md">Afinidad de clústeres</a></p>
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
                                        <p><a href="clustering-requirements.md">Requisitos de hardware de conmutación de clústeres por error y opciones de almacenamiento</a></p>
<HR />
                                        <p><a href="failover-cluster-csvs.md">Usar volúmenes compartidos de clúster (CSV)</a></p>               
<HR />
                                        <p><a href="../storage/storage-spaces/storage-spaces-direct-in-vm.md">Usar clústeres de máquina virtual de invitado con espacios de almacenamiento directo</a></p>
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
                                        <p><a href="prestage-cluster-adds.md">Preconfigurar objetos de equipo de clúster en Active Directory Domain Services</a></p>
<HR />
                                        <p><a href="create-failover-cluster.md">Creación de clústeres de conmutación por error</a></p> 
<HR />
                                        <p><a href="deploy-two-node-clustered-file-server.md">Implementar un servidor de archivos de dos nodos</a></p> 
<HR />
                                        <p><a href="manage-cluster-quorum.md">Administrar el cuórum y testigos</a></p> 
<HR />
                                        <p><a href="deploy-cloud-witness.md">Implementar un testigo en la nube</a></p>
<HR />
                                        <p><a href="file-share-witness.md">Implementar un testigo de uso compartido de archivos</a></p>
<HR />
                                        <p><a href="cluster-operating-system-rolling-upgrade.md">Actualizaciones graduales del sistema operativo de los clústeres</a></p> 
<HR />
                                        <p><a href="upgrade-option-same-hardware.md">Actualizar un clúster de conmutación por error en el mismo hardware</a></p>
<HR />
                                        <p><a href="https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn265970\(v%3dws.11\)">Implementar un clúster desconectado de Active Directory</a></p>
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
                                        <p><a href="cluster-aware-updating.md">Actualización compatible con clústeres</a></p> 
<HR />
                                        <p><a href="health-service-overview.md">Servicio de mantenimiento</a></p>
<HR />
                                        <p><a href="cluster-domain-migration.md">Migración de dominio de clústeres</a></p>
<HR />
                                        <p><a href="troubleshooting-using-wer-reports.md">Solución de problemas de uso del Informe de errores de Windows</a></p> 
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
                                        <p><a href="https://docs.microsoft.com/powershell/module/failoverclusters/?view=win10-ps">Cmdlets de Windows PowerShell de clúster de conmutación por error</a></p> 
<HR />
                                        <p><a href="https://docs.microsoft.com/powershell/module/clusterawareupdating/?view=win10-ps">Cmdlets de PowerShell de actualización compatible con clústeres</a></p> 
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
                                        <p><a href="http://blogs.msdn.com/b/clustering/">Blog del equipo de Clústeres de conmutación por error y Equilibrio de carga de red</a></p> 
                                        </div>
                                    </div>
                                </div>
                            </div>
                          </a>
                        </li>
</ul>
