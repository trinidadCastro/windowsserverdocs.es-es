---
ms.assetid: 231158d8-5e81-4630-b8d5-93fee16e0cd3
title: Identificación de la actualización de nivel funcional
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 02620fdce6132fa3868207ffe2afbf6c95e6a37b
ms.sourcegitcommit: 11421f4005f9f3a3f6c0db95b1836d0f765a9fa3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81624203"
---
# <a name="identifying-your-functional-level-upgrade"></a>Identificación de la actualización de nivel funcional

> Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Para poder elevar los niveles funcionales de dominio y bosque, debe evaluar el entorno actual e identificar el requisito de nivel funcional que mejor se adapte a las necesidades de su organización. Evalúe el entorno actual mediante la identificación de los dominios del bosque, los controladores de dominio que se encuentran en cada dominio, el sistema operativo y los Service Packs que ejecuta cada controlador de dominio, y la fecha en la que planea actualizar los controladores de dominio. Si tiene previsto retirar un controlador de dominio, asegúrese de que comprende todo el impacto que tendrá en su entorno.

Las siguientes circunstancias pueden impedir que se actualice una versión anterior del sistema operativo Windows Server al nivel funcional de Windows Server 2008 o Windows Server 2008 R2:

- Hardware insuficiente

- Un controlador de dominio que ejecute un programa antivirus que sea incompatible con Windows Server 2008 o Windows Server 2008 R2

- Uso de un programa específico de la versión que no se ejecuta en Windows Server 2008 o Windows Server 2008 R2

- La necesidad de actualizar un programa con la Service Pack más reciente

La documentación de esta información puede ayudarle a identificar los pasos que debe seguir para asegurarse de que tiene un entorno de Windows Server 2008 o Windows Server 2008 R2 totalmente funcional.

Después de evaluar el entorno actual, tiene que identificar la actualización de nivel funcional que se aplica a su organización. Estas opciones están disponibles:

- Entorno de modo nativo de Windows 2000 a Windows Server 2008 o Windows Server 2008 R2

- Bosque de Windows Server 2003 a Windows Server 2008 o Windows Server 2008 R2

- Nuevo bosque de Windows Server 2008

- Nuevo bosque de Windows Server 2008 R2

## <a name="upgrading-functional-levels-in-a-native-windows-2000-active-directory-forest"></a>Actualizar niveles funcionales en un bosque nativo de Windows 2000 Active Directory
En un entorno nativo de Windows 2000 que solo consta de controladores de dominio basados en Windows 2000, los niveles funcionales se establecen de forma predeterminada en los siguientes niveles y permanecen en estos niveles hasta que los genere manualmente:

- Nivel funcional de dominio nativo de Windows 2000

- Nivel funcional del bosque de Windows 2000

Para usar todas las características de nivel de bosque y de nivel de dominio en Windows Server 2008 o Windows Server 2008 R2, tiene que actualizar este entorno de Windows 2000 a Windows Server 2008 o Windows Server 2008 R2. Puede realizar esta actualización de cualquiera de las siguientes maneras:

- Introduzca controladores de dominio basados en Windows Server 2008 y Windows Server 2008 R2 recién instalados en el bosque y, a continuación, retire todos los controladores de dominio que ejecutan Windows 2000.

- Realice una actualización en contexto de todos los controladores de dominio existentes que ejecutan Windows 2000 en el bosque a controladores de dominio que ejecuten Windows Server 2003. A continuación, realice una actualización en contexto de los controladores de dominio a Windows Server 2008 o Windows Server 2008 R2. Para obtener más información, consulte [upgrading Active Directory Domains to Windows server 2008 and Windows server 2008 R2 AD DS Domains](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731188(v=ws.10)).

    > [!IMPORTANT]
    >  Windows Server 2008 R2 es un sistema operativo basado en x64. Si el servidor ejecuta una versión basada en x64 de Windows Server 2003, puede realizar correctamente una actualización local del sistema operativo de este equipo a Windows Server 2008 R2. Si el servidor ejecuta una versión basada en x86 de Windows Server 2003, no puede actualizar este equipo a Windows Server 2008 R2.

