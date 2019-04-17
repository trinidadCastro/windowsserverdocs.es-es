---
title: "Solucionar problemas de las directivas de restricción de Software"
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
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="troubleshoot-software-restriction-policies"></a>Solucionar problemas de las directivas de restricción de Software

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tema describen problemas comunes y sus soluciones al solucionar problemas de Windows Server 2008 y Windows Vista a partir de las directivas de restricción de Software (SRP).

## <a name="introduction"></a>Introducción
Directivas de restricción de software (SRP) es la característica de directiva de grupo que identifica los programas de software que se ejecutan en los equipos de un dominio y controla la capacidad de esos programas para ejecutarse. Usa las directivas de restricción de software para crear una configuración muy restringida para equipos, en los que permites solo específicamente identificadas aplicaciones que se ejecutan. Estos se integran con Microsoft Active Directory Domain Services y la directiva de grupo, pero también pueden configurarse en equipos independientes. Para obtener más información acerca de SRP, consulta el [las directivas de restricción de Software](software-restriction-policies.md).

A partir de Windows Server 2008 R2 y Windows 7, Windows AppLocker puede usarse en lugar de, o conjuntamente con SRP para una parte de la estrategia de control de la aplicación.

### <a name="windows-cannot-open-a-program"></a>Windows no puede abrir un programa
Los usuarios reciben un mensaje que dice "Windows no puede abrir este programa ya lo ha impedido una directiva de restricción de software. Para obtener más información, abre el Visor de eventos o ponte en contacto con el administrador del sistema." O bien, en la línea de comandos, un mensaje dice "el sistema no puede ejecutar el programa especificado".

**Causa:** el nivel de seguridad predeterminado (o una regla) se creó para que el programa de software se establece como **no permitido**, y como resultado no se iniciará.

**Solución:** buscar en el registro de eventos para obtener una descripción detallada del mensaje. El mensaje de registro de eventos indica qué programa de software se establece como **no permitido** y qué regla se aplica al programa.

### <a name="modified-software-restriction-policies-are-not-taking-effect"></a>Las directivas de restricción de software modificado no tienen ningún efecto
**Causa:** las directivas de restricción de Software que se especifican en un dominio mediante la directiva de grupo invalidación cualquier configuración de directiva que está configuradas localmente. Esto puede implicar que hay una configuración de directiva del dominio que está invalidando la configuración de directiva.

**Causa:** actualiza Directiva de grupo es posible que no dispone de su configuración de directiva. Directiva de grupo aplica cambios de configuración de directiva periódicamente; por lo tanto, es probable que no han sido actualizados los cambios de directiva que se realizaron en el directorio.

**Soluciones:**

1.  El equipo en el que modificar las directivas de restricción de software de la red debe ser capaz de ponerse en contacto con un controlador de dominio. Asegúrate de que el equipo puede ponerse en contacto con un controlador de dominio.

2.  Actualizar la directiva iniciando sesión en la red y, a continuación, iniciar sesión la red nuevamente. Si se aplica a cualquier directiva mediante la directiva de grupo, el registro en actualizará las directivas.

3.  Se puede actualizar la configuración de directiva con la utilidad de línea de comandos de gpupdate o cierre de sesión y, a continuación, volver a iniciar sesión el equipo. Para obtener mejores resultados, ejecutar gpupdate y, a continuación, cierra la sesión de y volver a iniciarla para el equipo. Por lo general, la configuración de seguridad se actualiza cada 90 minutos en un servidor o estación de trabajo y cada 5 minutos en un controlador de dominio. La configuración se actualiza también cada 16 horas, se los cambios o no. Dado que son opciones configurables, intervalos de actualización pueden ser diferentes en cada dominio.

4.  Comprueba qué directivas se aplican. Comprobar las directivas de nivel de dominio para **No anular** configuración.

5.  Las directivas de restricción de software que se especifican en un dominio mediante la directiva de grupo invalidación cualquier directivas configuradas localmente. Usa la herramienta de línea de comandos Gpresult para determinar cuál es el efecto neto de la directiva. Esto puede implicar que hay una directiva del dominio que está invalidando la configuración local.

6.  Si la configuración de directiva de SRP y AppLocker está en el mismo GPO, las opciones de AppLocker tienen prioridad en Windows 7, Windows Server 2008 R2 y versiones posteriores. Se recomienda que la configuración de directiva SRP y AppLocker en diferentes GPO.

### <a name="after-adding-a-rule-through-srp-you-cannot-log-on-to-your-computer"></a>Después de agregar una regla a través de SRP, no se puede iniciar sesión en tu equipo
**Causa:** accede a tu equipo muchos programas y archivos cuando se inicia. Puede haber accidentalmente establecido uno de estos programas o archivos **no permitido**. Dado que el equipo no puede acceder el programa o archivo, no se inicie correctamente.

**Solución:** inicia el equipo en modo seguro, iniciar sesión como administrador local y, a continuación, cambiar las directivas de restricción de software para permitir que el programa o archivo para ejecutarse.

### <a name="a-new-policy-setting-is-not-applying-to-a-specific-file-name-extension"></a>Una nueva configuración de directiva no se aplica a una extensión de nombre de archivo específico
**Causa:** la extensión de nombre de archivo no está en la lista de tipos de archivo admitidos.

**Solución:** agrega la extensión de nombre de archivo a la lista de tipos de archivo compatibles con SRP.

Las directivas de restricción de software solucionar el problema de regular el código de confianza o desconocido. Las directivas de restricción de software son opciones de configuración de seguridad para identificar el software y controlar su capacidad para ejecutarse en un equipo local, en un sitio, dominio o unidad organizativa y pueden implementarse a través de un GPO.

### <a name="a-default-rule-is-not-restricting-as-expected"></a>Una regla predeterminada no es restringir según lo esperado
**Causa:** reglas que se aplican en un determinado orden, lo que puede provocar que las reglas predeterminadas para ser invalidadas por las reglas específicas. SRP aplica reglas en el siguiente orden (más específico general):

1.  Reglas de hash

2.  Reglas de certificado

3.  Reglas de ruta

4.  Reglas de zona de Internet

5.  Reglas predeterminadas

**Solución:** evaluar las reglas de restricción de la aplicación y, si procede, quitar todo excepto la regla predeterminada.

### <a name="unable-to-discover-which-restrictions-are-applied"></a>No se puede detectar qué restricciones se aplican
**Causa:** no hay ninguna causa aparente para el comportamiento inesperado y actualización de GPO no habrá resuelto el problema, por lo que es necesario investigar.

**Soluciones:**

1.  Investiga el registro de eventos del sistema, filtrado en origen de la "Directiva de restricción de Software". Las entradas declara de forma explícita la regla que se implementa para cada aplicación.

2.  Habilitar el registro avanzado. Consulta [determinar lista Denegar permitir e inventario de aplicaciones para las directivas de restricción de Software](software-restriction-policies.md) para obtener más información.


