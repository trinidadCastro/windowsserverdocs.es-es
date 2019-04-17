---
title: Roles, servicios de rol y características no incluidas en Windows Server - Server Core
description: Obtenga información sobre las funciones y características no incluidas en la opción de instalación Server Core de Windows Server.
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 02/23/2018
ms.openlocfilehash: 308bc8a5d25e2ec67438f0ee03cbfce6f7411ca2
ms.sourcegitcommit: 4b9b21ca1f366388a78ead7413cb581f2b23d4c6
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/09/2018
ms.locfileid: "2604793"
---
# Roles, servicios de rol y características no incluidas en Windows Server - Server Core

> Se aplica a: (delimitadas anuales del canal) de Windows Server y Windows Server 2016

Se han quitado las siguientes funciones, servicios de rol y características de la opción de instalación Server Core de Windows Server. Use esta información para ayudar a averiguar si la opción Server Core funciona para su entorno.

> [!NOTE]
> También puede ver una lista de los roles, servicios de rol y características que [se incluyen en Server Core](server-core-roles-and-services.md). Es una lista muy grande, por lo que para obtener los mejores resultados, busque esa lista para el rol específico o una característica que le interesa.

## Funciones no en Server Core

- Fax
- MultiPointServerRole
- NPAS
- WDS

## Servicios de rol no está en Server Core
Tenga en cuenta que algunos servicios de rol de escritorio remoto se incluyen en Server Core (agente de conexión, administración de licencias, Host de virtualización), pero otros no lo están (puerta de enlace, Host de sesión de escritorio remoto, Web Access).

- Servidor de análisis de impresión
- Imprimir Internet
- Puerta de enlace de RDS
- Servidor de escritorio remoto de RDS
- Acceso de Web de RDS
- Consola de administración de Web
- Consola-Web-Lgcy-Mgmt
- Implementación de WDS
- Transporte de WDS *(antes de Windows Server versión 1803)*

## Características no incluidas en Server Core

- BITS-IIS-Ext
- NetworkUnlock de BitLocker
- Direct Play
- Cliente de impresión de Internet
- Monitor de puerto LPR
- Multidifusión de MSMQ
- CMAK
- Asistencia remota de
- RSAT SMTP
- RSAT-Feature-Tools-BitLocker-RemoteAdminTool
- Servidor de RSAT-Bits
- RSAT NLB
- RSAT SNMP
- RSAT WINS
- Hyper-V-Tools
- Herramientas de RDS RSAT
- RSAT-RDS-puerta de enlace
- RSAT-RDS-licencias-diagnóstico-la interfaz de usuario
- RDS licencias-la interfaz de usuario
- Interfaz de usuario de UpdateServices
- RSAT ADC
- RSAT-ADC-Mgmt
- RSAT Respondedor en línea
- RSAT-ADRMS
- RSAT-Fax
- RSAT-archivo-servicios
- RSAT-DFS-Mgmt-Con
- RSAT-FSRM-Mgmt
- RSAT-NFS-Admin
- RSAT NPAS
- Servicios de impresión de RSAT
- Herramientas de RSAT VA
- WDS AdminPack
- Servidor SMTP
- Cliente de TFTP
- Redirector de WebDAV
- Biométrica Framework
- Gui de Windows Defender
- Windows-Identity-Foundation
- PowerShell ISE
- Servicio de búsqueda
- IFilter de TIFF de Windows
- Redes inalámbricas
- Visor de XPS

