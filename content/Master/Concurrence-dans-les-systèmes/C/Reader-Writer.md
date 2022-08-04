---
title: Reader/writer
updated: 2021-12-29 16:46:05Z
created: 2021-12-29 16:16:50Z
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
#include<signal.h> // signal
#include<sys/time.h>
#include <sys/shm.h> // segment partage IPC
void tueurDeRoulette(int sig) {
    fin = 1;
    return;
}

#define MUTEX 0
#define READER 1
#define WRITER 2

key_t cle; /* cle ipc */
int semid; /* nom de l'ensemble des semaphores */
int shmid; /* nom du segment partage */
short fin; 
int lecteur;
int demandeLecteur;
int redacteur;
int demandeRedacteur;


void lecteurs(int nbRouleau, int*tab) {
//reader
    int i = 0;
    while(!fin) {
        struct sembuf op; // operation sur un semaphore
        //P(&MUTEX);
        op.sem_num=MUTEX;op.sem_op=-1;op.sem_flg=0;
        semop(semid,&op,1);
        if (redacteur || demandeRedacteur) {
            demandeLecteur++;
            //V(&MUTEX);
            op.sem_num=MUTEX;op.sem_op=1;op.sem_flg=0;
            semop(semid,&op,1);
            //P(&LECTEUR)
            op.sem_num=READER;op.sem_op=-1;op.sem_flg=0;
            semop(semid,&op,1);
            //P(&MUTEX);
            op.sem_num=MUTEX;op.sem_op=-1;op.sem_flg=0;
            semop(semid,&op,1);
            demandeLecteur--;

        }
        lecteur++;
        //V(&MUTEX);
        op.sem_num=MUTEX;op.sem_op=1;op.sem_flg=0;
        semop(semid,&op,1);

        // TODO: acces à la ressource
        // for (int i = 0; i < nbRouleau; i++)
        //     printf("%d",tab[i]);
        // printf("\r");
        // fflush(stdout);


        //P(&MUTEX);
        op.sem_num=MUTEX;op.sem_op=-1;op.sem_flg=0;
        semop(semid,&op,1);
        lecteur--;
        if (lecteur == 0 && demandeLecteur) {
            //V(&REDACTEUR);
            op.sem_num=WRITER;op.sem_op=1;op.sem_flg=0;
            semop(semid,&op,1);
        }
        //V(&MUTEX);
        op.sem_num=MUTEX;op.sem_op=1;op.sem_flg=0;
        semop(semid,&op,1);
        i++;
        sleep(1);
    }
    
    exit(0);
}

