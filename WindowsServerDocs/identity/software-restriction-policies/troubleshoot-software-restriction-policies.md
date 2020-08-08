---
title: Solución de problemas de las directivas de restricción de software
description: Seguridad de Windows Server
ms.topic: article
ms.assetid: 4fd53736-03e7-4bf9-ba90-d1212d93e19a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 6e4d31dd6e434c5a5b18491ea7f73b92c993e05a
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87953012"
---
# <a name="troubleshoot-software-restriction-policies"></a>Solución de problemas de las directivas de restricción de software

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En este tema se describen los problemas comunes y sus soluciones al solucionar problemas de las directivas de restricción de software (SRP) a partir de Windows Server 2008 y Windows Vista.

## <a name="introduction"></a>Introducción
Las directivas de restricción de software (SRP) son una característica basada en directivas de grupo que identifica los programas de software que se ejecutan en los equipos de un dominio y controla la capacidad de ejecución de dichos programas. Puede usar las directivas de restricción de software para crear una configuración muy restringida para los equipos, en los que solamente pueden ejecutarse aquellas aplicaciones específicamente identificadas. Se integran con Microsoft Active Directory Domain Services y directiva de grupo, pero también se pueden configurar en equipos independientes. Para obtener más información acerca de SRP, consulte las [directivas de restricción de software](software-restriction-policies.md).

A partir de Windows Server 2008 R2 y Windows 7, se puede usar Windows AppLocker en lugar de o junto con SRP para una parte de la estrategia de control de la aplicación.

### <a name="windows-cannot-open-a-program"></a>Windows no puede abrir un programa
Los usuarios reciben un mensaje que indica "Windows no puede abrir este programa porque se ha impedido mediante una directiva de restricción de software. Para obtener más información, abra Visor de eventos o póngase en contacto con el administrador del sistema ". O bien, en la línea de comandos, un mensaje indica "el sistema no puede ejecutar el programa especificado".

**Causa:** Se creó el nivel de seguridad predeterminado (o una regla) para que el programa de software se establezca como no **permitido**y, como resultado, no se iniciará.

**Solución:** Busque en el registro de eventos una descripción detallada del mensaje. El mensaje del registro de eventos indica qué programa de software está establecido como no **permitido** y qué regla se aplica al programa.

### <a name="modified-software-restriction-policies-are-not-taking-effect"></a>Las directivas de restricción de software modificadas no surten efecto
**Causa:** Las directivas de restricción de software que se especifican en un dominio a través de directiva de grupo invalidan las configuraciones de directiva configuradas localmente. Esto puede implicar que hay una configuración de directiva del dominio que invalida la configuración de la Directiva.

**Causa:** Es posible que directiva de grupo no haya actualizado su configuración de directiva. Directiva de grupo aplica los cambios a la configuración de directiva periódicamente; por lo tanto, es probable que los cambios en la Directiva que se realizaron en el directorio todavía no se hayan actualizado.

**Solución**

1.  El equipo en el que se modifican las directivas de restricción de software para la red debe ser capaz de ponerse en contacto con un controlador de dominio. Asegúrese de que el equipo puede ponerse en contacto con un controlador de dominio.

2.  Actualice la Directiva cerrando sesión en la red y, a continuación, vuelva a iniciar sesión en la red. Si alguna directiva se aplica a través de directiva de grupo, al volver a iniciarla, se actualizarán esas directivas.

3.  Puede actualizar la configuración de directiva con la utilidad de línea de comandos gpupdate o cerrando la sesión en el equipo. Para obtener los mejores resultados, ejecute gpupdate y cierre la sesión y vuelva a iniciarla en el equipo. Por lo general, la configuración de seguridad se actualiza cada 90 minutos en una estación de trabajo o servidor y cada 5 minutos en un controlador de dominio. La configuración también se actualiza cada 16 horas, haya o no cambios. Estos son los valores configurables, por lo que los intervalos de actualización pueden ser diferentes en cada dominio.

4.  Compruebe qué directivas se aplican. Compruebe la configuración de directivas de nivel de dominio para **no reemplazar** .

5.  Las directivas de restricción de software que se especifican en un dominio a través de directiva de grupo invalidan las directivas configuradas localmente. Use la herramienta de línea de comandos GPResult para determinar cuál es el efecto neto de la Directiva. Esto puede implicar que hay una directiva del dominio que invalida la configuración local.

6.  Si la configuración de directivas de SRP y AppLocker está en el mismo GPO, la configuración de AppLocker tendrá prioridad en Windows 7, Windows Server 2008 R2 y versiones posteriores. Se recomienda colocar la configuración de directivas de SRP y AppLocker en distintos GPO.

### <a name="after-adding-a-rule-through-srp-you-cannot-log-on-to-your-computer"></a>Después de agregar una regla a través de SRP, no puede iniciar sesión en el equipo
**Causa:** Al iniciarse, el equipo tiene acceso a muchos programas y archivos. Es posible que haya establecido involuntariamente uno de estos programas o archivos en no **permitido**. Dado que el equipo no puede tener acceso al programa o archivo, no se puede iniciar correctamente.

**Solución:** Inicie el equipo en modo seguro, inicie sesión como administrador local y, a continuación, cambie las directivas de restricción de software para permitir que se ejecute el programa o el archivo.

### <a name="a-new-policy-setting-is-not-applying-to-a-specific-file-name-extension"></a>Una nueva configuración de Directiva no se aplica a una extensión de nombre de archivo específica
**Causa:** La extensión de nombre de archivo no está en la lista de tipos de archivo admitidos.

**Solución:** Agregue la extensión de nombre de archivo a la lista de tipos de archivo admitidos por SRP.

Las directivas de restricción de software solucionan el problema de regular el código desconocido o que no es de confianza. Las directivas de restricción de software son configuraciones de seguridad para identificar el software y controlar su capacidad para ejecutarse en un equipo local, en un sitio, dominio o unidad organizativa y se pueden implementar a través de un GPO.

### <a name="a-default-rule-is-not-restricting-as-expected"></a>Una regla predeterminada no se restringe según lo esperado
**Causa:** Reglas que se aplican en un orden determinado que puede hacer que las reglas predeterminadas se invaliden mediante reglas específicas. SRP aplica las reglas en el siguiente orden (más específico a general):

1.  Reglas de hash

2.  Reglas de certificado

3.  Reglas de ruta de acceso

4.  Reglas de zona de Internet

5.  Reglas predeterminadas

**Solución:** Evalúe las reglas restringiendo la aplicación y, si es necesario, quite todas las reglas excepto la predeterminada.

### <a name="unable-to-discover-which-restrictions-are-applied"></a>No se pueden detectar las restricciones que se aplican
**Causa:** No hay ninguna causa aparente para el comportamiento inesperado y la actualización del GPO no ha resuelto el problema, por lo que es necesaria una investigación adicional.

**Solución**

1.  Investigue el registro de eventos del sistema, filtrando por el origen de "Directiva de restricción de software". Las entradas expresan explícitamente qué regla se implementa para cada aplicación.

2.  Habilitación del registro avanzado. Para obtener más información [, vea determinar la lista de permitidos y el inventario de aplicaciones para directivas de restricción de software](software-restriction-policies.md) .


