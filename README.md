

#include "stdio.h"
#include "stdlib.h"

#define SIZE 10000

struct Workers{

    int id;

    int age;

    char name[30];

    char email[50];

    char password[50];


};

struct Workers info[SIZE];

void printingAlldatafromFile();

int charcounting(char tocount[50]);

void StrCmp(char userInputchar[50]);

void login();

void recordingAlldataFromFile();

void lobby();

void loadingAlldatatoFile();

void passchecking(char userinputpass[50],char userindex);

void useractionsector();

void registration();

void gmailvalidation(char tovalidate[50]);

void copyToarray(char transmit[50]);

void myStrongPassword(char Strongpass[50]);

void myStringCopy(char first[50],char second[50]);

int eFound=-1;

int pFound=-1;

int globalIndex=0;

int gvalid=-1;

int strongpassword=-1;



int main(){


    loadingAlldatatoFile();

    printingAlldatafromFile();

    lobby();





    return 0;
}

void printingAlldatafromFile(){

    for(int gcc=0;gcc<globalIndex;gcc++){

        printf("Id: %d -Age: %d -Name: %s -Email: %s -Password: %s\n",info[gcc].id,info[gcc].age,info[gcc].name,info[gcc].email,info[gcc].password);


    }

}

void lobby(){

    int loption=0;

    printf("This is lobby sector!\n");

    printf("Press 1 to login!:  \n");

    printf("Press 2 to register!:  \n");

    printf("Press 3 to exit!:  \n");

    scanf("%d",&loption);

    if(loption==1){

        login();

    }else if(loption==2){

        registration();

    }else if(loption==3){

        recordingAlldataFromFile();

        exit(1);
    }else{

        printf("This is invalid option.");

        lobby();
    }
}

void login(){

    char loginEmail[50];

    char loginPass[50];

   int found=-1;



    printf("This is login form:\n");

    printf("Enter email to login: ");

    scanf(" %[^\n]",&loginEmail[0]);

     eFound=-1;

     StrCmp(loginEmail);

     printf("Enter your password: ");

     scanf(" %[^\n]",&loginPass[0]);

     pFound=-1;

    passchecking(loginPass,eFound);



  if(eFound!=-1  && pFound==1){

      useractionsector();

  }else{

      printf("Invalid email: or Password  \n");

      login();
  }


}

void useractionsector(){

    int useraction=0;


    printf("Welcome Sir %s\n",info[eFound].name);
    printf("Press 1 to User action sector:  \n");
    printf("Press 2 to Home: \n");
    printf("Press 3 to exit!");
    scanf(" %d",&useraction);

    if(useraction==1){

        printf("This is useractionsector!");
    }else if(useraction==2){

        login();
    }else if(useraction==3){

        recordingAlldataFromFile();
    }else{
        printf("Invalid option!");

        useractionsector();
    }

}

void StrCmp(char userInputchar[50]){

    int samecount=0;


    int second=charcounting(userInputchar);

    for(int j=0;j<globalIndex;j++){

        int first=charcounting(info[j].email);

        if(first==second){

            for(int gcc=0;gcc<first;gcc++){

                if(info[j].email[gcc]==userInputchar[gcc]){

                    samecount++;

                }else{

                    break;
                }
            }

        }
        if(second==samecount){

            eFound=j;

            break;
        }



    }



}

int charcounting(char tocount[50]){
    int charcount=0;

    for(int i=0;i<50;i++){

        if(tocount[i]=='\0'){
            break;
        }else{
            charcount++;

        }

    }
    return charcount;

}

void recordingAlldataFromFile(){

    FILE *fptr=fopen("recordFile.txt","w");

    if(fptr==NULL){

        printf("Error at recording all from file!");

    }else{

        for(int i=0;i<globalIndex;i++){


            fprintf(fptr, "Id: %d Age: %d Name: %s Email: %s Password: %s \n",info[i].id, info[i].age,
                    info[i].name,info[i].email, info[i].password);
        }

        printf("Recording all data from file is complete!");
    }
    fclose(fptr);

    
}

