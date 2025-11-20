#include <stdio.h>
#include <stdlib.h>

typedef long long ll;

/* Retourne le pgcd de a et b (valeurs absolues utilisées) */
ll absll(ll x) { return x < 0 ? -x : x; }

ll gcd(ll a, ll b) {
    a = absll(a); b = absll(b);
    while (b != 0) {
        ll t = a % b;
        a = b;
        b = t;
    }
    return a;
}

/* Algorithme d'Euclide étendu.
   Calcule d = gcd(a,b) et des entiers x, y tels que a*x + b*y = d.
   Renvoie d; x et y sont écrits via pointeurs. */
ll extended_gcd(ll a, ll b, ll *x, ll *y) {
    if (b == 0) {
        *x = (a >= 0) ? 1 : -1; /* si a négatif, mettre signe dans x */
        *y = 0;
        return absll(a);
    }
    ll x1 = 0, y1 = 0;
    ll d = extended_gcd(b, a % b, &x1, &y1);
    /* Mise à jour des coefficients */
    *x = y1;
    *y = x1 - (a / b) * y1;
    return d;
}

int main(void) {
    ll a, b, c;
    printf("Résolution de l'équation diophantienne ax + by = c\n");
    printf("Entrez a b c (séparés par des espaces) : ");
    if (scanf("%lld %lld %lld", &a, &b, &c) != 3) {
        fprintf(stderr, "Lecture invalide.\n");
        return 1;
    }

    /* Cas particuliers */
    if (a == 0 && b == 0) {
        if (c == 0) {
            printf("Tous les couples (x,y) ∈ Z^2 sont solutions (infinité de solutions).\n");
        } else {
            printf("Aucune solution entière (0·x + 0·y = c impossible si c != 0).\n");
        }
        return 0;
    }

    if (a == 0) {
        /* b*y = c */
        if (c % b == 0) {
            ll y0 = c / b;
            printf("Solutions : x libre (t ∈ Z), y = %lld\n", y0);
            printf("Forme générale : (x,y) = (t, %lld), t ∈ Z\n", y0);
        } else {
            printf("Pas de solution entière (b ne divise pas c).\n");
        }
        return 0;
    }

    if (b == 0) {
        /* a*x = c */
        if (c % a == 0) {
            ll x0 = c / a;
            printf("Solutions : y libre (t ∈ Z), x = %lld\n", x0);
            printf("Forme générale : (x,y) = (%lld, t), t ∈ Z\n", x0);
        } else {
            printf("Pas de solution entière (a ne divise pas c).\n");
        }
        return 0;
    }

    /* Cas général */
    ll x0 = 0, y0 = 0;
    ll d = extended_gcd(a, b, &x0, &y0); /* a*x0 + b*y0 = d (avec d >= 0) */

    if (c % d != 0) {
        printf("Pas de solution entière car gcd(%lld,%lld) = %lld ne divise pas %lld.\n", a, b, d, c);
        return 0;
    }

    /* Solution particulière : multiplier (x0,y0) par c/d */
    ll mult = c / d;
    ll xp = x0 * mult;
    ll yp = y0 * mult;

    /* Coefficients pour la solution générale :
       x = xp + (b/d) * t
       y = yp - (a/d) * t
       pour t ∈ Z
    */
    ll bx = b / d;
    ll ay = a / d;

    printf("Il existe des solutions entières.\n");
    printf("PGCD(a,b) = %lld\n", d);
    printf("Une solution particulière : x0 = %lld, y0 = %lld (vérifier : %lld*%lld + %lld*%lld = %lld)\n",
           xp, yp, a, xp, b, yp, c);
    printf("Famille des solutions :\n");
    printf("  x = %lld + (%lld) * t\n", xp, bx);
    printf("  y = %lld - (%lld) * t\n", yp, ay);
    printf("pour t ∈ Z.\n");

    return 0;
}
