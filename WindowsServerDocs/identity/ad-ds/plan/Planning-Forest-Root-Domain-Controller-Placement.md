---
ms.assetid: 2a2f493a-9796-454a-9721-e223b799dfa7
title: Planear la ubicación del controlador de dominio de raíz del bosque
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 4bbdbfe294695e068c2ec76a135526febb4b6485
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80822149"
---
# <a name="planning-forest-root-domain-controller-placement"></a>Planear la ubicación del controlador de dominio de raíz del bosque

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Los controladores de dominio raíz del bosque son necesarios para crear rutas de acceso de confianza para los clientes que necesitan acceder a recursos de dominios distintos de los suyos propios. Coloque los controladores de dominio raíz del bosque en ubicaciones de concentrador y en ubicaciones que hospedan centros de recursos. Si los usuarios de una ubicación determinada necesitan tener acceso a recursos de otros dominios de la misma ubicación y la disponibilidad de la red entre el centro de usuarios y la ubicación del usuario no es confiable, puede Agregar un controlador de dominio raíz del bosque en la ubicación o crear una confianza directa entre los dos dominios. Es más rentable crear una confianza directa entre los dominios, a menos que tenga otras razones para colocar un controlador de dominio raíz del bosque en esa ubicación.  
  
Las confianzas directas ayudan a optimizar las solicitudes de autenticación realizadas desde los usuarios ubicados en cualquiera de los dominios. Para obtener más información acerca de las confianzas directas entre dominios, consulte el artículo sobre [Cuándo crear una confianza directa](https://go.microsoft.com/fwlink/?LinkId=107061).  
  
Para obtener una hoja de cálculo que le ayude a documentar la ubicación del controlador de dominio raíz del bosque, vea el tema [sobre ayudas de trabajo para el kit de implementación de Windows Server 2003](https://go.microsoft.com/fwlink/?LinkID=102558), descargar Job_Aids_Designing_and_Deploying_Directory_and_Security_Services. zip y abrir "Ubicación del controlador de dominio" (DSSTOPO_4. doc).  
  
Tendrá que consultar esta información al crear el dominio raíz del bosque. Para obtener más información acerca de la implementación del dominio raíz del bosque, vea [implementar un dominio raíz del bosque de Windows Server 2008](https://technet.microsoft.com/library/cc731174.aspx).  
