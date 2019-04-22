---
ms.assetid: e2651dc8-4b31-4cd8-a400-3b8123890210
title: Procedimientos recomendados para proteger Active Directory
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 972def668634e794908a3ff2933d038ae38be5d6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817086"
---
# <a name="best-practices-for-securing-active-directory"></a>Procedimientos recomendados para proteger Active Directory

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este documento proporciona la perspectiva de un profesional y contiene un conjunto de técnicas prácticas para ayudar a los ejecutivos de TI a proteger un entorno de Active Directory empresarial. Active Directory desempeña un papel fundamental en la infraestructura de TI y garantiza la armonía y la seguridad de diferentes recursos de red en un entorno global interconectado. Los métodos descritos se basan en gran medida la experiencia de la organización de Microsoft Information Security and Risk Management (ISRM), que es responsable de proteger los activos de Microsoft IT y otras divisiones empresariales de Microsoft, además de asesorar a un número de clientes de Microsoft Global 500 seleccionado.  
  
-   [Resumen ejecutivo](../../../ad-ds/manage/component-updates/Executive-Summary.md)  
  
-   [Introducción](../../../ad-ds/manage/component-updates/Introduction.md)  
  
-   [Vías de compromiso](../../../ad-ds/plan/security-best-practices/Avenues-to-Compromise.md)  
  
-   [Cuentas atractivas para el robo de credenciales](../../../ad-ds/plan/security-best-practices/Attractive-Accounts-for-Credential-Theft.md)  
  
-   [Reducir la superficie de ataque de Active Directory](../../../ad-ds/plan/security-best-practices/Reducing-the-Active-Directory-Attack-Surface.md)  
  
-   [Implementación de modelos administrativos con privilegios mínimos](../../../ad-ds/plan/security-best-practices/Implementing-Least-Privilege-Administrative-Models.md)  
  
-   [Implementación de Hosts administrativos seguros](../../../ad-ds/plan/security-best-practices/Implementing-Secure-Administrative-Hosts.md)  
  
-   [Protección de los controladores de dominio frente a ataques](../../../ad-ds/plan/security-best-practices/Securing-Domain-Controllers-Against-Attack.md)  
  
-   [Supervisión de Active Directory en busca de indicios de riesgo](../../../ad-ds/plan/security-best-practices/Monitoring-Active-Directory-for-Signs-of-Compromise.md)  
  
-   [Recomendaciones de directiva de auditoría](../../../ad-ds/plan/security-best-practices/Audit-Policy-Recommendations.md)  
  
-   [Planeación de compromiso](../../../ad-ds/plan/security-best-practices/Planning-for-Compromise.md)  
  
-   [Mantener un entorno más seguro](../../../ad-ds/plan/security-best-practices/Maintaining-a-More-Secure-Environment.md)  
  
-   [Apéndices](../../../ad-ds/plan/security-best-practices/Appendices.md)  
   
-   [Apéndice B: Las cuentas con privilegios y grupos en Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-B--Privileged-Accounts-and-Groups-in-Active-Directory.md)  
  
-   [Apéndice C: Cuentas protegidas y grupos en Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md)  
  
-   [Apéndice D: Protección de cuentas de administrador integrada en Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)  
  
-   [Apéndice E: Protección de grupos de administradores de empresa en Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory.md)  
  
-   [Apéndice F: Protección de grupos de administradores de dominio de Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory.md)  
  
-   [Apéndice G: Protección de grupos de administradores en Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-G--Securing-Administrators-Groups-in-Active-Directory.md)  
  
-   [Apéndice H: Protección de cuentas de administrador Local y grupos](../../../ad-ds/plan/security-best-practices/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups.md)  
  
-   [Apéndice I: Creación de la administración de cuentas para cuentas protegidas y grupos en Active Directory](../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md)   
  
-   [Apéndice L: Eventos para supervisar](../../../ad-ds/plan/Appendix-L--Events-to-Monitor.md)  
  
-   [Apéndice M: Vínculos a documentos y lecturas recomendadas](../../../ad-ds/manage/Appendix-M--Document-Links-and-Recommended-Reading.md)  
  