Para usar las características de nivel de dominio de Windows Server 2008 o Windows Server 2008 R2 sin actualizar todo el bosque de Windows 2000 a Windows Server 2008 o Windows Server 2008 R2, solo debe elevar el nivel funcional del dominio a Windows Server 2008 o Windows Server 2008 R2.

> [!NOTE]
> Antes de elevar el nivel funcional del dominio, debe actualizar todos los controladores de dominio basados en Windows 2000 de ese dominio a Windows Server 2008 o Windows Server 2008 R2.

Después de reemplazar todos los controladores de dominio basados en Windows 2000 en el bosque por controladores de dominio que ejecutan Windows Server 2008 o Windows Server 2008 R2, puede elevar el nivel funcional del bosque a Windows Server 2008 o Windows Server 2008 R2. Al hacerlo, se genera automáticamente el nivel funcional de todos los dominios del bosque que se establecen en Windows 2000 nativo o superior en Windows Server 2008 o Windows Server 2008 R2.

Para obtener más información acerca de cómo elevar los niveles funcionales de bosque y de dominio, y para los procedimientos para realizar estas tareas, vea [implementar un dominio raíz del bosque de Windows Server 2008](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc731174(v=ws.10)).

## <a name="upgrading-functional-levels-in-a-windows-server-2003-active-directory-forest"></a>Actualizar niveles funcionales en un bosque de Active Directory de Windows Server 2003
En un entorno de Windows Server 2003 que solo consta de controladores de dominio basados en Windows Server 2003, los niveles funcionales se establecen de forma predeterminada en los siguientes niveles y permanecen en estos niveles hasta que los genere manualmente:

- Nivel funcional de dominio nativo de Windows 2000

- Nivel funcional del bosque de Windows 2000

Para usar todas las características de nivel de bosque y de nivel de dominio en Windows Server 2008 o Windows Server 2008 R2, tiene que actualizar este entorno de Windows Server 2003 a Windows Server 2008 o Windows Server 2008 R2. Puede realizar esta actualización de cualquiera de las siguientes maneras:

- Introduzca un controlador de dominio basado en Windows Server 2008 o Windows Server 2008 R2 recién instalado en el bosque y, a continuación, retire todos los controladores de dominio que ejecutan Windows Server 2003 o actualícelo a Windows Server 2008 o Windows Server 2008 R2.

- Realice una actualización en contexto de todos los controladores de dominio existentes que ejecutan Windows Server 2003 a los controladores de dominio que ejecutan Windows Server 2008 o Windows Server 2008 R2. Para obtener más información, consulte [upgrading Active Directory Domains to Windows server 2008 and Windows server 2008 R2 AD DS Domains](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731188(v=ws.10)).

> [!IMPORTANT]
> Windows Server 2008 R2 es un sistema operativo basado en x64. Si el servidor ejecuta una versión basada en x64 de Windows Server 2003, puede realizar correctamente una actualización local del sistema operativo de este equipo a Windows Server 2008 R2. Si el servidor ejecuta una versión basada en x86 de Windows Server 2003, no puede actualizar este equipo para ejecutar Windows Server 2008 R2.

Para usar todas las características de nivel de dominio de Windows Server 2008 o Windows Server 2008 R2 sin actualizar todo el bosque de Windows Server 2003 a Windows Server 2008 o Windows Server 2008 R2, solo debe elevar el nivel funcional del dominio a Windows Server 2008 o Windows Server 2008 R2.

> [!NOTE]
> Antes de elevar el nivel funcional del dominio, debe actualizar todos los controladores de dominio basados en Windows Server 2003 de ese dominio a Windows Server 2008 o Windows Server 2008 R2.

Después de actualizar todos los controladores de dominio basados en Windows Server 2003 en el bosque a Windows Server 2008 o Windows Server 2008 R2, puede elevar el nivel funcional del bosque a Windows Server 2008 o Windows Server 2008 R2. Al hacerlo, se genera automáticamente el nivel funcional de todos los dominios del bosque que se establecen en Windows Server 2003 en Windows Server 2008 o Windows Server 2008 R2.

Para obtener más información acerca de cómo elevar los niveles funcionales de bosque y de dominio, y para los procedimientos para realizar estas tareas, vea [implementar un dominio raíz del bosque de Windows Server 2008](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc731174(v=ws.10)).

