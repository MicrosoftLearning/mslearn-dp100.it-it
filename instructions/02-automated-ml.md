---
lab:
  title: Usare Machine Learning automatizzato
ms.openlocfilehash: 25312648c7957dfd958098bc74faac249382eec8
ms.sourcegitcommit: 48c912e43571d4bddcc70260e4dc85ebbc040b27
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/30/2021
ms.locfileid: "133289667"
---
# <a name="use-automated-machine-learning"></a>Usare Machine Learning automatizzato

Azure Machine Learning include una funzionalità di *Machine Learning automatizzato* che sfrutta il ridimensionamento del calcolo cloud per provare automaticamente più tecniche di pre-elaborazione e algoritmi di training del modello in parallelo per trovare il modello di Machine Learning con le prestazioni migliori per i dati.

In questo esercizio si userà l'interfaccia visiva per Machine Learning automatizzato in Azure Machine Learning Studio

> **Nota**: è possibile usare Machine Learning automatizzato tramite Azure Machine Learning SDK.

## <a name="before-you-start"></a>Prima di iniziare

Se non è già stato fatto, completare l'esercizio *[Creare un'area di lavoro Azure Machine Learning](01-create-a-workspace.md)* per creare un'area di lavoro di Azure Machine Learning e un'istanza di calcolo e clonare i notebook necessari per questo esercizio.

## <a name="configure-compute-resources"></a>Configurare le risorse di calcolo

Per poter usare Machine Learning automatizzato, è necessario un ambiente di calcolo in cui eseguire l'esperimento di training del modello.

