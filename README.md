#include <stdio.h>
#include <stdbool.h>

bool estPremier(int n) {
    if (n < 2) return false;
    for (int i = 2; i * i <= n; i++) {
        if (n % i == 0) {
            return false;
        }
    }
    return true;
}

int main() {
    int N = 100; // tu peux changer

    // 1. Afficher tous les nombres
    printf("Tous les nombres :\n");
    for (int i = 2; i <= N; i++) {
        printf("%d ", i);
    }

    // 2. Afficher seulement les nombres premiers
    printf("\n\nNombres premiers :\n");
    for (int i = 2; i <= N; i++) {
        if (estPremier(i)) {
            printf("%d ", i);
        }
    }

    printf("\n");
    return 0;
}
