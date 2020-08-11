---
title: Archivo por lotes de ejemplo de shell de red (Netsh)
description: Puedes usar este tema para obtener información sobre cómo crear un archivo por lotes que realice varias tareas mediante Netsh en Windows Server 2016.
ms.topic: article
ms.assetid: c94e37a4-3637-4613-9eb5-ed604e831eca
manager: brianlic
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 35690e25fa18512d729bbd323b8983284a41781d
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87953981"
---
# <a name="network-shell-netsh-example-batch-file"></a>Archivo por lotes de ejemplo de shell de red (Netsh)

Se aplica a: Windows Server 2016

Puedes usar este tema para obtener información sobre cómo crear un archivo por lotes que realice varias tareas mediante Netsh en Windows Server 2016. En este archivo por lotes de ejemplo, se utiliza el contexto **netsh wins**.

## <a name="example-batch-file-overview"></a>Información general sobre el archivo por lotes de ejemplo

Puedes usar los comandos de Netsh para el Servicio de nombres Internet de Windows \(WINS\) en archivos por lotes y otros scripts para automatizar las tareas. En el siguiente archivo por lotes de ejemplo se muestra cómo usar los comandos Netsh para WINS a fin de realizar una serie de tareas relacionadas.

En este archivo por lotes de ejemplo, WINS\-A es un servidor WINS con la dirección IP 192.168.125.30 y WINS\-B es un servidor WINS con la dirección IP 192.168.0.189.

El archivo por lotes de ejemplo realiza las siguientes tareas.

- Agrega un registro de nombres dinámico con la dirección IP 192.168.0.205, MY\_RECORD \[04h\], a WINS\-A.
- Establece WINS\-B como un asociado de replicación de inserción/extracción de WINS\-A.
- Se conecta a WINS\-B y, a continuación, establece WINS\-A como un asociado de replicación de inserción/extracción de WINS\-B.
- Inicia una replicación de extracción de WINS\-A a WINS\-B.
- Se conecta a WINS\-B para comprobar que el nuevo registro, MY\_RECORD, se ha replicado correctamente.

## <a name="netsh-example-batch-file"></a>Archivo por lotes de ejemplo de Netsh

En el siguiente archivo por lotes de ejemplo, las líneas que contienen comentarios van precedidas de "rem" para indicar que se trata de un comentario. Netsh omite los comentarios.

```
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

rem 5. Connect to (WINS-B), and check that the record MY_RECORD [04h] was replicated successfully.
netsh wins server 192.168.0.189 show name Name=MY_RECORD EndChar=04

rem 6. End example batch file.
```

## <a name="netsh-wins-commands-used-in-the-example-batch-file"></a>Comandos Netsh WINS utilizados en el archivo por lotes de ejemplo

En la siguiente sección se enumeran los comandos **netsh wins** que se usan en este procedimiento de ejemplo.

- **server**. Cambia el contexto de línea de comandos de WINS actual al servidor especificado por su nombre o dirección IP.
- **add name**. Registra un nombre en el servidor WINS.
- **add partner**. Agrega un asociado de replicación en el servidor WINS.
- **init push**. Inicia y envía un desencadenador de inserción a un servidor WINS.
- **show name**. Muestra información detallada de un registro determinado en la base de datos del servidor WINS.

## <a name="additional-references"></a>Referencias adicionales

- [Shell de red (Netsh)](netsh.md)
