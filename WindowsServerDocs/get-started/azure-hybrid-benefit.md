---
title: Ventaja híbrida de Azure para Windows Server
description: Usar las licencias locales de Windows Server para ahorrar en las máquinas virtuales de Azure
ms.date: 11/10/2017
ms.topic: article
author: eross-msft
ms.author: chrisrin
ms.localizationpriority: high
ms.openlocfilehash: a997f60d4b36d85a895dedd567560be4edcd4d98
ms.sourcegitcommit: fa325ef993e62fba59de803d8998922db1cb1dc8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/02/2020
ms.locfileid: "96482586"
---
# <a name="azure-hybrid-benefit-for-windows-server"></a>Ventaja híbrida de Azure para Windows Server

> Se aplica a: Windows Server 2019, Windows Server 2016 R2, Windows Server 2012

## <a name="benefit-description-rules-and-use-cases"></a>Descripción de la ventaja, las reglas y los casos de uso

La Ventaja híbrida de Azure para Windows Server te permite ahorrar hasta un 40 % en VM de Windows Server en Azure, mediante el uso tus licencias locales de Windows Server con Software Assurance.  Con esta ventaja, los clientes solo tienen que pagar los costos de infraestructura de la máquina virtual, porque la licencia de Windows Server viene cubierta por la ventaja de Software Assurance.  La ventaja es aplicable a las ediciones Standard y Datacenter de Windows Server para las versiones de Windows Server 2012 R2, Windows Server 2016 y Windows Server 2019. Esta ventaja está disponible en todas las regiones y nubes soberanas.

![imagen 1](media/ahb01.png)

Todo lo que necesitas para obtener la ventaja es una licencia de Software Assurance o de suscripción, como una suscripción EAS, SCE o una suscripción de Open Value en tus licencias de Windows Server.

Cada licencia de 2 procesadores de Windows Server con Microsoft Software Assurance/suscripción activa y cada conjunto de 16 licencias de núcleo de Windows Server con Microsoft Software Assurance/suscripción permite al cliente usar Windows Server en Microsoft Azure en hasta 16 núcleos virtuales asignados entre dos o menos instancias de Azure Base (máquinas virtuales). Cada conjunto adicional de 8 licencias de núcleos con Microsoft Software Assurance/suscripción da derecho a usar hasta 8 núcleos virtuales y una instancia de base (VM).

| Licencia con Microsoft Software Assurance/suscripción            | VM y núcleos concedidos            | Cómo se pueden usar                                |
|-----------------------------------------|----------------------------------|-----------------------------------------------------|
| Centro de datos de WS (16 núcleos o una L de 2 procesadores)  | Hasta dos VM y hasta 16 núcleos | Ejecutar máquinas virtuales en locales y en Azure  |
| WS estándar (16 núcleos o una L de 2 procesadores)    | Hasta dos VM y hasta 16 núcleos | Ejecutar máquinas virtuales en locales o en Azure |

Las VM que usan Ventaja híbrida de Azure pueden ejecutarse en Azure solo durante el periodo de Microsoft Software Assurance/suscripción. Cuando se acerca el momento de vencimiento de Microsoft Software Assurance/suscripción, el cliente tiene una opción para renovar su Microsoft Software Assurance/suscripción, desactivar la funcionalidad de las ventajas híbridas para dicha VM o desaprovisionar la máquina virtual con la ventaja híbrida.

### <a name="savings-examples"></a>Ejemplos de ahorro

![imagen 2](media/ahb02.png)

A continuación encontrarás una tabla de referencia para ayudarte a entender las reglas de las ventajas con más detalle.
La columna verde muestra la cantidad de VM del mismo tipo y la fila azul la densidad de núcleos de cada VM. Las celdas amarillas muestran el número de licencias de 2 procesadores (o conjuntos de 16 núcleos) que uno debe tener para implementar un determinado número de VM de una densidad de núcleos determinada.

Tabla de referencia de Windows Server con requisitos de Microsoft Software Assurance:

![imagen 3](media/ahb03.png)

La Ventaja híbrida de Azure para Windows Server también ofrece flexibilidad para ejecutar configuraciones según tus necesidades, así como combinar VM de diferentes tipos.

Configuraciones de ejemplo para distintas posiciones de licencias:

![imagen 4](media/ahb04.png)
![imagen 5](media/ahb05.png)


Si quieres obtener más información sobre la Ventaja híbrida de Azure para Windows Server, ve al sitio web de Ventaja híbrida de Azure.

## <a name="how-to-maintain-compliance"></a>Cómo mantener el cumplimiento

