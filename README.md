# PersonelBilgileri

#include <stdio.h>
#include <string.h>


struct Personel {

    char tcKimlik[11];
    char ad[15];
    char soyad[15];
    char telefon[11];
    char bolum[30];

};

void listele();
void ara();
void ekle();
void sil();
void guncelle();

int main() {
	 int secim;

	    do {
	        printf("\nPersonel Kayit Programina Hosgeldiniz\n");
	        printf("--------------------\n");
	        printf("1. Kayit Listele\n");
	        printf("2. Kayit Ara\n");
	        printf("3. Kayit Ekle\n");
	        printf("4. Kayit Sil\n");
	        printf("5. Kayit Guncelle\n");
	        printf("6. Programdan Cikis");
	        printf("\n--------------------\n");
	        printf("Yapmak istediginiz islemi secin: ");
	        scanf("%d", &secim);

	        switch (secim) {
	            case 1:
	                listele();
	                break;
	            case 2:
		             ara();
		             break;
		        case 3:
		             ekle();
		             break;
		        case 4:
		        	 sil();
		        	 break;
		        case 5:
				     guncelle();
		        case 6:
		        	printf("Cikis Yaptiniz.\n");
		            break;
		        default:
		            printf("Varolmayan islem sectiniz. Lutfen tekrar deneyin.\n");
	        }

	    } while (secim != 0);

	    return 0;
	}

void listele() {
    FILE * dosya = fopen("Personel.txt" , "r");

    if (dosya == NULL){
    	printf("Dosya acilamadi.\n");
    	return ;
    }

    struct Personel personel;
    printf("\nPersonel Listesi:\n");

     while (fscanf(dosya, "%11s %15s %15s %11s %30s", personel.tcKimlik, personel.ad, personel.soyad, personel.telefon, personel.bolum) == 5) {
        printf("TC Kimlik: %s\n", personel.tcKimlik);
        printf("Ad: %s\n", personel.ad);
        printf("Soyad: %s\n", personel.soyad);
        printf("Telefon: %s\n", personel.telefon);
        printf("Bolum: %s\n", personel.bolum);
        printf("------------------\n");
    }

    fclose(dosya);
    }

void ara() {
    char aranan[30];
    int secim;

    printf("Arama yapmak istediginiz kriteri secin:\n");
    printf("1. TC Kimlik\n");
    printf("2. Ad\n");
    printf("3. Soyad\n");
    printf("4. Telefon\n");
    printf("5. Bolum\n");
    printf("Seciminizi yapin (1-5): ");
    scanf("%d", &secim);

    struct Personel personel;
    int bulundu = 0;

    // Kullanıcıdan arama terimini alın
    printf("Aramak istediginiz terimi girin: ");
    scanf("%s", aranan);

    FILE *dosya = fopen("Personel.txt", "r");
    if (dosya == NULL) {
        printf("Dosya acilamadi.\n");
        return;
    }

    while (fscanf(dosya, "%s %s %s %s %s", personel.tcKimlik, personel.ad, personel.soyad, personel.telefon, personel.bolum) == 5) {
        switch (secim) {
            case 1: // TC Kimlik
                if (strcmp(aranan, personel.tcKimlik) == 0) {
                    bulundu = 1;
                }
                break;
            case 2: // Ad
                if (strcmp(aranan, personel.ad) == 0) {
                    bulundu = 1;
                }
                break;
            case 3: // Soyad
                if (strcmp(aranan, personel.soyad) == 0) {
                    bulundu = 1;
                }
                break;
            case 4: // Telefon
                if (strcmp(aranan, personel.telefon) == 0) {
                    bulundu = 1;
                }
                break;
            case 5: // Bolum
                if (strcmp(aranan, personel.bolum) == 0) {
                    bulundu = 1;
                }
                break;
            default:
                printf("Gecersiz secim. Lutfen tekrar deneyin.\n");
                fclose(dosya);
                return;
        }

        if (bulundu) {
            printf("\nPersonel Bulundu:\n");
            printf("TC Kimlik: %s\n", personel.tcKimlik);
            printf("Ad: %s\n", personel.ad);
            printf("Soyad: %s\n", personel.soyad);
            printf("Telefon: %s\n", personel.telefon);
            printf("Bolum: %s\n", personel.bolum);
            printf("------------------------\n");
            break;
        }
    }

    if (!bulundu) {
        printf("Aranan personel bulunamadi.\n");
    }

    fclose(dosya);
}


