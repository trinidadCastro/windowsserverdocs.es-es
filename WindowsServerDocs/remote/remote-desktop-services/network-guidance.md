---
title: Directrices de red
description: Recomendaciones de ancho de banda para implementaciones de Escritorio remoto.
ms.author: helohr
ms.date: 12/12/2019
ms.topic: article
author: Heidilohr
manager: lizross
ms.openlocfilehash: 3817178ef303573490cc98af8a35844715286e8e
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87948899"
---
# <a name="network-guidelines"></a>Directrices de red

Cuando se usa una sesión remota de Windows, el ancho de banda disponible de la red afecta en gran medida a la calidad de la experiencia. Las diferentes aplicaciones y resoluciones de pantalla requieren configuraciones de red diferentes, por lo que es importante asegurarse de que la red está configurada para satisfacer tus necesidades.

>[!NOTE]
>Las siguientes recomendaciones se aplican a redes con menos de 0,1 % de pérdida. Estas recomendaciones se aplican independientemente del número de sesiones que hospedes en tus máquinas virtuales (VM).

## <a name="applications"></a>Aplicaciones

En la tabla siguiente se enumeran los anchos de banda mínimos recomendados para una experiencia de usuario fluida. Estas recomendaciones se basan en las instrucciones de [Cargas de trabajo de Escritorio remoto](remote-desktop-workloads.md).

| Tipo de carga de trabajo   | Ancho de banda recomendado |
|-----------------|-----------------------|
| Ligero           | 1,5 Mbps              |
| Medio          | 3 Mbps                |
| Pesado           | 5 Mbps                |
| Potencia           | 15 Mbps               |

Ten en cuenta que el estrés que se aplica a la red depende de la resolución de pantalla y la velocidad de fotogramas de salida de la carga de trabajo de la aplicación. Si la velocidad de fotogramas o la resolución de pantalla aumentan, el requisito de ancho de banda también lo hará. Por ejemplo, una carga de trabajo ligera con una pantalla de alta resolución requiere más ancho de banda disponible que una carga de trabajo ligera con una resolución normal o baja.

Otros escenarios pueden cambiar sus requisitos de ancho de banda en función de cómo los uses, por ejemplo:

- Conferencias de audio o vídeo
- Comunicación en tiempo real
- Streaming de vídeo 4K

Asegúrate de realizar pruebas de carga de estos escenarios en tu implementación mediante herramientas de simulación como Login VSI. Modifica el tamaño de carga, ejecuta pruebas de esfuerzo y prueba escenarios de usuario comunes en sesiones remotas para comprender mejor los requisitos de la red.

## <a name="display-resolutions"></a>Resoluciones de pantalla

Diferentes resoluciones de pantalla requieren diferentes anchos de banda disponibles. En la tabla siguiente se enumeran los anchos de banda recomendados para lograr una experiencia de usuario fluida con resoluciones de pantalla típicas y una velocidad de fotogramas de 30 fotogramas por segundo (FPS). Estas recomendaciones se aplican a escenarios de uno y varios usuarios. Ten en cuenta que los escenarios que implican una velocidad de fotogramas inferior a 30 FPS, como la lectura de texto estático, requieren menos ancho de banda disponible.

| Resoluciones de pantalla típicas a 30 FPS    | Ancho de banda recomendado |
|------------------------------------------|-----------------------|
| Aproximadamente 1024 × 768 px                      | 1,5 Mbps              |
| Aproximadamente 1280 × 720 px                      | 3 Mbps                |
| Aproximadamente 1920 × 1080 px                     | 5 Mbps                |
| Aproximadamente 3840 × 2160 px (4K)                | 15 Mbps               |

## <a name="windows-virtual-desktop-experience-estimator"></a>Estimador de experiencia de Windows Virtual Desktop

La región de Azure en la que te encuentres puede afectar a la experiencia de usuario tanto como las condiciones de la red. Para obtener más información, consulta [Estimador de experiencia de Windows Virtual Desktop](https://azure.microsoft.com/services/virtual-desktop/assessment/).

## <a name="assistive-technologies"></a>Tecnologías de asistencia

Las cargas de trabajo de tecnología de asistencia, como el uso del Narrador en la sesión remota, requieren conexiones con un tiempo de ida y vuelta (RTT) de conexión de 20 milisegundos (ms) o superior a fin de obtener la mejor experiencia de usuario.
