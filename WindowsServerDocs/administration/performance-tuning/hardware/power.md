---
title: Consideraciones sobre la energía de hardware de Windows Server
description: Consideraciones acerca de la energía de hardware de Windows Server.
ms.topic: conceptual
ms.author: qizha
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: fe316dd1f21d3f5e151cef60f63c3644ad1af06d
ms.sourcegitcommit: 7cacfc38982c6006bee4eb756bcda353c4d3dd75
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/14/2020
ms.locfileid: "90077637"
---
# <a name="server-hardware-power-considerations"></a>Server Hardware Power Considerations (Consideraciones de alimentación del hardware de servidor)

Es importante reconocer la importancia creciente de la eficacia energética en entornos empresariales y centros de datos. El uso de alto rendimiento y bajo consumo de energía suelen ser objetivos conflictivos, pero al seleccionar cuidadosamente los componentes de servidor, puede lograr el equilibrio correcto entre ellos. En las secciones siguientes se enumeran las directrices para las características y capacidades de energía de los componentes de hardware de servidor.

## <a name="processor-recommendations"></a>Recomendaciones de procesador

La frecuencia, el voltaje operativo, el tamaño de la caché y la tecnología de proceso afectan al consumo energético de los procesadores. Los procesadores tienen una clasificación de punto de diseño térmico (TDP) que proporciona una indicación básica del consumo energético con respecto a otros modelos.

En general, opte por el procesador de TDP más bajo que se adaptará a sus objetivos de rendimiento. Además, las nuevas generaciones de procesadores suelen ser más eficaces y pueden exponer más Estados de energía para los algoritmos de administración de energía de Windows, lo que permite una mejor administración de energía en todos los niveles de rendimiento. O bien, pueden usar algunas de las nuevas técnicas de administración de energía "cooperativa" que Microsoft ha desarrollado en colaboración con los fabricantes de hardware.

Para obtener más información sobre las técnicas de administración de energía cooperativa, consulte la sección sobre el control de rendimiento de Collaborative Processor en la [especificación Advanced Configuration and Power Interface](http://www.uefi.org/sites/default/files/resources/ACPI_5_1release.pdf).

## <a name="memory-recommendations"></a>Recomendaciones de memoria

Cuentas de memoria para una fracción cada vez mayor de la potencia total del sistema. Muchos factores afectan al consumo de energía de un DIMM de memoria, como la tecnología de memoria, el código de corrección de errores (ECC), la frecuencia de bus, la capacidad, la densidad y el número de rangos. Por lo tanto, es mejor comparar las clasificaciones de energía esperadas antes de adquirir grandes cantidades de memoria.

La memoria de bajo consumo ya está disponible, pero debe tener en cuenta las ventajas y los costos de rendimiento. Si el servidor va a paginar, también debe tener en cuentan el coste energético de los discos de paginación.

## <a name="disks-recommendations"></a>Recomendaciones de discos

Una mayor cantidad de RPM significa un aumento del consumo energético. Las unidades SSD son más eficientes en cuanto a las unidades de rotación. Además, las unidades de 2,5 pulgadas generalmente requieren menos energía que las unidades de 3,5 pulgadas.

## <a name="network-and-storage-adapter-recommendations"></a>Recomendaciones de adaptador de almacenamiento y red

Algunos adaptadores reducen el consumo de energía durante los períodos de inactividad. Esta es una consideración importante para los adaptadores de red de 10 GB y los vínculos de almacenamiento de gran ancho de banda (4-8 GB). Estos dispositivos pueden consumir grandes cantidades de energía.

## <a name="power-supply-recommendations"></a>Recomendaciones de la fuente de alimentación

Mejorar la eficacia de la fuente de alimentación es una excelente manera de reducir el consumo energético sin afectar al rendimiento. Las fuentes de alimentación de alta eficiencia pueden ahorrar muchos kilovatios-horas al año, por servidor.

## <a name="fan-recommendations"></a>Recomendaciones de ventilador

Los aficionados, como las fuentes de alimentación, son un área en la que puede reducir el consumo energético sin que ello afecte al rendimiento del sistema. Los ventiladores de velocidad variable pueden reducir RPM a medida que disminuye la carga del sistema, lo que elimina el consumo energético innecesario.

## <a name="usb-devices-recommendations"></a>Recomendaciones de dispositivos USB

Windows Server 2016 habilita la suspensión selectiva para dispositivos USB de forma predeterminada. Sin embargo, un controlador de dispositivo mal escrito puede seguir interrumpiendo la eficacia energética del sistema en un margen mayor. Para evitar posibles problemas, desconecte los dispositivos USB, deshabilítelo en el BIOS o elija los servidores que no requieran dispositivos USB.

## <a name="remotely-managed-power-strip-recommendations"></a>Recomendaciones de la franja de energía administrada de forma remota

Las franjas eléctricas no son una parte integral del hardware de servidor, pero pueden hacer una gran diferencia en el centro de datos. Las medidas muestran que los servidores de volúmenes que están conectados, pero que se han desconectado de aparentemente formen, pueden requerir hasta 30 vatios de energía.

Para evitar la pérdida de electricidad, puede implementar una franja de energía administrada de forma remota para cada bastidor de servidores con el fin de desconectar mediante programación la alimentación de servidores específicos.

## <a name="processor-terminology"></a>Terminología del procesador

La terminología del procesador que se usa en este tema refleja la jerarquía de los componentes disponibles en la siguiente ilustración. Los términos que se usan de mayor a menor granularidad de los componentes son los siguientes:

- Socket de procesador
- nodo NUMA
- Core
- Procesador lógico

![terminología del procesador](../media/perftune-guide-figure-1.png)

## <a name="additional-references"></a>Referencias adicionales

- [Consideraciones de rendimiento del hardware de servidor](index.md)

- [Power and Performance Tuning](power/power-performance-tuning.md) (Optimización de potencia y rendimiento)

- [Processor Power Management Tuning](power/processor-power-management-tuning.md) (Optimización de la administración de energía del procesador)

- [Recommended Balanced Plan Parameters](power/recommended-balanced-plan-parameters.md) (Parámetros recomendados del plan equilibrado)
