---
lab:
  title: Eseguire esperimenti
ms.openlocfilehash: 875b204cd18963c53ff0b7d2ee2334ebc22c17e6
ms.sourcegitcommit: 18f734eeb1031a9cb69c3b294632efd2e69324ac
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/17/2021
ms.locfileid: "132832640"
---
# <a name="run-experiments"></a>Eseguire esperimenti

Gli esperimenti sono alla base dell'attività professionale di un data scientist. In Azure Machine Learning, gli *esperimenti* vengono usati per eseguire script o pipeline e, in genere, producono un output e registrano metriche. In questo esercizio si userà Azure Machine Learning SDK per eseguire codice Python per gli esperimenti.

## <a name="before-you-start"></a>Prima di iniziare

Se non è già stato fatto, completare l'esercizio *[Creare un'area di lavoro Azure Machine Learning](01-create-a-workspace.md)* per creare un'area di lavoro di Azure Machine Learning e un'istanza di calcolo e clonare i notebook necessari per questo esercizio.

## <a name="open-jupyter"></a>Aprire Jupyter

Anche se è possibile usare la pagina **Notebook** in Azure Machine Learning Studio per eseguire i notebook, è spesso più produttivo usare un ambiente di sviluppo di notebook più completo come *Jupyter*. Fortunatamente, l'istanza di ambiente di calcolo di Azure Machine Learning include un'installazione di Jupyter.

> **Suggerimento**: Jupyter Notebook è uno strumento open source comunemente usato nell'ambito del data science. Se non si ha familiarità con questo strumento, è possibile fare riferimento alla relativa [documentazione](https://jupyter-notebook.readthedocs.io/en/stable/notebook.html).

1. In [Azure Machine Learning Studio](https://ml.azure.com) visualizzare la pagina **Calcolo** per l'area di lavoro e nella scheda **Istanze di ambiente di calcolo** avviare l'istanza di calcolo se non è già in esecuzione.
2. Quando l'istanza dell'ambiente di calcolo è in esecuzione, fare clic sul collegamento **Jupyter** per aprire la home page di Jupyter in una nuova scheda del browser. Controllare bene di aprire *Jupyter* e non *JupyterLab*.

## <a name="verify-the-azure-machine-learning-sdk-is-installed"></a>Verificare che Azure Machine Learning SDK sia installato

Azure Machine Learning SDK viene installato per impostazione predefinita nell'istanza di ambiente di calcolo in uso. Seguire questa procedura per verificare l'installazione.

1. Nell'ambiente notebook Jupyter creare un nuovo **Terminale**. Verrà aperta una nuova scheda con una shell di comando.
2. Immettere il comando seguente per verificare che Azure ML SDK sia installato:

    ```bash
    pip show azureml-sdk
    ```

    Prendere nota della versione del pacchetto SDK installata.

3. Il pacchetto SDK **azureml-sdk** offre le principali librerie necessarie per usare Azure Machine Learning. Esistono tuttavia alcuni pacchetti aggiuntivi che contengono altre utili librerie non incluse nel pacchetto SDK principale. Usare il comando seguente per verificare che sia installato anche il pacchetto **azureml-widgets**, che contiene librerie per la visualizzazione delle informazioni di Azure Machine Learning nei notebook:

    ```bash
    pip show azureml-widgets
    ```

4. Chiudere la scheda **Terminale** e tornare alla scheda contenente la home page di Jupyter.

> **Altre informazioni**: per altre informazioni sull'installazione di Azure ML SDK e dei relativi componenti facoltativi, vedere la [Documentazione di Azure ML SDK](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py).

## <a name="run-experiments-in-a-notebook"></a>Eseguire esperimenti in un notebook

In Azure Machine Learning, gli esperimenti devono essere avviati da una sorta di livello di *controllo*, spesso uno script o un programma. In questo esercizio si userà un notebook per il controllo degli esperimenti.

1. Nella home page di Jupyter passare alla cartella **/users/*nome-utente*/mslearn-dp100** in cui è stato clonato il repository del notebook e aprire il notebook **Run Experiments**.
2. Leggere quindi le note disponibili nel notebook, eseguendo una cella di codice alla volta.
3. Dopo aver finito di eseguire il codice nel notebook, dal menu **File** scegliere **Close and Halt** (Chiudi e interrompi) per chiuderlo e arrestarne il kernel Python. Chiudere quindi tutte le schede del browser Jupyter.

## <a name="clean-up"></a>Eliminazione

Se per il momento non è più necessario usare Azure Machine Learning, nella scheda **Istanze di ambiente di calcolo** della pagina **Calcolo** di Azure Machine Learning, selezionare l'istanza di ambiente di calcolo desiderata e fare clic su **Arresta** per arrestarla. In caso contrario, lasciarlo in esecuzione per il lab successivo.

> **Nota**: l'arresto delle risorse di calcolo garantisce che alla sottoscrizione non vengano addebitati i costi di calcolo corrispondenti. Verrà tuttavia addebitato un importo ridotto per l'archiviazione dei dati, fintanto che l'area di lavoro di Azure Machine Learning è presente nella sottoscrizione. Se è stata completata l'esplorazione di Azure Machine Learning, è possibile eliminare l'area di lavoro di Azure Machine Learning e le risorse associate. Tuttavia, se si prevede di completare altri lab di questa serie, è necessario prima ripetere l'esercizio *[Creare un'area di lavoro di Azure Machine Learning](01-create-a-workspace.md)* per creare l'area di lavoro e preparare l'ambiente.