---
title: Integración de los servicios de Azure Site Recovery
description: Aprenda a usar Azure Site Recovery Services para crear replicaciones en tiempo real de las máquinas virtuales (VM) que se almacenan en el almacén de copia de seguridad en Azure.
ms.date: 10/01/2016
ms.topic: article
ms.assetid: 262701a6-8a97-4c4e-bfbf-9f8007c308d6
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: 1f6d0f69bdec22c264bfdea6a232892accafe414
ms.sourcegitcommit: 8e330f9066097451cd40e840d5f5c3317cbc16c2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/19/2020
ms.locfileid: "97696870"
---
# <a name="azure-site-recovery-services-integration"></a>Integración de los servicios de Azure Site Recovery

>Se aplica a: Windows Server 2016 Essentials

[Azure Site Recovery Services](/azure/site-recovery/) es un servicio que ofrece Microsoft Azure la habilitación de la replicación en tiempo real de las máquinas virtuales (VM) en un almacén de copia de seguridad de Azure. En caso de que el servidor o el sitio estén inactivos debido a un error de hardware u otro, puede conmutar por error a Azure, donde la imagen de máquina virtual almacenada en el almacén de copia de seguridad se aprovisionará como una máquina virtual en ejecución en Azure. En combinación con una red virtual de Azure, en el caso de una conmutación por error a Azure, los equipos cliente que anteriormente estaban conectados al servidor local se conectarán de forma transparente al servidor que se ejecuta en Azure.

La integración de Azure Site Recovery Services con Windows Server Essentials se inicia de la misma manera que la configuración de la [red virtual de Azure](azure-virtual-network-integration.md). En la página de **integración de Microsoft Cloud Services** en el panel, haga clic en **integrar con Azure Site Recovery Services** a la derecha del panel:

![Captura de pantalla que muestra la pestaña introducción en la Página principal del panel de Windows Server Essentials. En la pestaña introducción, se ha seleccionado la sección servicios y el panel indica en integración de Microsoft Cloud Services que la recuperación de Azure está deshabilitada actualmente.](media/azure-site-recovery-1.PNG)

Al igual que con la integración de la red virtual de Azure y la integración de Azure Backup, debe iniciar sesión en Azure con la cuenta de Azure existente o crear una cuenta nueva:

![Una captura de pantalla que muestra la página iniciar sesión en Microsoft Azure del Asistente para habilitar la replicación en Azure. Se muestra el botón iniciar sesión porque el usuario aún no ha iniciado sesión en Microsoft Azure.](media/azure-site-recovery-2.PNG)

Después de iniciar sesión correctamente en Azure, verá una pantalla que le preguntará qué suscripción desea asociar con el servicio de Azure Site Recovery, así como la región de Azure donde se almacenará y hospedará la máquina virtual:

![Una captura de pantalla que muestra la página iniciar sesión en Microsoft Azure del Asistente para habilitar la replicación en Azure. Dado que el usuario ha iniciado sesión en Microsoft Azure, esta página proporciona opciones para seleccionar una suscripción, una cuenta de almacenamiento y una región.](media/azure-site-recovery-3.PNG)

Después de la suscripción y la selección de la región, aparecerá una nueva pestaña en el **Panel de Windows Server Essentials** llamado **recuperación de Azure**. El análisis de redes se realiza para identificar y enumerar los servidores host compatibles (que ejecutan Windows Server Hyper-V 2012 R2 y versiones posteriores), así como las máquinas virtuales (invitados) en los hosts individuales:

![Captura de pantalla que muestra la página recuperación de Azure del panel de Windows Server Essentials. Se muestran dos hosts de Hyper-V junto con las máquinas virtuales que se ejecutan en estos hosts. Hay una máquina virtual llamada ramh157v01 en el host RAM-H1-7 seleccionada y la replicación en Azure está deshabilitada actualmente para esta máquina virtual.](media/azure-site-recovery-4.PNG)

### <a name="enabling-guest-virtual-machines-for-protection"></a>Habilitación de las máquinas virtuales invitadas para protección

Al seleccionar una máquina virtual presente en la ventana de recuperación de Azure, puede hacer clic en **Habilitar replicación en Azure** en el lado derecho del panel para preparar y copiar la imagen de la máquina virtual &trade; en Azure:

![Una captura de pantalla que muestra el cuadro de diálogo habilitar la replicación en Azure. Se muestra una barra de progreso a medida que se agrega un host.](media/azure-site-recovery-5.PNG)

