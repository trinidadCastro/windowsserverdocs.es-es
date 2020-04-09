---
ms.assetid: e2651dc8-4b31-4cd8-a400-3b8123890210
title: Procedimientos recomendados para proteger Active Directory
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 8935a08b9ab8f99b5f221a14b93974872f11a091
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80821298"
---
# <a name="best-practices-for-securing-active-directory"></a>Procedimientos recomendados para proteger Active Directory

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En este documento se proporciona la perspectiva de un profesional y contiene un conjunto de técnicas prácticas para ayudar a los ejecutivos de ti a proteger un entorno de Active Directory empresarial. Active Directory desempeña un papel fundamental en la infraestructura de TI y garantiza la armonía y la seguridad de diferentes recursos de red en un entorno global interconectado. Los métodos descritos se basan en gran medida en la experiencia de la organización de Microsoft Information Security and Risk Management (ISRM), que es responsable de proteger los activos de Microsoft IT y otras divisiones empresariales de Microsoft, además de aconsejar a un número seleccionado de clientes de Microsoft Global 500.  
  
-   [Resumen ejecutivo](../../../ad-ds/manage/component-updates/Executive-Summary.md)  
  
-   [Introducción](../../../ad-ds/manage/component-updates/Introduction.md)  
  
-   [Vías de compromiso](../../../ad-ds/plan/security-best-practices/Avenues-to-Compromise.md)  
  
-   [Cuentas atractivas para el robo de credenciales](../../../ad-ds/plan/security-best-practices/Attractive-Accounts-for-Credential-Theft.md)  
  
-   [Reducción de la superficie expuesta a ataques de Active Directory](../../../ad-ds/plan/security-best-practices/Reducing-the-Active-Directory-Attack-Surface.md)  
  
-   [Implementación de modelos administrativos con privilegios mínimos](../../../ad-ds/plan/security-best-practices/Implementing-Least-Privilege-Administrative-Models.md)  
  
-   [Implementación de hosts administrativos seguros](../../../ad-ds/plan/security-best-practices/Implementing-Secure-Administrative-Hosts.md)  
  
-   [Protección de los controladores de dominio frente a ataques](../../../ad-ds/plan/security-best-practices/Securing-Domain-Controllers-Against-Attack.md)  
  
-   [Supervisión Active Directory los síntomas de riesgo](../../../ad-ds/plan/security-best-practices/Monitoring-Active-Directory-for-Signs-of-Compromise.md)  
  
-   [Recomendaciones de la directiva de auditoría](../../../ad-ds/plan/security-best-practices/Audit-Policy-Recommendations.md)  
  
-   [Planeación del riesgo](../../../ad-ds/plan/security-best-practices/Planning-for-Compromise.md)  
  
-   [Mantenimiento de un entorno más seguro](../../../ad-ds/plan/security-best-practices/Maintaining-a-More-Secure-Environment.md)  
  
-   [Apéndices](../../../ad-ds/plan/security-best-practices/Appendices.md)  
   
-   [Apéndice B: cuentas y grupos con privilegios en Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-B--Privileged-Accounts-and-Groups-in-Active-Directory.md)  
  
-   [Apéndice C: cuentas y grupos protegidos en Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md)  
  
-   [Apéndice D: protección de cuentas de administrador integradas en Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)  
  
-   [Apéndice E: protección de grupos de administradores de empresa en Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory.md)  
  
-   [Apéndice F: protección de grupos Admins. del dominio en Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory.md)  
  
-   [Apéndice G: protección de grupos de administradores en Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-G--Securing-Administrators-Groups-in-Active-Directory.md)  
  
-   [Apéndice H: protección de grupos y cuentas de administrador local](../../../ad-ds/plan/security-best-practices/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups.md)  
  
-   [Apéndice I: crear cuentas de administración para cuentas y grupos protegidos en Active Directory](../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md)   
  
-   [Apéndice L: eventos para supervisar](../../../ad-ds/plan/Appendix-L--Events-to-Monitor.md)  
  
-   [Apéndice M: vínculos de documentos y lecturas recomendadas](../../../ad-ds/manage/Appendix-M--Document-Links-and-Recommended-Reading.md)  
  


