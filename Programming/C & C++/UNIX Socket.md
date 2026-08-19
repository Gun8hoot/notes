- Un socket est un FD qui sert a communiquer a distance 
- Il existe plusieurs type de socket, mais le plus souvent on utilise des socket internet
	- `SOCK_STREAM` : Socket en TCP permet de garantir l'integriter des données transmisent grace a un system nommee "three way handshake"
	- `SOCK_DGRAM` : Socket en UDP permet d'envoyer et recevoir des donnee a grande vitesse, cepandant ont ne peux pas savoir si des donnee ont ete alterer durant le transite

- Avant de continuer, il est important de bien avoir compris le principe d'[[Eldianness]]
- La librairie **`<arpa/inet.h>`** nous met a disposition des fonctions pour convertir les octet dans le bon format si besoin que l'on soit ayent des données en LSB ou MSB.
```c
uint32_t htonl(uint32_t hostlong);  //"Host to network long"
uint16_t htons(uint16_t hostshort); //"Host to network short"
uint32_t ntohl(uint32_t netlong);   //"Network to host long"
uint16_t ntohs(uint16_t netshort);  //"Network to host short"
```

---

- La librarie **`<netinet/in.h>`** fourni un ensemble de structure pour les sockets
```c
// Pour une adresse IPv4 uniquement
// (voir sockaddr_in6 pour IPv6)
struct sockaddr_in {
    sa_family_t    sin_family;
    in_port_t      sin_port;
    struct in_addr sin_addr;
};

struct in_addr {
    uint32_t       s_addr;
};
```
- **sin_family** est la famille d'addresse d'ip qui est utiliser (IPv4 ou IPv6)
- **sin_port** : Le port où l'on se cherche a se connecter. Attention il mettre le port dans le format de l'ordre reseau et non de l'host. Il faut donc utiliser **`htons()`** pour faire la convertion (on utilise la version short de hton car il existe que 65535 port different, sur 2 bytes)
- **sin_addr** : Structure de type `in_addr` qui va contenir l'addresse ip sous forme d'int que l'on va stocker dans **s_addr**. **(strct->sin_addr->s_addr)**. Il faut egalement convetir l'ip dans l'ordre des bytes reseau donc convertir avec **`htonl()`** car une ip est sur 4 bytes.
	- Il existe des constante comme :
		- `INADDR_LOOPBACK` : Le socket va envoyer ou recevoir sur 127.0.0.1
		- `INADDR_ANY` : Le socket va envoyer ou recevoir sur 0.0.0.0
		- `INADDR_BROADCAST` : Le socket va envoyer ou recevoir sur 255.255.255.255

---

- Pour nous devons convertir notre ip sous la forme de "xx.xx.xx.xx" en nombre avec la fonction `inet_pton()`, cette fonction est dans la librairie **`<arpa/inet.h>`**
```c
int inet_pton(int af, const char * src, void *dst);
```
- af : Est de nouveau le int qui represente la famille d'addresse ip qui est utiliser donc soit **AF_INET** ou **AF_INET6**
- src : Est l'ip a convertir sous forme de string
- dst: Est l'addresse de notre structure **`in_addr`**
- **`inet_pton()`** renvoie 1 en cas de success, 0 si la structure n'est pas valide et -1 si la famille mis dans **af** est pas valide
```c
// IPv4 uniquement
struct sockaddr_in sa;
inet_pton(AF_INET, "216.58.192.3", &sa.sin_addr);
```
- La fonction inverse **`inet_ntop()`** existe egalement pour avoir l'ip sous forme de string

---

- Pour obtenir l'ip associer a un nom de domaine on peux utiliser `getaddrinfo()` qui est dans la lib `<netdb.h>`
- En allant checher l'addresse ip depuis les serveur DNS
```c
int getaddrinfo(const char *node, const char *service,
                const struct addrinfo *hints,
                struct addrinfo **res);
```
- node: Represente l'addresse ip ou le nom de domaine que le veux chercher
- service: Est le port ou l'on va se connecter
- hints: Pointeur vers une structure en `addrinfo` qui va agir en temps que filtre de recherche
- res: Pointeur vers la structure de type `addrinfo` ou le resultat de notre getaddrinfo est mis
- La fonction `getaddrinfo()` renvoie 0 en cas de succes ou un autre code d'erreur que l'on peux traduire avec `gai_strerror()`
- Nous devons librer la memoire de notre structure **`addrinfo`** grace a la fonction `freeaddrinfo()`

---

```c
struct addrinfo {
    int              ai_flags;
    int              ai_family;
    int              ai_socktype;
    int              ai_protocol;
    size_t           ai_addrlen;
    struct sockaddr *ai_addr;
    char            *ai_canonname;
    struct addrinfo *ai_next;
};
```
- **ai_flags**: Va contenir toutes les options de notre scan
- **ai_family**: Contient la famille d'addresse internet (IPv4 = AF_INET, IPv6 = AF_INET6)
- **ai_socktype**: Type de socket desirer (TCP = SOCK_STREAM, UDP = SOCK_DGRAM). On peux lui mettre 0 pour qu'il essaye de chercher le type tous seul
- **ai_protocol**: protocole de l'addresse du socket, autrement dit de nouveau si c'est du TCP ou UDP. On peux mettre 0 et le `getaddrinfo()` va renvoyer le type de protocole
- **ai_addrlen**: Longeur de notre addresse ip. Est remplis par `getaddrinfo()`
- **ai_addr**: Pointeur vers une structure en **`ai_addr()`** qui va etre remplis par `getaddrinfo()` et contenir l'IP
- **ai_canonname**: Est utiliser si le flag `AI_CANONNAME` est renseigner. Sert a avoir le nom officiel de notre host
- **ai_next**: prochain maillons de notre chaine
- Nous devons utiliser **`socket()`** en important depuis **`<sys/socket.h>`**

