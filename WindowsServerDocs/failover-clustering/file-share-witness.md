---
title: Implementación de un testigo de recurso compartido de archivo en Windows Server 2019
ms.prod: windows-server-threshold
ms.manager: eldenc
ms.technology: failover-clustering
ms.topic: article
author: johnmarlin-msft
ms.date: 01/24/2019
description: Testigos de recurso compartido de archivo te permiten usar un recurso compartido de archivos para votar en cuórum de clúster. En este tema se describe testigos de recurso compartido de archivo y la nueva funcionalidad, incluido el uso de una unidad USB conectada a un enrutador como un testigo del recurso compartido de archivos.
ms.localizationpriority: medium
ms.openlocfilehash: 1888142f96208800a0417c9caeea89e8a0472e88
ms.sourcegitcommit: d622f7af181ed0063d716b30278d41887a57db19
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/08/2019
ms.locfileid: "9150895"
---
# Implementar un testigo de uso compartido de archivos

> Se aplica a: Windows Server 2019 Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Un testigo del recurso compartido de archivos es un recurso compartido SMB que usa el clúster de conmutación por error como un voto en el cuórum de clúster. Este tema proporciona una visión general de la tecnología y la nueva funcionalidad en Windows Server 2019, incluido el uso de una unidad USB conectada a un enrutador como un testigo del recurso compartido de archivos.

Testigos de recurso compartido de archivo son útiles en las siguientes circunstancias:  

- No se puede utilizar un testigo en la nube porque no todos los servidores del clúster tienen una conexión confiable de Internet
- No se puede utilizar un testigo de disco porque no hay las unidades que se usará para un testigo de disco compartidas. Esto podría ser un clúster de espacios de almacenamiento directo, SQL Server siempre en disponibilidad grupos (AG), Exchange base de datos de disponibilidad de grupo (DAG incluidos), etcetera.  Ninguno de estos tipos de clústeres usa discos compartidos.

## Requisitos de testigo de recurso compartido de archivos

Puedes alojar un testigo del recurso compartido de archivos en un servidor de Windows unido al dominio, o si el clúster se está ejecutando Windows Server 2019, cualquier dispositivo que puede host un 2 de SMB o archivo posterior compartir.

|Tipo de servidor de archivos                 | Clústeres admitidos |
|---------------------------------|--------------------|
|Cualquier dispositivo w/una SMB 2 recurso compartido de archivos | Windows Server 2019|
|Unido al dominio de Windows Server     | Windows Server 2008 y versiones posterior|

Si el clúster se está ejecutando Windows Server 2019, estos son los requisitos:

- Compartir un archivo SMB *en cualquier dispositivo que use el protocolo posterior o el 2 de SMB*, incluidos:
    - Dispositivos de almacenamiento conectado a la red (NAS)
    - Los equipos de Windows unido a un grupo de trabajo
    - Enrutadores con almacenamiento USB conectados localmente
- Una cuenta local en el dispositivo para autenticar el clúster
- Si estás usando en su lugar Active Directory para autenticar el clúster con el recurso compartido de archivos, el objeto de nombre de clúster (CNO) debe tener permisos de escritura en el recurso compartido y el servidor debe estar en el mismo bosque de Active Directory que el clúster
- El recurso compartido de archivo tiene un mínimo de 5 MB de espacio libre

Si el clúster se está ejecutando Windows Server 2016 o versiones anteriores, estos son los requisitos:

- Uso compartido de archivos SMB *en un servidor de Windows unido al mismo bosque de Active Directory que el clúster*
- El objeto de nombre de clúster (CNO) debe tener permisos de escritura en el recurso compartido
- El recurso compartido de archivo tiene un mínimo de 5 MB de espacio libre

Otras notas:
- Para usar un testigo de recurso compartido hospedado por los dispositivos que no sean de un servidor de Windows unido al dominio, actualmente debe utilizar la **Set-ClusterQuorum-Credential** cmdlet de PowerShell para establecer el testigo, tal como se describe más adelante en este tema.
- Para alta disponibilidad, puedes usar un testigo del recurso compartido de archivos en un clúster de conmutación por error independiente
- El recurso compartido de archivos puede usarse en varios clústeres
- No se admite el uso de un recurso compartido de sistema de archivos distribuido (DFS) o el almacenamiento replicado con cualquier versión de clústeres de conmutación por error.  Pueden causar una situación de cerebro dividido donde los servidores en clúster se ejecutan de forma independiente entre sí y podrían provocar la pérdida de datos.

## Creación de un testigo del recurso compartido de archivos en un enrutador con un dispositivo USB

En [Microsoft Ignite 2018](https://azure.microsoft.com/ignite/), [El almacenamiento de DataOn](http://www.dataonstorage.com/) tendría un clúster directo de espacios de almacenamiento en su área de pantalla completa.  Este clúster estaba conectado a un enrutador de Wi-Fi de [NetGear](https://www.netgear.com) Nighthawk X4S con el puerto USB como testigo de recurso compartido de archivo similar al siguiente.

![Testigo NetGear](media\File-Share-Witness\FSW1.png)

Los pasos para crear un testigo del recurso compartido de archivo con un dispositivo USB en este enrutador particular se enumeran a continuación.  Ten en cuenta que pasos en otros enrutadores y dispositivos NAS varían y deben realizarse mediante el proveedor suministra indicaciones.


1. Inicia sesión en el router con el dispositivo USB conectado.

   ![Interfaz de NetGear](media\File-Share-Witness\FSW2.png)

2. En la lista de opciones, selecciona ReadySHARE que es donde se pueden crear recursos compartidos.

   ![NetGear ReadySHARE](media\File-Share-Witness\FSW3.png)

3. Para un testigo de recurso compartido, un recurso compartido básico es todo lo que se necesita.  Selecciona el botón de edición aparecerá un cuadro de diálogo donde se puede crear el recurso compartido en el dispositivo USB.

   ![Interfaz NetGear compartir](media\File-Share-Witness\FSW4.png)

4. Una vez que seleccionar el botón Aplicar, el recurso compartido se crea y se puede ver en la lista.

   ![Recursos compartidos de NetGear](media\File-Share-Witness\FSW5.png)

5. Una vez creado el recurso compartido, crear al testigo del recurso compartido de clúster se realiza con PowerShell.

   ```PowerShell
   Set-ClusterQuorum -FileShareWitness \\readyshare\Witness -Credential (Get-Credential)
   ```

   Esto muestra un cuadro de diálogo para especificar la cuenta local en el dispositivo.

Estos mismos pasos similares pueden hacerse en otros enrutadores con funcionalidades USB, dispositivos NAS u otros dispositivos de Windows.
