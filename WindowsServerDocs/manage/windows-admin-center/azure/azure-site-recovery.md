---
title: Proteger el Virtual Machines de Hyper-V con Azure Site Recovery y el centro de administración de Windows
description: Use el centro de administración de Windows (Project Honolulu) para proteger las máquinas virtuales de Hyper-V con Azure Site Recovery.
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 07/17/2018
ms.localizationpriority: low
ms.openlocfilehash: 265589789f2966e7b6140543876f41058aa5d705
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87940086"
---
# <a name="protect-your-hyper-v-virtual-machines-with-azure-site-recovery-and-windows-admin-center"></a>Proteger el Virtual Machines de Hyper-V con Azure Site Recovery y el centro de administración de Windows

>Se aplica a: Versión preliminar de Windows Admin Center, Windows Admin Center

[Más información acerca de la integración de Azure con el centro de administración de Windows.](../plan/azure-integration-options.md)

El centro de administración de Windows simplifica el proceso de replicación de las máquinas virtuales en los servidores o clústeres de Hyper-V, lo que facilita el aprovechamiento de la eficacia de Azure desde su propio centro de recursos. Para automatizar la instalación, puede conectar la puerta de enlace del centro de administración de Windows a Azure.

Use la siguiente información para configurar las opciones de replicación y crear un plan de recuperación desde el Azure Portal, habilitando el centro de administración de Windows para iniciar la replicación de la máquina virtual y proteger las máquinas virtuales.

## <a name="what-is-azure-site-recovery-and-how-does-it-work-with-windows-admin-center"></a>¿Qué es Azure Site Recovery y cómo funciona con el centro de administración de Windows?

**Azure Site Recovery** es un servicio de Azure que replica cargas de trabajo que se ejecutan en máquinas virtuales para que la infraestructura crítica para la empresa esté protegida en caso de desastre.  [Más información sobre Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview).

Azure Site Recovery consta de dos componentes: **replicación** y **conmutación por error**. La parte de replicación protege las máquinas virtuales en caso de desastre mediante la replicación del VHD de la máquina virtual de destino en una cuenta de almacenamiento de Azure. Después, puede conmutar por error estas máquinas virtuales y ejecutarlas en Azure en caso de que se produzca un desastre. También puede realizar una conmutación por error de prueba sin afectar a las máquinas virtuales principales para probar el proceso de recuperación en Azure.

Completar el programa de instalación solo es suficiente para proteger la máquina virtual en caso de desastre. Sin embargo, no podrá iniciar la máquina virtual en Azure hasta que configure la parte de la conmutación por error. Puede configurar la parte de la conmutación por error si desea conmutar por error a una máquina virtual de Azure, lo cual no es necesario como parte de la configuración inicial. Si el servidor host deja de funcionar y aún no ha configurado el componente de conmutación por error, puede configurarlo en ese momento y acceder a las cargas de trabajo de la máquina virtual protegida. Sin embargo, se recomienda configurar las opciones relacionadas con la conmutación por error antes de un desastre.


## <a name="prerequisites-and-planning"></a>Requisitos previos y planeamiento

