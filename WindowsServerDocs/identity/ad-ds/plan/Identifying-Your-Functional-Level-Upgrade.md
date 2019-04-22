---
ms.assetid: 231158d8-5e81-4630-b8d5-93fee16e0cd3
title: Identificación de la actualización de nivel funcional
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 8aee6a5560edfae656582b3c2812ca69c6b90f57
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59812046"
---
# <a name="identifying-your-functional-level-upgrade"></a>Identificación de la actualización de nivel funcional

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Antes de poder elevar los niveles funcionales de dominios y bosques, deberá evaluar el entorno actual e identificar los requisitos de nivel funcional que mejor satisfaga las necesidades de su organización. Evaluar el entorno actual mediante la identificación de los dominios del bosque, los controladores de dominio que se encuentran en cada dominio, el sistema operativo y service packs que se ejecuta cada controlador de dominio y la fecha en que se va a actualizar el dominio controladores. Si va a retirar un controlador de dominio, asegúrese de que comprende las consecuencias que si lo hace, tendrá en su entorno.  
  
Las siguientes circunstancias es posible que impiden la actualización de una versión anterior del sistema operativo Windows Server en el nivel funcional de Windows Server 2008 o Windows Server 2008 R2:  
  
-   Hardware insuficiente  
  
-   Un controlador de dominio que ejecuta un programa antivirus que no es compatible con Windows Server 2008 o Windows Server 2008 R2   
  
-   Uso de un programa específico de la versión que no se ejecuta en Windows Server 2008 o Windows Server 2008 R2   
  
-   La necesidad de actualizar un programa con el service pack más reciente  
  
Documentar esta información puede ayudarle a identificar los pasos a seguir para asegurarse de que tiene un entorno completamente funcional de Windows Server 2008 o Windows Server 2008 R2.  
  
Después de evaluar el entorno actual, es necesario identificar la actualización de nivel funcional que se aplica a su organización. Estas opciones están disponibles:  
  
-   Entorno de modo nativo de Windows 2000 a Windows Server 2008 o Windows Server 2008 R2   
  
-   Bosque de Windows Server 2003 a Windows Server 2008 o Windows Server 2008 R2   
  
-   Nuevo bosque de Windows Server 2008  
  
-   Nuevo bosque de Windows Server 2008 R2  
  
## <a name="upgrading-functional-levels-in-a-native-windows-2000-active-directory-forest"></a>Actualizar los niveles funcionales de un bosque de Active Directory de Windows 2000 nativo  
En un entorno nativo de Windows 2000 que consta únicamente de los controladores de dominio basados en Windows 2000, los niveles funcionales se establecen de forma predeterminada para los siguientes niveles y permanecen en estos niveles hasta generar manualmente:  
  
-   Nivel funcional del dominio nativo de Windows 2000  
  
-   Nivel funcional del bosque de Windows 2000  
  
Para usar todas las características de nivel de bosque y dominio de Windows Server 2008 o Windows Server 2008 R2, tendrá que actualizar este entorno de Windows 2000 a Windows Server 2008 o Windows Server 2008 R2. Puede realizar esta actualización en cualquiera de las maneras siguientes:  
  
-   Introducir recién instalado Windows Server 2008-basa o Windows Server 2008 R2-controladores de dominio basados en el bosque y, a continuación, retirar todos los controladores de dominio que ejecutan Windows 2000.  
  
