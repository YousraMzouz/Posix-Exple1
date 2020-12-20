#include <stdio.h>
#include <stdlib.h>
#include <time.h>
#include <pthread.h>

clock_t clock(void);

//La fonction du thread
void* lire_entier(void *arg) {
     long clk_tck = CLOCKS_PER_SEC;

  int un_entier;
  int val_arg = (int) arg;
  printf("Bienvenue chez le thread ayant comme argument %d\n", val_arg);
  printf("Priere de saisir un entier:");
  scanf("%d", &un_entier);
  pthread_exit((void *) un_entier); 
}//fin lire_entier

int main(void) {
     long clk_tck = CLOCKS_PER_SEC;
  clock_t t1, t2;
   t1 = clock();

  int i, val_retour;
  pthread_t thread1,thread2,thread3;
  srand(time(NULL));
  i = 1 + rand() % 100;
  int succes = pthread_create(&thread1, NULL, lire_entier, (void *) i);
  if (succes != 0) {
    fprintf(stderr, "Erreur de creation du thread ...");
    exit(0);
  }//fin if (succes != 0)
 //Attente du thread de lecture
  pthread_join(thread1, (void *)&val_retour); 
  int a;
  printf("Tapez 1 pour entier,  Tapez 2 pour Octal,    Tapez 3 pour Hexadecimal,   Taper 0 pour afficher tout : \n");
  scanf("%d",&a);
if(a==1 || a==0 ){
  printf("Vous avez lu une valeur entiere egale a %d\n", val_retour);
  printf("Temps de lecture écoulé = %i millisecondes\n",clock());

}

if(a==2 || a==0 )
{
  pthread_join(thread2, (void *)&val_retour); 
  printf("Vous avez lu une valeur octall egale a %o\n", val_retour);
  printf("Temps de traitement (octal) écoulé = %i millisecondes\n",clock());

   
}

 
  if(a==3 || a==0 ){

  pthread_join(thread3, (void *)&val_retour); 
  printf("Vous avez lu une valeur hexatedimal egale a %x\n", val_retour);
  printf("Temps de traitement (hexa) écoulé = %i millisecondes\n",clock());
  }

  
   t2 = clock();
printf("Le Temps Total consomme en (s) : %lf \n",
                (double)(t2-t1)/(double)clk_tck);

  return 0;
}//fin main()
