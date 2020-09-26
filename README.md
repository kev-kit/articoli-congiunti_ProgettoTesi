# Progetto di Tesi
# Articoli congiunti: studio dei risultati di un software per la correlazione di testi con tematiche simili

            --- WORK IN PROGRESS ---

Progetto di tesi per il corso di laurea in Informatica Umanistica.

## ABSTRACT
Software per correlare articoli testuali con tematiche simili.
Il progetto rientra nel campo del *Natural Language Processing* perchè sono utilizzati strumenti di elaborazione del linguaggio naturale per annotare i testi e stabilire le correlazioni.

## FUNZIONAMENTO
Il software estrae e annota morfo-sintatticamente le informazioni presenti in articoli appartenenti a un corpus prevalentemente monotematico.
Gli articoli sono stati selezionati dal portale web teatronaurale.it (https://www.teatronaturale.it/).

Le informazioni annotate sono trattate con due metodi diversi per selezionare le informazioni in grado di descrivere le tematiche presenti negli articoli, anche dette informazioni chiave.

I metodi utilizzati privilegiano le informazioni che costituiscono sintagmi nominali, escludendo altri tipi di sintagma poiché considerati meno informativi al fine di individuare le tematiche presenti nel testo. I metodi sono:

  - **Longer is Better**, le informazioni chiave sono scelte in base al numero di occorrenza nei testi e alla lunghezza del sintagma nominale.
  Ai sintagmi più lunghi, cioè con più termini, è attribuito un punteggio maggiore poiché si assume che un numero maggiore di termini garantisca che l'informazione riguardo alle tematiche sia più precisa e, al momento del confronto delle informazioni chiave selezionate per gli articoli, offre potenzialmente più esiti positivi e quindi correlazioni più precise. 
  
  - **Close Context**, le informazioni chiave sono costruite come collocazioni, cioè coppie *<termine principale, termine contesto>* dove il termine contesto è estratto dall'intorno terminologico del termine principale. 
  I termini sono individuati all'interno di sintagmi nominali e le coppie sono definite bigrammi perchè composte da due termini. 
  Per ogni termine principale si ottengono più termini contesto, quindi più bigrammi. Tra questi sono selezionati i bigrami il cui punteggio di associazione lessicale tra i termini, valutato con la *Local Mutual Information*, LMI, è più alto. I bigrami sono poi ordinati per importanza in base al loro valore di LMI.
  Le informazioni chiave sono, perciò, individuate come bigrammi che hanno un forte rapporto di associazione tra di loro e quindi per il testo di appartenenza. Una forte capacita associativa tra i termini del bigramma implica che la loro occorrenza nel testo sia rilevante, pertanto si assume che sia una informazione tematica dell'articolo. Inoltre, il valore LMI fa emergere termini fortemente funzionali l'uno all'altro e perciò i bigrammi prodotti risultano informazioni chiave puntuali, cercando, così, di arginare il limite della generalità della lingua.
  
Individuate le informazioni chiave, queste vengono confrontate in base alla loro somiglianza lessicale: ogni esito positivo dei confronti aumenta il punteggio di correlazione tra gli articoli, più alto è il punteggio più gli articoli sono considerati correlati. Quindi, gli articoli del corpus con più informazioni in comune sono considerati correlati.

## STRUTTURA DEL SOFTWARE
Il software, che consiste in un web-crawler che estrapola e analizza gli articoli di un database o collezione di articoli, si compone di due parti:

  1. **GET**, la prima parte del software che recupera gli articoli dal web, effettua la pulizia degli articoli eliminando i residui di codice, HTML e vari, e normalizza i testi in un formato elaborabile dall'annotatore morfo-sintattico utilizato, che in questo caso è stato il Part of Speech Tagger messo a disposizione dall'ItaliaNLP Lab (http://www.italianlp.it/).
 
  2. **PROC**, la seconda parte del software che si distignue rispetto ai due metodi di analisi e confronto sopra indicati. In generale, i due metodi utilizzano le informazioni morfo-sintattiche ottenute nella parte precedente per definire le informazioni chiave degli articoli e confrontarle tra loro in modo da stabilire le correlazioni.  
