---
ms.assetid: fd427da3-3869-428f-bf2a-56c4b7d99b40
title: Bloquear la clonación en ReFS
author: gawatu
ms.author: gawatu
manager: gawatu
ms.date: 10/17/2018
ms.topic: article
ms.prod: windows-server
ms.technology: storage-file-systems
ms.openlocfilehash: cd74468029ff973846ddfd10cce8ba0e26a607e9
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "86961397"
---
# <a name="block-cloning-on-refs"></a>Bloquear la clonación en ReFS

>Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server (canal semianual)

La clonación de bloques da la orden al sistema de archivos para que copie un intervalo de bytes del archivo en nombre de una aplicación, en el que el archivo de destino puede que sea el mismo o diferente del archivo de origen. Las operaciones de copia son, desgraciadamente, costosas ya que se desencadenan caras lecturas y escrituras en los datos físicos subyacentes.

La clonación de bloques en ReFS, sin embargo, realiza copias de metadatos con una operación de bajo coste en lugar de leer y escribir los datos de los archivos. Ya que ReFS permite que varios archivos compartan los mismos clústeres lógicos (ubicaciones físicas en un volumen), las operaciones de copia solo necesitan volver a asignar una región de un archivo a una ubicación física independiente, convirtiendo así una costosa operación física en una operación rápida y lógica. Esto permite que las copias se realicen más rápido y se generen menos E/S en el almacenamiento subyacente. Esta mejora también beneficia a la virtualización de las cargas de trabajo, como las operaciones de fusión de punto de control .vhdx, que se aceleran considerablemente al usar las operaciones de clonación de bloques. Además, ya que varios archivos pueden compartir los mismos clústeres lógicos, los datos idénticos no se almacenarán físicamente varias veces y se mejorará la capacidad de almacenamiento.

## <a name="how-it-works"></a>Funcionamiento

La clonación de bloques en ReFS convierte una operación de archivo de datos en una operación de metadatos. Para llevar a cabo esta optimización, ReFS presenta recuentos de referencia en los metadatos para las regiones que se han copiado. Este recuento de referencias registra el número de las distintas regiones de archivo que hacen referencia a las mismas regiones físicas. Esto permite a varios archivos compartir los mismos datos físicos:

![Mostrar actualizaciones de recuento de referencia cuando varios archivos hacen referencia a la misma región](media/ref-count-example.gif)

Al mantener un recuento de referencias para cada clúster lógico, ReFS no interrumpe el aislamiento entre archivos: escrituras en regiones compartidas desencadenan un mecanismo de asignación de escritura, donde ReFS asigna una nueva región para la escritura entrante. Este mecanismo conserva la integridad de los clústeres lógicos compartidos.

### <a name="example"></a>Ejemplo
Supongamos que hay dos archivos, X e Y, que cada archivo se compone de tres regiones y cada región se asigna para separar los clústeres lógicos.

![Dos archivos, cada uno de ellos con tres regiones distintas que se asignan a las regiones con un recuento de referencia 1](media/block-clone-1.png)

Ahora supongamos que una aplicación realiza una operación de clonación de bloques del archivo X al archivo Y para que las regiones A y B se copien en el desplazamiento de la región E. A continuación, se produciría el siguiente estado en el sistema de archivos:

![Recuento de referencias muestra 2 en la región de bloques clonados.](media/block-clone-2.png)

Este estado en el sistema de archivos revela una duplicación con éxito de la región de bloques clonados. Dado que ReFS realiza esta operación de copia mediante la actualización de asignaciones VCN a LCN, no se podían leer datos físicos ni se sobrescribieron datos físicos en el archivo Y. Los archivos X e Y ahora comparten clústeres lógicos, reflejados en los recuentos de referencia en la tabla. Ya que no hay datos copiados físiciamente, ReFS reduce el consumo de capacidad en el volumen.

Ahora supongamos que la aplicación intenta sobrescribir la región A en el archivo X. ReFS duplicará la región compartida, actualizará los recuentos de referencia de manera adecuada y realizará la escritura entrante en la región que acaba de duplicarse. Esto asegura que el aislamiento entre los archivos se conserve.

![Aislamiento conservado al escribir en una nueva región G y al actualizar los recuentos de referencia](media/block-clone-3.png)

Tras de la escritura de modificación, la región B aún se comparte en ambos archivos. Ten en cuenta que si la región A fuera mayor que un clúster, solo el clúster modificado se habría duplicado y el resto seguiría siendo compartido.


## <a name="functionality-restrictions-and-remarks"></a>Comentarios y restricciones de funcionalidad
- La región de origen y de destino deben comenzar y terminar en los límites de un clúster.
- La región clonada debe ser inferior a 4 GB de longitud.
- El número máximo de regiones de archivo que se pueden asignar a la misma región física es 8175.
- La región de destino no debe extenderse más allá del final del archivo. Si la aplicación desea extender la región de destino con datos clonados, se debe llamar en primer lugar a [SetEndOfFile](/windows/win32/api/fileapi/nf-fileapi-setendoffile).
- Si las regiones de origen y destino están en el mismo archivo, no deben superponerse. (La aplicación puede continuar dividiendo la operación de clonación de bloques en varios bloques de clones que ya no se superpongan).
- Los archivos de origen y de destino deben tener el mismo volumen ReFS.
- Los archivos de origen y de destino deben tener la misma configuración de las [secuencias de integridad](/windows/win32/fileio/file-attribute-constants).
- Si el archivo de origen es disperso, el archivo de destino también debe serlo.
- La operación de clonación de bloques interrumpirá bloqueos oportunistas compartidos (también conocidos como [bloqueos oportunistas de nivel 2](/windows/win32/fileio/types-of-opportunistic-locks)).
- El volumen de ReFS debe haber sido formateado con Windows Server 2016, y si el clúster de conmutación por error se ha utilizado, el nivel funcional del clúster debe haber sido formateado con Windows Server 2016 o posterior en el momento de formatear.

## <a name="additional-references"></a>Referencias adicionales

-   [Información general de ReFS](refs-overview.md)
-   [Flujos de integridad de ReFS](integrity-streams.md)
-   [Información general de Espacios de almacenamiento directo](../storage-spaces/storage-spaces-direct-overview.md)
-   [DUPLICATE_EXTENTS_DATA](/windows/win32/api/winioctl/ns-winioctl-duplicate_extents_data)
-   [FSCTL_DUPLICATE_EXTENTS_TO_FILE](/windows/win32/api/winioctl/ni-winioctl-fsctl_duplicate_extents_to_file)
