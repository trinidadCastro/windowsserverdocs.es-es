---
ms.assetid: cf32926a-2083-408b-a264-2cad179ed18a
title: Distribuir certificados a los equipos cliente mediante la directiva de grupo
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 11cdd9c75ca588ebeac9387e6512fee439621bf8
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192162"
---
# <a name="distribute-certificates-to-client-computers-by-using-group-policy"></a>Distribuir certificados a los equipos cliente mediante la directiva de grupo


Puede usar el procedimiento siguiente para desplazar hacia abajo de la capa de Sockets seguros adecuado \(SSL\) certificados \(o equivalente en los certificados vinculados a una raíz de confianza\) para servidores de federación de cuenta, los servidores de federación de recursos y servidores Web para cada equipo cliente en el bosque del asociado de cuenta mediante la directiva de grupo.  
  
Pertenencia a **Admins. del dominio** o **administradores de empresas**, o equivalente, en servicios de dominio de Active Directory \(AD DS\) es el requisito mínimo para completar este procedimiento.  ¿Revise los detalles sobre el uso de las cuentas adecuadas y pertenencia a grupos en [dominio grupos predeterminados locales y](https://go.microsoft.com/fwlink/?LinkId=83477) \(http:\/\/go.microsoft.com\/fwlink\/? LinkId\=83477\).   
  
### <a name="to-distribute-certificates-to-client-computers-by-using-group-policy"></a>Para distribuir certificados a los equipos cliente mediante la directiva de grupo  
  
1.  En un controlador de dominio del bosque de la organización del asociado de cuenta, inicie la **Group Policy Management** ajustar\-en.  
  
2.  Buscar un objeto de directiva de grupo existente \(GPO\) o crear un nuevo GPO para que contenga la configuración del certificado. Asegúrese de que el GPO está asociado con el dominio, sitio o unidad organizativa \(OU\) donde residen las cuentas de usuario y equipo adecuadas.  
  
3.  Derecha\-haga clic en el GPO y, a continuación, haga clic en **editar**.  
  
4.  En el árbol de consola, abra **configuración del equipo\\directivas\\Windows configuración\\configuración de seguridad\\directivas de clave pública**, derecha\-haga clic en **Entidades emisoras raíz de confianza**y, a continuación, haga clic en **importación**.  
  
5.  En la página **Éste es el Asistente para importación de certificados**, haga clic en **Siguiente**.  
  
6.  En el **archivo para importar** página, escriba la ruta de acceso a los archivos de certificado adecuada \(por ejemplo, \\ \\fs1\\c$\\fs1.cer\)y, a continuación, haga clic en **Siguiente**.  
  
7.  En el **Certificate Store** página, haga clic en **colocar todos los certificados en el siguiente almacén**y, a continuación, haga clic en **siguiente**.  
  
8.  En el **completar el Asistente para importación de certificados** página, compruebe que la información que proporcionó es correcta y, a continuación, haga clic en **finalizar**.  
  
9. Repita los pasos del 2 al 6 para agregar certificados adicionales para cada uno de los servidores de federación en la granja de servidores.  
