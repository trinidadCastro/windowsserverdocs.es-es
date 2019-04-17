---
ms.assetid: e2651dc8-4b31-4cd8-a400-3b8123890210
title: Los procedimientos recomendados para la seguridad de Active Directory
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: ecef0c173677d379524189b1769d4721ab0774a8
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="best-practices-for-securing-active-directory"></a>Los procedimientos recomendados para la seguridad de Active Directory

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este documento proporciona una perspectiva de un médico y contiene un conjunto de técnicas prácticas para ayudar a los ejecutivos de TI a proteger un entorno de Active Directory de la empresa. Active Directory desempeña un papel esencial en la infraestructura de TI y garantiza la armonía y la seguridad de los recursos de red diferente en un entorno global e interconectado. Los métodos descritos se basan en gran medida la experiencia de la organización de seguridad de la información de Microsoft y administración de riesgo (ISRM), que es responsable de protección de los activos de IT de Microsoft y otros divisiones de empresas de Microsoft, además de un número de clientes de 500 Global de Microsoft seleccionados que informa.  
  
-   [Prólogo](https://technet.microsoft.com/library/dn487451.aspx)  
  
-   [Confirmaciones](https://technet.microsoft.com/library/dn487445.aspx)  
  
-   [Resumen](../../../ad-ds/manage/component-updates/Executive-Summary.md)  
  
-   [Introducción](../../../ad-ds/manage/component-updates/Introduction.md)  
  
-   [Vías peligro](../../../ad-ds/plan/security-best-practices/Avenues-to-Compromise.md)  
  
-   [Cuentas atractivas para el robo de credenciales](../../../ad-ds/plan/security-best-practices/Attractive-Accounts-for-Credential-Theft.md)  
  
-   [Reducir la superficie de ataque de Active Directory](../../../ad-ds/plan/security-best-practices/Reducing-the-Active-Directory-Attack-Surface.md)  
  
-   [Implementación de modelos de privilegios mínimos administrativos](../../../ad-ds/plan/security-best-practices/Implementing-Least-Privilege-Administrative-Models.md)  
  
-   [Implementación de Hosts administrativos seguros](../../../ad-ds/plan/security-best-practices/Implementing-Secure-Administrative-Hosts.md)  
  
-   [Seguridad de los controladores de dominio contra los ataques](../../../ad-ds/plan/security-best-practices/Securing-Domain-Controllers-Against-Attack.md)  
  
-   [Supervisión de Active Directory para signos de compromiso](../../../ad-ds/plan/security-best-practices/Monitoring-Active-Directory-for-Signs-of-Compromise.md)  
  
-   [Recomendaciones de la directiva de auditoría](../../../ad-ds/plan/security-best-practices/Audit-Policy-Recommendations.md)  
  
-   [Planeación de comprometer](../../../ad-ds/plan/security-best-practices/Planning-for-Compromise.md)  
  
-   [Mantenimiento de un entorno más seguro](../../../ad-ds/plan/security-best-practices/Maintaining-a-More-Secure-Environment.md)  
  
-   [Apéndices](../../../ad-ds/plan/security-best-practices/Appendices.md)  
   
-   [Apéndice B: cuentas con privilegios y grupos de Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-B--Privileged-Accounts-and-Groups-in-Active-Directory.md)  
  
-   [Apéndice C: cuentas protegidas y grupos de Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md)  
  
-   [Apéndice D: seguridad de las cuentas de administrador integrada en Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)  
  
-   [Apéndice E: proteger los grupos de administradores de empresa en Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory.md)  
  
-   [Apéndice F: proteger grupos de administradores de dominio de Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory.md)  
  
-   [Apéndice G: proteger el grupo de administradores en Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-G--Securing-Administrators-Groups-in-Active-Directory.md)  
  
-   [Apéndice H: proteger cuentas de administrador Local y grupos](../../../ad-ds/plan/security-best-practices/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups.md)  
  
-   [Apéndice I: crear administración de cuentas para cuentas protegidas y los grupos de Active Directory](../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md)   
  
-   [Apéndice L: eventos al Monitor](../../../ad-ds/plan/Appendix-L--Events-to-Monitor.md)  
  
-   [Apéndice M: vínculos del documento y lectura recomendada](../../../ad-ds/manage/Appendix-M--Document-Links-and-Recommended-Reading.md)  
  


