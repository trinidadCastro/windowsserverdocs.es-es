---
ms.assetid: bbb84ea6-7e31-4442-85ab-a9447e7c19e8
title: Agregar un certificado de firma de tokens
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.author: billmath
ms.openlocfilehash: 31a624b85d68611be0661d6efcc8f33d24efdc54
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87947626"
---
# <a name="add-a-token-signing-certificate"></a>Agregar un certificado de firma de tokens


Los servidores de Federación en Servicios de federación de Active Directory (AD FS) \( AD FS \) requieren certificados de firma de tokens \- para evitar que los atacantes modifiquen o falsifican los tokens de seguridad en un intento de obtener acceso no autorizado a los recursos federados. Cada \- certificado de firma de tokens contiene claves privadas criptográficas y claves públicas que se usan para firmar digitalmente \( por medio de la clave privada \) un token de seguridad. Más adelante, después de que un servidor de Federación asociado reciba estas claves, validará la autenticidad \( mediante la clave pública \) del token de seguridad cifrado.

> [!CAUTION]
> Los certificados usados para la \- firma de tokens son fundamentales para la estabilidad del servicio de Federación. Dado que la pérdida o la eliminación no planeada de los certificados configurados con este fin pueden interrumpir el servicio, debe realizar una copia de seguridad de todos los certificados configurados con este fin.

El \- certificado de firma de tokens debe encadenarse a una raíz de confianza en el servicio de Federación. Puede usar el siguiente procedimiento para agregar el certificado de firma de tokens \- al complemento de administración de AD FS \- desde un archivo que haya exportado.

La pertenencia al grupo **Administradores** o equivalente en el equipo local es el requisito mínimo necesario para completar este procedimiento.  Revise los detalles sobre el uso de las cuentas y pertenencias a grupos adecuadas en [grupos predeterminados locales y de dominio](https://go.microsoft.com/fwlink/?LinkId=83477) \( http: \/ \/ Go.Microsoft.com \/ fwlink \/ ? LinkId \= 83477 \) .

### <a name="to-add-a-token-signing-certificate"></a>Para agregar un certificado de firma de tokens \-

1.  En la pantalla **Inicio** , escriba**AD FS Management**y, a continuación, presione Entrar.

2.  En el árbol de consola, haga doble \- clic en **servicio**y, a continuación, haga clic en **certificados**.

3.  En el panel **acciones** , haga clic en el vínculo **Agregar certificado de \- firma de tokens** .

4.  En el cuadro de diálogo **Buscar archivo de certificado** , navegue hasta el archivo de certificado que desea agregar, seleccione el archivo de certificado y, a continuación, haga clic en **abrir**.

## <a name="additional-references"></a>Referencias adicionales
[Lista de comprobación: configurar un servidor de federación](Checklist--Setting-Up-a-Federation-Server.md)

[Requisitos de certificado para servidores de federación](../design/certificate-requirements-for-federation-servers.md)

