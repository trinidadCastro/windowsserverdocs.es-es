---
title: Implementar un testigo de recurso compartido de archivos en Windows Server 2019
description: Los testigos de recurso compartido de archivos permiten usar un recurso compartido de archivos para votar en el cuórum del clúster. En este tema se describen testigos de recurso compartido de archivos y la nueva funcionalidad, incluido el uso de una unidad USB conectada a un enrutador como testigo de recurso compartido de archivos.
ms.prod: windows-server
manager: eldenc
ms.technology: failover-clustering
ms.topic: article
author: johnmarlin-msft
ms.author: johnmar
ms.date: 01/24/2019
ms.localizationpriority: medium
ms.openlocfilehash: 63e016b8e00482529e69aaa12727f854afd51e41
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80827678"
---
# <a name="deploy-a-file-share-witness"></a>Implementación de un testigo de recurso compartido de archivos

> Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Un testigo de recurso compartido de archivos es un recurso compartido de SMB que el clúster de conmutación por error usa como voto en el cuórum del clúster. En este tema se proporciona información general sobre la tecnología y la nueva funcionalidad de Windows Server 2019, incluido el uso de una unidad USB conectada a un enrutador como testigo de recurso compartido de archivos.

Los testigos de recurso compartido de archivos son útiles en las siguientes circunstancias:  

- No se puede usar un testigo en la nube porque no todos los servidores del clúster tienen una conexión a Internet confiable
- No se puede usar un testigo de disco porque no hay ninguna unidad compartida para usar en un testigo de disco. Podría ser un clúster de Espacios de almacenamiento directo, SQL Server Always On de grupos de disponibilidad (AG), un grupo de disponibilidad de base de datos de Exchange (DAG), etc.  Ninguno de estos tipos de clústeres usa discos compartidos.

## <a name="file-share-witness-requirements"></a>Requisitos del testigo del recurso compartido de archivos

Puede hospedar un testigo de recurso compartido de archivos en un servidor Windows unido a un dominio o, si el clúster ejecuta Windows Server 2019, cualquier dispositivo que pueda hospedar un recurso compartido de archivos SMB 2 o posterior.

|Tipo de servidor de archivos                 | Clústeres admitidos |
|---------------------------------|--------------------|
|Cualquier dispositivo con un recurso compartido de archivos SMB 2 | Windows Server 2019|
|Windows Server unido a un dominio     | Windows Server 2008 y versiones posteriores|

Si el clúster ejecuta Windows Server 2019, estos son los requisitos:

- Un recurso compartido de archivos SMB *en cualquier dispositivo que use el protocolo SMB 2 o posterior*, incluido:
    - Dispositivos de almacenamiento conectado a la red (NAS)
    - Equipos Windows Unidos a un grupo de trabajo
    - Enrutadores con almacenamiento USB conectado localmente
- Una cuenta local en el dispositivo para autenticar el clúster
- Si en su lugar usa Active Directory para autenticar el clúster con el recurso compartido de archivos, el objeto de nombre de clúster (CNO) debe tener permisos de escritura en el recurso compartido y el servidor debe estar en el mismo bosque Active Directory que el clúster.
- El recurso compartido de archivos tiene un mínimo de 5 MB de espacio disponible

Si el clúster ejecuta Windows Server 2016 o una versión anterior, estos son los requisitos:

- Recurso compartido de archivos SMB *en un servidor de Windows unido al mismo bosque de Active Directory que el clúster*
- El objeto de nombre de clúster (CNO) debe tener permisos de escritura en el recurso compartido
- El recurso compartido de archivos tiene un mínimo de 5 MB de espacio disponible

Otras notas:
- Para usar un testigo de recurso compartido de archivos hospedado en dispositivos que no sean un servidor Windows unido a un dominio, actualmente debe usar el cmdlet **set-ClusterQuorum-Credential de** PowerShell para establecer el testigo, como se describe más adelante en este tema.
- Para lograr una alta disponibilidad, puede usar un testigo de recurso compartido de archivos en un clúster de conmutación por error independiente.
- Varios clústeres pueden usar el recurso compartido de archivos
- El uso de un recurso compartido de Sistema de archivos distribuido (DFS) o almacenamiento replicado no es compatible con ninguna versión de clústeres de conmutación por error.  Pueden provocar una situación de cerebro dividida en la que los servidores agrupados se ejecutan de forma independiente y podrían provocar la pérdida de datos.

## <a name="creating-a-file-share-witness-on-a-router-with-a-usb-device"></a>Creación de un testigo de recurso compartido de archivos en un enrutador con un dispositivo USB

En el [2018 de Microsoft](https://azure.microsoft.com/ignite/), el almacenamiento de [datos](http://www.dataonstorage.com/) tenía un clúster espacios de almacenamiento directo en su área de quiosco.  Este clúster se conectó a un enrutador Wi-Fi de [Netgear](https://www.netgear.com) Nighthawk X4S mediante el puerto USB como testigo de recurso compartido de archivos similar a este.

![Testigo de NetGear](media/File-Share-Witness/FSW1.png)

A continuación se indican los pasos para crear un testigo de recurso compartido de archivos mediante un dispositivo USB en este enrutador determinado.  Tenga en cuenta que los pasos de otros enrutadores y dispositivos NAS variarán y deben realizarse con las direcciones proporcionadas por el proveedor.


1. Inicie sesión en el enrutador con el dispositivo USB conectado.

   ![Interfaz de NetGear](media/File-Share-Witness/FSW2.png)

2. En la lista de opciones, seleccione ReadySHARE, que es donde se pueden crear los recursos compartidos.

   ![ReadySHARE de NetGear](media/File-Share-Witness/FSW3.png)

3. En el caso de un testigo de recurso compartido de archivos, todo lo que se necesita es un recurso compartido básico.  Al seleccionar el botón Editar aparecerá un cuadro de diálogo en el que se puede crear el recurso compartido en el dispositivo USB.

   ![Interfaz de recurso compartido de NetGear](media/File-Share-Witness/FSW4.png)

4. Una vez que seleccione el botón aplicar, se creará el recurso compartido y podrá verse en la lista.

   ![Recursos compartidos de NetGear](media/File-Share-Witness/FSW5.png)

5. Una vez creado el recurso compartido, la creación del testigo del recurso compartido de archivos para el clúster se realiza con PowerShell.

   ```PowerShell
   Set-ClusterQuorum -FileShareWitness \\readyshare\Witness -Credential (Get-Credential)
   ```

   Se muestra un cuadro de diálogo para especificar la cuenta local en el dispositivo.

Estos mismos pasos similares se pueden realizar en otros enrutadores con capacidades USB, dispositivos NAS u otros dispositivos Windows.
