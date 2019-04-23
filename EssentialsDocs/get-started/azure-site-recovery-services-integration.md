---
title: Integración de los servicios de Azure Site Recovery
description: Describe cómo usar Windows Server Essentials
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
ms.openlocfilehash: 94894cd9be06485bf842f96517172db709dbe93d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848746"
---
#<a name="azure-site-recovery-services-integration"></a>Integración de los servicios de Azure Site Recovery

>Se aplica a: Windows Server 2016 Essentials

[Servicios de Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/) es un servicio que ofrece Microsoft Azure, habilitar la replicación en tiempo real de las máquinas virtuales (VM) en un almacén de copia de seguridad en Azure. En caso de que el servidor o el sitio está inactivo debido a un hardware u otro error, puede conmutar por error a Azure donde se aprovisionará la imagen de máquina virtual almacenada en el almacén de copia de seguridad como una máquina virtual en ejecución en Azure. Combinado con una red Virtual de Azure, si se produce una conmutación por error a Azure, los equipos que había conectado al servidor local cliente transparente se conectarán al servidor que se ejecuta en Azure.

La integración de Azure Site Recovery Services con Windows Server Essentials se inicia en la misma manera que la configuración de [redes virtuales de Azure](azure-virtual-network-integration.md). Desde el **integración de servicios en la nube de Microsoft** página en el panel, haga clic en **integrar con Azure Site Recovery Services** a la derecha del panel:

![Captura de pantalla que muestra la pestaña de empezar a trabajar en la página principal del panel de Windows Server Essentials. En la ficha empezar a trabajar, se ha seleccionado la sección de servicios y el panel indica en la integración de servicios en la nube de Microsoft que la recuperación de Azure está deshabilitada actualmente.](media/azure-site-recovery-1.PNG)

Al igual que con integración de Azure Virtual network y la integración de Azure Backup, debe iniciar sesión en Azure con la cuenta de Azure existente o crear una nueva cuenta:

![Captura de pantalla que muestra la página de inicio de sesión en Microsoft Azure del Asistente para habilitar la replicación en Azure. Se muestra el botón Iniciar sesión porque el usuario no ha iniciado aún en Microsoft Azure.](media/azure-site-recovery-2.PNG)

Después de iniciar sesión correctamente en Azure, verá una pantalla que le preguntará qué suscripción desea asociar con el servicio Azure Site Recovery, así como la región de Azure donde se almacenará y hospeda la máquina virtual:

![Captura de pantalla que muestra la página de inicio de sesión en Microsoft Azure del Asistente para habilitar la replicación en Azure. Dado que el usuario ha iniciado sesión en Microsoft Azure, esta página proporciona opciones para seleccionar una suscripción, la cuenta de almacenamiento y la región.](media/azure-site-recovery-3.PNG)

Después de la selección de región y suscripción, una nueva pestaña aparecerá en el **panel de Windows Server Essentials** llamado **Azure Recovery**. La detección de redes se realiza para identificar y enumerar los servidores de host admitidos (ejecuta Windows Server Hyper-V 2012 R2 y versiones posteriores), así como las máquinas virtuales (invitados) en los hosts individuales:

![Captura de pantalla que muestra la página de Azure Recovery del panel de Windows Server Essentials. Dos hosts de Hyper-V se muestran junto con las máquinas virtuales que se ejecutan en estos hosts. Una máquina virtual denominada ramh157v01 en host de H1-RAM-7 está seleccionada y la replicación en Azure está deshabilitado actualmente para esta máquina virtual.](media/azure-site-recovery-4.PNG)

### <a name="enabling-guest-virtual-machines-for-protection"></a>Habilitar las máquinas virtuales de invitado para la protección

Tras la selección de una máquina virtual presente en la ventana de la recuperación de Azure, puede hacer clic en **habilitar la replicación en Azure** en el lado derecho del panel para preparar y copiar la máquina virtual™ imagen s en Azure:

![Captura de pantalla que muestra el cuadro de diálogo Habilitar la replicación en Azure. Una barra de progreso se muestra como un host que se va a agregar.](media/azure-site-recovery-5.PNG)

Durante este proceso, el agente del servicio Azure Site Recovery se instala en el servidor host, un almacén de copia de seguridad donde se crea la imagen del invitado que VM se almacenarán y comienza la replicación de la imagen en Azure. Según el tamaño de la máquina virtual que se replican, finalización del proceso de replicación puede tardar horas o días. Hasta que la imagen de máquina virtual completa y deltas más recientes se replican en Azure, las tareas de conmutación por error no están disponibles y la máquina virtual no está protegida. Una vez completada la replicación, la columna de estado de protección en la ventana de Azure Recovery cambiará de **replicando** a **habilitado**:

