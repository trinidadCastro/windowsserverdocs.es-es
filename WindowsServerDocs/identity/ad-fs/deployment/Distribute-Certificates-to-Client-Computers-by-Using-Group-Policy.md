---
description: Más información acerca de cómo distribuir certificados a equipos cliente mediante directiva de grupo
ms.assetid: cf32926a-2083-408b-a264-2cad179ed18a
title: Distribuir certificados a los equipos cliente mediante directiva de grupo
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.author: billmath
ms.openlocfilehash: fc053f7afebcab52862f23340653f2e27a44ad51
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97043433"
---
# <a name="distribute-certificates-to-client-computers-by-using-group-policy"></a>Distribuir certificados a los equipos cliente mediante directiva de grupo


Puede usar el siguiente procedimiento para desplazar los \( certificados SSL capa de sockets seguros \) adecuados \( o los certificados equivalentes que se encadenan a una raíz \) de confianza para los servidores de Federación de cuenta, los servidores de Federación de recursos y los servidores web a cada equipo cliente del bosque del asociado de cuenta mediante Directiva de grupo.

  \( \) Para completar este procedimiento, lo mínimo que se necesita es pertenecer al grupo Admins. del dominio o administradores de organización, o equivalente, en Active Directory Domain Services AD DS.  Revise los detalles sobre el uso de las cuentas y pertenencias a grupos adecuadas en [grupos predeterminados locales y de dominio](https://go.microsoft.com/fwlink/?LinkId=83477) \( http: \/ \/ Go.Microsoft.com \/ fwlink \/ ? LinkId \= 83477 \) .

### <a name="to-distribute-certificates-to-client-computers-by-using-group-policy"></a>Para distribuir certificados a los equipos cliente mediante directiva de grupo

1.  En un controlador de dominio del bosque de la organización del asociado de cuenta,  inicie el complemento \- de administración de directiva de grupo en.

2.  Busque un GPO de objeto de directiva de grupo existente \( \) o cree un nuevo GPO para que contenga la configuración del certificado. Asegúrese de que el GPO está asociado a la unidad organizativa de dominio, sitio o unidad organizativa \( \) donde residen las cuentas de usuario y equipo adecuadas.

3.  \-Haga clic con el botón secundario en el GPO y, a continuación, haga clic en **Editar**.

4.  En el árbol de la consola, abra configuración del equipo directivas configuración de Windows configuración de **\\ \\ \\ seguridad directivas de \\ clave pública**, haga clic con el botón secundario \- en entidades de **certificación raíz de confianza** y, a continuación, haga clic en **importar**.

5.  En la página **Éste es el Asistente para importación de certificados**, haga clic en **Siguiente**.

6.  En la página **archivo para importar** , escriba la ruta de acceso a los archivos de certificado correspondientes, \( por ejemplo, \\ \\ FS1 \\ c $ \\ FS1. cer \) y, a continuación, haga clic en **siguiente**.

7.  En la página **almacén de certificados** , haga clic en **colocar todos los certificados en el siguiente almacén** y, a continuación, haga clic en **siguiente**.

8.  En la página **finalización del Asistente para importación de certificados** , compruebe que la información proporcionada sea correcta y, a continuación, haga clic en **Finalizar**.

9. Repita los pasos del 2 al 6 para agregar certificados adicionales para cada uno de los servidores de Federación de la granja.
