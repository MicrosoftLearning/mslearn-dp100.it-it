---
title: Istruzioni online
permalink: index.html
layout: home
ms.openlocfilehash: a2eb157b1d188655f4cfbcc575befec4a2e9c623
ms.sourcegitcommit: 18f734eeb1031a9cb69c3b294632efd2e69324ac
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/17/2021
ms.locfileid: "137894579"
---
# <a name="azure-machine-learning-exercises"></a>Esercizi su Azure Machine Learning

Questo repository contiene le esercitazioni pratiche per il corso Microsoft [DP-100 *Designing and Implementing a Data Science Solution on Azure*](https://docs.microsoft.com/learn/certifications/courses/dp-100t01) e i relativi [moduli autogestiti su Microsoft Learn](https://docs.microsoft.com/learn/paths/build-ai-solutions-with-azure-ml-service/). Gli esercizi si integrano con i materiali di formazione e permettono di fare pratica con le tecnologie descritte.

Per completare gli esercizi, sarà necessaria una sottoscrizione di Microsoft Azure. Se non è stata fornita dal docente, è possibile iscriversi a una versione di valutazione gratuita in [https://azure.microsoft.com](https://azure.microsoft.com).

{% assign labs = site.pages | where_exp:"page", "page.url contains '/instructions'" %}
| Esercizi |
| ------- | 
{% for activity in labs  %}| [{{ activity.lab.title }}]({{ site.github.url }}{{ activity.url }}) |
{% endfor %}