-   Realizar una actualización en contexto de todos los controladores de dominio existentes ejecutan Windows 2000 en el bosque a controladores de dominio que ejecuta Windows Server 2003. A continuación, realice una actualización en contexto de estos controladores de dominio a Windows Server 2008 o Windows Server 2008 R2. Para obtener más información, consulte [actualizar dominios de Active Directory a dominios de AD DS de Windows Server 2008 \[LH\]](assetId:///9c91be5f-df14-40b2-b176-2b1852a51e61).  
  
    > [!IMPORTANT]  
    >  Windows Server 2008 R2 es un sistema operativo basado en x64 64. Si el servidor se está ejecutando una versión basado en x64 64 de Windows Server 2003, puede realizar correctamente una actualización en contexto de este sistema operativo a Windows Server 2008 R2. Si el servidor se está ejecutando una versión x86 de Windows Server 2003, no puede actualizar este equipo a Windows Server 2008 R2.  
  
Para usar las características de nivel de dominio de Windows Server 2008 o Windows Server 2008 R2 sin actualizar todo el bosque de Windows 2000 a Windows Server 2008 o Windows Server 2008 R2, generar solo el nivel funcional del dominio a Windows Server 2008 o Windows Server 2008 R2.  
  
> [!NOTE]  
> Antes de elevar el nivel funcional del dominio, debe actualizar todos los controladores de dominio basados en Windows 2000 en ese dominio a Windows Server 2008 o Windows Server 2008 R2.  
  
Después de reemplazar todos los controladores de dominio basados en Windows 2000 en el bosque con controladores de dominio que ejecutan Windows Server 2008 o Windows Server 2008 R2, puede elevar el nivel funcional del bosque a Windows Server 2008 o Windows Server 2008 R2. Al hacerlo, automáticamente eleva el nivel funcional de todos los dominios del bosque que se establecen en Windows 2000 nativo o posterior para Windows Server 2008 o Windows Server 2008 R2.  
  
Para obtener más información acerca de cómo elevar los niveles funcionales de bosque y dominio y los procedimientos para realizar esas tareas, consulte [implementar un dominio raíz del bosque de Windows Server 2008 \[LH\]](assetId:///92406e8d-dc1c-4740-a00a-2c4032896dd1).  
  
## <a name="upgrading-functional-levels-in-a-windows-server-2003-active-directory-forest"></a>Actualizar los niveles funcionales de un bosque de Active Directory de Windows Server 2003  
En un entorno de Windows Server 2003 que consta de sólo los controladores de dominio basados en Windows Server 2003, los niveles funcionales se establecen de forma predeterminada para los siguientes niveles y permanecen en estos niveles hasta generar manualmente:  
  
-   Nivel funcional del dominio nativo de Windows 2000  
  
-   Nivel funcional del bosque de Windows 2000  
  
Para usar todas las características de nivel de bosque y dominio de Windows Server 2008 o Windows Server 2008 R2, tendrá que actualizar este entorno de Windows Server 2003 a Windows Server 2008 o Windows Server 2008 R2. Puede realizar esta actualización en cualquiera de las maneras siguientes:  
  
-   Introducir un recién instalado Windows Server 2008-basándose o Windows Server 2008 R2: controlador de dominio basado en el bosque y, a continuación, retirar todos los controladores de dominio que ejecuta Windows Server 2003 o actualizarlos a Windows Server 2008 o Windows Server 2008 R2.  
  
-   Realizar una actualización en contexto de todos los controladores de dominio existentes ejecutan Windows Server 2003 a controladores de dominio que ejecutan Windows Server 2008 o Windows Server 2008 R2. Para obtener más información, consulte [actualizar dominios de Active Directory a dominios de AD DS de Windows Server 2008 \[LH\]](assetId:///9c91be5f-df14-40b2-b176-2b1852a51e61).  
  
> [!IMPORTANT]  
>  Windows Server 2008 R2 es un sistema operativo basado en x64 64. Si el servidor se está ejecutando una versión basado en x64 64 de Windows Server 2003, puede realizar correctamente una actualización en contexto de este sistema operativo a Windows Server 2008 R2. Si el servidor se está ejecutando una versión x86 de Windows Server 2003, no puede actualizar este equipo para ejecutar Windows Server 2008 R2.  
  
Para usar las características de nivel de dominio de la de Windows Server 2008 o Windows Server 2008 R2 sin actualizar todo el bosque de Windows Server 2003 a Windows Server 2008 o Windows Server 2008 R2, generar solo el nivel funcional de dominio a Windows Server 2008 o Windows S erver 2008 R2.  
  
> [!NOTE]  
> Antes de elevar el nivel funcional del dominio, debe actualizar todos los controladores de dominio basados en Windows Server 2003 en ese dominio a Windows Server 2008 o Windows Server 2008 R2.  
  
Después de actualizar todos los controladores de dominio basados en Windows Server 2003 en el bosque a Windows Server 2008 o Windows Server 2008 R2, puede elevar el nivel funcional del bosque a Windows Server 2008 o Windows Server 2008 R2. Al hacerlo, automáticamente eleva el nivel funcional de todos los dominios del bosque que se establecen en Windows Server 2003 a Windows Server 2008 o Windows Server 2008 R2.  
  
Para obtener más información acerca de cómo elevar los niveles funcionales de bosque y dominio y los procedimientos para realizar esas tareas, consulte [implementar un dominio raíz del bosque de Windows Server 2008 \[LH\]](assetId:///92406e8d-dc1c-4740-a00a-2c4032896dd1).  
  
## <a name="upgrading-functional-levels-in-a-new-windows-server-2008-forest"></a>Actualizar los niveles funcionales de un nuevo bosque de Windows Server 2008  
Cuando se instala el primer controlador de dominio en un nuevo bosque de Windows Server 2008, los niveles funcionales se establecen de forma predeterminada para los siguientes niveles y permanecen en estos niveles hasta generar manualmente:  
  
-   Nivel funcional del dominio nativo de Windows 2000  
  
-   Nivel funcional del bosque de Windows 2000  
  
Los niveles funcionales se establecen en estos niveles de forma predeterminada para ofrecerle la opción de agregar controladores de dominio basado en Windows Server 2003 o Windows 2000 para el nuevo bosque de Windows Server 2008. Después de crear un dominio raíz del bosque, el nivel funcional del dominio para cada dominio que agregue al bosque de Windows Server 2008 se establece en Windows 2000 nativo. Sin embargo, si desea que todos los controladores de dominio en el nuevo entorno de Windows Server 2008 para ejecutar Windows Server 2008, establezca el nivel funcional del bosque y, a continuación, en el nivel funcional de dominio a Windows Server 2008 cuando se instala el primer controlador de dominio en sus fores t. Esto ahorra tiempo y habilita todas las características de nivel de bosque y dominio en Windows Server 2008.  
  
> [!IMPORTANT]  
> Si el bosque funciona en Windows Server 2008 funcional nivel e intenta instalar Active Directory en un servidor miembro basado en Windows Server 2003 o se produce un error en un servidor miembro basado en Windows 2000, la instalación.  
  
Para obtener más información acerca de cómo elevar los niveles funcionales de bosque y dominio y los procedimientos para realizar esas tareas, consulte [implementar un dominio raíz del bosque de Windows Server 2008 \[LH\]](assetId:///92406e8d-dc1c-4740-a00a-2c4032896dd1).  
  
## <a name="upgrading-functional-levels-in-a-new-windows-server-2008-r2-forest"></a>Actualizar los niveles funcionales de un nuevo bosque de Windows Server 2008 R2  
Cuando se instala el primer controlador de dominio en un nuevo bosque de Windows Server 2008 R2, los niveles funcionales se establecen de forma predeterminada para los siguientes niveles y permanecen en estos niveles hasta generar manualmente:  
  
-   Nivel funcional del dominio de Windows Server 2003  
  
-   Nivel funcional del bosque de Windows Server 2003  
  
Los niveles funcionales se establecen en estos niveles de forma predeterminada para ofrecerle la opción de agregar controladores de dominio basados en Windows Server 2003 para el nuevo bosque de Windows Server 2008 R2. Después de crear un dominio raíz del bosque, se establece el nivel funcional del dominio para cada dominio que agregue al bosque de Windows Server 2008 R2 a Windows Server 2003. Sin embargo, si desea que todos los controladores de dominio en el nuevo entorno de Windows Server 2008 R2 para ejecutar Windows Server 2008 R2, establezca el nivel funcional del bosque y, a continuación, en el nivel funcional de dominio a Windows Server 2008 R2 al instalar el primer controlador de dominio en yo el bosque. Esto ahorra tiempo y habilita todas las características de nivel de bosque y dominio en Windows Server 2008 R2.  
  
> [!IMPORTANT]  
> Si el bosque funciona en el nivel funcional de Windows Server 2008 R2 e intenta instalar Active Directory en Windows Server 2008-servidor basado en Windows Server 2003 o en función de miembro, o en un servidor miembro basado en Windows 2000, la instalación produce errores.  
  
Para obtener más información acerca de cómo elevar los niveles funcionales de bosque y dominio y los procedimientos para realizar esas tareas, consulte [implementar un dominio raíz del bosque de Windows Server 2008 \[LH\]](assetId:///92406e8d-dc1c-4740-a00a-2c4032896dd1).  
  
> [!NOTE]  
> Si bien de ADMT v3.1 debe instalarse en Windows Server 2008, puede usar ADMT v3.1 para migrar objetos a un dominio que está hospedado en uno o más controladores de dominio de Windows Server 2008 R2. Para obtener más información, consulte [artículo 976659](https://go.microsoft.com/fwlink/?LinkId=180398) en Microsoft Knowledge Base (https://go.microsoft.com/fwlink/?LinkId=180398).  
  


