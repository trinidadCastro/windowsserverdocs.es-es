---
title: Actualizaciones de componentes de Active Directory Domain Services
description: Este documento describen las actualizaciones de componentes de AD DS para Windows Server 2012 R2
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 09/08/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: a3a91034-a4da-4ad7-93f8-0cd2ec3e7824
ms.technology: identity-adds
ms.openlocfilehash: 300bf7dafe0ef0ede754302f2c0aa8c36a7adbfd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889886"
---
# <a name="active-directory-domain-services-component-updates"></a>Actualizaciones de componentes de Active Directory Domain Services

>Se aplica a: Windows Server 2016, Windows Server 2012 R2

Este módulo presenta los componentes que recibieron actualizaciones secundarias en los servicios de directorio y espacios de identidad.  
  
|Acerca del autor|  
|--------------------|  
|**Autor:**|Justin Turner|  
|**Biografía:**|Justin es ingeniero de asignación de nivel de soporte senior con el equipo de servicios de directorio con sede en Irving, Texas, Estados Unidos.  Ha creado o contribuido en muchos cursos de formación y artículos de la Knowledge Base para la Microsoft Knowledgebase durante los últimos 12 años. Enseña a los empleados de Microsoft y la nueva arquitectura de productos a los clientes, es un contrato de Microsoft Certified Master (MCM), Microsoft Certified Trainer (MCT) y contiene una maestría grado en informática y sistemas cognitivos.|  
|**Colaboradores**|Este módulo de formación incluye las contribuciones de *Michiko Short*, *Dean Wells*, *Alan Jowett*, *Manu Pushpendran*, *Yashar Bahman*, *Anoosh Saboori*, *Rashmi Jha*, *Justin Hall* y *Herbert Mauerer*|  
|**Revisores**|Muchas gracias a las siguientes personas que dedicaron su tiempo a revisar y proporcionar comentarios: *Joey Seifert*, *Justin Hall*|  
  
> [!NOTE]  
> Este contenido está escrito por un ingeniero de asistencia al cliente de Microsoft y está destinado a los arquitectos de sistemas y administradores con experiencia que están buscando explicaciones técnicas más detalladas de características y soluciones de Windows Server 2012 R2 que los temas que se suelen proporcionar en TechNet. Sin embargo, no ha experimentado los mismos pasos de edición, por lo que parte del lenguaje puede parecer menos perfeccionado de lo que se encuentra normalmente en TechNet.  
  
### <a name="what-you-will-learn"></a>Qué aprenderá  
Tras finalizar este módulo, podrá:  
  
-   Explicar las actualizaciones del componente realizadas dentro de las áreas de tecnología de servicios de directorio e identidad en Windows Server 2012 R2  
  
    -   [Unicidad de SPN y UPN](../../../ad-ds/manage/component-updates/SPN-and-UPN-uniqueness.md)  
  
    -   [Inicio de sesión con reinicio automático de Winlogon &#40;ARSO&#41;](../../../ad-ds/manage/component-updates/Winlogon-Automatic-Restart-Sign-On--ARSO-.md)  
  
    -   [Atestación de clave de TPM](../../../ad-ds/manage/component-updates/TPM-Key-Attestation.md)  
  
    -   [Copia de seguridad de entidad de certificación y restauración de Windows PowerShell](../../../ad-ds/manage/component-updates/CA-Backup-and-Restore-Windows-PowerShell-cmdlets.md)  
  
    -   [Auditoría de proceso de línea de comandos](../../../ad-ds/manage/component-updates/Command-line-process-auditing.md)  
  
    -   [Protección y administración de credenciales](https://technet.microsoft.com/library/dn408190.aspx)  
  
    -   [Actualizaciones de componentes de Servicios de directorio](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md)  
  
        -   [Niveles funcionales de dominios y bosques](../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_FL)  
  
        -   [Degradación de NTFRS](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_NTFRS)  
  
        -   [Cambios en el optimizador de consultas LDAP](../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_LDAPQuery)  
  
        -   [Mejoras en el evento 1644](../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_1644)  
  
        -   [Mejora del rendimiento de replicación de Active Directory](../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/Directory-Services-component-updates.md#BKMK_ADRepl)  
  


