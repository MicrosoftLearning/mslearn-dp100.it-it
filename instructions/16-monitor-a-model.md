---
lab:
  title: Monitorare un modello
ms.openlocfilehash: d00816159cd605ad7aad2a275471db18411bce4a
ms.sourcegitcommit: 18f734eeb1031a9cb69c3b294632efd2e69324ac
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/17/2021
ms.locfileid: "132832629"
---
# <a name="monitor-a-model"></a>Monitorare un modello

Quando si distribuisce un modello come servizio, è utile poter tenere traccia delle informazioni sulle richieste che elabora.

Per riuscire a registrare i dati dai servizi distribuiti, è possibile integrare Azure Machine Learning con Azure Application Insights.

## <a name="before-you-start"></a>Prima di iniziare

Se non è già stato fatto, completare l'esercizio *[Creare un'area di lavoro Azure Machine Learning](01-create-a-workspace.md)* per creare un'area di lavoro di Azure Machine Learning e un'istanza di calcolo e clonare i notebook necessari per questo esercizio.

## <a name="open-jupyter"></a>Aprire Jupyter

Anche se è possibile usare la pagina **Notebook** in Azure Machine Learning Studio per eseguire i notebook, è spesso più produttivo usare un ambiente di sviluppo di notebook più completo come *Jupyter*.

1. In [Azure Machine Learning Studio](https://ml.azure.com) visualizzare la pagina **Calcolo** per l'area di lavoro e nella scheda **Istanze di ambiente di calcolo** avviare l'istanza di calcolo se non è già in esecuzione.
2. Quando l'istanza dell'ambiente di calcolo è in esecuzione, fare clic sul collegamento **Jupyter** per aprire la home page di Jupyter in una nuova scheda del browser.

## <a name="use-application-insights-to-monitor-a-real-time-service"></a>Usare Application Insights per monitorare un servizio in tempo reale

In questo esercizio il codice per configurare Application Insights per un servizio predittivo distribuito viene fornito in un notebook.

1. Nella home page di Jupyter passare alla cartella **/users/*nome-utente*/mslearn-dp100** in cui è stato clonato il repository del notebook e aprire il notebook **Monitor a Model**.
2. Leggere quindi le note disponibili nel notebook, eseguendo una cella di codice alla volta.
3. Dopo aver finito di eseguire il codice nel notebook, dal menu **File** scegliere **Close and Halt** (Chiudi e interrompi) per chiuderlo e arrestarne il kernel Python. Chiudere quindi tutte le schede del browser Jupyter.

## <a name="clean-up"></a>Eliminazione

Se per il momento non è più necessario usare Azure Machine Learning, nella scheda **Istanze di ambiente di calcolo** della pagina **Calcolo** di Azure Machine Learning, selezionare l'istanza di ambiente di calcolo desiderata e fare clic su **Arresta** per arrestarla. In caso contrario, lasciarlo in esecuzione per il lab successivo.

> **Nota**: l'arresto delle risorse di calcolo garantisce che alla sottoscrizione non vengano addebitati i costi di calcolo corrispondenti. Verrà tuttavia addebitato un importo ridotto per l'archiviazione dei dati, fintanto che l'area di lavoro di Azure Machine Learning è presente nella sottoscrizione. Se è stata completata l'esplorazione di Azure Machine Learning, è possibile eliminare l'area di lavoro di Azure Machine Learning e le risorse associate. Tuttavia, se si prevede di completare altri lab di questa serie, è necessario prima ripetere l'esercizio *[Creare un'area di lavoro di Azure Machine Learning](01-create-a-workspace.md)* per creare l'area di lavoro e preparare l'ambiente.