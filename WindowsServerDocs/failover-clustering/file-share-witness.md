---
title: Implementar a un testigo de recurso compartido de archivos en Windows Server de 2019
ms.prod: windows-server-threshold
ms.manager: eldenc
ms.technology: failover-clustering
ms.topic: article
author: johnmarlin-msft
ms.date: 01/24/2019
description: Testigos de recurso compartido de archivos le permiten usar un recurso compartido de archivos de voto de quórum de clúster. Este tema describe los testigos de recurso compartido de archivo y la nueva funcionalidad, incluido el uso de una unidad USB conectada a un enrutador como un testigo de recurso compartido de archivos.
ms.localizationpriority: medium
ms.openlocfilehash: 47371be946c08cac2f271138d701922fc340a89d
ms.sourcegitcommit: 48bb3e5c179dc520fa879b16c9afe09e07c87629
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66453037"
---
# <a name="deploy-a-file-share-witness"></a>Implementar un testigo del recurso compartido de archivos

> Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Un testigo del recurso compartido de archivos es un recurso compartido SMB que usa el clúster de conmutación por error como un voto de quórum del clúster. En este tema se proporciona información general de la tecnología y la nueva funcionalidad en Windows Server 2019, incluido el uso de una unidad USB conectada a un enrutador como un testigo de recurso compartido de archivos.

Testigos de recurso compartido de archivos son útiles en las siguientes circunstancias:  

- No se puede usar un testigo en la nube porque no todos los servidores del clúster tienen una conexión a Internet segura
- No se puede usar un testigo de disco porque no existe ningún unidades compartidas que se usará para un testigo de disco. Podría tratarse de un clúster de espacios de almacenamiento directo, SQL Server siempre en disponibilidad grupos (AG), del grupo de disponibilidad de base de datos (DAG) etcetera.  Ninguno de estos tipos de clústeres utiliza discos compartidos.

## <a name="file-share-witness-requirements"></a>Requisitos de testigo de recurso compartido de archivos

Puede hospedar a un testigo del recurso compartido de archivos en un servidor de Windows unido al dominio, o si el clúster se está ejecutando Windows Server 2019, cualquier dispositivo que puede host un SMB 2 o posterior archivo compartir.

|Tipo de servidor de archivos                 | Clústeres admitidos |
|---------------------------------|--------------------|
|Cualquier dispositivo w/un SMB 2 recurso compartido de archivos | Windows Server 2019|
|Unido al dominio de Windows Server     | Windows Server 2008 y versiones posterior|

Si el clúster se está ejecutando Windows Server 2019, estos son los requisitos:

- Un recurso compartido de archivos SMB *en cualquier dispositivo que usa el protocolo más adelante o SMB 2*, incluidos:
    - Dispositivos de almacenamiento conectado a la red (NAS)
    - Equipos de Windows Unidos a un grupo de trabajo
    - Enrutadores, con el almacenamiento USB conectado localmente
- Una cuenta local en el dispositivo para autenticar el clúster
- Si en su lugar usa Active Directory para autenticar el clúster con el recurso compartido de archivos, el objeto de nombre de clúster (CNO) debe tener permisos de escritura en el recurso compartido y el servidor debe estar en el mismo bosque de Active Directory que el clúster
- El recurso compartido de archivos tiene un mínimo de 5 MB de espacio libre

Si el clúster se está ejecutando Windows Server 2016 o versiones anteriores, estos son los requisitos:

- Recurso compartido de archivos SMB *en un servidor de Windows unido al mismo bosque de Active Directory que el clúster*
- El objeto de nombre de clúster (CNO) debe tener permisos de escritura en el recurso compartido
- El recurso compartido de archivos tiene un mínimo de 5 MB de espacio libre

Otras notas:
- Para usar un testigo de recurso compartido de archivo alojado en los dispositivos que no sea un servidor de Windows unido al dominio, actualmente debe usar el **Set-ClusterQuorum-Credential** cmdlet de PowerShell para establecer el testigo, tal como se describe más adelante en este tema.
- Para lograr alta disponibilidad, puede usar a un testigo del recurso compartido de archivos en un clúster de conmutación por error independiente
- El recurso compartido de archivos puede usarse por varios clústeres
- No se admite el uso de un recurso compartido de sistema de archivos distribuido (DFS) o almacenamiento replicado con cualquier versión de agrupación en clústeres de conmutación por error.  Pueden provocar una situación de cerebro dividido, donde los servidores en clúster se ejecutan independientemente unas de otras y podrían provocar la pérdida de datos.

## <a name="creating-a-file-share-witness-on-a-router-with-a-usb-device"></a>Creación de un testigo de recurso compartido de archivos en un enrutador con un dispositivo USB

En [2018 de Microsoft Ignite](https://azure.microsoft.com/ignite/), [almacenamiento de datos](http://www.dataonstorage.com/) tenía un clúster de espacios de almacenamiento directo en su área de pantalla completa.  Este clúster se ha conectado a un [NetGear](https://www.netgear.com) Nighthawk X4S Wi-Fi enrutador mediante el puerto USB como un archivo de recurso compartido testigo similar al siguiente.

![NetGear testigo](media/File-Share-Witness/FSW1.png)

Los pasos para crear un testigo del recurso compartido de archivos mediante un dispositivo USB en este enrutador concreto se enumeran a continuación.  Tenga en cuenta que los pasos en otros enrutadores y los dispositivos NAS variarán y deben realizarse mediante el proveedor había proporcionado instrucciones.


1. Inicie sesión en el enrutador con el dispositivo USB conectado.

   ![Interfaz NetGear](media/File-Share-Witness/FSW2.png)

2. En la lista de opciones, seleccione ReadySHARE que es donde se pueden crear recursos compartidos.

   ![NetGear ReadySHARE](media/File-Share-Witness/FSW3.png)

3. Para un testigo de recurso compartido de archivos, un recurso compartido de básico es todo lo que es necesario.  Seleccione el botón de edición se abrirá un cuadro de diálogo donde se puede crear el recurso compartido en el dispositivo USB.

   ![Interfaz NetGear compartir](media/File-Share-Witness/FSW4.png)

4. Una vez que seleccione el botón Aplicar, el recurso compartido se crea y se puede ver en la lista.

   ![Recursos compartidos de NetGear](media/File-Share-Witness/FSW5.png)

5. Una vez creado el recurso compartido, el testigo de recurso compartido de archivos para clúster se crean con PowerShell.

   ```PowerShell
   Set-ClusterQuorum -FileShareWitness \\readyshare\Witness -Credential (Get-Credential)
   ```

   Esto muestra un cuadro de diálogo para especificar la cuenta local en el dispositivo.

Estos mismos pasos similares pueden realizarse en otros enrutadores con capacidades USB, dispositivos NAS u otros dispositivos de Windows.
