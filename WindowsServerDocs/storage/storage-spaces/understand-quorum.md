---
title: Quórum de clúster y grupo de descripción
description: Descripción de clúster y grupo de quórum, con ejemplos específicos para ir a través de las complejidades.
keywords: Espacios de almacenamiento directo, quórum, testigo, S2D, quórum, grupo de quórum, clúster, grupo de clúster
ms.prod: windows-server-threshold
ms.author: adagashe
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: adagashe
ms.date: 01/18/2019
ms.localizationpriority: medium
ms.openlocfilehash: 30958b8b1e8b0009626509409d1f031611c76a20
ms.sourcegitcommit: fe621b72d45d0259bac1d5b9031deed3dcbed29d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/01/2019
ms.locfileid: "66455434"
---
# <a name="understanding-cluster-and-pool-quorum"></a>Quórum de clúster y grupo de descripción

>Se aplica a: Windows Server 2019, Windows Server 2016

[Clústeres de conmutación por error de Windows Server](../../failover-clustering/failover-clustering-overview.md) proporciona alta disponibilidad para cargas de trabajo. Estos recursos se consideran altamente disponibles si los nodos que hospedan recursos Sin embargo, el clúster normalmente requiere más de la mitad de los nodos que se esté ejecutando, lo que se conoce como si tuviera *quórum*.

Quórum está diseñado para evitar *cerebro dividido* escenarios que pueden ocurrir cuando hay una partición en la red y subconjuntos de nodos no pueden comunicarse entre sí. Esto puede causar dos subconjuntos de nodos para tratar la carga de trabajo el propietario y escribir en el mismo disco que puede provocar numerosos problemas. Sin embargo, esto no está permitido con el concepto de clústeres de conmutación por error de quórum que obliga a solo uno de estos grupos de nodos para continuar la ejecución, por lo que solo uno de estos grupos se mantendrá en línea.

Quórum determina el número de errores que el clúster puede admitir mientras permanece en línea. Quórum está diseñado para controlar el escenario cuando hay un problema con la comunicación entre los subconjuntos de los nodos del clúster, por lo que no intentan varios servidores simultáneamente un grupo de recursos de host y escribir en el mismo disco al mismo tiempo. Si tiene este concepto de quórum, el clúster forzará el servicio de clúster se detenga en uno de los subconjuntos de nodos para asegurarse de que hay solo un auténtico propietario de un grupo de recursos determinado. Una vez que una vez más, nodos que se han detenido pueden comunicarse con el grupo de nodos principal, automáticamente se unan al clúster e inicie su servicio de clúster.

En Windows Server 2019 y Windows Server 2016, hay dos componentes del sistema que tienen sus propios mecanismos de quórum:

- **Quórum de clúster**: Esto funciona en el nivel de clúster (es decir, puede perder los nodos y hacer que el clúster de estar al día)
- **Grupo de quórum**: Esto funciona en el nivel de grupo cuando se habilita espacios de almacenamiento directo (es decir, puede perder los nodos y las unidades y hacer que el grupo de estar al día). Los grupos de almacenamiento se diseñaron para usarse en escenarios clústeres y no agrupados, motivo por el que tienen un mecanismo de quórum diferentes.

## <a name="cluster-quorum-overview"></a>Información general sobre el quórum de clúster

En la tabla siguiente ofrece una visión general de los resultados de quórum de clúster por cada escenario:

| Nodos de servidor | Puede sobrevivir a un error de nodo de servidor | Puede sobrevivir a un error de nodo de un servidor y, después, otro | Puede sobrevivir a dos errores de nodo de servidor simultáneas |
|--------------|-------------------------------------|---------------------------------------------------|----------------------------------------------------|
| 2            | 50/50                               | No                                                | No                                                 |
| 2 + testigo  | Sí                                 | No                                                | No                                                 |
| 3            | Sí                                 | 50/50                                             | No                                                 |
| 3 + testigo  | Sí                                 | Sí                                               | No                                                 |
| 4            | Sí                                 | Sí                                               | 50/50                                              |
| 4 + testigo  | Sí                                 | Sí                                               | Sí                                                |
| 5 y versiones posteriores  | Sí                                 | Sí                                               | Sí                                                |

