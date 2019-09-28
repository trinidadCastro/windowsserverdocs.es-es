---
title: Descripción del cuórum de clúster y grupo
description: Descripción del Cuórum del clúster y del grupo, con ejemplos específicos para ver los detalles.
keywords: Espacios de almacenamiento directo, Cuórum, testigo, S2D, Quórum de clúster, Quórum de grupo, clúster, grupo
ms.prod: windows-server
ms.author: adagashe
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: adagashe
ms.date: 01/18/2019
ms.localizationpriority: medium
ms.openlocfilehash: 8950e9d09e3bd07dc02228c295ab223ead969ea6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366015"
---
# <a name="understanding-cluster-and-pool-quorum"></a>Descripción del cuórum de clúster y grupo

>Se aplica a: Windows Server 2019 y Windows Server 2016

Los [clústeres de conmutación por error de Windows Server](../../failover-clustering/failover-clustering-overview.md) proporcionan alta disponibilidad para las cargas de trabajo. Estos recursos se consideran de alta disponibilidad si los nodos que hospedan recursos están activados; sin embargo, el clúster requiere generalmente más de la mitad de los nodos que se están ejecutando, lo que se conoce como tener *quórum*.

Cuórum está diseñado para evitar escenarios *de división de cerebro* que pueden producirse cuando hay una partición en la red y los subconjuntos de nodos no se pueden comunicar entre sí. Esto puede hacer que ambos subconjuntos de nodos intenten poseer la carga de trabajo y escriban en el mismo disco, lo que puede provocar numerosos problemas. Sin embargo, esto se evita con el concepto de cuórum del clúster de conmutación por error que obliga a que solo uno de estos grupos de nodos continúe ejecutándose, por lo que solo uno de estos grupos permanecerá en línea.

Cuórum determina el número de errores que el clúster puede admitir mientras permanecen en línea. Cuórum está diseñado para controlar el escenario en el que hay un problema con la comunicación entre subconjuntos de nodos de clúster, de modo que varios servidores no intenten hospedar simultáneamente un grupo de recursos y escribir en el mismo disco al mismo tiempo. Al tener este concepto de cuórum, el clúster forzará que el servicio de clúster se detenga en uno de los subconjuntos de nodos para asegurarse de que solo hay un verdadero propietario de un grupo de recursos determinado. Una vez que los nodos detenidos pueden volver a comunicarse con el grupo principal de nodos, se volverán a unir automáticamente al clúster e iniciarán su servicio de Cluster Server.

En Windows Server 2019 y Windows Server 2016, hay dos componentes del sistema que tienen sus propios mecanismos de quórum:

- **Cuórum de clúster**: Esto funciona en el nivel de clúster (es decir, puede perder nodos y hacer que el clúster permanezca activo)
- **Cuórum de grupo**: Esto funciona en el nivel de grupo cuando Espacios de almacenamiento directo está habilitado (es decir, puede perder nodos y unidades y hacer que el grupo se mantenga activo). Los grupos de almacenamiento se diseñaron para usarse en escenarios agrupados y no agrupados, por lo que tienen un mecanismo de cuórum diferente.

## <a name="cluster-quorum-overview"></a>Información general de cuórum de clúster

En la tabla siguiente se proporciona información general sobre los resultados del Cuórum de clúster por escenario:

| Nodos de servidor | Puede sobrevivir a un error de nodo de servidor | Puede sobrevivir a un error de nodo de servidor, otro | Puede sobrevivir a dos errores simultáneos de nodo de servidor |
|--------------|-------------------------------------|---------------------------------------------------|----------------------------------------------------|
| 2            | 50/50                               | No                                                | No                                                 |
| 2 + testigo  | Sí                                 | No                                                | No                                                 |
| 3            | Sí                                 | 50/50                                             | No                                                 |
| 3 + testigo  | Sí                                 | Sí                                               | No                                                 |
| 4            | Sí                                 | Sí                                               | 50/50                                              |
| 4 testigos testigo  | Sí                                 | Sí                                               | Sí                                                |
| 5 y versiones posteriores  | Sí                                 | Sí                                               | Sí                                                |

### <a name="cluster-quorum-recommendations"></a>Recomendaciones de cuórum de clúster

