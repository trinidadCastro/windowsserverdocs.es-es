---
ms.assetid: 7dd905ea-4235-4519-8400-31b4fa0ed1bf
title: "Permitir que los clientes encontrar el controlador de dominio más próximo siguiente"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 39b1b79bba944c10b0c74c4bb18f6dcf80f8230e
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="enabling-clients-to-locate-the-next-closest-domain-controller"></a>Permitir que los clientes encontrar el controlador de dominio más próximo siguiente

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Si tienes un controlador de dominio que ejecute Windows Server 2008 o Windows Server 2008 R2, puede permitir que para los equipos cliente que ejecutan Windows Vista, Windows 7, Windows Server 2008 o Windows Server 2008 R2 para buscar controladores de dominio de forma más eficaz habilitando la **intenta siguiente sitio más cercano** configuración de directiva de grupo. Esta opción mejora el localizador de controlador de dominio (Ubicador de DC) al ayudar a optimizar el tráfico de red, especialmente en las grandes empresas que tienen muchas sucursales y sitios.  
  
Esta nueva configuración puede afectar a la configuración de los costos del vínculo de sitio esto porque afecta el orden en que se encuentran los controladores de dominio. Para las empresas que tienen muchas sucursales y sitios de concentrador, puede reducir significativamente el tráfico de Active Directory en la red asegurándose de que los clientes conmutación al siguiente sitio concentrador más cercano al que no pueden encontrar un controlador de dominio en el sitio de concentrador más cercano.  
  
Como procedimiento recomendado general, deben simplificar la topología del sitio y vínculo del sitio cuesta tanto como sea posible, si habilitas la **intenta siguiente sitio más cercano** configuración. En las empresas con muchos sitios de concentrador, esto puede simplificar los planes que realices para tratar las situaciones en que los clientes en un sitio necesitan por error a un controlador de dominio en otro sitio.  
  
De manera predeterminada, la **intenta siguiente sitio más cercano** no está habilitada. Cuando la configuración no está habilitada, ubicador de DC usa el siguiente algoritmo para encontrar un controlador de dominio:  
  
-   Tratar de encontrar un controlador de dominio en el mismo sitio.  
  
-   Si no hay ningún controlador de dominio está disponible en el mismo sitio, intente buscar cualquier controlador de dominio del dominio.  
  
> [!NOTE]  
> Este es el mismo algoritmo que usa el ubicador de DC en versiones anteriores de Active Directory. Para obtener más información, consulta cómo admitir DNS para funciona Active Directory ([https://go.microsoft.com/fwlink/?LinkId=108587](https://go.microsoft.com/fwlink/?LinkId=108587)).  
  
Si habilitas la **intenta siguiente sitio más cercano** localizador de DC configuración, usa el siguiente algoritmo para encontrar un controlador de dominio:  
  
-   Tratar de encontrar un controlador de dominio en el mismo sitio.  
  
-   Si no hay ningún controlador de dominio está disponible en el mismo sitio, intenta encontrar un controlador de dominio en el siguiente sitio más cercano. Un sitio esté más próxima si tiene un vínculo de sitio menor costo que otro sitio con un vínculo de sitio mayor costo.  
  
-   Si no hay ningún controlador de dominio está disponible en el siguiente sitio más cercano, intente buscar cualquier controlador de dominio del dominio.  
  
De manera predeterminada, ubicador de DC no tiene en cuenta cualquier sitio que contiene un controlador de dominio de solo lectura (RODC) cuando se determina el siguiente sitio más cercano. Además, porque solo Windows Server 2008 y Windows Server 2008 R2 dominio controladores la compatibilidad con la siguiente funcionalidad de sitio más cercana, cuando el cliente recibe una respuesta un controlador de dominio que se ejecuta una versión anterior de Windows Server, el comportamiento de ubicador de DC es el mismo que cuando después los estableces no está habilitado.  
  
Por ejemplo, supongamos que una topología de sitio tiene cuatro sitios con los valores de enlace de sitio en la siguiente ilustración. En este ejemplo, todos los controladores de dominio son los controladores de dominio grabable que ejecuten Windows Server 2008 o Windows Server 2008 R2.  
  
![Permitir que los clientes encontrar el controlador de dominio](media/Enabling-Clients-to-Locate-the-Next-Closest-Domain-Controller/beff4087-fb2a-463b-96ac-d440a9e29b75.gif)  
  
Cuando la **intenta siguiente sitio más cercano** configuración de directiva de grupo está habilitada en este ejemplo, si un equipo cliente que ejecuta Windows Vista, Windows 7, Windows Server 2008 o Windows Server 2008 R2 en sitio_B intenta buscar un controlador de dominio, primero intenta buscar un controlador de dominio en su propio sitio_B. Si ninguno está disponible en sitio_B, intenta buscar un controlador de dominio en sitio_A.  
  
Si no está habilitada, el cliente intenta buscar un controlador de dominio en sitio_A, Site_C o Site_D si no está disponible en sitio_B ningún controlador de dominio.  
  
> [!NOTE]  
> La **intenta siguiente sitio más cercano** opción funciona en coordinación con cobertura de sitios automática. Por ejemplo, si el sitio siguiente más cercano no tiene ningún controlador de dominio, ubicador de DC intenta encontrar el controlador de dominio que realiza la cobertura de sitios automática para ese sitio.  
  
Para aplicar el **intenta siguiente sitio más cercano** configuración, puedes crear un objeto de directiva de grupo (GPO) y vincularla al objeto apropiado para la organización o puedes modificar la directiva de dominio predeterminada que afectan a todos los clientes que ejecutan Windows Vista, Windows 7, Windows Server 2008 o Windows Server 2008 R2 en el dominio. Para obtener más información sobre cómo establecer la **intenta siguiente sitio más cercano** configuración, consulta [habilitar los clientes para encontrar un controlador de dominio del sitio más cercano siguiente](https://technet.microsoft.com/library/cc772592.aspx).  
  


