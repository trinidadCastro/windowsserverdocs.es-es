---
title: Descripción del cuórum de clúster y grupo
description: Descripción del Cuórum del clúster y del grupo, con ejemplos específicos para ver los detalles.
ms.author: adagashe
manager: eldenc
ms.topic: article
author: adagashe
ms.date: 01/18/2019
ms.localizationpriority: medium
ms.openlocfilehash: ffd1afdc76ddefbf53ff8f78d15666f343654022
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87970142"
---
# <a name="understanding-cluster-and-pool-quorum"></a>Descripción del cuórum de clúster y grupo

>Se aplica a: Windows Server 2019, Windows Server 2016

Los [clústeres de conmutación por error de Windows Server](../../failover-clustering/failover-clustering-overview.md) proporcionan alta disponibilidad para las cargas de trabajo. Estos recursos se consideran de alta disponibilidad si los nodos que los hospedan están activos; sin embargo, el clúster necesita generalmente que más de la mitad de los nodos estén en ejecución, es decir, que tengan *cuórum*.

El cuórum está diseñado para impedir escenarios de *cerebro dividido* que pueden producirse cuando hay una partición en la red y los subconjuntos de nodos no se pueden comunicar entre sí. Como consecuencia, ambos subconjuntos de nodos pueden intentar poseer la carga de trabajo y escribir en el mismo disco, lo que puede provocar numerosos problemas. Sin embargo, esto se evita con el concepto de cuórum de clústeres de conmutación por error que obliga a que solo uno de estos grupos de nodos continúe ejecutándose, por lo que solo uno de estos grupos permanecerá en línea.

El cuórum determina el número de errores que puede aguantar el clúster mientras permanece en línea. El cuórum está diseñado para administrar el escenario en que hay un problema con la comunicación entre los subconjuntos de nodos de clúster, de modo que varios servidores no intenten hospedar a la vez un grupo de recursos y escribir en el mismo disco a la vez. Con este concepto de cuórum, el clúster obligará a que el servicio de clúster se detenga en uno de los subconjuntos de nodos para garantizar que solo haya un verdadero propietario de un grupo de recursos determinado. Una vez que los nodos detenidos pueden volver a comunicarse con el grupo principal de nodos, se volverán a unir automáticamente al clúster e iniciarán su servicio de clúster.

En Windows Server 2019 y Windows Server 2016, hay dos componentes del sistema que tienen sus propios mecanismos de quórum:

- **Cuórum de clúster**: funciona en el nivel de clúster (es decir, puede perder nodos y hacer que el clúster permanezca activo).
- **Cuórum de grupos**: funciona en el nivel de grupo cuando Espacios de almacenamiento directo está habilitado (es decir, puede perder nodos y unidades y hacer que el grupo permanezca activo). Los bloques de almacenamiento se diseñaron para usarse en escenarios agrupados y no agrupados, por lo que tienen un mecanismo de cuórum diferente.

## <a name="cluster-quorum-overview"></a>Introducción al cuórum de clúster

En la tabla siguiente se proporciona información general sobre los resultados del cuórum de clúster por escenario:

| Nodos de servidor | Pueden sobrevivir a un error del nodo de servidor | Pueden sobrevivir a un error del nodo de servidor y luego a otro | Pueden sobrevivir a dos errores simultáneos del nodo de servidor |
|--------------|-------------------------------------|---------------------------------------------------|----------------------------------------------------|
| 2            | 50/50                               | No                                                | No                                                 |
| 2 + testigo  | Sí                                 | No                                                | No                                                 |
| 3            | Sí                                 | 50/50                                             | No                                                 |
| 3 + testigo  | Sí                                 | Sí                                               | No                                                 |
| 4            | Sí                                 | Sí                                               | 50/50                                              |
| 4 + testigo  | Sí                                 | Sí                                               | Sí                                                |
| 5 y más  | Sí                                 | Sí                                               | Sí                                                |

### <a name="cluster-quorum-recommendations"></a>Recomendaciones para el cuórum de clúster