Los clientes que buscan aplicar Ventaja híbrida de Azure a sus VM de Windows Server deben verificar el número de licencias elegibles y el período de cobertura respectivo de su Microsoft Software Assurance/suscripción antes de activar esta ventaja y aplicar las directrices anteriores para implementar el número correcto de VM con la ventaja.
Si ya tienes VM que se ejecutan con Ventaja híbrida de Azure, deberás realizar un inventario del número de unidades que estás ejecutando y comprobar frente a las licencias activas de Microsoft Software Assurance que tengas.  Ponte en contacto con tu especialista en licencias de contrato de empresas de Microsoft para validar tu posición de licencias de Microsoft Software Assurance.
Para ver y contar todas las máquinas virtuales implementadas con Ventaja híbrida de Azure para Windows Server en una suscripción, puedes realizar una de las siguientes acciones:

1. Configura el Portal de Microsoft Azure para mostrar la Ventaja híbrida de Azure para Windows Server, agregando la columna “Ventaja híbrida de Azure” en la vista de lista de la sección de máquinas virtuales del Portal de Azure de Microsoft.

    ![imagen 6](media/ahb06.png)

2.  Usar PowerShell para listar el uso de Ventaja híbrida de Azure para Windows Server

    ```
    $vms = Get-AzureRMVM
    foreach ($vm in $vms) {"VM Name: " + $vm.Name, "   Azure Hybrid Benefit for Windows Server: "+ $vm.LicenseType}
    ```

3.  Mira tu factura de Microsoft Azure para determinar cuántas máquinas virtuales estás ejecutando con Ventaja híbrida de Azure para Windows Server. La información sobre el número de instancias con la ventaja aparece en "Información adicional":

    ```
    "{"ImageType":"WindowsServerBYOL","ServiceType":"Standard_A1","VMName":"","UsageType":"ComputeHR"}"
    ```

Ten en cuenta que la facturación no se aplica en tiempo real; es decir, pasarán algunas horas desde que actives una VM con la ventaja híbrida hasta que aparezca en la factura.
A continuación, puedes rellenar los resultados en la **Herramienta de recuento de Microsoft Software Assurance de Ventaja híbrida de Azure para Windows Server** que viene a continuación para obtener el número de licencias de WS cubiertas con Microsoft Software Assurance o suscripciones que son necesarias.

Asegúrate de realizar un inventario en cada suscripción que poseas para generar una vista completa de la posición de la licencia.

[Herramienta de recuento de Microsoft Software Assurance de Ventaja híbrida de Azure WS](https://download.microsoft.com/download/7/1/2/712FEFF0-155C-4ABF-96C0-CE4EC4DB0516/Azure_Hybrid_Benefit_Windows_Server_SA_Count_Tool.xlsx)

Si has realizado lo anterior y confirmado que tienes todas las licencias necesarias para el número de instancias de Ventaja híbrida de Azure que estás ejecutando, no es necesaria ninguna otra acción. Si descubres que puedes cubrir VM incrementales con la ventaja, puede que te interese optimizar aun más los costes cambiando a ejecutar instancias con la ventaja frente al coste sin reducción.

Si no tienes suficientes licencias elegibles de Windows Server para el número de VM ya implementadas, necesitarás comprar licencias locales de Windows Server cubiertas con Software Assurance a través de uno de los canales enumerados a continuación, comprar VM de Windows Server con las tarifas horarias nominales o desactivar la funcionalidad Ventaja híbrida para algunas VM. Ten en cuenta que puedes adquirir licencias de núcleos en incrementos de 8 núcleos, para poder optar a cada VM de Ventaja híbrida de Azure.

Software Assurance y/o suscripciones de Windows Server están disponibles para su compra a través de una combinación de los siguientes canales de licencia de Microsoft:

| Canal                      | Abrir     | OVS      | Seleccionar/Seleccionar Plus  | MPSA       | EA/EAS   |
|------------------------------|----------|----------|-----------------------|-----------|----------|
| Tamaño habitual (n.º de dispositivos)  | 5-250    | 5-250    | >250                  | >250      | >500     |
| Microsoft Software Assurance/Suscripción            | Opcional | Incluido | Opcional              | Opcional  | Incluido |

Microsoft se reserva el derecho a auditar al cliente final en cualquier momento, para comprobar si es elegible para el uso de Ventaja híbrida de Azure.

## <a name="deployment-guidance"></a>Guía de implementación

Hemos habilitado la disponibilidad de imágenes de galería pregeneradas para todos nuestros clientes con licencias elegibles, independientemente de dónde las hayan comprado, y también hemos habilitado a los asociados para que puedan realizar implementaciones en nombre de los clientes.

Encontrarás las instrucciones para todas las opciones de implementación disponibles [aquí](https://azure.microsoft.com/pricing/hybrid-use-benefit/), incluyendo:
-   Vídeo detallado que destaca la nueva experiencia de implementación usando imágenes de galería pregeneradas
-   Instrucciones detalladas sobre cómo cargar una VM diseñada por el cliente
-   Instrucciones detalladas sobre la migración de máquinas virtuales existentes mediante la recuperación del sitio de Azure con PowerShell.
