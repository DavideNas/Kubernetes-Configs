### Kubernetes-Configs

Una panoramica dei possibili file di configurazione in **k8s**

---
**Cluster IP**

Un Cluster IP è un servizio che implementa un indirizzo ip diretto ad un pod.
Questo è configurato internamente e non è possibile accedere dall'esterno.
Il Cluster IP permette ai pods di comunicare tra loro e sono configurabili tramite il nome
diretto del servizio 

ad esempio :
**event-bus-srv** sarà accessibile tramite

```sh
http://event-bus-srv:4000/events
```
il nome identifica il servizio e lo codifica in un ip tramite un DNS interno a k8s.

---
**NodePort**

Un NodePort è un servizio usato per esporre il **cluster k8s** al traffico esterno.
Di base non fa altro che aprire una porta (random o definita tramite l'etichetta `nodePort`) nelle VM in esecuzione.
Questo traffico in entrata viene poi diretto al servizio che reindirizzerà le richieste provenienti dall'esterno ai pods del cluster.
> **NodePort** è limitato quindi consigliabile solo in fase di sviluppo o testing

* Puoi avere un solo servizio per porta
* Puoi utilizzare solo le porte 30000–32767
* Se l'indirizzo IP del tuo nodo/VM cambia, devi occupartene

---
**LoadBalancer**

E' il servizio migliore per esporre un cluster al traffico esterno.
Non ha limiti di o restrizioni (dovuti da filtri e routing).
Questo permette di incanalare traffico di qualsiasi tipo come : **HTTP** , **TCP** , **UPD** , **Websockets** , **gRPC** o qualsiasi altro tipo.
> Il limite maggiore è dovuto dal costo. Ciascun **LoadBalancer** è acquistato separatamente.

Ciò può rendere il costo elevato se si tratta di un app di microservizi.

---
**Ingress**

**Ingress** non è un servizio come gli altri ma più un assemblatore di tutti i servizi messi assieme.
E' considerato come uno "smart routing" per via del fatto che espone più servizi sotto lo stesso indirizzo IP.
Esistono diversi tipi di "Ingress Controller" e quello di base avvierà in automatico un servizio di LoadBalancer.
> Può essere complicato da usare ma è anche il più versatile

I diversi tipi di Ingress possono provenire da:
* GCP →  **GKE** (Google Kubernetes Engine) **Ingress**
* AWS → **ALB** (Application Load Balancer) **Ingress Controller**
* Azure → **AGIC** (Application Gateway Ingress Controller)
* ingress-nginx 
* Contour
* Istio

Alcuni Plugin possono aggiungere caratteristiche interessanti come **cert manager** che si occupa di aggiungere un certificato SSL ai tuoi servizi.
SSL, Auth, Routing sono solo alcuni esempi di caratteristiche facilmente integrabili grazie ad **Ingress**.

Tipicamente si basano tutti sullo stesso protocollo L7 (ovvero **HTTP**).
