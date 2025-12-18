#include <stdio.h>
#include <stdlib.h>

typedef long long ll;

/*
--------------------------------------------------------
 extended_gcd :
 Calcule d = gcd(a,b) et trouve x,y tels que ax + by = d
--------------------------------------------------------
*/
ll extended_gcd(ll a, ll b, ll *x, ll *y)
{
    if (b == 0) {
        *x = 1;
        *y = 0;
        return llabs(a);
    }

    ll x1, y1;
    ll d = extended_gcd(b, a % b, &x1, &y1);

    *x = y1;
    *y = x1 - (a / b) * y1;

    return d;
}

int main()
{
    ll a, b, c;

    printf("Résoudre ax + by = c dans Z.\n");
    printf("Entrer a, b, c : ");

    if (scanf("%lld %lld %lld", &a, &b, &c) != 3) {
        printf("Erreur d'entrée.\n");
        return 1;
    }

    /* Cas particuliers */
    if (a == 0 && b == 0) {
        if (c == 0)
            printf("Tous les couples (x,y) ∈ Z² sont solutions.\n");
        else
            printf("Aucune solution entière.\n");
        return 0;
    }

    if (a == 0) {
        if (c % b == 0) {
            ll y0 = c / b;
            printf("Solutions : x libre, y = %lld\n", y0);
        } else {
            printf("Pas de solution entière.\n");
        }
        return 0;
    }

    if (b == 0) {
        if (c % a == 0) {
            ll x0 = c / a;
            printf("Solutions : y libre, x = %lld\n", x0);
        } else {
            printf("Pas de solution entière.\n");
        }
        return 0;
    }

    /* Cas général */
    ll x0, y0;
    ll d = extended_gcd(a, b, &x0, &y0);

    if (c % d != 0) {
        printf("Pas de solution entière car gcd(%lld,%lld) = %lld ne divise pas %lld.\n",
               a, b, d, c);
        return 0;
    }

    ll mult = c / d;
    ll xp = x0 * mult;
    ll yp = y0 * mult;

    printf("Il existe des solutions entières.\n");
    printf("PGCD(a,b) = %lld\n", d);
    printf("Une solution particulière : x = %lld, y = %lld\n", xp, yp);
    printf("Famille des solutions :\n");
    printf("x = %lld + (%lld)t\n", xp, b / d);
    printf("y = %lld - (%lld)t\n", yp, a / d);

    return 0;
}