![Captura de pantalla que muestra la página de Azure Recovery del panel de Windows Server Essentials. Dos hosts de Hyper-V se muestran junto con las máquinas virtuales que se ejecutan en estos hosts. Una máquina virtual denominada ramh12v02 en host que está seleccionada la RAM-H1-2 y la replicación en Azure está habilitada actualmente para esta máquina virtual.](media/azure-site-recovery-6.PNG)

### <a name="failover-of-a-guest-vm-to-azure"></a>Conmutación por error de un Invitado VM en Azure

![Captura de pantalla que muestra Failover VM a la página de Azure.](media/azure-site-recovery-7.PNG)

Cuando una máquina virtual que está protegido se produce un error o el servidor de host que se pueden realizar las ejecuciones de la máquina virtual protegida en produce un error, conmutación por error a Azure para mantener la continuidad del negocio hasta el en local host o máquina servidor virtual es reparado y está disponible. Como muestra la ilustración anterior, hay tres tipos de conmutación por error que son compatibles con Azure Site Recovery Services:

-   **Probar conmutación por error** plan de recuperación ante desastres conveniente ƒA incorpora la capacidad de simular un desastre para garantizar el tiempo de inactividad mínimo si se produce un desastre real. Una conmutación por error de prueba toma la imagen de máquina virtual que se han replicado en el almacén de recuperación, se aprovisiona como una máquina virtual en ejecución en Azure y le permite conectarse al servidor para probar los escenarios que se aplican a la empresa. Durante una conmutación por error de prueba, la máquina virtual local continúa ejecutándose sin interrupciones como para no interrumpir el negocio durante la prueba de recuperación ante desastres. Una vez completada la conmutación por error de prueba, deberá detener la prueba a través del Portal de Azure, que desaprovisiona la máquina virtual y elimina el disco duro virtual. Durante la conmutación por error de prueba completa, la imagen de máquina virtual en el almacén de recovery continúa se replican desde la máquina virtual local como si nada ha sucedido.

-   **Conmutación por error imprevista** ƒAn conmutación por error imprevista se produce cuando hay un error real con el servidor de host protegido o que se ejecutan en el servidor host de máquina virtual. La conmutación por error se desencadena manualmente desde cualquier panel de Windows Server Essentials, o si el propio servidor con errores es el servidor que se ejecuta Windows Server Essentials, puede activarse desde el Portal de Azure directamente. En este caso, la conmutación por error no planeada toma la imagen de máquina virtual que se han replicado en el almacén de recuperación, se aprovisiona como una máquina virtual en ejecución en Azure y le permite conectarse al servidor para probar los escenarios que se aplican a la empresa. Cuando se restaure el servidor local, se puede desencadenar una conmutación por recuperación manual desde el Portal de Azure que, a continuación, copiará la imagen de máquina virtual de vuelta al servidor local.

-   **Conmutación por error planeada** ƒA planeada es una acción que se puede realizar en caso de que las actividades planeadas, como el mantenimiento de hardware, deben tener lugar que desea detener el servidor de conmutación por error. Si se produce una conmutación por error planeada, el mismo proceso tiene lugar con respecto a que el aprovisionamiento de la imagen de máquina virtual replicada en Azure. Sin embargo, antes de hacerlo, el servidor local se apaga de manera ordenada para asegurarse de que todos los cambios se replican en Azure antes del apagado. Una vez completado el mantenimiento planeado, puede desencadenar manualmente una conmutación por recuperación desde el panel de Windows Server Essentials, o el portal de Azure, lo que podría poner copia de seguridad del servidor local y, a continuación, cancelar el aprovisionamiento de la máquina virtual en Azure y elimine el. Archivo de disco duro virtual. Replicación de la máquina virtual local a Azure, a continuación, continuaría se vuelve a producir con normalidad.

En cualquiera de los tres casos anteriores, cuando una máquina virtual se conmuta por error a Azure, el panel de Windows Server Essentials mostrará la nueva máquina virtual de Azure que se ejecuta como se muestra en la ilustración siguiente.

![Captura de pantalla que muestra la página de Azure Recovery del panel de Windows Server Essentials. Ha habilitado la replicación en Azure para un host con el nombre de Essentials y una máquina virtual de prueba para Essentials con nombre que se ejecuta en Azure indica que el host no ha podido a través de Azure.](media/azure-site-recovery-8.PNG)

<a name="see-also"></a>Vea también
--------
[Introducción a Windows Server Essentials](get-started.md)
