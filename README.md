"""
Created on Tue Dec  2 12:35:10 2025

@author: pc-portable.net
"""

import matplotlib.pyplot as plt
import numpy as np
import os
import csv

print("SCRIPT LANCE ")

# ==========================
# FICHIER D'ENTRÉE
# ==========================
INPUT_FILENAME = "Anouck_data_small.csv"

# DOSSIERS DE SORTIE
OUTPUT_DIR = "output"
IMAGES_DIR = "images"

os.makedirs(OUTPUT_DIR, exist_ok=True)
os.makedirs(IMAGES_DIR, exist_ok=True)

# ==========================
# CLASSES
# ==========================
class Sample:
    def __init__(self, sample_type, mouse_id, treatment, experimental_day, counts_live_bacteria):
        self.Sample_type = sample_type
        self.Mouse_id = mouse_id
        self.Treatment = treatment
        self.Experimental_day = experimental_day
        self.Counts_live_bacteria = float(counts_live_bacteria)

class Intestin:
    IntestinSamples = []

# ==========================
# LECTURE DU CSV
# ==========================
def loadDataFromFile(filename):
    print("Chargement du fichier :", filename)

    count = 0

    with open(filename, newline='', encoding="utf-8") as csvfile:
        reader = csv.reader(csvfile, delimiter=';')
        header = next(reader)  # Ignorer en-tête

        for row in reader:
            try:
                sample = Sample(
                    row[2],  # sample_type
                    row[4],  # mouse_id
                    row[5],  # treatment
                    row[7],  # experimental_day
                    row[8]   # counts
                )
                Intestin.IntestinSamples.append(sample)
                count += 1
            except:
                pass

    print("Nombre d'échantillons chargés :", count)

# ==========================
# GRAPHE FECAL
# ==========================
def draw_graph_Fecal():
    print("Création graphe FECAL...")
    plt.figure(figsize=(7,4))

    mouse_ids = sorted(set(s.Mouse_id for s in Intestin.IntestinSamples if s.Sample_type == "fecal"))

    for mouse in mouse_ids:
        y = [np.log10(s.Counts_live_bacteria)
             for s in Intestin.IntestinSamples
             if s.Sample_type == "fecal" and s.Mouse_id == mouse]

        x = list(range(1, len(y)+1))
        plt.plot(x, y, marker='o', label=mouse)

    plt.title("Fecal live bacteria")
    plt.xlabel("Sample number")
    plt.ylabel("log10(live bacteria / g)")
    plt.legend(fontsize=7)
    plt.grid(True)

    path = os.path.join(IMAGES_DIR, "fecal_lines.png")
    plt.savefig(path, dpi=300)
    plt.show()   # <-- Affiche le graphique
    plt.close()
    print("Image sauvée :", path)

# ==========================
# VIOLON ILEAL
# ==========================
def draw_graph_Ileal():
    print("Création graphe ILEAL...")
    y_ABX = []
    y_placebo = []

    for s in Intestin.IntestinSamples:
        if s.Sample_type == "ileal":
            if s.Treatment == "ABX":
                y_ABX.append(np.log10(s.Counts_live_bacteria))
            elif s.Treatment == "placebo":
                y_placebo.append(np.log10(s.Counts_live_bacteria))

    plt.figure(figsize=(5,4))
    plt.violinplot([y_ABX, y_placebo], showmeans=True)
    plt.title("Ileal live bacteria")
    plt.xticks([1,2], ["ABX", "Placebo"])
    plt.ylabel("log10(live bacteria / g)")
    plt.grid(axis="y")

    path = os.path.join(IMAGES_DIR, "ileal_violin.png")
    plt.savefig(path, dpi=300)
    plt.show()   # <-- Affiche le graphique
    plt.close()
    print("Image sauvée :", path)

# ==========================
# VIOLON CECAL
# ==========================
def draw_graph_Cecal():
    print("Création graphe CECAL...")
    y_ABX = []
    y_placebo = []

    for s in Intestin.IntestinSamples:
        if s.Sample_type == "cecal":
            if s.Treatment == "ABX":
                y_ABX.append(np.log10(s.Counts_live_bacteria))
            elif s.Treatment == "placebo":
                y_placebo.append(np.log10(s.Counts_live_bacteria))

    plt.figure(figsize=(5,4))
    plt.violinplot([y_ABX, y_placebo], showmeans=True)
    plt.title("Cecal live bacteria")
    plt.xticks([1,2], ["ABX", "Placebo"])
    plt.ylabel("log10(live bacteria / g)")
    plt.grid(axis="y")

    path = os.path.join(IMAGES_DIR, "cecal_violin.png")
    plt.savefig(path, dpi=300)
    plt.show()   # <-- Affiche le graphique
    plt.close()
    print("Image sauvée :", path)

# ==========================
# PROGRAMME PRINCIPAL
# ==========================
loadDataFromFile(INPUT_FILENAME)

draw_graph_Fecal()
draw_graph_Ileal()
draw_graph_Cecal()

print("SCRIPT TERMINÉ AVEC SUCCÈS")

