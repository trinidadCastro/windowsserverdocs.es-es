---
title: Optimización del rendimiento de contenedores de Windows Server
description: Recomendaciones para la optimización del rendimiento de los contenedores en Windows Server 16
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: landing-page
ms.author: davso; ericam; yashi
author: akino
ms.date: 10/16/2017
ms.openlocfilehash: a4508e28e54562748422b198f703e23326d15720
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851638"
---
# <a name="performance-tuning-windows-server-containers"></a>Optimización del rendimiento de contenedores de Windows Server

## <a name="introduction"></a>Introducción
Windows Server 2016 es la primera versión de Windows para incluir la compatibilidad con la tecnología de contenedores integrada en el sistema operativo. En Server 2016 hay dos tipos de contenedores disponibles: Contenedores de Windows Server y contenedores de Hyper-V. Cada tipo de contenedor admite el SKU de Server Core o de Nano Server de Windows Server 2016. 

Estas configuraciones tienen distintas implicaciones de rendimiento que se detallarán a continuación para que comprenda cuál es la adecuada para sus escenarios. Además, detallaremos las configuraciones que afectan el rendimiento y describiremos las ventajas y desventajas de cada una de esas opciones.

### <a name="windows-server-container-and-hyper-v-containers"></a>Contenedores de Windows Server y contenedores y Hyper-V

Los contenedores de Windows Server y los contenedores de Hyper-V ofrecen muchas de las mismas ventajas de portabilidad y coherencia, pero difieren en términos de las garantías de aislamiento y las características de rendimiento.

**Contenedores de Windows Server** proporcionan aislamiento de aplicaciones mediante tecnología de aislamiento de procesos y espacios de nombres. Un contenedor de Windows Server comparte el kernel con el host de contenedor y todos los contenedores que se ejecutan en el host.

**Contenedores de Hyper-V** amplían el aislamiento que ofrecen los contenedores de Windows Server mediante la ejecución de cada contenedor en una máquina virtual altamente optimizada. En esta configuración, el kernel del host de contenedor no se comparte con los contenedores de Hyper-V.

El aislamiento adicional que proporcionan los contenedores de Hyper-V se logra en gran parte gracias a una capa de aislamiento de hipervisor entre el contenedor y el host de contenedor. Esto afecta la densidad del contenedor ya que, a diferencia de lo que ocurre con los contenedores de Windows Server, el uso compartido de archivos binarios y archivos del sistema puede disminuir, lo que genera una superficie de memoria y almacenamiento total mayor. Además, existe la sobrecarga adicional esperada en algunas rutas de CPU, E/S de almacenamiento y red.

### <a name="nano-server-and-server-core"></a>Nano Server y Server Core

