# Projet-biomodelisation-Lola-et-Anouck-
import matplotlib.pyplot as plt
import matplotlib.pyplot as pla
import matplotlib.pyplot as pltfecal
#import math
import numpy as np

class Sample:
    def __init__(self, sample_type, mouse_id, treatment, experimental_day, counts_live_bacteria):
        self.Sample_type = sample_type
        self.Mouse_id = mouse_id
        self.Treatment = treatment
        self.Experimental_day = experimental_day
        self.Counts_live_bacteria = counts_live_bacteria

class Intestin:
    IntestinSamples = []


filepath=('c:\\Claire\\data_real.csv')


def displaySample(sample, sample_type):
    if sample.Sample_type == sample_type:
            print(f"{sample.Sample_type};{sample.Mouse_id};{sample.Treatment};{sample.Experimental_day};{sample.Counts_live_bacteria}")

def draw_graph_Fecal():
    # importation du module requis
    figure,axes=pltfecal.subplots(figsize=(8, 4))

    pltfecal.title("Fecal live bacteria")
    axes.set_ylabel('log10(live bacteria/wet g)')

    axes.set_xlabel('treatment')

    mouse_ids_temp = []

    for sample in Intestin.IntestinSamples:
        if sample.Mouse_id != "mouse_ID":
            mouse_ids_temp.append(sample.Mouse_id)

    mouse_ids = set(mouse_ids_temp)

    for mouse_id in mouse_ids:

        # x axis values
        x = []
        for  sample in Intestin.IntestinSamples:
            if sample.Sample_type=='fecal' and sample.Mouse_id == mouse_id:
                x.append(np.log10(float(sample.Counts_live_bacteria)))

        y=[]
        for  sample in Intestin.IntestinSamples:
            if sample.Sample_type=='fecal' and sample.Mouse_id == mouse_id:
                y.append(np.log10(float(sample.Counts_live_bacteria)))
         # plotting the points
        pltfecal.plot(x, y)

    pltfecal.xlim([0.5,2.5])
    pltfecal.ylim([5.5,10.25])
    pltfecal.legend(loc='best')
    axes.legend(["ABX","placebo"],loc="lower right", fontsize=50)


    # function to show the plot
    pltfecal.show()

def draw_graph_Ileal():
    # importation du module requis
    figure,axes=pla.subplots(figsize=(8, 4))

    pla.title("Ileal live bacteria")
    axes.set_ylabel('log10(live bacteria/wet g)')

    axes.set_xlabel('treatment')
    # x axis values
    y_ABX=[]
    for  sample in Intestin.IntestinSamples:
        if sample.Sample_type=='ileal' and sample.Treatment=='ABX':
            y_ABX.append(np.log10(float(sample.Counts_live_bacteria)))

    y_placebo=[]
    for  sample in Intestin.IntestinSamples:
        if sample.Sample_type=='ileal' and sample.Treatment=='placebo':
            y_placebo.append(np.log10(float(sample.Counts_live_bacteria)))
    pla.xlim([0.5,2.5])
    pla.ylim([5.5,10.25])
    pla.legend(loc='best')
    axes.legend(["ABX","placebo"],loc="lower right", fontsize=50)
    axes.violinplot([y_ABX,y_placebo])

    # function to show the plot
    pla.show()

def draw_graph_Cecal():
    # importation du module requis
    figure,axes=plt.subplots(figsize=(8, 4))

    plt.title("Cecal live bacteria")
    axes.set_ylabel('log10(live bacteria/wet g)')

    axes.set_xlabel('treatment')
    # x axis values
    y_ABX=[]
    for  sample in Intestin.IntestinSamples:
        if sample.Sample_type=='cecal' and sample.Treatment=='ABX':
            y_ABX.append(np.log10(float(sample.Counts_live_bacteria)))

    y_placebo=[]
    for  sample in Intestin.IntestinSamples:
        if sample.Sample_type=='cecal' and sample.Treatment=='placebo':
            y_placebo.append(np.log10(float(sample.Counts_live_bacteria)))
    plt.xlim([0.5,3])
    plt.ylim([5.5,11])
    plt.legend(loc='best')
    axes.legend(["ABX","placebo"],loc="lower right", fontsize=50)
    axes.violinplot([y_ABX,y_placebo])

    # function to show the plot
    plt.show()

def loadDataFromFile(filepath):

    filepath = open(filepath, 'r')



    with filepath as file:
        # skip headers
        #line=file.readline()[:-1].split(';')

        line=file.readline()[:-1].split(';')

        while line[0] != '':

            Intestin.IntestinSamples.append(Sample(line[2], line[4], line[5], line[7], line[8]))

            file.readline()

            line=file.readline()[:-1].split(';')


def display_Samples():
    for sample in Intestin.IntestinSamples:
        displaySample(sample, "fecal")

    for sample in Intestin.IntestinSamples:
        displaySample(sample, "cecal")

    for sample in Intestin.IntestinSamples:
        displaySample(sample, "ileal")



if __name__ == '__main__':
    loadDataFromFile(filepath)
    display_Samples()

    draw_graph_Ileal()
    draw_graph_Fecal()
    draw_graph_Cecal()
