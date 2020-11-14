---
title: Compatibilidad de Pktmon con Monitor de red de Microsoft (Netmon)
description: Use esta página para analizar archivos ETL generados por el monitor de paquetes en Netmon.
ms.topic: how-to
author: khdownie
ms.author: v-kedow
ms.date: 11/12/2020
ms.openlocfilehash: 7c6e3a40558ea27a273464d157fa4fbdbacd7134
ms.sourcegitcommit: 8808f871c8cf131f819ef5540286218bd425da96
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/14/2020
ms.locfileid: "94632638"
---
# <a name="pktmon-support-for-microsoft-network-monitor-netmon"></a>Compatibilidad de Pktmon con Monitor de red de Microsoft (Netmon)

>Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows 10, Azure Stack HCl, Azure Stack Hub, Azure

El monitor de paquetes (Pktmon) genera registros en formato ETL. Estos registros se pueden analizar mediante el Monitor de red de Microsoft (Netmon) mediante analizadores especiales. En este tema se explica cómo analizar archivos ETL generados por el monitor de paquetes en Netmon.

## <a name="network-monitor-setup-and-configuration"></a>Instalación y configuración de Monitor de red

Siga estos pasos para instalar y configurar Netmon para analizar archivos ETL generados por el monitor de paquetes:

   1. [Instale Monitor de red 3,4](/download/4865).
   1. Inicie Monitor de red elevado y establezca Windows como perfil de analizador activo en (herramientas/opciones/perfiles de analizador).
   1. Copie etl_Microsoft-Windows-PktMon-Events. NPL desde [aquí](https://github.com/microsoft/NetMon_Parsers_for_PacketMon/blob/main/etl_Microsoft-Windows-PktMon-Events.npl)   a "%PROGRAMDATA%\Microsoft\Network monitor 3 \ NPL\NetworkMonitor Parsers\Windows"
   1. Copie stub_etl_Microsoft-Windows-PktMon-Events. NPL desde [aquí](https://github.com/microsoft/NetMon_Parsers_for_PacketMon/blob/main/stub_etl_Microsoft-Windows-PktMon-Events.npl) a "%PROGRAMDATA%\Microsoft\Network monitor 3 \ NPL\NetworkMonitor Parsers\Windows\Stubs"
   1. Cambie el nombre de stub_etl_Microsoft-Windows-PktMon-Events. NPL a etl_Microsoft-Windows-PktMon-Events. NPL
   1. Incluya etl_Microsoft-Windows-PktMon-Events. NPL en NetworkMonitor_Parsers_sparser. NPL en "%PROGRAMDATA%\Microsoft\Network monitor 3 \ NPL\NetworkMonitor parsers"
   1. Reinicie Monitor de red elevado para volver a generar los analizadores.

