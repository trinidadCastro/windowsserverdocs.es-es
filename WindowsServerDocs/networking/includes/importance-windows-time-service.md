---
author: shortpatti
ms.author: pashort
ms.date: 10/02/2018
ms.prod: windows-server-threshold
ms:topic: include
ms.openlocfilehash: 4e8e3234b89630bf16148eef644f0c6607ad38bd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852346"
---
## <a name="importance-of-time-protocols"></a>Importancia de los protocolos de tiempo
Protocolos de tiempo que se comunican entre dos equipos para intercambiar información de tiempo y, a continuación, usar esa información para sincronizar sus relojes. Con el protocolo de tiempo de servicio de hora de Windows, un cliente solicita información de hora de un servidor y sincroniza su reloj según la información que se recibe.
  
El servicio de hora de Windows usa NTP para sincronizar la hora a través de una red. NTP es un protocolo de tiempo de Internet que incluye los necesarios para sincronizar los relojes de los algoritmos de disciplina. NTP es un protocolo de tiempo más preciso que el protocolo de tiempo de red Simple (SNTP) que se utiliza en algunas versiones de Windows; Sin embargo, W32Time sigue admitiendo SNTP para habilitar la compatibilidad con versiones anteriores con los equipos que ejecutan servicios de tiempo basada en SNTP como Windows 2000.

Hay muchas razones diferentes, que podría necesitar tiempo precisa.  El caso típico de Windows es Kerberos, lo que requiere de 5 minutos de precisión entre el cliente y servidor.  Sin embargo, hay muchas otras áreas que pueden verse afectados mediante la inclusión de precisión de tiempo:


- Regulaciones gubernamentales, como:
    - precisión de 50 ms para FINRA en Estados Unidos
    - 1 ESMA de ms (MiFID II) en la Unión Europea.
- Algoritmos de criptografía
- Sistemas distribuidos como clúster/Exchange/SQL y bases de datos de documento
- Marco de la cadena de bloques para las transacciones de bitcoin
- Registros distribuidos y análisis de amenazas 
- Replicación de AD
- PCI (Payment Card Industry), precisión segunda actualmente 1