- Los servidores de destino que hospedan las máquinas virtuales que desea proteger deben tener acceso a Internet para replicar en Azure.
- [Conecte la puerta de enlace del centro de administración de Windows a Azure](azure-integration.md).
- [Revise la herramienta de planeación de capacidad para evaluar los requisitos de replicación y conmutación por error correctas](https://docs.microsoft.com/azure/site-recovery/hyper-v-site-walkthrough-capacity).

## <a name="step-1-set-up-vm-protection-on-your-target-host"></a>Paso 1: Configuración de la protección de máquinas virtuales en el host de destino

> [!NOTE]
> Debe realizar este paso una vez por cada servidor host o clúster que contenga las máquinas virtuales destinadas a la protección.

1. Navegue hasta el servidor o clúster que hospeda las máquinas virtuales que desea proteger (ya sea con Administrador del servidor o con el administrador de clústeres hiperconvergido).
2. Vaya a **virtual machines**  >  **inventario**.
3. Seleccione cualquier máquina virtual (esto no tiene que ser la máquina virtual que desea proteger).
4. Seleccione **más**  >  **configurar protección de VM**.
5. Inicie sesión en su cuenta de Azure.
6. Escriba la información necesaria:

   - **Suscripción:** La suscripción de Azure que quiere usar para la replicación de máquinas virtuales en este host.
   - **Ubicación:** La región de Azure en la que se deben crear los recursos de ASR.
   - **Cuenta de almacenamiento:** La cuenta de almacenamiento donde se guardarán las cargas de trabajo de máquinas virtuales replicadas en este host.
   - **Almacén:** Elija un nombre para el almacén de Azure Site Recovery para las máquinas virtuales protegidas en este host.

7. Seleccione **setup ASR**.
8. Espere hasta que vea la notificación: **Configuración completada de Site Recovery**.

Esto puede tardar hasta 10 minutos. Puede ver el progreso si va a **notificaciones** (el icono de campana en la parte superior derecha).

>[!NOTE]
> Este paso instala automáticamente el agente ASR en el servidor o nodos de destino (si se configura en un clúster), crea un **grupo de recursos** con la cuenta de **almacenamiento** y el **almacén** especificados, en la **Ubicación** especificada. También se registrará el host de destino con el servicio ASR y se configurará una directiva de replicación predeterminada.

## <a name="step-2-select-virtual-machines-to-protect"></a>Paso 2: seleccionar las máquinas virtuales que se van a proteger

1. Vuelva al servidor o clúster que configuró en el paso 2 anterior y vaya a **Virtual Machines > inventario**.
2. Seleccione la máquina virtual que desea proteger.
3. Seleccione **más**  >  **proteger VM**.
4. Revise los [requisitos de capacidad para proteger la máquina virtual](https://docs.microsoft.com/azure/site-recovery/site-recovery-capacity-planner).

    Si desea usar una cuenta de Premium Storage, [cree una en el Azure portal](https://docs.microsoft.com/azure/storage/common/storage-premium-storage). La opción **crear nueva** proporcionada en el panel del centro de administración de Windows crea una cuenta de almacenamiento estándar.

5. Escriba el nombre de la **cuenta de almacenamiento** que se usará para la replicación de la máquina virtual y seleccione **proteger VM**. Este paso habilita la replicación para la máquina virtual seleccionada.

6. ASR iniciará la replicación. La replicación se completa y la máquina virtual se protege cuando el valor de la columna **protegido** de la cuadrícula de **inventario de máquinas virtuales** cambia a **sí**. Esto puede tardar varios minutos.

## <a name="step-3-configure-and-run-a-test-failover-in-the-azure-portal"></a>Paso 3: Configuración y ejecución de una conmutación por error de prueba en Azure Portal

 Aunque no es necesario completar este paso al iniciar la replicación de la máquina virtual (la máquina virtual ya estará protegida con solo replicación), se recomienda configurar las opciones de conmutación por error al configurar Azure Site Recovery. Si quiere preparar la conmutación por error a una máquina virtual de Azure, complete los pasos siguientes:

1. [Configuración de una red de Azure](https://docs.microsoft.com/azure/site-recovery/hyper-v-site-walkthrough-prepare-azure) la máquina virtual conmutada por error se asociará a esta red virtual. Tenga en cuenta que el resto de los pasos enumerados en la página vinculada se completan automáticamente en el centro de administración de Windows.  solo tiene que configurar la red de Azure.

2. [Ejecute una conmutación por error de prueba](https://docs.microsoft.com/azure/site-recovery/hyper-v-site-walkthrough-test-failover).

## <a name="step-4-create-recovery-plans"></a>Paso 4: crear planes de recuperación

**Plan de recuperación** es una característica de Azure Site Recovery que le permite realizar una conmutación por error y recuperar una aplicación completa que consta de una colección de máquinas virtuales. Aunque es posible recuperar máquinas virtuales protegidas de forma individual, al agregar las máquinas virtuales que componen una aplicación a un plan de recuperación, podrá realizar la conmutación por error de toda la aplicación a través del plan de recuperación. También puede usar la característica de conmutación por error de prueba del plan de recuperación para probar la recuperación de la aplicación. El plan de recuperación permite agrupar las máquinas virtuales, secuenciar el orden en el que deben aparecer durante una conmutación por error y automatizar los pasos adicionales que se deben realizar como parte del proceso de recuperación. Una vez que haya protegido las máquinas virtuales, puede ir al almacén de Azure Site Recovery en el Azure Portal y crear planes de recuperación para estas máquinas virtuales. [Más información sobre los planes de recuperación](https://docs.microsoft.com/azure/site-recovery/site-recovery-create-recovery-plans).

## <a name="monitoring-replicated-vms-in-azure"></a>Supervisión de máquinas virtuales replicadas en Azure ##

Para comprobar que no hay ningún error en el registro del servidor, vaya al **Azure portal**  >  **todos los recursos**  >  **Recovery Services almacén** (el que especificó en el paso 2) > **trabajos**  >  **Site Recovery trabajos**.

Puede supervisar la replicación de la máquina virtual en **Recovery Services Vault**los  >  **elementos replicados**del almacén de Recovery Services.

Para ver todos los servidores que están registrados en el almacén, vaya a **Recovery Services Vault**  >  **Site Recovery infraestructura**  >  **hosts de Hyper-v** (en la sección sitios de Hyper-v).

## <a name="known-issue"></a>Problema conocido ##

Cuando se registra ASR con un clúster, si un nodo no puede instalar ASR o registrarse en el servicio ASR, es posible que las máquinas virtuales no estén protegidas. Para comprobar que todos los nodos del clúster están registrados en el Azure portal, vaya a los trabajos del **almacén de Recovery Services**  >  **Jobs**  >  **Site Recovery trabajos**.