### <a name="cluster-quorum-recommendations"></a>Recomendaciones de quórum de clúster

- Si tiene dos nodos, es un testigo **requiere**.
- Si tiene tres o cuatro nodos, el testigo es **recomienda**.
- Si tiene acceso a Internet, use un  **[testigo en la nube](../../failover-clustering/deploy-cloud-witness.md)**
- Si está en un entorno de TI con otras máquinas y los recursos compartidos de archivos, utilice un testigo del recurso compartido de archivos

## <a name="how-cluster-quorum-works"></a>Funcionamiento del quórum de clúster

Cuando hay errores en nodos, o cuando un subconjunto de nodos pierde el contacto con otro subconjunto, los nodos de supervivientes deben comprobar que constituyen el *mayoría* del clúster permanezcan en línea. Si no se comprueba que, vayan sin conexión.

Pero el concepto de *mayoría* sólo funciona correctamente cuando el número total de nodos del clúster es impar (por ejemplo, tres nodos en un clúster de cinco nodos). Por lo tanto, ¿qué sucede con los clústeres con un número par de nodos (por ejemplo, un clúster de cuatro nodos)?

Hay dos maneras de hacer que el clúster el *número total de votos* impar:

1. En primer lugar, puede ir *seguridad* uno mediante la adición de un *testigo* con un voto extra. Esto requiere configuraciones de usuario.
2.  O bien, puede ir *abajo* uno mediante la puesta a cero uno desafortunado voto del nodo (ocurre automáticamente según sea necesario).

Cada vez que los nodos que sobrevivan correctamente comprobar son la *mayoría*, la definición de *mayoría* se actualiza para que sea uno solo de los supervivientes. Esto permite que el clúster pierde un nodo, otro y, luego, otro y así sucesivamente. Este concepto de la *número total de votos* adaptar tras errores sucesivos se conoce como ***quórum dinámico***.  

### <a name="dynamic-witness"></a>Testigo dinámico

Testigo dinámico alterna el voto de testigo para asegurarse de que el *número total de votos* es impar. Si hay un número impar de votos, el testigo no tiene un voto. Si hay un número par de votos, el testigo tiene un voto. Testigo dinámica reduce considerablemente el riesgo de que el clúster dejarán de funcionar debido a un error de testigo. El clúster decide si se debe usar el voto de testigo en función del número de nodos que votan que están disponibles en el clúster.

Quórum dinámico funciona con testigo dinámica de la manera en que se describe a continuación.

### <a name="dynamic-quorum-behavior"></a>Comportamiento de quórum dinámico

- Si tiene un **incluso** número de nodos y sin testigo, *un nodo obtiene su voto zeroed*. Por ejemplo, solo tres de los cuatro nodos obtención votos, por lo que la *número total de votos* es tres y dos supervivientes con los votos se consideran una mayoría.
- Si tiene un **impar** número de nodos y sin testigo, *obtienen votos*.
- Si tiene un **incluso** número de nodos más testigo, *el testigo votos*, por lo que el total es impar.
- Si tiene un **impar** número de nodos más testigo, *el testigo no votar*.

Quórum dinámico permite la capacidad para asignar un voto a un nodo de forma dinámica para evitar la pérdida de la mayoría de los votos y para permitir que el clúster funcione con un nodo (conocido como última-man permanente). Por ejemplo, echemos un clúster de cuatro nodos. Suponga que quórum requiere 3 votos. 

En este caso, el clúster habrían pasado hacia abajo si ha perdido los dos nodos.

![Diagrama que muestra cuatro nodos de clúster, cada uno de los cuales obtiene un voto](media/understand-quorum/dynamic-quorum-base.png)

Sin embargo, quórum dinámico evita que esto suceda. El *número total de votos* requiere de quórum ahora se determina en función del número de nodos disponibles. Por lo tanto, con quórum dinámico, el clúster seguirá siendo seguridad incluso si pierde tres nodos.

![Diagrama que muestra cuatro nodos de clúster, con nodos que se producen errores en uno cada vez y el número de votos necesarios ajustar después de cada error.](media/understand-quorum/dynamic-quorum-step-through.png)

