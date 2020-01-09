---
title: Error de protocolo de enlace de tres vías TCP durante la conexión SMB
description: Presenta el error del Protocolo de enlace de tres vías TCP durante la conexión SMB.
author: Deland-Han
manager: dcscontentpm
audience: ITPro
ms.topic: article
ms.author: delhan
ms.date: 12/25/2019
ms.openlocfilehash: 8cef47e164b8768747cb383f4d7012130c7cb516
ms.sourcegitcommit: 8cf04db0bc44fd98f4321dca334e38c6573fae6c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/03/2020
ms.locfileid: "75654636"
---
# <a name="tcp-three-way-handshake-failure-during-smb-connection"></a>Error de protocolo de enlace de tres vías TCP durante la conexión SMB

Cuando se analiza un seguimiento de red, se observa que hay un error de protocolo de enlace de tres vías del Protocolo de control de transmisión (TCP) que hace que se produzca el problema de SMB. En este artículo se describe cómo solucionar este problema.

## <a name="troubleshooting"></a>de solución de problemas

Por lo general, la causa es un firewall local o de infraestructura que bloquea el tráfico. Este problema puede producirse en cualquiera de los escenarios siguientes.

## <a name="scenario-1"></a>Escenario 1

El paquete TCP SYN llega al servidor SMB, pero el servidor SMB no devuelve un paquete TCP SYN-ACK.

Para solucionar este escenario, siga estos pasos.

### <a name="step-1"></a>Paso 1

Ejecute **netstat** o **Get-NetTcpConnection** para asegurarse de que hay un agente de escucha en el puerto TCP 445 que debe ser propiedad del proceso del sistema.

```cmd
netstat -ano | findstr :445
```

```PowerShell
Get-NetTcpConnection -LocalPort 445
```

### <a name="step-2"></a>Paso 2

Asegúrese de que el servicio servidor está iniciado y en ejecución.

### <a name="step-3"></a>Paso 3

Realice una captura de la plataforma de filtrado de Windows (WFP) para determinar qué regla o programa está quitando el tráfico. Para ello, ejecute el siguiente comando en una ventana del símbolo del sistema:

```cmd
netsh wfp capture start
```

Reproduzca el problema y, a continuación, ejecute el siguiente comando:

```cmd
netsh wfp capture stop
```

Ejecute un seguimiento de escenario y busque caídas de WFP en el tráfico SMB (en el puerto TCP 445).

Opcionalmente, puede quitar los programas antivirus porque no están siempre basados en WFP.

### <a name="step-4"></a>Paso 4

Si el Firewall de Windows está habilitado, habilite el registro del firewall para determinar si registra una caída del tráfico.

Asegúrese de que las reglas "compartir archivos e impresoras (SMB de entrada)" adecuadas estén habilitadas en **firewall de Windows con seguridad avanzada** \> **reglas de entrada**.

> [!NOTE]
> En función de cómo esté configurado el equipo, "Firewall de Windows" puede llamarse "Firewall de Windows Defender".

## <a name="scenario-2"></a>Escenario 2

El paquete SYN de TCP nunca llega al servidor SMB.

En este escenario, debe investigar los dispositivos a lo largo de la ruta de acceso de red. Puede analizar los seguimientos de red que se capturan en cada dispositivo para determinar qué dispositivo está bloqueando el tráfico.