Durante este proceso, el agente de servicio de Azure Site Recovery se instala en el servidor host, se crea un almacén de copia de seguridad donde se almacena la imagen de la máquina virtual invitada y se inicia la replicación de la imagen en Azure. En función del tamaño de la máquina virtual que se va a replicar, la finalización del proceso de replicación puede tardar horas o días. Hasta que la imagen de máquina virtual completa y las diferencias más recientes se repliquen en Azure, las tareas de conmutación por error no están disponibles y la máquina virtual no está protegida. Una vez completada la replicación, la columna Estado de protección de la ventana de recuperación de Azure cambiará de **replicar** a **habilitado**:

![Captura de pantalla que muestra la página recuperación de Azure del panel de Windows Server Essentials. Se muestran dos hosts de Hyper-V junto con las máquinas virtuales que se ejecutan en estos hosts. Hay una máquina virtual llamada ramh12v02 en el host RAM-H1-2 seleccionada y la replicación en Azure está habilitada actualmente para esta máquina virtual.](media/azure-site-recovery-6.PNG)

### <a name="failover-of-a-guest-vm-to-azure"></a>Conmutación por error de una máquina virtual invitada a Azure

![Captura de pantalla que muestra la página máquina virtual de conmutación por error en Azure.](media/azure-site-recovery-7.PNG)

Cuando se produce un error en una máquina virtual protegida, o en el servidor host en el que se ejecuta la máquina virtual protegida, se puede realizar una conmutación por error a Azure para mantener la continuidad del negocio hasta que la máquina virtual o el servidor host local se reparen y estén disponibles. Como se muestra en la figura anterior, hay tres tipos de conmutación por error que se admiten con Azure Site Recovery Services:

-   La **conmutación por error de prueba** ƒA buen plan de recuperación ante desastres incorpora la capacidad de simular un desastre para garantizar un tiempo de inactividad mínimo en caso de que se produzca un desastre real. Una conmutación por error de prueba toma la imagen de máquina virtual que se ha replicado en el almacén de recuperación, lo aprovisiona como una máquina virtual en ejecución en Azure y le permite conectarse al servidor para probar los escenarios que se aplican a la empresa. Durante una conmutación por error de prueba, la máquina virtual local continúa ejecutándose sin interrupciones para no interrumpir el negocio durante la prueba de recuperación ante desastres. Una vez completada la conmutación por error de prueba, detendrá la prueba a través de Azure portal, que desaprovisiona la máquina virtual y elimina el VHD. Durante toda la conmutación por error de prueba, la imagen de máquina virtual del almacén de recuperación continúa replicándose desde la máquina virtual en el sitio como si no se hubiera producido nada.

-   Conmutación **por** error no planeada ƒAn la conmutación por error no planeada se produce cuando hay un error real con el servidor host protegido o la máquina virtual que se ejecuta en el servidor host. La conmutación por error se desencadena manualmente desde el panel de Windows Server Essentials, o bien, si el servidor que ha generado el error es el servidor en el que se ejecuta Windows Server Essentials, puede activarse directamente desde Azure portal. En este caso, la conmutación por error no planeada toma la imagen de máquina virtual que se ha replicado en el almacén de recuperación, lo aprovisiona como una máquina virtual en ejecución en Azure y le permite conectarse al servidor para probar los escenarios que se aplican a la empresa. Cuando el servidor se restaure de forma local, se puede desencadenar una conmutación por recuperación manual desde Azure portal que, a continuación, copiará de nuevo la imagen de máquina virtual en el servidor local.

-   La conmutación por error planeada ƒA de **conmutación por error** planeada es una acción que se puede llevar a cabo en el caso de que se produzcan actividades planeadas, como el mantenimiento de hardware, lo que haría que el servidor estuviera inactivo. En el caso de una conmutación por error planeada, se realiza el mismo proceso con respecto al aprovisionamiento de la imagen de máquina virtual replicada en Azure. Sin embargo, antes de hacerlo, el servidor local se apaga de manera ordenada para asegurarse de que todos los cambios se replican en Azure antes del cierre. Una vez completado el mantenimiento planeado, puede desencadenar manualmente una conmutación por recuperación desde el panel de Windows Server Essentials o desde el Azure Portal, que abriría el servidor local y, a continuación, desaprovisiona la máquina virtual en Azure y elimine el. Archivo VHD. La replicación desde la máquina virtual local a Azure seguiría apareciendo de nuevo de la manera habitual.

En cualquiera de los tres casos anteriores, cuando una máquina virtual conmute por error a Azure, el panel de Windows Server Essentials mostrará la nueva máquina virtual en Azure que se ejecuta como en la ilustración siguiente.

![Captura de pantalla que muestra la página recuperación de Azure del panel de Windows Server Essentials. La replicación en Azure se ha habilitado para un host con el nombre Essentials y una máquina virtual llamada Essentials-Test que se ejecuta en Azure indica que el host se ha conmutado por error a Azure.](media/azure-site-recovery-8.PNG)

<a name="see-also"></a>Consulte también
--------
[Introducción a Windows Server Essentials](get-started.md)