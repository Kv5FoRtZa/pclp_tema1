#include <ctype.h>
#include <stdio.h>
#include <string.h>
#include <stdlib.h>

typedef struct
{
    int id;
    char *nume;
} cartier;
typedef struct
{
    int id, idCartier, strada, numar, prioritate, codificare_mesaj;
    double greutate;
    int adresa[18];
    char *mesaj;
} pachet;
typedef struct
{
    int id, nrPachete, v[50];
} postas;
int recodificare(int id, int mesaj_curent)
{
    int copie_id = id, vf = 0, i;
    while (copie_id)
    { /// verificam daca sunt cifre comune
        int cifra = copie_id % 10, copie_mesaj = mesaj_curent;
        copie_id = copie_id / 10;
        while (copie_mesaj)
        { /// fiecare cifra din id o cautam in mesaj
            if (cifra == copie_mesaj % 10)
            {
                vf = 1;
                break;
            }
            copie_mesaj = copie_mesaj / 10;
        }
        if (vf == 1)
        {
            break;
        }
    }
    if (vf == 0)
    { /// nu sunt cifre comune
        return mesaj_curent;
    }
    else
    { /// daca sunt cifre comune
        copie_id = id;
        if (id == 0 || id == 1)
        {/// 0 si 1 tratate diferit
            if (mesaj_curent & (1 << id))
            {///daca sunt divizori primi inversam bitul
                mesaj_curent -= (1 << id);
            }
            else
            {
                mesaj_curent += (1 << id);
            }
        }
        for (i = 2; i <= copie_id; i++)
        {
            vf = 0;
            while (copie_id % i == 0)
            { // cautam divizorii primi
                vf = 1;
                copie_id = copie_id / i;
            }
            if (vf)
            {///daca sunt divizori primi inversam bitul
                if (mesaj_curent & (1 << i))
                {
                    mesaj_curent -= (1 << i);
                }
                else
                {
                    mesaj_curent += (1 << i);
                }
            }
        }
        return mesaj_curent;
    }
}
void afisare_7(int nr_corect, int nr_total)
{///afisarea la 7
    double d_nr_corect = nr_corect;
    double d_om_nrPachete = nr_total;
    if(d_om_nrPachete == 0)
    {///impartire la 0
        printf("0.000\n");
    }
    else
    {
        double procentaj = d_nr_corect / d_om_nrPachete;
        printf("%.3lf\n", procentaj);
    }
}
void afisare_6(pachet *packet, int n_postas, postas *om, int cerinta )
{//aici se afiseaza la 6
    int i,j;
    for (i = 0; i < n_postas; i++)
    {
        int nr_corect = 0;
        if(cerinta == 6)
        {
            printf("%d %d\n", i, om[i].nrPachete);
        }
        else
        {
            printf("%d ", i);
        }
        for (j = 0; j < om[i].nrPachete; j++)
        {
            int cod_nou = recodificare(i, packet[om[i].v[j]].codificare_mesaj);
            if(cerinta == 6)
            {///daca e 6 se afiseaza codul nou
                printf("%d %d\n", om[i].v[j], cod_nou);
            }
            else
            {
                if(cod_nou == packet[om[i].v[j]].codificare_mesaj)
                {///daca e 7 se numara cate coduri sunt bune
                    nr_corect ++;
                }
            }
        }
        if(cerinta == 7)
        {
            afisare_7(nr_corect,om[i].nrPachete);
        }
    }
}
void inversare(pachet *packet, int n, postas *om, int n_postas, int cerinta)
{
    int i, j;
    for (i = 0; i < n; i++)
    {
        int indice2 = strlen(packet[i].mesaj);
        char vector_secundar[100] = {0}; /// luam fiecare cuvant
        char vector_final[1000];  /// aici bagam fiecare cuvant
        int marime = 0;
        for (j = 0; j < indice2; j++)
        {
            if (isalnum(packet[i].mesaj[j]) || packet[i].mesaj[j] == '-')
            { /// daca e numar/cifra salvam in vector
                vector_secundar[marime] = packet[i].mesaj[j];
                marime++;
            }
            else
            { /// altfel bagam cuvantul in vectorul final
                int marime_final = strlen(vector_final);
                memmove(vector_final + marime, vector_final, marime_final + 1);
                memcpy(vector_final, vector_secundar, marime);
                for (int k = 0; k < marime; k++)
                {
                    vector_secundar[k] = 0;
                }
                marime = 0;
            }
        }
        /// ultimul cuvant nu are spatiu dupa el
        int marime_final = strlen(vector_final);
        memmove(vector_final + marime, vector_final, marime_final + 1);
        memcpy(vector_final, vector_secundar, marime);
        int suma = 0;
        marime_final = strlen(vector_final);
        for (j = 0; j <= marime_final; j++)
        { /// facem codul aici
            packet[i].mesaj[j] = vector_final[j];
            if (vector_final[j])
            {
                suma += j * vector_final[j];
            }
            vector_final[j] = 0;
        }
        suma = suma % (packet[i].strada * packet[i].numar + 1);
        packet[i].codificare_mesaj = suma;
    }
    if (cerinta == 5)
    {
        for (i = 0; i < n_postas; i++)
        { // afisare pt 5
            printf("%d %d\n", i, om[i].nrPachete);
            for (j = 0; j < om[i].nrPachete; j++)
            {
                printf("%d %d\n", om[i].v[j], packet[om[i].v[j]].codificare_mesaj);
            }
        }
    }
    else
    {
        afisare_6(packet,n_postas , om , cerinta);
    }
}
void sortare(pachet *packet, postas *om, int n, int cerinta)
{ /// afisare si sortare cerinta 4
    int i, j, k, pr[1600] = {0};
    double gr[1600] = {0};
    for (i = 0; i < n; i++)
    { /// practic un bubble sort
        if (om[i].nrPachete)
        {
            for (j = 0; j < om[i].nrPachete; j++)
            { /// salvez greutatea si prioritatea elementelor
                int id = om[i].v[j];
                pr[j] = packet[id].prioritate;
                gr[j] = packet[id].greutate;
            }
            for (j = 0; j < om[i].nrPachete - 1; j++)
            {
                for (k = 0; k < om[i].nrPachete - 1 - j; k++)
                {
                    int id1 = k;
                    int id2 = k + 1;
                    if (pr[k] < pr[k + 1])
                    { /// bubble sort cu prioritate
                        int val1, val3;
                        double val2;
                        val1 = pr[id1]; // cele 3 swapuri
                        val2 = gr[id1]; // pt greutate prioritate si element
                        val3 = om[i].v[k];
                        pr[id1] = pr[id2];
                        pr[id2] = val1;
                        gr[id1] = gr[id2];
                        gr[id2] = val2;
                        om[i].v[k] = om[i].v[k + 1];
                        om[i].v[k + 1] = val3;
                    }
                    else if (pr[id1] == pr[id2])
                    { /// bubble sort cu greutate
                        if (gr[id1] < gr[id2])
                        {
                            int val1, val3;
                            double val2;
                            val1 = pr[id1]; // cele 3 swapuri
                            val2 = gr[id1]; // pt greutate prioritate si element
                            val3 = om[i].v[k];
                            pr[id1] = pr[id2];
                            pr[id2] = val1;
                            gr[id1] = gr[id2];
                            gr[id2] = val2;
                            om[i].v[k] = om[i].v[k + 1];
                            om[i].v[k + 1] = val3;
                        }
                    }
                }
            }
        }
    }
    if (cerinta == 4)
    { /// afisez doar pt cerinta 4
        for (i = 0; i < n; i++)
        {
            if (om[i].nrPachete)
            {
                printf("%d %d\n", om[i].id, om[i].nrPachete);
                for (j = 0; j < om[i].nrPachete - 1; j++)
                {
                    printf("%d ", om[i].v[j]);
                }
                printf("%d", om[i].v[j]);
                printf("\n");
            }
            else
            { /// nu trebuie afisate pachete daca nu sunt
                printf("%d %d\n", i, om[i].nrPachete);
            }
        }
    }
}
void distribuire(pachet *packet, int id, postas *om)
{ // aici distribui postasilor
    int cartier = packet->idCartier;
    om->id = cartier;
    om->nrPachete++;
    om->v[om->nrPachete - 1] = id;
}
void afisare_3(int n, postas *om)
{ // afisare pt cerinta 3
    int i, j;
    for (i = 0; i < n; i++)
    {
        if (om[i].nrPachete)
        {
            printf("%d %d\n", om[i].id, om[i].nrPachete);
            for (j = 0; j < om[i].nrPachete - 1; j++)
            {
                printf("%d ", om[i].v[j]);
            }
            printf("%d", om[i].v[j]);
            printf("\n");
        }
        else
        { /// nu trebuie afisate pachete daca nu sunt
            printf("%d %d\n", i, om[i].nrPachete);
        }
    }
}
void extrage_pachete(pachet *packet, int cerinta) // aici e cerinta 2
{
    int i, nr = 0, putere;
    putere = 1;
    for (i = 4; i >= 0; i--) // primele 5 caractere se transforma in baza 10 si le bag in struct
    {
        nr = nr + packet->adresa[i] * putere;
        putere = putere * 2;
    }
    packet->idCartier = nr;
    putere = 1;
    nr = 0;
    for (i = 9; i >= 5; i--) // urmatoarele 5 caractere se transforma in baza 10 si le bag in struct
    {
        nr = nr + packet->adresa[i] * putere;
        putere = putere * 2;
    }
    packet->strada = nr;
    putere = 1;
    nr = 0;
    for (i = 17; i >= 10; i--) // ultimele 8 caractere se transforma in baza 10 si le bag in struct
    {
        nr = nr + packet->adresa[i] * putere;
        putere = putere * 2;
    }
    packet->numar = nr;
    if (cerinta == 2)
    {
        printf("%d\n", packet->id); // afisarea pt cerinta 2
        printf("%d %d %d\n", packet->idCartier, packet->strada, packet->numar);
    }
}
void citire(int *nr, int *nrp, int cerinta) // citirea se face aici(si cerinta 1)
{
    int i, j, cifra_adresa;
    char v[200];
    scanf("%d", nr);
    cartier numar[32];
    postas om[32];
    for (i = 0; i < 32; i++)
    {
        om[i].nrPachete = 0;
    }
    for (i = 0; i < *nr; i++) // citirea cartiere
    {
        scanf("%s", v); // citesc nume si il bag in struct alocat dinamic
        int marime = strlen(v);
        numar[i].nume = malloc((marime + 1) * sizeof(char));
        strcpy(numar[i].nume, v);
        numar[i].id = i;
        if (cerinta == 1)
        {
            printf("%d %s\n", numar[i].id, numar[i].nume); // afisarea pt cerinta 1
        }
    }
    scanf("%d", nrp);
    pachet packet[1600] = {0};
    for (i = 0; i < *nrp; i++) // citire packete
    {
        for (j = 0; j < 18; j++)
        {
            scanf("%d", &cifra_adresa); /// citesc vectorul de 0 si 1
            packet[i].adresa[j] = cifra_adresa;
        }
        scanf("%d%lf%*c", &packet[i].prioritate, &packet[i].greutate);
        fgets(v, sizeof(v), stdin); // cistesc mesajul si il bag alocat dinamic in struct
        int size = strlen(v);
        packet[i].mesaj = malloc((size + 1) * sizeof(char));
        strcpy(packet[i].mesaj, v);
        packet[i].id = i;
        extrage_pachete(&packet[i], cerinta);                 // extrage informatiile de adresa
        distribuire(&packet[i], i, &om[packet[i].idCartier]); // distribuie postasilor
        if (cerinta == 1)
        {
            printf("%d\n", packet[i].id); /// afisarea pentru cerinta 1;
            for (j = 0; j < 17; j++)
            {
                printf("%d ", packet[i].adresa[j]);
            }
            printf("%d", packet[i].adresa[17]);
            printf("\n%d ", packet[i].prioritate);
            printf("%.3lf\n", packet[i].greutate);
            printf("%s", packet[i].mesaj);
        }
    }
    if (cerinta == 3)
    { // afisarea pt cerinta 3
        int val = *nr;
        afisare_3(val, om);
    }
    if (cerinta >= 4)
    { // afisarea pt cerinta 4
        int val2 = *nr;
        sortare(packet, om, val2, cerinta);
        if (cerinta >= 5)
        { //la cerintele 5 6 si 7 este nevoie de rezultatele anterioare
            int val = *nrp;
            inversare(packet, val, om, val2, cerinta);
        }
    }
}
int main()
{
    int nrc, nrp, cerinta; // citire cerinta si apelare
    scanf("%d", &cerinta);
    citire(&nrc, &nrp, cerinta);
    return 0;
}
