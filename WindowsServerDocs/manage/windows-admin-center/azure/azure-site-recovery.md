---
title: Proteger el Virtual Machines de Hyper-V con Azure Site Recovery y el centro de administración de Windows
description: Usa Windows Admin Center (Project Honolulu) para proteger las VM Hyper-V con Azure Site Recovery.
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 07/17/2018
ms.localizationpriority: low
ms.prod: windows-server
ms.openlocfilehash: 4995ed433d34fddfa91548fa42d67eea3a319c1f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357353"
---
# <a name="protect-your-hyper-v-virtual-machines-with-azure-site-recovery-and-windows-admin-center"></a>Proteger el Virtual Machines de Hyper-V con Azure Site Recovery y el centro de administración de Windows

>Se aplica a: Vista previa del centro de administración de Windows, centro de administración de Windows

[Más información acerca de la integración de Azure con el centro de administración de Windows.](../plan/azure-integration-options.md)

Windows Admin Center simplifica el proceso de replicación de las máquinas virtuales en los servidores o clústeres Hyper-V, lo que facilita e aprovechamiento de la potencia de Azure desde tu propio centro de datos. Para automatizar la instalación, puedes conectar la puerta de enlace de Windows Admin Center a Azure.

Usa la siguiente información para configurar las opciones de replicación y crear un plan de recuperación desde el portal de Azure, habilitar Windows Admin Center para iniciar la replicación de la VM y proteger sus VM.

## <a name="what-is-azure-site-recovery-and-how-does-it-work-with-windows-admin-center"></a>¿Qué es Azure Site Recovery y cómo funciona con Windows Admin Center? 

**Azure Site Recovery** es un servicio de Azure que replica las cargas de trabajo que se ejecutan en VM para que su infraestructura empresarial crítica esté protegido en caso de un desastre.  [Obtén más información acerca de Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview).

Azure Site Recovery consta de dos componentes: **replicación** y **conmutación por error**. La parte de replicación protege las VM en caso de desastre mediante la replicación de VHD de CM de destino a una cuenta de Azure Storage. Puedes, a continuación, conmutar por error estas VM y ejecutarlas en Azure en caso de desastre. También puedes realizar una prueba de conmutación por error sin que afecte a las VM principales para probar el proceso de recuperación en Azure.

Completar la instalación para el componente de replicación por sí solo es suficiente para proteger la VM en caso de desastre. Sin embargo, no podrás iniciar la VM en Azure hasta que configures la parte de la conmutación por error. Puedes configurar la parte de la conmutación por error cuando desees la conmutación por error para una VM de Azure; no es necesario como parte de la instalación inicial. Si el servidor host deja de funcionar y todavía no has configurado el componente de conmutación por error, puedes configurarlo en ese momento y tener acceso a las cargas de trabajo de la VM protegida. Sin embargo, es una buena práctica configurar la configuración relacionada con la conmutación por error antes de un desastre.
 

## <a name="prerequisites-and-planning"></a>Requisitos previos y planificación