Los contenedores de Windows Server y los de Hyper-V ofrecen compatibilidad con Server Core y con una opción de instalación nueva disponible en Windows Server 2016: [Nano Server](https://technet.microsoft.com/windows-server-docs/compute/nano-server/getting-started-with-nano-server). 

Nano Server es un sistema operativo de servidor administrado de forma remota y optimizado para centros de datos y nubes privadas. Es similar a Windows Server en modo Server Core, pero mucho más pequeño; no tiene ninguna capacidad de inicio de sesión local y solo es compatible con agentes, herramientas y aplicaciones de 64 bits. Ocupa mucho menos espacio en disco y se inicia más rápido.

## <a name="container-start-up-time"></a>Tiempo de inicio del contenedor
El tiempo de inicio del contenedor es una métrica clave en muchos de los escenarios que los contenedores ofrecen como la mayor ventaja. Por lo tanto, resulta fundamental entender cómo optimizar de mejor manera el tiempo de inicio del contenedor. A continuación se muestran algunas ventajas y desventajas que hay que entender para mejorar el tiempo de inicio.

### <a name="first-logon"></a>Primer inicio de sesión

Microsoft incluye una imagen base de Nano Server y de Server Core. La imagen base que se incluye para Server Core se optimizó al quitar la sobrecarga del tiempo de inicio asociada con el primer inicio de sesión (configuración rápida). Esto no ocurre con la imagen base de Nano Server. Sin embargo, este costo se puede quitar de las imágenes base de Nano Server si se confirma al menos una capa en la imagen del contenedor. Los inicios subsiguientes del contenedor a partir de la imagen no incurrirán en el costo de primer inicio de sesión.
### <a name="scratch-space-location"></a>Ubicación del espacio de desecho

De manera predeterminada, los contenedores usan un espacio de desecho temporal en el medio de la unidad de sistema del host de contenedor para el almacenamiento durante la vigencia del contenedor en ejecución. Este espacio actúa como la unidad de sistema del contenedor y, como tal, muchas de las escrituras y lecturas que se realizan en la operación del contenedor siguen esta ruta. En el caso de sistemas host donde la unidad de sistema existe en medios magnéticos de disco giratorio (HDD) pero hay disponibles medios de almacenamiento más rápidos (SSD o HDD más rápidos), es posible migrar el espacio de desecho del contenedor a otra unidad. Esto se usa con el comando –g de dockerd. Este comando es global y afectará a todos los contenedores que se ejecutan en el sistema.

### <a name="nested-hyper-v-containers"></a>Contenedores de Hyper-V anidados
Hyper-V para Windows Server 2016 introduce la compatibilidad con hipervisor anidado. Es decir, ahora se puede ejecutar una máquina virtual desde dentro de una máquina virtual. Esto abre muchos escenarios útiles, pero también acentúa cierto impacto en el rendimiento que genera el hipervisor, porque hay dos niveles de hipervisores en ejecución sobre el host físico.

En el caso de los contenedores, esto tiene un impacto cuando se ejecuta un contenedor de Hyper-V dentro de una máquina virtual. Dado que un contenedor de Hyper-V ofrece aislamiento a través de una capa de hipervisor entre el contenedor mismo y el host de contenedor, cuando el host de contenedor es una máquina virtual basada en Hyper-V, hay una sobrecarga de rendimiento asociada en términos de tiempo de inicio del contenedor, E/S de almacenamiento, E/S y rendimiento de red y CPU.

## <a name="storage"></a>Almacenamiento
### <a name="mounted-data-volumes"></a>Volúmenes de datos montados

Los contenedores ofrecen la capacidad de usar la unidad de sistema del host de contenedor para el espacio de desecho del contenedor. Sin embargo, el espacio de desecho del contenedor tiene una vida útil igual a la del contenedor. Es decir, cuando el contenedor se detiene, el espacio de desecho y todos los datos asociados desaparecen.

Sin embargo, hay muchos escenarios en los que se desea que los datos persistan independientemente de la vida útil del contenedor. En estos casos, se permite montar volúmenes de datos del host de contenedor en el contenedor. En el caso de los contenedores de Windows Server, hay muy poca sobrecarga de la ruta de acceso de E/S asociada con los volúmenes de datos montados (cerca del rendimiento nativo). Sin embargo, cuando se montan volúmenes de datos en los contenedores de Hyper-V, sí hay alguna degradación del rendimiento de E/S en esa ruta de acceso. Además, este impacto se acentúa cuando se ejecutan contenedores de Hyper-V dentro de las máquinas virtuales.

### <a name="scratch-space"></a>Espacio de desecho

De manera predeterminada, tanto los contenedores de Windows Server como los de Hyper-V proporcionan un disco duro virtual dinámico de 20 GB para el espacio de desecho del contenedor. En ambos tipos de contenedor, el sistema operativo del contenedor ocupa una parte de ese espacio, lo que ocurre en cada contenedor que se inicia. Por lo tanto, es importante recordar que cada contenedor iniciado tiene cierto impacto en el almacenamiento y, dependiendo de la carga de trabajo, puede escribir hasta 20 GB de los medios de almacenamiento de respaldo. Hay que considerar esto al diseñar las configuraciones de almacenamiento de servidor.
(Es posible configurar el tamaño del espacio de desecho)

## <a name="networking"></a>Funciones de red
Los contenedores de Windows Server y de Hyper-V ofrecen una variedad de modos de red que se adaptan mejor a las necesidades de las distintas configuraciones de red. Cada una de estas opciones presenta sus propias características de rendimiento.

### <a name="windows-network-address-translation-winnat"></a>Traducción de direcciones de red de Windows (WinNAT)

Cada contenedor recibirá una dirección IP de un prefijo IP interno y privado (por ejemplo, 172.16.0.0/12). Se admite la asignación o el reenvío de puertos desde el host del contenedor a los puntos de conexión del contenedor. De manera predeterminada, Docker crea una red NAT cuando el dockerd se ejecuta por primera vez.

De estos tres modos, la configuración de NAT es la ruta de acceso de E/S de red más costosa, pero tiene la menor cantidad de configuración necesaria. 

Los contenedores de Windows Server usan una vNIC de host para conectarse al conmutador virtual. Los contenedores de Hyper-V usan una NIC de máquina virtual sintética (no expuesta a la máquina virtual de utilidad) para conectarse al conmutador virtual. Cuando los contenedores se comunican con la red externa, los paquetes se enrutan a través de WinNAT con las traducciones de direcciones aplicadas, lo que puede implicar una sobrecarga.

### <a name="transparent"></a>Transparente

Cada punto de conexión de contenedor está conectado directamente a la red física. Las direcciones IP de la red física se pueden asignar de manera estática o dinámica mediante un servidor DHCP externo.

El modo transparente es el menos costoso en términos de la ruta de acceso de E/S de red y los paquetes externos se pasan directamente al NIC virtual del contenedor, lo que brinda acceso directo a la red externa.

### <a name="l2-bridge"></a>Puente de nivel 2
Cada punto de conexión de contenedor estará en la misma subred IP que el host de contenedor. Las direcciones IP deben asignarse estáticamente desde el mismo prefijo que el host del contenedor. Todos los puntos de conexión de contenedor del host tendrán la misma dirección MAC debido a la traducción de direcciones de nivel 2.

El modo Puente de nivel 2 es más eficaz que el modo WinNAT, porque ofrece acceso directo a la red externa, pero menos eficaz que el modo Transparente, porque también introduce la traducción de direcciones MAC.