void ekle() {
    FILE *dosya = fopen("Personel.txt", "a");
    if (dosya == NULL) {
        printf("Dosya acilamadi.\n");
        return;
    }

    struct Personel personel;

    printf("Yeni personel bilgilerini girin:\n");
    printf("TC Kimlik: ");
    scanf("%s", personel.tcKimlik);
    printf("Ad: ");
    scanf("%s", personel.ad);
    printf("Soyad: ");
    scanf("%s", personel.soyad);
    printf("Telefon: ");
    scanf("%s", personel.telefon);
    printf("Bolum: ");
    scanf("%s", personel.bolum);

    fprintf(dosya, "%s %s %s %s %s\n", personel.tcKimlik, personel.ad, personel.soyad, personel.telefon, personel.bolum);

    fclose(dosya);

    printf("Personel eklendi.\n");
}

void sil() {
    char tcKimlikSil[11];

    printf("Silmek istediginiz personelin TC Kimlik numarasini girin: ");
    scanf("%s", tcKimlikSil);

    FILE *dosya = fopen("Personel.txt", "r");
    FILE *geciciDosya = fopen("TempPersonel.txt", "w");

    if (dosya == NULL || geciciDosya == NULL) {
        printf("Dosya acilamadi.\n");
        return;
    }

    struct Personel personel;

    while (fscanf(dosya, "%s %s %s %s %s", personel.tcKimlik, personel.ad, personel.soyad, personel.telefon, personel.bolum) == 5) {
        if (strcmp(tcKimlikSil, personel.tcKimlik) != 0) {
            fprintf(geciciDosya, "%s %s %s %s %s\n", personel.tcKimlik, personel.ad, personel.soyad, personel.telefon, personel.bolum);
        }
    }

    fclose(dosya);
    fclose(geciciDosya);

    remove("Personel.txt");
    rename("TempPersonel.txt", "Personel.txt");

    printf("Personel bilgileri silindi.\n");
}

void guncelle() {
    char tcKimlikGuncelle[11];

    printf("Güncellemek istediginiz personelin TC Kimlik numarasini girin: ");
    scanf("%s", tcKimlikGuncelle);

    FILE *dosya = fopen("Personel.txt", "r");
    FILE *geciciDosya = fopen("TempPersonel.txt", "w");

    if (dosya == NULL || geciciDosya == NULL) {
        printf("Dosya acilamadi.\n");
        return;
    }

    struct Personel personel;

    int bulundu = 0;

    while (fscanf(dosya, "%s %s %s %s %s", personel.tcKimlik, personel.ad, personel.soyad, personel.telefon, personel.bolum) == 5) {
        if (strcmp(tcKimlikGuncelle, personel.tcKimlik) == 0) {
            bulundu = 1;

            printf("Yeni bilgileri girin:\n");
            printf("Ad: ");
            scanf("%s", personel.ad);
            printf("Soyad: ");
            scanf("%s", personel.soyad);
            printf("Telefon: ");
            scanf("%s", personel.telefon);
            printf("Bolum: ");
            scanf("%s", personel.bolum);
        }

        fprintf(geciciDosya, "%s %s %s %s %s\n", personel.tcKimlik, personel.ad, personel.soyad, personel.telefon, personel.bolum);
    }

    fclose(dosya);
    fclose(geciciDosya);

    remove("Personel.txt");
    rename("TempPersonel.txt", "Personel.txt");

    if (bulundu) {
        printf("Personel bilgileri güncellendi.\n");
    } else {
        printf("Belirtilen TC Kimlik numarasına sahip personel bulunamadi.\n");
    }
}