## <a name="upgrading-functional-levels-in-a-new-windows-server-2008-forest"></a>Actualizar niveles funcionales en un nuevo bosque de Windows Server 2008
Al instalar el primer controlador de dominio en un nuevo bosque de Windows Server 2008, los niveles funcionales se establecen de forma predeterminada en los siguientes niveles y permanecen en estos niveles hasta que los genere manualmente:

- Nivel funcional de dominio nativo de Windows 2000

- Nivel funcional del bosque de Windows 2000

Los niveles funcionales se establecen en estos niveles predeterminados para ofrecer la opción de agregar controladores de dominio basados en Windows 2000 o Windows Server 2003 al nuevo bosque de Windows Server 2008. Después de crear un dominio raíz del bosque, el nivel funcional del dominio para cada dominio que agregue al bosque de Windows Server 2008 se establece en Windows 2000 nativo. Sin embargo, si desea que todos los controladores de dominio del nuevo entorno de Windows Server 2008 ejecuten Windows Server 2008, establezca el nivel funcional del bosque y, a continuación, el nivel funcional del dominio en Windows Server 2008 al instalar el primer controlador de dominio en el bosque. Esto ahorra tiempo y habilita todas las características de nivel de bosque y de dominio en Windows Server 2008.

> [!IMPORTANT]
> Si el bosque funciona en el nivel funcional de Windows Server 2008 e intenta instalar Active Directory en un servidor miembro basado en Windows Server 2003 o en un servidor miembro basado en Windows 2000, se produce un error en la instalación.

Para obtener más información acerca de cómo elevar los niveles funcionales de bosque y de dominio, y para los procedimientos para realizar estas tareas, vea [implementar un dominio raíz del bosque de Windows Server 2008](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc731174(v=ws.10)).

## <a name="upgrading-functional-levels-in-a-new-windows-server-2008-r2-forest"></a>Actualizar niveles funcionales en un nuevo bosque de Windows Server 2008 R2
Al instalar el primer controlador de dominio en un nuevo bosque de Windows Server 2008 R2, los niveles funcionales se establecen de forma predeterminada en los siguientes niveles y permanecen en estos niveles hasta que los genere manualmente:

- Nivel funcional del dominio de Windows Server 2003

- Nivel funcional del bosque de Windows Server 2003

Los niveles funcionales se establecen en estos niveles predeterminados para ofrecer la opción de agregar controladores de dominio basados en Windows Server 2003 al nuevo bosque de Windows Server 2008 R2. Después de crear un dominio raíz del bosque, el nivel funcional del dominio para cada dominio que agregue al bosque de Windows Server 2008 R2 se establece en Windows Server 2003. Sin embargo, si desea que todos los controladores de dominio del nuevo entorno de Windows Server 2008 R2 ejecuten Windows Server 2008 R2, establezca el nivel funcional del bosque y, a continuación, el nivel funcional del dominio en Windows Server 2008 R2 al instalar el primer controlador de dominio en el bosque. Esto ahorra tiempo y habilita todas las características de nivel de bosque y de dominio en Windows Server 2008 R2.

> [!IMPORTANT]
> Si el bosque funciona en el nivel funcional de Windows Server 2008 R2 e intenta instalar Active Directory en un servidor miembro basado en Windows Server 2008 o Windows Server 2003, o en un servidor miembro basado en Windows 2000, se produce un error en la instalación.

Para obtener más información acerca de cómo elevar los niveles funcionales de bosque y de dominio, y para los procedimientos para realizar estas tareas, vea [implementar un dominio raíz del bosque de Windows Server 2008](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc731174(v=ws.10)).

> [!NOTE]
> Aunque ADMT v 3.1 debe instalarse en Windows Server 2008, puede usar ADMT v 3.1 para migrar objetos a un dominio hospedado en uno o varios controladores de dominio de Windows Server 2008 R2. Para obtener más información, vea el artículo 976659 de Microsoft Knowledge base sobre [problemas conocidos que pueden producirse al usar ADMT 3,1 para migrar a un dominio que contiene controladores de dominio de Windows Server 2008 R2](https://support.microsoft.com/help/976659/).
