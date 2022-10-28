#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define ROOM_NUMBERS 100 


typedef struct{
	char *customerName;
	char *customerSurname;
}customerInformations;

typedef struct{
	int day;
	int month;
	int year;
}dateInformation;

typedef struct{
	int numberOfPeople;
	int roomReservation;
	int roomNumber;
	double priceInformation;
	dateInformation entryDate;
	dateInformation exitDate;
	customerInformations customer;
}roomInformation;


typedef enum{
	customer=0,staff=1
}accessLevel;

int dayDifference(dateInformation entryDate,dateInformation exitDate){
	
	if(entryDate.day>exitDate.day){
		exitDate.day+=30;
		exitDate.month-=1;
	}
	if(entryDate.month>exitDate.month){
		exitDate.month+=12;
		exitDate.year-=1;
	}
	dateInformation dayDifference;
	
	dayDifference.day=exitDate.day-entryDate.day;
	dayDifference.month=exitDate.month-entryDate.month;
	dayDifference.year=exitDate.year-entryDate.year;
	
	int days=dayDifference.year*365+dayDifference.month*30+dayDifference.day;
	return days;
}



double priceCalculation(roomInformation *rooms,int roomNumber){
	int days=dayDifference(rooms[roomNumber-1].entryDate,rooms[roomNumber-1].exitDate);
	printf("You have booked for %d days.\n",days);
	return days*200*rooms[roomNumber-1].numberOfPeople;
}


void cancelReservation(roomInformation *rooms,int roomNumber){
	rooms[roomNumber-1].roomReservation=0;
	free(rooms[roomNumber-1].customer.customerName);//musteri adi ve soyadi oda bilgisinden silindi.
	free(rooms[roomNumber-1].customer.customerSurname);
	
	
	printf("Check-out/cancellation of room %d completed",roomNumber);
	
}






void  newCustomerLogin(roomInformation *rooms){
	
	int roomCheck;
	char customerName[20],customerSurname[20];
	char ch;
	
	printf("Please enter the room number you want to register:");
	scanf("%d",&roomCheck);
	
	if(rooms[roomCheck-1].roomReservation!=0){
		printf("Sorry, the room you want to register is full until %d:%d:%d",rooms[roomCheck-1].exitDate.day,rooms[roomCheck-1].exitDate.month,rooms[roomCheck-1].exitDate.year);
		return;
	}
	
	rooms[roomCheck-1].roomNumber=roomCheck;
	
	printf("Please enter your name and surname:");
	scanf("%s%s",&customerName,&customerSurname);
	
	rooms[roomCheck-1].customer.customerName=(char*)malloc(sizeof(char)*strlen(customerName)+1);//+1 olmasının sebebei sondaki null karakterinden kaynaklanmaktadir
	rooms[roomCheck-1].customer.customerSurname=(char*)malloc(sizeof(char)*strlen(customerSurname)+1);
	
	strcpy(rooms[roomCheck-1].customer.customerName,customerName);
	strcpy(rooms[roomCheck-1].customer.customerSurname,customerSurname);
	
	printf("Please enter the number of people:");
	scanf("%d",&rooms[roomCheck-1].numberOfPeople);
	
	printf("Please enter the date of entry (day:month:year):");
	scanf("%d:%d:%d",&rooms[roomCheck-1].entryDate.day,&rooms[roomCheck-1].entryDate.month,&rooms[roomCheck-1].entryDate.year);
	
	printf("Please enter the date of exit(day:month:year):");
	scanf("%d:%d:%d",&rooms[roomCheck-1].exitDate.day,&rooms[roomCheck-1].exitDate.month,&rooms[roomCheck-1].exitDate.year);
	
	
	rooms[roomCheck-1].priceInformation=priceCalculation(rooms,roomCheck);
	printf("The amount you have to pay: %.2lfTL\n",rooms[roomCheck-1].priceInformation);
	printf("If you confirm the amount, press 'Y', if not, 'N':\n");
	fflush(stdin);
	ch=getchar();
	if(ch=='N' || ch=='n'){
		cancelReservation(rooms,roomCheck);//eger onaylamazsa kullanici bilgileri odadan silinecek ayrıca bu fonks. musteri odadan ayrilacagi zaman kullanilacak.
		return;
	}
	else if(ch=='Y' || ch=='y'){
	printf("Your new customer entry has been confirmed.");
	rooms[roomCheck-1].roomReservation=1;//musteri onayladigi icin oda durumu artık dolu.
	}
	else{
		printf("You made a wrong keystroke! Customer entry not confirmed.");
	}
	

	
}

void printRoomInformation(roomInformation *rooms){
	int i;
	for(i=0;i<ROOM_NUMBERS;i++){
		if(rooms[i].roomReservation!=0){
			printf("************************************\n");
			printf("Room number:%d \n",rooms[i].roomNumber);
			printf("Customer Name-Surname:%s %s\n",rooms[i].customer.customerName,rooms[i].customer.customerSurname);
			printf("Number of people:%d\n",rooms[i].numberOfPeople);
			printf("Price information:%.2lfTL\n",rooms[i].priceInformation);
			printf("Entry date:%d:%d:%d\n",rooms[i].entryDate.day,rooms[i].entryDate.month,rooms[i].entryDate.year);
			printf("Exit date:%d:%d:%d\n",rooms[i].entryDate.day,rooms[i].exitDate.month,rooms[i].entryDate.year);
		}
	}	
}