- Si tiene dos nodos, se **requiere**un testigo.
- Si tiene tres o cuatro nodos, se **recomienda encarecidamente**el testigo.
- Si tiene acceso a Internet, use un  **[testigo en la nube](../../failover-clustering/deploy-cloud-witness.md) .**
- Si se encuentra en un entorno de TI con otras máquinas y recursos compartidos de archivos, use un testigo de recurso compartido de archivos

## <a name="how-cluster-quorum-works"></a>Funcionamiento del cuórum de clúster

Cuando se produce un error en los nodos o cuando un subconjunto de nodos pierde contacto con otro subconjunto, los nodos supervivientes deben comprobar que conforman la *mayor parte* del clúster para permanecer en línea. Si no pueden comprobarlo, se desconectarán.

Pero el concepto de *mayoría* solo funciona correctamente cuando el número total de nodos del clúster es impar (por ejemplo, tres nodos en un clúster de cinco nodos). Por lo tanto, ¿qué sucede con los clústeres con un número par de nodos (por ejemplo, un clúster de cuatro nodos)?

Hay dos maneras en que el clúster puede hacer que el *número total de votos* sea extraño:

1. En primer lugar, puede *subir* uno agregando un *testigo* con un voto adicional. Esto requiere la configuración del usuario.
2.  O bien, puede *reducir* el voto de un nodo sin suerte (se realiza automáticamente según sea necesario).

Siempre que los nodos supervivientes comprueban que son la *mayoría*, la definición de la *mayoría* se actualiza para que se encuentre entre los supervivientes. Esto permite que el clúster pierda un nodo, luego otro, después otro, etc. Este concepto del *número total de votos* que se adaptan después de los errores sucesivos se conoce como ***cuórum dinámico***.  

### <a name="dynamic-witness"></a>Testigo dinámico

El testigo dinámico alterna el voto del testigo para asegurarse de que el *número total de votos* es impar. Si hay un número impar de votos, el testigo no tiene un voto. Si hay un número par de votos, el testigo tiene un voto. El testigo dinámico reduce significativamente el riesgo de que el clúster se apague debido a un error del testigo. El clúster decide si usar el voto de testigo en función del número de nodos de votación que están disponibles en el clúster.

El cuórum dinámico funciona con el testigo dinámico de la manera que se describe a continuación.

### <a name="dynamic-quorum-behavior"></a>Comportamiento del cuórum dinámico

- Si tiene un número **par** de nodos y ningún testigo, *un nodo obtiene el voto cero*. Por ejemplo, solo tres de los cuatro nodos obtienen votos, por lo que el *número total de votos* es tres y dos supervivientes con votos se consideran una mayoría.
- Si tiene un número **impar** de nodos y ningún testigo, *todos obtienen votos*.
- Si tiene un número **par** de nodos más el testigo, *el testigo vota*, por lo que el total es impar.
- Si tiene un número **impar** de nodos más testigo, *el testigo no vota*.

El cuórum dinámico permite asignar un voto a un nodo de forma dinámica para evitar la pérdida de la mayoría de los votos y permitir que el clúster se ejecute con un nodo (conocido como el último cambio de posición). Vamos a tomar un clúster de cuatro nodos como ejemplo. Supongamos que el cuórum requiere 3 votos. 

En este caso, el clúster habría dejado de funcionar si se han perdido dos nodos.

![Diagrama que muestra cuatro nodos de clúster, cada uno de los cuales obtiene un voto](media/understand-quorum/dynamic-quorum-base.png)

Sin embargo, el cuórum dinámico evita que esto suceda. El *número total de votos* necesarios para el cuórum se determina ahora según el número de nodos disponibles. Por lo tanto, con quórum dinámico, el clúster permanecerá activo incluso si pierde tres nodos.

![Diagrama que muestra cuatro nodos de clúster, con los nodos con errores de uno en uno y el número de votos necesarios ajustados después de cada error.](media/understand-quorum/dynamic-quorum-step-through.png)

