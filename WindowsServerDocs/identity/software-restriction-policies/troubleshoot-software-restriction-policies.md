---
title: Solución de problemas de las directivas de restricción de software
description: Seguridad de Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-software-restriction-policies
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4fd53736-03e7-4bf9-ba90-d1212d93e19a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: abf592930f5347fa10e83c644671f68f9dc1a3f6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872386"
---
# <a name="troubleshoot-software-restriction-policies"></a>Solución de problemas de las directivas de restricción de software

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En este tema se describe problemas comunes y sus soluciones al solucionar problemas de directivas de restricción de Software (SRP) a partir de Windows Server 2008 y Windows Vista.

## <a name="introduction"></a>Introducción
Las directivas de restricción de software (SRP) son una característica basada en directivas de grupo que identifica los programas de software que se ejecutan en los equipos de un dominio y controla la capacidad de ejecución de dichos programas. Puede usar las directivas de restricción de software para crear una configuración muy restringida para los equipos, en los que solamente pueden ejecutarse aquellas aplicaciones específicamente identificadas. Se integran con Microsoft Active Directory Domain Services y directiva de grupo, pero también puede configurarse en equipos independientes. Para obtener más información sobre SRP, consulte el [Software Restriction Policies](software-restriction-policies.md).

Empezando por Windows Server 2008 R2 y Windows 7, Windows AppLocker puede usarse en lugar de o junto con SRP para una parte de su estrategia de control de la aplicación.

### <a name="windows-cannot-open-a-program"></a>Windows no pueden abrir un programa
Los usuarios reciben un mensaje que dice "Windows no pueden abrir este programa porque lo impide una directiva de restricción de software. Para obtener más información, abra el Visor de eventos o póngase en contacto con el administrador del sistema". O bien, en la línea de comandos, un mensaje que dice "el sistema no puede ejecutar el programa especificado".

**Causa:** El nivel de seguridad predeterminado (o una regla) se creó para que el programa de software se establece como **no permitido**, y así no se iniciará.

**Solución:** Busque en el registro de eventos para obtener una descripción detallada del mensaje. El mensaje de registro de eventos indica qué programa de software se ha establecido como **no permitido** y qué regla se aplica al programa.

### <a name="modified-software-restriction-policies-are-not-taking-effect"></a>Las directivas de restricción de software modificado no surten efecto
**Causa:** Las directivas de restricción de software que se especifican en un dominio a través de la directiva de grupo reemplazan cualquier configuración de directiva configuradas localmente. Esto podría implicar que hay una configuración de directiva del dominio que está reemplazando la configuración de directiva.

**Causa:** Directiva de grupo no es posible que haya actualizado a su configuración de directiva. Directiva de grupo aplica los cambios de configuración de directiva periódicamente; por lo tanto, es probable que no se han actualizado todavía los cambios de directiva que se realizaron en el directorio.

**Soluciones:**

1.  El equipo en el que modifica las directivas de restricción de software para la red debe ser capaz de ponerse en contacto con un controlador de dominio. Asegúrese de que el equipo puede ponerse en contacto con un controlador de dominio.

2.  Actualizar la directiva de sesión fuera de la red y, a continuación, inicia sesión en la red nuevo. Si se aplica ninguna directiva a través de la directiva de grupo, vuelven a iniciar sesión actualizará esas directivas.

3.  Puede actualizar la configuración de directiva con la utilidad de línea de comandos de gpupdate o cerrar la sesión y, a continuación, volver a iniciar sesión el equipo. Para obtener mejores resultados, ejecute gpupdate y, a continuación, cierre la sesión de y volver a iniciarla para su equipo. Por lo general, la configuración de seguridad se actualiza cada 90 minutos en un servidor o estación de trabajo y cada 5 minutos en un controlador de dominio. La configuración también se actualiza cada 16 horas, haya o no cambios. Estos son los valores configurables para que intervalos de actualización pueden ser diferentes en cada dominio.

4.  Comprobación de las directivas que se aplican. Compruebe las directivas de nivel de dominio para **No reemplazar** configuración.

5.  Las directivas de restricción de software que se especifican en un dominio a través de la directiva de grupo anulan las directivas configuradas localmente. Use la herramienta de línea de comandos Gpresult para determinar cuál es el efecto neto de la directiva. Esto podría implicar que hay una directiva del dominio que está reemplazando la configuración local.

6.  Si configuración de directivas SRP y AppLocker se encuentran en el mismo GPO, la configuración de AppLocker tendrán prioridad en Windows 7, Windows Server 2008 R2 y versiones posteriores. Se recomienda que la configuración de directiva SRP y AppLocker en diferentes GPO.

### <a name="after-adding-a-rule-through-srp-you-cannot-log-on-to-your-computer"></a>Después de agregar una regla a través de SRP, no se puede iniciar sesión en su equipo
**Causa:** El equipo tiene acceso a muchos programas y archivos cuando se inicia. Es posible que accidentalmente configuró uno de estos programas o archivos que **no permitido**. Dado que el equipo no puede acceder el programa o archivo, no se puede iniciar correctamente.

**Solución:** Inicie el equipo en modo seguro, inicie sesión como administrador local y, a continuación, cambiar las directivas de restricción de software para permitir que el programa o archivo va a ejecutar.

### <a name="a-new-policy-setting-is-not-applying-to-a-specific-file-name-extension"></a>Una nueva configuración de directiva no se aplica a una extensión de nombre de archivo específico
**Causa:** La extensión de nombre de archivo no está en la lista de tipos de archivo admitidos.

**Solución:** Agregue la extensión de nombre de archivo a la lista de tipos de archivo compatibles con SRP.

Las directivas de restricción de software solucionan el problema de regulación código desconocido o no. Las directivas de restricción de software están la configuración de seguridad para identificar el software y controlar su capacidad para ejecutarse en un equipo local, en un sitio, dominio o unidad organizativa y pueden implementarse a través de un GPO.

### <a name="a-default-rule-is-not-restricting-as-expected"></a>Una regla predeterminada no es restringe según lo previsto
**Causa:** Reglas que se aplican en un orden concreto que puede hacer que las reglas predeterminadas que se invalide por determinadas reglas. SRP aplica reglas en el orden siguiente (más específica a general):

1.  Reglas de hash

2.  Reglas de certificado

3.  Reglas de ruta de acceso

4.  Reglas de zona de Internet

5.  Reglas predeterminadas

**Solución:** Evalúe las reglas de restricción de la aplicación y, si es necesario, quitar todo salvo la regla predeterminada.

### <a name="unable-to-discover-which-restrictions-are-applied"></a>No se puede detectar qué restricciones se aplican
**Causa:** No hay ninguna causa aparente de un comportamiento inesperado y actualización de GPO no ha resuelto el problema, por lo que es necesario seguir investigando.

**Soluciones:**

1.  Investigar el registro de eventos del sistema, el filtrado en el origen de la "Directiva de restricción de Software." Las entradas de indicar explícitamente qué regla se implementa para cada aplicación.

2.  Habilitar el registro avanzado. Consulte [determinar lista de denegación para permitir y el inventario de aplicaciones para las directivas de restricción de Software](software-restriction-policies.md) para obtener más información.