void roomCheck(roomInformation *rooms,int roomNumber){
	if(rooms[roomNumber-1].roomReservation!=0){
		printf("Room %d is occupied until %d:%d:%d.",roomNumber,rooms[roomNumber-1].exitDate.day,rooms[roomNumber-1].exitDate.month,rooms[roomNumber-1].exitDate.year);
	}
	else{
		printf("Room %d is empty.",roomNumber);
	}
}

void priceCheck(){
	int numberOfPeople;
	double priceAmount;
	dateInformation entryDate,exitDate;
	
	printf("Please enter how many people will stay in the room:");
	scanf("%d",&numberOfPeople);
	
		
	printf("Please enter the date of entry (day:month:year):");
	scanf("%d:%d:%d",&entryDate.day,&entryDate.month,&entryDate.year);
	
	printf("Please enter the date of exit(day:month:year):");
	scanf("%d:%d:%d",&exitDate.day,&exitDate.month,&exitDate.year);
	
	
	priceAmount=dayDifference(entryDate,exitDate)*numberOfPeople*200;
	printf("Total amount you have to pay:%.2lfTL\n",priceAmount);
	
	
	
}

void backupRecording(roomInformation *rooms){
	int i;
	FILE *hotelRecords=fopen("hotelRecords.txt","a");//a modu dosyanin icerigin silmez ve yeni gelen icerigi yazar.
	if(hotelRecords==NULL){
		fprintf(stderr,"An error occurred while retrieving the backup record.");
	}
	for(i=0;i<ROOM_NUMBERS;i++){
		if(rooms[i].roomReservation!=0){
			fprintf(hotelRecords,"************************************\n");
			fprintf(hotelRecords,"Room number:%d \n",rooms[i].roomNumber);
			fprintf(hotelRecords,"Customer Name-Surname:%s %s\n",rooms[i].customer.customerName,rooms[i].customer.customerSurname);
			fprintf(hotelRecords,"Number of people:%d\n",rooms[i].numberOfPeople);
			fprintf(hotelRecords,"Price information:%.2lfTL\n",rooms[i].priceInformation);
			fprintf(hotelRecords,"Entry date:%d:%d:%d\n",rooms[i].entryDate.day,rooms[i].entryDate.month,rooms[i].entryDate.year);
			fprintf(hotelRecords,"Exit date:%d:%d:%d\n",rooms[i].entryDate.day,rooms[i].exitDate.month,rooms[i].entryDate.year);
		}
	}
	fclose(hotelRecords);
}


int main(){
	
	int password=123456;
	int userPasswordEntry;
	char ch;
	int roomNumber;
	accessLevel userAuthorization;


	roomInformation *roomArray;
	roomArray=(roomInformation*)calloc(ROOM_NUMBERS,sizeof(roomInformation));
	
	printf("\n\t\t\t\t********WELCOME TO HOTEL RESERVATION SYSTEM********\n\n");
	printf("Please enter your staff password:");
	scanf("%d",&userPasswordEntry);
	if(password==userPasswordEntry){
		userAuthorization=staff;
	}
	else{
		userAuthorization=customer;
	}
	
	do{
		printf("\n1.If you want to make a new customer entry: 'E'\n2.If you want to query room information: 'S'\n3.If you want to view occupied room information: 'G'");
		printf("\n4.If you want to make a price inquiry 'F'\n5.If you want to perform customer check-out: 'C'\n6.If you want to quit of the system:'Q' \n");
		fflush(stdin);
		ch=getchar();
		switch(ch){
		
		case 'E' :
		case 'e' :
			if(userAuthorization==staff){
				newCustomerLogin(roomArray);
				backupRecording(roomArray);
				
			}
			else{
				printf("Please login as a staff to make a new entry!");
			}
			break;
		case 'S' :
		case 's' :
				printf("Please enter the number of the room whose status you want to know:");
				scanf("%d",&roomNumber);
				roomCheck(roomArray,roomNumber);
			break;
		case 'G' :
		case 'g' :
			if(userAuthorization==staff){
				printRoomInformation(roomArray);
			}
			else{
				printf("Please log in as a staff to see the occupied rooms!");
			}
			break;
		case 'C' :
		case 'c' :
			if(userAuthorization==staff){
				printf("Enter the room number of the customer you want to check out:");
				scanf("%d",&roomNumber);
				cancelReservation(roomArray,roomNumber);
			}
			else{
				printf("Please log in as a staff to check out the costumer");
			}
			break;
		case 'F' :
		case 'f' :
			priceCheck();
			break;
		case 'Q' :
		case 'q' :
			backupRecording(roomArray);
			break;
		default:
			printf("You made a wrong keystroke!\n");
			break;
		}
	}while(ch!='Q');
	
	return 0;
}