1. Accedere ad [Azure Machine Learning Studio](https://ml.azure.com?azure-portal=true) con le credenziali Microsoft associate alla sottoscrizione di Azure e selezionare l'area di lavoro Azure Machine Learning.
2. In Azure Machine Learning Studio visualizzare la pagina **Calcolo** e nella scheda **Istanze di ambiente di calcolo** avviare l'istanza di calcolo se non è già in esecuzione. Questa istanza di calcolo verrà usata per testare il modello sottoposto a training.
3. Mentre viene avviata l'istanza di ambiente di calcolo, passare alla scheda **Cluster di elaborazione** e aggiungere un nuovo cluster di elaborazione con le impostazioni specificate di seguito. L'esperimento di Machine Learning automatizzato verrà eseguito in questo cluster per usufruire della possibilità di distribuire le esecuzioni di training tra più nodi di calcolo:
    - **Posizione**: *la stessa posizione dell'area di lavoro*
    - **Priorità macchina virtuale**: Dedicata
    - **Tipo di macchina virtuale**: CPU
    - **Dimensioni macchina virtuale**: Standard_DS11_v2
    - **Nome dell'ambiente di calcolo**: *immettere un nome univoco*
    - **Numero minimo di nodi**: 0
    - **Numero massimo di nodi**: 2
    - **Secondi di inattività prima della riduzione delle prestazioni**: 120
    - **Abilita accesso SSH**: opzione non selezionata

## <a name="create-a-dataset"></a>Creare un set di dati

Ora che si hanno a disposizione alcune risorse di calcolo per l'elaborazione dei dati, è necessario trovare un modo per archiviare e inserire i dati da elaborare.

1. Visualizzare i dati delimitati da virgole in https://aka.ms/diabetes-data nel Web browser. Eseguire quindi il salvataggio come file locale denominato **diabetes.csv** (il percorso di salvataggio non è rilevante).
2. In Azure Machine Learning Studio visualizzare la pagina **Set di dati**. I set di dati rappresentano tabelle o file di dati specifici che si prevede di usare in Azure ML.
3. Creare un nuovo set di dati dai file locali, usando le impostazioni seguenti:
    * **Informazioni di base**:
        * **Nome**: set di dati diabete
        * **Dataset type** (Tipo di set di dati): tabulare
        * **Descrizione**: dati sul diabete
    * **Selezione archivio dati e file**:
        * **Selezionare o creare un archivio dati**: archivio dati attualmente selezionato
        * **Selezionare i file per il set di dati**: passare al file **diabetes.csv** scaricato.
        * **Percorso di caricamento**: *lasciare la selezione predefinita*
        * **Ignora convalida dei dati**: non selezionare
    * **Impostazioni e anteprima**:
        * **Formato di file**: delimitato
        * **Delimitatore**: virgola
        * **Codifica**: UTF-8
        * **Intestazioni colonna**: solo il primo file ha intestazioni
        * **Ignora righe**: Nessuno
    * **Schema**:
        * Includi tutte le colonne diverse da **Path**
        * Rivedi i tipi rilevati automaticamente
    * **Conferma i dettagli**:
        * Non profilare il set di dati dopo la creazione
4. Dopo aver creato il set di dati, aprirlo e visualizzare la pagina **Esplora** per visualizzare un esempio dei dati. Questi dati rappresentano i dettagli dei pazienti sottoposti al test del diabete e verranno usati per eseguire il training di un modello in grado di prevedere la probabilità che un paziente risulti positivo al test sulla base di misurazioni cliniche.

    > **Nota**: facoltativamente, è possibile generare un *profilo* del set di dati per poter vedere maggiori dettagli statistici.

## <a name="run-an-automated-machine-learning-experiment"></a>Eseguire un esperimento di Machine Learning automatizzato

In Azure Machine Learning, le operazioni eseguite sono denominate *esperimenti*. Attenersi alla procedura seguente per eseguire un esperimento che si avvalga di Machine Learning automatizzato per eseguire il training di un modello di classificazione in grado di prevedere le diagnosi di diabete.

1. In Azure Machine Learning Studio visualizzare la pagina **ML automatizzato** (disponibile in **Autore**).
2. Creare una nuova esecuzione di ML automatizzato con le impostazioni seguenti:
    - **Seleziona set di dati**:
        - **Set di dati**: set di dati diabete
    - **Configura esecuzione**:
        - **Nome nuovo esperimento**: mslearn-automl-diabetes
        - **Colonna di destinazione**: Diabetic (*questa è l'etichetta per la quale verrà eseguito il training del modello per la previsione)*
        - **Select compute type** (Seleziona tipo di calcolo): cluster di elaborazione
        - **Select Azure ML compute cluster** (Seleziona cluster di elaborazione di Azure ML): *il cluster di elaborazione creato in precedenza*
    - **Tipo di attività e impostazioni**:
        - **Tipo di attività**: classificazione
        - Selezionare **View additional configuration settings** (Visualizza altre impostazioni di configurazione) per aprire **Configurazioni aggiuntive**:
            - **Metrica primaria**: selezionare **AUC_Weighted** *(altre informazioni su questa metrica verranno fornite più avanti)*
            - **Spiega modello migliore**: Selezionato: *questa opzione fa in modo che il Machine Learning automatizzato calcoli l'importanza della funzionalità per il modello migliore, rendendo possibile determinare l'influenza di ogni funzionalità nell'etichetta stimata.*
            - **Algoritmi bloccati**: lasciare l'impostazione predefinita. *Teoricamente, è possibile usare tutti gli algoritmi durante il training*
            - **Criterio di uscita**:
                - **Tempo del processo di training (ore)**: 0,5. *L'esperimento terminerà dopo un massimo di 30 minuti.*
                - **Soglia di punteggio metrica**: 0,90. *L'esperimento termina se un modello raggiunge una metrica AUC ponderata del 90% o superiore.*
        - Selezionare **Visualizza impostazioni di definizione delle funzionalità** per aprire **Definizione delle funzionalità**:
            - **Abilita definizione delle funzionalità**: Selezionato: *questa operazione causa la pre-elaborazione automatica delle funzionalità da parte di Azure Machine Learning prima del training.*
    - **Selezionare il tipo di convalida e di test**:
        - **Tipo di convalida**: suddivisione tra training e convalida
        - **Percentage validation of data** (Percentuale di convalida dei dati): 30
        - **Set di dati di test**: nessun set di dati di test necessario

3. Al termine dell'invio dei dettagli di esecuzione del ML automatizzato, l'avvio verrà eseguito automaticamente. È possibile osservare lo stato dell'esecuzione nel riquadro **Proprietà**.
4. Quando lo stato di esecuzione passa a *In esecuzione*, visualizzare la scheda **Modelli** e osservare mentre viene provata ogni combinazione possibile di algoritmi di training e passaggi di pre-elaborazione e vengono valutate le prestazioni del modello risultante. La pagina verrà aggiornata automaticamente periodicamente, ma è anche possibile selezionare **&#8635; Aggiorna**. Potrebbero essere necessari circa dieci minuti prima che i modelli inizino a essere visualizzati poiché, per poter iniziare il training, è necessario che i nodi del cluster siano inizializzati e il processo di definizione delle caratteristiche dei dati sia completato. Potrebbe essere ora il momento giusto per una pausa caffè.
5. Attendere il completamento dell'esperimento.

## <a name="review-the-best-model"></a>Esaminare il modello migliore

Al termine dell'esperimento, è possibile esaminare il modello con le prestazioni migliori che è stato generato (si noti che in questo caso sono stati usati criteri uscita per arrestare l'esperimento, quindi il modello "migliore" trovato dall'esperimento potrebbe non essere il miglior modello possibile, ma solo quello più adatto tenendo conto dei vincoli di tempo e di metriche previsti in questo esercizio).

1. Nella scheda **Dettagli** dell'esecuzione del Machine Learning automatizzato, prendere nota del riepilogo del modello migliore.
2. Selezionare il **Nome algoritmo** relativo al modello migliore per visualizzare l'esecuzione figlio che ha generato.

    Il modello migliore viene identificato in base alla metrica di valutazione specificata (*AUC_Weighted*). Per calcolare questa metrica, il processo di training ha usato alcuni dei dati per eseguire il training del modello e ha applicato una tecnica denominata *convalida incrociata*, per testare in modo iterativo il modello sottoposto a training con dati con cui non è stato eseguito il training e confrontare il valore stimato con il valore noto effettivo. Da questi confronti viene creata una *matrice di confusione* in formato tabulare di veri positivi, falsi positivi, veri negativi e falsi negativi e vengono calcolate metriche di classificazione aggiuntive, incluso un grafico ROC (Receiving Operator Curve) che confronta il tasso dei veri positivi con quello dei falsi positivi. L'area sotto la curva (AUC) è una metrica comunemente usata per valutare le prestazioni di classificazione.
3. Accanto al valore *AUC_Weighted* selezionare **Visualizza tutte le altre metriche** per visualizzare i valori di altre metriche di valutazione possibili per un modello di classificazione.
4. Selezionare la scheda **Metriche** ed esaminare le metriche delle prestazioni che è possibile visualizzare per il modello. Tra queste sono incluse, ad esempio, la visualizzazione **confusion_matrix**, che mostra la matrice di confusione per il modello convalidato, e la visualizzazione **accuracy_table**, che include il grafico ROC.
5. Selezionare la scheda **Spiegazioni**, selezionare un **ID spiegazione** e quindi visualizzare la pagina relativa all'**importanza dell'aggregazione**, in cui viene illustrato in quale misura ogni funzionalità del set di dati influisce sulla stima dell'etichetta.

## <a name="deploy-a-predictive-service"></a>Distribuire un servizio predittivo

Dopo aver usato il Machine Learning automatizzato per eseguire il training di alcuni modelli, è possibile distribuire il modello con le prestazioni migliori come servizio per l'uso da parte delle applicazioni client.

> **Nota**: in Azure Machine Learning, è possibile distribuire un servizio come Istanze di Azure Container (ACI) o in un cluster del servizio Azure Kubernetes (AKS). Per gli scenari di produzione è consigliabile usare una distribuzione di AKS, per la quale è necessario creare una destinazione di calcolo *cluster di inferenza*. In questo esercizio si userà un servizio ACI, che è una destinazione di distribuzione adatta per i test e non richiede la creazione di un cluster di inferenza.

1. Selezionare la scheda **Dettagli** relativa all'esecuzione che ha prodotto il modello migliore.
2. Dall'opzione **Distribuisci** usare il pulsante **Distribuisci in Servizio Web** per distribuire il modello con le impostazioni seguenti:
    - **Nome**: auto-predict-diabetes
    - **Descrizione**: prevedere il diabete
    - **Tipo di ambiente di calcolo**: Istanza di Azure Container
    - **Abilita autenticazione**: Opzione selezionata
    - **Use custom deployment assets** (Usa risorse di distribuzione personalizzate): opzione deselezionata
3. Attendere l'avvio della distribuzione. L'operazione potrebbe richiedere alcuni secondi. Nella scheda **Modello** della sezione **Riepilogo modelli** osservare lo **Stato di distribuzione** del servizio **auto-predict-diabetes**, che deve essere **In esecuzione**. Attendere che questo stato cambi in **Riuscito**. Potrebbe essere necessario selezionare periodicamente **&#8635; Aggiorna**.  **NOTA** Questa operazione potrebbe richiedere alcuni minuti.
4. In Azure Machine Learning Studio visualizzare la pagina **Endpoint** e selezionare l'endpoint in tempo reale **auto-predict-diabetes**. Selezionare quindi la scheda **Utilizzo** e prendere nota delle informazioni seguenti. Queste informazioni sono necessarie per collegarsi al servizio distribuito da un'applicazione client.
    - Endpoint REST relativo al servizio
    - Chiave primaria relativa al servizio
5. Tenere presente che è possibile usare il collegamento &#10697; disponibile accanto ai valori per copiarli negli Appunti.

## <a name="test-the-deployed-service"></a>Testare il servizio distribuito

Ora che è stato distribuito un servizio, è possibile eseguirne il test usando un codice semplice.

1. Tenendo aperta nel browser la pagina **Utilizzo** relativa alla pagina del servizio **auto-predict-diabetes**, aprire una nuova scheda nel browser e una seconda istanza di Azure Machine Learning Studio. Nella nuova scheda aprire la pagina **Notebook**.
2. Nella pagina **Notebook**, in **File personali**, passare alla cartella **/users/*nome-utente*/mslearn-dp100** in cui è stato clonato il repository del notebook e aprire il notebook **Get AutoML Prediction**.
3. All'apertura del notebook, verificare che nella casella **Calcolo** sia selezionata l'istanza di ambiente di calcolo generata in precedenza e che tale istanza sia nello stato **In esecuzione**.
4. Nel notebook sostituire i segnaposto **ENDPOINT** e **PRIMARY_KEY** con i valori relativi al proprio servizio, che è possibile copiare dalla scheda **Utilizzo** nella pagina relativa all'endpoint.
5. Eseguire la cella di codice e visualizzare l'output restituito dal servizio Web.

## <a name="clean-up"></a>Eliminazione

Il servizio Web creato è ospitato in un'*Istanza di contenitore di Azure*. Se non si vogliono eseguire altri esperimenti con tale servizio, è consigliabile eliminare l'endpoint per evitare di accumulare tempi di utilizzo superflui per Azure. È anche opportuno arrestare l'istanza di ambiente di calcolo fino a quando non sarà nuovamente necessaria.

1. In Azure Machine Learning Studio, nella scheda **Endpoint**, selezionare l'endpoint **auto-predict-diabetes**. Selezionare quindi **Elimina** (&#128465;) e confermare l'eliminazione dell'endpoint.
2. Nella pagina **Calcolo**, nella scheda **Istanze di ambiente di calcolo**, selezionare l'istanza di ambiente di calcolo e quindi **Arresta**.

> **Nota**: l'arresto delle risorse di calcolo garantisce che alla sottoscrizione non vengano addebitati i costi di calcolo corrispondenti. Verrà tuttavia addebitato un importo ridotto per l'archiviazione dei dati, fintanto che l'area di lavoro di Azure Machine Learning è presente nella sottoscrizione. Se è stata completata l'esplorazione di Azure Machine Learning, è possibile eliminare l'area di lavoro di Azure Machine Learning e le risorse associate. Tuttavia, se si prevede di completare altri lab di questa serie, è necessario prima ripetere l'esercizio *[Creare un'area di lavoro di Azure Machine Learning](01-create-a-workspace.md)* per creare l'area di lavoro e preparare l'ambiente.