El escenario anterior se aplica a un clúster general que no tiene espacios de almacenamiento directo habilitado. Sin embargo, cuando se habilita espacios de almacenamiento directo, el clúster solo puede admitir errores en los dos nodos. Esto se explica más en el [grupo sección quórum](#pool-quorum-overview).

### <a name="examples"></a>Ejemplos

#### <a name="two-nodes-without-a-witness"></a>Dos nodos sin un testigo. 
Se pone a cero el voto de un nodo, por lo que la *mayoría* voto viene de un total de **1 voto**. Si el nodo que no votan deja de responder inesperadamente, la permanencia tiene 1/1 y sobrevive el clúster. Si el voto de nodo deja de responder inesperadamente, la permanencia tiene 0/1 y el clúster deja de funcionar. Si el voto de nodo está apagado correctamente, los votos se transfieren a otro nodo y sobrevive el clúster. ***Esto es por eso es fundamental para configurar a un testigo.***

![Se explica en el caso de dos nodos sin un testigo de quórum](media/understand-quorum/2-node-no-witness.png)

- Puede sobrevivir a un error de servidor: **Oportunidad de cincuenta por ciento**.
- Puede sobrevivir a un error de un servidor y luego otro: **No**.
- Puede sobrevivir a dos errores de servidor a la vez: **No**. 

#### <a name="two-nodes-with-a-witness"></a>Dos nodos con un testigo. 
Ambos nodos se vota, además de los votos el testigo, por lo que *mayoría* viene de un total de **3 votos**. Si uno de los nodos deja de funcionar, la permanencia tiene 2/3 y sobrevive el clúster.

![Se explica en el caso de dos nodos con un testigo de quórum](media/understand-quorum/2-node-witness.png)

- Puede sobrevivir a un error de servidor: **Sí**.
- Puede sobrevivir a un error de un servidor y luego otro: **No**.
- Puede sobrevivir a dos errores de servidor a la vez: **No**. 

#### <a name="three-nodes-without-a-witness"></a>Tres nodos sin un testigo.
Todos los nodos se vota, por lo que la *mayoría* viene de un total de **3 votos**. Si ningún nodo deja de funcionar, los supervivientes son 2/3 y sobrevive el clúster. El clúster pasa a ser de dos nodos sin un testigo, en ese momento, está en el escenario 1.

![Se explica en el caso con tres nodos sin un testigo de quórum](media/understand-quorum/3-node-no-witness.png)

- Puede sobrevivir a un error de servidor: **Sí**.
- Puede sobrevivir a un error de un servidor y luego otro: **Oportunidad de cincuenta por ciento**.
- Puede sobrevivir a dos errores de servidor a la vez: **No**. 

#### <a name="three-nodes-with-a-witness"></a>Tres nodos con un testigo.
Votan en todos los nodos, por lo que el testigo no votar inicialmente. El *mayoría* viene de un total de **3 votos**. Después de un error, el clúster tiene dos nodos con un testigo, que es al escenario 2. Por lo tanto, ahora los dos nodos y el testigo votan.

![Se explica en el caso con tres nodos con un testigo de quórum](media/understand-quorum/3-node-witness.png)

- Puede sobrevivir a un error de servidor: **Sí**.
- Puede sobrevivir a un error de un servidor y luego otro: **Sí**.
- Puede sobrevivir a dos errores de servidor a la vez: **No**. 

#### <a name="four-nodes-without-a-witness"></a>Cuatro nodos sin ningún testigo
Se pone a cero el voto de un nodo, por lo que la *mayoría* viene de un total de **3 votos**. Después de un error, el clúster se convierte en tres nodos y está en el escenario 3.

![Se explica en el caso de cuatro nodos sin un testigo de quórum](media/understand-quorum/4-node-no-witness.png)

- Puede sobrevivir a un error de servidor: **Sí**.
- Puede sobrevivir a un error de un servidor y luego otro: **Sí**.
- Puede sobrevivir a dos errores de servidor a la vez: **Oportunidad de cincuenta por ciento**. 

#### <a name="four-nodes-with-a-witness"></a>Cuatro nodos con un testigo.
Todos los votos de nodos y los votos de testigo, por lo que la *mayoría* viene de un total de **5 votos**. Después de un error, se encuentra en el escenario 4. Después de dos errores simultáneos, omitir hasta el escenario 2.

![Se explica en el caso de cuatro nodos con un testigo de quórum](media/understand-quorum/4-node-witness.png)

- Puede sobrevivir a un error de servidor: **Sí**.
- Puede sobrevivir a un error de un servidor y luego otro: **Sí**.
- Puede sobrevivir a dos errores de servidor a la vez: **Sí**. 

#### <a name="five-nodes-and-beyond"></a>Cinco nodos y mucho más.
Todos los nodos se vota o todas, pero un voto, cualquier hace que el total impar. Espacios de almacenamiento directo no se puede controlar más de dos nodos hacia abajo en cualquier caso, por lo que en este momento, ningún testigo es necesario o útil.

![Se explica en el caso de cinco nodos y más allá de quórum](media/understand-quorum/5-nodes.png)

- Puede sobrevivir a un error de servidor: **Sí**.
- Puede sobrevivir a un error de un servidor y luego otro: **Sí**.
- Puede sobrevivir a dos errores de servidor a la vez: **Sí**. 

Ahora que entendemos cómo funciona el cuórum, echemos un vistazo a los tipos de testigos de cuórum.

### <a name="quorum-witness-types"></a>Tipos de testigo de quórum

Agrupación en clústeres de conmutación por error admite tres tipos de testigos de quórum:

- **[Testigo en la nube](../../failover-clustering/deploy-cloud-witness.md)**  -almacenamiento de blobs en Azure puede tener acceso todos los nodos del clúster. Mantiene la información de agrupación en clústeres en el archivo witness.log, pero no almacena una copia de la base de datos del clúster.
- **Testigo del recurso compartido de archivos** : un recurso compartido que está configurada en un servidor de archivos que ejecutan Windows Server. Mantiene la información de agrupación en clústeres en el archivo witness.log, pero no almacena una copia de la base de datos del clúster.
- **Testigo de disco** -un disco clúster pequeño que se encuentra en el grupo de almacenamiento de clúster disponibles. Este disco es de alta disponibilidad y puede conmutar por error entre nodos. Contiene una copia de la base de datos del clúster.  ***No se admite un testigo de disco con espacios de almacenamiento directo***.

## <a name="pool-quorum-overview"></a>Información general sobre el cuórum de grupo

Acabamos de acerca del quórum de clúster, que funciona en el nivel de clúster. Ahora, vamos a profundizar en el grupo de quórum, que opera en el nivel de grupo (es decir, puede perder los nodos y las unidades y tiene el grupo de estar al día). Los grupos de almacenamiento se diseñaron para usarse en escenarios clústeres y no agrupados, motivo por el que tienen un mecanismo de quórum diferentes.

En la tabla siguiente ofrece una visión general de los resultados de Cuórum de grupo por cada escenario:

| Nodos de servidor | Puede sobrevivir a un error de nodo de servidor | Puede sobrevivir a un error de nodo de un servidor y, después, otro | Puede sobrevivir a dos errores de nodo de servidor simultáneas |
|--------------|-------------------------------------|---------------------------------------------------|----------------------------------------------------|
| 2            | No                                  | No                                                | No                                                 |
| 2 + testigo  | Sí                                 | No                                                | No                                                 |
| 3            | Sí                                 | No                                                | No                                                 |
| 3 + testigo  | Sí                                 | No                                                | No                                                 |
| 4            | Sí                                 | No                                                | No                                                 |
| 4 + testigo  | Sí                                 | Sí                                               | Sí                                                |
| 5 y versiones posteriores  | Sí                                 | Sí                                               | Sí                                                |

## <a name="how-pool-quorum-works"></a>Cómo funciona el cuórum de grupo

Cuando las unidades fallan o cuando pierde el contacto con otro subconjunto algún subconjunto de las unidades, necesitan unidades que permanece comprobar que constituyen el *mayoría* del grupo permanezcan en línea. Si no se comprueba que, vayan sin conexión. El grupo es la entidad que se queda sin conexión o permanece en línea en función de si tiene suficientes discos para el quórum (50% + 1). El propietario del recurso de grupo (nodo de clúster activo) puede ser + 1.

Pero el cuórum de grupo funciona de manera diferente de quórum de clúster de las maneras siguientes:

- el grupo usa un nodo del clúster como un testigo como un factor de desempate para sobrevivir a la mitad de las unidades pasadas (este nodo es el propietario del recurso de grupo)
- el grupo no tiene quórum dinámico
- el grupo de implementar su propia versión de la eliminación de un voto

### <a name="examples"></a>Ejemplos

#### <a name="four-nodes-with-a-symmetrical-layout"></a>Cuatro nodos con un diseño simétrico. 
Cada una de las 16 unidades de disco tiene un voto y dos nodo también tiene un voto (ya que es el propietario del recurso de grupo). El *mayoría* viene de un total de **16 votos**. Si los nodos, tres y cuatro dejan de funcionar, el subconjunto que permanece tiene 8 unidades y el propietario del recurso de grupo, que es 9/16 votos. Por lo tanto, el grupo sobrevive.

![Grupo de quórum 1](media/understand-quorum/pool-1.png)

- Puede sobrevivir a un error de servidor: **Sí**.
- Puede sobrevivir a un error de un servidor y luego otro: **Sí**.
- Puede sobrevivir a dos errores de servidor a la vez: **Sí**. 

#### <a name="four-nodes-with-a-symmetrical-layout-and-drive-failure"></a>Cuatro nodos con un error de diseño y la unidad simétrico. 
Cada una de las 16 unidades de disco tiene un voto y el nodo 2 también tiene un voto (ya que es el propietario del recurso de grupo). El *mayoría* viene de un total de **16 votos**. En primer lugar, unidad 7 deja de funcionar. Si los nodos, tres y cuatro dejan de funcionar, el subconjunto que permanece tiene 7 unidades y el propietario del recurso de grupo, que es 8/16 votos. Por lo tanto, el grupo no tiene la mayoría y deja de funcionar.

![Grupo de quórum 2](media/understand-quorum/pool-2.png)

- Puede sobrevivir a un error de servidor: **Sí**.
- Puede sobrevivir a un error de un servidor y luego otro: **No**.
- Puede sobrevivir a dos errores de servidor a la vez: **No**. 

#### <a name="four-nodes-with-a-non-symmetrical-layout"></a>Cuatro nodos con un diseño que no son simétricos. 
Cada una de las 24 unidades de disco tiene un voto y dos nodo también tiene un voto (ya que es el propietario del recurso de grupo). El *mayoría* viene de un total de **24 votos**. Si los nodos, tres y cuatro dejan de funcionar, el subconjunto que permanece tiene 8 unidades y el propietario del recurso de grupo, que es 9/24 votos. Por lo tanto, el grupo no tiene la mayoría y deja de funcionar.

![Grupo de quórum 3](media/understand-quorum/pool-3.png)

- Puede sobrevivir a un error de servidor: **Sí**.
- Puede sobrevivir a un error de un servidor y luego otro: ** Depends ** (no se puede sobrevivir si ambos nodos, tres y cuatro dejan de funcionar, pero pueden sobrevivir a todos los demás escenarios.
- Puede sobrevivir a dos errores de servidor a la vez: ** Depends ** (no se puede sobrevivir si ambos nodos, tres y cuatro dejan de funcionar, pero pueden sobrevivir a todos los demás escenarios.

### <a name="pool-quorum-recommendations"></a>Recomendaciones de quórum de grupos

- Asegúrese de que cada nodo del clúster es simétrica (cada nodo tiene el mismo número de unidades)
- Habilitar triple o paridad doble para que pueda tolerar errores de un nodo y mantener los discos virtuales en línea. Consulte nuestra [página instrucciones de volumen](plan-volumes.md) para obtener más detalles.
- Si un disco en otro nodo y más de dos nodos están inactivos o dos nodos están inactivos, volúmenes no pueden tener acceso a las tres copias de sus datos y por lo tanto, dejarse sin conexión y no esté disponible. Se recomienda para devolver los servidores o reemplazar los discos rápidamente para garantizar la resistencia de la mayoría de todos los datos en el volumen.

## <a name="more-information"></a>Más información

- [Configurar y administrar el cuórum](../../failover-clustering/manage-cluster-quorum.md)
- [Implementación de un testigo en la nube](../../failover-clustering/deploy-cloud-witness.md)
