---
title: Descripción del bloqueo (o la falta de bloqueo) de archivos distribuidos en DFSR
description: En este artículo se describe la ausencia de un mecanismo de bloqueo de archivos distribuido de varios hosts dentro de Windows y, en concreto, en carpetas replicadas por DFSR.
ms.date: 06/10/2020
author: Deland-Han
ms.author: delhan
ms.openlocfilehash: 4c04ed0f97f3f6192817121bc1bd67ea8fc73fdd
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87965862"
---
# <a name="understanding-the-lack-of-distributed-file-locking-in-dfsr"></a>Descripción del bloqueo (o la falta de bloqueo) de archivos distribuidos en DFSR

En este artículo se describe la ausencia de un mecanismo de bloqueo de archivos distribuido de varios hosts dentro de Windows y, en concreto, en carpetas replicadas por DFSR.

## <a name="some-background"></a>Información general

  - Bloqueo de archivos distribuido: se refiere al concepto de tener varias copias de un archivo en varios equipos y, cuando se abre un archivo para escribir en él, se bloquean todas las demás copias. Esto evita que varios usuarios modifiquen un archivo en varios servidores al mismo tiempo.
  - Replicación de Sistema de archivos distribuido: [DFSR](/previous-versions/windows/desktop/dfsr/distributed-file-system-replication--dfsr-) funciona en un diseño de varios maestros y basado en el estado. En la replicación basada en estado, cada servidor del sistema con varios maestros aplica las actualizaciones a su réplica a medida que llegan, sin intercambiar los archivos de registro (en su lugar, usa vectores de versión para mantener la información de "actualización"). No hay ningún servidor autoritativo de forma arbitraria después de la sincronización inicial, por lo que tiene una alta disponibilidad y es muy flexible en varias topologías de red.
  - Bloque de mensajes del servidor: [SMB](/openspecs/windows_protocols/ms-smb/f210069c-7086-4dc2-885e-861d837df688) es el protocolo común que se usa en Windows para tener acceso a los archivos a través de la red. En términos simplificados, es un protocolo cliente-servidor que hace que el uso de un redirector para que los sistemas de archivos remotos parezcan sistemas de archivos locales. No es específico de Windows y es bastante común: un ejemplo conocido que no es de Microsoft es Samba, que permite que Linux, Mac y otros sistemas operativos actúen como clientes o servidores SMB y participen en redes de Windows.


Es importante realizar una desactivación de los lugares en los que DFSR y SMB viven en el entorno de datos replicados. SMB permite a los usuarios tener acceso a sus archivos y no tiene conocimiento de DFSR. Del mismo modo, DFSR (mediante el protocolo RPC) mantiene los archivos sincronizados entre los servidores y no reconoce SMB. No confunda el bloqueo distribuido tal y como se define en esta entrada y el [bloqueo oportunista](/windows/win32/fileio/opportunistic-locks).

Aquí es donde las cosas pueden ir en forma de pera, como Brits.

Dado que los usuarios pueden modificar los datos en varios servidores, y dado que cada servidor de Windows solo conoce un bloqueo de archivo en sí mismo, *y* dado que [DFSR no sabe nada sobre los bloqueos en otros servidores](/previous-versions/windows/it-pro/windows-server-2003/cc773238(v=ws.10)), es posible que los usuarios sobrescriban los cambios de los demás. DFSR utiliza un algoritmo de conflicto "último escritor gana", por lo que alguien tiene que perder y la persona que se guarda en último lugar para conservar los cambios. La copia de archivo que pierde se encuentra en la carpeta *ConflictAndDeleted* .

Ahora, esto es mucho *menos* común que el de personas como creo. Normalmente, los archivos compartidos verdaderos se modifican en un entorno local; en la sucursal o en la misma fila de cubículos. Normalmente, los usuarios trabajan en el mismo equipo, por lo que las personas suelen tener en cuenta los compañeros que modifican los datos. Y como están normalmente en el mismo sitio, las probabilidades son mucho mayores que todos los usuarios que trabajan en un documento compartido van a usar el mismo servidor. Windows SMB controla la situación aquí. Cuando un usuario tiene un archivo bloqueado para su modificación y su compañero intenta editarlo, el otro usuario obtendrá un error similar al siguiente:

![Archivo en uso](./media/understanding-the-lack-of-distributed-file-locking-in-dfsr/1.jpg)

Además, si la aplicación que abre el archivo es realmente inteligente, como Word 2007, puede proporcionarle:

![Archivo en uso](./media/understanding-the-lack-of-distributed-file-locking-in-dfsr/2.jpg)

DFSR tiene un mecanismo para los archivos bloqueados, pero solo está dentro del propio contexto del servidor. DFSR no replicará un archivo dentro o fuera si su copia local tiene un bloqueo exclusivo. Pero esto no impide que nadie en otro servidor modifique el archivo.

De nuevo en el tema *, existe el* problema de que los datos compartidos que se están modificando geográficamente existen y, para algunas personas, es bastante gnarly. En ocasiones se pregunta por qué DFSR no controla este bloqueo y toma todo con una ola de la varita mágica. Resulta que es un escenario interesante y difícil de resolver para un sistema de replicación con varios maestros. Comencemos a explorar.

## <a name="third-party-solutions"></a>Soluciones de terceros

Hay algunas soluciones de proveedor que se aplican a este problema, que suelen abordarse a través de uno o varios de los métodos siguientes \* :

  - Uso de un mecanismo de agente

> Tener un "COP de tráfico" central permite que un servidor tenga en cuenta todos los demás servidores y los archivos bloqueados por los usuarios. Desafortunadamente, esto también significa que, a menudo, hay un único punto de error en el sistema de bloqueo distribuido.