- Si tiene dos nodos, **se requiere** un testigo.
- Si tiene tres o cuatro nodos, **se recomienda firmemente** un testigo.
- Si tiene acceso a Internet, use un **[testigo en la nube](../../failover-clustering/deploy-cloud-witness.md)** .
- Si se encuentra en un entorno de TI con otras máquinas y recursos compartidos de archivos, use un testigo de recurso compartido de archivos.

## <a name="how-cluster-quorum-works"></a>Funcionamiento del cuórum de clúster

Cuando se produce un error en los nodos, o cuando un subconjunto de nodos pierde contacto con otro, los nodos supervivientes deben comprobar que conforman la *mayoría* del clúster para permanecer en línea. Si no pueden comprobarlo, se desconectan.

Pero el concepto de *mayoría* solo funciona correctamente cuando el número total de nodos del clúster es impar (por ejemplo, tres nodos en un clúster de cinco). Por lo tanto, ¿qué sucede con los clústeres con un número par de nodos (por ejemplo, un clúster de cuatro nodos)?

Hay dos maneras en que el clúster puede hacer que el *número total de votos* sea impar:

1. En primer lugar, puede *activar* uno agregando un *testigo* con un voto adicional. Esto requiere la configuración del usuario.
2.    O bien, puede *desactivar* uno si pone a cero el voto de un nodo menos afortunado (se realiza automáticamente según sea necesario).

Siempre que los nodos supervivientes comprueben correctamente que son la *mayoría*, la definición de *mayoría* se actualiza para que estén entre los supervivientes. Esto permite que el clúster pierda un nodo, luego otro, después otro, etc. Este concepto de la adaptación del *número total de votos* después de errores sucesivos se conoce como ***cuórum dinámico***.

### <a name="dynamic-witness"></a>Testigo dinámico

El testigo dinámico alterna el voto del testigo para asegurarse de que el *número total de votos* sea impar. Si hay un número impar de votos, el testigo no tiene un voto. Si hay un número par de votos, el testigo tiene un voto. El testigo dinámico reduce considerablemente el riesgo de que el clúster se desactive debido a un error del testigo. El clúster decide si usará el voto del testigo en función del número de nodos votantes que estén disponibles en el clúster.

El cuórum dinámico funciona con el testigo dinámico de la manera que se describe a continuación.

### <a name="dynamic-quorum-behavior"></a>Comportamiento del cuórum dinámico

- Si tiene un número **par** de nodos y ningún testigo, *un nodo pone su voto a cero*. Por ejemplo, solo tres de los cuatro nodos obtienen votos, por lo que el *número total de votos* es tres, y dos supervivientes con votos se consideran una mayoría.
- Si tiene un número **impar** de nodos y ningún testigo, *todos obtienen votos*.
- Si tiene un número **par** de nodos más un testigo, *el testigo vota*, por lo que el total es impar.
- Si tiene un número**impar** de nodos más un testigo, *el testigo no vota*.

El cuórum dinámico ofrece la posibilidad de asignar un voto a un nodo de forma dinámica para evitar la pérdida de la mayoría de los votos y permitir que el clúster se ejecute con un nodo (el último que queda). Vamos a tomar como ejemplo un clúster de cuatro nodos. Supongamos que el cuórum necesita tres votos.

En este caso, el clúster se habría vuelto inactivo si ha perdido dos nodos.

![Diagrama que muestra cuatro nodos del clúster, cada uno de los cuales obtiene un voto](media/understand-quorum/dynamic-quorum-base.png)

Sin embargo, el cuórum dinámico evita que esto suceda. El *número total de votos* necesario para el cuórum se determina ahora según el número de nodos disponibles. Por lo tanto, con un cuórum dinámico, el clúster permanecerá activo incluso si pierde tres nodos.

![Diagrama que muestra cuatro nodos del clúster, con nodos que van dejando de funcionar de uno en uno, y el número de votos necesarios que se ajustan después de cada error.](media/understand-quorum/dynamic-quorum-step-through.png)

