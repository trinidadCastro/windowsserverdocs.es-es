---
title: Ajuste del tamaño de la máquina virtual
description: Recomendaciones de tamaño para cada tipo de carga de trabajo.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.author: helohr
ms.date: 12/02/2019
ms.topic: article
author: Heidilohr
manager: lizross
ms.openlocfilehash: 1d6a7daa3966488c951117b083411587d13be56b
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2020
ms.locfileid: "80857108"
---
# <a name="virtual-machine-sizing-guidance"></a>Guía de ajuste del tamaño de la máquina virtual

Tanto si estás ejecutando la máquina virtual en Servicios de Escritorio remoto como en Windows Virtual Desktop, los distintos tipos de cargas de trabajo requieren diferentes configuraciones de máquina virtual (VM) del host de sesión. Para obtener la mejor experiencia posible, escala la implementación en función de las necesidades de los usuarios.

## <a name="multi-session-recommendations"></a>Recomendaciones para varias sesiones

En la tabla siguiente se indica el número máximo sugerido de usuarios por unidad central de procesamiento virtual (vCPU) y la configuración de VM mínima para cada carga de trabajo. Estas recomendaciones se basan en las [cargas de trabajo del Escritorio remoto](remote-desktop-workloads.md).

| Tipo de carga de trabajo | Número máximo de usuarios por vCPU | Mínimo de vCPU/RAM/almacenamiento de SO | Instancias de Azure de ejemplo | Almacenamiento mínimo del contenedor de perfiles |
| --- | --- | --- | --- | --- |
| Ligero | 6 | 2 vCPUs, 8 GB de RAM, 16 GB de almacenamiento | D2s_v3, F2s_v2 | 30 GB |
| Medio | 4 | 4 vCPUs, 16 GB de RAM, 32 GB de almacenamiento | D4s_v3, F4s_v2 | 30 GB |
| Pesado | 2 | 4 vCPUs, 16 GB de RAM, 32 GB de almacenamiento | D4s_v3, F4s_v2 | 30 GB |
| Potencia | 1 | 6 vCPUs, 56 GB de RAM, 340 GB de almacenamiento | D4s_v3, F4s_v2, NV6 | 30 GB |

## <a name="single-session-recommendations"></a>Recomendaciones para una única sesión

En el caso de las recomendaciones de tamaño de VM para escenarios de una única sesión, se recomiendan al menos dos núcleos de CPU físicos por VM (normalmente, cuatro vCPU con hyperthreading). Si necesitas recomendaciones de ajuste de tamaño de VM más específicas para escenarios de sesión única, pregunta a los proveedores de software específicos de tu carga de trabajo. El ajuste de tamaño de VM para VM de una sola sesión, probablemente, se alinee con las directrices del dispositivo físico.

## <a name="general-virtual-machine-recommendations"></a>Recomendaciones sobre máquinas virtuales generales

Para conocer los requisitos de VM para ejecutar el sistema operativo, consulta [Cómo buscar las especificaciones del equipo y los requisitos del sistema para Windows 10](https://www.microsoft.com/windows/windows-10-specifications).

Se recomienda usar almacenamiento SSD Premium en el disco del sistema operativo para las cargas de trabajo de producción que requieran un contrato de nivel de servicio (SLA). Para obtener más información, consulta el [SLA para máquinas virtuales](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_8/).

Las unidades de procesamiento gráfico (GPU) suelen ser una buena elección para los usuarios que usan programas con muchos gráficos para representar vídeo, diseño 3D y simulaciones. Para obtener más información sobre la aceleración de gráficos, consulta [Elección de la tecnología de representación de gráficos](rds-graphics-virtualization.md). Azure tiene disponibles varias opciones de implementación de aceleración de gráficos y varios tamaños de máquina virtual de GPU. Más información en [Tamaños de máquinas virtuales optimizadas para GPU](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-gpu).

Las [VM ampliables de serie B](https://docs.microsoft.com/azure/virtual-machines/windows/b-series-burstable) son una buena elección para los usuarios que no siempre necesitan un rendimiento máximo de la CPU. Para obtener más información sobre los tipos y tamaños de VM, consulta [Tamaños de las máquinas virtuales Windows en Azure](https://docs.microsoft.com/azure/virtual-machines/windows/sizes) y la información de precios en [nuestra página de series de máquinas virtuales](https://azure.microsoft.com/pricing/details/virtual-machines/series/).

## <a name="test-your-workload"></a>Prueba de la carga de trabajo

Por último, se recomienda usar herramientas de simulación para probar la implementación con las pruebas de esfuerzo y las simulaciones de uso real. Asegúrate de que el sistema responde y de que es lo suficientemente resistente como para satisfacer las necesidades del usuario, y recuerda cambiar el tamaño de la carga para evitar sorpresas.
