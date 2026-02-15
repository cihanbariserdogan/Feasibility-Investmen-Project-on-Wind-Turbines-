# Feasibility-Investmen-Project-on-Wind-Turbines-
#include <stdio.h>
#include <string.h>
#include <stdbool.h>
#include <stdlib.h>

#define MAXCHAR 1000
#define ARRAYROWS 1000
float Enercon_wind_speed_price_calculator(float Wind_speed,float Kwh_selling_price);
float Siemens_wind_speed_price_calculator(float Wind_speed,float Kwh_selling_price);
float Vestas_wind_speed_price_calculator(float Wind_speed,float Kwh_selling_price);

struct Data {
    double N;
    double F_P;
    double P_F;
    double F_A;
    double P_A;
    double A_F;
    double A_P;
    
};

int main() {

    printf("----------------------------------------------------------\n");
    printf("This software allows you to calculate Maximum Annual Rate of Return (MARR) for wind turbine market place in Türkiye and make an analysis for is it feasible or not.\n");
  printf("----------------------------------------------------------\n");
  printf("There is 3 major brands and their model names for wind turbine purchase please pick the one you will invest.\n\n1-) Enercon E138-EP3\n2-) Siemens SWT-3.3-130\n3-) Vestas V112-3.3-IEC-IB\n-----------------------------------------------------------");
    FILE *fp;
    char row[MAXCHAR];
    struct Data *d;
    int i = 0, n, k;
    float money, future, maintenance, maintenance_total, annual_income,annual_income_total;
    float residual, residual_real, result, interest_rate,Future_worth;
    float Mwh;
    float Kwh_selling_price,your_possible_future_value_from_bank;
    float Wind_speed,installation_money,choice ;

    int e;
    printf("\nPlease type your choice and press enter: ");
    scanf("%d", &e);
    system("clear");

    if (e==1){
      printf("You have picked Enercon E138-EP3 model\n\nRated Power: 4,260kW\n\nRotor Diameter: 138.25 m\n\nHub Height in meter: 81 / 99/ 111 / 131 / 160\n\nWind class (IEC):IIA\n\nTurbine concept:gearless, variable speed, single blade adjustment\n\nOperatinal wind speed range 5.1 m/s-7.5 m/s\n\nNow i need your goverment interest rate for my calculations\n\n");
    }
    if (e==2){
      printf("\nYou have picked Siemens SWT-3.3-130 model\n\nRated Power: 3,300kW\n\nRotor Diameter: 130 m\n\nHub Height up to 135m\n\nWind class (IEC):IB\n\nTurbine concept:gearless, variable speed, single blade adjustment\n\nOperatinal wind speed range 5 m/s-10 m/s\n\n Now i need your goverment interest rate for my calculations\n");}
   
    if (e==3){
      printf("\nYou have picked Vestas V112-3.3-IEC-IB model\n\nRated Power: 3,300kW\n\nRotor Diameter: 112.0 m m\n\nHub Height in meter: 84/ 94/ 119/ 140 m\n\nWind class (IEC):	IB \n\nTurbine concept:gearless, variable speed, single blade adjustment\n\nOperatinal wind speed range 6 m/s-10 m/s\n\n now i need your goverment interest rate for my calculations\n");}
   printf("--------------------------------------------------");
    printf("\nWhat's your goverment interest rate: ");
    scanf("%f", &interest_rate);

    char filename[20];

    if (interest_rate <= 0.25) {
        strcpy(filename, "%0.25.csv");
    } else if (interest_rate >= 0.375 && interest_rate < 0.625) {
        strcpy(filename, "%0.5.csv");
    } 
      else if (interest_rate >= 0.625 && interest_rate < 0.875) {
      strcpy(filename, "%0.75.csv");
    }
    else if (interest_rate >= 0.875 && interest_rate < 1.5) {
        strcpy(filename, "%1.csv");
    }
    else if (interest_rate >= 1.5 && interest_rate < 2.5) {
        strcpy(filename, "%2.csv");
    }
    else if (interest_rate >= 2.5 && interest_rate < 3.5) {
        strcpy(filename, "%3.csv");
    }
    else if (interest_rate >= 3.5 && interest_rate < 4.5) {
        strcpy(filename, "%4.csv");
    }
    else if (interest_rate >= 4.5 && interest_rate < 5.5) {
        strcpy(filename, "%5.csv");
    }else if (interest_rate >= 5.5 && interest_rate < 6.5) {
        strcpy(filename, "%6.csv");
    }else if (interest_rate >= 6.5 && interest_rate < 7.5) {
        strcpy(filename, "%7.csv");
    }else if (interest_rate >= 7.5 && interest_rate < 8.5) {
        strcpy(filename, "%8.csv");
    }else if (interest_rate >= 8.5 && interest_rate < 9.5) {
        strcpy(filename, "%9.csv");
    }else if (interest_rate >= 9.5 && interest_rate < 11) {
        strcpy(filename, "%10.csv");
    }else if (interest_rate >= 11 && interest_rate < 13.5) {
        strcpy(filename, "%12.csv");
    }else if (interest_rate >= 13.5 && interest_rate < 16.5) {
        strcpy(filename, "%15.csv");
    }else if (interest_rate >= 16.5 && interest_rate < 19) {
        strcpy(filename, "%18.csv");
    }else if (interest_rate >= 19 && interest_rate < 22.5) {
        strcpy(filename, "%20.csv");
    }else if (interest_rate >= 22.5 && interest_rate <= 25) {
        strcpy(filename, "%25.csv");
    }else {
        printf("Invalid interest rate.\n");
        return 1;
    }

    fp = fopen(filename, "r");
    if (fp == NULL) {
        printf("Error opening the file.\n");
        return 1;
    }

    while (fgets(row, MAXCHAR, fp) != NULL) {
        i++;
    }

    n = i - 1;
    rewind(fp);

    d = (struct Data*)malloc(n * sizeof(struct Data));
    if (d == NULL) {
        printf("Memory allocation failed.\n");
        fclose(fp);
        return 1;
    }

    fgets(row, MAXCHAR, fp); // Skip header line

    for (i = 0; i < n; i++) {
        if (fgets(row, MAXCHAR, fp) == NULL) {
            printf("Error reading row %d\n", i);
            break;
        }

        k = sscanf(row, "%lf,%lf,%lf,%lf,%lf,%lf,%lf\n",
            &d[i].N, &d[i].F_P, &d[i].P_F, &d[i].F_A,
            &d[i].P_A, &d[i].A_F, &d[i].A_P);

        if (k != 7) {
            //printf("Row %d does not have 7 elements\n", i);
            break;
        }
    }

    fclose(fp);

    for (i = 0; i < n - 1; i++) {
      
    }

    

    switch (e) {
        case 1:
            printf("\nGive me installation_money as ₺: ");
            scanf("%f",&installation_money);
            printf("\nGive me your selling price per kWh as ₺ : ");
            scanf("%f",&Kwh_selling_price);
        do {
        printf("\nGive me your wind speed as m/s: ");
        scanf("%f", &Wind_speed);

        if (5.1 <= Wind_speed && Wind_speed <= 7.5) {
            break;  // Do something with valid wind speed
        } else {
printf("Your wind speed value is out of range. It must be between 5 m/s and 7.5 m/s.\n");
            printf("Do you want to try again (0 to exit, any other number to try again): ");
            scanf("%f", &choice);

            if (choice == 0) {
                printf("Exiting...\n");
                return 0;
            }
        }
    } while (choice != 0);
   //printf("%f\n",wind_speed_price_calculator(Wind_speed,Kwh_selling_price));
          
          
          annual_income = Enercon_wind_speed_price_calculator(Wind_speed,Kwh_selling_price);
             
            //printf("%f\n",annual_income);
            printf("\nGive me your investment period (N): ");
            scanf("%d", &i);
            printf("\nGive me your mean maintenance value as ₺: ");
            scanf("%f", &maintenance);
            printf("\nGive me your residual value as ₺: ");
            scanf("%f", &residual);

            if (i >= 1 && i <= 25) {
maintenance_total = d[i - 1].P_A *-maintenance;
            } else {
                printf("Invalid row number.\n");
                break;
            }
          if (i >= 1 && i <= 25) {
          annual_income_total = d[i - 1].P_A * annual_income;
            
            } else {
                printf("Invalid row number.\n");
                break;
            }
            if (i >= 1 && i <= 25) {
residual_real = d[i - 1].P_F * residual;
               // printf("%f",residual_real);
            } else {
                printf("Invalid row number.\n");
                break;
            }
           if (i >= 1 && i <= 25) {
          money = d[i - 1].P_F * residual;
               // printf("%f",residual_real);
            } else {
                printf("Invalid row number.\n");
                break;
            }
          
            result = -installation_money+ annual_income_total + residual_real + maintenance_total;
          your_possible_future_value_from_bank= installation_money * d[i - 1].F_P;
          if (i >= 1 && i <= 25) {
          Future_worth = d[i - 1].F_P * result;
          //printf("%f this is your future value in the end of the investment",Future_worth);
               // printf("%f",residual_real);
            } else {
                printf("Invalid row number.\n");
                break;
            }
            if (result>=0){
            printf("\n\n\nYour investment is economically justified with:\n\n%.2f MARR value\n\nand your future worth will be\n\n%.2f ₺\n\n\n",result,Future_worth);}
            else{ printf("\n\n\nYour investment is did not economically justified with\n%.2f ₺ MARR value\n\nif you would put in a bank instead of making this investment this is your future value getting from the bank:\n %.2f\n\nand this is your oppurtunity cost for this investment:\n %.2f ₺ ",result,your_possible_future_value_from_bank,Future_worth);
          }
            //printf("Result: %.2f\n", result);
            break;
            
            case 2:
            printf("\nGive me installation_money as ₺: ");
            scanf("%f",&installation_money);
            printf("\nGive me your selling price per kWh as ₺: ");
          scanf("%f",&Kwh_selling_price);
            do {
        printf("\nGive me your wind speed as m/s: ");
        scanf("%f", &Wind_speed);

        if (5 <= Wind_speed && Wind_speed <= 10) {
            break;  // Do something with valid wind speed
        } else {
printf("Your wind speed value is out of range. It must be between 5 m/s and 10 m/s.\n");
            printf("Do you want to try again (0 to exit, any other number to try again): ");
            scanf("%f", &choice);

            if (choice == 0) {
                printf("Exiting...\n");
                return 0;
            }
        }
    } while (choice != 0);
            //printf("%f\n",wind_speed_price_calculator(Wind_speed,Kwh_selling_price));
  
              
              annual_income = Siemens_wind_speed_price_calculator(Wind_speed,Kwh_selling_price);

              
            
            printf("Give me your investment period (N): ");
            scanf("%d", &i);
            printf("Give me your mean maintenance value as ₺: ");
            scanf("%f", &maintenance);
            printf("Give me your residual value as ₺: ");
            scanf("%f", &residual);

            if (i >= 1 && i <= 25) {
maintenance_total = d[i - 1].P_A *-maintenance;
            } else {
                printf("Invalid row number.\n");
                break;
            }
          if (i >= 1 && i <= 25) {
          annual_income_total = d[i - 1].P_A * annual_income;
            
            } else {
                printf("Invalid row number.\n");
                break;
            }
            if (i >= 1 && i <= 25) {
residual_real = d[i - 1].P_F * residual;
               // printf("%f",residual_real);
            } else {
                printf("Invalid row number.\n");
                break;
            }
           if (i >= 1 && i <= 25) {
          money = d[i - 1].P_F * residual;
               // printf("%f",residual_real);
            } else {
                printf("Invalid row number.\n");
                break;
            }
          
            result = -installation_money+ annual_income_total + residual_real + maintenance_total;
           your_possible_future_value_from_bank= installation_money * d[i - 1].F_P;
          if (i >= 1 && i <= 25) {
          Future_worth = d[i - 1].F_P * result;
          //printf("%f this is your future value in the end of the investment",Future_worth);
               // printf("%f",residual_real);
            } else {
                printf("Invalid row number.\n");
                break;
            }
            if (result>=0){
            printf("\n\n\nYour investment is economically justified with %.2f ₺ MARR value  and your future worth will be %.2f ₺\n\n\n",result,Future_worth);}
            else{ printf("\n\n\nYour investment is not economically justified with\n%.2f ₺ MARR value\n\nif you would put in a bank instead of making this investment this is your future value getting from the bank:\n %.2f ₺\n\nand this is your oppurtunity cost for this investment:\n %.2f ₺ ",result,your_possible_future_value_from_bank,Future_worth);
          }
            //printf("Result: %.2f\n", result);
            break;
            
            case 3:
            printf("\nGive me installation_money as ₺: ");
            scanf("%f",&installation_money);
            printf("\nGive me your selling price per kWh: ");
            scanf("%f",&Kwh_selling_price);
            do {
            printf("\nGive me your wind speed as m/s: ");
            scanf("%f", &Wind_speed);

        if (6 <= Wind_speed && Wind_speed <= 10) {
            break;  // Do something with valid wind speed
        } else {
printf("Your wind speed value is out of range. It must be between 6 m/s and 10 m/s.\n");
            printf("Do you want to try again (0 to exit, any other number to try again): ");
            scanf("%f", &choice);

            if (choice == 0) {
                printf("Exiting...\n");
                return 0;
            }
        }
    } while (choice != 0);
              
            //printf("%f\n",wind_speed_price_calculator(Wind_speed,Kwh_selling_price));
            
              
              annual_income = Vestas_wind_speed_price_calculator(Wind_speed,Kwh_selling_price);
            
            //printf("annual =%f\n",annual_income);
            printf("Give me your investment period (N): ");
            scanf("%d", &i);
            printf("Give me your mean maintenance value as ₺: ");
            scanf("%f", &maintenance);
            printf("Give me your residual value as ₺: ");
            scanf("%f", &residual);

            if (i >= 1 && i <= 25) {
maintenance_total = d[i - 1].P_A *-maintenance;
            } else {
                printf("Invalid row number.\n");
                break;
            }
          if (i >= 1 && i <= 25) {
          annual_income_total = d[i - 1].P_A * annual_income;
            
            } else {
                printf("Invalid row number.\n");
                break;
            }
            if (i >= 1 && i <= 25) {
residual_real = d[i - 1].P_F * residual;
               // printf("%f",residual_real);
            } else {
                printf("Invalid row number.\n");
                break;
            }
           if (i >= 1 && i <= 25) {
          money = d[i - 1].P_F * residual;
               // printf("%f",residual_real);
            } else {
                printf("Invalid row number.\n");
                break;
            }
          
            result = -installation_money+ annual_income_total + residual_real + maintenance_total;
            your_possible_future_value_from_bank= installation_money * d[i - 1].F_P;
            
            
          if (i >= 1 && i <= 25) {
          Future_worth = d[i - 1].F_P * result;
          //printf("%f this is your future value in the end of the investment",Future_worth);
               // printf("%f",residual_real);
            } else {
                printf("Invalid row number.\n");
                break;
            }
            if (result>=0){
            printf("\n\n\nYour investment is economically justified with %f MARR value  and your future worth will be %f\n\n\n",result,Future_worth);}
            else{ printf("\n\n\nYour investment is did not economically justified with\n%.2f ₺ MARR value\n\nif you would put in a bank instead of making this investment this is your future value getting from the bank:\n %.2f\n\nand this is your oppurtunity cost for this investment:\n %.2f ₺ ",result,your_possible_future_value_from_bank,Future_worth);
          }
          
            break;
            
        default:
            printf("Invalid equation number.\n");
            break;
    }

    free(d);
    return 0;
}
float Enercon_wind_speed_price_calculator(float Wind_speed,float Kwh_selling_price)
{
float total_income_per_year;
float Mwh;
Mwh = 3095.238* Wind_speed-7403.5;
total_income_per_year = Mwh*1000*Kwh_selling_price;
return total_income_per_year;
}
float Siemens_wind_speed_price_calculator(float Wind_speed,float Kwh_selling_price)
{
float total_income_per_year;
float Mwh;
Mwh = -0.196* Wind_speed*Wind_speed+5190*Wind_speed-14300;
total_income_per_year = Mwh*1000*Kwh_selling_price;
return total_income_per_year;
}
float Vestas_wind_speed_price_calculator(float Wind_speed,float Kwh_selling_price)
{
float total_income_per_year;
float Mwh;
Mwh = -0.125*Wind_speed* Wind_speed+4250*Wind_speed-13000;
total_income_per_year = Mwh*1000*Kwh_selling_price;
return total_income_per_year;
}
