## DOCUMENTATION

##### English below

### Uruchomienie programu
1. Sklonuj repozytorium:
    ```sh
   git clone https://github.com/Tesa44/so_2-project-1.git
2. Otwórz projekt w CLion
3. Skonfiguruj kompilator
4. Zbuduj projekt
5. Uruchom program
## Opis problemu
Wyobraź sobie pięciu filozofów siedzących przy okrągłym stole. Każdy z nich ma przed sobą talerz z jedzeniem, ale żeby móc jeść, potrzebuje dwóch pałeczek. Między każdym sąsiadującym filozofem leży jedna pałeczka – łącznie jest ich więc pięć.

Filozofowie spędzają czas na dwóch czynnościach: myśleniu i jedzeniu. Kiedy są głodni, próbują podnieść dwie pałeczki – jedną z lewej strony, drugą z prawej. Jeśli uda im się zdobyć obie, zaczynają jeść. Po skończonym posiłku odkładają pałeczki i wracają do myślenia.

Problem pojawia się, gdy kilku filozofów jednocześnie próbuje wziąć pałeczki. Może dojść do sytuacji, w której każdy podniesie jedną pałeczkę, ale nie będzie w stanie zdobyć drugiej, bo sąsiad już ją zajął. W takim przypadku wszyscy pozostają w zawieszeniu – trzymają jedną pałeczkę i czekają na drugą, ale nikt jej nie zwolni, bo nikt nie może zacząć jeść.

Jest to metafora problemu współdzielenia zasobów w systemach wielozadaniowych – filozofowie reprezentują procesy, a pałeczki to zasoby, które muszą być odpowiednio zarządzane, by uniknąć zakleszczenia (deadlocka).

## Program
### Wątki
W programie korzysta się z wątków z biblioteki *thread*.
Każdy wątek reprezentuje jednego filozofa, który ciągle próbuje jeść, a gdy już mu się to uda to zaczyna myśleć.
 Oznacza to, że w programie działa pięć wątków współbieżnie.
### Opis funkcji
- `philosopher(int position)` – główna funkcja wykonywana przez każdy wątek (filozofa). W nieskończonej pętli filozof próbuje jeść, następnie myśli i ponownie próbuje jeść.  
- `try_eat(int philosopher)` – filozof oznacza się jako głodny i sprawdza, czy może rozpocząć jedzenie. Jeśli nie, czeka.  
- `think(int philosopher)` – po zakończeniu jedzenia filozof odkłada pałeczki i przechodzi w stan myślenia.  
- `check(int philosopher)` – sprawdza, czy filozof może rozpocząć jedzenie (czy sąsiedzi nie jedzą).
### Sekcje krytyczne i ich rozwiązanie
Sekcja krytyczna to fragment kodu, w którym kilka wątków może próbować jednocześnie uzyskiwać dostęp do jednego zasobu.
W przypadku tego problemu jest to rywalizacja o pałeczki, które znajdują się przy filozofach.  
Aby uniknąć problemów z jednoczesnym dostępem do zasobów, program używa mechanizmu blokady:  
- `lock()` - blokuje dostęp do sekcji krytycznej  
- `unlock()` - odblokowuje dostęp po zakończeniu operacji  
#### Sekcje krytyczne w programie
- Próba podniesienia jednej pałeczki przez wszystkich filozofów jednocześnie
- Omijanie filozofów, którzy wiecznie czekają na wolne pałeczki.
- Próba podniesienia współdzielonych pałeczek przez wielu filozofów naraz
#### Rozwiązanie
Mechanizm synchronizacji działa na zasadzie prostej flagi `locker`, która wskazuje, czy sekcja krytyczna jest dostępna dla wątku, czy może używa ją już inny wątek.
Gdy wątek chce uzyskać dostęp do sekcji krytycznej, sprawdza, czy `locker == 1`. Jeśli tak, to przechodzi dalej i ustawia `locker = 0`, blokując dostęp dla innych wątków.  
Jeśli `locker == 0`, oznacza to, że sekcja krytyczna jest zajęta i wątek czeka w pętli `while`, dopóki nie zwolni się dostęp. Zwolnienie dostępu następuje po zakończeniu operacji w sekcji krytycznej, wątek ustawia `locker = 1`, umożliwiąc innym wątkom wejście do sekcji krytycznej.

===========================================================================================

### Program startup
1. Clone the repo:
    ```sh
   git clone https://github.com/Tesa44/so_2-project-1.git
2. Open project in CLion
3. Configure compiler
4. Build project
5. Run the program
## Descripton of the problem
Imagine five philosophers sitting around the round table. Each of them have one plate full of food, but he need two forks to eat it. Between each neighboring philosopher is one fork - so there are five of them.

Philosophers spend time on two activities: eating and thinking. If they are hungry, they try to take two forks - one on the left side, one on the right. When they get both, they start eating. After a meal they put the forks down and they start thinking.

The problem is, when several philosophers try to take forks at the same time. There is situation in which every philosopher take only one fork, and nobody can eat and put the fork down.

This is a metaphor of resource sharing in multitask systems - philosophers represent processes and forks are resources, which have to be managed properly to avoid deadlocks.

## Program
### Threads
Program uses threads from *thread* library.
Every thread represents one philosopher, who tries to eat and when it happend, he starts thinking. That means there are five threads in program, which work concurrently.
### Functions
- `philosopher(int position)` – main function executed by each thread (philosopher). In infinite loop philosopher tries to eat and next he thinks and again tries to eat.  
- `try_eat(int philosopher)` – philosopher markup himself as a hungry and checks, if he can eat. If not, he waits.  
- `think(int philosopher)` – after meal philosopher put the forks down and start thinking.  
- `check(int philosopher)` – checks, if philosopher can start eating. It means, if he has forks on the left and on the right.
### Critical sections and their solution
A critical section is a part of the code where multiple threads may attempt to access a shared resource simultaneously. In this problem, the competition is for the chopsticks placed between the philosophers.
To prevent issues caused by simultaneous access to resources, the program uses a locking mechanism:
- `lock()` - blocks access to the critical section
- `unlock()` - unblocks access after the operation is completed
#### Critical sections in the program
- Attempting to pick up a chopstick by all philosophers at the same time
- Preventing philosophers from waiting indefinitely for available chopsticks
- Avoiding multiple philosophers trying to pick up the same shared chopsticks simultaneously
#### Solution
The synchronization mechanism is based on a simple `locker` flag,
which indicates whether the critical section is available to a thread
or if another thread is already using it.
When a thread wants to enter the critical section, it checks if `locker == 1`.
If true, it proceeds and sets `locker = 0`, blocking access for other threads.
If `locker == 0`, it means the critical section is occupied, and the thread waits
in a `while` loop until access is released. After completing its operation,
the thread sets `locker = 1`, allowing other threads to enter the critical section.