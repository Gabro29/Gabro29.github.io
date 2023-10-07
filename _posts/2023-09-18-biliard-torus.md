---
layout: post
title:  "Unfolding the Billiard"
summary: 'A simulation has been developed to show the results obtained in the paper "Trajectory tracking in rectangular billiards by unfolding the billiard table" of the University of Rome Tor Vergata. In particular, the motion of a ball inside a billiard table with elastic walls can be mapped onto the surface of a torus'

author: gabro
date: '2023-08-28 1:35:23 +0530'
category: ['python', 'math']
tags: python
thumbnail: /assets/img/posts/biliard/mini_bili.png
keywords: biliard, math, python, unfolding
usemathjax: true
permalink: /blog/unfolding-the-biliard/
---

<script type="text/javascript" async
        src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.7/MathJax.js?config=TeX-MML-AM_CHTML">
</script>

# Unfolding the Billiard

**Introduzione**

È stata sviluppata una simulazione volta a mostrare i risultati ottenuti
nel paper *"Trajectory tracking in rectangular billiards by unfolding
the billiard table"* dell'Università di Roma Tor Vergata. In
particolare, il moto di una pallina all'interno di un biliardo, con
pareti elastiche, può essere mappato sulla superficie di un toroide.

**Tavolo del Biliardo**

Innanzitutto, mediante l'uso di Matplot Lib, una libreria di Python,
vengono tracciate delle linee di riferimento che individuano il
perimetro di un quadrilatero. Tale oggetto rappresenta il "campo da
gioco" della simulazione ossia il *biliardo*. Successivamente, si
definiscono le coordinate di un punto detto *spawn point*, il quale
giace all'interno del *biliardo* definito precedentemente, che
costituisce il punto di partenza della simulazione, di fatto una
pallina.

Il moto della pallina è costruito analiticamente: si fissa una direzione
arbitraria e si traccia una retta che ne indica la traiettoria. È chiaro
che, durante il moto, la pallina urterà le pareti del *biliardo*. Per
cui, piuttosto che far rimbalzare la pallina, è stato implementato
l'effetto pac-man sulle pareti, in questo modo la pallina sbucherà fuori
dalla parte opposta.

![](/assets/img/posts/biliard/image1.png){: width="600" height="300"}

La pallina sbuca fuori dalla parte opposta, senza far variare la sua direzione. 

**Costruzione delle traiettorie**

A partire dalla prima traiettoria vengono tracciate le successive sulla
base della seguente logica:

![](/assets/img/posts/biliard/image2.PNG){: width="600" height="300"}

Tale logica implementa l’effetto pac-man: il punto d’intersezione con la parete del biliardo viene traslato dalla parte opposta per disegnare la nuova traiettoria. La pendenza non viene cambiata mentre tramite formula inversa si ricava l’intercetta.

È importante notare che l'angolo d'incidenza traiettoria-parete,
rispetto alla normale della parete stessa, non può essere pari a
$$\Rightarrow \frac{\pi}{2}$$ per due motivi. Il primo è che la pallina
continuerebbe a rimbalzare da bordo a bordo (orizzontalmente o
verticalmente) e le traiettorie sarebbero tutte sovrapposte, quindi un
caso banale privo di interesse. Il secondo motivo, decisamente più
matematico, è che la pendenza di una retta è pari alla tangente
dell'angolo che essa forma con l'asse delle ascisse:

$$\Rightarrow m = {tg}(\alpha)$$

Ricordando il dominio della funzione tangente risulta chiaro che
l'angolo di incidenza non può essere mai pari a
$$\Rightarrow \frac{\pi}{2}$$ .

Inoltre, si precisa che: nel caso in cui la pallina urti perfettamente
uno degli spigoli del *biliardo*, le traiettorie sarebbero tutte
sovrapposte e quindi un ulteriore caso privo d'interesse.

**Traiettorie Dense**

Innanzitutto si osservi l'animazione riportata di seguito:

![](/assets/img/posts/biliard/image3.gif){: width="600" height="300"}

Si nota che a partire da un segmento, unendo le due estremità, si giunge
ad una circonferenza. Si passa poi ad un Toroide facendo ruotare la
circonferenza attorno ad un asse e preservandone la traccia durante il
suo moto (colorata in arcobaleno).