---

- Permet de creer un nouveau fd qui sera utiliser pour notre socket
```c
int socket(int domain, int type, int protocol);
```
- domain: Est la famille de protocole qui est utiliser (`AF_INET`/`AF_INET6`)
- type: Type de socket (`SOCK_STREAM`/`SOCK_DGRAM`)
- protocol: Protocole a utiliser, pour les socket en SOCK_STREAM et en SOCK_DGRAM il existe que 1 protocole valide par type de socket, donc on peux mettre a 0

---

- On se connect a un serveur distant avec la fonction `connect()`
```c
int connect(int sockfd, const struct sockaddr *serv_addr,
            socklen_t addrlen);
```
- **sockfd**: fd de notre socket obtenu grace a au syscall **`socket()`**
- **serv_addr**: Pointeur vers notre structure en `sockaddr_in`
- **addrlen**: Taille en octet de la structure **`sockaddr_in`**

---

- On lie un socket a une ip et a un port grace a `bind()`
```c
int bind(int sockfd, const struct sockaddr *addr, socklen_t addrlen);
```
- **sockfd**: fd de notre socket obtenu grace a au syscall **`socket()
- **addr**: Pointeur vers la structure **`sockaddr`**  qui va contenir l'ip 
- **addrlen**: Taille de notre structure en octet
- Renvoie 0 en cas de success et -1 en cas d'erreur avec un `errno`

---

- On marque notre socket en passif pour qu'il puissent accepter toutes les demande de connection entrante
```c
int listen(int sockfd, int backlog);
```
- **sockfd**: fd de notre socket obtenu grace a au syscall **`socket()
- **backlog**: Int qui represente le nombre de connection maximal autoriser dans la file d'attente
- Renvoie 0 en cas de success et -1 en cas d'erreur avec un errno


---

- Permet d'accepter les connection entrante 
```c
int accept(int sockfd, struct sockaddr *addr, socklen_t *addrlen);
```
- **sockfd**: fd de notre socket obtenu grace a au syscall **`socket()
- **addr**: Pointeur vers la structure **`sockaddr`**  qui va contenir l'ip 
- **addrlen**: Taille de notre structure en octet
- Il renvoie un fd avec lier a la nouvelle corrrection ou -1 en cas d'erreur

---

- On envoie des donnee grace a `send()`
```c
ssize_t send(int socket, const void *buf, size_t len, int flags);
```
- **socket**: fd de notre connection obtenu grace a accept
- **buf**: Contenu a envoye
- **len**: Taille de notre buff
- **flags**: Int qui contient les options de transimision du message
- La fonction retourne le nombre d'octet qui a ete envoyer ou -1 en cas d'erreur
- Les socket datagram (UDP) utilisent la fonction `sendto()`

---

- On recois des donne grace a la fonction `recv()`
```c
ssize_t recv(int socket, void *buf, ssize_t len, int flags);
```
- **socket**: fd du socket ou l'on veux recevoir les donnee (pour le client le fd recup avec `socket()`, coter server le fd recup avec `accept()`)
- **buf**: Zone memoire ou le contenue du message recu sera mis 
- **len**: Taille maximal du buffer mis dans `buf`
- **flags**: Flag de parametre pour la reception du message
- `recv()` renvoie le nombre d'octet recuperer, si c'est 0 ça peux vouloir dire que l'host distant a ete fermer la connection
- Les socket datagram (UDP) utilisent la fonction `recvfrom()`

---

- Pour fermer le socket on utilise a fonction `shutdown()`
```c
int shutdown(int sockfd, int how);
```
- **sockfd**: fd du socket a fermer.
- **how**: Flag qui dit quoi fermer (**SHUT_RD**, **SHUT_WR** ou **SHUT_RDWR**)
- Renvoie 0 en cas de success et -1 en cas d'erreur

--- 

- Les fonction comme `accept()` et `recv()` sont bloquantes ce qui veux dire que l'on reste dans notre fonction tant que la fonction n'a pas fini d'etre executer. Dans le cas de notre serveur, imaginons que nous avions 3 client. En non bloquant si notre client nous a pas envoyerr de donnees nous allons rester dans le recv tant qu'il nous ne nous envoye pas de donnees
- Nous pouvons rendre notre socket non bloquant grace a la fonction `fcntl()`
```c
socket_fd = socket(PF_INET, SOCK_STREAM, 0);
fcntl(socket_fd, F_SETFL, O_NONBLOCK);
```
- Si on prend un example avec `recv()` il va renvoyer -1 et mettre le errno a `EAGAIN` ou `EWOULDBLOCK` 
- Mais utiliser fcntl dans ce cas n'est conseiller car le programme va faire enormement dope ce qui tire enormement sur le CPU. Il est mieux d'utiliser des syscall comme poll()/epoll()/select()
- De ce que j'ai vu epoll est mieux plus opti (+ conseiller par sbonneau) donc je pense que c'est le mieux a prendre dans notre cas. Meme si c'est un peu chiant a prendre en main

## SOURCES:
- [Codequoi](https://www.codequoi.com/programmation-reseau-via-socket-en-c/#quest-ce-quune-socket-)
- [Wikipedia socket](https://en.wikipedia.org/wiki/Network_socket)
- [The Linux Programming Interface]()
	- I/O multiplexing : page 1369 du pdf