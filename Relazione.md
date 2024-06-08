### 1. Funzionamento TRIS

Il gioco prevede che due giocatori si sfidino su un campo 3 x 3. L'obbiettivo è allineare 3 gettoni consecutivi in modo orizzontale o verticale o diagonale. La vittoria viene assegnata al primo giocatore che riesce a ottenere questo allineamento.

### 2. Casi d'uso

Per iniziare una partita, dovrebbe essere eseguito il lato server, se il server non sta eseguendo, il client non possono giocare e terminano l'esecuzione. Il seguente comando per eseguire il server:

- ./TriServer <Gettone1> <Gettone2> <Timeout>（Opzionale）

Gettone 1 e 2: devono essere un carattere, sono i gettoni per stampa la matrice nel lato cliente.

Timeout（Opzionale）: dove essere un numero intero, indica il tempo massimo del inserimento del gettone. Se non viene scritto vuol dire i giocatori può inserire in tempo infinito.

Quando il server sta eseguendo, i client possono raggiungere nel server per giocare. Il seguente comando per eseguire il client:

- ./TriClient <username>

Il client si può giocare con un bot, Il seguente comando per giocare con un bot:

- ./TriClient <username> <\\*>

### 3. Comunicazione tra il server e i client

Quando il server viene eseguito, inizializzare la memoria condivisa e il set dei semafori, registra i funzioni handler per i segnali e si comunicano con questi meccanismi.

### 4. Memoria condivisa

La memoria condivisa viene implementato con i seguente struttura:

```c
struct sharedMemory {
  int playerInsert[3][3]; 
  char symbol[2];	
  int players;
  int winner;
  int step;
  int bot;
  pid_t pid[3];
  char *playername[2];
  char newgame;
  int whosturn;
  int gameend;
  int abbandoPlayer;
  int timeout;
}
```

- **playerInsert**: è la matrice della partita

- **symbol**: è un array per salvare i gettoni da stapare nel lato client

- **players**: un numero intero indica quanti giocatori sono presenti nel server

- **winner**: un numero intero specifica il vincitore della partita

- **step**: un numero intero specifica i step disponibili

- **bot**: un numero intero specifica che nel server è presente un bot, se esiste un bot, sarrebe numero intero 2

- **pid**: un array per salvare i pid dei processi, sia il server che i client

- **playername**: un array per salvare i username dei giocatori

- **newgame**: un carattere per specificare il vincitore se vuole iniziare una nuova partita oppure no. 

  Se si, dovrebbe essere 'y'.

  Se no, doverbbe essere 'n'.

  Se la partita non ha finito ancora: doverbbe essere '\0'.

- **whosturn**: un numero intero indica il turno tocca qual giocatore

- **gameend**: un numero intero indica se la situazione della partita,

  Se la partita non ha ancora finita, dovrebbe essere 0

  Se la partita è finita, dovrebbe essere 1 

- **abbandoPlayer**: un numero intero indica il giocatore che ha abbandonato, se nessun ha abbandonato, doverbbe essere 0

- **timeout**: un numero intero indica il massimo tempo del inserimento del giocatore, se non viene specicato doverbbe essere 0

### 5.Semaforo

Il set di semaforo viene inizializzato con 4 semafori:

```c
unsigned short semInitVal[] = {0, 0, 0, 0};
```

i semafori vengono definiti:

1. Server
2. Giocatore1
3. Giocatore2
4. Quando un giocatore ha vinto, il vincitore deve scegliere se vuole continua oppure uscire dal Server, in cui il perdente deve aspettare. Dopo il vincitore ha scelto il perdente può continuare di eseguire. 

### 6.Segnali

#### Server:

```c
signal(SIGINT, signalShutDown);
signal(SIGUSR1, signalAbbando);
signal(SIGUSR2, signalBot);
signal(SIGALRM, signalTimeOut);
```

- SIGINT: per terminare il processo server
- SIGUSR1: riceve il segnale dal lato client quando un giocatore vuole abbandonare
- SIGUSR2: riceve il segnale dal lato client quando un giocatore vuole giocare con un bot
- SIGALRM: quando il server ha specificato un tempo massimo per i turni del inserimento

Funzioni:

```c
void signalShutDown(int signal);
```

La funzione per gestire la chiusura del Server. Quando il lato Server viene premuto Ctrl C 2 volte consecutive, questa funzione il segnale SIGTERM ai figli e cancellare la memoria condivisa e semafori in maniera giusta, alla fine termina il processo.

```c
void signalAbbando(int signal);
```

La funzione viene eseguita quando un giocatore ha deciso di abbandonare il gioco, cioè ricevuto il segnale SIGUSR1 dal giocatore. La funzione verifica chi ha abbanato, cancellare i suoi dati nella menmoria condivisa e termina il suo processo tramite il segnale SIGTERM, in fine stampa un messagio sul terminale.

```c
void signalBot(int signal);
```

 Quando un giocatore vuole giocare con un bot, il Server riceve un segnale SIGUSR2 dal Client, il Server genera un processo con il systemcall fork(), dopo di che il nuovo processo esegue il systemcall execlp, "TriClient" come parametro, fin qui il bot viene generato.

```c
void signalTimeOut(int signal);
```

Quando il giocatore gioca in fuori tempo, il Server manda il segnale SIGTERM ai giocatori. 

#### Client:

```c
signal(SIGTERM, signalHandler);
signal(SIGUSR2, signalAbando);
signal(SIGINT, signalAbando);
```

- SIGTERM: riceve il segnale dal lato Server in 2 casi:
  1. Quando il Server sta terminando con Ctrl C
  2. Quando un giocatore sta fuori tempo
- SIGUSR2: riceve il segnale dal lato Server quando l'atro giocatore ha deciso di abbandonare il gioco.
- SIGINT: Quando il giocatore ha deciso di abbandonare il gioco

```c
void signalHandler(int signal);
```

Questa funzione esegue in 2 casi:

1. Quando il Server sta per terminare, riceve il segnale SIGTERM dal Server, stampa un messagio e termina.
2. Quando un giocatore gioca in fuori tempo, riceve il segnale SIGTERM dal Server, stampa un messagio e  termina.

```c
void signalAbando(int signal);
```

Chi ha abbandonato il gioco manda il segnale SIGUSR1 al Server e stampa il messagio, invece l'atro giocatore riceve il segnale SIGUSR2 dal Server e stampa un messaggio