Tale animazione è intesa come strumento esemplificativo per comprendere
che le considerazioni relative al caso unidimensionale possono essere
estese al caso tridimensionale.

Si consideri dunque la funzione:

$$\Rightarrow \ \ T_{\theta}:\lbrack 0,1) \rightarrow \lbrack 0,1)\ con\ \theta \mathbb{\in R}.$$

$$x \mapsto \{x + \theta\}$$

Per semplicità si considera un segmento di lunghezza unitaria e si fissa
un punto di partenza $$x \in \lbrack 0,1)$$. Vale che:

$$\Rightarrow T_{\theta}^{0}(x) = x$$

$$\Rightarrow T_{\theta}^{1}(x) = T_{\theta}^{0}(x)$$

$$\Rightarrow T_{\theta}^{2}(x) = T_{\theta}^{1}( T_{\theta}^{0}(x) )$$

$$\Rightarrow T_{\theta}^{n} = ( T_{\theta} \circ T_{\theta} \circ \cdots \circ T_{\theta} )(x)$$

***Lemma 1***

Sia $$\Rightarrow T_{\theta}^{n}(x) = \{ x + n\theta \}$$ per
ogni $$n\mathbb{\in N}$$. Si dimostra per induzione.

Per $$n = 0$$, si ha per definizione che:

$$\Rightarrow T_{\theta}^{0}(x) = x$$

Al passo n si ha:

$$\Rightarrow T_{\theta}^{n}(x) = \{ x + n\theta \}$$

Si applica $$T_{\theta}$$ ambo i membri:

$$\Rightarrow T_{\theta} \circ T_{\theta}^{n}(x) = T_{\theta}( \{ x + n\theta \} )$$

$$\Rightarrow T_{\theta}^{n + 1}(x) = \{ \{ x + n\theta \} + \theta \}$$

$$= \{ (x + n\theta) - \lbrack x + n\theta\rbrack + \theta \}$$

$$= \{ x + (n + 1)\theta - \lbrack x + n\theta\rbrack \}$$

$$= \{ x + (n + 1)\theta - \lbrack x + n\theta\rbrack - \lbrack x + (n + 1)\theta - \lbrack x + n\theta\rbrack \rbrack \}$$

$$= x + (n + 1)\theta - \lbrack x + n\theta\rbrack - \lbrack x + (n + 1)\theta \rbrack + \lbrack x + n\theta\rbrack$$

$$= \{ x + (n + 1)\theta \}$$

> ◻

Si definisce inoltre l'immagine della successione
$$\{ T_{\theta}^{n}(x) \}_{n}$$ ,

l'insieme
$$\Rightarrow \tau(x) = \{ T_{\theta}^{n}(x):n\mathbb{\in N} \}$$.
Vale che $$\Rightarrow \tau(x) \subseteq \lbrack 0,1)$$.

***Teorema 1***

La successione $$\{ T_{\theta}^{n}(x) \}_{n}$$ è periodica
$$\Leftrightarrow \theta \mathbb{\in Q}$$.

DIM:

Se la successione è periodica, allora esiste un $$N\mathbb{\in N}$$ tale
che:

$$\Rightarrow T_{\theta}^{0}(x) = x = T_{\theta}^{N}(x) = x + N\theta - \lbrack x + N\theta\rbrack$$

Da cui:

$$\Rightarrow \theta = \frac{\lbrack x + N\theta\rbrack}{N}$$

Essendo $$\theta$$ dato dal rapporto di due numeri interi, allora è un
numero razionale e si può scrivere più in generale:

$$\Rightarrow \theta = \frac{N}{M}\mathbb{\in Q}$$ con $$N,M\in\mathbb{Z}, e\, N,M\neq 0$$


Per $$\theta = 0$$, si ha il caso base della definizione ed il teorema è
chiaramente verificato. Mentre per dimostrare il teorema globalmente
basta considerare anche un solo caso in cui si nota la periodicità. Per
esempio, al passo $$M$$ si avrà:

$$\Rightarrow T_{\theta}^{M}(x) = \{ x + M\theta \} = \{ x + M\frac{N}{M} \} = \{ x + N \} = \{ x \} = x = T_{\theta}^{0}(x)$$

> ◻

***Lemma 2***

Si definisce in $$\tau(x)$$ la distanza tra due punti
$$x,y \in \lbrack 0,1)$$, la quantità:

