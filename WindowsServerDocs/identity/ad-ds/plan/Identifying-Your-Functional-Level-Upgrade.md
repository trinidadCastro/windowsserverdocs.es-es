---
ms.assetid: 231158d8-5e81-4630-b8d5-93fee16e0cd3
title: "Identificación de la actualización de nivel funcional"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 9527a186cb20c470e0b5644fff58f90786520f97
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="identifying-your-functional-level-upgrade"></a>Identificación de la actualización de nivel funcional

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Antes de poder elevar niveles funcionales de bosque y dominio, tienes que evalúes tu entorno actual e identificar el requisito de nivel funcional que mejor satisfaga las necesidades de la organización. Evaluar el entorno actual, así como identificar los dominios en el bosque, los controladores de dominio que se encuentran en cada dominio, el sistema operativo y los service packs que se ejecuta cada controlador de dominio y la fecha en que vas a actualizar los controladores de dominio. Si vas a retirar un controlador de dominio, debes asegurarte de que comprender el impacto completo que esto tiene en su entorno.  
  
Las siguientes circunstancias pueden impedir la actualización de una versión anterior del sistema operativo Windows Server en el nivel funcional de Windows Server 2008 o Windows Server 2008 R2:  
  
-   Hardware insuficiente  
  
-   Un controlador de dominio que se ejecuta un programa antivirus que no es compatible con Windows Server 2008 o Windows Server 2008 R2   
  
-   Uso de un programa específico de la versión que no se ejecuta en Windows Server 2008 o Windows Server 2008 R2   
  
-   La necesidad de actualizar un programa con el último service pack  
  
Documentar esta información puede ayudarte a identificar los pasos para asegurarse de que tiene un entorno de Windows Server 2008 o Windows Server 2008 R2 completamente funcional.  
  
Después de evaluar el entorno actual, tienes que identificar la actualización de nivel funcional que se aplica a la organización. Estas opciones están disponibles:  
  
-   Entorno de Windows 2000 en modo nativo para Windows Server 2008 o Windows Server 2008 R2   
  
-   Bosque de Windows Server 2003, Windows Server 2008 o Windows Server 2008 R2   
  
-   Bosque nuevo de Windows Server 2008  
  
-   Bosque nuevo de Windows Server 2008 R2  
  
## <a name="upgrading-functional-levels-in-a-native-windows-2000-active-directory-forest"></a>Actualizar niveles funcionales en un bosque de Active Directory de Windows 2000 nativo  
En un entorno de Windows 2000 nativo que sólo consiste en controladores de dominio basados en Windows 2000, se establecen los niveles funcionales de forma predeterminada para los siguientes niveles y permanecerán en estos niveles hasta generar manualmente:  
  
-   Nivel funcional del dominio nativa de Windows 2000  
  
-   Nivel funcional del bosque de Windows 2000  
  
Para usar todas las características de nivel de bosque y dominio de Windows Server 2008 o Windows Server 2008 R2, tienes que actualizar este entorno de Windows 2000 a Windows Server 2008 o Windows Server 2008 R2. Puedes realizar la actualización en cualquiera de las siguientes maneras:  
  
-   Introducir acaba de instalar Windows Server 2008-según o Windows Server 2008 R2-controladores de dominio basados en el bosque y, a continuación, retirar todos los controladores de dominio que ejecutan Windows 2000.  
  
