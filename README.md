#include <stdio.h>
#include <stdlib.h>

typedef long long ll;

/* --------------------------------------------------------
   extended_gcd :
   Calcule d = gcd(a,b) et trouve x,y tels que a*x + b*y = d
   -------------------------------------------------------- */
ll extended_gcd(ll a, ll b, ll *x, ll *y) {
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

int main() {
    ll a, b, c;
    printf("Résoudre a*x + b*y = c dans Z.\n");
    printf("Entrer a, b, c : ");
    if (scanf("%lld %lld %lld", &a, &b, &c) != 3) {
        printf("Erreur d'entrée.\n");
        return 1;
    }

    /* Cas particuliers */
    if (a == 0 && b == 0) {
        if (c == 0) {
            printf("Tous les couples (x,y) ∈ Z² sont solutions (infinité de solutions).\n");
        } else {
            printf("Aucune solution entière (0*x + 0*y = c impossible si c != 0).\n");
        }
        return 0;
    }

    if (a == 0) {
        /* b*y = c */
        if (b != 0 && c % b == 0) {
            ll y0 = c / b;
            printf("Solutions : x libre, y = %lld\n", y0);
            printf("Forme générale : (x,y) = (t, %lld), t ∈ Z\n", y0);
        } else {
            printf("Pas de solution entière (b ne divise pas c).\n");
        }
        return 0;
    }

    if (b == 0) {
        /* a*x = c */
        if (a != 0 && c % a == 0) {
            ll x0 = c / a;
            printf("Solutions : y libre, x = %lld\n", x0);
            printf("Forme générale : (x,y) = (%lld, t), t ∈ Z\n", x0);
        } else {
            printf("Pas de solution entière (a ne divise pas c).\n");
        }
        return 0;
    }

    /* Cas général */
    ll x0, y0;
    ll d = extended_gcd(a, b, &x0, &y0); /* x0,y0 -> solution pour a*x + b*y = d */

    if (c % d != 0) {
        printf("Pas de solution entière car gcd(%lld,%lld) = %lld ne divise pas %lld.\n",
               a, b, d, c);
        return 0;
    }

    /* Solution particulière : multiplier (x0,y0) par c/d */
    ll mult = c / d;
    ll xp = x0 * mult;
    ll yp = y0 * mult;

    /* Coefficients pour la solution générale
       x = xp + (b/d) * t
       y = yp - (a/d) * t
       t ∈ Z
    */
    ll bx = b / d;
    ll ay = a / d;

    printf("Il existe des solutions entières.\n");
    printf("PGCD(a,b) = %lld\n", d);
    printf("Une solution particulière : x0 = %lld, y0 = %lld\n", xp, yp);
    printf("Vérification : %lld * %lld + %lld * %lld = %lld\n", a, xp, b, yp, c);
    printf("Famille des solutions :\n");
    printf("  x = %lld + (%lld)*t\n", xp, bx);
    printf("  y = %lld - (%lld)*t\n", yp, ay);
    printf("pour t ∈ Z.\n");

    return 0;
}