El escenario anterior se aplica a un clúster general que no tiene habilitado Espacios de almacenamiento directo. Sin embargo, cuando Espacios de almacenamiento directo está habilitado, el clúster solo puede admitir dos errores del nodo. Esto se explica con más detalle en la [sección sobre el cuórum de grupo](#pool-quorum-overview).

### <a name="examples"></a>Ejemplos

#### <a name="two-nodes-without-a-witness"></a>Dos nodos sin un testigo.
El voto de un nodo se pone a cero, por lo que la *mayoría* de votos se determina a partir de un total de **1 voto**. Si el nodo que no vota se vuelve inactivo de manera inesperada, el superviviente tiene 1/1 y el clúster sobrevive. Si el nodo votante se vuelve inactivo de manera inesperada, el superviviente tiene 0/1 y el clúster deja de funcionar. Si el nodo votante se apaga correctamente, el voto se transfiere al otro nodo y el clúster sobrevive. ***Este es el motivo por el que es fundamental configurar un testigo.***

![Cuórum explicado en el caso de dos nodos sin un testigo](media/understand-quorum/2-node-no-witness.png)

- Puede sobrevivir a un error del servidor: **Cincuenta por ciento de probabilidad**.
- Puede sobrevivir a un error del servidor y luego a otro: **No**.
- Puede sobrevivir a dos errores seguidos del servidor: **No**.

#### <a name="two-nodes-with-a-witness"></a>Dos nodos con un testigo.
Ambos nodos votan, además de los votos del testigo, por lo que la *mayoría* se determina a partir de un total de **3 votos**. Si uno de los nodos se vuelve inactivo, el superviviente tiene 2/3 y el clúster sobrevive.

![Cuórum explicado en el caso de dos nodos con un testigo](media/understand-quorum/2-node-witness.png)

- Puede sobrevivir a un error del servidor: **Sí**.
- Puede sobrevivir a un error del servidor y luego a otro: **No**.
- Puede sobrevivir a dos errores seguidos del servidor: **No**.

#### <a name="three-nodes-without-a-witness"></a>Tres nodos sin un testigo.
Todos los nodos votan, por lo que la *mayoría* se determina a partir de un total de **3 votos**. Si un nodo deja de funcionar, los supervivientes son 2/3 y el clúster sobrevive. El clúster pasa a tener dos nodos sin testigo: en ese momento, estaría en el escenario 1.

![Cuórum explicado en el caso de tres nodos sin un testigo](media/understand-quorum/3-node-no-witness.png)

- Puede sobrevivir a un error del servidor: **Sí**.
- Puede sobrevivir a un error del servidor y luego a otro: **Cincuenta por ciento de probabilidad**.
- Puede sobrevivir a dos errores seguidos del servidor: **No**.

#### <a name="three-nodes-with-a-witness"></a>Tres nodos con un testigo.
Todos los nodos votan, por lo que el testigo no vota inicialmente. La *mayoría* se determina a partir de un total de **3 votos**. Después de un error, el clúster tiene dos nodos con un testigo, lo que supone la vuelta al escenario 2. Por lo tanto, ahora los dos nodos y el testigo votan.

![Cuórum explicado en el caso de tres nodos con un testigo](media/understand-quorum/3-node-witness.png)

- Puede sobrevivir a un error del servidor: **Sí**.
- Puede sobrevivir a un error del servidor y luego a otro: **Sí**.
- Puede sobrevivir a dos errores seguidos del servidor: **No**.

#### <a name="four-nodes-without-a-witness"></a>Cuatro nodos sin un testigo
El voto de un nodo se pone a cero, por lo que la *mayoría* se determina a partir de un total de **3 votos**. Después de un error, el clúster pasa a tener tres nodos y se encontraría en el escenario 3.

![Cuórum explicado en el caso de cuatro nodos sin un testigo](media/understand-quorum/4-node-no-witness.png)

- Puede sobrevivir a un error del servidor: **Sí**.
- Puede sobrevivir a un error del servidor y luego a otro: **Sí**.
- Puede sobrevivir a dos errores seguidos del servidor: **Cincuenta por ciento de probabilidad**.

#### <a name="four-nodes-with-a-witness"></a>Cuatro nodos con un testigo.
Todos los nodos y el testigo votan, por lo que la *mayoría* se determina a partir de un total de **5 votos**. Después de un error, se encontraría en el escenario 4. Después de dos errores simultáneos, pasaría directamente al escenario 2.

![Cuórum explicado en el caso de cuatro nodos con un testigo](media/understand-quorum/4-node-witness.png)

- Puede sobrevivir a un error del servidor: **Sí**.
- Puede sobrevivir a un error del servidor y luego a otro: **Sí**.
- Puede sobrevivir a dos errores seguidos del servidor: **Sí**.

#### <a name="five-nodes-and-beyond"></a>Cinco nodos y más.
Todos los nodos votan, o todos menos uno, lo que hace que el total sea impar. De todos modos, Espacios de almacenamiento directo no puede controlar más de dos nodos inactivos, por lo que en este momento ningún testigo es necesario o útil.

![Cuórum explicado en el caso de cinco nodos y más](media/understand-quorum/5-nodes.png)

- Puede sobrevivir a un error del servidor: **Sí**.
- Puede sobrevivir a un error del servidor y luego a otro: **Sí**.
- Puede sobrevivir a dos errores seguidos del servidor: **Sí**.

Ahora que sabemos cómo funciona el cuórum, echemos un vistazo a los tipos de testigos de cuórum.

### <a name="quorum-witness-types"></a>Tipos de testigo de cuórum

Los clústeres de conmutación por error admiten tres tipos de testigos de cuórum:

- **[Testigo en la nube](../../failover-clustering/deploy-cloud-witness.md)** : todos los nodos del clúster pueden acceder al almacenamiento de blobs de Azure. Este testigo mantiene la información de la agrupación en clústeres en un archivo witness.log, pero no almacena una copia de la base de datos del clúster.
- **Testigo de recurso compartido de archivos**: un recurso compartido de archivos de SMB que se configura en un servidor de archivos que ejecuta Windows Server. Este testigo mantiene la información de la agrupación en clústeres en un archivo witness.log, pero no almacena una copia de la base de datos del clúster.
- **Testigo de disco**: un pequeño disco agrupado que se encuentra en el grupo de almacenamiento disponible del clúster. Este disco presenta alta disponibilidad y puede conmutar por error entre nodos. Contiene una copia de la base de datos del clúster.  ***No se admite un testigo de disco con Espacios de almacenamiento directo***.

## <a name="pool-quorum-overview"></a>Introducción al cuórum de grupo

Acabamos de hablar sobre el cuórum de clúster, que funciona en el nivel de clúster. Ahora, vamos a profundizar en el cuórum de grupo, que funciona en el nivel de grupo (es decir, puede perder nodos y unidades y hacer que el grupo se mantenga activo). Los bloques de almacenamiento se diseñaron para usarse en escenarios agrupados y no agrupados, por lo que tienen un mecanismo de cuórum diferente.

En la tabla siguiente se proporciona información general sobre los resultados del cuórum de grupo por escenario:

| Nodos de servidor | Pueden sobrevivir a un error del nodo de servidor | Pueden sobrevivir a un error del nodo de servidor y luego a otro | Pueden sobrevivir a dos errores simultáneos del nodo de servidor |
|--------------|-------------------------------------|---------------------------------------------------|----------------------------------------------------|
| 2            | No                                  | No                                                | No                                                 |
| 2 + testigo  | Sí                                 | No                                                | No                                                 |
| 3            | Sí                                 | No                                                | No                                                 |
| 3 + testigo  | Sí                                 | No                                                | No                                                 |
| 4            | Sí                                 | No                                                | No                                                 |
| 4 + testigo  | Sí                                 | Sí                                               | Sí                                                |
| 5 y más  | Sí                                 | Sí                                               | Sí                                                |

## <a name="how-pool-quorum-works"></a>Funcionamiento del cuórum de grupo

Cuando se produce un error en las unidades, o cuando un subconjunto de unidades pierde el contacto con otro, las unidades supervivientes deben comprobar que constituyen la *mayoría* del grupo para permanecer en línea. Si no pueden comprobarlo, se desconectan. El grupo es la entidad que se desconecta o permanece en línea según si tiene discos suficientes para el cuórum (50 % + 1). El propietario del recurso del grupo (nodo de clúster activo) puede ser el +1.

Sin embargo, el cuórum de grupo funciona de forma diferente al cuórum de clúster de las siguientes maneras:

- el grupo usa como testigo un nodo del clúster a modo de "desempate" para que sobrevivan la mitad de las unidades que han desaparecido (este nodo es el propietario del recurso del grupo)
- el grupo NO tiene cuórum dinámico
- el grupo NO implementa su propia versión de quitar un voto

### <a name="examples"></a>Ejemplos

#### <a name="four-nodes-with-a-symmetrical-layout"></a>Cuatro nodos con un diseño simétrico.
Cada una de las 16 unidades tiene un voto y el nodo 2 también tiene un voto (puesto que es el propietario del recurso del grupo). La *mayoría* se determina a partir de un total de **16 votos**. Si los nodos 3 y 4 están inactivos, el subconjunto superviviente tiene 8 unidades y el propietario del recurso del grupo, lo que suma 9/16 votos. Por lo tanto, el grupo sobrevive.

![Cuórum de grupo 1](media/understand-quorum/pool-1.png)

- Puede sobrevivir a un error del servidor: **Sí**.
- Puede sobrevivir a un error del servidor y luego a otro: **Sí**.
- Puede sobrevivir a dos errores seguidos del servidor: **Sí**.

#### <a name="four-nodes-with-a-symmetrical-layout-and-drive-failure"></a>Cuatro nodos con un diseño simétrico y un error de unidad.
Cada una de las 16 unidades tiene un voto y el nodo 2 también tiene un voto (puesto que es el propietario del recurso del grupo). La *mayoría* se determina a partir de un total de **16 votos**. En primer lugar, la unidad 7 deja de funcionar. Si los nodos 3 y 4 están inactivos, el subconjunto superviviente tiene 7 unidades y el propietario del recurso del grupo, lo que suma 8/16 votos. Por lo tanto, el grupo no tiene mayoría y deja de funcionar.

![Cuórum de grupo 2](media/understand-quorum/pool-2.png)

- Puede sobrevivir a un error del servidor: **Sí**.
- Puede sobrevivir a un error del servidor y luego a otro: **No**.
- Puede sobrevivir a dos errores seguidos del servidor: **No**.

#### <a name="four-nodes-with-a-non-symmetrical-layout"></a>Cuatro nodos con un diseño no simétrico.
Cada una de las 24 unidades tiene un voto y el nodo dos también tiene un voto (puesto que es el propietario del recurso del grupo). La *mayoría* se determina a partir de un total de **24 votos**. Si los nodos 3 y 4 están inactivos, el subconjunto superviviente tiene 8 unidades y el propietario del recurso del grupo, lo que suma 9/24 votos. Por lo tanto, el grupo no tiene mayoría y deja de funcionar.

![Cuórum de grupo 3](media/understand-quorum/pool-3.png)

- Puede sobrevivir a un error del servidor: **Sí**.
- Puede sobrevivir a un error del servidor y luego a otro: **depende** (no puede sobrevivir si los nodos 3 y 4 están inactivos, pero puede sobrevivir en todos los demás escenarios.
- Puede sobrevivir a dos errores seguidos del servidor: **depende** (no puede sobrevivir si los nodos 3 y 4 están inactivos, pero puede sobrevivir en todos los demás escenarios.

### <a name="pool-quorum-recommendations"></a>Recomendaciones para los cuórums de grupo

- Asegúrese de que cada nodo del clúster sea simétrico (mismo número de unidades).
- Habilite el reflejo triple o la paridad dual para que pueda tolerar errores de nodo y mantener en línea los discos virtuales. Consulte nuestra [Página de instrucciones de volumen](plan-volumes.md) para más información.
- Si hay más de dos nodos inactivos, o dos nodos y un disco de otro nodo están inactivos, es posible que los volúmenes no tengan acceso a las tres copias de sus datos y, por tanto, se desconecten y no estén disponibles. Se recomienda recuperar los servidores o reemplazar los discos rápidamente para garantizar la máxima resistencia de todos los datos del volumen.

## <a name="more-information"></a>Más información

- [Configurar y administrar el cuórum](../../failover-clustering/manage-cluster-quorum.md)
- [Implementar un testigo en la nube](../../failover-clustering/deploy-cloud-witness.md)
