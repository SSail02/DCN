#include <stdio.h>
#include <math.h>
int main() {
    int data[10], hamming[20];
    int m, r = 0, i, j, k = 0, error_pos = 0;

    printf("Enter number of data bits: ");
    scanf("%d", &m);

    printf("Enter data bits (one by one): ");
    for (i = 0; i < m; i++) {
        scanf("%d", &data[i]);
    }

    while (pow(2, r) < (m + r + 1)) {
        r++;
    }

    for (i = 1, j = 0, k = 0; i <= m + r; i++) {
        if (i == pow(2, k)) {
            hamming[i] = 0;  
            k++;
        } else {
            hamming[i] = data[j];
            j++;
        }
    }

    for (i = 0; i < r; i++) {
        int parity = 0;
        for (j = 1; j <= m + r; j++) {
            if (j & (1 << i)) {
                parity ^= hamming[j];
            }
        }
        hamming[(int)pow(2, i)] = parity;
    }

    printf("\nHamming Code: ");
    for (i = 1; i <= m + r; i++) {
        printf("%d ", hamming[i]);
    }

    printf("\n\nEnter received bits:\n");
    for (i = 1; i <= m + r; i++) {
        scanf("%d", &hamming[i]);
    } 
    for (i = 0; i < r; i++) {
        int parity = 0;
        for (j = 1; j <= m + r; j++) {
            if (j & (1 << i)) {
                parity ^= hamming[j];
            }
        }
        error_pos += parity * pow(2, i);
    }

    if (error_pos == 0) {
        printf("\nNo error detected.");
    } else {
        printf("\nError detected at position: %d", error_pos);
        hamming[error_pos] ^= 1;

        printf("\nCorrected Hamming Code: ");
        for (i = 1; i <= m + r; i++) {
            printf("%d ", hamming[i]);
        }
    }

    return 0;
}
