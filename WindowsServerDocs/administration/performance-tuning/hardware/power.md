---
title: Consideraciones sobre la alimentación de Hardware de servidor
description: Consideraciones sobre la alimentación de Hardware de servidor
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Qizha;TristanB
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 5261c856a0a29f9f58526e4f9580a16bbed5be56
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874306"
---
# <a name="server-hardware-power-considerations"></a>Consideraciones sobre la alimentación de Hardware de servidor

Es importante tener en cuenta la importancia creciente de eficiencia energética en entornos de centros de datos y enterprise. Uso de baja energía y alto rendimiento a menudo son objetivos en conflicto, pero seleccionando cuidadosamente los componentes de servidor, puede lograr el equilibrio correcto entre ellos. Las secciones siguientes se enumeran las directrices para las características de energía y capacidades de los componentes de hardware de servidor.

## <a name="processor-recommendations"></a>Recomendaciones de procesador

Frecuencia de funcionamiento de tensión, tamaño de caché y la tecnología de proceso afecta al consumo de energía de procesadores. Procesadores tienen un diseño térmico punto clasificación (TDP) que proporciona una indicación básica de consumo de energía en relación con otros modelos.

En general, opte por el procesador TDP más bajo que cumpla sus objetivos de rendimiento. Además, las últimas generaciones de procesadores son energía suele ser más eficaz, y pueden exponer más Estados de energía para los algoritmos de administración de energía de Windows, lo que permite una mejor administración de energía en todos los niveles de rendimiento. ¿O pueden usar algunas de la nuevas "cooperativa? técnicas de administración de energía que Microsoft ha desarrollado en colaboración con fabricantes de hardware.

Para obtener más información sobre las técnicas de administración de energía cooperativa, consulte la sección denominada Control de rendimiento de procesador de colaboración en el [avanzada de configuración y especificación de interfaz de energía](http://www.uefi.org/sites/default/files/resources/ACPI_5_1release.pdf).


## <a name="memory-recommendations"></a>Recomendaciones de memoria
Cuentas de memoria para una creciente fracción de la capacidad total del sistema. Muchos factores afectan el consumo de energía de una memoria DIMM, como la tecnología de memoria, código de corrección de errores (ECC), frecuencia de bus, capacidad, densidad y número de rangos. Por lo tanto, es mejor comparar las clasificaciones de alimentación esperado antes de adquirir grandes cantidades de memoria.

Memoria de bajo consumo de energía ya está disponible, pero debe tener en cuenta las ventajas y desventajas de rendimiento y costo. Si el servidor se se pagina, también debe tener en cuenta el coste de energía de los discos de paginación.


## <a name="disks-recommendations"></a>Recomendaciones de discos
RPM mayor significa que el consumo de energía mayor. Las unidades SSD son más energía eficaces que las unidades de rotación. Además, las unidades de 2,5 pulgadas generalmente requieren menos energía que las unidades de 3,5 pulgadas.

## <a name="network-and-storage-adapter-recommendations"></a>Recomendaciones del adaptador de almacenamiento y de red
Algunos adaptadores disminuya el consumo de energía durante los períodos de inactividad. Ésta es una consideración importante para los adaptadores de red de 10 Gb y los vínculos de almacenamiento de alto ancho de banda (4 a 8 Gb). Estos dispositivos pueden consumir grandes cantidades de energía.


## <a name="power-supply-recommendations"></a>Recomendaciones de suministro de energía
Mejorar la eficacia de suministro de energía es una excelente manera de reducir el consumo de energía sin afectar al rendimiento. Fuentes de alimentación de alta eficacia pueden ahorrar muchas kilovatios al año por servidor.


## <a name="fan-recommendations"></a>Recomendaciones del ventilador
Ventiladores, como fuentes de alimentación, son un área donde puede reducir el consumo de energía sin afectar al rendimiento del sistema. Ventiladores de velocidad variable pueden reducir RPM como el sistema carga disminuye, eliminando el consumo de energía innecesarias en caso contrario.


## <a name="usb-devices-recommendations"></a>Recomendaciones de los dispositivos USB
Windows Server 2016 habilita la suspensión selectiva para dispositivos USB de forma predeterminada. Sin embargo, un controlador de dispositivo mal escrita puede interrumpir la eficiencia de energía del sistema por un margen considerable. Para evitar posibles problemas, desconecte los dispositivos USB, deshabilitarlos en el BIOS o elegir los servidores que no requieren dispositivos USB.


## <a name="remotely-managed-power-strip-recommendations"></a>Recomendaciones de franja Power administrados de forma remota
Toma de corriente no es una parte integral del hardware del servidor, pero pueden suponer una diferencia sustancial en el centro de datos. Las mediciones muestran que los servidores de volumen que están conectados, pero han sido presumiblemente apagados, todavía pueden requerir hasta 30 vatios de energía.

Para evitar desperdiciar electricidad, puede implementar una regleta administrado de forma remota para cada bastidor de servidores a fin de desconectar mediante programación servidores específicos.

## <a name="processor-terminology"></a>Terminología de procesador
La terminología de procesador utilizada en este tema refleja la jerarquía de componentes disponibles en la ilustración siguiente. Los términos usados de mayor a menor granularidad de los componentes son los siguientes:

-   Socket de procesador
-   Nodo NUMA
-   Core
-   procesador lógico

![terminología de procesador](../media/perftune-guide-figure-1.png)

## <a name="see-also"></a>Vea también
- [Consideraciones de rendimiento del Hardware de servidor](index.md)
- [Energía y ajuste del rendimiento](power/power-performance-tuning.md)
- [Optimización de la administración de energía del procesador](power/processor-power-management-tuning.md)
- [Parámetros de Plan equilibrado recomendados](power/recommended-balanced-plan-parameters.md)
