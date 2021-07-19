# zmm sample per eta-partition

import ROOT, csv

def defTH1(title, name, binning) : 
    if len(binning) == 3 :
        hist = ROOT.TH1D(name, title, binning[0], binning[1], binning[2])
    return hist

# ME0 : 6 layers, 18 chambers, 8 ieta
# GE1/1 : 2 layers, 36 chambers, 8 ieta / We should count odd/even chamber separately
# GE2/1 : 2 layers, 18 chambers, 16 ieta
# count for each ieta : count for all chambers, and fill one histogram
# for region +1, -1


def Hits_Counter(station, layer, ieta) : 
    hist = defTH1("%s_%s_%s" %(station, layer, ieta), "%s_%s_%s" %(station, layer, ieta), [30, 0, 30])
    for fn in range(180) : 
        f = ROOT.TFile("/store/user/jlee/RelValZMM_14/crab_zmm_2026D76PU200/210419_200712/0000/muTuple_%s.root" %(fn + 1)) # zmm sample
        #f = ROOT.TFile("/store/user/jlee/RelValNuGun/crab_NuGun_2026D76PU200/210502_100147/0000/muTuple_%s.root" %(fn + 1)) # single Nu sample
        tree = f.Get("muNtupleProducer/MuDPGTree")
        for i_ev in range(tree.GetEntries()) : # Event loop : Fill histogram 36 times for one event (2 regions * 18 chambers)
            tree.GetEntry(i_ev)
            chamber_p = []
            chamber_m = []
            for j, i_chamber in enumerate(tree.gemRecHit_chamber) :
                # region == +1
                if tree.gemRecHit_station[j] == station and tree.gemRecHit_layer[j] == layer and tree.gemRecHit_etaPartition[j] == ieta and tree.gemRecHit_region[j] == 1 : 
                    chamber_p.append(i_chamber)
                # region == -1
                if tree.gemRecHit_station[j] == station and tree.gemRecHit_layer[j] == layer and tree.gemRecHit_etaPartition[j] == ieta and tree.gemRecHit_region[j] == -1 : 
                    chamber_m.append(i_chamber)
            for num in range(18) : # 18 chambers
                hist.Fill(chamber_p.count(num + 1))
                hist.Fill(chamber_m.count(num + 1))
    return hist

with open('zmm_st0_ly1.csv', 'w') as file :
    writer = csv.writer(file)
    writer.writerow(["station", "layer", "ieta", "mean"])
    for eta_id in range(8) : # GE2/1 ieta = 16 
        c = ROOT.TCanvas()
        hist = Hits_Counter(0, 1, eta_id + 1)
        hist.Draw()
        c.SaveAs('nu_st0_ly1_eta%s.png' %(eta_id + 1))
        writer.writerow([0, 1, eta_id + 1, hist.GetMean()])

# zmm sample per chamber

import ROOT, csv

def defTH1(title, name, binning) :
    if len(binning) == 3 :
        hist = ROOT.TH1D(name, title, binning[0], binning[1], binning[2])
    return hist

# ME0 : 6 layers, 18 chambers, 8 ieta
# GE1/1 : 2 layers, 36 chambers, 8 ieta / We should count odd/even chamber separately
# GE2/1 : 2 layers, 18 chambers, 16 ieta
# count for each ieta : count for all chambers, and fill one histogram
# for region +1, -1


def Hits_Counter(station, layer) :
    hist = defTH1("%s_%s" %(station, layer), "%s_%s" %(station, layer), [70, 0, 70])
    for fn in range(180) :
        f = ROOT.TFile("/store/user/jlee/RelValZMM_14/crab_zmm_2026D76PU200/210419_200712/0000/muTuple_%s.root" %(fn + 1)) # zmm sample
        #f = ROOT.TFile("/store/user/jlee/RelValNuGun/crab_NuGun_2026D76PU200/210502_100147/0000/muTuple_%s.root" %(fn + 1)) # single Nu sample
        tree = f.Get("muNtupleProducer/MuDPGTree")
        for i_ev in range(tree.GetEntries()) : # Event loop : Fill histogram 36 times for one event (2 regions * 18 chambers)
            tree.GetEntry(i_ev)
            chamber_p = []
            chamber_m = []
            for j, i_chamber in enumerate(tree.gemRecHit_chamber) :
                # region == +1
                if tree.gemRecHit_station[j] == station and tree.gemRecHit_layer[j] == layer and tree.gemRecHit_region[j] == 1 :
                    chamber_p.append(i_chamber)
                # region == -1
                if tree.gemRecHit_station[j] == station and tree.gemRecHit_layer[j] == layer and tree.gemRecHit_region[j] == -1 :
                    chamber_m.append(i_chamber)
            for num in range(18) : # 18 chambers
                # GE1/1 : 36 chambers, We should count the odd/even chamber separately
                hist.Fill(chamber_p.count(num + 1))
                hist.Fill(chamber_m.count(num + 1))
    return hist