-   Realizar una actualización en contexto de todos los controladores de dominio existentes que ejecutan Windows 2000 en el bosque en controladores de dominio que ejecutan Windows Server 2003. A continuación, realiza una actualización en contexto de los controladores de dominio a Windows Server 2008 o Windows Server 2008 R2. Para obtener más información, consulta [actualizar dominios de Microsoft Active Directory para Windows Server 2008 AD DS dominios \[LH\]](assetId:///9c91be5f-df14-40b2-b176-2b1852a51e61).  
  
    > [!IMPORTANT]  
    >  Windows Server 2008 R2 es un sistema operativo x64 64. Si el servidor está ejecutando una versión x64 64 de Windows Server 2003, puede realizar correctamente una actualización en contexto de este sistema operativo a Windows Server 2008 R2. Si el servidor ejecuta una versión basada en x86 de Windows Server 2003, no se puede actualizar este equipo a Windows Server 2008 R2.  
  
Para usar las características de nivel de dominio de Windows Server 2008 o Windows Server 2008 R2 sin actualizar todo el bosque de Windows 2000 a Windows Server 2008 o Windows Server 2008 R2, generar solo el nivel funcional de dominio a Windows Server 2008 o Windows Server 2008 R2.  
  
> [!NOTE]  
> Antes de generar el nivel funcional del dominio, debes actualizar todos los controladores de dominio basados en Windows 2000 en ese dominio a Windows Server 2008 o Windows Server 2008 R2.  
  
Después de reemplazar todos los controladores de dominio basados en Windows 2000 en el bosque con controladores de dominio que ejecutan Windows Server 2008 o Windows Server 2008 R2, puedes generar el nivel funcional del bosque a Windows Server 2008 o Windows Server 2008 R2. Al hacerlo, automáticamente, genera el nivel funcional de todos los dominios del bosque que están configurados para Windows 2000, nativo o posterior para Windows Server 2008 o Windows Server 2008 R2.  
  
Para obtener más información acerca de cómo elevar niveles funcionales de bosque y dominio y para conocer los procedimientos realizar esas tareas, consulta [implementar un Windows Server 2008 bosque raíz dominio \[LH\]](assetId:///92406e8d-dc1c-4740-a00a-2c4032896dd1).  
  
## <a name="upgrading-functional-levels-in-a-windows-server-2003-active-directory-forest"></a>Actualizar niveles funcionales en un bosque de Active Directory de Windows Server 2003  
En un entorno de Windows Server 2003 que consta de sólo los controladores de dominio basados en Windows Server 2003, los niveles funcionales se establecen de forma predeterminada para los siguientes niveles y permanecerán en estos niveles hasta generar manualmente:  
  
-   Nivel funcional del dominio nativa de Windows 2000  
  
-   Nivel funcional del bosque de Windows 2000  
  
Para usar todas las características de nivel de bosque y dominio de Windows Server 2008 o Windows Server 2008 R2, tienes que actualizar este entorno de Windows Server 2003 a Windows Server 2008 o Windows Server 2008 R2. Puedes realizar la actualización en cualquiera de las siguientes maneras:  
  
-   Introducir un Windows Server 2008 recién instalado-según o Windows Server 2008 R2-controlador de dominio basado en el bosque y, a continuación, retirar todos los controladores de dominio que ejecutan Windows Server 2003 o actualización a Windows Server 2008 o Windows Server 2008 R2.  
  
-   Realizar una actualización en contexto de todos los controladores de dominio existentes que ejecutan Windows Server 2003 en controladores de dominio que ejecutan Windows Server 2008 o Windows Server 2008 R2. Para obtener más información, consulta [actualizar dominios de Microsoft Active Directory para Windows Server 2008 AD DS dominios \[LH\]](assetId:///9c91be5f-df14-40b2-b176-2b1852a51e61).  
  
> [!IMPORTANT]  
>  Windows Server 2008 R2 es un sistema operativo x64 64. Si el servidor está ejecutando una versión x64 64 de Windows Server 2003, puede realizar correctamente una actualización en contexto de este sistema operativo a Windows Server 2008 R2. Si el servidor ejecuta una versión basada en x86 de Windows Server 2003, no se puede actualizar este equipo para que ejecute Windows Server 2008 R2.  
  
Para usar características de nivel de dominio de la de Windows Server 2008 o Windows Server 2008 R2 sin actualizar todo el bosque de Windows Server 2003, Windows Server 2008 o Windows Server 2008 R2, generar solo el nivel funcional de dominio a Windows Server 2008 o Windows Server 2008 R2.  
  
> [!NOTE]  
> Antes de generar el nivel funcional del dominio, debes actualizar todos los controladores de dominio basados en Windows Server 2003 en ese dominio a Windows Server 2008 o Windows Server 2008 R2.  
  
Después de actualizar todos los controladores de dominio basados en Windows Server 2003 en el bosque a Windows Server 2008 o Windows Server 2008 R2, puedes generar el nivel funcional del bosque a Windows Server 2008 o Windows Server 2008 R2. Al hacerlo, automáticamente, genera el nivel funcional de todos los dominios del bosque que están configurados para Windows Server 2003, Windows Server 2008 o Windows Server 2008 R2.  
  
Para obtener más información acerca de cómo elevar niveles funcionales de bosque y dominio y para conocer los procedimientos realizar esas tareas, consulta [implementar un Windows Server 2008 bosque raíz dominio \[LH\]](assetId:///92406e8d-dc1c-4740-a00a-2c4032896dd1).  
  
## <a name="upgrading-functional-levels-in-a-new-windows-server-2008-forest"></a>Actualizar niveles funcionales en un bosque nuevo de Windows Server 2008  
Cuando se instala el primer controlador de dominio en un bosque nuevo de Windows Server 2008, niveles funcionales se establecen de forma predeterminada para los siguientes niveles, y permanecen con estos niveles hasta que generar manualmente:  
  
-   Nivel funcional del dominio nativa de Windows 2000  
  
-   Nivel funcional del bosque de Windows 2000  
  
Niveles funcionales se establecen con estos niveles de manera predeterminada para dar la opción de agregar controladores de dominio basados en Windows Server 2003 o Windows 2000 en un bosque nuevo de Windows Server 2008. Después de crear un dominio raíz del bosque, el nivel funcional de dominio para cada dominio que agregas al bosque de Windows Server 2008 se establece en Windows 2000 nativo. Sin embargo, si quieres que todos los controladores de dominio en el entorno de Windows Server 2008 nuevo para ejecutar Windows Server 2008, Establece el nivel funcional del bosque y, a continuación, en el nivel funcional de dominio a Windows Server 2008, cuando instalas el primer controlador de dominio en el bosque. Esto ahorra tiempo y habilita todas las características de nivel de bosque y dominio en Windows Server 2008.  
  
> [!IMPORTANT]  
> Si el bosque funciona en Windows Server 2008 funcional nivel e intenta instalar de Active Directory en un servidor miembro basado en Windows Server 2003 o se produce un error en un servidor miembro basado en Windows 2000, la instalación.  
  
Para obtener más información acerca de cómo elevar niveles funcionales de bosque y dominio y para conocer los procedimientos realizar esas tareas, consulta [implementar un Windows Server 2008 bosque raíz dominio \[LH\]](assetId:///92406e8d-dc1c-4740-a00a-2c4032896dd1).  
  
## <a name="upgrading-functional-levels-in-a-new-windows-server-2008-r2-forest"></a>Actualizar niveles funcionales en un bosque nuevo de Windows Server 2008 R2  
Cuando se instala el primer controlador de dominio en un bosque nuevo de Windows Server 2008 R2, niveles funcionales se establecen de forma predeterminada para los siguientes niveles, y permanecen con estos niveles hasta que generar manualmente:  
  
-   Nivel funcional del dominio de Windows Server 2003  
  
-   Nivel funcional del bosque de Windows Server 2003  
  
Niveles funcionales se establecen con estos niveles de manera predeterminada para dar la opción de agregar controladores de dominio basados en Windows Server 2003 en un bosque nuevo de Windows Server 2008 R2. Después de crear un dominio raíz del bosque, se establece el nivel funcional de dominio para cada dominio que agregas al bosque de Windows Server 2008 R2 en Windows Server 2003. Sin embargo, si quieres que todos los controladores de dominio en el nuevo entorno de Windows Server 2008 R2 para ejecutar Windows Server 2008 R2, Establece el nivel funcional del bosque y, a continuación, en el nivel funcional de dominio, en Windows Server 2008 R2 cuando instalas el primer controlador de dominio en el bosque. Esto ahorra tiempo y permite que todas las características de nivel de bosque y dominio de Windows Server 2008 R2.  
  
> [!IMPORTANT]  
> Si el bosque funciona en el nivel funcional de Windows Server 2008 R2 e intenta instalar de Active Directory en Windows Server 2008-servidor miembro basado en Windows Server 2003 o en función de, o en un servidor miembro basado en Windows 2000, error durante la instalación.  
  
Para obtener más información acerca de cómo elevar niveles funcionales de bosque y dominio y para conocer los procedimientos realizar esas tareas, consulta [implementar un Windows Server 2008 bosque raíz dominio \[LH\]](assetId:///92406e8d-dc1c-4740-a00a-2c4032896dd1).  
  
> [!NOTE]  
> Aunque ADMT v3.1 debe estar instalado en Windows Server 2008, puedes usar ADMT v3.1 para migrar los objetos a un dominio que está hospedado por uno o varios controladores de dominio de Windows Server 2008 R2. Para obtener más información, consulta [artículo 976659](https://go.microsoft.com/fwlink/?LinkId=180398) en Microsoft Knowledge Base (https://go.microsoft.com/fwlink/?LinkId=180398).  
  


