---
title: Solución de problemas de activación del volumen de Windows
description: Enumera los recursos que proporcionan información acerca de los procedimientos recomendados para la activación de un volumen e información sobre la solución de problemas de activación.
ms.topic: troubleshooting
ms.date: 09/24/2019
author: Teresa-Motiv
ms.author: v-tea
manager: dcscontentpm
ms.localizationpriority: medium
ms.openlocfilehash: ce6c2e830e7c30e24112854b54e12909c43fa6f7
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87972332"
---
# <a name="troubleshooting-windows-volume-activation"></a>Solución de problemas de activación del volumen de Windows

La activación de productos es el proceso de validar el software después de haberlo instalado en un equipo concreto. La activación confirma que se trata de un producto original (no una copia fraudulenta) y que su clave o número de serie es válido y no se ha revocado, ni ha perdido su carácter confidencial. La activación también establece un vínculo o una relación entre la clave del producto y la instalación.

La activación del volumen es el proceso de activación de productos con licencias por volumen. Para convertirse en clientes de licencias por volumen, una organización debe firmar un contrato de licencia por volumen con Microsoft. Microsoft ofrece programas de licencias por volumen que se adaptan al tamaño y a las preferencias de compra de la organización. Para obtener más información, consulta el [Centro de servicios de licencias por volumen de Microsoft](https://www.microsoft.com/Licensing/servicecenter/default.aspx).

La [Guía de activación de Windows Server 2016](server-2016-activation.md) se centra en la tecnología de activación del Servicio de administración de claves (KMS). En esta sección se tratan problemas comunes y se ofrecen instrucciones para la solución de problemas de KMS y otras varias tecnologías de activación de volúmenes.

## <a name="best-practices-for-volume-activation"></a>Procedimientos recomendados para la activación de un volumen

En los siguientes artículos se proporciona información técnica y procedimientos recomendados para las tecnologías de activación por volumen de Microsoft.

### <a name="key-management-service-kms"></a>Servicio de administración de claves (KMS)

- [Plan de activación de volumen](/windows/deployment/volume-activation/plan-for-volume-activation-client)
- [Descripción de KMS](/previous-versions/tn-archive/ff793434(v=technet.10))
- [Implementación de la activación de KMS](/previous-versions/tn-archive/ff793409%28v=technet.10%29)
- [Configuración de hosts de KMS](/previous-versions/tn-archive/ff793407%28v%3dtechnet.10%29)
- [Configuring DNS](/previous-versions/tn-archive/ff793405%28v%3dtechnet.10%29) (Configuración de DNS)
- [Activar con el Servicio de administración de claves](/windows/deployment/volume-activation/activate-using-key-management-service-vamt)

### <a name="active-directory-based-activation-adba"></a>Activación basada en Active Directory (ADBA)

- [Deploy Active-Directory-based Activation](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn502534%28v%3dws.11%29) (Implementación de la activación basada en Active Directory)
- [Activar con la activación basada en Active Directory](/windows/deployment/volume-activation/activate-using-active-directory-based-activation-client)
- [Información general sobre la activación basada en Active Directory](/windows/deployment/volume-activation/active-directory-based-activation-overview)

### <a name="multiple-activation-key-mak-activation"></a>Activación de la Clave de activación múltiple (MAK)

- [Using MAK Activation](/previous-versions/tn-archive/ff793438%28v=technet.10%29) (Uso de la activación de MAK)
- [Understanding MAK Activation](/previous-versions/tn-archive/ff793435%28v%3dtechnet.10%29) (Descripción de la activación de MAK)
- [Activating MAK Clients](/previous-versions/tn-archive/ff793398%28v%3dtechnet.10%29) (Activación de clientes de MAK)

### <a name="subscription-activation"></a>Activación de suscripciones

- [Activación de la suscripción de Windows 10](/windows/deployment/windows-10-subscription-activation)
- [Implementar licencias de Windows 10 Enterprise](/windows/deployment/deploy-enterprise-licenses)
- [Windows 10 Enterprise E3 en CSP](/windows/deployment/windows-10-enterprise-e3-overview)

## <a name="resources-for-troubleshooting-activation-issues"></a>Recursos para la solución de problemas de activación

En los artículos siguientes se brindan instrucciones e información acerca de las herramientas para solucionar problemas de activación de volúmenes:

- [Instrucciones para solucionar problemas del Servicio de administración de claves (KMS)](activation-troubleshoot-kms-general.md)
- [Opciones de Slmgr.vbs para obtener información de activación de volúmenes](activation-slmgr-vbs-options.md)
- [Ejemplo: Solución de problemas de clientes de ADBA que no se activan](activation-troubleshoot-adba-clients.md)

En los artículos siguientes se proporcionan instrucciones para abordar problemas de activación más específicos:

- [Resolución de problemas de códigos de error de activación](activation-error-codes.md)
- [Activación de KMS: problemas conocidos](activation-troubleshoot-KMS-issues.md)
- [Activación de MAK: problemas conocidos](activation-troubleshoot-MAK-issues.md)
- [Instrucciones para solucionar problemas de activación relacionados con DNS](common-troubleshooting-procedures-kms-dns.md)
- [Volver a generar el archivo Tokens.dat](activation-rebuild-tokens-dat-file.md)
