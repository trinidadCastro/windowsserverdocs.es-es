---
title: Cuórum de clúster y grupo de descripción
description: Descripción de clúster y grupo de Cuórum, con ejemplos específicos para ir a través de la dificultad.
keywords: Espacios de almacenamiento directo, Cuórum, testigo, S2D, Cuórum, Cuórum de grupo, clúster, agrupación de clústeres
ms.prod: windows-server-threshold
ms.author: adagashe
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: adagashe
ms.date: 01/18/2019
ms.localizationpriority: medium
ms.openlocfilehash: 24890b191db8bc6934132857e830d4f77c394b02
ms.sourcegitcommit: 28dc7c7f1e44fee7ab2112228af329a9ce0e02ba
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2019
ms.locfileid: "9024160"
---
# Cuórum de clúster y grupo de descripción

>Se aplica a: Windows Server 2019, Windows Server 2016

[Clústeres de conmutación por error de Windows Server](../../failover-clustering/failover-clustering-overview.md) proporciona alta disponibilidad de las cargas de trabajo. Estos recursos se consideran altamente disponibles si los nodos que hospedan los recursos son Sin embargo, el clúster requiere normalmente más de la mitad de los nodos que se esté ejecutando, que se conoce como tener *cuórum*.

Cuórum está diseñado para evitar *Brain* escenarios, lo cual pueden suceder cuando hay una partición en la red y subconjuntos de nodos no se comunican entre sí. Esto puede provocar ambos subconjuntos de nodos para intentar el propietario de la carga de trabajo y escribir en el mismo disco que se puede producir muchos problemas. Sin embargo, esto no es posible con el concepto de clústeres de conmutación por error de cuórum que fuerza solo uno de estos grupos de nodos siga ejecutándose, por lo que solo uno de estos grupos permanecerán en línea.

Cuórum determina el número de errores que puede admitir el clúster mientras aún está en línea. Cuórum está diseñada para controlar el escenario cuando se produce un problema con la comunicación entre subconjuntos de nodos de clúster, para que no intentan varios servidores alojar un grupo de recursos al mismo tiempo y escribir en el mismo disco al mismo tiempo. Al tener este concepto de cuórum, el clúster forzará el servicio de clúster se detenga en uno de los subconjuntos de nodos para asegurarse de que hay un solo propietario de un determinado grupo de recursos. Una vez que los nodos que se han detenido una vez más pueden comunicarse con el grupo principal de nodos, automáticamente se a unir el clúster y empezar a su servicio de clúster.

En Windows Server 2016, hay dos componentes del sistema que tienen sus propios mecanismos de cuórum:

- <strong>Cuórum de clúster</strong>: Esto funciona en el nivel de clúster (es decir, puede perder nodos y hacer que el clúster permanecen en)
- <strong>Grupo de Cuórum</strong>: Esto funciona en el nivel de grupo, cuando se habilita espacios de almacenamiento directo (es decir, puede perder nodos y unidades y hacer que el grupo mantienen actualizados). Los grupos de almacenamiento se han diseñado para usarse en escenarios en clúster y no agrupados, motivo por el que tienen un mecanismo de cuórum diferentes.

## Información general de cuórum de clúster

La siguiente tabla ofrece una visión general de los resultados de Cuórum de clúster por escenario:

| Nodos de servidor | Puede sobrevivir a un error de nodo de servidor | Puede sobrevivir error de nodo de un servidor, a continuación, otro | Puede sobrevive a dos errores de nodo de servidor simultáneos |
|--------------|-------------------------------------|---------------------------------------------------|----------------------------------------------------|
| 2            | 50/50                               | No                                                | No                                                 |
| 2 + testigo  | Sí                                 | No                                                | No                                                 |
| 3            | Sí                                 | 50/50                                             | No                                                 |
| 3 + testigo  | Sí                                 | Sí                                               | No                                                 |
| 4            | Sí                                 | Sí                                               | 50/50                                              |
| 4 + testigo  | Sí                                 | Sí                                               | Sí                                                |
| 5 y superior  | Sí                                 | Sí                                               | Sí                                                |

### Recomendaciones de cuórum de clúster

