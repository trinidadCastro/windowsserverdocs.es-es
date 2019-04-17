---
title: "Integración de servicios de recuperación de sitio en Azure"
description: "Describe cómo usar Windows Server Essentials"
ms.custom: na
ms.date: 10/01/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 262701a6-8a97-4c4e-bfbf-9f8007c308d6
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 85e681cfc19241941773dd94f05ba59759aaf62a
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
#<a name="azure-site-recovery-services-integration"></a>Integración de servicios de recuperación de sitio en Azure

>Se aplica a: Windows Server 2016 Essentials

[Servicios de Azure sitio recuperación](https://azure.microsoft.com/en-us/documentation/services/site-recovery/) es un servicio que ofrece Microsoft Azure habilitar la replicación en tiempo real de las máquinas virtuales (VM) a un depósito de copia de seguridad en Azure. En caso de que el servidor o el sitio está inactivo debido a un hardware o a otro error, puede conmutación por error a Azure donde la imagen de máquina virtual almacenada en el almacén de copia de seguridad se aprovisionará como una VM en ejecución en Azure. Combinada con una red Virtual de Azure, en caso de una conmutación por error a Azure, equipos que conectado anteriormente en el servidor local de cliente de forma transparente se conectan al servidor que ejecuta en Azure.

Integración de servicios de recuperación del sitio de Azure con Windows Server Essentials se inicia en la misma manera que la configuración de [redes virtuales de Azure](azure-virtual-network-integration.md). Desde el **integración de servicios de nube de Microsoft** página en el panel, haz clic en **integrar con servicios de Azure sitio recuperación** a la derecha del panel:

![Captura de pantalla que muestra la pestaña introducción en la página principal del panel de Windows Server Essentials. En la pestaña introducción, se ha seleccionado la sección Servicios y el panel indica en la integración de servicios de nube de Microsoft Azure recuperación está deshabilitada.](media/azure-site-recovery-1.PNG)

Al igual que con la integración de red Virtual de Azure y la integración de copia de seguridad de Azure, debes iniciar sesión en Azure con la cuenta de Azure existente o crear una nueva cuenta:

![Captura de pantalla que muestra la página de inicio de sesión en a Microsoft Azure del Asistente para la replicación de habilitar a Azure. Dado que el usuario aún no ha firmado en Microsoft Azure, se muestra el botón de inicio de sesión.](media/azure-site-recovery-2.PNG)

Después de iniciar sesión correctamente en Azure, verás una pantalla que te preguntará qué quieres asociar con el servicio de recuperación del sitio de Azure, así como la región donde se almacenará y hospedado la VM de Azure de suscripción:

![Captura de pantalla que muestra la página de inicio de sesión en a Microsoft Azure del Asistente para la replicación de habilitar a Azure. Dado que el usuario inició sesión en Microsoft Azure, esta página ofrece opciones para seleccionar una suscripción, cuenta de almacenamiento y de región.](media/azure-site-recovery-3.PNG)

Después de suscripción y la selección de región, una nueva pestaña aparecerán en la **panel de Windows Server Essentials** llama **Azure recuperación**. La detección de redes se realiza para identificar y enumerar los servidores host compatible (ejecuta Hyper-V de Windows Server 2012 R2 y superior), así como las máquinas virtuales (invitados) en los hosts individuales:

![Captura de pantalla que muestra la página de recuperación de Azure del panel de Windows Server Essentials. Dos hosts de Hyper-V se muestran junto con las máquinas virtuales que se ejecutan en estos hosts. Una máquina virtual denominado ramh157v01 en host RAM-H1-7 está seleccionado y replicación a Azure actualmente está deshabilitada para esta máquina virtual.](media/azure-site-recovery-4.PNG)

### <a name="enabling-guest-virtual-machines-for-protection"></a>Habilitación de máquinas virtuales de invitado para la protección

Al realizar la selección de una máquina virtual presente en la ventana de recuperación de Azure, puedes hacer clic en **Habilitar replicación a Azure** en el lado derecho del panel para preparar y copia la máquina virtual™ imagen s a Azure:

![Captura de pantalla que muestra el cuadro de diálogo Habilitar replicación a Azure. Una barra de progreso se muestra como se agrega un host.](media/azure-site-recovery-5.PNG)

Durante este proceso, el agente del servicio de recuperación del sitio de Azure está instalado en el servidor host, un depósito de copia de seguridad donde la image del huésped que se almacenará la máquina virtual se crea y se comienza la replicación de la imagen a Azure. Según el tamaño de la máquina virtual que se replica, finalización del proceso de replicación puede tardar horas o días. Hasta que la imagen completa de máquina virtual y deltas más recientes se replican a Azure, las tareas de conmutación por error no están disponibles y la máquina virtual no está protegida. Una vez completada la replicación, la columna de estado de protección de la ventana de recuperación de Azure cambiará de **replicar** a **habilitado**:

![Captura de pantalla que muestra la página de recuperación de Azure del panel de Windows Server Essentials. Dos hosts de Hyper-V se muestran junto con las máquinas virtuales que se ejecutan en estos hosts. Una máquina virtual denominado ramh12v02 en host RAM-H1-2 está seleccionado y replicación a Azure está habilitada para esta máquina virtual.](media/azure-site-recovery-6.PNG)

### <a name="failover-of-a-guest-vm-to-azure"></a>Conmutación por error de Invitado VM de Azure

![Captura de pantalla que muestra Failover VM a la página de Azure.](media/azure-site-recovery-7.PNG)

Cuando una máquina virtual que está protegido se produce un error o el servidor host que se ejecuta la máquina virtual en se produce un error, conmutación por error a Azure puede llevar a cabo para mantener la continuidad del negocio hasta que el servidor de local virtual equipo o de host está reparado y disponible. Como la figura anterior muestra, hay tres tipos de conmutación por error que son compatibles con los servicios de recuperación de sitios de Azure:

-   **Probar la conmutación por error** ƒA buen plan de recuperación incorpora la capacidad para simular un desastre para garantizar el tiempo de inactividad mínimo un desastre real. Una conmutación por error probar toma la imagen de máquina virtual que ha sido replicar en el almacén de recuperación, se aprovisiona como una máquina virtual en Azure y te permite conectarte al servidor para probar los escenarios que se aplican a la empresa. Durante una prueba de conmutación por error, la máquina virtual local continúa ejecutándose sin interrupciones que afecten no empresa durante la prueba de recuperación ante desastres. Una vez completada la conmutación por error probar, dejarás de la prueba a través del Portal de Azure, que elimina aprovisiona la máquina virtual y elimina el disco duro virtual. Durante la prueba completa de conmutación por error, la imagen de máquina virtual en el almacén de recuperación sigue se replican desde la máquina virtual in situ como si nunca ha ocurrido nada.

-   **La conmutación por error imprevisto** ƒAn imprevisto de conmutación por error se produce cuando se produce un error real con el servidor de host protegido o ejecutarse en el servidor host de máquina virtual. La conmutación por error se desencadena manualmente desde cualquier panel de Windows Server Essentials, o si el servidor que ha fallado es el servidor que se ejecuta Windows Server Essentials, se puede desencadenar desde el Portal de Azure directamente. En este caso, la conmutación por error imprevisto toma la imagen de máquina virtual que ha sido replicar en el almacén de recuperación, se aprovisiona como una máquina virtual en Azure y te permite conectarte al servidor para probar los escenarios que se aplican a la empresa. Cuando se restaure el servidor local, se puede desencadenar una recuperación manual desde el Portal de Azure que, a continuación, copia la imagen de máquina virtual baja por el servidor local.

-   **Planeado de conmutación por error** ƒA planeado conmutación por error es una acción que se pueden realizar en caso de que las actividades planeadas, como el mantenimiento de hardware, deben realizar lo que haría que el servidor hacia abajo. En caso de una conmutación por error planeada, el mismo proceso tiene lugar en relación con el aprovisionamiento de la imagen de máquina virtual duplicada en Azure. Sin embargo, antes de hacerlo, el servidor local se cierra de manera ordenada para asegurarte de que todos los cambios se replicarán en Azure antes del apagado. Una vez completado el mantenimiento planeado, puedes activar manualmente una recuperación desde el panel de Windows Server Essentials o el portal de Azure, lo que lleva hacia arriba el servidor local y, a continuación, cancelará el aprovisionamiento la máquina virtual en Azure y elimina el. Archivo VHD. Replicación de la máquina virtual local a Azure seguirán, a continuación, vuelve a ocurrir de forma normal.

En cualquiera de los tres casos anteriores, cuando una máquina virtual se conmuta a Azure, el panel de Windows Server Essentials mostrará la nueva máquina virtual en Azure ejecutando como se muestra en la figura siguiente.

![Captura de pantalla que muestra la página de recuperación de Azure del panel de Windows Server Essentials. Se ha habilitado la replicación de Azure para un host denominado Essentials y una máquina virtual llamado prueba Essentials ejecutar en Azure indica que el host no ha podido sobre Azure.](media/azure-site-recovery-8.PNG)

<a name="see-also"></a>Consulta también
--------
[Introducción a Windows Server Essentials](get-started.md)
