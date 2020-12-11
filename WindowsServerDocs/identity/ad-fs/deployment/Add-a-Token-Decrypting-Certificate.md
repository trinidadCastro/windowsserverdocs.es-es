---
description: Más información acerca de cómo agregar un certificado de Token-Decrypting
ms.assetid: 27e1e299-0beb-4e86-8143-1ba031dc3502
title: Agregar un certificado de descifrado de tokens
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.author: billmath
ms.openlocfilehash: 86f45f534567b88ad07c1334c0536ebcfe05524e
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97047893"
---
# <a name="add-a-token-decrypting-certificate"></a>Agregar un certificado de descifrado de tokens

Los servidores de Federación usan un \- certificado de descifrado de token cuando un servidor de Federación de usuario de confianza debe descifrar los tokens que se emiten con un certificado anterior después de establecer un nuevo certificado como el certificado de descifrado principal. Servicios de federación de Active Directory (AD FS) \( AD FS \) usa el \( certificado Capa de sockets seguros SSL \) para Internet Information Services \( IIS \) como el certificado de descifrado predeterminado.

> [!CAUTION]
> Los certificados utilizados para \- descifrar los tokens son fundamentales para la estabilidad del servicio de Federación. Dado que la pérdida o la eliminación no planeada de los certificados configurados con este fin pueden interrumpir el servicio, debe realizar una copia de seguridad de todos los certificados configurados con este fin.

Puede usar el siguiente procedimiento para agregar el certificado de \- descifrado de tokens al complemento de administración de AD FS \- desde un archivo que haya exportado.

La pertenencia al grupo **Administradores** o equivalente en el equipo local es el requisito mínimo necesario para completar este procedimiento.  Revise los detalles sobre el uso de las cuentas y pertenencias a grupos adecuadas en [grupos predeterminados locales y de dominio](https://go.microsoft.com/fwlink/?LinkId=83477) \( http: \/ \/ Go.Microsoft.com \/ fwlink \/ ? LinkId \= 83477 \) .

### <a name="to-add-a-token-decrypting-certificate"></a>Para agregar un \- certificado de descifrado de tokens

1.  En la pantalla **Inicio** , escriba **AD FS Management** y, a continuación, presione Entrar.

2.  En el árbol de consola, haga doble \- clic en **servicio** y, a continuación, haga clic en **certificados**.

3.  En el panel **acciones** , haga clic en el vínculo **Agregar certificado de \- descifrado de tokens** .

4.  En el cuadro de diálogo **Buscar archivo de certificado** , navegue hasta el archivo de certificado que desea agregar, seleccione el archivo de certificado y, a continuación, haga clic en **abrir**.

## <a name="additional-references"></a>Referencias adicionales
[Lista de comprobación: configurar un servidor de federación](Checklist--Setting-Up-a-Federation-Server.md)

[Requisitos de certificado para servidores de federación](../design/certificate-requirements-for-federation-servers.md)