- Los servidores de destino que hospedan las VM que desees proteger deben tener acceso a Internet para replicar en Azure.
- [Conecta la puerta de enlace de Windows Admin Center a Azure](azure-integration.md).
- [Revisa la herramienta de planificación de capacidad para evaluar los requisitos para la replicación y la conmutación por error correctas](https://docs.microsoft.com/azure/site-recovery/hyper-v-site-walkthrough-capacity).

## <a name="step-1-set-up-vm-protection-on-your-target-host"></a>Paso 1: Configuración de la protección de máquinas virtuales en el host de destino

> [!NOTE] 
> Debes realizar este paso una vez por cada clúster o servidor de hosts que contenga VM elegidas para protección.

1. Navega hasta el servidor o clúster que aloja las VM que desea proteger (ya sea con el Administrador de servidores o el Administrador de clústeres hiperconvergidos).
2. Ve a **Máquinas virtuales** > **Inventario**.
3. Selecciona cualquier VM (no tiene que ser la VM que deseas proteger).
4. Selecciona **Más** > **Configurar la protección de VM**.
5. Inicia sesión en tu cuenta de Azure.
6. Escribe la información necesaria:

   - **Suscribi** La suscripción de Azure que quiere usar para la replicación de máquinas virtuales en este host.
   - **Ubicación:** La región de Azure en la que se deben crear los recursos de ASR.
   - **Cuenta de almacenamiento:** La cuenta de almacenamiento donde se guardarán las cargas de trabajo de máquinas virtuales replicadas en este host.
   - **Almacén** Elija un nombre para el almacén de Azure Site Recovery para las máquinas virtuales protegidas en este host.

7. Selecciona **Instalar ASR**.
8. Espere hasta que vea la notificación: **Site Recovery configuración completada**.
 
Esto puede tardar hasta 10 minutos. Puedes ver el progreso yendo a **Notificaciones** (el icono de campana en la parte superior derecha).

>[!NOTE]
> Automáticamente, en este paso se instala el agente de ASR en el servidor o nodos de destino (si se está configurando un clúster), crea un **Grupo de recursos** con la **Cuenta de almacenamiento** y el **Almacén** especificados, en la **Ubicación** especificada. Esto también registrará el host de destino con el servicio de ASR y configurará una directiva de replicación predeterminada.

## <a name="step-2-select-virtual-machines-to-protect"></a>Paso 2: Seleccionar las máquinas virtuales que se van a proteger

1. Navega hasta el servidor o el clúster configurado en el paso 2 anterior y ve a **Máquinas virtuales > Inventario**.
2. Selecciona la máquina virtual que deseas proteger.
3. Selecciona **Más** > **Proteger VM**.
4. Revisa los [requisitos de capacidad para proteger la VM](https://docs.microsoft.com/azure/site-recovery/site-recovery-capacity-planner).

    Si deseas usar una cuenta de almacenamiento premium, [crea una en el portal de Azure](https://docs.microsoft.com/azure/storage/common/storage-premium-storage). La opción **Crear nueva** proporcionada en el panel de Windows Admin Center crea una cuenta de almacenamiento estándar.

5. Escribe el nombre de la **Cuenta de almacenamiento** que se usará para la replicación de la VM y selecciona **Proteger VM**. Este paso permite la replicación de la máquina virtual seleccionada. 

6. ASR iniciará la replicación. La replicación se completa y la VM se protege cuando el valor de la columna **Protegido** de la cuadrícula de **Inventario de máquina Virtual** en **Sí**. Esto puede tardar varios minutos.  

## <a name="step-3-configure-and-run-a-test-failover-in-the-azure-portal"></a>Paso 3: Configure y ejecute una conmutación por error de prueba en el Azure Portal

 Aunque no es necesario completar este paso al iniciar una replicación de máquina virtual (la máquina virtual ya estará protegida con replicación), se recomienda configurar la configuración de conmutación por error al configurar Azure Site Recovery. Si deseas preparar para conmutación por error en una VM de Azure, completa los siguientes pasos:

1. [Configura una red de Azure](https://docs.microsoft.com/azure/site-recovery/hyper-v-site-walkthrough-prepare-azure), la VM conmutado por error se unirá a esta VNET. Ten en cuenta que el resto de los pasos que aparecen en la página vinculada los completa automáticamente Windows Admin Center; solo debes instalar la red de Azure.

2. [Ejecuta una prueba de conmutación por error](https://docs.microsoft.com/azure/site-recovery/hyper-v-site-walkthrough-test-failover).

## <a name="step-4-create-recovery-plans"></a>Paso 4: Crear planes de recuperación

**Plan de recuperación** es una característica de Azure Site Recovery que permite la conmutación por error y recuperar una aplicación completa que contiene una colección de VM. Aunque es posible recuperar VM protegidas de forma individual añadiendo las VM que contienen una aplicación para un plan de recuperación, podrás hacer la conmutación por error de toda la aplicación a través del plan de recuperación. También puedes usar la característica de conmutación por error del Plan de recuperación para probar la recuperación de la aplicación. El plan de recuperación permite a las VM de grupo, secuenciar el orden en el que deben llevarse durante una conmutación por error y automatizar pasos adicionales para que se realicen como parte del proceso de recuperación. Una vez protegidas las VM, puedes ir al almacén de Azure Site Recovery en el portal de Azure y crear planes de recuperación para estas VM. [Obtén más información acerca de los planes de recuperación](https://docs.microsoft.com/azure/site-recovery/site-recovery-create-recovery-plans).

## <a name="monitoring-replicated-vms-in-azure"></a>Supervisión de VM replicadas en Azure ##

Para comprobar que no hay ningún error en el registro del servidor, ve al **portal de Azure** > **Todos los recursos** > **Almacén de servicios de recuperación** (el especificado en el paso 2) > **Trabajos** > **Trabajos de recuperación del sitio**.

Puedes supervisar la replicación de MV yendo a **Cámara de servicios de recuperación** > **Elementos replicados**.

Para ver todos los servidores que están registrados a la cámara, ve a **Almacén de servicios de recuperación** > **Infraestructura de recuperación del sitio** > **Hosts Hyper-V** (en la sección Sitios Hyper-V).

## <a name="known-issue"></a>Problema conocido ##

Al registrar ASR con un clúster, si se produce un error en un nodo para instalar ASR o registrarse en el servicio de ASR, es posible que las VM no estén protegidos. Comprueba que todos los nodos del clúster estén registrados en el portal de Azure yendo a **Almacén de servicios de recuperación** > **Trabajos** > **Trabajos de recuperación del sitio**.