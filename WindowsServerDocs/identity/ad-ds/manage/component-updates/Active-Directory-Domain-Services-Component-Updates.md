---
title: Actualizaciones de componentes de Active Directory Domain Services
description: En este documento se describen las actualizaciones de componentes de AD DS para Windows Server 2012 R2
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 09/08/2017
ms.topic: article
ms.assetid: a3a91034-a4da-4ad7-93f8-0cd2ec3e7824
ms.openlocfilehash: 41e696ffd7339269311a9ccf21e096b48ef37439
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87943438"
---
# <a name="active-directory-domain-services-component-updates"></a>Actualizaciones de componentes de Active Directory Domain Services

>Se aplica a: Windows Server 2016, Windows Server 2012 R2

Este módulo presenta los componentes que recibieron actualizaciones secundarias en los servicios de directorio y espacios de identidad.


| Acerca del autor |
|------------------|
|   **Autor:**    |
|     **Biografía:**     |
| **Colaboradores** |
|  **Revisores**   |

> [!NOTE]
> Este contenido está escrito por un ingeniero de asistencia al cliente de Microsoft y está destinado a los arquitectos de sistemas y administradores con experiencia que están buscando explicaciones técnicas más detalladas de características y soluciones de Windows Server 2012 R2 que los temas que se suelen proporcionar en TechNet. Sin embargo, no ha experimentado los mismos pasos de edición, por lo que parte del lenguaje puede parecer menos perfeccionado de lo que se encuentra normalmente en TechNet.

### <a name="what-you-will-learn"></a>Aprendizaje
Tras finalizar este módulo, podrá:

-   Explicar las actualizaciones del componente realizadas dentro de las áreas de tecnología de servicios de directorio e identidad en Windows Server 2012 R2

    -   [Unicidad de SPN y UPN](../../../ad-ds/manage/component-updates/SPN-and-UPN-uniqueness.md)

    -   [Inicio de sesión con reinicio automático de Winlogon &#40;ARSO&#41;](../../../ad-ds/manage/component-updates/Winlogon-Automatic-Restart-Sign-On--ARSO-.md)

    -   [Atestación de clave de TPM](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md)

    -   [Copia de seguridad de entidad de certificación y restauración de Windows PowerShell](../../../ad-ds/manage/component-updates/CA-Backup-and-Restore-Windows-PowerShell-cmdlets.md)

    -   [Auditoría de procesos de línea de comandos](../../../ad-ds/manage/component-updates/Command-line-process-auditing.md)

    -   [Protección y administración de credenciales](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn408190(v=ws.11))

    -   [Actualizaciones de componentes de Servicios de directorio](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md)

        -   [Niveles funcionales de dominios y bosques](../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_FL)

        -   [Degradación de NTFRS](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_NTFRS)

        -   [Cambios en el optimizador de consultas LDAP](../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_LDAPQuery)

        -   [Mejoras en el evento 1644](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_1644)

        -   [Mejora del rendimiento de replicación de Active Directory](../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_ADRepl)