- Si tienes dos nodos, es un testigo <strong>requiere</strong>.
- Si tiene tres o cuatro nodos, testigo es <strong>recomienda</strong>.
- Si tienes acceso a Internet, usa un <strong> [testigo en la nube](../../failover-clustering/deploy-cloud-witness.md)</strong>
- Si estás en un entorno de TI con otros equipos y recursos compartidos de archivos, usa un testigo del recurso compartido de archivos

## Cómo funciona el cuórum de clúster

Cuando se produce un error nodos, o cuando un subconjunto de nodos pierde contacto con otro subconjunto, consultando los nodos que compruebe que constituyen la *mayor parte* del clúster que permanezcan en línea. Si no se comprueban, pasan sin conexión.

Pero el concepto de *mayoría* solo funciona perfectamente cuando el número total de los nodos del clúster es impar (por ejemplo, tres nodos en un clúster de cinco nodos). Por lo tanto, ¿qué acerca de los clústeres con un número par de nodos (por ejemplo, un clúster de cuatro nodos)?

Existen dos formas para hacer el *número total de votos* extrañas el clúster:

1. En primer lugar, puede ir *de* una mediante la adición de un *testigo* con un voto extra. Esto requiere una configuración específica del usuario.
2.  O bien, puede ir *hacia abajo* una eliminando todas voto de una suerte del nodo (ocurre automáticamente según sea necesario).

Siempre que los nodos que sigue funcionando correctamente comprobar son la *mayoría*, se actualiza la definición de la *mayoría* para estar entre solo la supervivencia. Esto permite que el clúster pierda un nodo, otro, a continuación, otro, y así sucesivamente. Este concepto de la adaptación de *número total de votos* después sucesivos errores se conoce como <strong> *cuórum dinámico*</strong>.  

### Testigo dinámico

Testigo dinámico activa o desactiva el voto de testigo para asegurarte de que el *número total de votos* es impar. Si hay un número de votos impar, el testigo no tiene un voto. Si hay un número par de votos, el testigo tiene un voto. Testigo dinámico reduce significativamente el riesgo de que el clúster se bloquearse debido a un error de testigo. El clúster decide si vas a usar el voto de testigo basado en el número de nodos voto que están disponibles en el clúster.

Cuórum dinámica funciona con dinámico testigo en la forma en que se describe a continuación.

### Comportamiento de cuórum dinámico

- Si tienes un <strong>incluso</strong> número de nodos y ninguna testigo, *un nodo obtiene su voto ceros*. Por ejemplo, sólo tres de los cuatro nodos Obtén votos, por lo que es el *número total de votos* tres y dos supervivencia con votos se considera mayoría.
- Si tienes un <strong>extrañas</strong> número de nodos y ninguna testigo, *entran votos*.
- Si tienes un <strong>incluso</strong> número de nodos, además de testigo, *vota el testigo*, por lo que el total es extrañas.
- Si tienes un <strong>extrañas</strong> número de nodos, además de testigo, *el testigo no votar*.

Cuórum dinámica permite la capacidad de asignar un voto a un nodo dinámicamente para evitar la pérdida de la mayoría de votos y permitir que el clúster se ejecute con un nodo (conocido como último-man permanente). Echemos un clúster de cuatro nodos como ejemplo. Se supone que cuórum requiere 3 votos. 

En este caso, el clúster enviaban hacia abajo si ha perdido dos nodos.

![Diagrama que muestra cuatro nodos de clúster, cada una de ellas Obtiene un voto](media/understand-quorum/dynamic-quorum-base.png)

Sin embargo, cuórum dinámico impide que esto ocurra. El *número total de votos* necesarios para el cuórum ahora se determina según el número de nodos disponibles. Por lo tanto, cuórum dinámico, el clúster permanecerá arriba, incluso si se pierden tres nodos.

![Diagrama que muestra cuatro nodos de clúster, con nodos que se produzca un error de uno en un momento y el número de votos necesarios ajustar después de cada error.](media/understand-quorum/dynamic-quorum-step-through.png)

