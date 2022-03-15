---
lab:
  title: Creare un'area di lavoro di Machine Learning di Azure
ms.openlocfilehash: 03b79f321ac3b5f7a5b9a03a3760db5649898eed
ms.sourcegitcommit: 66d8872bc3d24c2121e225be132b56f4df7920ac
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/12/2022
ms.locfileid: "138597268"
---
# <a name="create-and-explore-an-azure-machine-learning-workspace"></a>Creare ed esplorare un'area di lavoro di Azure Machine Learning

In questo esercizio si creerà e si esplorerà un'area di lavoro di Azure Machine Learning.

## <a name="create-an-azure-machine-learning-workspace"></a>Creare un'area di lavoro di Machine Learning di Azure

Come suggerisce il nome, un'area di lavoro è un'area centralizzata in cui è possibile gestire tutti gli asset di Azure ML necessari per lavorare a un progetto di Machine Learning.

1. Nel [portale di Azure](https://portal.azure.com) creare una nuova risorsa **Machine Learning** specificando le impostazioni seguenti:

    - **Sottoscrizione**: *la propria sottoscrizione di Azure*
    - **Gruppo di risorse**: *creare o selezionare un gruppo di risorse*
    - **Nome area di lavoro**: *immettere un nome univoco per l'area di lavoro*
    - **Area**: *selezionare l'area geografica più vicina*
    - **Account di archiviazione**: *prendere nota del nuovo account di archiviazione predefinito che verrà creato per l'area di lavoro*
    - **Insieme di credenziali delle chiavi**: *prendere nota del nuovo insieme di credenziali delle chiavi predefinito che verrà creato per l'area di lavoro*
    - **Application Insights**: *prendere nota della nuova risorsa Application Insights predefinita che verrà creata per l'area di lavoro*
    - **Registro contenitori**: Nessuno (*ne verrà creato uno automaticamente la prima volta che si distribuisce un modello in un contenitore*)

    > **Nota**: quando si crea un'area di lavoro di Azure Machine Learning, è possibile usare alcune opzioni avanzate che consentono di limitare l'accesso tramite un *endpoint* privato e di specificare chiavi personalizzate per la crittografia dei dati. Queste opzioni non verranno usate in questo esercizio, ma è importante sapere che sono disponibili.

2. Dopo aver creato l'area di lavoro e le risorse associate, visualizzare l'area di lavoro nel portale.

## <a name="explore-azure-machine-learning-studio"></a>Esplorare Azure Machine Learning Studio

È possibile gestire alcuni asset dell'area di lavoro nel portale di Azure, ma per i data scientist questo strumento contiene innumerevoli informazioni e collegamenti irrilevanti che riguardano la gestione delle risorse generali di Azure. *Azure Machine Learning Studio* offre un portale Web dedicato per l'utilizzo dell'area di lavoro personale.

1. Nel pannello del portale di Azure dedicato all'area di lavoro di Azure Machine Learning, fare clic sul collegamento per avviare Studio; in alternativa, in una nuova scheda del browser aprire [https://ml.azure.com](https://ml.azure.com). Se richiesto, accedere usando l'account Microsoft usato nell'attività precedente e selezionare la sottoscrizione e l'area di lavoro di Azure personali.

    > **Suggerimento** Se si hanno più sottoscrizioni di Azure, è necessario scegliere la *directory* di Azure in cui è definita la sottoscrizione desiderata, quindi selezionare la sottoscrizione e infine scegliere l'area di lavoro.

2. Visualizzare l'interfaccia di Azure Machine Learning Studio per l'area di lavoro, da cui è possibile gestire tutti gli asset disponibili.
3. In Azure Machine Learning Studio fare clic sull'icona &#9776; in alto a sinistra per visualizzare o nascondere le varie pagine dell'interfaccia. Queste pagine consentono di gestire le risorse nell'area di lavoro.

## <a name="create-a-compute-instance"></a>Creare un'istanza di ambiente di calcolo

Uno dei vantaggi offerti da Azure Machine Learning è la possibilità di creare un ambiente di calcolo basato sul cloud in cui è possibile eseguire esperimenti e script di training su larga scala.

1. In Azure Machine Learning Studio visualizzare la pagina **Calcolo**, da cui verranno gestite le risorse di calcolo per le attività di data science. È possibile creare i quattro tipi di risorse di calcolo descritti di seguito.
    - **Istanze di ambiente di calcolo**: workstation per lo sviluppo che i data scientist possono usare per lavorare con dati e modelli.
    - **Cluster di elaborazione**: cluster scalabili di macchine virtuali per l'elaborazione su richiesta del codice di un esperimento.
    - **Cluster di inferenza**: destinazioni di distribuzione per servizi predittivi che usano i modelli con training.
    - **Ambiente di calcolo collegato**: collegamenti a risorse di calcolo di Azure già esistenti, ad esempio macchine virtuali o cluster di Azure Databricks.

    Per questo esercizio si creerà un'istanza di ambiente di calcolo per poter eseguire il codice necessario nell'area di lavoro.

2. Nella scheda **Istanze di ambiente di calcolo** aggiungere una nuova istanza di ambiente di calcolo con le impostazioni specificate di seguito. Verrà usata come workstation per l'esecuzione del codice nei notebook.
    - **Nome dell'ambiente di calcolo**: *immettere un nome univoco*
    - **Posizione**: *la stessa posizione dell'area di lavoro*
    - **Tipo di macchina virtuale**: CPU
    - **Dimensioni macchina virtuale**: Standard_DS11_v2
    - **Quote totali disponibili**: vengono visualizzati i core dedicati disponibili.
    - **Mostra impostazioni avanzate**: esaminare le impostazioni seguenti, ma non selezionarle. 
        - **Abilita accesso SSH:** deselezionata *(è possibile usarla per abilitare l'accesso diretto alla macchina virtuale usando un client SSH)*
        - **Abilita rete virtuale**: deselezionata *(viene solitamente usata negli ambienti aziendali per migliorare la sicurezza di rete)*
        - **Assegna a un altro utente**: deselezionata *(è possibile usarla per assegnare un'istanza di ambiente di calcolo a un data scientist)* . Attendere che l'istanza di calcolo venga avviata e che ne venga modificato lo stato in **Esecuzione**.

> **Nota**: Le istanze di calcolo e i cluster sono basati su immagini di macchine virtuali di Azure Standard. Per questo esercizio, è consigliabile usare l'immagine *Standard_DS11_v2* per ottenere un equilibrio ottimale tra costi e prestazioni. Se la quota della sottoscrizione in uso non include questa immagine, scegliere un'immagine alternativa, ma tenere presente che un'immagine superiore può generare costi più elevati e un'immagine inferiore potrebbe non essere sufficiente per completare le attività. In alternativa, chiedere all'amministratore di Azure di estendere la quota.

## <a name="clone-and-run-a-notebook"></a>Clonare ed eseguire un notebook

La sperimentazione di processi di data science e Machine Learning viene eseguita per lo più eseguendo codice nei *notebook*. L'istanza di ambiente di calcolo include ambienti notebook Python completi (*Jupyter* e *JuypyterLab*) che è possibile usare per eseguire processi di lavoro estesi, per apportare modifiche di base al notebook, tuttavia, è sufficiente usare la pagina **Notebook** predefinita in Azure Machine Learning Studio.

1. In Azure Machine Learning Studio visualizzare la pagina **Notebook**.
2. Se viene visualizzato un messaggio che descrive le nuove funzionalità, chiuderlo.
3. Selezionare **Terminale** o l'icona di **apertura del terminale** per aprire un terminale e verificare che l'opzione **Calcolo** sia impostata sull'istanza di ambiente di calcolo e che il percorso corrente coincida con la cartella **/users/nome-utente**.
4. Immettere il comando seguente per clonare nell'area di lavoro un repository Git contenente notebook, dati e altri file:

    ```bash
    git clone https://github.com/MicrosoftLearning/mslearn-dp100 mslearn-dp100
    ```

4. Dopo il completamento del comando, nel riquadro **File personali** fare clic su **&#8635;** per aggiornare la visualizzazione e verificare che sia stata creata una nuova cartella **/users/*nome-utente*/mslearn-dp100**. Nella cartella sono contenuti più file notebook con estensione **ipynb**.
5. Chiudere il riquadro del terminale per terminare la sessione.
6. Nella cartella **/users/*nome-utente*/mslearn-dp100** aprire il notebook **Get Started with Notebooks**. Leggere quindi le note e seguire le istruzioni riportate.

> **Suggerimento**: per eseguire una cella di codice, selezionarla e fare clic sul pulsante **&#9655;** per eseguirla. 

> **Se non si conosce Python** Usare la [scheda di informazioni di Python](cheat-sheets/dp100-cheat-sheet-python.pdf) per comprendere il codice.

> **Se non si ha esperienza di Machine Learning** Usare la [panoramica di Machine Learning](cheat-sheets/dp100-cheat-sheet-machine-learning.pdf) per una panoramica semplificata del processo di Machine Learning in Azure Machine Learning.

## <a name="stop-your-compute-instance"></a>Arrestare l'istanza di ambiente di calcolo

Se per il momento non è necessario esplorare ulteriormente Azure Machine Learning, è opportuno arrestare l'istanza di ambiente calcolo per evitare inutili addebiti nella sottoscrizione di Azure.

1. Nella pagina **Calcolo** di Azure Machine Learning Studio selezionare l'istanza di ambiente di calcolo creata.
2. Fare clic su **Arresta** per arrestare l'istanza di ambiente di calcolo. Al termine del processo di arresto, lo stato verrà modificato in **Arrestato**.

> **Nota**: l'arresto delle risorse di calcolo garantisce che alla sottoscrizione non vengano addebitati i costi di calcolo corrispondenti. Verrà tuttavia addebitato un importo ridotto per l'archiviazione dei dati, fintanto che l'area di lavoro di Azure Machine Learning è presente nella sottoscrizione. Se è stata completata l'esplorazione di Azure Machine Learning, è possibile eliminare l'area di lavoro di Azure Machine Learning e le risorse associate. Tuttavia, se si prevede di completare altri lab di questa serie, è necessario prima ripetere questo lab per creare l'area di lavoro e preparare l'ambiente.