$$\Rightarrow \mathbb{d}(x,y) = min(\{ |x - y|,|x - y + 1|,|x - y - 1| \})$$

E si scrive:

$$
\Rightarrow \mathbb{d}(x,y) = \mathbb{d}( T_{\theta}^{n}(x), T_{\theta}^{n}(y) )
$$


![](/assets/img/posts/biliard/image4.gif){: width="600" height="350"}

Quando a partire dal segmento si giunge alla circonferenza, si hanno due
modi per collegare due punti: si formano due archi. La distanza è data
quindi dall'arco di lunghezza minima. La facoltà di scegliere il
percorso minore è contemplata quando si considera l'offset di $$\pm 1$$ a
seconda dei casi.

***Teorema 2***

La successione $$\{ T_{\theta}^{n}(x) \}_{n}$$ non è periodica
$$\Leftrightarrow \theta\mathbb{\in R \smallsetminus Q}$$. In altre parole
l'immagine $$\tau(x)$$ è densa in $$\lbrack 0,1)$$.

DIM:

i.  Si verifica innanzitutto che elementi di $$T_{\theta}^{n}(x)$$ siano
    tutti diversi.

Se così non fosse sarebbe:

$$\Rightarrow T_{\theta}^{n}(x) = T_{\theta}^{m}(x) \\ per \\ qualche \\ n,m\mathbb{\in N}$$

$$\Rightarrow \{ x + n\theta \} = \{ x + m\theta \}$$

$$\Rightarrow x + n\theta - \lbrack x + n\theta\rbrack = x + m\theta - \lbrack x + m\theta\rbrack$$

$$\Rightarrow \theta = \frac{\lbrack x + n\theta\rbrack - \lbrack x + m\theta\rbrack}{n - m}$$

In questo caso $$\theta$$ è dato dal rapporto di due numeri interi per cui
$$\Rightarrow \theta\mathbb{\in Q}$$. Ma si era detto che
$$\theta \in \mathbb{R \smallsetminus Q}$$. Quindi gli elementi della
successione sono tutti diversi
$$\Leftrightarrow \theta\mathbb{\in R \smallsetminus Q}$$, altrimenti si
ricadrebbe nel caso del *Teorema 1*, ossia la successione è periodica.

ii. Si verifica adesso che l'immagine $$\tau(x)$$ sia densa in
    $$\lbrack 0,1)$$.

Si divida il segmento unitario, in cui è definita la successione, in $$N$$
intervallini di lunghezza $$\frac{1}{N}$$. Se si considera il fatto che la
successione parte dal passo $$T_{\theta}^{0}(x)$$ ed arriva al paso
$$T_{\theta}^{n}(x)$$, allora saranno generati $$N + 1$$ numeri. Affinché
questi stiano all'interno della precedente suddivisione, deve essere che
almeno un intervallino ne contenga almeno due. Siano questi:

$$
\Rightarrow T_{\theta}^{n_{0}}(x) \quad \text{e} \quad T_{\theta}^{m_{0}}(x) \quad \text{con} \quad n_{0}, m_{0} \in \{0, 1, 2, \ldots, N\}
$$


Vale quindi che:

$$\Rightarrow | T_{\theta}^{n_{0}}(x)\  - \ T_{\theta}^{m_{0}}(x) | < \frac{1}{N}$$

Tenendo conto del *Lemma 2* si ottiene che:

$$\Rightarrow \mathbb{d}( \ T_{\theta}^{m_{0}}(x),\ T_{\theta}^{n_{0}}(x) ) < \frac{1}{N}$$

Si applica $$T_{\theta}^{- m_{0}}$$ (con $$n_{0} > m_{0}$$):

$$
\Rightarrow \mathbb{d}(T_{\theta}^{-m_0}(x) \circ T_{\theta}^{m_0}(x), T_{\theta}^{-m_0} \circ T_{\theta}^{n_0}(x)) < \frac{1}{N}
$$

$$\Rightarrow \mathbb{d}( x,T_{\theta}^{n_{0} - m_{0}}(x) ) < \frac{1}{N}$$

Si consideri ora la successione:

$$
\Rightarrow \{x_{j}\}_{j} = \{T_{\theta}^{j(n_{0} - m_{0})}(x)\}
$$



