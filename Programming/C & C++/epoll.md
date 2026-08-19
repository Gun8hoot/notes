- Le I/O multiplexing permet a un processus de gerer plusieurs fds en meme temps. Les syscall comme `poll()` et `select()` permettent les I/O multiplexing
![comp](socket_comparison.png)
- `epoll()` a de meilleurs performance lorsque l'on monitor une grande quantiter de fds, cepandant il est dispo que sur les kernel Linux >= 2.6 (bsd et macos ont des alternative mais pas `epoll()`)

---

- On creer des instance `epoll` avec `epoll_create()`
```c
#include <sys/epoll.h>
int epoll_create(int size);
```
- **size**: Est le nombre de FD que l'on pense gerer via notre instance epoll
- Il va retourne un fd qui est lier a notre instance `epoll()`
- Lorsque l'on a fini avec notre instance, nous devons le fermer avec `close()`

---

- On ajoute, retire et liste des fd de la liste avec `epoll_ctl()`
```c
#include <sys/epoll.h>
int epoll_ctl(int epfd, int op, int fd, struct epoll_event *ev);
```
-  **epfd**: Fd de notre instance
- **op**: Est un indicatif sur ce que l'on veux faire sur ce fd genre `EPOLL_CTL_ADD` pour ajouter un fd a notre liste, `EPOLL_CTL_MOD` pour modifier les paramettre de l'event
- **fd**: Est le fd que l'on veux ajouter, retirer ou modifier
- **ev**: Structure qui definit les parametre pour notre **fd**
- Retourne 0 en cas de success et -1 en cas d'erreur

---

- `epoll_wait()` nous permet de nous donner des info si il y a eu des I/O sur un des fd a surveiller 
```c
#include <sys/epoll.h>
int epoll_wait(int epfd, struct epoll_event *evlist, int maxevents, int timeout);
```
-  **epfd**: Fd de notre instance
- **evlist**: Est un array de structure de la taille de **maxevents** qui doit etre allouer avant de le donner a la fonction
- **maxevents**: Est la taille de notre tableau **evlist**
- **timeout**: Est le nombre de ms ou l'on va check si il y a eu un fd qui a trigger un event. Si le timeout est egal a -1 la fonction est rendu bloquante, si il est a 0 la fonction check et direct return qu'il aille vu un fd qui a trigger un event ou non.
- Retourne le nombre de fd qui ont trigger un event, 0 si rien ne c'est passer et -1 en cas d'erreur

---

- Les option mis `events` de la structure `struct epoll_event` lors du `epoll_ctl()` est mis dans `evlist[].events` lors du `epoll_wait()`