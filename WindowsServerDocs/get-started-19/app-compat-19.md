---
title: Compatibilidad de aplicaciones de servidor de Microsoft y Windows Server 2019
description: Tabla de compatibilidad de aplicaciones de servidor de Microsoft y Windows Server 2019
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2afe7c32-1fda-4441-989b-4115dabdcd34
author: coreyp
ms.author: coreyp-at-msft
manager: jasgroce
ms.localizationpriority: medium
ms.openlocfilehash: 8dcaff6ab8a296790158f59035bd4a5c1a093cbd
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/17/2019
ms.locfileid: "66442372"
---
# <a name="windows-server-2019-and-microsoft-server-application-compatibility"></a>Compatibilidad de aplicaciones de servidor de Microsoft y Windows Server 2019

>Se aplica a: Windows Server 2019

En esta tabla se enumeran las aplicaciones de servidor de Microsoft que admiten la instalación y la funcionalidad en Windows Server 2019. Esta información es para referencia rápida y no está diseñada para reemplazar las especificaciones de productos individuales, los requisitos, los anuncios o las comunicaciones generales de cada aplicación de servidor individual. Consulta la documentación oficial de cada producto para comprender perfectamente las opciones y la compatibilidad.

Si eres partner proveedor de software y buscas más información sobre la compatibilidad de Windows Server con aplicaciones que no son de Microsoft, visita el [portal de certificación de aplicaciones comerciales](https://commercialappcertification.microsoft.com/).

| **Producto**                                                  | **Admitido en Server Core**             |   | **Admitido en servidor con Experiencia de escritorio** | **¿Publicado?** |   | **Vínculo web del producto**                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
|--------------------------------------------------------------|------------------------------------------|---|-------------------------------------------------|---------------|---|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Exchange Server 2019                                         | Sí                                      |   | Sí                                             | Sí           |   | [Requisitos del sistema de Exchange Server](https://docs.microsoft.com/Exchange/plan-and-deploy/system-requirements?view=exchserver-2019)                                                                        |
| Host Integration Server 2016, CU3                            | Sí                                      |   | Sí                                             | Sí            |   | [Requisitos del sistema de Host Integration Server](https://docs.microsoft.com/host-integration-server/install-and-config-guides/system-requirements)                                                            |
| Visual Studio Team Foundation Server 2017                    | Sí\*                                    |   | Sí                                             | Sí           |   | [Team Foundation Server 2017](https://docs.microsoft.com/tfs/server/requirements?view=vsts)                                                                                                                |
| Visual Studio Team Foundation Server 2018                    | Sí\*                                    |   | Sí                                             | Sí           |   | [Team Foundation Server 2018](https://docs.microsoft.com/tfs/server/requirements?view=vsts)                                                                                                                  |
| Microsoft SQL Server 2014                                    | Sí\*                                    |   | Sí                                             | Sí           |   | [Requisitos de hardware y software para instalar SQL Server 2014](https://docs.microsoft.com/sql/sql-server/install/hardware-and-software-requirements-for-installing-sql-server?view=sql-server-2014)   |
| Microsoft SQL Server 2016                                    | Sí\*                                    |   | Sí                                             | Sí           |   | [Requisitos de hardware y software para instalar SQL Server 2016](https://docs.microsoft.com/sql/sql-server/install/hardware-and-software-requirements-for-installing-sql-server?view=sql-server-2016)   |
| Microsoft SQL Server 2017                                    | Sí\*                                    |   | Sí                                             | Sí           |   | [Requisitos de hardware y software para instalar SQL Server 2017](https://docs.microsoft.com/sql/sql-server/install/hardware-and-software-requirements-for-installing-sql-server?view=sql-server-2017) |
| Microsoft System Center Configuration Manager (versión 1806) | Sí como cliente administrado, no como servidor de sitio |   | Sí como cliente administrado, no como servidor de sitio        | Sí           |   | [Novedades de la versión 1806 de System Center Configuration Manager](https://docs.microsoft.com/sccm/core/plan-design/changes/whats-new-in-version-1806)                                                    |
| Microsoft System Center Operations Manager 2019              | Sí\*                                    |   | Sí                                             | Sí           |   | [Requisitos del sistema para System Center Operations Manager](https://docs.microsoft.com/system-center/scom/plan-system-requirements)                                                                                                      |
| Microsoft System Center Virtual Machine Manager 2019         | Sí\*                                    |   | Sí                                             | Sí           |   | [Requisitos del sistema para System Center Virtual Machine Manager](https://docs.microsoft.com/system-center/vmm/system-requirements)                                                                                                      |
| Microsoft System Center Data Protection Manager 2019         | No                                       |   | Sí                                             | Sí           |   | [Preparación del entorno para System Center Data Protection Manager](https://docs.microsoft.com/system-center/dpm/prepare-environment-for-dpm?view=sc-dpm-2019)                                                                                                      |
| SharePoint Server 2016                                       | No                                       |   | Sí                                             | Sí           |   | [Requisitos de hardware y software de SharePoint Server 2016](https://docs.microsoft.com/SharePoint/install/hardware-and-software-requirements)                                                                |
| SharePoint Server 2019                                       | No                                       |   | Sí                                             | Sí           |   | [Requisitos de hardware y software de SharePoint Server 2019](https://docs.microsoft.com/sharepoint/install/hardware-and-software-requirements-2019)                                                       |
| Project Server 2016                                          | No                                       |   | Sí                                             | Sí           |   | [Requisitos de software de Project Server 2016](https://docs.microsoft.com/project/software-requirements-for-project-server-2016)                                                                                |
| Project Server 2019                                          | No                                       |   | Sí                                             | Sí           |   | [Requisitos de software de Project Server 2019](https://docs.microsoft.com/project/software-requirements-for-project-server-2019)                                                                          |
| Skype Empresarial 2019                                      | No                                       |   | Sí                                             | Sí           |   | [Requisitos previos para la instalación de del servidor para Skype Empresarial Server](https://docs.microsoft.com/skypeforbusiness/deploy/install/install-prerequisites)                                                                          |

\*Puede tener limitaciones o exigir la [Característica a petición (FOD) App Compatibility de Server Core](install-fod-19.md).
Consulta la documentación del producto o FOD específico.