void loadingAlldatatoFile(){


    FILE *fptr=fopen( "recordFile.txt","r");

    if(fptr==NULL){

        printf("Error at loading all data to file!");

    }else{

        for(int i=0;i<SIZE;i++){


            fscanf(fptr, "Id: %d Age: %d Name: %s Email: %s Password: %s \n",&info[i].id, &info[i].age,
                    &info[i].name,&info[i].email, &info[i].password);


            if(info[i].name[0]=='\0'){

                break;
            }
            globalIndex++;

        }




        fclose(fptr);

    }
}
void passchecking(char userinputpass[50],char userindex){

    int samepass=0;

    int passcount=charcounting(userinputpass);

    int  infopasscount=charcounting(info[userindex].password);




        if(passcount==infopasscount){


            for(int j=0;j<passcount;j++){

                if(info[userindex].password[j]==userinputpass[j]){

                    samepass++;


                }else{

                    break;
                }


            }
            if(samepass==passcount){

                pFound=1;



            }




        }




}

void registration(){

    char rePassword[50];

     char reEmail[50];

    printf("This is registration!\n");

    printf("Enter your email to register: ");

    scanf(" %[^\n]",&reEmail[0]);

    gvalid=-1;

    gmailvalidation(reEmail);

    if(gvalid!=-1){


        eFound=-1;

        StrCmp(reEmail);


        if(eFound==-1 ){


           copyToarray(reEmail);


            info[globalIndex].id=globalIndex+1;

            printf(" Enter your name pls: ");

            scanf(" %[^\n]",&info[globalIndex].name[0]);


            printf("Enter your age pls: ");

            scanf(" %d",&info[globalIndex].age);

         int a=0;

           while(a==0){


                printf("Enter your password pls: ");

                scanf(" %[^\n]",&info[globalIndex].password[0]);

              strongpassword=-1;

                myStrongPassword(rePassword);

                if(strongpassword!=-1){

                    myStringCopy(info[globalIndex].password,rePassword);

                    a=1;



                }else{

                    printf("Weak password!Try again!");


                }

            }



           globalIndex++;

            printingAlldatafromFile();

            login();





        }else{

            printf("Your email was already exit!");
        }
    }else{

        printf("Your gmail is not valid format!");

        registration();
    }




}

void gmailvalidation(char tovalidate[50]){

    int toEndpoint=charcounting(tovalidate);

    int count=0;


    char form[10]={'@','g','m','a','i','l','.','c','o','m'};

    for(int j=0;j<toEndpoint;j++){

        if(tovalidate[j]==' '){

            break;
        }else if(tovalidate[j]=='@'){

            break;
        }else{

            count++;
        }
    }


    int tocheck=0;

    for(int i=0;i<toEndpoint;i++){



        if(tovalidate[count]==form[i]){

            count++;

            tocheck++;


        }else{

            break;
        }


    }

    if(tocheck==10 ){

        gvalid=1;
    }


}

void copyToarray(char transmit[50]){

 int toEnd= charcounting(transmit);

 for(int i=0;i<toEnd;i++){

     info[globalIndex].email[i]=transmit[i];


 }

}

void myStringCopy(char first[50],char second[50]){

    int secondCount=charcounting(second);

    for(int i=0;i<50;i++){

        first[i]='\0';
    }
    for(int a=0;a<secondCount;a++){

        first[a]=second[a];
    }

}

void myStrongPassword(char Strongpass[50]){

    int strpass=0;
    int i=0;
    int special=0;
   int numberchar=0;
    int specialchar=0;

      while(strongpassword==-1){


              if(Strongpass[i]>=33 && Strongpass[i]<=42){

                  strpass++;
              }else if(Strongpass[i]>=42 && Strongpass[i]<=57){

                  special++;
              }else if(Strongpass[i]>=65 && Strongpass[i]<=90){
                  specialchar++;

              }else if(Strongpass[i]>=97 && Strongpass[i]<=122){

                  numberchar++;

              }

              if(strpass>0 && special>0 && specialchar>0 && numberchar>0){

                  strongpassword=1;
              }else{

                  printf("Weak password!\n");


              }

             i++;
              numberchar++;
              specialchar++;
              strpass++;
              special++;


      }



}





