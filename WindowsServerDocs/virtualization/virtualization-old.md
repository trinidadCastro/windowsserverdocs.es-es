---
title: Virtualización
description: Proporciona una descripción general de las tecnologías de virtualización, como contenedores, Hyper-V e Interruptor virtual Hyper-V, además de vínculos a contenido adicional para Windows Server 2016 y versiones posteriores del sistema operativo.
ms.prod: windows-server-threshold
manager: dougkim
ms.technology: compute
ms.topic: article
author: shortpatti
ms.author: pashort
ms.localizationpriority: medium
ms.date: 03/16/2018
ms.openlocfilehash: b3b018037d788d47fafe7d3adda50cfb5831ab56
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2018
ms.locfileid: "6066729"
---
# Virtualización

>Se aplica a: Windows Server (canal semianual), Windows Server 2016 

>[!TIP]
> ¿Buscas información sobre versiones anteriores de Windows Server? Echa un vistazo a nuestras otras [bibliotecas de Windows Server](/previous-versions/windows/) en docs.microsoft.com. También puedes [buscar en este sitio](https://docs.microsoft.com/search/index?search=Windows+Server&dataSource=previousVersions) para obtener información específica.

<img src="../media/landing-icons/virtualization.png" style='float:left; padding:.5em;' alt="Icon showing a box with spokes"> La virtualización en Windows Server 2016 es una de las tecnologías fundamentales necesarias para crear tu infraestructura definida por software. Junto con la red y almacenamiento, las funciones de virtualización ofrecen la flexibilidad que necesitas para impulsar las cargas de trabajo para tus clientes.

Tecnologías de virtualización de Windows Server incluyen actualizaciones de Hyper-V, conmutador Virtual de Hyper-V y \(VMs\) tejido protegido y máquinas virtuales blindadas, que mejoran la seguridad, escalabilidad y confiabilidad. Las actualizaciones de clústeres de conmutación por error, redes y almacenamiento facilitan aún más la implementación y administración de estas tecnologías cuando se usan con Hyper-V. 


<ul class="cardsI panelContent">
<li>
        <a href="../security/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms-top-node.md">
          <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-access.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3>VM de tejido protegido y blindadas</h3>
                        <p>Como administrador de empresa de nube privada y proveedor de servicio en la nube, puedes usar un tejido protegido para proporcionar un entorno más seguro para las máquinas virtuales. Un tejido protegido consta de un Servicio de protección de host \(HGS\), por lo general un clúster de tres nodos, además de uno o más hosts protegidos y un conjunto de VM blindadas.</p>
                    </div>
                </div>
            </div>
        </div>
       </a>
    </li>
<li>
        <a href="/hyper-v/Hyper-V-on-Windows-Server.md">
          <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-access.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3>Windows 10 para empresas: formas de usar los dispositivos para el trabajo</h3>
                        <p>La tecnología Hyper-V proporciona recursos informáticos mediante la virtualización de hardware. Hyper-V crea una versión de software de un equipo, denominada máquina virtual, que usas para ejecutar un sistema operativo y las aplicaciones. Puedes ejecutar varias máquinas virtuales al mismo tiempo y puedes crearlas y eliminarlas según sea necesario. </p>
                    </div>
                </div>
            </div>
        </div>
       </a>
     </li>

<li>
        <a href="https://docs.microsoft.com/windows-server/virtualization/hyper-v/hyper-v-server-2016">
          <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-access.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3>Microsoft Hyper-V Server</h3>
                        <p>La tecnología Hyper-V proporciona recursos informáticos mediante la virtualización de hardware. Hyper-V crea una versión de software de un equipo, denominada máquina virtual, que usas para ejecutar un sistema operativo y las aplicaciones. Puedes ejecutar varias máquinas virtuales al mismo tiempo y puedes crearlas y eliminarlas según sea necesario. </p>
                    </div>
                </div>
            </div>
        </div>
       </a>
     </li>


<li>
        <a href="hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md">
          <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-access.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3>Conmutador virtual de Hyper-V</h3>
                        <p>El conmutador virtual de Hyper\-V es un conmutador software de red Ethernet basado en software, de nivel 2, que está incluido en todas las versiones de Hyper-V.</p>

                        <p>El conmutador Virtual de Hyper\-V está disponible en el Administrador de Hyper\-V después de instalar el rol de servidor de Hyper\-V.</p>

                        <p>El conmutador virtual de Hyper\-V incluye funcionalidades administradas mediante programación y extensibles que te permiten conectar máquinas virtuales tanto a redes virtuales como a la red física.</p> 

                        <p>Además el conmutador virtual de Hyper-V exige la aplicación de la directiva en los niveles de servicio, aislamiento y seguridad.</p>
                    </div>
                </div>
            </div>
        </div>
       </a>
     </li>


<li>
       <a href="https://docs.microsoft.com/virtualization/windowscontainers">
          <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-access.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3>Contenedores de Windows</h3>
                        <p>Los contenedores de Windows proporcionan una virtualización a nivel de sistema operativo que permite ejecutar varias aplicaciones aisladas en un solo sistema. Se incluyen dos tipos diferentes de contenedores en tiempo de ejecución con la función, cada uno de ellos con distintos grados de aislamiento de aplicaciones.</p>
                    </div>
                </div>
            </div>
        </div>
       </a>
     </li>




## Relacionados

Hyper-V requiere hardware específico para crear el entorno de virtualización. Para obtener detalles, consulta [Requisitos del sistema para Hyper-V en Windows Server 2016](./hyper-v/system-requirements-for-hyper-v-on-windows.md). 

Para obtener más información, consulta [Hyper-V en Windows 10](https://docs.microsoft.com/virtualization/hyper-v-on-windows).