Si nota che questa è una sotto-successione di
$$\{ {T_{\theta}^{n}}(x) \}_{n}$$.

Si applichi ora il *Lemma 2* tra due punti della sotto-successione:

$$\Rightarrow \mathbb{d}( T_{\theta}^{j( n_{0} - m_{0} )}(x),T_{\theta}^{(j + 1)( n_{0} - m_{0} )}(x) )$$

$$= \mathbb{d}( T_{\theta}^{j( n_{0} - m_{0} )}(x),T_{\theta}^{j( n_{0} - m_{0} )}( T_{\theta}^{n_{0} - m_{0}}(x) ) )$$

Si applica $$T_{\theta}^{- j( n_{0} - m_{0} )}$$:

$$= \mathbb{d}( x,T_{\theta}^{n_{0} - m_{0}}(x) ) < \frac{1}{N}$$

Quindi si è ottenuta una sotto-successione che ha un passo più breve di quella di partenza. Poiché è possibile scegliere $$N$$ grande quanto si vuole, allora anche il passo può essere piccolo quanto si vuole.

$$\Rightarrow \tau(x)$$ è densa in $$\lbrack 0,1)$$.

**Mappatura sul Toroide**

Un Toroide può essere definito mediante le seguenti equazioni
parametriche:

$$
\begin{cases}
x(\omega, \varphi) = (R + r \cos(\omega)) \cos(\varphi) \\
y(\omega, \varphi) = (R + r \cos(\omega)) \sin(\varphi) \\
z(\omega, \varphi) = r \sin(\omega)
\end{cases}
\quad \text{con } \omega, \varphi \in [0, 2\pi)
$$


Dove $$\omega$$, *φ* sono gli angoli di rotazione, *r*  è il raggio del
tubo del Toroide, *R*  è la distanza dal centro del tubo al centro di
rotazione del Toroide.

Il motivo per il quale si giunge al Toroide è dato dalla seguente
animazione esemplificativa:

![](/assets/img/posts/biliard/image5.gif){: width="600" height="350"}

È possibile ottenere un Toroide a partire da un rettangolo se si incollano tra di loro il bordo superiore e inferiore e poi quello destro e sinistro

Ricordando che la traiettoria della pallina è costruita mediante rette,
e che queste vengono disegnate operativamente unendo dei punti tra di
loro, è possibile sfruttare tali punti per mappare una traiettoria
curvilinea sul Toroide. Passando quindi da un grafico in 2D ad uno
tridimensionale mediante le seguenti proporzioni:

$$
\begin{cases}
\theta : 2\pi = x : b \\
\varphi : 2\pi = y : h \\
\end{cases}
$$


$$
\begin{cases}
\theta = \frac{2\pi \cdot x}{b} \\
\varphi = \frac{2\pi \cdot y}{h}
\end{cases}
$$


Dove *b* e *h* sono rispettivamente la base e l'altezza del rettangolo.
Infine sfruttando le coordinate parametriche di prima si costruisce il
Toroide.

**Toroide denso**

Si consideri un punto $$( x_{0},y_{0} ) \in \lbrack 0,1)^{2}$$
e due numeri
$$\alpha, \beta \in \mathbb{R} \setminus \{0\}$$.

Quindi si definisce la mappa:

$$\Rightarrow \phi\mathbb{:R \rightarrow}\lbrack 0,1)^{2}$$

$$t \mapsto ( \{ x_{0} + \alpha t \},\{ y_{0} + \beta t \} )$$

Tale mappa rappresenta un moto di natura continua sul Toroide, con
velocità:

$$\Rightarrow \overrightarrow{v} = (\alpha,\beta)$$

Per semplicità si scrive:

$${\Rightarrow \phi}_{1}(t) = \{ x_{0} + \alpha t \}$$

$${\Rightarrow \phi}_{2}(t) = \{ y_{0} + \beta t \}$$

$$\Rightarrow \phi(t) = ( \phi_{1}(t),\phi_{2}(t) )$$

***Teorema 3***

$$\phi(t)$$ è periodica
$$\Leftrightarrow \frac{\alpha}{\beta}\mathbb{\in Q}$$.

DIM:

Se $$\phi(t) = ( x_{0},y_{0} )$$ allora
$${\Rightarrow \phi}_{1}(t) = x_{0}$$ e
$${\Rightarrow \phi}_{2}(t) = y_{0}$$.

