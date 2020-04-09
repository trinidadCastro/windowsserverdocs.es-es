---
title: Optimización del rendimiento para la memoria caché y los subsistemas de administración de memoria
description: Optimización del rendimiento para la memoria caché y los subsistemas de administración de memoria
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: landing-page
ms.author: pavel; atales
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: d9b43b418f2f2ddee0e5b064c4a4e2b5d19a4520
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851678"
---
# <a name="performance-tuning-cache-and-memory-manager"></a>Optimización del rendimiento de la memoria caché y el Administrador de memoria

De forma predeterminada, Windows almacena en caché los datos de archivo que se leen de discos y se escriben en discos. Esto implica que las operaciones de lectura leen datos de archivos de un área de la memoria del sistema conocida como caché de archivos del sistema, en lugar de hacelo desde el disco físico. En consecuencia, las operaciones de escritura escriben datos de archivo en la memoria caché de archivos, en lugar de en el disco. Este tipo de caché se conoce como caché de reescritura. El almacenamiento en caché se administra por objeto de archivo. El almacenamiento en caché se produce bajo la dirección del Administrador de caché, que funciona continuamente mientras se ejecuta Windows.

Los datos de archivo de la memoria caché de archivos del sistema se escriben en el disco a intervalos definidos por el sistema operativo. Las páginas vaciadas permanecen en el espacio de trabajo de caché del sistema (si se ha definido FILE\_FLAG\_RANDOM\_ACCESS y el identificador de archivos no se ha cerrado) o en la lista de espera, donde se convierten en parte de la memoria disponible.

La directiva de retrasar la escritura de los datos en el archivo y conservarlos en la memoria caché hasta que esta se vacíe se denomina escritura diferida y la desencadena el Administrador de caché en un intervalo de tiempo determinado. La hora a la que se vacía un bloque de datos de archivo se basa, parcialmente, en la cantidad de tiempo que ha estado almacenado en la memoria caché y la cantidad de tiempo desde la última vez que se accedió a los datos en una operación de lectura. Esto garantiza que los datos de archivo que se suelen leer con frecuencia seguirán accesibles en la caché de archivos del sistema durante la cantidad máxima de tiempo.

Este proceso de almacenamiento en caché de datos de archivo se ilustra en la figura siguiente:

![almacenamiento en caché de datos de archivo](../../media/perftune-guide-file-data-caching.png)

Como muestran las flechas sólidas de la figura anterior, una región de 256 KB de datos se lee en una ranura de memoria caché de 256 KB en el espacio de direcciones del sistema cuando así lo solicita el Administrador de caché durante un operación de lectura de archivo. A continuación, un proceso en modo de usuario copia los datos de esta ranura en su propio espacio de direcciones. Cuando el proceso ha finalizado su acceso a los datos, escribe los datos modificados en la misma ranura de la memoria caché del sistema, como se muestra mediante la flecha de puntos entre el espacio de direcciones del proceso y la memoria caché del sistema. Cuando el Administrador de caché determina que los datos ya no se necesitarán durante un periodo de tiempo concreto, escribe los datos modificados en el archivo del disco, como se muestra mediante la flecha de puntos entre la memoria caché del sistema y el disco.

**En esta sección:**

-   [Memoria caché y problemas de rendimiento potenciales del Administrador de memoria](troubleshoot.md)

-   [Mejoras en la memoria caché y el Administrador de memoria en Windows Server 2016](improvements-in-2016.md)