El escenario anterior se aplica a un clúster general que no tiene Espacios de almacenamiento directo habilitado. Sin embargo, cuando Espacios de almacenamiento directo está habilitado, el clúster solo puede admitir dos errores de nodo. Esto se explica con más detalle en la [sección Grupo de quórum](#pool-quorum-overview).

### <a name="examples"></a>Ejemplos

#### <a name="two-nodes-without-a-witness"></a>Dos nodos sin un testigo. 
El voto de un nodo está cero, por lo que el voto *mayoritario* se determina con un total de **1 voto**. Si el nodo que no es de votación se vuelve inesperadamente, el superviviente tiene 1/1 y el clúster sobrevive. Si el nodo de votación deja de funcionar inesperadamente, el superviviente tiene 0/1 y el clúster deja de funcionar. Si el nodo de votación está apagado correctamente, el voto se transfiere al otro nodo y el clúster sobrevive. ***Este es el motivo por el que es fundamental configurar un testigo.***

![Cuórum explicado en el caso de dos nodos sin un testigo](media/understand-quorum/2-node-no-witness.png)

- Puede sobrevivir a un error del servidor: **50 por ciento de probabilidad**.
- Puede sobrevivir a un error del servidor y luego a otro: **No**.
- Puede sobrevivir a dos errores de servidor a la vez: **No**. 

#### <a name="two-nodes-with-a-witness"></a>Dos nodos con un testigo. 
Ambos nodos votan, además de los votos del testigo, por lo que la *mayoría* se determina con un total de **3 votos**. Si uno de los dos nodos deja de funcionar, el superviviente tiene 2/3 y el clúster sobrevive.

![Cuórum explicado en el caso de dos nodos con un testigo](media/understand-quorum/2-node-witness.png)

- Puede sobrevivir a un error del servidor: **Sí**.
- Puede sobrevivir a un error del servidor y luego a otro: **No**.
- Puede sobrevivir a dos errores de servidor a la vez: **No**. 

#### <a name="three-nodes-without-a-witness"></a>Tres nodos sin un testigo.
Todos los nodos votan, por lo que la *mayoría* se determina con un total de **3 votos**. Si un nodo deja de funcionar, los supervivientes son 2/3 y el clúster sobrevive. El clúster se convierte en dos nodos sin testigo: en ese momento se encuentra en el escenario 1.

![Cuórum explicado en el caso de tres nodos sin un testigo](media/understand-quorum/3-node-no-witness.png)

- Puede sobrevivir a un error del servidor: **Sí**.
- Puede sobrevivir a un error del servidor y luego a otro: **50 por ciento de probabilidad**.
- Puede sobrevivir a dos errores de servidor a la vez: **No**. 

#### <a name="three-nodes-with-a-witness"></a>Tres nodos con un testigo.
Todos los nodos votan, por lo que el testigo no vota inicialmente. La *mayoría* se determina con un total de **3 votos**. Después de un error, el clúster tiene dos nodos con un testigo, que vuelve al escenario 2. Por lo tanto, ahora los dos nodos y el testigo de la votación.

![Cuórum explicado en el caso de tres nodos con un testigo](media/understand-quorum/3-node-witness.png)

- Puede sobrevivir a un error del servidor: **Sí**.
- Puede sobrevivir a un error del servidor y luego a otro: **Sí**.
- Puede sobrevivir a dos errores de servidor a la vez: **No**. 

#### <a name="four-nodes-without-a-witness"></a>Cuatro nodos sin un testigo
El voto de un nodo está cero, por lo que la *mayoría* se determina con un total de **3 votos**. Después de un error, el clúster se convierte en tres nodos y se encuentra en el escenario 3.

![Cuórum explicado en el caso de cuatro nodos sin un testigo](media/understand-quorum/4-node-no-witness.png)

- Puede sobrevivir a un error del servidor: **Sí**.
- Puede sobrevivir a un error del servidor y luego a otro: **Sí**.
- Puede sobrevivir a dos errores de servidor a la vez: **50 por ciento de probabilidad**. 

#### <a name="four-nodes-with-a-witness"></a>Cuatro nodos con un testigo.
Todos los votos de los nodos y los votos del testigo, por lo que la *mayoría* se determina con un total de **5 votos**. Después de un error, se encuentra en el escenario 4. Después de dos errores simultáneos, omita hasta el escenario 2.

![Cuórum explicado en el caso de cuatro nodos con un testigo](media/understand-quorum/4-node-witness.png)

- Puede sobrevivir a un error del servidor: **Sí**.
- Puede sobrevivir a un error del servidor y luego a otro: **Sí**.
- Puede sobrevivir a dos errores de servidor a la vez: **Sí**. 

#### <a name="five-nodes-and-beyond"></a>Cinco nodos y más.
Todos los nodos votan, o todo menos un voto, lo que hace que el total sea impar. De todos modos, Espacios de almacenamiento directo no puede controlar más de dos nodos, por lo que en este momento no se necesita ningún testigo o es útil.

![Cuórum explicado en el caso de cinco nodos y más allá](media/understand-quorum/5-nodes.png)

- Puede sobrevivir a un error del servidor: **Sí**.
- Puede sobrevivir a un error del servidor y luego a otro: **Sí**.
- Puede sobrevivir a dos errores de servidor a la vez: **Sí**. 

Ahora que sabemos cómo funciona el cuórum, echemos un vistazo a los tipos de testigos de cuórum.

### <a name="quorum-witness-types"></a>Tipos de testigo de cuórum

Los clústeres de conmutación por error admiten tres tipos de testigos de Cuórum:

- **[Testigo en la nube](../../failover-clustering/deploy-cloud-witness.md)** : el almacenamiento de blobs en Azure es accesible para todos los nodos del clúster. Mantiene la información de la agrupación en clústeres en un archivo testigo. log, pero no almacena una copia de la base de datos del clúster.
- **Testigo de recurso compartido de archivos** : un recurso compartido de archivos SMB que se configura en un servidor de archivos que ejecuta Windows Server. Mantiene la información de la agrupación en clústeres en un archivo testigo. log, pero no almacena una copia de la base de datos del clúster.
- **Testigo de disco** : un pequeño disco en clúster que se encuentra en el grupo almacenamiento disponible del clúster. Este disco tiene alta disponibilidad y puede conmutar por error entre nodos. Contiene una copia de la base de datos del clúster.  ***No se admite un testigo de disco con espacios de almacenamiento directo***.

## <a name="pool-quorum-overview"></a>Información general del cuórum del grupo

Acabamos de hablar sobre el Cuórum de clústeres, que funciona en el nivel de clúster. Ahora, vamos a profundizar en el Cuórum del grupo, que funciona en el nivel de grupo (es decir, puede perder nodos y unidades y dejar que el grupo se mantenga activo). Los grupos de almacenamiento se diseñaron para usarse en escenarios agrupados y no agrupados, por lo que tienen un mecanismo de cuórum diferente.

En la tabla siguiente se proporciona información general sobre los resultados del Cuórum del grupo por escenario:

| Nodos de servidor | Puede sobrevivir a un error de nodo de servidor | Puede sobrevivir a un error de nodo de servidor, otro | Puede sobrevivir a dos errores simultáneos de nodo de servidor |
|--------------|-------------------------------------|---------------------------------------------------|----------------------------------------------------|
| 2            | No                                  | No                                                | No                                                 |
| 2 + testigo  | Sí                                 | No                                                | No                                                 |
| 3            | Sí                                 | No                                                | No                                                 |
| 3 + testigo  | Sí                                 | No                                                | No                                                 |
| 4            | Sí                                 | No                                                | No                                                 |
| 4 testigos testigo  | Sí                                 | Sí                                               | Sí                                                |
| 5 y versiones posteriores  | Sí                                 | Sí                                               | Sí                                                |

## <a name="how-pool-quorum-works"></a>Cómo funciona el cuórum de grupo

Cuando se produce un error en las unidades, o cuando un subconjunto de unidades pierde el contacto con otro subconjunto, las unidades supervivientes deben comprobar que representan la *mayor parte* del grupo para permanecer en línea. Si no pueden comprobarlo, se desconectarán. El grupo es la entidad que se queda sin conexión o permanece en línea en función de si tiene discos suficientes para el cuórum (50% + 1). El propietario del recurso de grupo (nodo de clúster activo) puede ser el + 1.

Pero el cuórum del grupo funciona de forma diferente del cuórum del clúster de las siguientes maneras:

- el grupo usa un nodo en el clúster como testigo como un disyuntor para sobrevivir a la mitad de las unidades que han desaparecido (este nodo es el propietario del recurso de grupo)
- el grupo no tiene quórum dinámico
- el grupo no implementa su propia versión de quitar un voto

### <a name="examples"></a>Ejemplos

#### <a name="four-nodes-with-a-symmetrical-layout"></a>Cuatro nodos con un diseño simétrico. 
Cada una de las 16 unidades tiene un voto y el nodo dos también tiene un voto (puesto que es el propietario del recurso de grupo). La *mayoría* se determina con un total de **16 votos**. Si los nodos tres y cuatro están inactivos, el subconjunto superviviente tiene 8 unidades y el propietario del recurso de grupo, que es 9/16 votos. Por lo tanto, el grupo sobrevive.

![Cuórum de grupo 1](media/understand-quorum/pool-1.png)

- Puede sobrevivir a un error del servidor: **Sí**.
- Puede sobrevivir a un error del servidor y luego a otro: **Sí**.
- Puede sobrevivir a dos errores de servidor a la vez: **Sí**. 

#### <a name="four-nodes-with-a-symmetrical-layout-and-drive-failure"></a>Cuatro nodos con un diseño simétrico y un error de unidad. 
Cada una de las 16 unidades tiene un voto y el nodo 2 también tiene un voto (puesto que es el propietario del recurso de grupo). La *mayoría* se determina con un total de **16 votos**. En primer lugar, la unidad 7 deja de funcionar. Si los nodos tres y cuatro están inactivos, el subconjunto superviviente tiene 7 unidades y el propietario del recurso de grupo, que es 8/16 votos. Por lo tanto, el grupo no tiene mayoría y deja de funcionar.

![Cuórum de grupo 2](media/understand-quorum/pool-2.png)

- Puede sobrevivir a un error del servidor: **Sí**.
- Puede sobrevivir a un error del servidor y luego a otro: **No**.
- Puede sobrevivir a dos errores de servidor a la vez: **No**. 

#### <a name="four-nodes-with-a-non-symmetrical-layout"></a>Cuatro nodos con un diseño no simétrico. 
Cada una de las 24 unidades tiene un voto y el nodo dos también tiene un voto (puesto que es el propietario del recurso de grupo). La *mayoría* se determina con un total de **24 votos**. Si los nodos tres y cuatro están inactivos, el subconjunto superviviente tiene 8 unidades y el propietario del recurso de grupo, que es 9/24 votos. Por lo tanto, el grupo no tiene mayoría y deja de funcionar.

![Cuórum de grupo 3](media/understand-quorum/pool-3.png)

- Puede sobrevivir a un error del servidor: **Sí**.
- Puede sobrevivir a un error del servidor y, a continuación, otro: * * depende de * * (no puede sobrevivir si ambos nodos están inactivos, pero pueden sobrevivir a todos los demás escenarios.
- Puede sobrevivir a dos errores de servidor a la vez: * * depende de * * (no puede sobrevivir si ambos nodos están inactivos, pero pueden sobrevivir a todos los demás escenarios.

### <a name="pool-quorum-recommendations"></a>Recomendaciones de cuórum de grupo

- Asegúrese de que cada nodo del clúster es simétrico (cada nodo tiene el mismo número de unidades)
- Habilite reflejo triple o paridad dual para que pueda tolerar errores de nodo y mantener los discos virtuales en línea. Consulte nuestra [Página de instrucciones de volumen](plan-volumes.md) para más información.
- Si hay más de dos nodos inactivos, o dos nodos y un disco en otro nodo están inactivos, es posible que los volúmenes no tengan acceso a las tres copias de sus datos y, por tanto, estén sin conexión y no estén disponibles. Se recomienda devolver los servidores o reemplazar los discos rápidamente para garantizar la máxima resistencia de todos los datos del volumen.

## <a name="more-information"></a>Más información

- [Configurar y administrar el cuórum](../../failover-clustering/manage-cluster-quorum.md)
- [Implementar un testigo en la nube](../../failover-clustering/deploy-cloud-witness.md)