El escenario anterior se aplica a un clúster general que no tenga espacios de almacenamiento directo habilitado. Sin embargo, cuando se habilita espacios de almacenamiento directo, el clúster puede admitir solo dos errores en nodos. Esto se explica más en la [sección de cuórum de grupo](#poolQuorum).

### Ejemplos

#### Dos nodos sin un testigo. 
Voto de un nodo se pone a cero, por lo que se determina *la mayoría* de un total de <strong>1 voto</strong>. Si el nodo no voto inesperadamente deja de funcionar, la supervivencia tiene 1/1 y prevalecerá en el clúster. Si el nodo voto inesperadamente deja de funcionar, la supervivencia tiene 0 o 1 y el clúster deja de funcionar. Si el nodo voto está apagado correctamente, el voto se transfiere a otro nodo y prevalecerá en el clúster. *<strong>Esto es por eso es fundamental para configurar a un testigo.</strong>*

![Se explica en el caso de dos nodos sin un testigo de cuórum](media/understand-quorum/2-node-no-witness.png)

- Puede sobrevivir a un error de servidor: <strong>posibilidad de 50 por ciento</strong>.
- Puede sobrevivir error de un servidor, a continuación, otro: <strong>No</strong>.
- Puede sobrevive a dos errores de servidor a la vez: <strong>No</strong>. 

#### Dos nodos con un testigo. 
Voten de ambos nodos, además de los votos testigo, por lo que se determina la *mayor parte* de un total de <strong>3 votos</strong>. Si ninguno de los nodos se desconecta, la supervivencia tiene 2 o 3 y prevalecerá en el clúster.

![Se explica en el caso de dos nodos con un testigo de cuórum](media/understand-quorum/2-node-witness.png)

- Puede sobrevivir a un error de servidor: <strong>Sí</strong>.
- Puede sobrevivir error de un servidor, a continuación, otro: <strong>No</strong>.
- Puede sobrevive a dos errores de servidor a la vez: <strong>No</strong>. 

#### Tres nodos sin un testigo.
Voten de todos los nodos, por lo que se determina la *mayor parte* de un total de <strong>3 votos</strong>. Si cualquier nodo deja de funcionar, la supervivencia es 2/3 y prevalecerá en el clúster. El clúster se convierte en dos nodos sin un testigo: en ese momento, estás en el escenario 1.

![Se explica en el caso de tres nodos sin un testigo de cuórum](media/understand-quorum/3-node-no-witness.png)

- Puede sobrevivir a un error de servidor: <strong>Sí</strong>.
- Puede sobrevivir error de un servidor, a continuación, otro: <strong>posibilidad de 50 por ciento</strong>.
- Puede sobrevive a dos errores de servidor a la vez: <strong>No</strong>. 

#### Tres nodos con un testigo.
Todos los nodos votación, por lo que el testigo inicialmente no votar. Se determina la *mayor parte* de un total de <strong>3 votos</strong>. Después de un error, el clúster tiene dos nodos con un testigo: que vuelve a escenario 2. Por lo tanto, ahora los dos nodos y el testigo votación.

![Se explica en el caso de tres nodos con un testigo de cuórum](media/understand-quorum/3-node-witness.png)

- Puede sobrevivir a un error de servidor: <strong>Sí</strong>.
- Puede sobrevivir error de un servidor, a continuación, otro: <strong>Sí</strong>.
- Puede sobrevive a dos errores de servidor a la vez: <strong>No</strong>. 

#### Cuatro nodos sin un testigo
Voto de un nodo se pone a cero, por lo que se determina la *mayor parte* de un total de <strong>3 votos</strong>. Después de un error, el clúster se convierte en tres nodos y estás en el escenario 3.

![Se explica en el caso de cuatro nodos sin un testigo de cuórum](media/understand-quorum/4-node-no-witness.png)

- Puede sobrevivir a un error de servidor: <strong>Sí</strong>.
- Puede sobrevivir error de un servidor, a continuación, otro: <strong>Sí</strong>.
- Puede sobrevive a dos errores de servidor a la vez: <strong>posibilidad de 50 por ciento</strong>. 

#### Cuatro nodos con un testigo.
Votos de todos los nodos y los votos testigo, por lo que se determina la *mayor parte* de un total de <strong>5 votos</strong>. Después de un error, estás en el escenario 4. Después de dos errores simultáneos, se omite hasta 2 del escenario.

![Se explica en el caso de cuatro nodos con un testigo de cuórum](media/understand-quorum/4-node-witness.png)

- Puede sobrevivir a un error de servidor: <strong>Sí</strong>.
- Puede sobrevivir error de un servidor, a continuación, otro: <strong>Sí</strong>.
- Puede sobrevive a dos errores de servidor a la vez: <strong>Sí</strong>. 

#### Cinco nodos y mucho más.
Voten de todos los nodos, o todos, pero un voto, cualquier hace que el total extrañas. Espacios de almacenamiento directo no se pueden manipular más de dos nodos hacia abajo de todos modos, por lo que en este punto, ningún testigo es necesario o útil.

![Cuórum se explica en el caso con cinco nodos y versiones posteriores](media/understand-quorum/5-nodes.png)

- Puede sobrevivir a un error de servidor: <strong>Sí</strong>.
- Puede sobrevivir error de un servidor, a continuación, otro: <strong>Sí</strong>.
- Puede sobrevive a dos errores de servidor a la vez: <strong>Sí</strong>. 

Ahora que sabemos cómo funciona el cuórum, echemos un vistazo a los tipos de testigos de cuórum.

### Tipos de testigo de cuórum

Clústeres de conmutación por error admite tres tipos de testigos de Cuórum:

- <strong>[En la nube testigo](../../failover-clustering\deploy-cloud-witness.md) </strong> -Blob de almacenamiento en Azure puede tener acceso a todos los nodos del clúster. Mantiene la información de agrupación en clústeres en un archivo witness.log, pero no almacenar una copia de la base de datos del clúster.
- <strong>Testigo de recurso compartido de archivos</strong> : recurso compartido de archivos de SMB A que esté configurado en un servidor de archivos que ejecutan Windows Server. Mantiene la información de agrupación en clústeres en un archivo witness.log, pero no almacenar una copia de la base de datos del clúster.
- <strong>En el disco testigo</strong> -un disco de clúster pequeño que se encuentra en el grupo de almacenamiento de clúster disponibles. El disco está altamente disponible y conmutar por error entre los nodos. Contiene una copia de la base de datos del clúster.  <strong>*Un testigo de disco no es compatible con espacios de almacenamiento directo*</strong>.

## <a id="poolQuorum"></a>Introducción al grupo cuórum

Hemos analizado Cuórum de clúster, lo que funciona en el nivel de clúster. Ahora, vamos a profundizar en el grupo de Cuórum, que funciona en el nivel de grupo (es decir, se pueden perder nodos y unidades y tiene el grupo permanecen en). Los grupos de almacenamiento se han diseñado para usarse en escenarios en clúster y no agrupados, motivo por el que tienen un mecanismo de cuórum diferentes.

La siguiente tabla ofrece una visión general de los resultados de Cuórum de grupo por cada escenario:

| Nodos de servidor | Puede sobrevivir a un error de nodo de servidor | Puede sobrevivir error de nodo de un servidor, a continuación, otro | Puede sobrevive a dos errores de nodo de servidor simultáneos |
|--------------|-------------------------------------|---------------------------------------------------|----------------------------------------------------|
| 2            | No                                  | No                                                | No                                                 |
| 2 + testigo  | Sí                                 | No                                                | No                                                 |
| 3            | Sí                                 | No                                                | No                                                 |
| 3 + testigo  | Sí                                 | No                                                | No                                                 |
| 4            | Sí                                 | No                                                | No                                                 |
| 4 + testigo  | Sí                                 | Sí                                               | Sí                                                |
| 5 y superior  | Sí                                 | Sí                                               | Sí                                                |

## Cómo funciona el cuórum de grupo

Cuando se producirá un error de unidades, o cuando un subconjunto de las unidades pierde contacto con otro subconjunto, unidades restantes que compruebe que constituyen la *mayor parte* del grupo que permanezcan en línea. Si no se comprueban, pasan sin conexión. El grupo es la entidad que se desconecta o permanece en línea en función de si tiene suficientes discos para cuórum (50% + 1). El propietario del recurso de grupo (nodo de clúster activo) puede ser + 1.

Pero cuórum grupo funciona de manera diferente de cuórum de clúster de las siguientes maneras:

- el grupo utiliza un nodo del clúster como testigo como un separador de empate para sobrevivir a mitad de unidades pasadas (este nodo que sea el propietario del grupo recurso)
- el grupo no tiene cuórum dinámico
- el grupo de implementar su propia versión de la eliminación de un voto

### Ejemplos

#### Cuatro nodos con un diseño simétrico. 
Cada uno de los 16 unidades de disco tiene un voto y nodo dos también tiene un voto (ya que es el propietario del recurso de grupo). Se determina la *mayor parte* de un total de <strong>16 votos</strong>. Si los nodos tres y cuatro ir hacia abajo, el subconjunto consultando tiene 8 unidades y el propietario del grupo recurso, que es 9 o 16 votos. Por lo tanto, prevalecerá en el grupo.

![Grupo Cuórum 1](media/understand-quorum/pool-1.png)

- Puede sobrevivir a un error de servidor: <strong>Sí</strong>.
- Puede sobrevivir error de un servidor, a continuación, otro: <strong>Sí</strong>.
- Puede sobrevive a dos errores de servidor a la vez: <strong>Sí</strong>. 

#### Cuatro nodos con un error de diseño y unidad simétrico. 
Cada uno de los 16 unidades de disco tiene un voto y nodo 2 también tiene un voto (ya que es el propietario del recurso de grupo). Se determina la *mayor parte* de un total de <strong>16 votos</strong>. En primer lugar, unidad 7 deja de funcionar. Si los nodos tres y cuatro ir hacia abajo, el subconjunto consultando tiene 7 unidades y el propietario del grupo recurso, que es 8 o 16 votos. Por lo tanto, el grupo no tiene mayoría y deja de funcionar.

![Grupo Cuórum 2](media/understand-quorum/pool-2.png)

- Puede sobrevivir a un error de servidor: <strong>Sí</strong>.
- Puede sobrevivir error de un servidor, a continuación, otro: <strong>No</strong>.
- Puede sobrevive a dos errores de servidor a la vez: <strong>No</strong>. 

#### Cuatro nodos con un diseño que no sean simétricos. 
Cada uno de los 24 unidades de disco tiene un voto y nodo dos también tiene un voto (ya que es el propietario del recurso de grupo). Se determina la *mayor parte* de un total de <strong>24 votos</strong>. Si los nodos tres y cuatro ir hacia abajo, el subconjunto consultando tiene 8 unidades y el propietario del grupo recurso, que es 9/24 votos. Por lo tanto, el grupo no tiene mayoría y deja de funcionar.

![Grupo Cuórum 3](media/understand-quorum/pool-3.png)

- Puede sobrevivir a un error de servidor: <strong>Sí</strong>.
- Puede sobrevivir error de un servidor, a continuación, otro: <strong>Depends </strong>(no sobrevive a si ambos nodos tres y cuatro ir hacia abajo, pero pueden sobrevivir a todos los demás escenarios.
- Puede sobrevive a dos errores de servidor a la vez: <strong>Depends </strong>(no sobrevive a si ambos nodos tres y cuatro ir hacia abajo, pero pueden sobrevivir a todos los demás escenarios.

### Recomendaciones de cuórum de grupo

- Asegúrate de que cada nodo del clúster es simétrica (cada nodo tiene el mismo número de unidades)
- Habilitar el reflejo triple o la paridad dual para que pueda tolerar un errores en nodos y mantener los discos virtuales en línea. Consulta nuestra [página de instrucciones de volumen](plan-volumes.md) para obtener más detalles.
- Si un disco a otro nodo y más de dos nodos están inactivos o dos nodos están inactivos, volúmenes no pueden tener acceso a todos los tres copias de sus datos y, por tanto, se desconecta y que no esté disponible. Se recomienda volver a los servidores o reemplazar los discos rápidamente para garantizar que la mayoría resistencia para todos los datos en el volumen.

## Más información

- [Configurar y administrar el cuórum](../../failover-clustering/manage-cluster-quorum.md)
- [Implementación de un testigo en la nube](../../failover-clustering/deploy-cloud-witness.md)