c = ROOT.TCanvas()
hist = Hits_Counter(0, 1)
hist.Draw()
c.SaveAs('station_0_layer_1.png')

# Nu sample per eta-partition

import ROOT, csv

def defTH1(title, name, binning) : 
    if len(binning) == 3 :
        hist = ROOT.TH1D(name, title, binning[0], binning[1], binning[2])
    return hist

# ME0 : 6 layers, 18 chambers, 8 ieta
# GE1/1 : 2 layers, 36 chambers, 8 ieta / We should count odd/even chamber separately
# GE2/1 : 2 layers, 18 chambers, 16 ieta
# count for each ieta : count for all chambers, and fill one histogram
# for region +1, -1


def Hits_Counter(station, layer, ieta) : 
    hist = defTH1("%s_%s_%s" %(station, layer, ieta), "%s_%s_%s" %(station, layer, ieta), [20, 0, 20])
    for fn in range(180) : 
        #f = ROOT.TFile("/store/user/jlee/RelValZMM_14/crab_zmm_2026D76PU200/210419_200712/0000/muTuple_%s.root" %(fn + 1)) # zmm sample
        f = ROOT.TFile("/store/user/jlee/RelValNuGun/crab_NuGun_2026D76PU200/210502_100147/0000/muTuple_%s.root" %(fn + 1)) # single Nu sample
        tree = f.Get("muNtupleProducer/MuDPGTree")
        for i_ev in range(tree.GetEntries()) : # Event loop : Fill histogram 36 times for one event (2 regions * 18 chambers)
            tree.GetEntry(i_ev)
            chamber_p = []
            chamber_m = []
            for j, i_chamber in enumerate(tree.gemRecHit_chamber) :
                # region == +1
                if tree.gemRecHit_station[j] == station and tree.gemRecHit_layer[j] == layer and tree.gemRecHit_etaPartition[j] == ieta and tree.gemRecHit_region[j] == 1 : 
                    chamber_p.append(i_chamber)
                # region == -1
                if tree.gemRecHit_station[j] == station and tree.gemRecHit_layer[j] == layer and tree.gemRecHit_etaPartition[j] == ieta and tree.gemRecHit_region[j] == -1 : 
                    chamber_m.append(i_chamber)
            for num in range(18) : # 18 chambers
                hist.Fill(chamber_p.count(num + 1))
                hist.Fill(chamber_m.count(num + 1))
    return hist

with open('zmm_st0_ly1.csv', 'w') as file :
    writer = csv.writer(file)
    writer.writerow(["station", "layer", "ieta", "mean"])
    for eta_id in range(8) : # GE2/1 ieta = 16 
        c = ROOT.TCanvas()
        hist = Hits_Counter(0, 1, eta_id + 1)
        hist.Draw()
        c.SaveAs('nu_st0_ly1_eta%s.png' %(eta_id + 1))
        writer.writerow([0, 1, eta_id + 1, hist.GetMean()])

# Nu sample per chamber

import ROOT, csv

def defTH1(title, name, binning) :
    if len(binning) == 3 :
        hist = ROOT.TH1D(name, title, binning[0], binning[1], binning[2])
    return hist

# ME0 : 6 layers, 18 chambers, 8 ieta
# GE1/1 : 2 layers, 36 chambers, 8 ieta / We should count odd/even chamber separately
# GE2/1 : 2 layers, 18 chambers, 16 ieta
# count for each ieta : count for all chambers, and fill one histogram
# for region +1, -1


def Hits_Counter(station, layer) :
    hist = defTH1("%s_%s" %(station, layer), "%s_%s" %(station, layer), [70, 0, 70])
    for fn in range(180) :
        #f = ROOT.TFile("/store/user/jlee/RelValZMM_14/crab_zmm_2026D76PU200/210419_200712/0000/muTuple_%s.root" %(fn + 1)) # zmm sample
        f = ROOT.TFile("/store/user/jlee/RelValNuGun/crab_NuGun_2026D76PU200/210502_100147/0000/muTuple_%s.root" %(fn + 1)) # single Nu sample
        tree = f.Get("muNtupleProducer/MuDPGTree")
        for i_ev in range(tree.GetEntries()) : # Event loop : Fill histogram 36 times for one event (2 regions * 18 chambers)
            tree.GetEntry(i_ev)
            chamber_p = []
            chamber_m = []
            for j, i_chamber in enumerate(tree.gemRecHit_chamber) :
                # region == +1
                if tree.gemRecHit_station[j] == station and tree.gemRecHit_layer[j] == layer and tree.gemRecHit_region[j] == 1 :
                    chamber_p.append(i_chamber)
                # region == -1
                if tree.gemRecHit_station[j] == station and tree.gemRecHit_layer[j] == layer and tree.gemRecHit_region[j] == -1 :
                    chamber_m.append(i_chamber)
            for num in range(18) : # 18 chambers
                # GE1/1 : 36 chambers, We should count the odd/even chamber separately
                hist.Fill(chamber_p.count(num + 1))
                hist.Fill(chamber_m.count(num + 1))
    return hist

