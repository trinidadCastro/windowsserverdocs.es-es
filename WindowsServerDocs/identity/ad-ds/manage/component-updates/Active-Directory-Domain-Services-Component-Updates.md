---
title: Las actualizaciones de componentes de servicios de dominio de Active Directory
description: Este documento describe las actualizaciones de componentes de AD DS para Windows Server 2012 R2
author: billmath
ms.author: billmath
manager: femila
ms.date: 09/08/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: a3a91034-a4da-4ad7-93f8-0cd2ec3e7824
ms.technology: identity-adds
ms.openlocfilehash: 52d3dab542b4250670067e927f42ddf1597fc852
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="active-directory-domain-services-component-updates"></a>Las actualizaciones de componentes de servicios de dominio de Active Directory

>Se aplica a: Windows Server 2016, Windows Server 2012 R2

Este módulo presenta los componentes que reciben actualizaciones secundarias en los servicios de directorio y los espacios de identidad.  
  
|Acerca del autor|  
|--------------------|  
|**Autor:**|Tema Justin sintonizador|  
|**Biografía:**|Tema Justin es un ingeniero de escalación de soporte técnico con el equipo de servicios de directorio basado en Irving, Texas, EE.  Ha creado o contribuyeron a muchos cursos de formación y artículos de Knowledge Base para Microsoft Knowledgebase durante los últimos 12 años. Enseña los empleados de Microsoft y la nueva arquitectura de producto de los clientes, es un contrato de maestro de certificados de Microsoft (MCM), Microsoft Certified Trainer (MCT) y contiene una maestría Grado en sistemas cognitivos y Education de equipo.|  
|**Colaboradores**|Este módulo de formación incluye las contribuciones de *Michiko Short*, *Dean Wells*, *Alan Jowett*, *Manu Pushpendran*, *Yashar Bahman*, *Anoosh Saboori*, *Rashmi Jha*, *Justin Hall* y *Herbert Mauerer*|  
|**Revisores**|Muchas gracias a las siguientes personas que emplea en su propio tiempo revisar y proporcionar comentarios: *Joey Seifert*, *Justin Hall*|  
  
> [!NOTE]  
> Este contenido está escrito por un ingeniero de soporte técnico al cliente de Microsoft y está destinada a arquitectos de sistemas que buscan explicaciones técnicas más profundos de características y soluciones de Windows Server 2012 R2 que normalmente que proporcionan los temas en TechNet y administradores experimentados. Sin embargo, no ha sufrido las mismas fases de edición, forma parte del lenguaje que parezca que menos refinada que lo que normalmente se encuentra en TechNet.  
  
### <a name="what-you-will-learn"></a>Lo que aprenderá  
Después de finalizar este módulo, podrás:  
  
-   Explicar las actualizaciones de componente realizadas dentro de las áreas de tecnología de servicios de directorio y de identidad en Windows Server 2012 R2  
  
    -   [Exclusividad SPN y UPN](../../../ad-ds/manage/component-updates/SPN-and-UPN-uniqueness.md)  
  
    -   [Sesión reinicio automático de Winlogon & #40; ARSO & #41;](../../../ad-ds/manage/component-updates/Winlogon-Automatic-Restart-Sign-On--ARSO-.md)  
  
    -   [Atestación de clave de TPM](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md)  
  
    -   [Cmdlets de copia de seguridad de CA y restaurar Windows PowerShell](../../../ad-ds/manage/component-updates/CA-Backup-and-Restore-Windows-PowerShell-cmdlets.md)  
  
    -   [Auditoría de proceso de línea de comandos](../../../ad-ds/manage/component-updates/Command-line-process-auditing.md)  
  
    -   [Administración y protección de credenciales](https://technet.microsoft.com/library/dn408190.aspx)  
  
    -   [Actualizaciones de componentes de servicios de directorio](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md)  
  
        -   [Niveles funcionales de dominios y bosques](../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_FL)  
  
        -   [Desuso de NTFRS](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_NTFRS)  
  
        -   [Cambios del optimizador de consultas LDAP](../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_LDAPQuery)  
  
        -   [Mejoras de evento 1644](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_1644)  
  
        -   [Mejora de rendimiento de replicación de directorio activo](../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_ADRepl)  
  


