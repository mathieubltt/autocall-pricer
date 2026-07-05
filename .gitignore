import numpy as np
import matplotlib.pyplot as plt

#Choice of number (random)

r = 0.03        #risk free
q = 0.02        #div
sigma = 0.22    #vol

S0 = 100.0      #spot
notional = 1000

autocall_barrier = 100
coupon_barrier = 70
protection_barrier = 60
coupon = 0.08 * notional
n_annees = 5


#Simulation

def simule_une_trajectoire():
    prix = S0
    niveaux = []
    for _ in range(n_annees):
        Z = np.random.normal()
        prix = prix * np.exp((r - q - 0.5 * sigma**2) + sigma * Z)
        niveaux.append(prix)
    return niveaux


#autocall
def paye_combien(niveaux):
    memoire = 0
    total = 0.0
    vivant = True

    for annee, niveau in enumerate(niveaux, start=1):
        if not vivant:
            break

        df = np.exp(-r * annee)


        if niveau >= coupon_barrier:
            total += coupon * (1 + memoire) * df
            memoire = 0
        else:
            memoire += 1


        if annee < n_annees and niveau >= autocall_barrier:
            total += notional * df
            vivant = False


        if annee == n_annees and vivant:
            if niveau >= protection_barrier:
                total += notional * df
            else:
                total += notional * niveau / 100 * df

    return total



def prix_autocall(n_scenarios=100_000):
    somme = 0.0
    for _ in range(n_scenarios):
        niveaux = simule_une_trajectoire()
        somme += paye_combien(niveaux)
    return somme / n_scenarios


def trace_trajectoires(n_trajectoires=60):
    plt.figure(figsize=(9, 5))

    for _ in range(n_trajectoires):
        niveaux = simule_une_trajectoire()     
        annees = list(range(1, n_annees + 1))
        plt.plot([0] + annees, [S0] + niveaux, color="steelblue", alpha=0.35, linewidth=0.8)

    plt.axhline(autocall_barrier, color="green", linestyle="--", label="Barriere de rappel (100%)")
    plt.axhline(coupon_barrier, color="orange", linestyle="--", label="Barriere de coupon (70%)")
    plt.axhline(protection_barrier, color="red", linestyle="--", label="Barriere de protection (60%)")

    plt.title(f"{n_trajectoires} trajectoires simulees du sous-jacent")
    plt.xlabel("Annee")
    plt.ylabel("Niveau du sous-jacent (%)")
    plt.legend()
    plt.tight_layout()
    plt.savefig("trajectoires.png", dpi=120)
    print("Graphique enregistre dans trajectoires.png")
    plt.show()


if __name__ == "__main__":
    np.random.seed(42)
    prix = prix_autocall()
    print(f"Prix de l'autocall : {prix:.1f}  (pour un nominal de {notional})")

    trace_trajectoires()