int main ( int argc , char **argv ) {
    if( argc ==1){
        fprintf(stderr,"usage : %s  nombredeFils \n"),argv[0];
        exit(1);
    }
    lecteur=0;
    demandeLecteur=0;
    redacteur=0;
    demandeRedacteur =0;
    //TODO: initialisation des variables
    // int nbRouleau = atoi(argv[1]);
    // int* tab = (int*)malloc(nbRouleau*sizeof(int));
    // for (int i = 0; i < nbRouleau; i++) {
    //     tab[i] = rand() % 10;
    // }

   // creation d'une cle IPC en fonction de argv[0]
    if ((cle=ftok(argv[0],'0')) == -1 ) {
        fprintf(stderr,"Probleme sur ftoks\n");
        exit(1);
    }
    short init_sem[3]={3};
    // demande un ensemble de semaphore (ici un)
    if ((semid=semget(cle,3,IPC_CREAT|0666))==-1) {
        fprintf(stderr,"Probleme sur semget\n");
        exit(2);
    } 
    // initialise l'ensemble
    if (semctl(semid,2,SETALL,init_sem)==-1) { 
        fprintf(stderr,"Probleme sur semctl SETALL\n");
        exit(3);
    }

    // CREE ET INITALISE  LE SEGMENT PARTAGE
    // recupère le segment partagée
    if ((shmid = shmget(cle, 4096, IPC_CREAT|0644)) == -1) {
        fprintf(stderr, "Probleme sur shmget\n");
        exit(2);
    }
    // attache et s'empoisone au segment partagée
    // shmat permet de récupérer l'adresse du segment partagé
    if ((tab=(int*)shmat(shmid,NULL,0))==(int*)-1){
        perror("shmat ");
        fprintf(stderr, "Probleme sur shmat\n");
        exit(3);
    }

    struct sigaction actions;
    int rc;
    sigset_t set,oset;
    sigemptyset (&set);
    sigaddset (&set, SIGUSR1);
    sigprocmask (SIGUSR1, &set, &oset);
    sigemptyset(&actions.sa_mask);
    actions.sa_flags = 0;
    actions.sa_handler = tueurDeRoulette; // fonction a lancer 
    rc = sigaction(SIGUSR1,&actions,NULL);

    // for (int i=0; i < 10; i++)
    pid_t pidLecteur[10];
    for (int i = 0; i < 10; i++) {
        if (pidLecteur[i] = fork() == 0) {
            lecteurs(nbRouleau, tab);
        }
    }


    pid_t pid[nbRouleau];
    for (int i = 0; i < nbRouleau; i++) {  
        pid[i] = fork();
        if (pid[i] == 0) {
            //arme le signal
            struct sembuf op; // operation sur un semaphore
            int res = tab[i];
            while (fin != 1) {
                sigprocmask (SIGUSR1, &oset, NULL);
                //P(&MUTEX);
                op.sem_num=1;op.sem_op=-1;op.sem_flg=0;
                if (lecteur || redacteur || demandeRedacteur) {
                    demandeRedacteur++;
                    //V(&MUTEX);
                    op.sem_num=MUTEX;op.sem_op=1;op.sem_flg=0;
                    semop(semid,&op,1);
                    //P(&REDACTEUR)
                    op.sem_num=WRITER;op.sem_op=-1;op.sem_flg=0;
                    semop(semid,&op,1);
                    //P(&MUTEX);
                    op.sem_num=MUTEX;op.sem_op=-1;op.sem_flg=0;
                    semop(semid,&op,1);
                    demandeRedacteur--;
                }
                redacteur++;
                //V(&MUTEX);
                op.sem_num=MUTEX;op.sem_op=1;op.sem_flg=0;
                semop(semid,&op,1);
                res++;
                tab[i] = res%10;
                //P(&MUTEX);
                op.sem_num=MUTEX;op.sem_op=-1;op.sem_flg=0;
                semop(semid,&op,1);
                redacteur--;
                if (demandeRedacteur) {
                    //V(&REDACTEUR);
                    op.sem_num=WRITER;op.sem_op=1;op.sem_flg=0;
                    semop(semid,&op,1);
                }
                else if (demandeLecteur) {//V(&LECTEUR);
                    int nb;
                    for (nb=0; nb < demandeLecteur; nb++) {
                        //V(&READER)
                        op.sem_num=READER;op.sem_op=1;op.sem_flg=0;
                        semop(semid,&op,1);
                    }
                    //V(&MUTEX);
                    op.sem_num=MUTEX;op.sem_op=1;op.sem_flg=0;
                    semop(semid,&op,1);
                }
                sigprocmask (SIGUSR1, &set, NULL);
                usleep(100000);
            }

            shmdt(tab);
            exit(0);
        }
    }

    for (int i = 0; i < nbRouleau; i++) {
        // printf("La roue n°%d tourne, appuyez sur une touche pour l'arreter\n",i+1);
        getchar();
        kill(pid[i],SIGUSR1);
    }
    for (int i = 0; i < 10; i++) {
       kill(pidLecteur[i],SIGUSR1);
    }
    
    printf("numero des roulettes : ");
    for (int i = 0; i < nbRouleau; i++){
        printf("%d ",tab[i]);
    }


    int valuesArray[10] = {0};
    for (int i = 0; i < nbRouleau; i++) {
        valuesArray[tab[i]]++;
    }
    int nbSameMax = 0;
    for (int i = 0; i < 10; i++) {
        if (valuesArray[i] > nbSameMax) {
            nbSameMax = valuesArray[i];
        }
    }

    printf("\n");
    if (nbSameMax >= nbRouleau-1) printf("Gagné !\n");
    else printf("Perdu !\n");

    /* liberation du semaphore */
    semctl(semid,0,IPC_RMID,0);
    /* liberation du segment */
    semctl(shmid,0,IPC_RMID,0);
}
```