c = ROOT.TCanvas()
hist = Hits_Counter(0, 1)
hist.Draw()
c.SaveAs('station_0_layer_1.png')

# Plot Hit rate

import ROOT, csv, math
import numpy as np

def getData(sample, station, layer) : # ieta : 1 to 8 (1 to 16 in case of GE2/1)
    # Load csv file
    if sample == 'zmm' : data = open('zmm_ME0.csv', 'r')
    elif sample == 'nu' : data = open('singleNu_ME0.csv', 'r')
    else : print("wrong sample name")
    rdr = csv.reader(data)
    r_pos = []
    trArea = []
    mean = []
    total = [] # instead sigma
    for i, line in enumerate(rdr) :
        if i == 0 : continue
        elif int(line[0]) == station and int(line[2]) == layer :
            r_pos.append(float(line[5]))
            trArea.append(float(line[8]))
            mean.append(float(line[11]))
            total.append(float(line[13]))
        else : continue
    data.close()
    return r_pos, trArea, mean, total

def HitRate(sample, station, layer) :
    SF = 2544./(3564. * 25e-9) # including fill factor 2544/3564
    if sample == 'zmm' : nofev = 15348.
    elif sample == 'nu' : nofev = 9000.
    Hit_rate = []
    error = []
    dataset = getData(sample, station, layer)
    for i in range(len(dataset[0])) :
        Hit_rate.append(SF * dataset[2][i] / dataset[1][i])
        error.append(SF * math.sqrt(dataset[3][i]) / (dataset[1][i] * nofev))
    return Hit_rate, error

def Plotting_ME0(sample) :
    # average of 6 layers of station == 0
    c = ROOT.TCanvas("c1", "Hit rate vs. R")
    gr = ROOT.TGraphErrors()
    dataset = getData(sample, 0, 1)
    Hit_rate_avg = []
    error_avg = []
    # average of all layers and even/odd chamber
    for i in range(len(dataset[0])) :
        array_HitRate = np.array([HitRate(sample, 0, 1)[0][i], HitRate(sample, 0, 2)[0][i], HitRate(sample, 0, 3)[0][i], HitRate(sample, 0, 4)[0][i], HitRate(sample, 0, 5)[0][i], HitRate(sample, 0, 6)[0][i]])
        array_error = np.array([HitRate(sample, 0, 1)[1][i], HitRate(sample, 0, 2)[1][i], HitRate(sample, 0, 3)[1][i], HitRate(sample, 0, 4)[1][i], HitRate(sample, 0, 5)[1][i], HitRate(sample, 0, 6)[1][i]])
        Hit_rate_avg.append(np.mean(array_HitRate))
        error_avg.append(np.mean(array_error))
    print(Hit_rate_avg)
    for i in range(len(Hit_rate_avg)) :
        gr.SetPoint(i, dataset[0][i], Hit_rate_avg[i])
        gr.SetPointError(i, 0, error_avg[i])
    # Fitting
    gr.Fit("expo")
    f = gr.GetListOfFunctions().FindObject("expo")
    f.SetLineColor(1)
    f.SetLineWidth(1)
    gr.SetMarkerSize(1.5)
    gr.SetMarkerStyle(22)
    gr.GetXaxis().SetTitle("R [cm]")
    gr.GetYaxis().SetTitle("Hit Rate [Hz/cm^{2}]")
    gr.SetMinimum(0)
    gr.Draw("APZ")
    txt = ROOT.TLatex()
    txt.SetNDC()
    txt.SetTextFont(42)
    txt.SetTextSize(0.030)
    txt.SetTextAlign(13)
    txt.DrawLatex(.6, .75, "Hit rate of PU = 200 simulation")
    c.Update()
    leg = ROOT.TLegend(0.6, 0.55, 0.85, 0.7)
    leg.SetTextSize(0.030)
    leg.SetBorderSize(0)
    leg.AddEntry(gr, "average of 6 layers", "lpe")
    leg.Draw()
    c.SaveAs("%s_ME0.pdf" %(sample))

Plotting_ME0('zmm')
Plotting_ME0('nu')
