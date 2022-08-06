---
title: Consomme
updated: 2021-12-29 16:16:41Z
created: 2021-12-29 16:04:17Z
latitude: 47.54460000
longitude: -2.89140000
altitude: 0.0000
tags:
  - concurrence
  - s1
---

```C
#include <unistd.h> // appel systeme fork
#include <fcntl.h>  // appel system unix ES
#include <stdio.h> // librairie standard C
#include <stdlib.h> // exit
#include <sched.h>  // sche_yield
#include <sys/types.h>
#include <sys/sem.h> // semaphore IPC
#include <sys/ipc.h> // services IPC
#include <sys/wait.h> // wait
#define MUTEX 0
#define PLACE 1
#define ARTICLE 2

key_t cle; /* cle ipc */
int semid; /* nom local de l'ensemble des semaphores */

#define MAX 10 // taille du buffer

int objet_value = 0;
int nombre_objets = 0;
int tableau_articles[MAX];
int chiffre = 0;



void produire(int * objet) 
{
    //TODO: créer l'objet
//   *objet = objet_value;
//   objet_value++;
}


void deposer(int objet) 
{
    //TODO: déposer l'objet dans le tableau et écrire sur le fichier
  //déposer l'objet dans le tableau et écrire sur le fichier
//   fic2tab("db.txt",tableau_articles,sizeof(tableau_articles));
//   nombre_objets = tableau_articles[0];
//   tableau_articles[nombre_objets+1] = objet;
//   nombre_objets++;
//   tableau_articles[0] = nombre_objets;
//   printf("je depose %d en %d\n", objet, nombre_objets);
//   tab2fic("db.txt",tableau_articles,sizeof(tableau_articles));
//   print_array(tableau_articles,sizeof(tableau_articles));
}



void extraire(int * objet)
{
    //TODO: extraire 
//   fic2tab("db.txt",tableau_articles,sizeof(tableau_articles)); 
//   nombre_objets = tableau_articles[0];
//   if (nombre_objets > 0) {
//       *objet = tableau_articles[1];
//       for (int i = 1; i < 9; i++) {
//           tableau_articles[i] = tableau_articles[i+1];
//       }
//        nombre_objets--;
//        tableau_articles[0] = nombre_objets;
//        tab2fic("db.txt",tableau_articles,sizeof(tableau_articles));
//   }

}



void consommer(int objet) {
    //TODO: consommer l'objet
    printf("je consomme %d\n",objet);
}

void producteur(void){
//producteur
    int i = 0;
    int objet = 0;
    while (i < 10){
        produire(&objet);
        struct sembuf op; // operation sur un semaphore    
        /*P(&places); on demande une place pour pouvoir déposer l'objet*/
        op.sem_num=PLACE;op.sem_op=-1;op.sem_flg=0;
        semop(semid,&op,1);

        /*P(&mutex); on demande l'accès au MUTEX */
        op.sem_num=MUTEX;op.sem_op=-1;op.sem_flg=0;
        semop(semid,&op,1);
        
        //TODO: deposer(objet);

        /*V(&mutex); on libère le MUTEX*/
        op.sem_num=MUTEX;op.sem_op=1;op.sem_flg=0;
        semop(semid,&op,1);
        /*V(&atricles); on ajoute un article*/
        op.sem_num=ARTICLE;op.sem_op=1;op.sem_flg=0;
        semop(semid,&op,1);	
        usleep(10000);
        i++;
    }

}


void consommateur(void){
//consommateur
    int i = 0;
    int objet = 0;
    while (i <= 10){
        struct sembuf op; // operation sur un semaphore
        /*P(&articles); on veut obtenir un article */
        op.sem_num=ARTICLE;op.sem_op=-1;op.sem_flg=0;
        semop(semid,&op,1);
        /*P(&mutex); on demande l'accès au MUTEX */
        op.sem_num=MUTEX;op.sem_op=-1;op.sem_flg=0;
        semop(semid,&op,1);

        //TODO: extraire(&objet);

        /*V(&mutex); on demande l'accès au MUTEX */
        op.sem_num=MUTEX;op.sem_op=1;op.sem_flg=0;
        semop(semid,&op,1);	
        /*V(&place); on ajoute une place vu que l'article est récupéré*/
        op.sem_num=PLACE;op.sem_op=1;op.sem_flg=0;
        semop(semid,&op,1);

        //TODO: consommer(objet);
        i++;
        usleep(10000);
    }
}

// programme principal
int main ( int argc , char **argv ) {
    int pid; // numero des fils
    ushort init_sem[3]={1,MAX,0}; //initialise le semaphore mutex
   
   // creation d'une cle IPC en fonction du nom du programme
    if ((cle=ftok(argv[0],'0')) == -1 ) {
        fprintf(stderr,"Probleme sur ftoks\n");
        exit(1);
    }
   // demande un ensemble de semaphore (ici un seul mutex)
    if ((semid=semget(cle,3,IPC_CREAT|0666))==-1) {
        fprintf(stderr,"Probleme sur semget\n");
        exit(2);
    } 
   // initialise l'ensemble
    if (semctl(semid,3,SETALL,init_sem)==-1) { 
        fprintf(stderr,"Probleme sur semctl SETALL\n");
        exit(3);
    }
    //TODO: initialiser le tableau
    // 	system("echo "" > db.txt "); 
    // print_array(tableau_articles,sizeof(tableau_articles));
    // tab2fic("db.txt",tableau_articles,sizeof(tableau_articles));
    if (fork() == 0) { 
        producteur();
        exit(0);
    }
    if (fork() == 0) { 
        consommateur();
        exit(0);
    }

    int i = 1;
    for (i=1 ;i < argc; i++){
        int message;
        pid=wait(&message);
    } 
    semctl(semid,0,IPC_RMID,0); // supprime le semaphore
}
```