Dalla prima si ha che:

$$\Rightarrow \{ x_{0} + \alpha t \} = x_{0}$$

$$\Rightarrow x_{0} + \alpha t - \lbrack x_{0} + \alpha t \rbrack = x_{0}$$

$$\Rightarrow t = \frac{\lbrack x_{0} + \alpha t \rbrack}{\alpha}$$

O più in generale:

$$\Rightarrow t = \frac{n}{\alpha}$$ con
$$n \in \mathbb{Z} \setminus \{0\}$$

Dalla seconda si ha che:
$$\Rightarrow \{ y_{0} + \beta t \} = y_{0}$$

$$\Rightarrow y_{0} + \beta t - \lbrack y_{0} + \beta t \rbrack = y_{0}$$

$$\Rightarrow t = \frac{\lbrack y_{0} + \beta t \rbrack}{\beta}$$

O più in generale:

$$\Rightarrow t = \frac{m}{\beta}$$ con
$$m\mathbb{\in Z \smallsetminus}{ 0 }$$

Essendo che la $$t$$ è la stessa in entrambi i casi, si possono eguagliare
le equazioni:

$$\Rightarrow t = \frac{n}{\alpha} = \frac{m}{\beta}$$

$$\Rightarrow \frac{\alpha}{\beta} = \frac{n}{m}\mathbb{\  \in Q}$$

Quindi preso $$t = \frac{n}{\alpha}$$ (ma lo stesso avviene con
$$t = \frac{m}{\beta}$$ ) risulta che:

$$\Rightarrow \phi(t) = ( \{ x_{0} + n \},\{ y_{0} + \frac{\beta}{\alpha} \} )$$

$$= ( \{ x_{0} + n \},\{ y_{0} + \frac{m}{n} \} ) = ( x_{0},y_{0} )$$
per ogni $$n,m\mathbb{\in Z \smallsetminus}{ 0 }$$

***Teorema 4***

$$\phi(t)$$ riempie densamente $$\lbrack 0,1)^{2}$$
$$\Leftrightarrow \frac{\alpha}{\beta}\mathbb{\in R \smallsetminus Q}$$.

DIM:

Sia $$( x_{1},y_{1} ) \in \lbrack 0,1)^{2}$$, essendo
$$\beta > 0$$ supponiamo sia
$${\Rightarrow \phi}_{2}( t_{0} ) = y_{1}$$.

Allora si avrà ad un istante di tempo successivo che:

$$\Rightarrow \phi_{2}( t_{0} + \frac{n}{\beta} ) = \{ y_{0} + \beta t_{0} + n \} = \{ y_{0} + \beta t_{0} \}$$

$$= {\phi_{2}( t_{0} ) = y}_{1}$$ per ogni $$n\mathbb{\in Z}$$

In altre parole, ad intervalli di tempo $$\frac{1}{\beta}$$ la traiettoria
della pallina interseca la retta $$y = y_{1}$$.

Considerando la coordinata $$\phi_{1}( t_{0} )$$ al tempo
$$\frac{n}{\beta}$$ , si ha che:

$${\Rightarrow \phi}_{1}( t_{0} + \frac{n}{\beta} ) = \{ x_{0} + \alpha t_{0} + {n \cdot \ }_{\overline{\beta}}^{\alpha} \} = T_{\frac{\alpha}{\beta}}^{n}( x_{0} + \alpha t_{0} )$$

Dal *Teorema 2* è noto che
$$\{ T_{\frac{\alpha}{\beta}}^{n}( x_{0} + \alpha t_{0} ) \}_{n}$$ è
densa in $$\lbrack 0,1)$$ se
$$\frac{\alpha}{\beta}\mathbb{\in R \smallsetminus Q}$$.

Se ne conclude che:

$$\mathbb{N} \ni n \mapsto \phi( t_{0} + \frac{n}{\alpha} ) \in \lbrack 0,1)^{2}$$

Riempie densamente il segmento $$( x,y_{1} )$$ con
$$x \in \lbrack 0,1)$$.

Ma allora $$t \mapsto \phi(t)$$ si avvicina quanto si vuole anche a
$$( x_{1},y_{1} )$$.

**Relazione tra Biliardo e Intervallo**