![Topología](./media/understanding-the-lack-of-distributed-file-locking-in-dfsr/3.png)

  - Requisito de una red totalmente enrutada

> Dado que un agente central debe ser capaz de comunicarse con todos los servidores que participan en la replicación de archivos, esto elimina la capacidad de administrar topologías de red complejas. Las topologías en anillo y las topologías de varios concentradores y radios no suelen ser posibles. En una red no enrutada totalmente, es posible que algunos servidores no puedan ponerse en contacto directamente con el otro o con un agente, y solo pueden comunicarse con un asociado que pueda comunicarse con otro servidor, etc. Esto está bien en un entorno con varios maestros, pero no con un mecanismo de agente.

![Topología](./media/understanding-the-lack-of-distributed-file-locking-in-dfsr/4.png)

  - Se limitan a un par de servidores

> Algunas soluciones limitan la topología a un par de servidores con el fin de simplificar el mecanismo de bloqueo distribuido. En entornos más grandes, es posible que esto no sea factible.

  - Uso de agentes en clientes y servidores
  - No usar replicación de varios maestros
  - No hacer uso de la agrupación en clústeres de MS
  - Uso de dispositivos especializados


***\*** Tenga en cuenta que, por lo general, \! no se pueden exponer amenazas de muerte porque tiene una solución que no implementa uno o más de estos métodos.\!*

## <a name="deeper-thoughts"></a>Pensamientos más profundos

A medida que cree más información sobre este problema, algunos problemas fundamentales comenzarán a recortarse. Por ejemplo, si tenemos cuatro servidores con datos que los usuarios pueden modificar en cuatro sitios y la conexión WAN a uno de ellos se queda sin conexión, ¿qué hacemos? Los usuarios aún pueden tener acceso a sus servidores individuales, pero deberían permitirlos. No queremos que estos cambios entren en conflicto, pero definitivamente queremos que sigan trabajando y hagan que el dinero de su empresa sea económico. Si bloqueamos arbitrariamente los cambios en ese momento, ningún usuario puede trabajar aunque no haya ningún conflicto en realidad.\! No hay ninguna manera de indicar a los demás servidores que el archivo está en uso y que vuelve al cuadrado uno.

![Topología](./media/understanding-the-lack-of-distributed-file-locking-in-dfsr/5.png)

A continuación, hay un SMB y el control de errores de los bloqueos de informes. En realidad, no podemos cambiar el modo en que SMB informa de las infracciones de recursos compartidos, ya que en una gran cantidad de aplicaciones y clientes no se sabrían nuevos mensajes de error extendido. Las aplicaciones como Word 2007 realizan algunas bazas difíciles de descubrir para averiguar quién está bloqueando los archivos, pero la inmensa mayoría de las aplicaciones no saben quién tiene un archivo en uso (o incluso si existe SMB). Realmente). Por lo tanto, cuando un usuario recibe el mensaje "este archivo está en uso", no es especialmente útil: ¿todos ellos llaman al Departamento de soporte técnico? ¿El Departamento de soporte técnico tiene acceso a todos los servidores de archivos para ver qué usuarios tienen acceso a los archivos? Desordenados.

Dado que deseamos la alta disponibilidad de los servidores maestros, es menos recomendable un sistema de agente. es posible que necesitemos algo que se ejecute en todos los servidores que les permitan comunicarse, incluso a través de redes no enrutadas por completo. Esto requerirá técnicas de sincronización muy complejas. Agregará cierta sobrecarga en la red (aunque probablemente no sea mucho) y tendrá que ser increíblemente rápido para asegurarse de que no se mantiene al usuario en su trabajo. es necesario que Outrun la replicación de archivos en sí; de hecho, puede que tenga que estar ligada realmente a la replicación de algún modo. También tendrá que tener en cuenta las interrupciones del servidor que están relacionadas con la red y no los bloqueos del servidor, de algún modo.

![Topología](./media/understanding-the-lack-of-distributed-file-locking-in-dfsr/6.png)

Y, a continuación, regresamos al software cliente especial para este escenario que entiende mejor los bloqueos y que puede proporcionar al usuario alguna información útil ("go Call Susie in Accounting e indíquele que publique ese documento", "lo sentimos, la topología de bloqueo de archivos se interrumpe y el administrador le impide abrir este archivo hasta que se corrija", etc.). La obtención de esta práctica con los millones de aplicaciones que se ejecutan en Windows será interesante. Hay muchos sistemas operativos que no se admitirían u obtengan el software: Windows 2000 está fuera del soporte estándar y XP pronto lo estará. Los clientes de Linux y Mac no dispondrán de este software hasta que pensaban que era importante, por lo que el cliente tendría que esperar que los proveedores fueran algo análogos.

## <a name="more-inforamtion"></a>Más Information

En la actualidad, la manera más fácil de controlar esta situación en DFSR es usar espacios de nombres DFS para guiar a los usuarios en ubicaciones de predicción, con un espacio de nombres coherente. Al configurar correctamente la topología del sitio de DFSN y los vínculos del servidor, obliga a los usuarios a compartir el mismo servidor local y solo les permite acceder a los equipos remotos cuando su servidor "principal" esté inactivo. Para la mayoría de los entornos, esto funciona bastante bien. Como alternativa a DFSR, SharePoint es una opción por su sistema de desprotección/protección. BranchCache (próximamente en Windows Server 2008 R2 y Windows 7) puede ser una opción que se ha diseñado para facilitar la lectura de archivos en un escenario de bifurcación, pero, al final, los datos autoritativos permanecerán activos solo en un servidor: más información [aquí](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj127252(v=ws.11)). Y de nuevo, estos proveedores tienen sus soluciones.

