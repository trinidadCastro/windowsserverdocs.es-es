---
ms.assetid: cf32926a-2083-408b-a264-2cad179ed18a
title: Distribuir certificados en los equipos cliente mediante la directiva de grupo
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: d3a7e05e4d16565b17b69de254e353df749bbc3a
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="distribute-certificates-to-client-computers-by-using-group-policy"></a>Distribuir certificados en los equipos cliente mediante la directiva de grupo

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


Puedes usar el siguiente procedimiento para exponer los certificados de capa de Sockets seguros \(SSL\) apropiados \ (o equivalente certificados que se encadenan a una confianza root\) para los servidores de federación de cuenta, servidores de federación de recursos y servidores Web en cada equipo cliente en el bosque de asociado de la cuenta mediante la directiva de grupo.  
  
Pertenencia a **administradores de dominio** o **administradores**, o equivalente, en los servicios de dominio de Active Directory \(AD DS\) es lo mínimo necesario para completar este procedimiento.  ¿Revisar detalles sobre el uso de las cuentas adecuadas y agrupar pertenencias a [Local y dominio predeterminada grupos](https://go.microsoft.com/fwlink/?LinkId=83477) \ (http:///\/ go.microsoft.com\/fwlink\ /? LinkId\ = 83477\).   
  
### <a name="to-distribute-certificates-to-client-computers-by-using-group-policy"></a>Para distribuir los certificados en los equipos cliente mediante la directiva de grupo  
  
1.  En un controlador de dominio en el bosque de la organización de partner de cuenta, inicia el **Group Policy Management** en snap\.  
  
2.  Buscar un \(GPO\) objeto de directiva de grupo existente o crear un nuevo GPO para contener la configuración de certificado. Asegúrate de que el GPO está asociado con el dominio, un sitio o una unidad organizativa \(OU\) donde residen las cuentas de usuario y el equipo adecuadas.  
  
3.  Right\ y haga clic en el GPO y, a continuación, haz clic en **editar**.  
  
4.  En el árbol de consola, abre **directivas de clave de equipo Configuration\\Policies\\Windows Settings\\Security Settings\\Public**, right\ clic **entidades de certificación raíz de confianza**y, a continuación, haz clic en **importar**.  
  
5.  En la **el Asistente para importación de certificados** página, haz clic en **siguiente**.  
  
6.  En la **archivo para importar**, escriba la ruta de acceso a los archivos de certificado apropiado \ (por ejemplo, \\\fs1\\c$\\fs1.cer\) y, a continuación, haz clic en **siguiente**.  
  
7.  En la **almacén de certificados** página, haz clic en **colocar todos los certificados en el siguiente almacén**y, a continuación, haz clic en **siguiente**.  
  
8.  En la **completar el Asistente para importación de certificados** página, comprueba que la información que proporcionó es correcta y, a continuación, haz clic en **finalizar**.  
  
9. Repite los pasos 2a 6 para agregar certificados adicionales para cada uno de los servidores de federación de la batería.  