Sulla base dei precedenti teoremi è possibile ora mettere in relazione
$$\frac{\alpha}{\beta}$$ del caso tridimensionale, con il passo $$\theta$$
del caso unidimensionale.

Si ricorda che la traiettoria della pallina viene definita impostando
una determinata pendenza, e che se non ci fosse alcuna parete si avrebbe
una retta. Le limitazioni del calcolatore trattano tale retta come un
grande segmento, in particolare tale segmento rappresenta l'ipotenusa di
un triangolo rettangolo. Il passo $$\theta$$ del caso unidimensionale può
essere calcolato considerando la proiezione dell'ipotenusa sull'asse
delle ascisse.

$$\Rightarrow \theta = i\cos\gamma$$

Dove $$\gamma$$ è l'angolo che la retta forma con l'asse delle ascisse.

Per quanto riguarda la pendenza, si ricorda che:

$$\Rightarrow \overrightarrow{v} = (\alpha,\beta)$$

Per cui se si considera l'equazione della traiettoria si deduce che:

$$\Rightarrow f^{'}(x) = m = \beta$$

Un'ulteriore conferma di ciò si può ricavare dal *Teorema 4*:

ad intervalli di tempo $$\frac{1}{\beta}$$ la traiettoria della pallina
interseca

la retta $$y = y_{1}$$. Dunque si può ricavare $$\beta$$ come:

$$\Rightarrow \beta = \frac{1}{\theta}$$

Infine $$\alpha$$ può essere ricavato in due modi differenti: considerando
il reciproco della proiezione dell'ipotenusa sull'asse delle ordinate,
oppure considerando che $$\frac{\alpha}{\beta} = \theta$$.

![](/assets/img/posts/biliard/image6.PNG){: width="600" height="300"}

**Simulazioni**

Di seguito si riporta una simulazione relativa al
caso di moto periodico.

![](/assets/img/posts/biliard/image7.gif){: width="600" height="300"}

Il moto periodico produce una linea chiusa sul Toroide. Il punto di spawn della pallina è (0.2,0.4), mentre la pendenza è 0.2.


Si riportano inoltre i valori che caratterizzano tale simulazione.

| Intersezione | **m** | **α** | **β** | **θ** |
|--------------|-------|-------|-------|-------|
| (0.2, 0.4)   | 0.2   | 1     | 0.2   | 5.0   |
| (0.2, 0.4)   | 0.2   | 1     | 0.2   | 5.0   |
| (0.2, 0.4)   | 0.2   | 1     | 0.2   | 5.0   |
| (0.2, 0.4)   | 0.2   | 1     | 0.2   | 5.0   |
| (0.2, 0.4)   | 0.2   | 1     | 0.2   | 5.0   |
| (0.2, 0.4)   | 0.2   | 1     | 0.2   | 5.0   |


Di seguito si riporta una simulazione relativa al caso di moto non periodico.

![](/assets/img/posts/biliard/image8.gif){: width="600" height="300"}

Il moto non periodico riempie densamente il Toroide. Il punto di spawn della pallina è (0.2,0.4), mentre la pendenza è π.

Si riportano inoltre alcuni valori che caratterizzano tale simulazione.

| Intersezione   | **m** | **α** | **β** | **θ** |
|----------------|-------|-------|-------|-------|
| (0.882, 0.4)   | $$\pi$$ | 1     | $$\pi$$ | 0.31830988618379086 |
| (0.563, 0.4)   | $$\pi$$ | 1     | $$\pi$$ | 0.31830988618379086 |
| (0.245, 0.4)   | $$\pi$$ | 1     | $$\pi$$ | 0.31830988618379086 |
| (0.927, 0.4)   | $$\pi$$ | 1     | $$\pi$$ | 0.31830988618379086 |
| (0.608, 0.4)   | $$\pi$$ | 1     | $$\pi$$ | 0.31830988618379086 |
| (0.290, 0.4)   | $$\pi$$ | 1     | $$\pi$$ | 0.31830988618379086 |


**Riferimenti bibliografici**

| Fonti Utilizzate                                                      |
|-----------------------------------------------------------------------|
| Appunti presi durante le lezioni di Analisi I tenute 
|dal Professore Matteo Dalla Riva                                                     
| Paper scientifico di riferimento: *https://www.sciencedirect.com/science/article/pii/S240589632032317X* 
| Codice sorgente:                                                      |
