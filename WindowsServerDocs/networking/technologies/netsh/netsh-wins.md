---
title: Archivo por lotes de shell de red (Netsh) de ejemplo
description: Puede usar este tema para obtener información sobre cómo crear un archivo por lotes que realice varias tareas mediante Netsh en Windows Server 2016.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: c94e37a4-3637-4613-9eb5-ed604e831eca
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 86fbe66978f7c09a332bba16a27a13fa029cb5a6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71401916"
---
# <a name="network-shell-netsh-example-batch-file"></a>Shell de red \(Netsh @ no__t-1 archivo por lotes de ejemplo

Se aplica a: Windows Server 2016

Puede usar este tema para obtener información sobre cómo crear un archivo por lotes que realice varias tareas mediante Netsh en Windows Server 2016. En este archivo por lotes de ejemplo, se utiliza el contexto **netsh wins** .

## <a name="example-batch-file-overview"></a>Información general del archivo por lotes de ejemplo

Puede usar los comandos Netsh para el servicio de nombres Internet de Windows \(WINS @ no__t-1 en archivos por lotes y otros scripts para automatizar tareas. En el siguiente ejemplo de archivo por lotes se muestra cómo usar los comandos Netsh para WINS a fin de realizar una serie de tareas relacionadas.

En este archivo por lotes de ejemplo, WINS @ no__t-0A es un servidor WINS con la dirección IP 192.168.125.30 y WINS @ no__t-1B es un servidor WINS con la dirección IP 192.168.0.189.

El archivo por lotes de ejemplo realiza las siguientes tareas.

- Agrega un registro de nombre dinámico con la dirección IP 192.168.0.205, mi @ no__t-0RECORD \[04h @ no__t-2, a WINS @ no__t-3A
- Establece WINS @ no__t-0B como asociado de replicación de inserción/extracción de WINS @ no__t-1A
- Se conecta a WINS @ no__t-0B y, a continuación, establece WINS @ no__t-1A como asociado de replicación de inserción/extracción de WINS @ no__t-2B
- Inicia una replicación de extracción desde WINS @ no__t-0A a WINS @ no__t-1B
- Se conecta a WINS @ no__t-0B para comprobar que el nuevo registro, MY @ no__t-1RECORD, se replicó correctamente

## <a name="netsh-example-batch-file"></a>Archivo por lotes de ejemplo de Netsh

En el siguiente archivo por lotes de ejemplo, las líneas que contienen comentarios van precedidas de "REM" para el comentario. Netsh omite los comentarios.

    rem: Begin example batch file.
    
    rem two WINS servers:
    
    rem (WINS-A) 192.168.125.30
    
    rem (WINS-B) 192.168.0.189
    
    rem 1. Connect to (WINS-A), and add the dynamic name MY\_RECORD \[04h\] to the (WINS-A) database.
    
    netsh wins server 192.168.125.30 add name Name=MY\_RECORD EndChar=04 IP={192.168.0.205}
    
    rem 2. Connect to (WINS-A), and set (WINS-B) as a push/pull replication partner of (WINS-A).
    
    netsh wins server 192.168.125.30 add partner Server=192.168.0.189 Type=2
    
    rem 3. Connect to (WINS-B), and set (WINS-A) as a push/pull replication partner of (WINS-B).
    
    netsh wins server 192.168.0.189 add partner Server=192.168.125.30 Type=2
    
    rem 4. Connect back to (WINS-A), and initiate a push replication to (WINS-B).
    
    netsh wins server 192.168.125.30 init push Server=192.168.0.189 PropReq=0
    
    rem 5. Connect to (WINS-B), and check that the record MY\_RECORD \[04h\] was replicated successfully.
    
    netsh wins server 192.168.0.189 show name Name=MY\_RECORD EndChar=04
    
    rem 6. End example batch file.

## <a name="netsh-wins-commands-used-in-the-example-batch-file"></a>Comandos Netsh WINS utilizados en el archivo por lotes de ejemplo

En la siguiente sección se enumeran los comandos **netsh wins** que se usan en este procedimiento de ejemplo.

- **servidor**. Desplaza el contexto actual del comando WINS @ no__t-0line al servidor especificado por su nombre o dirección IP.
- **Agregar nombre**. Registra un nombre en el servidor WINS.
- **Agregar asociado**. Agrega un asociado de replicación en el servidor WINS.
- **inicialización de init**. Inicia y envía un desencadenador de envío a un servidor WINS.
- **Mostrar nombre**. Muestra información detallada de un registro determinado en la base de datos del servidor WINS.  

Para obtener más información, vea [Shell de red (netsh)](netsh.md).
