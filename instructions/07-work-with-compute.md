---
lab:
  title: Usare le risorse di calcolo
ms.openlocfilehash: 7f20c3ac54bd6dd770c9874cf4e9f9f96d5924f0
ms.sourcegitcommit: 18f734eeb1031a9cb69c3b294632efd2e69324ac
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/17/2021
ms.locfileid: "132832677"
---
# <a name="work-with-compute"></a>Usare le risorse di calcolo

Tutto il codice Python viene eseguito nel contesto di un ambiente che determina i pacchetti Python disponibili. Il codice può essere eseguito in un ambiente presente nella workstation locale o in un'altra destinazione di calcolo, ad esempio un cluster se si desidera un'elevata scalabilità.

In questo esercizio verranno esaminati gli *ambienti* e le *destinazioni di calcolo* che è possibile usare per eseguire esperimenti in Azure Machine Learning.

## <a name="before-you-start"></a>Prima di iniziare

Se non è già stato fatto, completare l'esercizio *[Creare un'area di lavoro Azure Machine Learning](01-create-a-workspace.md)* per creare un'area di lavoro di Azure Machine Learning e un'istanza di calcolo e clonare i notebook necessari per questo esercizio.

## <a name="open-jupyter"></a>Aprire Jupyter

Anche se è possibile usare la pagina **Notebook** in Azure Machine Learning Studio per eseguire i notebook, è spesso più produttivo usare un ambiente di sviluppo di notebook più completo come *Jupyter*.

1. In [Azure Machine Learning Studio](https://ml.azure.com) visualizzare la pagina **Calcolo** per l'area di lavoro e nella scheda **Istanze di ambiente di calcolo** avviare l'istanza di calcolo se non è già in esecuzione.
2. Quando l'istanza dell'ambiente di calcolo è in esecuzione, fare clic sul collegamento **Jupyter** per aprire la home page di Jupyter in una nuova scheda del browser.

## <a name="work-with-environments-and-compute-targets"></a>Usare ambienti e destinazioni di calcolo

In questo esercizio il codice da usare nella risorsa di calcolo viene fornito in un notebook.

1. Nella home page di Jupyter passare alla cartella **/users/*nome-utente*/mslearn-dp100** in cui è stato clonato il repository del notebook e aprire il notebook **Work with Compute**.
2. Leggere quindi le note disponibili nel notebook, eseguendo una cella di codice alla volta.
3. Dopo aver finito di eseguire il codice nel notebook, dal menu **File** scegliere **Close and Halt** (Chiudi e interrompi) per chiuderlo e arrestarne il kernel Python. Chiudere quindi tutte le schede del browser Jupyter.

## <a name="clean-up"></a>Eliminazione

Se per il momento non è più necessario usare Azure Machine Learning, nella scheda **Istanze di ambiente di calcolo** della pagina **Calcolo** di Azure Machine Learning, selezionare l'istanza di ambiente di calcolo desiderata e fare clic su **Arresta** per arrestarla. In caso contrario, lasciarlo in esecuzione per il lab successivo.

> **Nota**: l'arresto delle risorse di calcolo garantisce che alla sottoscrizione non vengano addebitati i costi di calcolo corrispondenti. Verrà tuttavia addebitato un importo ridotto per l'archiviazione dei dati, fintanto che l'area di lavoro di Azure Machine Learning è presente nella sottoscrizione. Se è stata completata l'esplorazione di Azure Machine Learning, è possibile eliminare l'area di lavoro di Azure Machine Learning e le risorse associate. Tuttavia, se si prevede di completare altri lab di questa serie, è necessario prima ripetere l'esercizio *[Creare un'area di lavoro di Azure Machine Learning](01-create-a-workspace.md)* per creare l'area di lavoro e preparare l'ambiente.